# Dynamic and Multi-faceted Spatio-temporal Deep Learning for Traffic Speed Forecasting

## 摘要

*Dynamic Graph Neural Networks (DGNNs) have become one of the most promising methods for traffic speed forecasting. However, when adapting DGNNs for traffic speed forecasting, existing approaches are usually built on a static adjacency matrix (no matter predefined or self-learned) to learn spatial relationships among different road segments, even if the impact of two road segments can be changeable dynamically during a day. Moreover, the future traffic speed cannot only be related with the current traffic speed, but also be affected by other factors such as traffic volumes. To this end, in this paper, we aim to explore these dynamic and multi-faceted spatio-temporal characteristics inherent in traffic data for further unleashing the power of DGNNs for better traffic speed forecasting.

Specifically, we design a dynamic graph construction method to learn the time-specific spatial dependencies of road segments. Then, a dynamic graph convolution module is proposed to aggregate hidden states of neighbor nodes to focal nodes by message passing on the dynamic adjacency matrices. Moreover, a multi-faceted fusion module is provided to incorporate the auxiliary hidden states learned from traffic volumes with the primary hidden states learned from traffic speeds. Finally, experimental results on real-world data demonstrate that our method can not only achieve the state-ofthe-art prediction performances, but also obtain the explicit and interpretable dynamic spatial relationships of road segments.*

动态图神经网络（DGNNs）已经成为交通速度预测中最有前景的方法之一。然而，在调整DGNNs用于交通速度预测时，现有的方法通常基于静态邻接矩阵（不管是预定义的还是自学习的）来学习不同道路段之间的空间关系，即使两个道路段的影响在一天之内可能是动态变化的。此外，未来的交通速度不仅与当前的交通速度相关，还可能受到其他因素的影响，如交通流量。因此，在本文中，我们旨在探索交通数据中固有的这些动态和多方位时空特性，以更好地发挥DGNNs在交通速度预测方面的潜力。

具体而言，我们设计了一种动态图构建方法，以学习道路段的时间特定的空间依赖关系。然后，提出了一个动态图卷积模块，通过在动态邻接矩阵上进行消息传递，将相邻节点的隐藏状态聚合到焦点节点。此外，提供了一个多方位融合模块，将从交通流量中学到的辅助隐藏状态与从交通速度中学到的主要隐藏状态集成起来。最后，在真实世界的数据上进行的实验证明，我们的方法不仅可以达到最先进的预测性能，还可以获得明确且可解释的道路段动态空间关系。

## 一、介绍

*In recent years, intelligent transportation systems have been largely promoted by deep learning techniques. The successful applications include but not limited to computer vision methods for traffic data collection [9] and regulations enforcement [2], reinforcement learning for automatic pilot [16, 18] and traffic signal control [6], big data analysis for human mobility modelling [10, 11], etc. Among these techniques, deep spatio-temporal neural networks are the most promising methods due to its ability to capture spatial dependencies and temporal dynamics simultaneously, which have been widely used in traffic speed forecasting, transportation demand prediction and travel time estimation. In the early stage, with the distributions of traffic status or demands divided into grids on a map, CNNs were used to learn the spatial interactions of different grids, and RNNs were used to learn the evolutionary traffic dynamics [28, 30, 33]. Not long after that, researchers found that it is more reasonable to model the spatial interactions of traffic systems as graph due to the fact that most of the traffic indicators are given along road network (such as traffic speeds and volumes of road segments) or associated with fixed stations (pick-up demands of car-hailing stations or check-in amounts of subway stations). Most of the recent research has focused on geometric spatio-temporal learning [1, 14, 19, 26, 27, 31, 32, 35].*

近年来，深度学习技术在智能交通系统中得到了广泛推广。其成功应用包括但不限于用于交通数据收集的计算机视觉方法[9]和规章执行[2]，用于自动驾驶的强化学习[16, 18]和交通信号控制[6]，以及用于人类移动模型的大数据分析[10, 11]等。

在这些技术中，深度时空神经网络是最有前景的方法之一，因为它具有同时捕捉空间依赖性和时间动态的能力，已广泛应用于交通速度预测、交通需求预测和行程时间估计等任务。在早期阶段，通过将交通状态或需求分布到地图上的网格中，使用卷积神经网络（CNN）学习不同网格之间的空间交互作用，并使用循环神经网络（RNN）学习交通动态的演变[28, 30, 33]。不久之后，研究人员发现，将交通系统的空间交互作用建模为图更为合理，因为大多数交通指标是沿着道路网络给出的（如道路段的交通速度和流量）或与固定站点相关联的（如打车站的乘车需求或地铁站的签到数量）。近期的研究大多集中在几何时空学习上[1, 14, 19, 26, 27, 31, 32, 35]。

*Traffic speed forecasting is a well-defined representative geometric spatio-temporal learning problem, which encodes each road segment as a node in a graph, and the edges between nodes correspond to spatial influences of road segments. A number of methods have been proposed for traffic speed forecasting, for example, Yu et al proposed STGCN to integrate graph convolution and gated temporal convolution [32], Li et al designed DCRNN to use bidirectional graph random walk to model spatial dependency and recurrent neural network to capture the temporal dynamics [19], Wu et al proposed MTGNN to exploit the inherent dependency relationships among multiple segments [26].*

交通速度预测是一个明确定义的典型的几何时空学习问题，它将每个道路段编码为图中的一个节点，节点之间的边对应于道路段之间的空间影响。已经提出了许多用于交通速度预测的方法，例如，Yu等人提出了STGCN，将图卷积和门控时间卷积结合起来[32]，Li等人设计了DCRNN，使用双向图随机游走来建模空间依赖性，并使用循环神经网络捕捉时间动态[19]，Wu等人提出了MTGNN，利用多个段之间的固有依赖关系[26]。

