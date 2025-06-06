# Multimodal fusion for large-scale traffic prediction with heterogeneous retentive networks

利用异构记忆网络进行大规模交通预测的多模态融合

期刊：INFORMATION FUSION

IF ： 14.7

## 摘要

由于城市交通具有复杂的时空动态性，交通速度预测在交通运输研究领域是一项严峻的挑战。本研究提出了一个新颖的框架，用于融合不同的数据模态，以提高短期交通速度预测的准确性。 我们引入了异构**记忆（retentive）**网络（H-RetNet），==它能将多源城市数据整合为带有地理空间关系编码的高维表示形式==。通过将异构保留网络与门控循环单元（GRU）相结合，我们的模型能够捕捉复杂的时空相关性。 我们==使用一个包含社交媒体、房地产以及兴趣点数据的北京现实交通数据集对该方法进行了验证==。实验表明，与现有方法相比，我们的方法性能更优，而且这种融合架构提高了稳健性。尤其值得一提的是，我们观察到均方误差（MSE）降低了21.91%，这凸显了我们的框架在为交通管理策略提供参考并加以完善方面的潜力。 



## 一、简介



为应对上述提及的挑战，我们提出了一种基于异构记忆网络（H-RetNet）的多模态数据模型，用于交通速度预测。此前使用循环神经网络（RNNs）的多模态预测方法，尽管在跨不同层级捕捉多模态数据方面颇为有效，但存在预测精度低以及训练速度慢的问题。 我们的方法整合了时空数据，利用异构保留网络来综合广泛地图中的全面信息，进而提高交通场景预测的准确性。这使得我们能够将与交通预测相关的各种多模态数据融合到一个时空超平面中。通过将该超平面与地理位置相结合，我们可以在时空维度上的各个点预测交通速度。因此，本文引入了一种全新的交通速度预测框架。该系统将多模态数据整合为具有高维度、地理空间感知的表示形式，从而能够对不同地点和不同时间的交通状况进行预测，如图1所示。然后，通过将这些具有地理保留特性的表示形式与地图位置相结合，我们就能够预测各个地点和各个时间点的交通状况。 

本文的主要贡献总结如下： 

+ 我们引入了一种用于大规模交通速度预测的多层面方法，该方法整合了多维数据，将时空动态与额外的背景变量相结合。这种方法提供了一个全面的视角，提高了交通预测的准确性和稳健性。 
+ 本文提出并详细阐述了异构保留网络（H-RetNet），这是一种新的神经网络架构，旨在有效同化异构数据源。在局部交通状况受外部区域因素影响较小的场景中，这种架构展现出了卓越的性能，从而为那些统一考虑全局影响的模型提供了一个更精细的替代方案。 
+ 通过在一个全面的现实世界数据集上进行大量的实证评估，我们的方法在预测精度方面优于已有的神经网络模型。我们方法的优越性能凸显了其在交通管理和城市规划方面的实际相关性，有可能改变交通运营的方式。 

本文的其余部分结构如下：第2节对相关文献进行综述，追溯交通预测方法的发展历程；第3节，我们对交通速度预测问题进行定义；第4节，介绍时空交通预测框架，包括多模态数据注入和模型构建；第5节使用来自北京的现实世界交通速度数据集对我们的预测模型进行评估，并将其与近期研究中知名的现有模型进行比较；最后，第6节对我们的工作进行总结，并为未来的研究提出方向。 

 

## 三、问题描述

本研究着重于开发一种短期预测模型，该模型利用此前`P`个时间步长的历史交通速度数据，对未来`R`个时间步长范围内的交通速度进行预测。其目标是综合利用这些历史数据以及各类辅助数据集来生成准确的交通速度预测结果。我们会详细介绍所使用的数据，并对预测目标进行定义。本项工作中的计算实验是基于赵等人提出的一个公开数据集开展的，不过该方法的设计初衷是使其能够适用于其他类似的数据集。 

**数据描述** 整个城市的交通速度数据是以固定的时间间隔 $\Upsilon $ 进行采样的。在每个采样点，我们会计算在时间间隔 $\Upsilon $ 内所有经过车辆的平均速度。历史交通数据形成了一个三维网格，其维度为 $T\times N^1\times N^2$，其中 $T$ 表示时间步长的总数，$N^1$ 和 $N^2$ 分别是对应水平轴和垂直轴的空间维度。这个网格中的每一项（记为 $X_{\tau}$）都反映了特定时间和特定位置的交通速度。 我们的目标是预测该网格内 $N$ 个特定兴趣点的交通速度。 

为增强我们模型的预测能力，我们纳入了辅助信息矩阵：$\chi^2$ 用于社交媒体文本，$\chi^3$ 用于房地产价格，$\chi^4$用于与兴趣点（POI）相关的特征，它们的维度分别为 $C_2\times N^1\times N^2$、 $C_3\times N^1\times N^2$ 和 $C_4\times N^1\times N^2$。 $C_2$、$C_3$ 和 $C_4$ 维度分别代表这些辅助数据集的特征数量，这些辅助数据集都已经过预处理，以便在空间上与交通数据对齐。为确保矩阵的一致性，我们将 $C_1$定义为等于 $P$ ，这样历史交通速度矩阵 $\chi^1$ 的第一维度就与所考虑的过去时间步长的数量相匹配。该数据集被随机划分为训练集、验证集和测试集。 

