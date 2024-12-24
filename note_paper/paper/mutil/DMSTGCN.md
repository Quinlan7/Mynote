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

尽管如此，两个关键问题很少被讨论。==第一个问题是如何动态地建模道路段之间的空间依赖关系。==在大多数现有的研究中，两个道路段之间的影响被视为静态的，并由预定义的或自学得的邻接矩阵确定。然而，两个道路段在不同时间的相互作用应该是不同的。如图1中所示，两个段的交通速度（右上角）显示，它们的速度模式在早高峰时期相似，而在晚高峰时期完全不同。==第二个问题是如何将交通速度预测问题与丰富的多方位辅助数据结合起来==。城市交通网络是复杂、动态且庞大的系统。不同类型的交通数据记录了从不同角度观察交通系统的观测结果。因此，通过从多方位数据中深入充分挖掘交通系统潜在的模式和动态，有望提高交通速度预测的性能。正如图1中交通速度和交通量的组合（右下角）所示，在早晚高峰期间交通量迅速增加后，交通速度急剧下降，这揭示了交通量在辅助交通速度预测方面的潜力。

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
2. 提供了一个多方位的融合模块，用于在空间和时间上将辅助隐藏状态与主要隐藏状态相结合。它是一个通用的框架，用于处理多面向的时空数据。
3. 我们不仅通过对真实数据集的实验证明了所提方法的有效性，还揭示了多层面数据之间关系的显式和发现的动态模式。



## 二、预备知识

在本节中，我们首先介绍本文所使用的符号及预备知识。然后，我们将利用多层面数据进行交通速度预测的问题形式化。

### 2.1 符号和定义

定义2.1（交通速度）。交通速度是路段描述交通状况的一个关键特征。在本文中，我们将具有历史速度数据的路段集合记为$V_{p}=\{v_{1},v_{2},\cdots,v_{N_{p}}\}$，其中 $N_{p}$ 是路段的数量，并且将第 $i$ 个路段在时隙 $t$ 的交通速度记为$x^{p}_{i,t}\in \mathbb{R}$。此外，我们使用 $X^{p}_{t}\in \mathbb{R}^{N_{p}}$ 来表示在时隙 $t$ 的所有交通速度观测值。 

定义2.2（辅助特征）。不同的交通特征（如交通速度和交通流量）之间存在着潜在的相关性。尽管某些特征在某些特定应用中并不能直接发挥作用，但基于某些原因，它们能够有助于交通速度的预测。在本文中，具有历史辅助特征的路段集合记为 $V_{a}=\{s_{1},s_{2},\cdots,s_{N_{a}}\}$，其中 $N_{a}$ 是路段的数量，并且将第 $i$ 个路段在时隙 $t$ 的辅助特征记为 $x^{a}_{i,t}\in \mathbb{R}$ 。与交通速度类似，我们使用 $X^{a}_{t}\in \mathbb{R}^{N_{a}}$ 来表示在时隙 $t$ 的所有路段的辅助特征。

定义2.3（动态图）。不同路段之间的相关性是动态变化的，这种相关性从黎明到黄昏都会有所不同。这促使我们构建动态图，该动态图以一组路段作为其节点，但在每个时刻有着不同的边。对于时隙 $t$，交通图记为 $G_{t}=\{\mathbb{V} , \mathbb{E} ^{t}\}$ ，其中 $\mathbb{V} $ 是节点集合，$\mathbb{E} ^{t}$ 是边的集合。$e_{t,i,j}=(v_{i},v_{j},A_{t,i,j})\in E^{t}$ 表示在时隙 $t$ 存在一条从$v_{j}$ 指向 $v_{i}$ 的边，其边权重为 $A_{t,i,j}$，它是一个三阶邻接张量 $A$ 的第 $(t,i,j)$ 个元素。此外，$G_{t}$ 可以用矩阵 $A_{t}$ 的形式来表示，而整个动态图则可以用 $A$ 来表示。  

### 2.2 问题公式化