*Even though, two key issues have rarely been discussed. The first problem is how to model the spatial dependencies of road segments dynamically. In most of the existing research, the impacts of two road segments are viewed as static, and determined by a predefined Research Track Paper KDD ’21, August 14–18, 2021, Virtual Event, Singapore or self-learned adjacency matrix. However, the interactions of two road segments at different time are supposed to be different. As traffic speed of two segments (top right) in Figure 1 shows, the speed pattern of them is similar in morning peak while completely different in evening peak. The second problem is how to incorporate the traffic speed forecasting problem with rich multi-faceted auxiliary data. Urban traffic networks are complex, dynamic and giant systems. Different types of traffic data record the observations of traffic systems from different perspectives. Therefore, it is promising to improve the performance of traffic speed forecasting by mining the latent patterns and dynamics underlying traffic systems deeply and sufficiently from multi-faceted data. As the combination of traffic speed and traffic volume (bottom right) in Figure 1 shows, traffic speed have a stiff drop after traffic volume increase rapidly in morning and evening peak, which reveals the potential of traffic volume to assist traffic speed forecasting.*

尽管如此，两个关键问题很少被讨论。第一个问题是如何动态地建模道路段之间的空间依赖关系。在大多数现有的研究中，两个道路段之间的影响被视为静态的，并由预定义的或自学得的邻接矩阵确定。然而，两个道路段在不同时间的相互作用应该是不同的。如图1中所示，两个段的交通速度（右上角）显示，它们的速度模式在早高峰时期相似，而在晚高峰时期完全不同。第二个问题是如何将交通速度预测问题与丰富的多方位辅助数据结合起来。城市交通网络是复杂、动态且庞大的系统。不同类型的交通数据记录了从不同角度观察交通系统的观测结果。因此，通过从多方位数据中深入充分挖掘交通系统潜在的模式和动态，有望提高交通速度预测的性能。正如图1中交通速度和交通量的组合（右下角）所示，在早晚高峰期间交通量迅速增加后，交通速度急剧下降，这揭示了交通量在辅助交通速度预测方面的潜力。

*This paper aims to address the above two issues. However, it is a nontrivial endeavor to design such a dynamic and multi-faceted traffic speed forecasting method due to the following challenges: First, traffic speed of multiple road segments follows a dynamic and implicit spatial pattern. Hand-designed spatial graph is easy to interpret but could not really reflect the interaction relationship.*

*Second, generating a graph for each time slot through existing selflearned methods would introduce plenty of parameters making it hard to coverage. Third, it is difficult to model the effects of auxiliary information on traffic speed forecasting, as the impact may also have spatio-temporal characteristics*

这篇论文旨在解决上述两个问题。然而，设计这样一种动态且多层面的交通速度预测方法是一项非常艰巨的任务，原因如下：首先，多个道路段的交通速度遵循动态且隐含的空间模式。手工设计的空间图易于解释，但无法真正反映交互关系。其次，通过现有的自学习方法为每个时间槽生成图会引入大量参数，使其难以覆盖。第三，模拟辅助信息对交通速度预测的影响也很难，因为其影响也可能具有时空特性。

*In this paper, we propose a Dynamic and Multi-faceted SpatioTemporal Graph Convolution Network (DMSTGCN) for traffic speed forecasting with multi-faceted auxiliary data. First, aiming to model dynamic spatial relationship between segments, a dynamic graph constructor inspired by tensor decomposition is designed to take periodicity and dynamic characteristics of traffic into consideration. Moreover, dynamic graph convolution is proposed to capture varying spatial dependencies of road segments. Then, to handle unique spatio-temporal dependencies in multi-faceted data, several parallel structures are employed, each of which consists of several dynamic graph convolutions and temporal convolution layers. For extra effects of auxiliary data on traffic speed, we design a framework powered by the multi-faceted fusion module to integrate auxiliary hidden states with traffic speed hidden states in a graph perspective. Finally, skip connections are added after each temporal convolution layer to several fully connected layers for final prediction. It is worth noting that the proposed method in this paper is a general framework to handle other problem with multi-faceted spatio-temporal graph sequences. In summary, our contributions are three folds: 

+ A dynamic spatial dependencies learning method is proposed. Different from the existing methods, which are founded on a predefined or self-learned static adjacency matrix, our method propagates hidden states of nodes according to dynamic spatial relationships. We design a dynamic graph constructor and the dynamic graph convolution method to accomplish this. 

+ A multi-faceted fusion module is provided to incorporate the auxiliary hidden states with primary hidden states spatially and temporally. It is a generic framework to handle multifaceted spatio-temporal data. *

* We not only validate the effectiveness of the proposed methods by experiments on real-world datasets, but also uncover the explicit and discovered dynamic patterns of relationships among multi-faceted data.*

这篇论文提出了一种用于交通速度预测的动态多层面时空图卷积网络（DMSTGCN），结合了多层面的辅助数据。首先，为了模拟片段之间的动态空间关系，设计了一种受张量分解启发的动态图构建器，考虑了交通的周期性和动态特性。此外，提出了动态图卷积，以捕捉道路段之间不断变化的空间依赖关系。然后，为了处理多层面数据中独特的时空依赖关系，采用了几个并行结构，每个结构包括几个动态图卷积和时态卷积层。为了增强辅助数据对交通速度的额外影响，设计了一个由多层面融合模块支持的框架，将辅助隐藏状态与交通速度隐藏状态在图形视角上集成。最后，在每个时态卷积层后添加了跳跃连接到几个全连接层，用于最终预测。值得注意的是，本文提出的方法是一个通用框架，可处理其他多层面时空图序列的问题。