**预测任务** 我们的模型旨在预测未来交通速度，针对坐标为 $(x,y) \in \mathbb{Z}$ 的每个兴趣点生成预测值 $\hat{Y}^{P + 1 + \tau}_{x,y} \in \mathbb{R}^{N\times R}$。 $\hat{Y}^{P + 1 + \tau}_{x,y}$ 与来自历史数据的真实值 $Y_{x,y}$ 之间应具有最小差异。这些预测涵盖了从 $P + 1 + \tau$ 到 $P + R + \tau$ 的时间范围，其依据是从时间步长 $\tau$ 到 $\tau + P$ 的历史速度数据。在此，$\tau$ 表示任意一个起始时间步长。交通速度的历史序列由 $\chi^{1} \in \mathbb{R}^{N\times P\times N^1\times N^2}$ 表示，该序列定义为 $\chi^{1}=(X_{\tau},\ldots,X_{P + \tau})$，并且与辅助数据集进行了整合。预测框架不仅利用了给定兴趣点的历史速度数据，还纳入了来自地图上所有其他点（$N_1 \times N_2$范围内）的速度信息，以此增强对交通动态的情境理解，进而提高预测准确性。 本文所使用的符号展示于表1和表2中。 

![image-20241118171958195](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202411181719445.png)

![image-20241118172012993](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202411181720095.png)



## 四、解决方法

在接下来的内容中，我们介绍了用于数据预处理和预测任务的方法。

### 4.1 框架和方法

![image-20241118172608821](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202411181726947.png)

> 图2. 所提出的框架按如下方式运作：首先，全局参考信息通过各自的扁平化与映射操作进行处理，以分别生成高维表示形式。随后，这些表示形式汇聚于异构记忆网络（H-RetNet）中，该网络会对各种不同的输入进行整合。与此同时，查询位置通过高斯位置掩码被转换为处于同一超空间内的位置感知映射表示形式。此外，查询点处的速度值会经过扁平化与映射操作，从而生成具有时间感知的速度表示形式。最后阶段采用门控循环单元来融合这些输入，并输出在坐标为 $(x,y)$ 处、未来时间区间从 $P + 1$ 到 $P + Q + \tau$ 的预测速度值$\hat{Y}$。 

一种新颖的深度学习模型通过利用时空架构来解决交通速度预测问题，该架构能够高效地表示和汇总多模态信息。这种设计通过对目标查询点进行对齐和整合，有助于实现精准预测。此任务的框架如图2所示。 首先，全局参考信息分别通过单独的扁平化（Flatten）和映射（Map）操作进行处理，以生成 高维表示形式 $H_1$、$H_2$和$H_3$。随后，这些表示形式在异构保留网络（H-RetNet）中汇聚，该网络会对这些不同的输入进行整合。同时，查询位置通过高斯位置掩码被转换为处于同一超空间内的位置感知映射表示形式 $x^5$。 此外，查询点处的速度值会经过扁平化和映射操作，生成具有时间感知的速度表示形式 $x^1$。 最后阶段采用门控循环单元（Gated Recurrent Unit，GRU）来融合这些输入，并输出在坐标为 $(x,y)$ 处、未来时间区间从 $P + 1$ 到 $P + Q + \tau$ 的预测速度值$\hat{Y}$。 

### 4.2 数据预处理

![image-20241119211630846](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202411192116083.png)

> 该框架展示了一个神经网络处理流程。首先，输入数据通过卷积（Convolution）和激活（Activation）操作进行处理，以生成特征图（Feature Maps）。随后，这些特征图通过池化（Pooling）操作进行简化。接着，特征图经由扁平化层（Flattening Layer）被转换为向量，最后，全连接层（Fully Connected Layer）会对该向量进行分析以做出预测。

交通速度预测中的一个主要挑战在于数据的稀疏性，这是由交通速度为零的网格单元导致的，这些单元通常表示没有道路或交通的区域。为应对这一问题，我们对数据集进行了优化，只保留那些在整个规划时段内至少出现过一次非零交通速度的单元格，以此表明存在交通流。 这种优化将 $\mathcal{N}$ 定义为具有相关兴趣点（POI）信息的单元格数量，而这些单元格成为我们模型关注的重点。通过聚焦这些单元格，我们旨在提高交通预测的准确性和适用性。

 在我们的分析中，我们基于这样一个假设开展研究：交通速度数据在空间上是相互关联的，并且随着与兴趣点距离的增加，这种关联性的显著性会逐渐减弱。因此，我们将更广泛的数据集 $\chi$ 截断为一个子集，该子集仅包含目标位置紧邻区域的数据。具体而言，我们将注意力集中在以兴趣点为中心的 $N\times N$ 个点构成的网格上，在东、南、西、北四个基本方向上各选取 $\frac{N - 1}{2}$ 个点。如果这个子集超出了我们数据集所定义的边界范围，我们就用默认的零交通速度来替代缺失的数据，以此表示不存在测量数据。因此，子集 $\chi^{j}$ 的维度为 $\mathbb{R}^{B\times C_{j}\times N\times N}$，其中 $B$ 表示批量大小，$C_{j}$ 表示来自信息源 $j$ 所考虑的通道或特征数量。值得注意的是，在本节内容中，为了清晰和简洁起见，我们在讨论时暂不考虑批量大小这一维度。 