交通速度预测旨在利用过去一段时间内的历史速度数据预测所有路段在未来一段时间内的交通速度。在本文中，我们进一步考虑历史辅助数据的影响。形式化地表述为，给定交通速度的 $P$ 个历史步长以及辅助特征的 $P$ 个历史步长，我们的目标是学习一个模型 $f$ 来预测未来 $Q$ 个步长的交通速度，即 $\hat{X}^{p}_{t + 1:t + Q}=f(X^{p}_{t - P + 1:t}, X^{a}_{t - P + 1:t})$。 在本文中，我们重点关注交通速度预测，因此在本文的后半部分我们也将交通速度称为“主要特征”。 



## 三、方法

在本节中，我们将详细介绍我们所提出的模型。我们所提模型的整体框架如图2所示，该框架大致可划分为四个部分：动态图构建器、主要部分、辅助部分以及多层面融合模块。 

![image-20231204162742462](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202312041627607.png)

> DMSTGCN的架构。该框架主要由两个并行部分组成：一个用于主要特征，另一个用于辅助特征。动态图构造器为图卷积层生成动态图。主要部分包括 𝐿 个主要块，每个块由一个时间卷积层和一个动态图卷积层组成。辅助部分类似于主要部分。
>
> 多方面融合模块首先将辅助块的输出传播并与主要块的输出聚合，作为下一层的输入。为每个块添加了残差连接，主要部分每个时间卷积后的隐藏状态连接到输出层，用于最终预测。

### 3.1 动态图构造器

由于交通的动态特性，路段之间的空间依赖关系是隐含的并且不断变化的，因此有必要设计一个合适的图学习模块来解决这个问题。在先前的工作中，节点之间的依赖关系要么是由人类知识（如距离和功能相似性）定义的，要么是生成的静态图，这忽略了交通空间依赖关系的动态特性。在本文中，为了捕捉节点之间变化的关系，我们提出了动态图构建器，它产生用于动态图卷积和多层面融合模块的动态可学习图。

然而，在每一对节点之间自适应地建模关系并不是一件简单的事情。最简单的方法是直接分配一个可学习的参数张量来表示动态关系，但是这种方法的复杂性是 𝑂(𝑇 𝑁 𝑁)，其中 𝑇 是总时隙的数量，𝑁 是节点的数量，使得计算和覆盖变得困难。考虑到交通状况的周期性，我们假设一天中相同时间的交通可以共享同一个图。因此，我们的目标转变为一个邻接张量 𝐴 ∈ R 𝑁𝑡×𝑁 ×𝑁，其中 𝑁𝑡 是一天中的时间槽数。在这里，时隙 𝑡 的图是 𝐴𝜙 (𝑡)，其中 𝜙 (𝑡) 是获取一天中时间的函数。但是这种解决方案的复杂性仍然是 𝑂(𝑁𝑡𝑁 𝑁)，随着图的变大，它呈二次增长。实际上，动态图的一些结构可能会在时间和空间上共享。例如，两个相邻的路段可能在一天中保持相关性，人们在早上会从不同的居住区到不同的工作区。

因此，我们提出了一种受 Tucker 分解 [24] 启发的方法，用于形成如图3所示的邻接张量。在交通速度预测中，我们的目标不是分解已知张量，而是用可学习的参数为空间依赖性组成未知张量。这个过程与原始的 Tucker 分解相反，并在训练过程中通过反向传播更新动态图。

具体而言，我们分配了三个可学习矩阵和一个可学习的核心张量，包括时间槽嵌入 E 𝑡 ∈ R 𝑁𝑡×𝑑、源节点嵌入 E 𝑠 ∈ R 𝑁𝑠×𝑑、目标节点嵌入 E 𝑒 ∈ R 𝑁𝑒×𝑑 以及一个核心张量 E 𝑘 ∈ R 𝑑×𝑑×𝑑，其中 𝑁𝑡、𝑁𝑠、𝑁𝑒、𝑑 分别表示时间槽数、原始节点数量、目标节点数量和嵌入维度。邻接张量的计算如下：
$A ′_{𝑡,𝑖,𝑗} = \sum_{o=1}^{d} \sum_{q=1}^{d} \sum_{r=1}^{d} E 𝑘_{o,q,r}E 𝑡_{t,o}E 𝑒_{i,q}E 𝑠_{j,r}$
$A ′′_𝑡,𝑖,𝑗 = \max(0, A ′_𝑡,𝑖,𝑗)$
$A_𝑡,𝑖,𝑗 = \frac{e^{A ′′_𝑡,𝑖,𝑗}}{\sum_{n=1}^{N_s} e^{A ′′_𝑡,𝑖,𝑛}}$

