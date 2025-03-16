# Multiple Information Spatial–Temporal Attention based Graph Convolution Network for traffic prediction

基于多信息空时注意力的图卷积网络用于交通预测 2023

![image-20231221092639694](C:\Users\zhf\AppData\Roaming\Typora\typora-user-images\image-20231221092639694.png)

## 摘要

*Traffic prediction (forecasting) is a key problem in intelligent transportation. It helps engineers to obtain traffic trends in advance so that they can make favorable decisions quickly and accurately, or even improve the working mode of existing systems. However, it is very challenging to design a model for such problem that fully utilize the factors related to traffic. This paper investigates machine learning in traffic prediction and proposes Multiple Information Spatial–Temporal Attention based Graph Convolution Networks (MISTAGCN). The model consists of two parts. The first part utilizes combinations of different inputs and graph structures to compute the corresponding latent variables. In the second part of the model, the l**atent variables** are comprehensively integrated and deeply mined. In particular, as the basic component of the model, a STBlock integrates mechanisms such as temporal attention, spatial attention, graph convolution, ordinary convolution and residual connections to fully explore the potential information contained in the data. Experiments on Haikou online carhailing dataset and New York yellow taxi trip dataset illustrate that the proposed model outperforms state-of-art baseline models.*

交通预测（预测）是智能交通领域的一个关键问题。它帮助工程师提前获取交通趋势，使他们能够迅速准确地做出有利的决策，甚至改进现有系统的工作模式。然而，设计一个能充分利用与交通相关的因素的模型对于这样的问题来说非常具有挑战性。本文研究了机器学习在交通预测中的应用，并提出了基于多信息空时注意力的图卷积网络（MISTAGCN）。该模型由两部分组成。第一部分利用不同输入和图结构的组合来**计算相应的潜在变量**。在模型的第二部分，**潜在变量被综合整合和深度挖掘**。特别是作为模型的基本组件，STBlock整合了诸如时间注意力、空间注意力、图卷积、普通卷积和残差连接等机制，以充分探索数据中包含的潜在信息。对海口在线打车数据集和纽约黄色出租车行程数据集的实验证明，所提出的模型优于现有基线模型。

## 一、介绍

*Spatial–temporal data prediction is one of the most challenging components in many research fields, such as transportation [1,2], ecology & environmental management [3], earth science [4] and climatology [5], etc. Traffic prediction is a special case of spatial–temporal data prediction, which aims to predict a sequence of traffic features in the future, given historical traffic records*

~~空时数据预测是许多研究领域中最具挑战性的组成部分之一，例如交通[1,2]、生态与环境管理[3]、地球科学[4]和气候学[5]等。交通预测是空时数据预测的一种特殊情况，其目标是在给定历史交通记录的情况下，预测未来一系列交通特征。~~

*Modern transportation systems encompass road vehicles, rail transport, as well as various emerging shared travel modes, such as online ride-hailing, bike-sharing, and e-scooter sharing, etc. [6].Thanks to the dynamic and uneven distribution of traffic, expanding cities face many transportation related problems, such as low resource utilization, air pollution and traffic congestion, etc. An accurate and reliable traffic prediction model assists in proactive and dynamic traffic control as well as intelligent route guidance, which will help alleviate the huge congestion problem in Intelligent Traffic System (ITS) [7].*

~~现代交通系统涵盖了道路车辆、铁路交通，以及各种新兴的共享出行方式，如在线打车、共享单车和电动滑板车等[6]。由于交通的动态和不均匀分布，不断扩张的城市面临着许多与交通相关的问题，例如低资源利用率、空气污染和交通拥堵等。准确可靠的交通预测模型有助于积极动态地进行交通控制和智能路径引导，这将有助于缓解智能交通系统（ITS）中的严重交通拥堵问题[7]。~~

