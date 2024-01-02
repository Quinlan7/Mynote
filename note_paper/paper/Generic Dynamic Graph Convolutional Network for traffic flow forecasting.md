# Generic Dynamic Graph Convolutional Network for traffic flow forecasting

通用动态图卷积网络用于交通流量预测。

## 摘要

*In the field of traffic forecasting, methods based on Graph Convolutional Network (GCN) are emerging. But existing methods still have limitations due to insufficient sharing patterns, inflexible temporal relations and static relation assumptions. To address these issues, a Generic Dynamic Graph Convolutional Network (GDGCN) for traffic flow forecasting is proposed. A generic framework with both parameter-sharing and independent blocks across stacked layers is proposed to explore parameter sharing systematically in all data dimensions, which can exploit distinct patterns from layer to layer and stable patterns across layers simultaneously.*

*Then, we design a novel c graph convolutional block to view historical time slots as nodes in graph perspective and handle temporal dynamics with graph convolution. This temporal convolutional block can capture flexible and global temporal relations to have a better understanding of current traffic conditions.*

*Lastly, a dynamic graph constructor is proposed to model not only the time-specific spatial dependencies between nodes, but also the changing temporal interactions between time slots to discover dynamic relations from data thoroughly. Experimental results on four real-world datasets show that GDGCN not only outperforms state-of-the-art methods, but also obtains interpretable dynamic spatial relations of segments. Codes are available at https://anonymous.4open.science/r/GDGCN.*

在交通预测领域，基于图卷积网络（GCN）的方法正在崭露头角。但由于共享模式不足、时间关系不灵活以及对静态关系的假设，现有方法仍然存在一些局限性。为了解决这些问题，提出了一种用于交通流量预测的通用动态图卷积网络（GDGCN）。该通用框架具有跨叠加层的参数共享和独立块，以系统地在所有数据维度中探索参数共享，从而能够同时利用层与层之间的不同模式和层内的稳定模式。

接着，我们设计了一种新颖的图卷积块，将历史时间段视为图中的节点，并使用图卷积处理时间动态性。这种时空卷积块可以捕捉灵活而全局的时间关系，更好地理解当前交通状况。

最后，提出了一个动态图构造器，用于不仅建模节点之间特定时间点上的空间依赖关系，而且发现数据中时间间隔之间不断变化的时态交互关系。在四个真实世界数据集上的实验结果表明，GDGCN不仅优于最先进的方法，而且获得了可解释的动态空间关系信息。代码可在https://anonymous.4open.science/r/GDGCN 获取。



## 一、介绍

*Spatial–temporal data mining especially spatial–temporal traffic data mining has attracted extensive attention in industry and academics because: (1) spatial–temporal traffic data mining acts as a basic research problem in the field of traffic systems (2) the researched data form of traffic data mining is representative so the research on it can be easily transferred to other spatial–temporal learning problems like human mobility modeling [1], points of interest recommendation [2].Many attempts have been taken on this subject of spatial–temporal traffic data mining. Early-stage methods are empirical statistical methods [12], which ignore the spatial dependencies between nodes. Then, deep learning methods [13,14] come, where Convolutional Neural Network (CNN) and Recurrent Neural Network (RNN) are introduced.The former is used for mining relations in space and the latter can discover patterns in time. But CNN-based methods fail to work when the traffic graph is constructed in non-Euclidean space. Then came the Graph Convolutional Network (GCN), which has been effectively used in traffic forecasting. At first, researchers construct spatial graphs based on distance or functional similarity [3,15], which requires expert knowledge and may be incomplete. Later, some researchers [5,7] propose to construct adaptive spatial adjacency matrices from data.The rationale of these methods is to use cross-layer sharing parameters to construct a matrix and update it through gradient descent. Despite the fact that GCN has been proven to be beneficial in spatial–temporal data mining, there are still three issues that have not been thoroughly addressed.*