在这项工作中，生成三个张量 $A_p \in R^{N_t \times N_p \times N_p}$、$A_a \in R^{N_t \times N_a \times N_a}$ 和 $A_{ap} \in R^{N_t \times N_p \times N_a}$ 用于表示动态图：主要节点之间的图 $G_p$、辅助节点之间的图 $G_a$、主要节点和辅助节点之间的图 $G_{ap}$。

![image-20240529212429844](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202405292124104.png)

> 动态图的构建。在单个时间槽上的图可以表示为一个矩阵，而在所有时间槽上的图可以通过添加时间维度表示为一个张量。为了简化模型，可以使用张量分解的思想来构建这个张量，并利用空间依赖的低秩特性。

### 3.2 动态图卷积

![image-20240627222044583](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202406272220948.png)

> 动态图卷积。输入是多个无序的孤立数据段。通过从动态图张量中获取随时间变化的邻接矩阵，可以将输入数据根据动态邻接矩阵进行组织，并在其上应用图卷积操作。根据最终的预测结果，邻接张量将通过反向传播进行更新。

不同道路段的交通速度之间存在的空间依赖关系可用于提高交通速度预测的性能。尽管图卷积可以聚合相邻节点的隐藏状态以处理道路段之间的空间依赖关系，但大多数现有方法在静态图上使用它。在本文中，为了建模节点之间的潜在且时变的空间依赖关系，我们提出了图卷积的动态版本，以在不同时间上的不同图上进行操作。

给定输入X𝑡−𝑃+1:𝑡，A𝜙(𝑡)反映了时间𝑡时节点之间的空间关系。如图4所示，首先隔离每个节点的输入隐藏状态，然后在从邻接张量中提取A𝜙(𝑡)后，根据它们的动态空间关系将隐藏状态组织成图信号。此外，信息聚合的方法受到了DCRNN [19]的启发，它将交通流视为图上传播过程。在数学形式上，该操作可以通过邻接矩阵、节点的隐藏状态和可学习参数的矩阵乘法来实现。在每一步中，通过加权链接聚合邻居的隐藏状态来更新焦点节点的隐藏状态，动态图卷积可以定义为:

![image-20240103111147679](C:\Users\zhf\AppData\Roaming\Typora\typora-user-images\image-20240103111147679.png)

其中，$H_{l}^{t}$ 代表第 $l$ 个块中时间卷积层的输出隐藏状态，它作为第𝑙个块中动态图卷积的输入。$W^{k}$ 是深度为𝑘的参数，而𝐾是最大扩散步数。

