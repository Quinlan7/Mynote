# 毕业论文

## 一、第一个模型

+ 图输入改为动态图，图卷积 改为 动态图卷积      √

  + 先空间卷积，还是先动态图卷积
  + 弄明白动态图卷积是干啥的

+ 还是 $X_r$、$X_d$、$X_w$ 三种输入，
  + t 和 m 拆开分别输入不同的模型中       √

    > t 和 m 拆开 输入模型中
    >
    > ![image-20240618210528491](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202406182105743.png)
    >
    > 问题： 作者的数据集中训练输入有两个维度的数据是交通流数据，一个是流入一个是流出，但是标签数据中只有流出车流量数据的预测值，训练结果也只有流出车流量，我现在输入的时候 流入车流量的数据集 是当作 多模态信息还是 主要特征的数据。
    >
    > HaiKou
    >
    > | 模型\结果                        | MAE    | RMSE   | MAPE   |
    > | -------------------------------- | ------ | ------ | ------ |
    > | MFFDSTGCN-850epoch               | 1.5145 | 5.3820 | 0.4540 |
    > | MFFDSTGCN-850epoch-noAirQuality  | 1.6940 | 6.0739 | 0.4736 |
    > | MFFDSTGCN-850epoch-noHoliday     | 1.5949 | 5.5003 | 0.4697 |
    > | MFFDSTGCN-850epoch-noWeather     | 1.6443 | 5.7489 | 0.5271 |
    > | MFFDSTGCN-850epoch-noWind        | 1.8200 | 6.7797 | 0.5135 |
    > | MFFDSTGCN-850epoch-noTemperature | 2.4077 | 8.9624 | 0.5633 |
    > | MFFDSTGCN-2000epoch              | 1.1035 | 3.5433 | 0.4796 |
    >
    > HaiKou-ablation
    >
    > + MFFDSTGCN-850epoch-noMFFmoudle：在这个实验中我们移除掉多模态特征融合模块（图卷积部分），只是把提取完特征的交通流信息和多模态信息叠加到一起
    > + MFFDSTGCN-850epoch-noMImoudle：在这个实验中，我们移除掉专门处理多模态信息的并行模块，我们直接把辅助信息叠加到我们的交通流信息上，来验证这个并行模块的作用
    >
    > | 模型\结果                              | MAE    | RMSE   | MAPE   |
    > | -------------------------------------- | ------ | ------ | ------ |
    > | MFFDSTGCN-850epoch                     | 1.5145 | 5.3820 | 0.4540 |
    > | MFFDSTGCN-850epoch-noMFFmoudle         | 1.7550 | 6.1579 | 0.4955 |
    > | MFFDSTGCN-850epoch-noMImoudle-allInfor | 1.3775 | 4.0976 | 0.4490 |
    > | MFFDSTGCN-850epoch-noMImoudle-noInfor  | 2.2108 | 7.7609 | 0.5444 |
    > |                                        |        |        |        |
    >
    > ​                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       
    >
    > Yellowtrip
    >
    > | 模型\结果                    | MAE    | RMSE   | MAPE   |
    > | ---------------------------- | ------ | ------ | ------ |
    > | author                       | 2.18   | 6.27   | 无     |
    > | MFF-64B-dim32-runs1-850epoch | 1.5579 | 3.8317 | 0.4110 |
    > |                              |        |        |        |
    > |                              |        |        |        |

  + t 和 m 叠加输入相同模型                          √

    > a 和 p 叠加输入模型
    >
    > ![image-20240616160808741](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202406161608905.png)
    >
    > 目前效果 怎么说呢。通过实践发现，由于是动态图，所以需要训练的epoch次数要更大才能效果更好。
    >
    > | 模型\结果                     | MAE  | RMSE  |
    > | ----------------------------- | ---- | ----- |
    > | DGC-32B-dim32-runs1           | 2.48 | 7.30  |
    > | DGC-32B-dim32-runs10          | 2.45 | 10.38 |
    > | DGC-16B-dim32-runs1           | 2.30 | 8.24  |
    > | DGC-64B-dim32-runs1           | 2.02 | 6.96  |
    > | DGC-64B-dim32-runs10          | 2.59 | 10.44 |
    > | DGC-128B-dim32-runs1          | 8.79 | 36.76 |
    > | DGC-64B-dim32-runs1-1000epoch | 2.28 | 7.87  |
    >
    > 