空间-时间数据挖掘，尤其是空间-时间交通数据挖掘，在工业界和学术界引起了广泛关注，原因如下：（1）空间-时间交通数据挖掘在交通系统领域是一个基础性的研究问题；（2）交通数据挖掘的研究数据形式具有代表性，因此可以轻松地转移到其他空间-时间学习问题，如人类移动建模[1]和兴趣点推荐[2]。在这一领域，已经进行了许多尝试。早期的方法是经验统计方法[12]，忽略了节点之间的空间依赖关系。然后，引入了深度学习方法[13,14]，其中引入了卷积神经网络（CNN）和循环神经网络（RNN）。前者用于挖掘空间关系，而后者可以发现时间模式。但是，当交通图在非欧几里德空间中构建时，基于CNN的方法无法正常工作。然后出现了图卷积网络（GCN），在交通预测中得到了有效应用。首先，研究人员基于距离或功能相似性[3,15]构建空间图，这需要专业知识且可能不完整。后来，一些研究人员[5,7]提出从数据中构建自适应空间邻接矩阵。这些方法的基本原理是使用跨层共享参数来构建矩阵，并通过梯度下降进行更新。尽管GCN在空间-时间数据挖掘中已被证明是有效的，但仍然存在三个尚未得到彻底解决的问题。

*First, former methods manage to exploit latent spatial relations automatically [5,7,10,16], but the layer-wise spatial relations and the stable patterns in the temporal dimension and feature dimension have not been well explored as shown in Table 1. For spatial dimension, current graph convolutional methods generally stack graph convolutional layers with only one relation matrix, which tends to smooth nodes’ input signals and makes it difficult to obtain hidden differences between layers. For example, neighbor nodes may have tighter relations in shallow layers due to the diffusion of sudden traffic conditions, while nodes with similar function may be more related in deep layers due to intrinsic characteristics. Furthermore, for temporal dimension and feature dimension, existing methods assign independent parameters for different layers as shown in Table 1. However, these methods ignore some patterns shared across layers existing in these dimensions. For instance, the future traffic flow may depend more on the near past than the far past, which is useful in multiple layers.*

首先，先前的方法成功地自动利用了潜在的空间关系[5,7,10,16]，但层次空间关系以及在时间维度和特征维度上的稳定模式尚未得到很好的探索，如表1所示。对于空间维度，当前的图卷积方法通常堆叠具有单一关系矩阵的图卷积层，这倾向于平滑节点的输入信号，使得在层之间难以获取隐藏差异。例如，由于突发交通状况的扩散，相邻节点在浅层中可能具有更紧密的关系，而在深层中，由于内在特性，具有相似功能的节点可能更相关。此外，在时间维度和特征维度上，现有方法在表1中显示为为不同层分配独立参数。然而，这些方法忽略了在这些维度中存在于层之间的一些共享模式。例如，未来的交通流可能更依赖于近期而不是远期，这在多个层次中是有用的。

*Second, existing spatial–temporal models may be insufficient to understand temporal dynamics in historical flow data. They mainly leverage CNN or RNN as sequential learning modules [5,14–16]. The function of these modules can be viewed as establishing a temporal graph to describe temporal relations between time slots. However, these methods are built on assumptions that temporal relations between time slots are local. The temporal relations between different time slots they can model is only a limited set of the whole time space in graph view. They may fail to capture flexible and global temporal relations across the entire time of input.*

其次，现有的时空模型可能不足以理解历史流数据中的时间动态。它们主要利用CNN或RNN作为时序学习模块[5,14–16]。这些模块的功能可以被看作是建立一个时间图，描述时间槽之间的时间关系。然而，这些方法建立在一种假设上，即时间槽之间的时间关系是局部的。它们能够建模的不同时间槽之间的时间关系仅仅是图视图中整个时间空间的有限集。它们可能无法捕捉整个输入时间的灵活和全局的时间关系。

*Third, both the spatial relations of two segments and the temporal relations of two-time slots should be dynamic over time. As Fig. 1 shows, the graphs of spatial relations between road segments at different time (𝑡1 and 𝑡2 ) change a lot; in the morning peak, road 1 and road 2 perform differently while they perform similarly in the evening peak.*

*Besides, the temporal relations between time slots are also changing dynamically, which is necessary to consider but has rarely been discussed by previous research. For example, in peak hours, the relations between near-time slots may be greater as the traffic conditions change swiftly; while in off-peak hours, the relations between all time slots may be even to avoid the influence of noises*