> **动态图卷积（Dynamic Graph Convolution）** 是一种图神经网络操作，用于在动态或时间变化的图上进行卷积操作，以捕捉随时间变化的空间依赖关系。这里的动态图卷积扩展了传统的静态图卷积，使得卷积操作能够根据不同时间点的图结构进行自适应调整。下面我们分两个部分详细解释一下动态图卷积的概念和公式。
>
> ### 动态图卷积的概念
>
> #### **背景**
> - **静态图卷积**: 传统的图卷积（Graph Convolution）方法在一个固定的图结构上进行操作，这种方法适用于捕捉节点之间的空间依赖关系，但不能处理随时间变化的图结构。
> - **动态图卷积**: 为了解决这个问题，动态图卷积引入了时间维度，可以在不同时间点上动态调整图结构，从而更准确地捕捉节点之间的时空依赖关系。
>
> #### **目的**
> 动态图卷积的主要目的是在不同时间点上，根据时间变化的图结构对节点的特征进行卷积操作。这对于处理交通流预测等需要考虑时空动态变化的任务特别有用。
>
> ### 公式解释
>
> 公式描述了动态图卷积操作的数学定义：
>
> $$ \mathbf{H}_l = \sum_{k=0}^{K} \left(\mathbf{A}_{\phi(t)}\right)^k \mathbf{H}_l^t \mathbf{W}^k $$
>
> - **$$\mathbf{H}_l$$**: 是动态图卷积输出的隐藏状态，表示在 \(l\) 层卷积后的特征矩阵。
> - **$$\mathbf{H}_l^t$$**: 是在时间 \(t\) 的 \(l\) 层动态图卷积输入的隐藏状态。它是时序卷积层的输出，作为当前时间步的输入特征。
> - **$$\mathbf{A}_{\phi(t)}$$**: 是在时间 \(t\) 的邻接矩阵，表示时间 \(t\) 的图结构。它反映了节点之间的动态空间关系。
> - **$$\mathbf{W}^k$$**: 是学习到的权重参数，用于在深度 \(k\) 处的卷积操作。
> - **$$K$$**: 是最大扩散步数，表示在图卷积中进行多少次扩散操作。
>
> #### 公式解读
> 1. **矩阵乘积 $$\left(\mathbf{A}_{\phi(t)}\right)^k$$**: 这表示邻接矩阵的 $$k$$ 次幂，用来在 $$k$$ 步邻居范围内进行扩散操作。随着 $$k$$ 的增加，节点信息会向更远的邻居扩散。
>   
> 2. **聚合操作**: 对于每个扩散深度 $$k$$，执行矩阵乘积 $$\left(\mathbf{A}_{\phi(t)}\right)^k \mathbf{H}_l^t \mathbf{W}^k$$。这个操作将节点 $$l$$ 在时间 $$t$$ 的隐藏状态 $$\mathbf{H}_l^t$$ 通过邻接矩阵 $$\mathbf{A}_{\phi(t)}$$ 的 $$k$$ 次幂扩散，并与学习到的权重 $$\mathbf{W}^k$$ 相乘，完成邻居信息的聚合。
>
> 3. **求和操作**: 最终将不同扩散深度 $$k$$ 的结果求和，得到层 $$l$$ 的输出 $$\mathbf{H}_l$$。这个求和操作结合了不同扩散深度的信息，为每个节点提供了聚合后的特征。
>
> ### 举例说明
>
> 假设我们有一个交通网络的图，其中每个节点代表一个路段，边表示路段之间的相连关系。假设图在不同时刻的连接关系会变化，例如因交通流量变化或路障调整等。动态图卷积将：
>
> 1. **根据当前时间 $$t$$ 提取的邻接矩阵 $$\mathbf{A}_{\phi(t)}$$**，反映当前时间点的节点连接关系。
> 2. **对每个节点进行扩散操作**，通过不同深度 $$k$$ 次扩散来捕捉从直接邻居到更远邻居的信息。
> 3. **聚合这些信息**，结合每个节点的初始隐藏状态和学习到的权重，生成时间 $$t$$ 的特征输出。
>
> 通过这种方法，动态图卷积能够更精细地捕捉到随时间变化的交通流模式，从而提高交通速度预测的准确性。
>
> ### 总结
>
> 动态图卷积通过在不同时间点上动态调整图结构，来有效捕捉节点之间的时空依赖关系。公式 $$ \mathbf{H}_l = \sum_{k=0}^{K} \left(\mathbf{A}_{\phi(t)}\right)^k \mathbf{H}_l^t \mathbf{W}^k $$ 详细描述了如何利用时间变化的邻接矩阵 $$\mathbf{A}_{\phi(t)}$$ 和隐藏状态 $$\mathbf{H}_l^t$$ 在多层图卷积中进行信息聚合，从而在图上进行时空扩散并更新节点特征。

### 3.3 辅助特征

辅助特征不仅可以影响相同位置的主要特征，而且还有助于预测高度相关部分的主要特征。例如，上游流量的增加将影响未来的下游流量，从而对下游交通速度的降低产生影响，表现为另一种扩散模式。因此，有必要建模主节点和辅助节点之间的空间依赖关系。在这里，我们提出了一个由多方面融合模块驱动的通用框架，以在图的角度集成辅助特征与主要特征。

同时，我们认为在具有辅助特征的节点之间也存在空间-时间依赖关系。在每个层中，我们为每种类型的特征（主要特征和辅助特征）分配一个单独的空间-时间块。在第 l 层的分离结构之后，我们可以得到主要特征的隐藏状态 $H_{p_l}$ 和辅助特征的隐藏状态 $H_{a_l}$。为了建模辅助特征与主要特征之间的相互依赖关系，我们遵循“传播和聚合”的范例，并提出了多方面融合模块：