总的来说，我们的贡献有三个方面：
1. 提出了一种动态空间依赖关系学习方法。与现有方法不同，其基于预定义或自学习的静态邻接矩阵，我们的方法根据动态空间关系传播节点的隐藏状态。我们设计了动态图构建器和动态图卷积方法来实现这一点。
2. 提供了一个多层面融合模块，以在时空上空间地将辅助隐藏状态与主要隐藏状态整合。这是一个处理多层面时空数据的通用框架。
3. 我们不仅通过对真实数据集的实验证明了所提方法的有效性，还揭示了多层面数据之间关系的显式和发现的动态模式。



## 三、方法

![image-20231204162742462](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202312041627607.png)

> DMSTGCN的架构。该框架主要由两个并行部分组成：一个用于主要特征，另一个用于辅助特征。动态图构造器为图卷积层生成动态图。主要部分包括 𝐿 个主要块，每个块由一个时间卷积层和一个动态图卷积层组成。辅助部分类似于主要部分。
>
> 多方面融合模块首先将辅助块的输出传播并与主要块的输出聚合，作为下一层的输入。为每个块添加了残差连接，主要部分每个时间卷积后的隐藏状态连接到输出层，用于最终预测。

### 3.1 动态图构造器

*As spatial dependencies between segments are implicit and changing constantly due to dynamic characteristic of traffic, it is necessary to properly design a graph learning module to address this. In previous work, dependencies between nodes were either defined by human knowledge like distance and functional similarity or generated as a static graph which omits the dynamic characteristic of traffic spatial dependencies. In this paper, to capture varying relationships between nodes, we propose the dynamic graph constructor which produces dynamic learnable graphs used in the dynamic graph convolution and the multi-faceted fusion module.*

由于交通的动态特性，路段之间的空间依赖关系是隐含的并且不断变化的，因此有必要设计一个合适的图学习模块来解决这个问题。在先前的工作中，节点之间的依赖关系要么是由人类知识（如距离和功能相似性）定义的，要么是生成的静态图，这忽略了交通空间依赖关系的动态特性。在本文中，为了捕捉节点之间变化的关系，我们提出了动态图构建器，它产生用于动态图卷积和多层面融合模块的动态可学习图。

*However, it is nontrivial to adaptively model relationships between each pair of nodes at each time. The simplest way is to directly assign a learnable parameter tensor to represent dynamic relationships, but the complexity of this method is 𝑂(𝑇 𝑁 𝑁), where 𝑇 is the number of total time slots and 𝑁 is the number of nodes, making it hard to compute and coverage. Considering the periodicity of traffic status, we assume that traffic at the same time in a day could share a same graph. Thus, our goal is transferred to an adjacency tensor A ∈ R 𝑁𝑡×𝑁 ×𝑁 where 𝑁𝑡 is the number of time slots in a day. And the graph at time slot 𝑡 is A𝜙 (𝑡) where 𝜙 (𝑡) is a function to get time in a day. But the complexity of this solution is still 𝑂(𝑁𝑡𝑁 𝑁) which grows quadratically as the graph becomes larger. In reality, some structures of dynamic graph could be shared across time and space. For example, two adjacent segments could be correlated across the day, and people will leave from different resident areas to different work areas in the morning.*

*So we propose a method inspired by Tucker decomposition [24] to form the adjacency tensor as shown in Figure 3. In traffic speed forecasting, instead of decomposing an known tensor, we aim to compose an unknown tensor with learnable parameters for spatial dependencies. This procedure performs reversely as original Tucker decomposition and updates dynamic graph by backpropagation during training.*

然而，在每一对节点之间自适应地建模关系并不是一件简单的事情。最简单的方法是直接分配一个可学习的参数张量来表示动态关系，但是这种方法的复杂性是 𝑂(𝑇 𝑁 𝑁)，其中 𝑇 是总时隙的数量，𝑁 是节点的数量，使得计算和覆盖变得困难。考虑到交通状况的周期性，我们假设一天中相同时间的交通可以共享同一个图。因此，我们的目标转变为一个邻接张量 𝐴 ∈ R 𝑁𝑡×𝑁 ×𝑁，其中 𝑁𝑡 是一天中的时间槽数。在这里，时隙 𝑡 的图是 𝐴𝜙 (𝑡)，其中 𝜙 (𝑡) 是获取一天中时间的函数。但是这种解决方案的复杂性仍然是 𝑂(𝑁𝑡𝑁 𝑁)，随着图的变大，它呈二次增长。实际上，动态图的一些结构可能会在时间和空间上共享。例如，两个相邻的路段可能在一天中保持相关性，人们在早上会从不同的居住区到不同的工作区。

因此，我们提出了一种受 Tucker 分解 [24] 启发的方法，用于形成如图3所示的邻接张量。在交通速度预测中，我们的目标不是分解已知张量，而是用可学习的参数为空间依赖性组成未知张量。这个过程与原始的 Tucker 分解相反，并在训练过程中通过反向传播更新动态图。

*Specifically, we assign three learnable matrices and one learnable core tensor including embedding of time slots E 𝑡 ∈ R 𝑁𝑡×𝑑 , embedding of source nodes E 𝑠 ∈ R 𝑁𝑠×𝑑 , embedding of target nodes E 𝑒 ∈ R 𝑁𝑒×𝑑 and a core tensor E 𝑘 ∈ R 𝑑×𝑑×𝑑 , where 𝑁𝑡 , 𝑁𝑠, 𝑁𝑒 , 𝑑 represent the number of time slots, number of original nodes, number of target nodes, and embedding dimension respectively. And the adjacency tensor is calculated as: A ′ 𝑡,𝑖,𝑗 = 𝑑 ≂ 𝑜=1 𝑑 ≂ 𝑞=1 𝑑 ≂ 𝑟=1 E 𝑘 𝑜,𝑞,𝑟E 𝑡 𝑡,𝑜E 𝑒 𝑖,𝑞E 𝑠 𝑗,𝑟, (1) A ′′ 𝑡,𝑖,𝑗 = max(0, A ′ 𝑡,𝑖,𝑗), (2) A𝑡,𝑖,𝑗 = 𝑒 A ′′ 𝑡,𝑖, 𝑗 ∺𝑁𝑠 𝑛=1 𝑒 A ′′ 𝑡,𝑖,𝑛 . (3) In this work, three tensors A𝑝 ∈ R 𝑁𝑡×𝑁𝑝×𝑁𝑝 , A𝑎 ∈ R 𝑁𝑡×𝑁𝑎×𝑁𝑎 and A𝑎𝑝 ∈ R 𝑁𝑡×𝑁𝑝×𝑁𝑎 are generated to represent dynamic graphs: graph among primary nodes G 𝑝 , graph among auxiliary nodes G 𝑎 , graph between primary nodes and auxiliary nodes G 𝑎𝑝 respectively.*