第三，两个路段之间的空间关系和两个时间槽之间的时间关系都应该随时间而动态变化。如图1所示，不同时间（𝑡1和𝑡2）的道路段之间的空间关系图变化很大；在早高峰时段，道路1和道路2表现不同，而在晚高峰时段它们表现相似。

此外，不同时间槽之间的时间关系也是动态变化的，这是需要考虑的，但在先前的研究中很少被讨论到。例如，在高峰时段，由于交通状况迅速变化，相邻时间槽之间的关系可能更加紧密；而在非高峰时段，为了避免噪音的影响，所有时间槽之间的关系可能更加均匀。

*To cope with the aforementioned issues, we present a Generic and Dynamic GCN model (GDGCN) to solve the problem of traffic flow forecasting. Previous methods fail to capture multiple spatial patterns in different layers and stable patterns across layers. To solve this, a generic and systematic framework is proposed that extracts various and stable patterns by combining both parameter-sharing and independent blocks across layers in all dimensions (spatial, temporal, feature). Moreover, to learn flexible and global temporal relations between time slots and better understand temporal dynamics in historical data, we view time steps as nodes in graph view, generalize the spatial relation learning module to the temporal domain and design a temporal dynamic graph convolutional block. Furthermore, c. This innovative approach accounts for the periodicity and dynamic attributes of traffic. GDGCN can be summarized with the following key contributions:*

为了解决上述问题，我们提出了一个通用且动态的图卷积网络模型（GDGCN）来解决交通流预测问题。先前的方法未能捕捉不同层次的多个空间模式以及层间的稳定模式。为了解决这个问题，我们提出了一个通用而系统的框架，通过在所有维度（空间、时间、特征）上结合参数共享和独立块，提取各种稳定模式。此外，为了学习时间段之间的灵活且全局的时间关系，并更好地理解历史数据中的时间动态，我们将时间步视为图中的节点，将空间关系学习模块推广到时间领域，并设计了一个时间动态图卷积块。此外，为了捕捉不断变化的段之间的空间连接和时间段之间的时间连接，我们开发了一个受张量分解影响的图构建器。这种创新性的方法考虑了交通的周期性和动态属性。GDGCN的关键贡献可以总结如下：

+ A generic and systematic prediction framework is proposed to model both parameter-sharing and independent relationships across layers in spatial, temporal, and feature dimensions respectively.We summarize existing spatio-temporal methods from the view of parameter sharing in Table 1 and show that previous GCNbased forecasting models can be viewed as special cases of our framework. To our understanding, this article represents the initial exploration of the parameter-sharing mechanism in traffic forecasting

+ A novel temporal graph convolutional block is designed. This block views time slots as nodes in graph perspective and establishes flexible and global temporal relations between them. The block can breakthrough the limits of previous CNN-based or RNNbased methods which can only model local or inflexible temporal relations.

+ A dynamic graph learning module is introduced. In contrast to the prevailing approaches that rely on fixed or predetermined spatial connections, this module effectively captures not only the changing temporal relations among time slots but also the dynamic spatial interdependencies between segments.

+ Comprehensive evaluations are performed on four benchmark datasets, showing that GDGCN consistently outperforms other approaches in all instances.

+ 提出了一个通用而系统的预测框架，用于分别模拟在空间、时间和特征维度上层之间的参数共享和独立关系。我们从参数共享的角度总结了现有的时空方法，将其列在表格1中，并表明先前基于GCN的预测模型可以看作是我们框架的特例。据我们了解，本文代表了在交通预测中参数共享机制的初步探索。

+ 设计了一种新颖的时间图卷积块。该块将时间槽视为图中的节点，并在它们之间建立灵活且全局的时间关系。该块能突破先前基于CNN或RNN方法的局部或不灵活时态关系建模的限制。

+ 引入了一个动态图学习模块。与依赖固定或预定的空间连接的主流方法相比，该模块有效捕捉不仅是时间槽之间变化的时间关系，还有段之间动态的空间相互依赖关系。

+ 在四个基准数据集上进行了全面的评估，结果显示在所有情况下，GDGCN始终优于其他方法。



## 四、方法