![image-20231122164113937](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311221641136.png)

具体而言，我们不是直接将辅助隐藏状态与相同位置的主要隐藏状态连接起来，而是生成一个动态图 $G_{ap}$，表示为 $A_{ap} \in \mathbb{R}^{N_t \times N_p \times N_a}$，以反映它们之间的空间依赖关系。传播模块可以将最相关的辅助隐藏状态传播到需要的主节点，计算公式如下：

![image-20231122164323516](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311221643591.png)

其中，$H_{apl} \in \mathbb{R}^{N_p \times P \times d_p}$ 表示辅助特征对主节点的影响，$d_p$ 是主要隐藏状态的维度。

为了聚合这些无序信息（主要特征和辅助特征的影响），我们选择 SUM(·) 作为聚合函数，这是可微的并且具有很高的表征能力：

![image-20231122164545348](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311221645421.png)

这一部分讨论了如何根据不同的任务选择聚合函数，包括 MEAN(·)、MAX(·) 或 CONCAT(·)。

我们的方法以图的方式传递辅助信息，不仅可以考虑主节点与辅助节点之间的空间依赖关系，而且还能在辅助节点与主节点不对齐的情况下工作。由于动态图构造器的存在，我们可以轻松地构建一个适用于不对齐的 𝑁𝑎 辅助节点和 𝑁𝑝 主节点的动态图，从而支持消息传递的过程。这使得我们的方法更加通用，因为在现实世界中，数据不对齐的情况非常普遍：例如，如果我们想要预测商场的顾客流量，考虑到商场周围道路段的交通流量会很有帮助，但传统的串联方法无法实现这一点。传统的串联方法可以看作是这个框架的一个特例，其中每个时刻的图被表示为一个单位矩阵。

在存在多个辅助特征集 {H 𝑎1 𝑙 , H 𝑎2 𝑙 , · · · , H 𝑎𝑀 𝑙 } 以及不同数量的辅助节点 𝑁𝑎𝑖 (𝑖 ∈ [1, 𝑀]) 的情况下，我们的框架中的方程 5-7 可以进行扩展。

![image-20231122170926410](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311221709506.png)





### 3.4 时间卷积层

*Traffic speed of a segment is highly correlated with its historical status. This part processes data in time dimension to capture temporal dynamics of traffic. As shown in Figure 2, we adopt temporal convolution layer [23] with the consideration of training speed and simplicity. By stacking dilated convolution layers with increasing dilation factors, the receptive field of models grows exponentially.

And compared to recurrent neural network, dilated convolution layers could be computed in parallel and thus lower time complexity a lot. Meanwhile, gating mechanism shows strengths in handling sequence data, so it’s used in temporal convolution layer to improve model capacity. Specially, the temporal convolution layer is in the form: H 𝑡 𝑙 = tanh(W𝑓 ,𝑙 ★ F𝑙 ) ⊙ 𝜎(W𝑔,𝑙 ★ F𝑙 ), (9) where ⊙ denotes an element-wise multiplication operator, 𝜎(·) is a sigmoid function, ★ is the dilated convolution operation and W(·) is learnable parameters of convolution filters.*

一个路段的交通速度与其历史状态高度相关。这一部分在时间维度上处理数据，以捕捉交通的时间动态。如图2所示，我们采用了考虑到训练速度和简单性的时间卷积层 [23]。通过堆叠增大膨胀因子的膨胀卷积层，模型的感受野呈指数级增长。

与循环神经网络相比，膨胀卷积层可以并行计算，因此大大降低了时间复杂度。同时，==门控机制==在处理序列数据方面显示出优势，因此在时间卷积层中使用了门控机制以提高模型容量。具体而言，时间卷积层的形式为

$ H_t^l = \tanh(W_{f,l} \star F_l) \odot \sigma(W_{g,l} \star F_l) $

其中，⊙表示逐元素乘法运算符，𝜎(·)是一个 sigmoid 函数，★是膨胀卷积操作，W(·)是卷积滤波器的可学习参数。

![image-20240530143911589](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202405301439748.png)