+ 时间卷积改进为内部改进为膨胀卷积（或空洞卷积）          √

> 在 WAVENET 中首次提出。
>
> 查了几篇文章中关于时间特征提取的部分，一部分是使用的RNN（LSTM或者GRU）来提取的时间特征，但是这样会严重的降低训练速度，所以我使用了膨胀卷积。与循环神经网络相比，膨胀卷积层可以并行计算，因此大大降低了时间复杂度。同时，==门控机制==在处理序列数据方面显示出优势，因此在时间卷积层中使用了门控机制以提高模型容量。

+ 跳跃链接
+ 在测试集的训练结果中加上 mape  mae                 √

> mae 加入完成
>
> 但是mape 无法实现，mape如果真实值存在0的话，会出现除零问题，这个数据集很多0。所以无法使用

+ 评价标准 加入 MAPE                                        √

> 实现工作量大，因为作者使用的是没人用过的数据集，别的模型的实验结果也没有mape，如果你要实现 mape 的对比就要在其他所有的实验代码中加入，并且重新跑其他所有的实验代码（共十个）

+ 实现不同预测步长实验
> 要实现不同的预测步长，也需要重新运行haikou_gen_data_samples.py，重新生成 data_samples文件，才可以。

+ 改进 train.py 实现调用时可以传入关键参数，新写一个脚本调用 train.py，实现运行一次新的脚本，可以实现对于模型的不同参数的实验结果的保存     √

+ 下一步：查找关于对这些多模态辅助信息处理方式的论文

> 目前看到的论文 对于多模态信息的处理都很简单，直接编码。
>
> 有无更好的处理方式？
>
> #### ABSTGCN
>
> 时间、工作日标记和交通事故记录\
>
> + 时间
>
> > 第一个因素是对应于交通流时间戳的时间段（TiD）。考虑这一因素可以更好地区分早晚高峰以及白天和夜间的交通流趋势。==标记方法是将带时间戳的小时数加1再除以24，转换为标准化数据==。
>
> + 第二个因素是对应于交通流时间戳的星期几（DiW），用于区分交通流时间是否为工作日。目的是区分工作日和非工作日交通流的不同特征。
> + 第三个是交通事故标记，用于因交通事故导致的交通流突变。
>
> > 我的数据集中无相关数据
>
> #### MFFSTGCN
>
> 论文中可以先用数据证明这个辅助信息是对交通流有影响的
>
> 消融实验可以一个个的验证辅助数据的作用
>
> + 使用独热编码处理非数值型数据
> + 缺失值处理：线性插值、用前一个或者后一个填充
> + 数据归一化：如z-score归一化、min-max归一化、softmax归一化

+ 辅助信息数据源优化，并且考虑辅助信息影响的时间空间依赖性
  + 获取每个小时的辅助信息数据（与交通流量数据一致的时间间隔）
  + 获取不同的地理位置的辅助信息数据源

+ 输入输出通道数的拓展       ==直接在整个大模型外边使用通道扩展==

> + 仅将 时间卷积模块应用 通道扩展
>
> | 模型\结果             | MAE  | RMSE |
> | --------------------- | ---- | ---- |
> | dilateTconv           | 2.48 | 8.05 |
> | dilateTconv-16channel | 2.26 | 7.07 |
>
> 实验结果明显变好，输入输出通道拓展有明显作用
>
> + 将 通道拓展应用于整个模型
>
> 出现很多问题，训练 10 runs 时出现 OOM ，修改 runs ，先看一下效果如何 再决定是否解决 OOM 问题。
>
> 将 runs 修改为 1，2
>
> | 模型\结果                   | MAE  | RMSE  |
> | --------------------------- | ---- | ----- |
> | dilateTconv                 | 2.48 | 8.05  |
> | dilateTconv-16channel-1runs | 2.35 | 10.59 |
> | dilateTconv-16channel-2runs | 2.41 | 10.75 |
> | dilateTconv-8channel-2runs  | 2.38 | 10.75 |
>
> 