*Fig. 2 displays the structure of GDGCN from a macro perspective.GDGCN is comprised of multiple identical layers. Each layer contains two parts: a sharing part for capturing sharing patterns and an independent part for layerwise patterns. Three blocks in each part process hidden states in spatial, temporal, and feature dimensions respectively.Among the blocks, the spatial graph convolutional block views segments as nodes, and the temporal graph convolutional block views time slots as nodes. Both of them leverage the proposed dynamic graph constructor to establish adaptive and dynamic node relations.*

图2从宏观角度展示了GDGCN的结构。GDGCN由多个相同的层组成。每个层包含两个部分：一个用于捕捉共享模式的共享部分，以及一个用于层内模式的独立部分。每个部分中的三个块分别处理空间、时间和特征维度的隐藏状态。在这些块中，空间图卷积块将路段视为节点，而时间图卷积块将时间槽视为节点。它们都利用了提出的动态图构造器来建立自适应和动态的节点关系。



### 4.1 交通预测的整体框架

*Table 1 reveals that the layer-wise spatial relations and the stable patterns in temporal dimension and feature dimension have not been well explored by previous studies. To address these issues, this paper proposes a brief, extensible and generic framework to systematically handle shared across layers and independent patterns for traffic flow forecasting. To be specific, this framework is comprised of 𝐿 identical layers. Each GDGCN layer contains a sharing part and an independent part. The blocks in the sharing part of different layers share the same set of parameters, which aims to learn cross-layer stable and common patterns, and the blocks in the independent part of different layers keep different sets of parameters for distinct patterns across layers.*

*Moreover, there are three different blocks in both the sharing part and independent part, which process hidden states in spatial, temporal and feature dimensions respectively. After six blocks in a layer, a fusion block is employed to integrate their output and keeps the size of the hidden states consistent.*

*表1显示，以往的研究未能充分探索逐层的空间关系以及在时间维度和特征维度上的稳定模式。为解决这些问题，本文提出了一个简洁、可扩展和通用的框架，系统地处理交通流预测中的跨层共享和独立模式。具体而言，该框架由 𝐿 个相同的层组成。每个GDGCN层包含一个共享部分和一个独立部分。不同层共享部分中的块共享相同的参数集，旨在学习跨层的稳定和共同模式，而不同层独立部分中的块保留不同的参数集，用于处理层间不同的模式。*

*此外，共享部分和独立部分均包含三个不同的块，分别处理空间、时间和特征维度的隐藏状态。在每个层的六个块之后，使用融合块来整合它们的输出并保持隐藏状态大小的一致性。*



### 4.2 动态图构造器

*In the traffic domain, spatial relations between segments are latent and changing constantly. Thus, it is important to construct a proper graph to address this. In the majority of prior work, researchers use expert knowledge, such as distance, to determine spatial relations. These relations are explicit and interpretable, but they may be incomplete and may be unsuitable for traffic flow forecasting. Some studies propose to construct adaptive graphs but their generated graphs are static omitting that the spatial–temporal relations are evolving across time. In order to learn latent and varying relations, the dynamic graph constructor is designed. This constructor can produce dynamic graphs and optimize them during training (see Fig. 3).*

在交通领域，路段之间的空间关系是潜在且不断变化的。因此，构建一个合适的图来解决这个问题是很重要的。在大多数先前的研究中，研究人员使用专业知识，如距离，来确定空间关系。这些关系是明确和可解释的，但它们可能是不完整的，可能不适用于交通流预测。一些研究提出构建自适应图，但它们生成的图是静态的，忽略了空间-时间关系随时间的演变。为了学习潜在且变化的关系，设计了动态图构造器。这个构造器可以生成动态的图，并在训练过程中对其进行优化（见图3）。

*In this paper, the process of producing dynamic graphs is simplified twice to make it tractable. First, inspired by the periodicity, it is presumed that inputs from the same time slot on different days can share one dynamic graph. Correspondingly, it is needed to construct 𝑁𝑡 dynamic graphs and we use tensor 𝐀 ∈ R𝑁𝑡×𝑁×𝑁 . 𝐀 to represent these dynamic graphs. Given a specific time 𝑡, 𝜙(𝑡) is used to compute the corresponding index and 𝐀𝜙(𝑡) is the chosen dynamic graph for 𝑡.

Second, some substructures in dynamic relation graphs can be reused.

