# Semantics-Aware Dynamic Graph Convolutional Network for Traffic Flow Forecasting

用于交通流量预测的语义感知动态图卷积网络

## 摘要

*Abstract—Traffic flow forecasting is a challenging task due to its spatio-temporal nature and the stochastic features underlying complex traffic situations. Currently, Graph Convolutional Network (GCN) methods are among the most successful and promising approaches. However, most GCNs methods rely on a static graph structure, which is generally unable to extract the dynamic spatiotemporal relationships of traffic data and to interpret trip patterns or motivation behind traffic flows. In this paper, we propose a novel Semantics-aware Dynamic Graph Convolutional Network (SDGCN) for traffic flow forecasting. A sparse, state-sharing, hidden Markov model is applied to capture the patterns of traffic flows from sparse trajectory data; this way, latent states, as well as transition matrices that govern the observed trajectory, can be learned. Consequently, we can build dynamic Laplacian matrices adaptively by jointly considering the trip pattern and motivation of traffic flows. Moreover, high-order Laplacian matrices can be obtained by a newly designed forward algorithm of low time complexity. GCN is then employed to exploit spatial features, and Gated Recurrent Unit (GRU) is applied to exploit temporal features. We conduct extensive experiments on three real-world traffic datasets.Experimental results demonstrate that the prediction accuracy of SDGCN outperforms existing traffic flow forecasting methods. In addition, it provides better explanations of the generative Laplace matrices, making it suitable for traffic flow forecasting in large cities and providing insight into the causes of various phenomena such as traffic congestion.*

摘要——交通流量预测是一项具有挑战性的任务，因为它具有时空特性以及复杂交通情况背后的随机特征。目前，图卷积网络（GCN）方法是其中最成功且最有前景的方法之一。然而，大多数GCN方法依赖于静态图结构，这通常无法提取交通数据的动态时空关系，也无法解释出行模式或交通流背后的动机。在本文中，我们提出了一种新颖的语义感知动态图卷积网络（SDGCN）用于交通流量预测。==我们应用了一个稀疏的、状态共享的隐马尔可夫模型来从稀疏轨迹数据中捕获交通流模式==；==通过这种方式，可以学习潜在的状态以及控制观测轨迹的转移矩阵。因此，我们可以结合考虑交通流的出行模式和动机，自适应地构建动态拉普拉斯矩阵==。此外，通过新设计的低时间复杂度前向算法，可以获得高阶拉普拉斯矩阵。然后，==利用GCN来挖掘空间特征，并应用门控循环单元（GRU）来挖掘时间特征==。我们在三个真实世界的交通数据集上进行了广泛的实验。

实验结果表明，SDGCN的预测精度优于现有的交通流量预测方法。此外，它提供了对生成拉普拉斯矩阵的更好解释，使其适用于大城市的交通流量预测，并为各种现象（如交通拥堵）的原因提供了见解。



## 一、介绍

准确的交通流量预测对于智能交通管理和智能城市应用至关重要，如指导道路建设、感知交通拥堵和推荐旅行路线。探索合适的空间和时间特性是实现高性能的关键。

基于这一思想，过去几十年中文献中提出了许多方法。早期的交通流量预测方法基于传统机器学习和经典统计学，包括时间序列模型[1]、[2]、[3]，卡尔曼滤波模型[4]，马尔可夫链模型[5]，非参数方法，仿真模型[6]、[7]，以及局部回归模型[8]、[9]。

这些方法主要关注于探索时间序列数据中的时间关系，但忽略了由道路网络拓扑产生的空间相关性。

最近，随着深度学习的进步[10]，[11]，卷积神经网络（CNNs）被用于捕获基于网格数据的空间特征[12]，而循环神经网络（RNNs）[13]，[14]则被用来探索时间相关性。然而，这些方法很难同时模拟交通数据的时空特性和动态关系。尽管CNN和RNN的混合模型可以捕获一些时空特征[15]，但交通道路拓扑的复杂性超出了CNN方法捕获这些非欧几里得交通数据潜在空间特征的能力。通过将交通预测制定为图建模问题，图卷积网络（GCNs）在交通流量预测中取得了最先进的性能[16]。

然而，这些工作的性能受到使用固定经验图和未充分考虑交通数据动态特征的限制。一些学者考虑使用动态图来构建GCNs[17]，[18]，[19]，[20]，[21]，[22]，[23]，[24]，[25]，但其中大多数使用静态图（例如，道路网络拓扑）通过深度学习方法来构建动态图。此外，由于深度学习方法通常被视为一个黑盒，由它们生成的动态图往往缺乏强有力的解释性。另外，为了聚合多跳邻居节点信息，应用了高阶拉普拉斯矩阵。然而，计算这样的高阶拉普拉斯矩阵是耗时的。