具体而言，我们分配了三个可学习矩阵和一个可学习的核心张量，包括时间槽嵌入 E 𝑡 ∈ R 𝑁𝑡×𝑑、源节点嵌入 E 𝑠 ∈ R 𝑁𝑠×𝑑、目标节点嵌入 E 𝑒 ∈ R 𝑁𝑒×𝑑 以及一个核心张量 E 𝑘 ∈ R 𝑑×𝑑×𝑑，其中 𝑁𝑡、𝑁𝑠、𝑁𝑒、𝑑 分别表示时间槽数、原始节点数量、目标节点数量和嵌入维度。邻接张量的计算如下：
$A ′_𝑡,𝑖,𝑗 = \sum_{o=1}^{d} \sum_{q=1}^{d} \sum_{r=1}^{d} E 𝑘_{o,q,r}E 𝑡_{t,o}E 𝑒_{i,q}E 𝑠_{j,r}$
$A ′′_𝑡,𝑖,𝑗 = \max(0, A ′_𝑡,𝑖,𝑗)$
$A_𝑡,𝑖,𝑗 = \frac{e^{A ′′_𝑡,𝑖,𝑗}}{\sum_{n=1}^{N_s} e^{A ′′_𝑡,𝑖,𝑛}}$

在这项工作中，生成三个张量 $A_p \in R^{N_t \times N_p \times N_p}$、$A_a \in R^{N_t \times N_a \times N_a}$ 和 $A_{ap} \in R^{N_t \times N_p \times N_a}$ 用于表示动态图：主要节点之间的图 $G_p$、辅助节点之间的图 $G_a$、主要节点和辅助节点之间的图 $G_{ap}$。



### 3.2 动态图卷积

*Spatial dependencies of the traffic speeds of different road segments could be used to improve the traffic speed prediction performance.Though the graph convolution could aggregate neighbor hidden states to handle the spatial dependencies between segments, majority of existing methods utilized it on a static graph. In this paper, to Research Track Paper KDD ’21, August 14–18, 2021, Virtual Event, Singapore model latent and time varying spatial dependencies between nodes, we propose a dynamic version of graph convolution to conduct it on different graphs at different time.*

*Given input X𝑡−𝑃+1:𝑡 , A𝜙 (𝑡) reflects spatial relationship between nodes at time 𝑡. As shown in Figure 4, input hidden states of each node are isolated at first, and after fetching A𝜙 (𝑡) from adjacency tensor, the hidden states can be organized as graph signal according to their dynamic spatial relationships. Moreover, the method to aggregate information is inspired by DCRNN [19], which views the traffic flow as the diffusion procedure on graph. In math form, this operation can be realized by matrix multiplication of the adjacency matrix, hidden states of nodes and learnable parameters. At each step, the hidden states of focal node are updated by aggregating neighbors’ hidden states through weighted links and the dynamic graph convolution can be defined as:*



不同道路段的交通速度之间存在的空间依赖关系可用于提高交通速度预测的性能。尽管图卷积可以聚合相邻节点的隐藏状态以处理道路段之间的空间依赖关系，但大多数现有方法在静态图上使用它。在本文中，为了建模节点之间的潜在且时变的空间依赖关系，我们提出了图卷积的动态版本，以在不同时间上的不同图上进行操作。

给定输入X𝑡−𝑃+1:𝑡，A𝜙(𝑡)反映了时间𝑡时节点之间的空间关系。如图4所示，首先隔离每个节点的输入隐藏状态，然后在从邻接张量中提取A𝜙(𝑡)后，根据它们的动态空间关系将隐藏状态组织成图信号。此外，信息聚合的方法受到了DCRNN [19]的启发，它将交通流视为图上传播过程。在数学形式上，该操作可以通过邻接矩阵、节点的隐藏状态和可学习参数的矩阵乘法来实现。在每一步中，通过加权链接聚合邻居的隐藏状态来更新焦点节点的隐藏状态，动态图卷积可以定义为:

![image-20240103111147679](C:\Users\zhf\AppData\Roaming\Typora\typora-user-images\image-20240103111147679.png)

*where H𝑡 𝑙 represents output hidden states of temporal convolution layer in 𝑙-th block which serves as input of dynamic graph convolution in 𝑙-th block, W𝑘 is parameters for depth 𝑘 and 𝐾 is max diffusion steps.*

其中，$H_{l}^{t}$ 代表第 $l$ 个块中时间卷积层的输出隐藏状态，它作为第𝑙个块中动态图卷积的输入。$W^{k}$ 是深度为𝑘的参数，而𝐾是最大扩散步数。

### 3.3 辅助特征

*Auxiliary feature could not only affect primary feature at the same location, but also be helpful in predicting primary feature of highly related segments. For example, the increasing upstream flow will affect the downstream flow in the future and thus have an effect on the drop of the downstream traffic speed, which behaves as another diffusion pattern. So it is necessary to model spatial dependencies between primary nodes and auxiliary nodes. Here we propose a general framework powered by multi-faceted fusion module to integrate auxiliary feature with primary feature in graph perspective.*