For instance, the traffic flow of two adjacent segments can be correlated from dawn to dusk, and people living in different areas share a similar travel pattern from residential places to workplaces when they go to work. Therefore, inspired by Tucker Decomposition [31], the adjacency tensor is reorganized. Given a specific tensor, Tucker decomposition aims to approximate it with the product of one core tensor and factor matrices for each mode*

在这篇论文中，生成动态图的过程被简化了两次以便于处理。首先，受周期性启发，假设在不同天的相同时间槽的输入可以共享一个动态图。相应地，需要构建𝑁𝑡个动态图，我们使用张量𝐀 ∈ R𝑁𝑡×𝑁×𝑁来表示这些动态图。对于特定的时间𝑡，使用𝜙(𝑡)来计算相应的索引，𝐀𝜙(𝑡)是选择的𝑡时刻的动态图。

其次，动态关系图中的一些子结构可以被重用。例如，两个相邻路段的交通流可以从黎明到黄昏存在相关性，而居住在不同地区的人在上班时从居住地到工作地的出行模式相似。因此，受Tucker分解的启发，邻接张量被重新组织。对于特定的张量，Tucker分解旨在用每个模式的核心张量和因子矩阵的乘积来近似它。

*Specifically, the adjacency tensor 𝐀 ∈ R𝑁𝑡×𝑁×𝑁 is calculated with four matrices 𝐄 𝑡 ∈ R𝑁𝑡×𝑑 ′ , 𝐄 𝑠 ∈ R𝑁×𝑑 ′ , 𝐄 𝑒 ∈ R𝑁×𝑑 ′ , 𝐄 𝑘 ∈ R𝑑 ′×𝑑 ′×𝑑 ′ that can be updated automatically: 𝐀 = 𝑆𝑜𝑓 𝑡𝑚𝑎𝑥(𝑅𝑒𝐿𝑈(𝐄 𝑘 ×1 𝐄 𝑡 ×2 𝐄 𝑠 ×3 𝐄 𝑒 )), (7) where ×𝑖 is the tensor-matrix multiplication on 𝑖th dimension. 𝐄 𝑡 ∈ R𝑁𝑡×𝑑 ′ represents 𝑁𝑡 time slots. 𝐄 𝑠 ∈ R𝑁×𝑑 ′ denotes the information of start nodes. 𝐄 𝑒 ∈ R𝑁×𝑑 ′ is the parameters for end nodes. Besides establishing the spatial relation 𝐀𝑆, temporal relations between input time slots can also be established as dynamic graphs 𝐀𝑇 ∈ R𝑁𝑡×𝑃×𝑃 , which will be detailed later.*

具体来说，邻接张量 𝐀 ∈ R𝑁𝑡×𝑁×𝑁 是由四个矩阵 𝐄 𝑡 ∈ R𝑁𝑡×𝑑 ′ ，𝐄 𝑠 ∈ R𝑁×𝑑 ′ ，𝐄 𝑒 ∈ R𝑁×𝑑 ′ ，𝐄 𝑘 ∈ R𝑑 ′×𝑑 ′×𝑑 ′ 计算而得，这些矩阵可以自动更新：𝐀 = 𝑆𝑜𝑓 𝑡𝑚𝑎𝑥(𝑅𝑒𝐿𝑈(𝐄 𝑘 ×1 𝐄 𝑡 ×2 𝐄 𝑠 ×3 𝐄 𝑒))，其中 ×𝑖 表示在𝑖维上的张量-矩阵乘法。𝐄 𝑡 ∈ R𝑁𝑡×𝑑 ′ 表示 𝑁𝑡 个时间槽。𝐄 𝑠 ∈ R𝑁×𝑑 ′ 表示起始节点的信息。𝐄 𝑒 ∈ R𝑁×𝑑 ′ 是结束节点的参数。除了建立空间关系 𝐀𝑆，输入时间槽之间的时序关系也可以建立为动态图 𝐀𝑇 ∈ R𝑁𝑡×𝑃×𝑃，这将在后面详细介绍。



### 4.3 空间图卷积块

*Spatial relations between spatial traffic nodes benefit forecasting.For example, taking the traffic flow of upstream segments into consideration is helpful for the flow forecasting of downstream segments.Though the convolution operation on graphs extracts local information to handle the relations between segments in space, the majority utilize it without the consideration of evolving characteristics across time.*