> 是的，这个函数表达式 *H**t**l*=tanh(*W**f*,*l*⋆*F**l*)⊙*σ*(*W**g*,*l*⋆*F**l*) 使用了门控机制。
>
> 在这个表达式中，*H**t**l* 表示第 *l* 层在时间步 *t* 的输出。这里有两个主要的组成部分：
>
> 1. tanh(*W**f*,*l*⋆*F**l*)：这部分计算了一个候选状态，其中 *W**f*,*l* 是权重矩阵，⋆ 表示卷积操作（或者在其他上下文中可能是矩阵乘法），*F**l* 是第 *l* 层的输入特征图（或输入向量）。tanh 函数将结果映射到 [−1,1] 的范围内。
> 2. *σ*(*W**g*,*l*⋆*F**l*)：这部分计算了一个门控信号，其中 *W**g*,*l* 是另一个权重矩阵，*σ* 是 sigmoid 函数。sigmoid 函数的输出在 [0,1] 范围内，可以解释为信息通过门控的比例。
>
> ⊙ 表示逐元素相乘（Hadamard乘积），它将候选状态与门控信号相乘，从而控制候选状态中哪些信息应该被保留或丢弃。这种机制允许网络学习在不同时间步和不同层之间选择性地传递信息，从而实现了门控机制。
>
> 这种门控机制在卷积LSTM（ConvLSTM）等网络结构中很常见，用于处理具有空间依赖性的序列数据，如视频或图像序列。

### 3.5 其他组件

*In the proposed model, F (·) 𝑙 is input of 𝑙-th block and output of (𝑙 − 1)-th block. Before the first block, input data is transformed by a fully connected layer:*

在提出的模型中，$F_l^{(\cdot)}$是第 𝑙 个块的输入和 (𝑙 − 1) 块的输出。在第一个块之前，输入数据通过一个全连接层进行转换：

![image-20231204163254411](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202312041632468.png)

> 这个全连接层实现了：(B, 1, N, T) -> (B, residual_channels 32, N, T)
>
> 在深度学习中，特别是在处理序列数据（如音频、视频或时间序列数据）或空间数据（如图像）时，改变特征通道数（或称为特征维度、特征映射数量）是一个常见的操作。当你提到一个模型在输入之前使用一个全连接层（或卷积层，但在这里更可能是1x1卷积，因为它不改变空间维度）将特征通道数从1增加到32，这一操作的意义可能包括以下几点：
>
> 1. **增加模型容量**：更多的特征通道意味着模型可以学习更复杂的表示。每个新的通道都可以看作是一个新的“特征检测器”，它可以从输入数据中捕获不同的信息。
> 2. **多模态或多元数据处理**：虽然在这个特定的例子中你只提到了从1个通道增加到32个通道，但在处理多模态数据（如同时包含音频和视频的数据）时，这样的操作很常见。每个通道可能对应于不同的模态或不同的特征集。
> 3. **非线性变换**：全连接层（或带有激活函数的卷积层）可以对输入进行非线性变换，这有助于模型学习输入和输出之间的复杂关系。
> 4. **减少过拟合风险**（在一定程度上）：虽然增加模型容量可能会增加过拟合的风险，但使用更多的特征通道（如果正确使用正则化技术，如Dropout、L1/L2正则化等）也可以帮助模型更好地泛化到未见过的数据。
> 5. **适应预训练模型**：如果你的模型后面要连接到一个预训练的模型（如一个预训练的卷积神经网络），那么调整输入通道数以匹配预训练模型的输入要求是很常见的。
> 6. **特征融合**：在某些情况下，你可能希望将来自不同来源或不同处理阶段的特征融合在一起。通过增加特征通道数，你可以轻松地将这些特征“堆叠”在一起，并将它们作为模型的输入。

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

我们的实验基于三个真实世界的数据集：PeMSD4、PeMSD8 和 England。这些数据集都包含由传感器收集的交通速度和交通流量信息。交通速度被视为主要特征，而交通流量则作为辅助特征进行预测。这些数据集的统计信息如表2所示。以下是这些数据集的详细介绍：

- **PeMSD4：** 由加利福尼亚州运输部性能测量系统（PeMS）收集，发布在ASTGCN [14]中，包括旧金山湾区的平均速度和交通流量。时间跨度为2018年1月至2月。
- **PeMSD8：** 与PeMSD4类似，包含了由PeMS在2016年7月至8月在圣贝纳迪诺收集的平均速度和交通流量。
- **England：** 此数据集源自英国政府开放的高速公路英格兰交通数据 [3]，包括全国范围内的平均速度和交通流量。本文选取了2014年1月至6月的数据。