由于全球定位系统（GPS）和交通传感器已被广泛用于记录道路上车辆的大规模轨迹[26]，==通过移动性建模来理解交通数据的动态相关性成为可能，从而可以进行更准确的动态图嵌入==。最近，已有几项研究致力于这一领域[27]，[28]，[29]。然而，这些移动性模型的主要缺点是它们主要关注轨迹的局部相关性，==但忽略了丰富的语义关系结构，即行程模式或移动背后的动机==。例如，如图1所示，==在工作日的交通高峰时段，即使起点位于远离同一目的地的地方，但具有相同意图（例如，两个相距较远的住宅区的人们前往办公室上班）的起点应被视为在语义上高度相关。同时，在周末，由于访问娱乐场所的相似目的，起点可能在语义上高度相关==

如何发现动态的隐藏语义关系图而非静态图（例如道路拓扑）是动态图构建中的关键挑战。

为了解决上述问题，受关系提取领域知识的启发，我们提出应用基于稀疏隐马尔可夫模型的方法来学习嵌入在历史轨迹中的内在语义。由于轨迹的上下文语义可以为这些起点和终点（OD）之间存在的关系提供有用的见解，它们可以揭示出道路网络中的隐藏拓扑。

例如，如果人们通常在下班后到达A区和B区，这样的轨迹可以表明A区和B区在语义上是相连的（即，具有相似的动机，因为它们可能都在那里购物），即使它们相距甚远。

*We introduce a sparse state-sharing hidden Markov model to capture the patterns of traffic flows from the sparse trajectory data, and to reveal the latent states and transition matrices among them. Consequently, we can build dynamic Laplacian matrices adaptively by jointly considering time, space, and the trip pattern (or motivation) of users. The primary contributions of this paper are as follows: *
*A novel Semantics-aware Dynamic Graph Convolutional Network (SDGCN) is proposed for traffic flow forecasting. SDGCN transforms the traditional static graphs into adaptive dynamic semantic graphs, and represents spatiotemporal road connections through dynamic semanticsaware relationships.*
*To address the low accuracy problem caused by the scarcity and inconsistency of trajectory data, a State-share Sparse Hidden Markov Model is introduced. It mines personalized features, through which k-hop dynamic semantic Laplacian matrix can be obtained, and consequently simplify the computational complexity of calculating high-order Laplacian matrix.*
*We conduct extensive experiments on three real-world traffic datasets. Experimental results demonstrate the superiority of SDGCN in traffic flow forecasting, compared to existing baselines. Furthermore, we showcase examples of improved interpretability of the generated graphs*

我们引入了一个稀疏状态共享的隐马尔可夫模型，以从稀疏轨迹数据中捕获交通流模式，并揭示它们之间的潜在状态和转移矩阵。因此，我们可以通过综合考虑时间、空间和用户的出行模式（或动机）来自适应地构建动态拉普拉斯矩阵。本文的主要贡献如下：

+ 提出了一种新颖的语义感知动态图卷积网络（SDGCN）用于交通流预测。==SDGCN将传统静态图转换为自适应动态语义图，并通过动态语义感知关系表示时空道路连接。==
+ ==为了解决轨迹数据稀缺和不一致导致的低准确性问题，我们引入了状态共享的稀疏隐马尔可夫模型。它通过挖掘个性化特征，可以获得k-hop动态语义拉普拉斯矩阵，从而简化了计算高阶拉普拉斯矩阵的计算复杂度。==
+ 我们在三个真实世界的交通数据集上进行了广泛的实验。实验结果表明，与现有基线相比，SDGCN在交通流预测方面具有优越性。此外，我们还展示了生成的图在可解释性方面的改进示例。

## 三、方法论

交通信息预测的目的是基于当前或过去观察到的交通状态来预测未来几个时刻的交通信息状态。交通信息包括多种传感器数据，如交通速度、交通密度和行驶时间。==在这项研究中，我们专注于交通流量预测中的交通速度预测==。在智能城市中，可以通过多个相关传感器来测量交通速度。

### A.问题定义

定义1. 交通网络G：交通网络的结构可以自然地用图G = (V,E,A)来表示，其中V代表城市中的一组道路，V = {v1, v2, ..., vN}，N是道路的总数。在这项研究中，我们将道路视为图的节点，v或vn代表一个普通的道路节点。E是节点之间的边的集合。我们采用邻接矩阵A来表示道路节点之间的连接。特别是，对于加权图，A ∈ RN×N，而对于静态图（例如基于道路网络拓扑），A的元素是0或1。

定义2. 轨迹流观测O：随时间T变化的车辆轨迹的概率分布用矩阵O = [O1, O2, ..., OT] ∈ RN×T 表示，其中Ot = [ot(v1); ot(v2); ...; ot(vN)] ∈ RN×1 表示在时间步t的交通流轨迹，ot(vn) ∈ V 表示在时间t，道路节点起源和目的地之间的交通流变化。例如，ot(vn) = vm 表示在时间t从vn到道路vm的交通流。

定义3. 动态语义邻接矩阵At：这是对定义1的扩展，以描述动态图。与传统方法不同，传统方法中边由静态图的拓扑结构确定，而动态语义边则用于表示道路之间的关系。每个在时间t的动态边被表达为在时间t从vn到vm的交通流的条件概率，可以公式化为：At = amn = p(ot(vn) = vm) = p(ot = vm|ot−1 = vn)。