在所描述的框架中，扁平化与映射（Flatten&Map）过程是为异构保留网络（H-RetNet）准备异构输入的关键步骤。具体来说，从各种数据源（社交媒体文本 $\chi^{2}$、房地产信息 $\chi^{3}$ 以及兴趣点信息 $\chi^{4}$ ）中提取的特征，会经历一种转换，即==将它们的多维结构扁平化为统一的表示形式==，分别记为 $H^{1}$、$H^{2}$ 和 $H^{3}$，如图3所示。同时，基于查询位置（$\chi^{5}$）会生成一个高斯位置掩码，该掩码也会被扁平化和映射。这些扁平化后的表示形式随后由异构保留网络（H-RetNet）进行合成，从而实现对不同数据模态的融合。最后，门控循环单元（GRU）利用融合后的信息来预测查询点处的交通速度随时间的变化情况，生成在坐标为 $(x,y)$ 处、特定未来时间区间（从 $P + 1$ 到 $P + Q + \tau$）的预测速度值 $\hat{Y}$。然后，我们利用异构保留网络（H-RetNet）融合来自不同来源（$\chi^{1}$ 、$\chi^{2}$、$\chi^{3}$ 、$\chi^{4}$ 以及高斯掩码 $\chi^{5}$）的信息，接着使用门控循环单元（GRU）网络来捕捉时间关系。我们使用 $j$ 作为数据源的索引，用 $\mathbb{J}$ 表示数据源索引的集合。为了清晰地阐述我们的解决方案方法，我们省略了批量大小这一维度。不过需要注意的是，在模型训练阶段，我们会采用批量大小为 $B$（其中 $B\leq N$ ）的设置。 

### 4.3 高斯掩码

在我们的框架中不可或缺的高斯掩码 $\chi^{5}$，用于根据空间相关性来衡量目标位置附近各点的重要性。我们采用的是与周等人在文献[72]中所使用的相同的高斯掩码。该掩码是依据高斯函数的原理构建而成的，高斯函数具有钟形曲线的特征，能够使权重呈现出平滑的梯度变化，即随着与中心点距离的增加，权重逐渐减小。采用这样的掩码可确保距离较近的点对分析产生更大的影响，这反映了空间相关性随距离自然衰减的特性。 该掩码定义在与子集 $\chi^{1}$到 $\chi^{4}$ 相同的 $N\times N$ 网格上，其峰值与目标位置对齐，并且可通过调整其标准差来控制权重的分布范围。从数学角度来讲，相对于中心点而言，高斯掩码在点 $(x,y)$ 处的值由 $e^{-\frac{x^{2}+y^{2}}{2\sigma^{2}}}$ 给出，其中 $\sigma$ 是高斯函数的标准差参数。 这种加权机制在与交通速度数据进行卷积运算时，会产生一种经过调制的表示形式，它能精确地突出近邻空间数据，同时逐步减弱外围点的影响，从而为我们所提出框架中的后续处理阶段优化输入数据。 

### 4.4 用于空间融合的异构记忆网络

![image-20241119214048386](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202411192140565.png)

> 图4. 异构保留网络（H-RetNet）架构：三种不同的数据源，分别记为 $\chi^{1}$ 、$\chi^{2}$ 和 $\chi^{3}$ ，被输入到异构记忆网络（H-RetNet）中，以生成适用于后续应用的情境化表示形式 $H_{l}$ 。每种数据源类型都通过颜色加以区分。异构保留网络（H-RetNet）由三个主要部分构成：多模态数据处理、源自源节点的异构消息传递以及针对特定目标定制的异构消息聚合。 

我们提出了一种异构保留网络（H-RetNet），它是基于孙等人在文献[71]中的已有工作开发而来的。图4展示了异构保留网络（H-RetNet）的整体架构。我们基于记忆（Retention）的并行表示来构建我们的架构，该架构能够并行容纳多种信息。 鉴于我们拥有从时间步长 $\tau$ 到时间步长 $\tau + P$ 的交通速度数据，我们希望预测坐标为 $x$ 和 $y$ 的点在时间步长从 $\tau + P + 1$ 到 $\tau + P + R$ 的交通速度。其数学表达式如下： 

![image-20241119214934405](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202411192149479.png)

一种简单的保留（Retention）操作定义如下：

  $\text{Retention}(\{\mathcal{H}^{j}\}_{\forall j\in\mathbb{J}}, \gamma) = (QK^{\dagger}_{1} \odot D)V_{1} + \sum_{j = 2}^{J} (QK^{\dagger}_{j})V_{j} $ 

其中，$\mathcal{H}^{j}$是来自 $\chi^{j}$ 的映射隐藏信息向量。在此情境下， $\mathcal{H}^{j}$ 是拆分后的 $h$ 个头（heads）之一。

$Q\in\mathbb{R}^{P\times d}$、$K_{j}\in\mathbb{R}^{C_{j}\times d}$、$V_{j}\in\mathbb{R}^{C_{j}\times d}$ 分别表示查询（Query）向量、键（Key）向量和值（Value）向量，