+ 时间注意力和空间注意力的优化方案

+ 原数据 车流量中零很多







### 10.28

#### 问题：

多模态信息为什么要怎么处理，这么处理有什么意义吗。

> 多模态信息处理流程：
>
> 1. 预处理：非数值型数据 -> 数值型数据
> 2. 特征融合：对于交通流的影响如何表示出来
>    1. when：什么时候进行特征融合？先融合再走模型还是先走模型再融合
>    2. How：怎么融合

![image-20241028160023357](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202410281600531.png)

改进之后

![image-20241205093711121](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202412050937303.png)





HaiKou

| 模型\结果                        | MAE    | RMSE   | MAPE   |
| -------------------------------- | ------ | ------ | ------ |
| MFFDSTGCN-850epoch               | 1.5145 | 5.3820 | 0.4540 |
| MFFDSTGCN-850epoch-noAirQuality  | 1.6940 | 6.0739 | 0.4736 |
| MFFDSTGCN-850epoch-noHoliday     | 1.5949 | 5.5003 | 0.4697 |
| MFFDSTGCN-850epoch-noWeather     | 1.6443 | 5.7489 | 0.5271 |
| MFFDSTGCN-850epoch-noWind        | 1.8200 | 6.7797 | 0.5135 |
| MFFDSTGCN-850epoch-noTemperature | 2.0077 | 6.9624 | 0.5633 |
|                                  |        |        |        |

HaiKou-ablation

+ MFFDSTGCN-850epoch-noMFFmoudle：在这个实验中我们移除掉多模态特征融合模块（图卷积部分），只是把提取完特征的交通流信息和多模态信息叠加到一起
+ MFFDSTGCN-850epoch-noMImoudle-noInfor：在这个实验中，我们移除掉所有多模态信息，来验证多模态信息的有效性。

| 模型\结果                             | MAE    | RMSE   | MAPE   |
| ------------------------------------- | ------ | ------ | ------ |
| MFFDSTGCN-850epoch                    | 1.5145 | 5.3820 | 0.4540 |
| MFFDSTGCN-850epoch-noMFFmoudle        | 1.7550 | 6.1579 | 0.4955 |
| MFFDSTGCN-850epoch-noMImoudle-noInfor | 2.2108 | 7.7609 | 0.5444 |
|                                       |        |        |        |

​                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     

Yellowtrip

| 模型\结果                    | MAE    | RMSE   | MAPE   |
| ---------------------------- | ------ | ------ | ------ |
| author                       | 2.18   | 6.27   | 无     |
| MFF-64B-dim32-runs1-850epoch | 1.5579 | 3.8317 | 0.4110 |
|                              |        |        |        |
|                              |        |        |        |

## 二、第二个模型

![image-20241205093801899](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202412050938999.png)

![image-20241205093825700](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202412050938834.png)

![image-20241205093900127](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202412050939222.png)

非线性时间交互：

$\mathcal{X}_{\text{odd\_upd}} = \Phi(\mathcal{X}_{\text{odd}}) \odot \operatorname{Softmax}\left(\frac{\Phi(\mathcal{X}_{\text{odd}}) \odot \mathcal{X}_{\text{even}}}{\sqrt{F}} + \sigma(W_{\text{odd}} \cdot \mathcal{X}_{\text{odd}})\right)$