Previous work on this topic falls into two categories: classical statistical methods [8–11] and machine learning models [1,2,4,12–19]. Most of the statistical methods assume that data samples follow some distributions and their relationship are linear, while the real traffic is too complex to satisfy these assumptions. Machine learning methods, on the other hand, can capture complex nonlinear relationships embedded in large amount of traffic data. Most of recent work on machine learning focuses on the application of Convolution Neural Networks (CNNs), Recurrent Neural Networks (RNNs), Graph Convolution Networks (GCNs) and combination of spatial and temporal attention with these models [1,2,4,13,15–17], thanks to their capability of better capturing spatial and temporal relationship in traffic data

~~过去针对这个主题的研究可以分为两类：经典的统计方法[8–11]和机器学习模型[1,2,4,12–19]。大多数统计方法假设数据样本遵循某些分布，并且它们的关系是线性的，然而真实的交通数据过于复杂，无法满足这些假设。另一方面，机器学习方法可以捕捉大量交通数据中嵌入的复杂非线性关系。最近的机器学习工作主要集中在应用卷积神经网络（CNNs）、循环神经网络（RNNs）、图卷积网络（GCNs）以及将空间和时间注意力与这些模型结合的方法上[1,2,4,13,15–17]，因为它们能更好地捕捉交通数据中的空间和时间关系。~~

*In this paper, in order to further utilize available information and improve prediction accuracy of traffic data, we construct Multiple Information Spatial–Temporal Attention based Graph Convolution Network (MISTAGCN). The proposed model consists of two parts, the first part utilizes several combinations of inputs and graph structures, applies Spatial Attention (SA), Temporal Attention (TA), convolution over time and graph convolution networks, etc. to generate the latent variables; the second part utilizes a combination of the latent variables and follows a similar process, to generate prediction of the target traffic.*

为了进一步利用可用信息并提高交通数据的预测准确性，本文构建了基于多信息空时注意力的图卷积网络（MISTAGCN）。所提出的模型包括两个部分，第一部分利用多种输入和图结构的组合，应用空间注意力（SA）、时间注意力（TA）、时间卷积和图卷积网络等方法生成潜在变量；第二部分利用潜在变量的组合并遵循类似的过程，生成目标交通的预测。

主要贡献：

+ A novel deep learning model is designed to organize and process historical traffic records in traffic networks and predict future traffic trends.
+ Novel formulas involving trainable parameters are devised to calculate spatial and temporal attentions. Furthermore, a class of STBlocks is designed to integrate the novel spatial and temporal attentions, graph convolution, ordinary convolution over time and residual connection, etc. which acts as the only basic component in the main structure of MISTAGCN.
+ Extensive experiments on two open datasets are conducted to verify the competitiveness of this model compared with state-of-art baseline models.

+ 设计了一种新颖的深度学习模型，用于组织和处理交通网络中的历史交通记录并预测未来的交通趋势。
+ 创造了涉及可训练参数的新型空间和时间注意力计算公式。此外，设计了一类STBlock，用于整合新颖的空间和时间注意力、图卷积、随时间的普通卷积和残差连接等，它作为MISTAGCN主体结构中唯一的基本组件。
+ 在两个开放数据集上进行了大量实验证明了与最先进的基准模型相比，该模型的竞争力。

*The rest of this paper is organized as follows: Section 2 gives a brief review of related works which helps highlight the novelties of this work. Section 3 gives definition of the problem and basic theory of GCN model. Section 4 describes in detail the proposed framework. In Section 5, experiments and results, as well as analysis are presented. Discussion of conclusion and future work are presented in Section 6.*

本文的其余部分组织如下：第2节简要回顾了相关工作，以突出本文的创新之处。第3节给出了问题的定义和GCN模型的基本理论。第4节详细描述了提出的框架。在第5节中，呈现了实验和结果，以及分析。讨论结论和未来工作在第6节呈现。



## 二、相关工作







## 三、预备知识

### 3.1 问题描述