$W_{Q}$、$W^{j}_{K}$、$W^{j}_{V}\in\mathbb{R}^{d\times d}$是可学习的参数，

$\Theta^{j}\in\mathbb{R}^{C_{j}\times d}$ 是一个包含位置信息的复矩阵，

$\Theta^{j}_{n}$表示信息源 $j$、序列索引为 $n$ 时的复数向量，

$\overline{\Theta}^{j}$ 是 $\Theta^{j}$ 的复共轭，

$D\in\mathbb{R}^{C_{1}\times C_{1}}$ 将因果掩码和沿相对距离的指数衰减合并为一个矩阵，

$\gamma$ 是折扣因子，它是一个标量，在 $D_{nm}$ 中以 $n - m$ 次幂的形式出现，用于对沿序列长度方向的相对重要性进行编码，

$\dagger$ 表示共轭转置。 

值得注意的是，复矩阵是不可学习的，它们起着嵌入位置信息的参数作用。此外，矩阵 $D$ 反映了时间依赖性。如果索引 $n$ 和 $m$ 足够接近，折扣因子就会相对较大，因而也就更重要，反之亦然。交通速度特征存在时间依赖性。然而，这种依赖性并不延伸到其他输入特征上，因为其他输入特征本质上是静态的——它们不会随时间变化，所以不具备时间维度。因此，在该模型中，对于这些静态输入而言，不需要与之相关联的时间依赖矩阵 $D$。 多尺度注意力（MSR）机制会为每个头（head）分配不同的 $\gamma$（折扣因子）。Swish门（激活函数）用于捕捉非线性特性。 

![image-20241120102357677](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202411201024824.png)

其中，$h$ 是头（head）的索引，

$\vert h\vert$ 表示头的数量，且 $\vert h\vert = d_{model} / d$，

$\mathcal{H}^{hj}$ 是拆分后的头，

$h$ 是头的集合，$d$ 是头的维度，

$W_{G}$、$W_{O} \in \mathbb{R}^{d_{model} \times d_{model}}$ 是可学习的参数，

$GroupNorm_{\vert h\vert}$ 是具有 $\vert h\vert$ 个头的组归一化（Group Normalisation）操作，

$swish$ 是一种激活函数。 

$\chi_{j}$ 的最后两个维度被扁平化处理，使得$\chi_{j} \in \mathbb{R}^{C_{j} \times (N \times N)}$。

保留网络（Retentive Network）共有\(L\)层。该架构设计如下： 

![image-20241120103440489](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202411201034564.png)

其中，$LN(\cdot)$ 表示层归一化（Layer Normalisation），

$W_{6}$、$W_{7} \in \mathbb{R}^{d_{model} \times d_{model}}$ 是可学习的参数，$gelu$ 是一种激活函数。 

保留网络（Retentive Network）模型的计算复杂度为 $O(N)$  [71]。它具有线性复杂度，因此相较于标准的Transformer来说，其计算量更小。 

> 输入：（B F T）
>
> 输出：（B $F^{\prime}$ $T^{\prime}$）
>
> ```
> Parallel (default) representation of the retention mechanism.
> X: (batch_size, sequence_length, hidden_size)
> ```

### 4.5 用于时间融合的门控循环单元

所得到的输出 $H^{1}_{L + 1}$ 会被传入一个门控循环单元（Gated Recurrent Unit，简称GRU），作为输入进行进一步处理。需要注意的是，$H^{1}_{L + 1}$ 由 $P$ 个时间步组成。我们使用 $H_{t}$ 来表示时间步 $t$ 处的隐藏信息，这样 $H^{1}_{L + 1}=(H_{1}, \ldots, H_{P})$。令 $h_{t - 1} \in \mathbb{R}^{d_{model}}$ 表示前一个隐藏状态，$h_{t}$ 表示当前隐藏状态，并且 $r_{t}$、$z_{t}$ 分别表示重置门（reset gate）和更新门（update gate）。时间 $t$  处的候选激活（candidate activation）用 $\tilde{h}_{t}$ 表示。权重矩阵为 $W_{r}$、$W_{z}$ 和 $W_{8}$（它们的维度均为 $\mathbb{R}^{d_{model} \times (2d_{model} + 1)}$），偏置向量为 $b_{r}$、$b_{z}$和 $b$。门控循环单元（GRU）的计算公式如下： 

![image-20241123201124716](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202411232011820.png)

其中，$\sigma$ 表示Sigmoid激活函数，

$\tanh$ 是双曲正切函数，

$*$ 表示逐元素乘法，

$\chi_{POI}^{x,y,t} \in \mathbb{R}$ 表示在时间步 $t$ 时感兴趣点 $(x,y)$ 处的速度信息（不包括相邻点的信息）。 

最后，我们通过应用一个前馈网络在坐标 $ (x,y)$ 处输出结果 $\hat{Y}_{x,y}$ 。 

$\hat{Y}_{x,y} = h_{P}W_{9} + b_{f}$，

其中 $W_{9} \in \mathbb{R}^{d_{model} \times R}$，