这些数据集的详细统计信息请参见表2。

![image-20240701190705157](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202407011907262.png)

### 4.2 基线

为了验证DMSTGCN的性能，我们选择了包括统计方法、基于预定义图的方法、基于注意力的方法和基于自适应图的方法作为基线模型进行比较。这些基线模型的描述如下：

• HA：历史平均法。考虑到交通的周期性，我们使用训练数据集中同一天相同时段的交通速度平均值作为预测值。

• VAR [36]：一种用于捕捉多个量之间关系的统计模型。

• LR：一种回归模型，利用输入和输出之间的线性相关性。

• XGBoost [5]：一种基于梯度提升树的方法。

• DCRNN [19]：一种时空网络，结合了扩散图卷积和递归神经网络。

• ASTGCN [14]：一种模型，在应用空间和时间卷积之前应用空间和时间注意力机制。为了公平起见，我们只使用其最近的组件。

• GMAN [35]：一种基于输入特征、空间嵌入和时间嵌入的考虑空间和时间相关性的图多注意力模型。

• Graph Wavenet [27]：一种时空网络，介绍了一种生成自适应图的方法，并将扩散图卷积与一维膨胀卷积相结合。

• MTGNN [26]：一种使用外部特征生成单向自适应图的时空网络。

### 4.3 实验设置

在我们的模型中，每个部分的块数设置为 8，每个层的扩张比率为 [1, 2, 1, 2, 1, 2, 1, 2]，图卷积层的最大深度设置为 2。膨胀卷积和图卷积的通道大小为 32，动态图构造器中的隐藏维度设置为 16。批量大小设置为 64，学习率设置为 0.001。主要超参数在验证集上进行了调整。模型由 Adam 优化器优化，采用了一个耐心值为 20的提前停止策略。所有数据集按照时间顺序按比例分为 6:2:2。对于 PeMSD4 和 PeMSD8，缺失值通过线性插值填充。预定义的图是根据传感器之间的距离初始化的。为了公平比较实验，在HA和VAR以外的所有基线模型中，每个基线的输入特征包括历史主要特征、历史辅助特征、一天中的时间和一周中的天。对于LR和XGBoost，我们为每个预测步骤训练一个模型。每个实验重复 5 次，报告平均性能。实验在一台配备有四个 TitanXp GPU 的机器上执行。采用三个指标来评估每个模型的性能：平均绝对误差（MAE）、平均百分比误差（MAPE）、平方根均方误差（RMSE）。



### 4.4 与现有模型的比较

与基准方法的比较结果如表1所示。在收集自多个位置且具有不同采样率的三个数据集上，我们提出的模型始终在长期和短期方面取得了最先进的性能，这证明了我们提出的研究模型的有效性。此外，基于深度学习的模型能够比传统统计方法实现更好的性能，这表明了深度学习模型的卓越能力。像DCRNN和ASTGCN这样的方法高度依赖于预定义图，这可能无法捕捉节点之间的关键依赖关系，从而导致性能较差。尽管GMAN在Horizon 12方面取得了竞争性能，但短期预测的结果与我们的方法有明显的差距。但与其他工作相比，GMAN在Horizon 12方面有明显的改善，这可能归功于变换注意力层。除了我们的模型之外，Graph Wavenet和MTGNN总体表现良好，这可能得益于它们采用自适应图来建模节点之间的关系，表明自适应图方法可以有效地利用历史交通数据中有价值但潜在的空间依赖关系。

然而，这些工作忽略了交通空间依赖关系的动态特性以及多方面特征之间的空间依赖关系。动态图构造器和动态图卷积的设计有助于我们的模型捕捉节点之间更微妙的相关性。此外，作为包含一天时间信息的另一种方式，我们的方法与直接将其作为输入的方法相比具有更好的性能。多方面融合模块通过辅助特征与主要特征之间的交互也在此匹配中起到了很大的作用。考虑到这两个因素使得我们的模型在一致性上表现更好。

![image-20240701190734556](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202407011907693.png)

### 4.5 评估提出模型中关键设计的有效性