在这项研究中，我们将交通网络定义为至少一个无向图 $G = (V, E, A)$ 的组合，其中 $V$ 表示节点的集合（$|V| = N$，在每个图中都是共享的，E 表示这些节点之间的边的集合（$|E| = M$），$A$ 表示图的邻接矩阵。在每个时间片段，每个节点以相同的采样频率检测到  $F$  个测量值（或特征，例如流量、天气和空气条件等[2,4,13]）。

$x^{if}_{t} \in R$表示节点 $i$ 在时刻 $t$ 的第 $f$ 个特征的值，$x^{i}_{t}=(x^{i1}_{t},\dots,x^{iF}_{t})\in R^{F}$表示节点 $i$ 在时刻 $t$ 所有特征的值，$X_{t}=(x^{1}_{t},\dots,x^{N}_{t})\in R^{N\times F}$表示在时刻 t 所有节点的所有特征值，$\mathcal{X}=(X_{1},\dots,X_{\tau})\in R^{\tau \times N\times F}$表示在 $\tau$ 个时间片段内所有节点的所有特征值，$X^{i}=\mathcal{X} [:,i,:]=(x_{t}^{i,f})_{t = 1..\tau,f = 1..F}$ 表示节点 $i$ 在 $\tau$ 个时间片段内所有特征值。如图1所示，交通预测问题表达为：

![image-20231203212615198](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202312032126282.png)

给定交通网络中所有节点的历史观测值 $X$，利用可用的图结构来预测未来 $\tau_{p}$ 个时间片上的交通序列 $\mathcal{Y} =(Y_{1},\cdots,Y_{\tau_{p}})\in\mathbb{R}^{\tau_{p}\times N}$，其中 $Y_{s}=(y_{s}^{1},\cdots,y_{s}^{N})=(x_{\tau + s}^{1,1},\cdots,x_{\tau + s}^{1,N})\in\mathbb{R}^{N}(1\leq s\leq\tau_{p})$表示在时刻 $\tau + s$ 时所有节点的交通特征。 

注记1：上述定义不仅适用于对第一个特征的预测，也适用于对第 $f$ 个（$1\leq f\leq F$）特征的预测。实际上，特征的顺序可以重新排列以适应此类问题。

### 3.2 图卷积网络

令 $D \in \mathbb{R}^{N \times N} = \text{diag}(d_1, \ldots, d_N) $ 是图 $ G $ 的度矩阵，其中 $d_i = \sum_j A_{ij} $ 表示节点 $i \in 1 : N$ 的度。图 $G$ 的对称归一化拉普拉斯矩阵被定义为 $L = D^{-\frac{1}{2}}(D - A)D^{-\frac{1}{2}} = I_N - D^{-\frac{1}{2}}AD^{-\frac{1}{2}} $。拉普拉斯矩阵的特征值分解为 $L = U\Lambda U^T = U \text{diag}(\lambda_1, \ldots, \lambda_N)U^T $。对于给定的输入 $ x \in \mathbb{R}^N $，傅里叶变换被定义为 $\hat{x} = U^Tx $，逆傅里叶变换为 $ x = U\hat{x} $。在图 $\ast_ \mathcal{G} $ 上的卷积操作在傅里叶域中定义，如下：

$$x \ast_ \mathcal{G}  \ y = U((U^T x) \odot  (U^T y))$$

其中 $ \odot  $ 是逐元素的Hadamard乘积。在傅立叶域中的参数化滤波器定义为

 $$g_{\theta} \ast_ \mathcal{G} x = g_{\theta} (U\Lambda U^T)x = U g_{\theta} (\Lambda)U^T x $$ 

其中 $ \theta $ 是一个参数向量，不一定属于 $ \mathbb{R}^n $；而非参数化滤波器，即其参数全部自由，可以定义为

$$g_{\theta}(\Lambda) = \text{diag}(\theta) $$ 