*Meanwhile, we argue that there also exists spatio-temporal dependencies among nodes with auxiliary feature. In each layer, we assign a separate spatio-temporal block for each type of feature (primary feature and auxiliary feature). After the separate structure of 𝑙-th layer, we can get hidden states for primary feature H 𝑝 𝑙 and hidden states for auxiliary feature H𝑎 𝑙 . To model inter-dependencies between auxiliary feature and primary feature, we follow a "propagation and aggregation" paradigm and propose the multi-faceted fusion module:*

辅助特征不仅可以影响相同位置的主要特征，而且还有助于预测高度相关部分的主要特征。例如，上游流量的增加将影响未来的下游流量，从而对下游交通速度的降低产生影响，表现为另一种扩散模式。因此，有必要建模主节点和辅助节点之间的空间依赖关系。在这里，我们提出了一个由多方面融合模块驱动的通用框架，以在图的角度集成辅助特征与主要特征。

同时，我们认为在具有辅助特征的节点之间也存在空间-时间依赖关系。在每个层中，我们为每种类型的特征（主要特征和辅助特征）分配一个单独的空间-时间块。在第 l 层的分离结构之后，我们可以得到主要特征的隐藏状态 $H_{p_l}$ 和辅助特征的隐藏状态 $H_{a_l}$。为了建模辅助特征与主要特征之间的相互依赖关系，我们遵循“传播和聚合”的范例，并提出了多方面融合模块：

![image-20231122164113937](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311221641136.png)

*Specifically, instead of directly concatenating auxiliary hidden states with primary hidden states at same location, we generate a dynamic graph G 𝑎𝑝 represented as A𝑎𝑝 ∈ R 𝑁𝑡×𝑁𝑝×𝑁𝑎 to reflect spatial dependencies between them. The propagation module could propagate the most relative auxiliary hidden states to needed primary nodes and is calculated as:*

具体而言，我们不是直接将辅助隐藏状态与相同位置的主要隐藏状态连接起来，而是生成一个动态图 $G_{ap}$，表示为 $A_{ap} \in \mathbb{R}^{N_t \times N_p \times N_a}$，以反映它们之间的空间依赖关系。传播模块可以将最相关的辅助隐藏状态传播到需要的主节点，计算公式如下：

![image-20231122164323516](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311221643591.png)

where H 𝑎𝑝 𝑙 ∈ R 𝑁𝑝×𝑃×𝑑𝑝 represents effects of the auxiliary feature on primary nodes and 𝑑𝑝 is the dimension of primary hidden states.

To aggregate these unordered information (primary feature and effects of auxiliary feature), we choose SUM(·) as the aggregator function which is differentiable and maintains high representational capacity:

其中，$H_{apl} \in \mathbb{R}^{N_p \times P \times d_p}$ 表示辅助特征对主节点的影响，$d_p$ 是主要隐藏状态的维度。

为了聚合这些无序信息（主要特征和辅助特征的影响），我们选择 SUM(·) 作为聚合函数，这是可微的并且具有很高的表征能力：

![image-20231122164545348](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311221645421.png)

*And the aggregator function could also be chosen as MEAN(·), MAX(·), or CONCAT(·) according to different tasks. Our method passing auxiliary information in a graph perspective can not only consider spatial dependencies between primary nodes and auxiliary nodes, but also work in situation that auxiliary nodes are unaligned with primary nodes. Thanks to dynamic graph constructor, we can easily construct a dynamic graph for unaligned 𝑁𝑎 auxiliary nodes and 𝑁𝑝 primary nodes which could support the procedure of message passing. This makes our method more generic since situation of unaligned data is extremely common in real world: if we want to predict the customer flow in a mall, considering traffic volume of road segments around the mall will be helpful but it cannot be achieved by traditional concatenation methods. 

*And traditional concatenation methods could be viewed as a special case of this framework where the graph at each time is represented as an identity matrix. Under situation where there exist multiple auxiliary features {H 𝑎1 𝑙 , H 𝑎2 𝑙 , · · · , H 𝑎𝑀 𝑙 } and numbers of auxiliary nodes are 𝑁𝑎𝑖 ,𝑖 ∈ [1, 𝑀], Equation 5-7 in our framework could be extended to:*

这一部分讨论了如何根据不同的任务选择聚合函数，包括 MEAN(·)、MAX(·) 或 CONCAT(·)。

我们的方法以图的方式传递辅助信息，不仅可以考虑主节点与辅助节点之间的空间依赖关系，而且还能在辅助节点与主节点不对齐的情况下工作。由于动态图构造器的存在，我们可以轻松地构建一个适用于不对齐的 𝑁𝑎 辅助节点和 𝑁𝑝 主节点的动态图，从而支持消息传递的过程。这使得我们的方法更加通用，因为在现实世界中，数据不对齐的情况非常普遍：例如，如果我们想要预测商场的顾客流量，考虑到商场周围道路段的交通流量会很有帮助，但传统的串联方法无法实现这一点。传统的串联方法可以看作是这个框架的一个特例，其中每个时刻的图被表示为一个单位矩阵。

在存在多个辅助特征集 {H 𝑎1 𝑙 , H 𝑎2 𝑙 , · · · , H 𝑎𝑀 𝑙 } 以及不同数量的辅助节点 𝑁𝑎𝑖 (𝑖 ∈ [1, 𝑀]) 的情况下，我们的框架中的方程 5-7 可以进行扩展。

![image-20231122170926410](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311221709506.png)





### 3.4 时间卷积层