$\mathcal{X}_{\text{even\_upd}} = \Gamma(\mathcal{X}_{\text{even}}) \odot \operatorname{Softmax}\left(\frac{\Gamma(\mathcal{X}_{\text{even}}) \odot \mathcal{X}_{\text{odd}}}{\sqrt{F}} + \sigma(W_{\text{even}} \cdot \mathcal{X}_{\text{even}})\right)$



基于注意力交互的GLU：改进

> 第三种方案是 **非线性时间加权机制（Nonlinear Temporal Weighting）**，它通过引入非线性激活函数来增强模型对时间序列中不同时间步之间关系的捕捉能力，特别是 **非线性依赖**。在时间序列任务中，某些时间步之间的依赖关系可能不是线性的，例如，一些事件可能在时间上有较长的延迟效应，而传统的线性模型可能无法捕捉到这些复杂的非线性关系。通过引入非线性加权机制，可以让模型更好地捕捉这类复杂的时序依赖。
>
> ### 1. **背景与动机**
>
> 在传统的图神经网络（GNN）或注意力机制中，通常使用线性关系来计算每个节点（或时间步）与其他节点的关系。而在很多实际的时序数据中（例如交通流量、气象数据等），不同时间步之间的关系可能是高度非线性的。为了处理这种情况，**非线性加权机制**可以通过引入非线性激活函数（如 **ReLU**、**sigmoid** 或 **tanh**）来调整时间步之间的权重，这样可以捕捉到复杂的时间依赖模式。
>
> ### 2. **如何实现非线性时间加权机制**
>
> 假设我们有一个时间序列数据 $\mathcal{X}_{\text{even}}$ 和 $\mathcal{X}_{\text{odd}}$，它们分别是通过奇偶下采样得到的特征。我们希望通过非线性方式加权这些特征之间的关系。
>
> #### 改进的公式：
>
> 我们可以在计算注意力权重时，引入一个非线性激活函数，来对每个时间步之间的依赖进行调整。具体来说，首先计算基于当前特征的线性加权，然后再通过激活函数对结果进行非线性转换，从而调整不同时间步之间的关系。
>
> - **Step 1**：首先对特征 $\mathcal{X}_{\text{even}}$ 和 $\mathcal{X}_{\text{odd}} $ 进行线性加权。这里，假设 $\Gamma$ 和 $\Phi$ 是可学习的卷积核或线性变换矩阵，$W_{even}$ 和 $W_{odd}$ 是学习的参数。
>
>   $Q_{\text{even}} = \Gamma(\mathcal{X}_{\text{even}}) = \mathcal{X}_{\text{even}} \cdot W_{even}$
>
>   $Q_{\text{odd}} = \Phi(\mathcal{X}_{\text{odd}}) = \mathcal{X}_{\text{odd}} \cdot W_{odd}$
>
> - **Step 2**：计算每个时间步的加权关系。这时，我们使用点积或者其他类似的操作来计算时间步之间的关系，之后加上一个基于特征的非线性加权因子。
>
>   例如，我们可以使用 **sigmoid** 或 **ReLU** 激活函数来处理每个时间步的加权结果，使得时间步之间的关系更加灵活：
>
>   $\mathcal{X}_{\text{even\_upd}} = \Gamma(\mathcal{X}_{\text{even}}) \odot \operatorname{Softmax}\left(\frac{\Gamma(\mathcal{X}_{\text{even}}) \odot \mathcal{X}_{\text{odd}}}{\sqrt{F}} + \sigma(W_{\text{even}} \cdot \mathcal{X}_{\text{even}})\right)$
>
>   这里， $\sigma(W_{\text{even}} \cdot \mathcal{X}_{\text{even}})$ 是通过 **sigmoid** 或 **ReLU** 激活函数调节的非线性时间加权项，它根据当前特征 $\mathcal{X}_{\text{even}}$ 的内容进行动态调整。
>
> - **Step 3**：类似地，针对 `odd` 的部分，我们也可以引入非线性加权机制：
>
>   $\mathcal{X}_{\text{odd\_upd}} = \Phi(\mathcal{X}_{\text{odd}}) \odot \operatorname{Softmax}\left(\frac{\Phi(\mathcal{X}_{\text{odd}}) \odot \mathcal{X}_{\text{even}}}{\sqrt{F}} + \sigma(W_{\text{odd}} \cdot \mathcal{X}_{\text{odd}})\right)$
>
> #### 解释：
>
> - **非线性激活**：$\sigma(W_{\text{even}} \cdot \mathcal{X}_{\text{even}})$ 和 $\sigma(W_{\text{odd}} \cdot \mathcal{X}_{\text{odd}})$ 是通过激活函数（例如 `sigmoid`、`ReLU` 等）对输入特征进行非线性变换后的结果。这些非线性变换允许模型根据当前的特征动态调整每个时间步之间的依赖关系。
> - **Softmax**：`Softmax` 用来归一化计算出的权重，以确保它们的和为1，这样可以得到每个时间步的重要性评分。
> - **加权更新**：使用 `\odot` 表示逐元素乘法，这意味着模型不仅结合了基于特征计算的加权信息，还结合了通过非线性激活得到的动态调整后的权重。
>
> ### 3. **非线性时间加权机制的优势**
>
> 通过这种非线性加权机制，模型能够：
>
> - **捕捉复杂的非线性关系**：通过使用非线性激活函数（如 `ReLU` 或 `sigmoid`），可以捕捉到时间序列数据中的非线性依赖。例如，某些事件之间可能存在复杂的非线性延迟效应，非线性激活能够帮助模型适应这些模式。
> - **增强时间步之间的动态依赖**：模型可以根据每个时间步的内容动态调整权重，使得远距离时间步之间的关系可以得到适当的建模，而不仅仅是依赖于邻近时间步。
> - **灵活性**：与传统的线性加权机制相比，非线性加权提供了更多的自由度，可以让模型更加灵活地适应不同类型的时序数据。
>
> ### 4. **如何选择激活函数（激活函数的选择）**
>
> - **sigmoid**：适合用于捕捉平滑的非线性关系，尤其是当期望时间步之间的关系有一定的限制时（例如，通过压缩权重范围到0和1之间来控制影响力）。
> - **ReLU**：适合捕捉突变的非线性关系，尤其适合处理稀疏数据或需要快速激活某些特定特征的场景。
> - **tanh**：适合于捕捉周期性的非线性关系，通常用来解决一些反向传播时梯度消失的问题。
>
> ### 总结：
>
> **非线性时间加权机制**通过引入非线性激活函数，可以使模型更加灵活地捕捉时序数据中的复杂依赖，尤其是那些非线性或具有长时延效应的依赖。通过调整时间步之间的权重，模型能够动态地适应数据中的不同模式，从而提高预测和建模能力。



## 三、写作问题

1. 证明多模态信息的影响(引言部分)，这部分内容要写在哪里？
2. 创新点（引言） 写在哪里？
3. 3.2.2.1 预处理中编码映射的映射表要不要提供，提供的话七十种映射怎









> 数据集信息：
>
> | Datasets | Samples | Nodes | Sample Rate | Time Span |
> | -------- | ------- | ----- | ----------- | --------- |
> | Haikou   | 2208    | 195   | 1 hour      | 3 months  |
> | New York | 1416    | 261   | 1hour       | 2 months  |
>
> 以下是这三个评估指标在 LaTeX 格式下的公式： 
>
> 均方根误差（RMSE）： $RMSE=\sqrt{\frac{1}{n}\sum_{i = 1}^{n}(y_{i}-\hat{y}_{i})^{2}}$ 
>
> 平均绝对误差（MAE）： $MAE=\frac{1}{n}\sum_{i = 1}^{n}\vert y_{i}-\hat{y}_{i}\vert$ 
>
> 平均绝对百分比误差（MAPE）： $MAPE=\frac{1}{n}\sum_{i=1}^{n}\left\vert\frac{y_{i}-\hat{y}_{i}}{y_{i}}\right\vert\times 100\%$