其中参数 $ \theta \in \mathbb{R}^n $ 是傅立叶系数的向量 [12, 16]。为了避免特征值分解，$ g_{\theta} $ 可以通过进行Chebyshev多项式$T_k(Λ)$的截断展开来近似，直到第K阶：

$$ g_{\theta}(\Lambda) \approx \sum_{k=0}^{K-1} \theta_k T_k(\tilde{\Lambda}) $$

其中 $ \tilde{\Lambda} = \frac{2}{\lambda_{\text{max}}}\Lambda - I_N $，$ \lambda_{\text{max}} $ 表示L的最大特征值，

$ \theta = (\theta_0, \ldots, \theta_{K-1})^T \in \mathbb{R}^K $，并且

$$g_{\theta} \ast_ \mathcal{G} x = g_{\theta}(L)x = \sum_{k=0}^{K-1} \theta_k T_k(\tilde{L})x$$

其中 $ \tilde{L} = \frac{2}{\lambda_{\text{max}}} L - I_N $。Chebyshev多项式的递归定义是 $ T_k(x) = 2xT_{k-1}(x) - T_{k-2}(x) $，其中 $ T_0(x) = 1 $，$ T_1(x) = x $。



## 四、提出的方法

在这一部分，我们将详细介绍所提出的基于多信息时空注意力的图卷积网络（MISTAGCN），如图2所示。

### 4.1 输入的准备

与[2]类似，我们构建了MISTAGCN的输入，其中包含沿时间轴的三个时间序列段，分别为Tr、Td和Tw，即最近输入、每日时段输入和每周时段输入。

设q为采样频率（每天q次）、t为当前时间、Tp为预测窗口的大小，则可以计算三个时间序列段为：

![image-20231203215838850](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202312032158950.png)

> 考虑了周期性，每天的周期性，每周的周期性

### 4.2 图结构

我们扩展了[4]的思想，为MISTAGCN添加了三个图结构，分别是距离图（DG），交互图（IG）和相关图（CG）。其中，DG的邻接矩阵（ADG）定义为：

![image-20231203220303748](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202312032203855.png)

其中，dij表示区域i和j之间的距离，即两个区域中心之间的距离；IG的邻接矩阵（AIG）定义为每对区域（i，j）之间交互记录的聚合，即：

![image-20231203220721409](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202312032207471.png)

其中，iij表示区域i和j之间的交互记录数；CG的邻接矩阵（ACG）定义为每对区域（i，j）之间的相关性，即：

![image-20231203221126084](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202312032211136.png)

其中，rij表示区域i和j之间的Pearson相关性，Tc是预定义的时间窗口常数，ρti和ρtj分别表示时间t时刻区域i和j的密度，即：

> Pearson相关性（Pearson correlation）是衡量两个变量之间线性关系强弱的统计量。它的取值范围在-1到1之间，其中1表示完全正相关，-1表示完全负相关，0表示无相关性。 Pearson相关性通过计算变量之间的协方差除以它们各自标准差的乘积来衡量它们的线性关系。 Pearson相关性通常用r表示。

![image-20231203221524327](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202312032215387.png)

此外，为了保证GCN的收敛性和稳定性，我们将三个邻接矩阵转换为对称半正定形式，使用 softmax 函数进行处理。

![image-20231203221751781](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202312032217825.png)

### 4.3 空间和时间注意力

空间-时间注意力机制（STAtt）包括两种类型的注意力，即空间注意力（SAtt），捕捉空间维度上节点之间的动态关系，和时间注意力（TAtt），捕捉时间维度上的交通状况。我们从一批输入Hs中提取和组织SAtt和TAtt（如图2所示，H ∈ R N×F×T是STBlock的输入），记为H ∈ R batch×N×F×T。具体而言，空间注意力S' ∈ R N×N 定义为

![image-20240701123940121](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202407011239283.png)

### 4.4 空间和时间模块