$\hat{Y}^{P + 1 + \tau}_{x,y} \in \mathbb{R}^{R}$ 是预测输出。标签 $Y^{P + 1 + \tau}_{x,y}$ 与 $\hat{Y}^{P + 1 + \tau}_{x,y}$ 之间的差异应当被最小化。 门控循环单元（GRU）的计算复杂度为 $O(N^{2})$。因此，整体的计算复杂度同样为 $O(N^{2})$。与其他方法相比，所提出的框架并不会降低复杂度。 















## 五、实验

我们使用英特尔至强金牌 6226R 处理器以及英伟达特斯拉 V100（32GB SXM2）图形处理器开展计算实验。本节将介绍我们所使用的数据集、实验设置、超参数、评估指标以及实验结果。

### 5.1 数据集

在我们的研究中，我们使用一个来自北京的公开可用交通速度数据集对新提出的异构保留网络（H-RetNet）与门控循环单元（GRU）相结合的方法的性能进行了评估。城区被系统地划分为网格模式，整个城市被划分为500×500个网格。每个网格单元代表该特定区域的平均交通速度。该数据集以5分钟为间隔记录交通速度，即$\Upsilon  = 5$ 分钟，这样每天就会产生 $T = 288$ 个样本。 

在我们的计算实验中，对于预测步长有两种配置情况。在第一种配置中，为了与文献[72]中提供的基准保持一致，我们将参数调整为 $P = 27$ 以及 $Q = 9$。在第二种配置中，为了与主流文献相符，我们将参数调整为 $P = 12$ 且$ Q = 12$ 。这种设置利用前135分钟的速度数据来预测接下来45分钟的交通状况。此外，该数据集明确了以下常量：$C^2 = 1$ ，$C^3 = 23$ ，$C^4 = 32$，以及$C^5 = 1$。 

鉴于同时预测全部25万个网格的交通速度颇具挑战性，我们采用了一种顺序方法，逐个网格地预测近期未来的交通速度。这种方法确保每个网格都能被单独考量。而且，我们对时间维度进行了组织安排，使得第一个训练样本包含从 $T = 1$ 到 $T = 36$ 的数据，后续样本则从 $T = 2$ 到 $T = 37$，依此类推。这样在时间维度上就有253个样本，在空间维度上有25万个样本，总计大约有 $63.25×10^6$ 个样本。交通速度始终为零的网格被排除在分析之外。我们将这些样本划分为训练集（占70%）、验证集（占15%）和测试集（占15%），以便于预测模型的开发和评估。 

除此之外，我们的实验分析还纳入了来自不同城市环境的数据集（正如近期研究[29, 73]所引用的那样），涵盖了全面的城市数据范围。其中包括28,550条房地产挂牌信息、497,256个兴趣点以及超过1亿条带有地理标记的社交媒体记录。这些异构数据集被整合在一起，形成了一个丰富的背景信息库，将房地产信息、社交媒体活动以及兴趣点信息融为一体。 

+ **社交媒体文本**：从各类社交媒体平台收集到的文本数据会使用由德夫林等人[74]提出的预训练BERT模型进行处理。这会为每个空间网格单元生成一个多维向量表示，将文本信息封装其中。再通过由鲁梅尔哈特等人[75]提出的预训练自编码器实现进一步的降维，生成一个压缩的多通道矩阵，该矩阵构成了训练数据集的一部分。该数据集包含460个文本文件，其中有来自北京的超过1亿条微博帖子。每个文件包含从2013年9月12日至2015年4月20日按日发布的帖子，每行都包含以空格分隔的微博内容、位置坐标以及发布日期和时间等数值。
+ **房地产**：每个网格单元内房地产房源的平均价格可作为衡量当地生活水平的一项指标。该指标经过归一化处理后被转换为一个向量，用于表征每个网格单元的房产价格特征。若某个网格单元没有房产数据，则会被赋予空值。这些向量的汇总形成了一个单通道矩阵，用以描绘房地产相关特征。 
+ **兴趣点**：该类别中的数据涵盖了23种不同的类型，能够提供一系列关于每个网格单元内设施及景点情况的信息。采用独热编码（one-hot encoding）方法将兴趣点的分类数据转换为每个网格单元对应的向量。这些向量的聚合生成了一个多通道矩阵，该矩阵展示了兴趣点在整个网格中的分布情况。
+ **交通数据**：交通数据存储在一个Pickle文件中，包含针对北京不同区域的250,000个元素。每个元素包含216个值，这些值代表着以千米/小时为单位的日交通速度，记录的是从早上6点到中午12点每隔5分钟的交通速度情况。 

### 5.2 实验设置和超参数

按照既定的基准参数，我们对实验设置进行了如下配置：输入序列长度设为27个历史步长，输出序列长度设为9个预测步长。 我们将网络的隐藏层大小设为256，并采用大小为50的批次进行训练。我们模型的架构在H - 保留网络（H - RetNet）和门控循环单元（GRU）组件中均包含3层。在主要训练阶段，我们对模型进行18542次迭代训练，另外还有17115次迭代专门用于测试，以此确保所学模式的稳健性和泛化能力。训练过程设定最长训练时长为5小时，以此维持计算效率，而全局模型的结果是从一篇未采用训练时间限制的现有论文中获取的。我们采用亚当（Adam）优化器进行训练，初始学习率设为 $1×10^{-3}$，并应用0.1的衰减因子在训练期间动态调整学习率。均方误差（Mean Square Error，简称MSE）准则被用作损失函数，引导优化过程朝着准确预测的方向进行。为防止过拟合并促进模型泛化，在整个网络各层中采用了20%的丢弃率（dropout rate）。 上述超参数及设置汇总于表3中。 