Here, we aim to model time-varying relationships in space.Given input 𝐗𝑡−𝑃+1∶𝑡 , 𝐀𝜙(𝑡) ∈ R𝑁×𝑁 is the relation matrix in space. The aim of spatial graph convolutional block is to aggregate information to the focal node according to 𝐀𝜙(𝑡) ; the (𝑖, 𝑗)-entry of 𝐀𝜙(𝑡) reflects node 𝑗’s impact strength on node 𝑖. The spatial convolution on the dynamic graph in the independent part of layer 𝑙 can be defined as:*

空间交通节点之间的空间关系有助于预测。例如，考虑上游段的流量对下游段的流量预测是有帮助的。尽管图上的卷积操作提取本地信息以处理空间段之间的关系，但大多数方法在处理跨越时间的演变特性时未加考虑。

在这里，我们的目标是模拟时变的空间关系。

给定输入 𝐗𝑡−𝑃+1∶𝑡 ，𝐀𝜙(𝑡) ∈ R𝑁×𝑁 是时空关系矩阵。空间图卷积块的目标是根据 𝐀𝜙(𝑡) 将信息聚合到焦点节点；𝐀𝜙(𝑡) 的（𝑖, 𝑗）条目反映了节点 𝑗 对节点 𝑖 的影响强度。第 𝑙 层的独立部分中动态图的空间卷积可以定义为：

*This module in this framework is different from traditional graph convolution. First, spatial graph convolution is only applied one step on the dynamic graph. The rationale is that the graph is learnable.

The learnable graph can adaptively consider multi-hop relations in traditional graph convolution. Meanwhile, feature transformation is absent and the transformation on the feature dimension is placed in another module. However, this does not make it less expressive since traditional GCN can be viewed as the combination of these two modules and achieved by stacking multiple layers.*

这个框架中的这个模块与传统的图卷积不同。首先，空间图卷积仅在动态图上应用一步。其原理是图是可学习的。

可学习的图可以自适应地考虑传统图卷积中的多跳关系。同时，特征转换是缺失的，特征维度上的转换被放置在另一个模块中。然而，这并不使其表达能力减弱，因为传统的 GCN 可以被视为这两个模块的组合，并通过堆叠多个层来实现。



### 4.4 时间图卷积块

*Understanding input historical data is critical in traffic flow predictions. And the key is to handle the temporal dynamics of historical traffic flow, i.e. exploiting temporal relations between historical time slots. The most prevalent methods to do this are CNN and RNN.

However, they have the following drawbacks: First, these methods tend to model local temporal relations between different time slots as shown in Fig. 4. Second, the temporal relations modeled by these methods are basically static, as it maintains the same set of parameters at different time.

This work adopts a completely different routine, where the spatial relation learning module is generalized to the temporal domain and proposes a temporal graph convolutional block.

Formally, as shown in Fig. 2, 𝑃 time slots of input can be viewed as 𝑃 nodes in a graph, and temporal relation dynamic graphs 𝐀𝑇 ∈ R𝑁𝑡×𝑃×𝑃 is built by the dynamic graph constructor. Similar to the spatial graph convolution module, the temporal graph convolution in the independent part of layer 𝑙 can be defined as:*

理解输入的历史数据在交通流预测中至关重要。关键在于==处理历史交通流的时间动态==，即利用历史时间段之间的时间关系。最常见的方法是使用 CNN 和 RNN。

然而，它们具有以下缺点：首先，这些方法倾向于建模不同时间槽之间的局部时间关系，如图4所示。其次，这些方法建模的时间关系基本上是静态的，因为它在不同时间保持相同的参数集。

本文采用了一种完全不同的方法，其中将空间关系学习模块推广到了时间领域，并提出了一个时间图卷积块。

形式上，如图2所示，输入的 P 个时间槽可以看作是图中的 P 个节点，而时间关系动态图 A_T ∈ R𝑁𝑡×𝑃×𝑃 由动态图构造器构建。类似于独立层的空间图卷积模块，第 𝑙 层的时间图卷积可以定义为：



![image-20231123092624145](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311230926275.png)