如图2所示，空间-时间块（STBlock）由TAtt、GCN、SAtt、时间卷积（TempConv）和残差连接等组成。首先，我们直接将时间注意力应用于STBlock的一批输入，记为H ∈ R batch×N×F×T，得到

$$\mathcal{H^{1}} = \mathcal{H} E'$$($E'$是时间注意力)

然后，我们重新排列ST特征的元素，应用图卷积和空间注意力，其中参数Θ = (Θ0, . . . , ΘK−1) ∈ R K×F×Fo，如公式(22)-(24)所示，

![image-20241214004640964](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202412140046110.png)

在这里，$H^2 \in \mathbb{R}^{\text{batch} \times N \times F_o \times T}$ 捕捉了图中每个节点的相邻信息，但这些数据的时间维度与我们的目标不匹配。我们在时间维度上添加了一个标准的卷积层，并通过 LeakyReLU 映射结果，通过合并相邻时间片的信息来更新每个节点的信号，具体如下：

![image-20241214004702340](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202412140047418.png)

其中，∗ 表示标准的卷积操作，Φ 表示在时间维度上的卷积核，LeakyReLU 是激活函数。

### 4.5 多信息融合

MISTAGCN包括两个阶段。在第一阶段（对应于模型的第一部分，如图2和Alg. 2所示），选择两种图结构（例如DG、IG）分别构建STBlock。同时，将三个输入（Xr，Xd，Xw）中的每一个重新排列并馈送到每个STBlock中，以分别计算包含潜在变量的张量。换句话说，每个（输入，图）对应于包含潜在变量的张量。总的来说，我们有三个输入和两种图结构，馈送到MISTAGCN的第一部分。对于这样的数据，有六种可能的组合，因此会生成六个输出。每个输出都包含目标的潜在信息的一部分。在第二阶段（对应于MISTAGCN的第二部分，如图2和Alg. 3所示），将六个张量的组合馈送到多层网络，以产生目标交通特征的近似值。网络的每一层都是基于另一种图结构（例如CG）的STBlock。此外，最后一层输出的特征数被设置为1。



## 五、实验和分析

本节详细描述了数据集、训练与评估过程，以及数值结果和性能分析等内容。

### 5.1 数据描述和预处理

海口在线打车数据：这个交通数据集是由滴滴出行GAIA计划收集和发布的，在中国海口市。该城市区域位于东经110°07′∼110°42′和北纬19°31′∼20°04′之间，被划分为20×10个相同大小的区域。该数据集的时间跨度为2017年8月1日至2017年10月31日，被划分为相等的时间片段，每个时间片段跨足1小时。数据集中有24个属性和14160162条记录，表示人们的出行类型（由滴滴提供）、服务类型、出发时间、出发地点、行程持续时间和到达时间等。图3显示了出行订单事件的累积分布的部分情况。基于这个划分，我们计算了每个时间片段t中每个区域i的流入和流出交通流量。同时，我们收集了海口市在相同时间段内的天气和空气条件，并将它们与交通流量结合起来。最终，我们得到了一个表示以交通流为中心的多维时间序列的大表。

![image-20240701192227373](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202407011922503.png)

纽约黄色出租车行程数据：黄色出租车行程记录包括捕捉上车和下车的日期/时间、上车和下车的位置、行程距离、详细票价、费率类型、支付类型和司机报告的乘客数量等字段。所附数据集中使用的数据是由纽约市出租车和豪华轿车委员会（TLC）授权的技术提供商在出租车和豪华轿车乘客增强计划（TPEP/LPEP）下收集并提供的。该数据集的区域来源于该城市的行政计划。时间跨度从2022年1月1日到2022年2月28日，每个时间片段跨足1小时。

### 5.2 基线与评估标准