![image-20241124202757496](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202411242028653.png)

### 5.3 评价指标



### 5.4 对比结果

我们将所提出的模型H - RetNet + GRU与近期文献中已发表的主流模型进行比较。这些具有代表性的模型目前被视为解决交通预测问题的前沿方法。我们的对比分析如下：

- **MLP + LSTM（全局感知）**：首先，我们使用多层感知机（MLP）模型将全局元表示信息展平，然后利用标准的长短期记忆网络（LSTM）捕捉时间依赖性。 
- **MLP + GRU（全局感知）**：同样先通过MLP模型将全局元表示信息展平，接着采用标准的门控循环单元（GRU）来捕捉时间依赖性。
- **MLP + LSTM（区域感知）**：在此配置下，神经网络不会感知全局信息，而仅关注周边区域。我们用MLP模型将区域感知元表示信息展平，随后由LSTM进行处理，以识别每个区域特有的时间模式。 
- **MLP + GRU（区域感知）**：在这种设置中，我们利用MLP模型将区域感知元表示信息展平，然后由GRU对这些数据进行分析，以理解区域时间动态变化。
- **CNN + GRU（全局感知）**：卷积神经网络（CNN）对全局信息进行处理，以捕捉空间依赖性，随后由GRU对时间维度进行建模。此方法基于周等人[72]的研究成果。 

- **GNN + GRU（区域感知）**：图神经网络（GNN）对区域感知信息进行处理，以捕捉空间依赖性，随后由门控循环单元（GRU）对时间维度进行建模。 该方法基于郑等人[63]的研究成果。 

![image-20241230185136641](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202412301851793.png)

> 我们的预测模型与现有算法对比（27 步到 9 步设置下）的性能表现.
>
> 测试集是在一台高性能设备上进行测试的，该设备的运行速度比我们的设备快得多。
>
> 无可用数据（N/A）：参考文献中未提供平均绝对百分比误差（MAPE）值。
>
> 最佳结果以粗体突出显示，第二好的结果以斜体表示。

![image-20241230185531689](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202412301855862.png)

> 我们的预测模型相较于现有算法的性能表现（12步到12步情形） 

在我们的对比结果分析中，如表4和表5所示，我们观察到不同模型配置在交通预测任务中的表现各异。鉴于当前常用的12步到12步的实验设置，我们在两种预测步长设置下进行了对比实验：27步到9步以及12步到12步。 从实验结果来看，H - RetNet与GRU相结合，尤其是针对区域数据进行配置时，相较于已有的算法展现出卓越的性能。在平均绝对误差（MAE）、平均绝对百分比误差（MAPE）、均方误差（MSE）、均方根误差（RMSE）以及R²等多个指标上，这种组合优于传统的时间序列模型，如MLP + LSTM（全局）、MLP + GRU（全局）、CNN + GRU（全局），以及其他区域感知算法，如MLP + LSTM（区域）和MLP + GRU（区域）。我们承认，尽管我们的模型在均方误差（MSE）和其他性能指标上表现相对较低，但报告的平均绝对百分比误差（MAPE）值却异常高。我们将这种差异归因于MAPE指标对实际值的固有敏感性，特别是当实际值接近零时。我们的数据集包含北京的交通速度测量数据，其中在高峰拥堵时段有大量极低的值，并且在道路上无车辆时，交通速度会记录为0。这些低数值和零数值的数据点会扭曲MAPE的计算，导致百分比误差人为地升高。 值得注意的是，无论设置为27步到9步还是12步到12步，我们的方法在所有评估指标上均优于其他方法。 H - RetNet + GRU（区域）模型的结果尤为显著，反映出其增强的预测能力。H - RetNet与GRU的整合使得对区域动态有更细致的理解，这在空间关系对数据结构有显著影响的场景中至关重要。据我们所知，图神经网络（GNN）是一种在交通预测任务中广泛应用的方法。 虽然在使用GRU捕捉时间依赖性时，我们的方法在平均推理时间方面不如GNN，但在MAE、MAPE、MSE、RMSE和R²等指标上，它显著优于GNN。 城市交通系统是典型的复杂网络，理解其整体结构需要对长距离空间依赖性进行建模。例如，在预测市中心的交通流量时，有必要考虑郊区的交通状况，因为从郊区到市中心的通勤流量会显著影响市中心的交通。H - RetNet通过保留历史注意力分布来明确地对长距离依赖性进行建模，从而应对这一挑战。此外，H - RetNet的工作记忆机制显著增强了对这些依赖性的建模能力，在计算效率、鲁棒性等方面进行了优化。因此，我们的方法取得了显著的改进。 



### 5.5 消融实验

为了证明我们所提出模型的有效性，我们开展了一系列广泛的消融实验，重点研究社交媒体、房地产以及兴趣点表征的各自贡献。各部分具体含义如下：