我们提出的模型的两个关键设计是动态图构造器，用于捕获节点随时间变化的空间依赖性，以及多面融合模块，用于从图的视角将辅助特征与主要特征相结合。为了验证我们提出的组件的有效性，我们设计了三个变体：不带多面融合模块的DMSTGCN（DMSTGCN w/o multi-faceted fusion）、不带辅助部分的DMSTGCN（DMSTGCN w/o auxiliary part）以及不带动态图的DMSTGCN（DMSTGCN w/o dynamic graph）：

• DMSTGCN w/o multi-faceted fusion：在这个变体中，我们没有使用基于图的融合模块，而是直接将辅助隐藏状态和主要隐藏状态相加，以展示我们提出的模块的有效性。

• DMSTGCN w/o auxiliary part：在这个变体中，我们完全移除了辅助模块，以展示辅助信息的重要性。

• DMSTGCN w/o dynamic graph：在这个变体中，我们将时间槽的数量设置为1，只生成一个静态自适应图，以展示动态图构造器的有效性。

消融研究的结果如图5所示，从中可以看出，关键设计都对所提出模型的改进有所贡献。与不带多面融合模块的DMSTGCN相比，DMSTGCN取得了更好的性能，这表明主要特征受到多个邻居辅助特征的影响，因此在动态图上进行聚合可以更好地支持主要特征的预测。与不带辅助部分的DMSTGCN相比，不带多面融合模块的DMSTGCN表现更好，这显示了辅助特征的重要性。DMSTGCN相对于不带动态图的DMSTGCN的优势表明，建模节点之间动态空间依赖性的重要性。

![image-20240701191059208](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202407011910321.png)

### 4.6 动态图研究

我们学习的动态图也可以用于发现节点之间的依赖关系，这将有利于诸如交通控制等应用。它是否能够发现合理的交通模式也是判断自适应图是否训练良好的一个标准。我们在PeMSD4数据集上进行了一个案例研究。对于同类节点之间的相关性，图6(a)展示了两个路段的历史平均交通速度，而图6(b)则展示了经过平滑和归一化后，我们的模型学习到的所有时间槽的边权重。在第一时段，两个路段的速度随机波动，并且学习到的动态图中的边权重较低。在第二时段，两个路段的趋势相似，其中一个领先于另一个，这有助于预测后者的速度。在这个时段，学习到的动态图中的边权重较高。在第三时段，由于晚高峰，一个路段的速度下降，而另一个路段的速度变化不大，学习到的动态图中的边权重较低。

对于辅助特征与主要特征之间的依赖关系，图6(c)展示了一个传感器的历史平均交通速度和另一个传感器的历史平均交通量。在第一时段和第二时段，交通量总是先于交通速度发生变化，这显示了它们之间的高度相关性。这种交通量的特性可以用来预测即将发生的交通速度变化，而这种变化是很难仅从历史交通速度数据中挖掘出来的。在第三时段，交通量迅速下降，而交通速度回升并随机波动，显示它们之间的相关性较低。图6(d)显示了学习到的动态图中的边权重，第一时段和第二时段权重较高，第三时段权重较低，这验证了我们在现实世界中的观察。总之，我们提出的动态图构造器能够有效地挖掘路段之间以及多方面特征之间的动态空间依赖关系。

![image-20240701191706494](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202407011917661.png)

### 4.7 利用未对齐辅助信息的研究

与直接将辅助特征和同一路段的主要特征进行拼接相比，我们的方法能够在这些特征未对齐的情况下将辅助特征整合到主要特征中。这种特性在现实世界中非常有用，比如使用周边路段的交通量来预测商场的客流量。为了探索利用未对齐辅助特征的能力，我们在PeMSD4数据集上设计了一组实验，通过改变辅助信息的比例来检验模型性能。具体来说，输入包含所有路段的主要特征以及不同比例（包括0%、25%、50%、75%和100%）的路段的辅助特征。如图7所示，随着更多辅助特征的加入，性能变得更好。这表明我们的方法在处理辅助信息与主要信息未对齐的场景下是有效的，并且总是能够利用相关的辅助信息来提高主要特征的预测性能。

![image-20240701191833912](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202407011918003.png)

> 添加的辅助信息越多，我们的模型就能达到越高的性能。



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