*Traffic speed of a segment is highly correlated with its historical status. This part processes data in time dimension to capture temporal dynamics of traffic. As shown in Figure 2, we adopt temporal convolution layer [23] with the consideration of training speed and simplicity. By stacking dilated convolution layers with increasing dilation factors, the receptive field of models grows exponentially.

And compared to recurrent neural network, dilated convolution layers could be computed in parallel and thus lower time complexity a lot. Meanwhile, gating mechanism shows strengths in handling sequence data, so it’s used in temporal convolution layer to improve model capacity. Specially, the temporal convolution layer is in the form: H 𝑡 𝑙 = tanh(W𝑓 ,𝑙 ★ F𝑙 ) ⊙ 𝜎(W𝑔,𝑙 ★ F𝑙 ), (9) where ⊙ denotes an element-wise multiplication operator, 𝜎(·) is a sigmoid function, ★ is the dilated convolution operation and W(·) is learnable parameters of convolution filters.*

一个路段的交通速度与其历史状态高度相关。这一部分在时间维度上处理数据，以捕捉交通的时间动态。如图2所示，我们采用了考虑到训练速度和简单性的时间卷积层 [23]。通过堆叠增大膨胀因子的膨胀卷积层，模型的感受野呈指数级增长。

与循环神经网络相比，膨胀卷积层可以并行计算，因此大大降低了时间复杂度。同时，门控机制在处理序列数据方面显示出优势，因此在时间卷积层中使用了门控机制以提高模型容量。具体而言，时间卷积层的形式为

$ H_t^l = \tanh(W_{f,l} \star F_l) \odot \sigma(W_{g,l} \star F_l) $

其中，⊙表示逐元素乘法运算符，𝜎(·)是一个 sigmoid 函数，★是膨胀卷积操作，W(·)是卷积滤波器的可学习参数。

### 3.5 其他组件

*In the proposed model, F (·) 𝑙 is input of 𝑙-th block and output of (𝑙 − 1)-th block. Before the first block, input data is transformed by a fully connected layer:*

在提出的模型中，F (·) 𝑙 是第 𝑙 个块的输入和 (𝑙 − 1) 块的输出。在第一个块之前，输入数据通过一个全连接层进行转换：

![image-20231204163254411](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202312041632468.png)

*And residual links are added for each block:*

并且为每个块添加了残差连接：

![image-20231204163351682](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202312041633739.png)

*Moreover, skip connection is added after each temporal convolution layer of primary blocks to the output layer. Hidden states from layers of different depth are concatenated and passed into two fullyconnected layers for the final prediction of primary traffic feature in the future 𝑄 steps:*

此外，在主要块的每个时间卷积层之后，添加了跳过连接到输出层。来自不同深度的层的隐藏状态被连接起来，传递到两个全连接层，用于未来主要交通特征的最终预测，即 𝑄 个步骤：

![image-20231204163546861](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202312041635916.png)

*where || is the concatenation operation, reshape(·) is the function to reshape hidden states H 𝑝,𝑡 𝑙 for concatenation, W(·) and b(·) are learnable parameters. And MAE is used as the objective function to train the model in an end-to-end manner:*

其中，|| 是连接操作，reshape(·) 是用于重塑隐藏状态 H 𝑝,𝑡 𝑙 以进行连接的函数，W(·) 和 b(·) 是可学习的参数。 MAE 被用作端到端训练模型的目标函数：

![image-20231204163639886](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202312041636947.png)





## 四、实验

In this section, we evaluate our proposed model by empirically examining on three real-world datasets. The following research questions (RQs) are used to guide our experiments: 

+ RQ1. How does the proposed model perform compared to existing traffic speed forecasting methods? 
+ RQ2. How does each component contribute to the performance of the proposed model? 
+ RQ3. How does the constructed dynamic graph help to improve the performance of the proposed model? 
+ RQ4. How does the proposed model perform with unaligned auxiliary features?

在本节中，我们通过在三个真实世界的数据集上进行实证研究来评估我们提出的模型。以下的研究问题（RQs）被用来引导我们的实验：

- **RQ1：** 与现有的交通速度预测方法相比，提出的模型的性能如何？

- **RQ2：** 模型的每个组件对提出的模型的性能有何贡献？

- **RQ3：** 构建的动态图如何帮助提高提出的模型的性能？

- **RQ4：** 在辅助特征不对齐的情况下，提出的模型的性能如何？

这些研究问题将用于评估我们的模型，并提供关于模型性能、各组件贡献、动态图的作用以及对于不对齐的辅助特征的适应性的实证结果。



### 4.1 数据集

Our experiments are conducted on three real-world datasets: PeMSD4, PeMSD8 and England respectively1 . These datasets both contains traffic speed and traffic volume collected by sensors. The traffic speed is viewed as the primary feature to predict with traffic volume as auxiliary feature. Statistics of these datasets are shown in Table 2. And other details of the datasets are introduced below: 

+ PeMSD4. It is collected by Caltrans Performance Measurement System (PeMS)2 and released in ASTGCN [14] consisting of average speed, traffic volume in San Francisco Bay Area. Time span is from January to February in 2018. 
+ PeMSD8. Similar as PeMSD4, it consists of average speed, traffic volume collected by PeMS in San Bernardino from July to August in 2016. 
+ England. This dataset is derived from Highways England Traffic Data from Opening up Government of UK3 consisting of average speed, traffic volume around the country. In this paper, data from January to June in 2014 is selected.

我们的实验基于三个真实世界的数据集：PeMSD4、PeMSD8 和 England。这些数据集都包含由传感器收集的交通速度和交通流量信息。交通速度被视为主要特征，而交通流量则作为辅助特征进行预测。这些数据集的统计信息如表2所示。以下是这些数据集的详细介绍：