- **区域感知表征**：此方法利用卷积神经网络（CNNs）处理地图上的区域空间数据，包括社交媒体文本、兴趣点和房地产信息，然后对其进行压缩，并输入到门控循环单元（GRU）中以生成速度预测结果。
- **位置感知表征**：它采用高斯掩码将特定位置信息融入模型。‘
- **时间感知表征**：该方法利用先前时间框架的历史交通速度数据来进行预测。
- **社交媒体表征**：此方法利用卷积神经网络（CNNs）处理地图上的社交媒体文本，然后对其进行压缩，并输入到门控循环单元（GRU）中以生成速度预测结果。
- **房地产表征**：此方法利用卷积神经网络（CNNs）处理地图上的社交媒体文本，然后对其进行压缩，并输入到门控循环单元（GRU）中以生成速度预测结果。
- **兴趣点表征**：此方法利用卷积神经网络（CNNs）处理地图上的社交媒体文本，然后对其进行压缩，并输入到门控循环单元（GRU）中以生成速度预测结果。 

我们在两种预测步长设置下进行了对比实验：27步到9步以及12步到12步。表6展示了聚焦于不同感知表征方法对预测模型性能影响的消融研究。

![image-20241230190154957](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202412301901055.png)

表7和表8展示了聚焦于多模态信息对预测模型性能影响的消融研究。结果表明，每个不完整的模型都出现了不同程度的精度下降，这突出了每种表征在捕捉时空相关性方面的重要作用。 

![image-20241230190248542](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202412301902634.png)

![image-20241230190255901](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202412301902001.png)

对于感知表征组件的去除实验，去掉时间感知模块会导致精度下降最为显著。这表明时间相关性对交通预测的影响最为重大。值得注意的是，去掉区域感知模块同样会使精度大幅降低。这意味着整合了社交媒体文本、兴趣点以及房地产信息的区域感知模块，对完整模型的出色表现有着重大贡献。去掉位置感知组件并未导致模型整体性能显著下降，但该组件的加入仍对模型起到了有益作用。 在两种不同预测步长设置下的分析中，我们注意到了相似的结果。具体而言，去掉社交媒体表征会导致预测精度下降最为明显，这凸显了其在交通预测模型整体有效性中的关键作用。同样，去掉房地产表征也会使精度显著降低，证实了它们对模型卓越性能的重要贡献。这一观察结果强调了模型中的每个组件都对提升整体性能有所贡献，凸显了所采用的多种数据表征的综合优势。 

![image-20241230190647702](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202412301906833.png)

图5以箱线图展示了不同消融研究中的速度预测对比情况。结果发现，我们模型的输出与实际值更为接近。 

![image-20241230190752681](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202412301907763.png)

表9显示，就均值（31.18对比30.68）、标准差（16.40对比18.64）以及关键百分位数（尤其是第50百分位数，27.17对比27.00）而言，我们的模型H - RetNet + GRU的表现与实际值最为接近。虽然最小值和最大值与实际值存在一定偏差，但鉴于极端情况的复杂性，这是可以预料的。总体而言，与其他模型相比，我们的模型在主要统计指标上展现出了卓越的一致性。 实验表明，我们所提出的利用多种表征的模型，能够有效挖掘交通数据中的空间相关性，并取得准确的预测结果。 

### 5.6 敏感性测试

异构RetNet + GRU模型的敏感性分析聚焦于多种配置，包括层数、批量大小、学习率以及网格大小。此次详尽的研究旨在优化模型设置，以提高交通速度预测的准确性和效率（见表10 - 13）。 

+ **网格大小的影响**：分析表明，50×50的网格大小在计算效率与准确性之间达到了最佳平衡，实现了最低的预测误差以及最高的决定系数，$R^2$ = 0.75。更大的网格大小并不能提升性能，而且由于5小时训练时间限制过早截止，可能会导致效率低下。相反，较小的网格大小虽然能给出不错的结果，但无法捕捉到足够的时空信息，因此50×50的网格大小为最优选择。 
+ **层数的影响**：三层结构展现出最佳性能，能实现最准确的预测，误差（平均绝对误差MAE、均方误差MSE、均方根误差RMSE）最低，$R^2$ 值最高。将层数增加到四层可能会引发过拟合，而减少到两层则会略微降低模型的学习能力。
+ **批量大小的影响**：批量大小为50时性能最佳。将批量大小增加到100可能会引发过拟合，误差指标上升就是证明。相反，将批量大小减少到25会略微降低模型的学习能力，导致更高的MAE和MSE值。 
+ **学习率**：学习率为1e - 3被确定为最优。向任何一侧变动都会导致性能变差。 

![image-20241230191359543](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202412301913720.png)

### 5.7 预测结果与真实值的可视化

我们通过图6和图7对模型的预测结果与真实值进行可视化展示，呈现不同区域在不同时间间隔的交通状况。这些图包含一系列子图，每个子图对应未来15分钟、30分钟和45分钟这几个不同时间点的交通状况。这些可视化内容特别聚焦于特定区域子集，分别由坐标[0∶500, 0∶500]、[250∶400, 250∶400]以及[120∶140, 160∶180]划定范围。 每个子图采用颜色编码方案来展示交通速度，即整个路网的平均车速。这种颜色渐变能有效地突出拥堵区域和畅行区域，让人一眼就能直观地了解模型预测的准确性。 