我们将MISTAGCN与以下基线进行比较：矢量自回归（VAR，[24]）、全连接长短时记忆网络（FC-LSTM，[17]）、时空图卷积网络（STGCN，[20]）、动态时空图卷积神经网络（DGCNN，[7]）、基于注意力的时空图卷积网络（ASTGCN，[2]）、Graph WaveNet [18]、多空间时序信息融合网络（MSTIF-NET，[4]）、自适应图卷积循环网络（AGCRN，[21]）、图多头注意力网络（GMAN，[23]）、时空融合图神经网络（STFGNN，[22]）。同时，我们采用均方根误差（RMSE）和平均绝对误差（MAE）指标来评估不同方法的性能。



### 5.3 实验设置

![image-20240416135937375](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202404161359524.png)

我们基于MXNet框架（版本1.9.1）实现了MISTAGCN，使用了一台装有2块NVIDIA TITAN Xp GPU、16核CPU和64GB内存的工作站。我们实验的关键参数和设置总结如表2所示。

### 5.4 不同模型之间的比较和结果分析

![image-20240701192503391](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202407011925463.png)

我们对每个模型进行了10次训练（运行）的重复，并报告了平均结果。如表3和图4所示，MISTAGCN在黄色出租车数据集上的交通预测中取得了最低的RMSE和MAE，优于所有竞争对手；与此同时，在海口数据集上，MISTAGCN在RMSE上优于其他模型，并在MAE上表现几乎与ASTGCN相同。特别是在早期阶段，MISTAGCN明显优于所有竞争对手。

简而言之，传统的非GCN方法（如FC-LSTM、VAR等）忽视节点间的空间相关性，在交通预测中表现较差，不如设计良好的基于GCN的模型。虽然一些单一结构的基于GCN的模型（如ASTGCN、Graph WaveNet等）取得了不错的结果，但其收敛速度和准确性受到限制。设计良好的自学习GCN模型（如DGCNN、GMAN等）也取得了良好的预测精度，但这些模型的性能受到嵌入函数定义的影响较大。多结构模型（如MSTIF-Net、MISTAGCN等）利用更多的图结构，获得了更多有关交通数据空间关系的知识，从而取得更好的性能。另一方面，多维度时间信息的提取有助于我们获得更多有关交通数据时间维度的知识，从而提高了模型的预测精度（如ASTGCN、MISTAGCN等）。

![](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202407011925285.png)

### 5.5 模型与结果分析的比较

![image-20240701192757781](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202407011927859.png)

**不同图结构对性能的影响比较：**由于MISTAGCN涉及三种不同的图结构，我们对所有可能出现的这些图结构进行了比较实验。比较结果如表4和图5所示。总体而言，无论是哪种图结构的组合，该模型都表现出良好的性能。此外，与基于单一结构的情况相比，基于组合结构的情况利用了更多有用的信息，因此获得了更好的预测准确性。特别是，在同时结合所有三种结构的情况下，该模型达到了最佳性能。

![image-20240701192811779](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202407011928926.png)

***参数比较和性能分析**。我们还对MISTAGCN的不同参数对平均性能的影响进行了比较实验。如图6(a)所示，在其他参数不变的情况下，改变Tr将影响模型的性能。特别是，预测准确性并非Tr的单调递增函数。在海口数据集上，当Tr = 8时达到最佳性能；而在YellowTrip数据集上，当Tr = 6时达到最佳性能。如图6(b)、(c)所示，当分别改变Td或Tw时，得到的结果与改变Tr时类似。*

*如图6(d)所示，在其他参数不变的情况下，改变Td将影响模型的性能。特别是，预测准确性并非Tp的单调递减函数。在海口数据集上，当Tp = 4时达到最佳性能；而在YellowTrip数据集上，当Tp = 5时达到最佳性能。*

![image-20241005133700724](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202410051337001.png)



## 六、总结与未来工作

在这项研究中，我们提出了基于多信息时空注意力的图卷积网络（MISTAGCN），用于训练和预测交通流量。该模型通过首先对K阶图卷积网络应用空间注意力和时间注意力，然后通过类似的过程融合多种潜在信息，充分利用了时空中信息的不平衡性。与几种最先进的模型相比，MISTAGCN的实验结果表现出色。

