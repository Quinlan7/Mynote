

# WAVENET: A GENERATIVE MODEL FOR RAW AUDIO

## 摘要

这篇论文介绍了WaveNet，一个用于生成原始音频波形的深度神经网络。该模型是一个完全的概率自回归模型，每个音频样本的预测分布都基于之前所有的样本；尽管如此，我们展示了它可以在每秒包含数万个样本的音频数据上高效地进行训练。当应用于文本到语音（TTS）时，它达到了最先进的性能，人类听众认为它在英语和普通话方面都比最佳的参数和拼接系统听起来更加自然。单个WaveNet可以以相同的保真度捕捉许多不同说话者的特征，并且可以通过条件于说话者身份来进行切换。当训练用于模拟音乐时，我们发现它能够生成新颖且常常高度真实的音乐片段。我们还展示了它可以作为判别模型使用，在音素识别方面取得了有前景的结果。



## 一、介绍

本文介绍了WaveNet，一个基于PixelCNN（van den Oord等人，2016a；b）架构的音频生成模型。本文的主要贡献如下：

• 我们展示了WaveNet能够生成原始语音信号，这些信号在人类评估者评估的文本到语音（TTS）领域中达到了前所未有的主观自然度。

• ==为了处理原始音频生成所需的长范围时间依赖关系，我们基于扩展因果卷积开发了新的架构，这些架构具有非常大的感受野。==

• 我们展示了当以说话者身份为条件时，可以使用单个模型生成不同的声音。

• 在一个小型语音识别数据集上测试时，相同的架构表现出了强大的结果，并且在生成其他音频模式（如音乐）时也具有前景。

我们相信WaveNet为处理许多依赖于音频生成的应用（例如TTS、音乐、语音增强、语音转换、源分离）提供了一个通用且灵活的框架。



### 2.1 扩张因果卷积

*The main ingredient of WaveNet are causal convolutions. By using causal convolutions, we make sure the model cannot violate the ordering in which we model the data: the prediction p (xt+1 | x1, ..., xt) emitted by the model at timestep t cannot depend on any of the future timesteps xt+1, xt+2, . . . , xT as shown in Fig. 2. For images, the equivalent of a causal convolution is a masked convolution (van den Oord et al, 2016a) which can be implemented by constructing a mask tensor and doing an elementwise multiplication of this mask with the convolution kernel before applying it. For 1-D data such as audio one can more easily implement this by shifting the output of a normal convolution by a few timesteps.*

WaveNet的主要成分是因果卷积。通过使用因果卷积，我们确保模型不会违反我们建模数据的顺序：模型在时间步t处发出的预测p(xt+1 | x1, ..., xt)不能依赖于任何未来的时间步xt+1, xt+2, ..., xT，如图2所示。对于图像，因果卷积的等价物是掩码卷积（van den Oord等人，2016a），这可以通过构造一个掩码张量并在应用之前将该掩码与卷积核进行逐元素乘法来实现。对于一维数据（如音频），可以通过将正常卷积的输出移动几个时间步来更容易地实现这一点。

*At training time, the conditional predictions for all timesteps can be made in parallel because all timesteps of ground truth x are known. When generating with the model, the predictions are sequential: after each sample is predicted, it is fed back into the network to predict the next sample. Because models with causal convolutions do not have recurrent connections, they are typically faster to train than RNNs, especially when applied to very long sequences. One of the problems of causal convolutions is that they require many layers, or large filters to increase the receptive field. For example, in Fig. 2 the receptive field is only 5 (= #layers + filter length - 1). In this paper we use dilated convolutions to increase the receptive field by orders of magnitude, without greatly increasing computational cost.*

在训练时，所有时间步的条件预测都可以并行进行，因为所有时间步的真实值x都是已知的。当使用模型进行生成时，预测是顺序的：在预测每个样本后，它会反馈回网络以预测下一个样本。

由于使用因果卷积的模型没有循环连接，它们通常比RNNs更快地进行训练，特别是当应用于非常长的序列时。因果卷积的一个问题是它们需要许多层或大滤波器来增加感受野。例如，在图2中，感受野仅为5（= 层数 + 滤波器长度 - 1）。在本文中，我们使用膨胀卷积（dilated convolutions）来增加感受野的数量级，而不会大大增加计算成本。

膨胀卷积（也称为trous`，或带孔卷积）是一种卷积，其中滤波器通过在输入值之间以一定的步长跳过而应用于比其长度更大的区域。这相当于使用由原始滤波器通过用零膨胀而得到的更大滤波器进行卷积，但效率要高得多。膨胀卷积有效地允许网络以比正常卷积更粗的尺度进行操作。这与池化或步幅卷积类似，但在这里输出的尺寸与输入的尺寸相同。作为一个特例，膨胀率为1的膨胀卷积会产生标准卷积。图3展示了膨胀率为1、2、4和8的膨胀因果卷积。膨胀卷积以前已在各种上下文中使用，例如信号处理（Holschneider等人，1989；Dutilleux，1989）和图像分割（Chen等人，2015；Yu和Koltun，2016）。

堆叠的膨胀卷积使网络仅通过几层就能拥有非常大的感受野，同时在整个网络中保持输入分辨率以及计算效率。

在本文中，每层的膨胀率都翻倍直到达到一个限制，然后重复：例如，

1, 2, 4, ..., 512, 1, 2, 4, ..., 512, 1, 2, 4, ..., 512。

这种配置的直觉有两方面。首先，膨胀因子的指数增加导致深度上的感受野的指数增长（Yu和Koltun，2016）。例如，每个1, 2, 4, ..., 512块都具有大小为1024的感受野，可以看作是1×1024卷积的更高效和更具判别性的（非线性）对应物。其次，堆叠这些块进一步增加了模型的容量和感受野的大小。