- **PeMSD4：** 由加利福尼亚州运输部性能测量系统（PeMS）收集，发布在ASTGCN [14]中，包括旧金山湾区的平均速度和交通流量。时间跨度为2018年1月至2月。
- **PeMSD8：** 与PeMSD4类似，包含了由PeMS在2016年7月至8月在圣贝纳迪诺收集的平均速度和交通流量。
- **England：** 此数据集源自英国政府开放的高速公路英格兰交通数据 [3]，包括全国范围内的平均速度和交通流量。本文选取了2014年1月至6月的数据。

这些数据集的详细统计信息请参见表2。



### 4.3 实验设置

*In our model, the number of blocks in each part is set to 8, dilated ratio of each layer is [1, 2, 1, 2, 1, 2, 1, 2] and the max depth of graph convolution layers is set to 2. Channel size of dilated convolution and graph convolution is 32 and hidden dimension in dynamic graph constructor is set to 16. Batch size is set to 64 and learning rate is set to 0.001. The main hypeparameters are tuned on validation set. The model is optimized by Adam optimizer and an early stop strategy is used with patience of 20. All datasets are split by ratio of 6:2:2 in chronological order. For PeMSD4 and PeMSD8, the missing values are filled by the linear interpolation. Predefined graph is initialized according distance between sensors. For the fairness of comparison experiments, except for HA and VAR, input features of each baseline include historical primary feature, historical auxiliary feature, time in a day and day in a week. For LR and XGBoost, we train a model for each prediction step. Every experiment is repeated 5 times and the average performance is reported. Experiments are executed on a machine with four TitanXp GPUs. Three metrics are adopted to evaluate performance of each model: Mean Absolute Error (MAE), Mean Absolute Percentage Error (MAPE), Root Mean Squared Error (RMSE) respectively.*

在我们的模型中，每个部分的块数设置为 8，每个层的扩张比率为 [1, 2, 1, 2, 1, 2, 1, 2]，图卷积层的最大深度设置为 2。扩张卷积和图卷积的通道大小为 32，动态图构造器中的隐藏维度设置为 16。批量大小设置为 64，学习率设置为 0.001。主要超参数在验证集上进行了调整。模型由 Adam 优化器优化，采用了一个耐心值为 20的提前停止策略。所有数据集按照时间顺序按比例分为 6:2:2。对于 PeMSD4 和 PeMSD8，缺失值通过线性插值填充。预定义的图是根据传感器之间的距离初始化的。为了公平比较实验，在HA和VAR以外的所有基线模型中，每个基线的输入特征包括历史主要特征、历史辅助特征、一天中的时间和一周中的天。对于LR和XGBoost，我们为每个预测步骤训练一个模型。每个实验重复 5 次，报告平均性能。实验在一台配备有四个 TitanXp GPU 的机器上执行。采用三个指标来评估每个模型的性能：平均绝对误差（MAE）、平均百分比误差（MAPE）、平方根均方误差（RMSE）。



### 4.4 与现有模型的比较

*The results of the comparison with baselines are shown in Table 1. On all three datasets which are collected at multiple locations and with various sampling rates, our proposed model always achieves the start-of-the-art performance whether long-term or short-term, which demonstrates the effectiveness of our proposed Research Track Paper KDD ’21, August 14–18, 2021, Virtual Event, Singapore model. Moreover, deep learning based models could achieve better performance than traditional statistic methods which demonstrates superior capacity of deep learning based models. Methods like DCRNN and ASTGCN highly rely on predefined graph which may not capture crucial dependencies between nodes leading to a worse performance. Although GMAN achieves competitive performance in Horizon 12, the results of short-term prediction has a distinct margin with our method. But compared with other work, GMAN has a clear improvement in Horizon 12, which may owe to the transform attention layer. Except for our model, Graph Wavenet and MTGNN perform well generally which might benefit from that they adopt adaptive graph to model relationships between nodes, indicating that adaptive graph based methods could effectively exploit valuable but latent spatial dependencies from historical traffic data. However, dynamic characteristic of traffic spatial dependencies and spatial dependencies between multi-faceted features are omitted in these work. The design of dynamic graph constructor and dynamic graph convolution help our model capture the subtler correlations between nodes. Moreover, as another way containing information of time in a day, our method has a better performance compared with taking it as input directly. And the interactions between auxiliary features and primary feature by multi-faceted fusion module also contribute a lot in this match. Taking these two factors into consideration make our model perform better than these methods consistently.*

与基准方法的比较结果如表1所示。在收集自多个位置且具有不同采样率的三个数据集上，我们提出的模型始终在长期和短期方面取得了最先进的性能，这证明了我们提出的研究模型的有效性。此外，基于深度学习的模型能够比传统统计方法实现更好的性能，这表明了深度学习模型的卓越能力。像DCRNN和ASTGCN这样的方法高度依赖于预定义图，这可能无法捕捉节点之间的关键依赖关系，从而导致性能较差。尽管GMAN在Horizon 12方面取得了竞争性能，但短期预测的结果与我们的方法有明显的差距。但与其他工作相比，GMAN在Horizon 12方面有明显的改善，这可能归功于变换注意力层。除了我们的模型之外，Graph Wavenet和MTGNN总体表现良好，这可能得益于它们采用自适应图来建模节点之间的关系，表明自适应图方法可以有效地利用历史交通数据中有价值但潜在的空间依赖关系。

然而，这些工作忽略了交通空间依赖关系的动态特性以及多方面特征之间的空间依赖关系。动态图构造器和动态图卷积的设计有助于我们的模型捕捉节点之间更微妙的相关性。此外，作为包含一天时间信息的另一种方式，我们的方法与直接将其作为输入的方法相比具有更好的性能。多方面融合模块通过辅助特征与主要特征之间的交互也在此匹配中起到了很大的作用。考虑到这两个因素使得我们的模型在一致性上表现更好。





## 五、相关工作

*This section reviews the previous literature related to traffic speed forecasting and spatio-temporal graph neural network.*