![image-20241230191714939](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202412301917231.png)

>  不同区域及时间间隔的预测可视化展示

![image-20241230191736403](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202412301917704.png)

> 不同区域和时间间隔的真实值可视化展示。

### 5.8 计算效率研究

我们将所提出的模型H - RetNet + GRU的推理时间效率与几个主要的基准模型以及其他区域/全局模型进行评估对比。基准模型包括MLP + LSTM（全局）、MLP + GRU（全局）、MLP + LSTM（区域）、MLP + GRU（区域）以及CNN + GRU（全局）。我们的算法将GRU与H - RetNet架构相结合，专注于区域交通数据，而基准模型则在全局或区域层面采用MLP、LSTM、GRU和CNN的各种组合。 为确保公平比较，所有模型均在相同数据集上进行评估，定量结果如图8所示。H - RetNet + GRU的推理时间明显少于区域模型MLP + LSTM和MLP + GRU，分别减少了13.07%和1.12%。与包括MLP + LSTM、MLP + GRU和CNN + GRU在内的全局模型相比，我们的模型在推理时间方面效率略低。这是因为全局模型的测试集是在高性能设备上评估的，该设备的运行速度比我们的设备更快。这意味着在标准化设备上，这些模型的推理时间可能会更长。 总之，与现有的基准模型相比，我们的模型在计算效率方面展现出显著的竞争力，尤其是在区域预测方面。 

![image-20241230192248093](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202412301922196.png)

### 5.9. 空间复杂度研究 

![image-20241230194156326](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202412301941408.png)

为探究我们模型的空间效率，我们针对模型大小所体现的空间复杂度，将其与几个著名的基准模型进行了对比分析。图9描绘了模型大小与准确率之间的关系，其中准确率通过均方根误差（RMSE）来衡量。每个模型的RMSE也列于表14中。我们可以看到，MLP + GRU（区域）模型的大小最小，但在准确率方面表现并不理想。 CNN + GRU（全局）以相对较小的模型大小实现了稳健的预测准确率。H - RetNet + LSTM在对比组中表现位居第二，尽管其空间复杂度较高。特别值得注意的是H - RetNet + GRU模型，它在不相应增加模型维度的情况下，展现出了较高的预测性能。总之，我们的模型在空间复杂度和性能之间实现了有效平衡。 

![image-20241230194235869](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202412301942046.png)

### 5.10. 训练损失与验证损失 

![image-20241230194456392](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202412301944508.png)

如图10中H - RetNet算法的训练损失曲线所示，随着迭代次数的增加，模型的训练损失逐渐降低，这表明模型的学习过程有效，且收敛模式可靠。该曲线显示，原始训练损失在初始阶段存在波动，这是常见现象，归因于所采用优化算法的随机性以及交通数据本身的复杂性。平滑后的训练损失总体呈下降趋势。 损失值从一个相当高的起点开始，随着优化过程在数据集上循环，迅速减小，促使模型参数不断优化。接近第2000次迭代时，平滑后的损失曲线开始趋于平稳，这表明模型正在接近与训练数据集范围相符的优化状态。在此之后，曲线趋于稳定，保持在一个持续较低的损失值。平滑损失的这一趋势表明模型正稳步朝着稳定的最优解推进。 验证损失仅在最新的验证损失小于前一次验证损失时才会更新。

![image-20241230194552325](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202412301945419.png)

图11展示了完整的验证损失曲线。我们发现，由于波动的存在，在整个训练过程中仅更新了16次。验证损失稳定在95左右。 综上所述，训练损失曲线和验证损失曲线反映出训练阶段进展顺利，模型的训练损失大幅下降，并呈现出收敛到稳定解的迹象。 

## 六、总结与未来工作

综上所述，我们的研究引入了一种H - RetNet与GRU相结合的神经网络架构。H - RetNet是一种新型模型，通过整合多模态数据来提升交通速度预测能力。这种方法有效解决了以往交通预测方法的局限性，包括泛化能力不足以及无法统一不同数据模态等问题。H - RetNet已证明其能够整合大规模地图上的综合信息，显著提高交通场景预测的准确性。结果表明，在交通预测方面，H - RetNet超越了所有其他专业的多模态交通预测模型，并且在识别拥堵点方面展现出最高的准确性。 在城市基础设施迅速扩张以及各种数据源日益丰富的背景下，H - RetNet在单一框架内处理和融合多模态数据的能力尤为可贵。我们的模型简化了预测分析流程，无需针对不同数据类型使用多个专门模型。借助H - RetNet，相关方能够依靠一个统一且适应性强的解决方案，实现对各种城市环境的交通预测。通过实现更高效的路线规划以及降低交通拥堵对经济的影响，我们的模型朝着打造更智能、响应更迅速的城市交通系统迈出了重要一步。 关于未来的工作，我们旨在通过探索更优的架构和先进的训练技术，继续优化H - RetNet，以进一步提高预测准确性和模型的稳健性。我们还计划纳入实时数据源，如天气预报和活动日程安排等，这将使交通预测更具动态性和准确性，以反映不断变化的城市环境。 