未来，可以通过添加可调模块和参数，或增加算法的并行性以加速训练和测试过程，进一步改进MISTAGCN的结构。同时，还可以将一些可能的扩展应用于具有周期性规律的其他时间序列数据。



致谢：本研究得到了中国国家自然科学基金的支持（项目编号：61772386）。我们要感谢数据集来自滴滴出行的 Gaya 数据开放计划（数据来源：滴滴出行 GAIA 计划，https://gaia.didichuxing.com）以及纽约市出租车和豪华轿车委员会（TLC）（数据来源：TLC 行程记录数据，https://www1.nyc.gov/site/tlc/about/tlctrip-record-data.page）。





+ 图

> 划分为10 X 20，但是可能是由于地理位置等因素，总共的点数不是200，而是126

+ ADG

>计算数据源：data-samples
>
>使用球面三角学中的 Haversine 公式计算地球表面上两点之间的大圆距离（Great Circle Distance）或球面距离。这通常用于测量地球上两个地点之间的最短路径。
>$A_{ij}=\frac{1}{d_{ij}}$ ，然后对列进行softmax操作（每张图最后都进行了这个操作）
>
>这种列上的 softmax 操作有助于解决图卷积网络中的数值稳定性和归一化的问题，同时更好地捕捉图中节点之间的局部结构。这在图卷积操作中是常见的步骤，有助于提高模型对图结构的理解和学习。

+ ACG

> 计算数据源：data-samples，traffic-flows.db
>
> 使用 Pearson相关性计算ACG（直接调用pandas的方法），本应该由两个区域的总人口计算 $A_{ij}$ 可是并没有总人口数据，所以使用区域车辆流入量和区域测量流出量计算 $A_{ij}$
>
> Pearson相关性（Pearson correlation）是衡量两个变量之间线性关系强弱的统计量。它的取值范围在-1到1之间，其中1表示完全正相关，-1表示完全负相关，0表示无相关性。 Pearson相关性通过计算变量之间的协方差除以它们各自标准差的乘积来衡量它们的线性关系。 Pearson相关性通常用r表示。

+ AIG

> 计算数据源：data-samples，trip-2017.db（无该文件）
>
> 计算方式：$A_{ij}$ 用区域 $i$ 和区域 $j$ 之间的，交通数据来计算。$A_{ij}$ 对应的旅行数据中，出发地点为 $i$ 终点为 $j$ 的订单个数。

+ data 和 data-sample

> 生成脚本：scripts\haikou_gen_data_samples.py
>
> 计算数据源：traffic-flows.db，weather-air-condition-haikou-2017.xlsx
>
> 用途：可以修改预测步长 $T_p$ 

+ 参数含义

|       符号       |            值            |
| :--------------: | :----------------------: |
|        N         |        图中节点数        |
|        M         |         图中边数         |
|        F         |          特征数          |
|      $X_t$       |      时间t的流量数       |
|  $\mathcal{x} $  |    所有时间的流量记录    |
|  $\mathcal{y} $  |     未来交通特征序列     |
| $\mathcal{x_r} $ |         最近输入         |
| $\mathcal{x_d} $ |     以天为周期的输入     |
| $\mathcal{x_w} $ |     以周为周期的输入     |
|      $T_r$       |    最近输入的时间片数    |
|      $T_d$       | 日为周期输入中的时间片数 |
|      $T_w$       | 周为周期输入中的时间片数 |
|      $T_p$       |     要预测的时间片数     |
|        K         |   切比雪夫多项式的阶数   |
|     $A_{DG}$     |     距离图的邻接矩阵     |
|     $A_{IG}$     |     交互图的邻接矩阵     |
|     $A_{CG}$     |     关系图的邻接矩阵     |
|      $S^‘$       |      空间注意力矩阵      |
|      $E^‘$       |      时间注意力矩阵      |