(1) 其中，At ∈ RN×N 是语义动态邻接矩阵，amn 是第m行第n列的元素。相应地，G = (V,E,A) 将变为Gt = (V,Et, At)。

定义4. 特征矩阵X：交通数据包含各种数据，这些数据可以表示为矩阵X ∈ RN×P，其中P表示节点属性的数量。在这项研究中，我们考虑交通流速度作为我们关注的特征，并在特征矩阵X = [Xt−p, ..., Xt−2, Xt−1]中捕获过去P个时间步长的数据，其中Xt ∈ RN×1用于表示时间t时所有节点V的速度。

基于上述定义，动态语义邻接矩阵可以按以下方式计算：[O1, O2, ...Ot−1] g(.) → ˆOt (1) → ˆAt。(2)
在第一步中，我们学习一个函数g(.)，将过去的t-1个时间序列观测值[O1, O2, ...Ot−1]映射到时间t的未来观测值ˆOt。根据(1)，我们可以得到预测的动态语义邻接矩阵ˆAt。

在第二步中，交通速度预测可以表述为：Xt−p, ..., Xt−1; ˆAt f(.) → [Xt, ..., Xt+T−1]。(3) 我们的目标是学习一个函数f(.)，该函数利用预测的动态语义邻接矩阵ˆAt，将过去的p个时间步长的图信号Xt−p, ..., Xt−1映射到未来T个时间步长的图信号。



### B.方法概述

如图2所示，SDGCN由三个组件组成：动态语义图生成器、图卷积网络（GCN）和时间门递归预测器。首先，将先前观察到的t-1个交通轨迹[O1, O2, ..., Ot−1]输入到动态状态共享潜在层中，以获得一个共同的潜在状态共享空间C。每个道路节点v的k-hop（k-hop代表一般的多跳，k={0, 1, 2, ..., K}，其中K是最大跳数）个人特征由一个随时间t变化的个人潜在变量Zv t控制，并且与状态共享潜在空间C稀疏连接。对于每个道路节点v，我们可以将k-hop概率聚合到发射概率中，以捕获v的内在语义特征关系。然后，在通过这种方式处理完所有道路节点V之后，生成动态语义图。其次，输出的动态语义图将以各种k阶拉普拉斯矩阵的组合形式传递给GCN。第三，节点信号[Xt−p, ..., Xt−2, Xt−1]，以及生成的图，被输入到GCN中以捕获空间特征。最后，GCN的最新P个输出特征被输入到时间门递归预测器中，以捕获时间特征并获得预测值ˆXt。



### C.GCN

图卷积网络（GCN）模型在傅里叶域中构建了一个滤波器。拉普拉斯矩阵可以表示为L = D−1/2(A + I)D1/2，其中I是单位矩阵，表示添加自连接，D是度数矩阵。对于GCN，考虑到K-hop聚合，GCN模型可以通过以下公式表达[51]：

X(l+1) = f(A, X(l)) = σ(∑_{k=1}^{K} L^k X(l) w(l))，

(4) 其中X(l) ∈ RN×P是第l层的输入，而X(l+1) ∈ RN×T是第l层在未来T个时间步的输出，k表示聚合的跳数，且k ∈ {1, ..., K}。此外，w ∈ RP×T是一个可学习的参数，σ表示非线性激活函数。为了捕获动态语义关系，定义3被用作新的邻接矩阵。

两层GCN模型[52]被用来获取动态的语义交通关系。在T-GCN[49]中，为了简化，K被设置为1，这意味着只考虑了一阶邻域节点而忽略了高阶邻域节点，因为计算K阶拉普拉斯矩阵LK是耗时的。如果直接计算，时间复杂度将是O(N3(K-1))。此外，随着拉普拉斯矩阵的阶数增高，它容易出现过度平滑的问题[53]。为了利用高阶邻居节点的信息，应该考虑k阶拉普拉斯矩阵。因此，如何计算语义动态的k阶拉普拉斯矩阵Lk和∑_{k=1}^{K} Lk成为了一个关键问题。







### E. 时间门递归预测器

为了捕获时间特征，本研究采用了GRU（门控循环单元）作为时间门递归预测器。GRU是一种类似于LSTM的RNN网络，但更为简化，因为它只有两个门。GRU将LSTM中的输入门和遗忘门合并为一个单一的门，称为更新门，它控制之前记忆的信息在当前时刻保留的多少。另一个门是重置门，它控制在当前时刻会忘记多少过去的信息。其工作机制可以通过以下方程来解释[38]：

(g_t = \sigma (W_z \cdot [h_{t-1}, x_t] + b_u)) (11)

(r_t = \sigma (W_r \cdot [h_{t-1}, x_t] + b_r)) (12)

(\tilde{h}*t = \tanh (W \cdot [r_t \* h*{t-1}, x_t] + b_c)) (13)

(h_t = (1 - g_t) * h_{t-1} + g_t * \tilde{h}_t) (14)

其中，(h_t) 表示时间t的隐藏状态，(\tilde{h}_t) 是时间t的候选状态，(g_t) 是更新门，(r_t) 是重置门。