这一部分回顾了与交通速度预测和时空图神经网络相关的先前文献。

### 5.1 交通速度预测

*Traffic forecasting has attracted much attention of enormous researchers in the past decades [21]. To exploit traffic patterns from data and provide prediction, simple statistic methods including ARIMA[3], VAR [36] for time series forecasting have been developed. Though they can leverage accumulated traffic data, they were built upon stationary assumptions. In recent years, researchers have been shifting to deep learning methods that are powerful to capture non-linear patterns. Thanks to gated mechanism, recurrent neural network, such as LSTM [15], could exploit patterns from sequential data and were leveraged in traffic forecasting [22]. However, spatial dependencies between segments were omitted which encourage researchers to propose methods taking spatial dependencies into consideration. Methods were proposed to split the city into grids and utilized the idea from computer vision to handle spatial dependencies in a local area [28–30, 33]. Though these methods took a big step to model spatial dependencies and temporal dependencies simultaneously, the spatial dependencies were in the Euclidean space while segments are naturally organized as a graph in real world.*

*The inaccurate assumptions limited the performance and generality of them. More recently, several methods combining graph neural network and sequential learning network were proposed to handle spatial and temporal dependencies in traffic scenario and achieved satisfying performance [1, 14, 19, 26, 27, 32]. On the other hand, some work [8, 14, 20] realized the importance of auxiliary features, but they simply linked auxiliary features to the primary feature with the same location and time slot lacking generality and omitting spatial dependencies between multi-faceted features.*

交通预测在过去几十年中引起了大量研究者的关注[21]。为了从数据中挖掘交通模式并提供预测，已经开发了一些简单的统计方法，包括用于时间序列预测的ARIMA [3]、VAR [36]等。尽管它们可以利用积累的交通数据，但它们建立在平稳的假设上。近年来，研究者们逐渐转向深度学习方法，这些方法能够捕捉非线性模式。借助门控机制，例如LSTM [15]这样的循环神经网络可以从序列数据中挖掘模式，并被用于交通预测[22]。然而，它们忽略了路段之间的空间依赖关系，促使研究者提出考虑空间依赖关系的方法。有人提出将城市划分为网格，并利用计算机视觉的思想在局部区域处理空间依赖关系[28–30, 33]。尽管这些方法在同时建模空间依赖性和时间依赖性方面取得了重要进展，但空间依赖性是在欧几里得空间中，而在真实世界中，路段自然组织成一个图。

这些不准确的假设限制了它们的性能和通用性。最近，一些结合图神经网络和顺序学习网络的方法被提出，以处理交通场景中的空间和时间依赖性，并取得了令人满意的性能[1, 14, 19, 26, 27, 32]。另一方面，一些研究[8, 14, 20]意识到了辅助特征的重要性，但它们只是简单地将辅助特征与相同位置和时间槽的主要特征连接起来，缺乏通用性并忽略了多方面特征之间的空间依赖关系。



### 5.2 时空图神经网络

*Due to advantage of both graph neural network and sequential learning methods, spatio-temporal graph neural network methods could handle the spatial dependencies in non-Euclidean space and temporal dependencies simultaneously. Along this line, it is a challenging problem to construct a suitable graph between segments. STGCN [32], DCRNN [19], ASTGCN [14] combined spatial and temporal network including Chebnet [17], GRU [7], diffusion convolution and 1-D convolution. GMAN [35] utilized node2vec [13], an embedding method for nodes in graph, and extended selfattention mechanism [25] to spatial dimension. CurbGAN [34] leveraged Pearson Correlation Coefficient to measure edge weights between nodes. Some works [4, 12] took a further step to consider multiple types of correlations between nodes include distance, Pearson Correlation Coefficient of historical data, functional similarity and interactions between each pair of nodes. These methods could handle correlations with arbitrary number of neighbors, but their graphs were designed, which is intuitive but incomplete. In recent years, some work to generate adaptive graph from data have been proposed to solve this problem. Graph Wavenet [27] added an adaptive term by generating one learnable correlation matrix to extend diffusion convolution from DCRNN for better capturing latent graph behind data. AGCRN [1] kept adaptive term only and still achieved superior performance. MTGNN [26] took available external node features to generate one adaptive graph by graph learning layer. Though these methods could exploit correlations from data, they all focused on static graphs omitting dynamic characteristics of spatial dependencies between segments*

由于图神经网络和顺序学习方法的优势，时空图神经网络方法可以同时处理非欧几里得空间的空间依赖性和时间依赖性。沿着这一线路，构建合适的分段图成为一个具有挑战性的问题。STGCN [32]、DCRNN [19]、ASTGCN [14]结合了空间和时间网络，包括Chebnet [17]、GRU [7]、扩散卷积和1-D卷积。GMAN [35]利用了node2vec [13]，一种用于图中节点嵌入的方法，并扩展了空间维度的自注意机制 [25]。CurbGAN [34]利用Pearson相关系数来衡量节点之间的边权重。一些工作 [4, 12] 迈出了进一步的一步，考虑了节点之间的多种关联，包括距离、历史数据的Pearson相关系数、功能相似性以及每对节点之间的相互作用。这些方法可以处理任意数量邻居的相关性，但它们的图是被设计好的，这种设计是直观但不完整的。近年来，一些通过数据生成自适应图的工作已经提出来解决这个问题。Graph Wavenet [27]通过生成一个可学习的相关矩阵添加自适应项，以扩展DCRNN中的扩散卷积，更好地捕捉数据背后的潜在图。AGCRN [1]仅保留了自适应项，仍然取得了卓越的性能。MTGNN [26]利用可用的外部节点特征通过图学习层生成一个自适应图。尽管这些方法可以从数据中挖掘相关性，但它们都集中在静态图上，忽略了路段之间动态空间依赖关系的特征。
