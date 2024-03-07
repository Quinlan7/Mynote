# AARGNN: An Attentive Attributed Recurrent Graph Neural Network for Traffic Flow Prediction Considering Multiple Dynamic Factors

AARGNN：考虑多个动态因素的交通流量预测的注意力属性循环图神经网络

## 摘要

*Abstract— Traffic flow prediction is a fundamental part of ITS (Intelligent Transportation System). Since the correlations of traffic data are complicated and are affected by various factors, traffic flow prediction is a challenging task. Existing traffic flow prediction methods generally take limited static factors (e.g., the distance between sensors and road network topological structure) into consideration and model the correlations of the traffic data separately to predict the future traffic. In this paper, we propose AARGNN (Attentive Attributed Recurrent Graph Neural Network), a GNN (graph neural network) based method considering multiple dynamic factors to predict short-term traffic flow. With multi-source urban data (e.g., POI, road network, incident, weather, etc.), AARGNN considers both static factors and dynamic factors (e.g., spatial distance, semantic distance, road characteristic, road situation, and global context) to predict the short-term traffic flow. Specifically, AARGNN constructs an attributed graph and encodes various factors into the attributes.The correlations of the traffic data are modeled by utilizing the GNN combined with LSTM (long short-term memory). In addition, AARGNN specifies the contributions of each factor based on attention mechanism. Experiments on real-world datasets show that the proposed method outperforms all baseline methods.*

摘要— 交通流量预测是智能交通系统（ITS）的基本组成部分。由于交通数据的相关性复杂，并受到各种因素的影响，交通流量预测是一项具有挑战性的任务。现有的交通流量预测方法通常仅考虑有限的静态因素（例如传感器之间的距离和道路网络拓扑结构），并单独对交通数据的相关性进行建模以预测未来的交通流量。在本文中，我们提出了基于图神经网络（GNN）的AARGNN（Attentive Attributed Recurrent Graph Neural Network）方法，考虑了多个动态因素来预测短期交通流量。利用==多源城市数据（例如POI、道路网络、事件、天气等）==，AARGNN考虑了==静态因素和动态因素（例如空间距离、语义距离、道路特征、道路情况和全局上下文）来预测短期交通流量==。具体而言，AARGNN构建了一个带有属性的图，并将各种因素编码为属性。交通数据的相关性通过利用GNN与LSTM（长短期记忆）相结合来建模。此外，AARGNN基于注意力机制指定了每个因素的贡献。对真实世界数据集的实验表明，所提出的方法优于所有基线方法。



## 一、介绍

~~*TRAFFIC flow prediction, especially short-term traffic flow prediction, has been a focus issue in ITS. The task of traffic flow prediction is to predict the future traffic flow in the road network by leveraging history traffic data and related urban data. Accurate short-term traffic flow prediction not only facilitates people’s daily commuting, but also provides great help to governments in transportation management [1].*~~

~~交通流量预测，特别是短期交通流量预测，一直是智能交通系统（ITS）中的焦点问题。交通流量预测的任务是通过利用历史交通数据和相关城市数据，预测道路网络中未来的交通流量。准确的短期交通流量预测不仅有助于人们的日常通勤，还为政府在交通管理方面提供了极大的帮助。~~

~~*There have been a lot of efforts made in the traffic flow prediction, which mainly fall into two categories: the traditional methods and the recent deep-learning based methods.*~~

~~*The traffic data are correlated in temporal, spatial, and semantic aspects. Based on the correlations modeled in prediction process, the traditional methods could be divided into univariate time series learning methods and multivariate time series learning methods. Univariate time series learning methods, e.g., non-parameter regression-based methods [2], Kalman filtering-based methods [3], and ARIMA (auto regressive integrated moving average)-based methods [4]–[6], mainly focus on the temporal correlations of the traffic data from a single sensor. These methods can capture the temporal patterns but ignore the spatial and semantic correlations from multiple sensors. Multivariate time series learning methods, e.g., KNN (k-nearest neighbor)-based methods [7], Bayesian networkbased methods [8], STARIMA (spatiotemporal autoregressive integrated moving average)-based methods [9], SVM (support vector machi nes)-based methods [10], [11], and ANN (artificial neural networks)-based methods [12], [13], consider the spatial/semantic correlations of the traffic data from multiple sensors. However, these methods usually utilize the traffic data from sensors within a fixed range, which is either too small to incorporate necessary information, or too large that leads to high computation complexity and overfitting.*~~

~~在交通流量预测方面已经进行了大量的工作，主要分为两类：传统方法和最近基于深度学习的方法。~~

~~交通数据在时间、空间和语义方面存在相关性。基于预测过程中建模的相关性，传统方法可以分为单变量时间序列学习方法和多变量时间序列学习方法。单变量时间序列学习方法，例如非参数回归方法[2]、卡尔曼滤波方法[3]和ARIMA（自回归积分移动平均）方法[4]–[6]，主要关注来自单个传感器的交通数据的时间相关性。这些方法可以捕捉时间模式，但忽略了来自多个传感器的空间和语义相关性。多变量时间序列学习方法，例如基于KNN（k最近邻）的方法[7]、基于贝叶斯网络的方法[8]、基于STARIMA（时空自回归积分移动平均）的方法[9]、基于SVM（支持向量机）的方法[10]、[11]以及基于ANN（人工神经网络）的方法[12]，[13]，考虑了来自多个传感器的交通数据的空间/语义相关性。然而，这些方法通常利用了范围内传感器的交通数据，这个范围可能太小以包含必要的信息，或者太大以致于导致高计算复杂性和过拟合。~~

~~*The recent deep-learning based methods for traffic flow prediction can model the correlations of the traffic data from multiple sensors and expand the range of related sensors along with iterations. CNN (convolutional neural network)based methods [14]–[16] represent the traffic data from multiple sensors as an image to capture the spatial correlations of the traffic data. However, traffic sensors are not regularly deployed, and thus GNN (graph neural network)-based methods [18]–[35] represent the traffic data from multiple sensors as a graph and the correlations are modelled as edges in the graph. Previous GNN-based methods usually construct a graph with fixed structure and consider only static factors. However, the correlations of the traffic data are affected by not only static factors but also dynamic factors.*~~

~~最近基于深度学习的交通流量预测方法能够建模来自多个传感器的交通数据的相关性，并随着迭代扩展相关传感器的范围。基于CNN（卷积神经网络）的方法[14]–[16]将来自多个传感器的交通数据表示为图像，以捕捉交通数据的空间相关性。然而，交通传感器的部署并不规则，因此基于GNN（图神经网络）的方法[18]–[35]将来自多个传感器的交通数据表示为图，并将相关性建模为图中的边。先前基于GNN的方法通常构建具有固定结构的图，并且仅考虑静态因素。然而，交通数据的相关性不仅受到静态因素的影响，还受到动态因素的影响。~~

*Recently, spatial-based GNN [36] is proposed. Spatial-based GNN can support flexible representations of the attributes of the graph. Thus, they can handle graph-structured data with dynamic node, edge, and global attributes.*

最近，==提出了基于空间的GNN [36]。基于空间的GNN可以支持对图的属性进行灵活的表示。因此，它们可以处理具有动态节点、边和全局属性的图结构数据。==

In this paper, we propose AARGNN (attentive attributed recurrent graph neural network), a GNN-based method to predict short-term traffic flow. AARGNN takes both static factors (e.g., spatial distance, semantic distance, road characteristic) and dynamic factors (e.g., road situation, and global context) into consideration when modeling the correlations of the traffic data, and quantifies the contributions of each factor based on the attention mechanism. Our contributions are summarized as follows: 

+  We identify factors that affect the correlations of the traffic data based on multi-source urban data and construct an attributed graph for the traffic data to consider both static and dynamic factors.

+ We propose a hybrid deep neural network to model the temporal, spatial, and semantic correlations of the traffic data for traffic flow prediction. We encode various factors into the attributes of the graph and combine GNN with LSTM to jointly model the correlations of the traffic data. In addition, we utilize the attention mechanism to quantify the contributions of each factor.

+ We evaluate AARGNN on real-world traffic datasets and make comparisons with other traffic flow prediction methods.

The results show that the proposed method is superior to the compared methods.

在本文中，我们提出了AARGNN（注意力属性循环图神经网络），这是一种基于GNN的方法，用于预测短期交通流量。AARGNN在建模交通数据的相关性时，考虑了静态因素（例如空间距离、语义距离、道路特征）和动态因素（例如道路情况和全局背景），并基于注意力机制量化了每个因素的贡献。我们的贡献总结如下：

1. 我们==根据多源城市数据确定影响交通数据相关性的因==素，并==构建了一个用于考虑静态和动态因素的交通数据的属性图==。

2. 我们提出了一个==混合深度神经网络，用于建模交通数据的时间、空间和语义相关性==，以进行交通流量预测。我们将==各种因素编码为图的属性==，并==将GNN与LSTM结合起来共同建模交通数据的相关性==。此外，我们利用注意力机制量化每个因素的贡献。

3. 我们在真实交通数据集上评估了AARGNN，并与其他交通流量预测方法进行了比较。

结果表明，所提出的方法优于其他方法。



### 三、方法

### A.准备工作

*In this section, we firstly give the definitions of the basic concepts and terms, and then formulate the problem.*

在本节中，我们首先给出基本概念和术语的定义，然后阐述问题的形式化描述。

+ *Notation: In this paper, upper-case letters denote sets and lower-case letters denote variables. Bold upper-case letters denote matrices and bold lower-case letters denote vectors.[] denotes a sequence, {} denotes a set, and <> denotes a vector. || denotes the operation of vector concatenation*

+ 符号表示：在本文中，大写字母表示集合，小写字母表示变量。粗体大写字母表示矩阵，粗体小写字母表示向量。[] 表示序列，{} 表示集合，<> 表示向量。|| 表示向量连接操作。

+ *Definition 1 (Graph-Structured Traffic Data): The traffic data in the road network can be structured as an attributed graph G = (N, E, V, A, u), where N is the set of nodes corresponding to the sensors deployed in the road network, E is the set of edges corresponding to the correlations of different sensors, V represents the attributes of the nodes, A represents the attributes of the edges, and u represents the global attributes of the graph.*

  *The node attributes consist of the traffic data and other node level factors. The edge attributes represent the correlations between two sensors, which are affected by both static factors and dynamic factors. The global attributes are features shared by all the nodes in the graph.*

+ )定义 1（图结构的交通数据）：道路网络中的交通数据可以被结构化为一个属性图 G = (N, E, V, A, u)，其中 N 是节点集合，对应于部署在道路网络中的传感器，E 是边的集合，对应于不同传感器之间的相关性，V 表示节点的属性，A 表示边的属性，u 表示图的全局属性。

  节点属性包括交通数据和其他节点级别因素。边的属性表示两个传感器之间的相关性，受静态因素和动态因素的影响。全局属性是图中所有节点共享的特征。

+ *Problem Formulation: Let $G^t$ = (N, E, $V^t$ , $A^t$ , $u^t$ ) denote the graph-structured traffic data at time slot t, where Vt , At , and ut represent different attributes at time slot t, Y t represents the traffic flow at time slot t. Then, the short-term traffic flow prediction problem can be formulated as: given τ time slots of the graph-structured traffic data [Gt−τ+1, Gt−τ+2,..., Gt ], learning a function h()that maps the input data to the future traffic flow: [Gt−τ+1, Gt−τ+2,..., Gt ] h() −→ Y t+l (1)*
+ 问题公式化：设$G^t$ = (N, E, $V^t$ , $A^t$ , $u^t$ ) 表示时间槽 t 的图结构交通数据，其中 $V^t$，$A^t$ 和$u^t$ 表示时间槽 t 的不同属性，$Y^t$ 表示时间槽 t 的交通流量。那么，短期交通流量预测问题可以形式化为：给定 $\tau$ 个时间槽的图结构交通数据 [Gt−τ+1, Gt−τ+2,..., Gt ]，学习一个函数 h()，将输入数据映射到未来的交通流量：

![image-20240303153626987](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202403031536120.png)

### B.架构

![image-20240303153830485](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202403031538575.png)

AARGNN的架构如图所示：(a) 属性图构建。(b) 交通流量预测。

*As shown in Fig. 1, AARGNN is composed of two parts: attributed graph construction and traffic flow prediction. The attributed graph construction part structures the traffic data as an attributed graph with node attributes, edge attributes, and global attributes, and encodes various factors into the graph.*

*The traffic flow prediction part utilizes the attention mechanism to quantify the contributions of each factor and models the correlations of the traffic data with a hybrid network with GNN and LSTM.*

如图1所示，==AARGNN由两部分组成：属性图构建和交通流量预测==。==属性图构建部分将交通数据结构化为具有节点属性、边属性和全局属性的属性图，并将各种因素编码到图中==。

==交通流量预测部分利用注意力机制量化每个因素的贡献，并使用GNN和LSTM的混合网络模型来建模交通数据的相关性==。

### C.属性图构建

*The urban traffic data are correlated in temporal, spatial, and semantic aspects. On one hand, the traffic data from the same sensor are temporally correlated. On the other hand, the traffic data from different sensors are spatially and semantically correlated. In order to capture the correlations of the traffic data, we structure the traffic data and related urban data as an attributed graph and encode both static and dynamic factors into the attributes of the graph, as shown in Fig. 2. Specifically, given the traffic data and related urban data, we can construct an attributed graph with node attributes, edge attributes, and global attributes for each time slot. For the attributed graph at time slot t, the nodes represent sensors in the road network and the edges represent correlations of different sensors. The node attributes represent the current attributes of the sensors.*

*The edge attributes and global attributes represent the factors that affect the correlations of the traffic data from different sensors. To simplify the model, we construct the graph as a directed graph according to the topology of the traffic system, i.e., there is an edge between two sensors if there is a road segment connecting one sensor to the other.*

城市交通数据在时间、空间和语义方面存在相关性。一方面，来自同一传感器的交通数据在时间上相关。另一方面，来自不同传感器的交通数据在空间和语义上相关。为了捕获交通数据的相关性，我们将交通数据和相关的城市数据结构化为一个属性图，并将静态和动态因素编码到图的属性中，如图2所示。具体来说，给定交通数据和相关的城市数据，我们可以为每个时间槽构建一个具有节点属性、边属性和全局属性的属性图。==对于时间槽t的属性图，节点表示道路网络中的传感器，边表示不同传感器之间的相关性。节点属性表示传感器的当前属性==。

边属性和全局属性表示影响来自不同传感器的交通数据相关性的因素。为了简化模型，我们根据交通系统的拓扑结构构建图，即==如果有一条连接一个传感器到另一个传感器的道路段，则两个传感器之间存在一条边==。

![image-20240303161220057](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202403031612133.png)

​      

+ *Node Attributes: The nodes in the graph represent the sensors in the traffic system. We aggregate the traffic flow data and the node-level spatiotemporal data (e.g., vehicle occupancy) as the node attributes. The traffic flow data are the target to predict. The vehicle occupancy data are used as dynamic factor in node attributes. Suppose there are nse nodes in the graph, then the node attributes at time slot t are defined as Vt = {vt i}i=1:nse , where vt i =< f t 1|| f t 2|| ... || f t z1 > and f t k is one of the feature vectors of the traffic data or related node-level spatiotemporal data at time slot t*

+ 节点属性：==图中的节点代表交通系统中的传感器==。我们==将交通流量数据和节点级别的时空数据（例如车辆占用率）聚合为节点属性==。==交通流量数据是要预测的目标。车辆占用率数据被用作节点属性中的动态因素==。假设图中有 $n_{se}$ 个节点，则时间槽 t 的节点属性定义为 $V^t = \{v^t_i\}i =1:n_{se}$ ，其中 $v^t_i =<f^t_1||f^t_2|| …… ||f^t_i>$，而 $f^t_k$ 是时间槽 t 的交通数据或相关节点级别时空数据的特征向量之一。

> 车辆占用率？？？？

+ *dge Attributes: The edges in the graph represent the correlations of pair-wise sensors, which may be affected by various factors. For example, if there is an incident happened on the road, the correlations of the traffic data between two sides of the road would change. To model the correlations, we construct the attributes of an edge with static factors and dynamic factors. Suppose there are ned edges in the graph, then the edge attributes at time slot t are defined as At = {at i }i=1:ned , where at i =< st 1||st 2|| ... ||st z2 > and s t k is one of the feature vector of the factors that affect traffic data correlations*

  *Previous traffic flow prediction methods ignore the impact of dynamic factors when modeling the correlations. To take both static and dynamic factors into consideration, the attributes of an edge contain two parts: the static part and the dynamic part. We extract three sets of static factors from related urban data for the static part as follows:*

+ 边属性：==图中的边表示两两传感器之间的相关性==，可能受到各种因素的影响。例如，如果道路上发生了事故，道路两侧的交通数据之间的相关性将发生变化。为了模拟这种相关性，我们构建了一个边的属性，包括静态因素和动态因素。假设图中有 ned 条边，则时间槽 t 的边属性定义为 At = {at i }i=1:ned，其中 at i =< st 1||st 2|| ... ||st z2 >，而 s t k 是影响交通数据相关性的因素之一的特征向量。

  先前的交通流量预测方法在建模相关性时忽略了动态因素的影响。为了考虑到静态和动态因素，==边的属性包含两个部分：静态部分和动态部分==。我们从相关的城市数据中提取==三组静态因素作为静态部分==，具体如下：

  + *Spatial proximity measures the sensor correlations based on geographic distances. Let dis(i, j) denote the road driving distance between two upstream sensor i and downstream sensor j, then ssp(i,j) = 1/dis(i, j) is the spatial proximity of the edge connecting i and j.*

  + 空间接近度根据地理距离衡量传感器之间的相关性。设 dis(i, j) 表示两个上游传感器 i 和下游传感器 j 之间的道路驾驶距离，==则 $s_{sp(i,j)} = 1/dis(i.j)$ 是连接 i 和 j 的边的空间接近度==。

  + Semantic similarity measures the sensor correlations from semantic aspect. POIs in the region where a sensor is deployed indicate the land use of the region, which can be utilized to calculate the semantic distance of two sensors.

    Given the location of a sensor, we collect POIs around the sensor and calculate the density distribution of the POIs of different categories. Suppose there are npoi categories of POIs, the land use of a sensor i can be formulated as a vector pi of length npoi, where each dimension denotes the density of nearby POIs of a specific category. Then, the semantic similarity of the edge connecting sensor i and j can be calculated as slu(i,j) = pi · p j .

  + 语义相似性从语义角度衡量传感器之间的相关性。==传感器部署区域的POIs表示区域的土地利用情况，可以用于计算两个传感器的语义距离==。

    给定传感器的位置，我们收集传感器周围的POIs，并计算不同类别的POIs的密度分布。==假设有 $n_{poi}$ 个类别的POIs，传感器 i 的土地利用情况可以被表示为长度为 $n_{poi}$ 的向量 $p_i$，其中每个维度表示传感器 i 附近的特定类别的 POIs 的密度==。然后，==连接传感器 i 和 j 的边的语义相似度可以计算为 $s_{lu(i,j)} = p_i \cdot p_j$==  。

  + *Road characteristics refer to the physical properties of the road between two sensors. In our method, we consider road width, the number of road lanes, and road speed limit as the road characteristic features. Let src(i,j) denote the road characteristics of the edge connecting sensor i to sensor j, then src(i,j) =< r1||r2|| ... ||rz3 > and rk is one of the road characteristic features*

  + ==道路特性指的是两个传感器之间道路的物理属性==。在我们的方法中，我们==将道路宽度、道路车道数量和道路限速视为道路特性特征==。设 $s_{rc(i,j)}$ 表示连接传感器 i 到传感器 j 的边的道路特性，则 src(i,j) =< r_1||r_2|| ... ||r_{z3} >，其中 r_k 是道路特性特征之一。

*The dynamic part contains dynamic factors, which would change over time. In our method, we utilize road situation represented by the incidents happened on the road as dynamic factors. Suppose there are ninc types of incidents, the road situation at time slot t can be formulated as st rs(i,j) ∈ Rninc , and the k-th dimension denotes the influence of the k-th type of incident on the correlations of the traffic data. Specifically, we set the value to 0 if the corresponding type of incident happens on the road, and 1 otherwise.*

==动态部分包含会随时间变化的动态因素==。在我们的方法中，我们==利用在道路上发生的事件所表示的道路状况作为动态因素==。==假设有 $n_{\text{inc}}$ 种类型的事件，时间槽 t 的道路状况可以表示为 $s_{\text{rs}}^t(i,j) \in \mathbb{R}^{n_{\text{inc}}}$，第 k 维表示第 k 种类型的事件对交通数据相关性的影响。具体地，如果相应类型的事件发生在道路上，则将值设为 0，否则设为 1。==

Since both static factors and dynamic factors can affect the correlations of the traffic data, we concatenate both types of factors as the edge attributes of the graph. The static factors remain unchanged while the dynamic factors change with time.Thus, for the edge attributes at time slot t, the edge attributes are At = {at i}i=1:ned , where at i =< ssp||slu||src||st rs > and ned is the number of edges

==由于静态因素和动态因素都可以影响交通数据的相关性，我们将两种类型的因素连接起来作为图的边属性。静态因素保持不变，而动态因素随时间变化。==

因此，==在时间槽 t 的边属性中，边属性为 $A^t = \{a^t_i\}i=1:n_{ed}$，其中 $a^t_i = < s_{sp} || s_{lu} || s_{rc} || s^t_{\text{rs}} >$，$n_{ed}$ 是边的数量。==

*Global attributes The global attributes of the graph refer to the factors shared by all sensors. Since the static factors shared by all sensors may not affect prediction results, we mainly consider the dynamic factors in the global attributes. In our method, we consider weather condition and calendar features as the global attributes, which are strongly related to the traffic data [37]. The calendar features include time of day, day of week, day of month, month of year, and day type (i.e., work day or vacation). The weather condition and current calendar features of all sensors in a city are regarded as the same. Let ut represent the global attributes of the attributed graph at time slot t, then ut =< ut wea||ut cal >, where ut wea denotes the weather condition feature and ut cal denotes the calendar features.*

全局属性指的是所有传感器共享的因素。由于所有传感器共享的静态因素可能不会影响预测结果，我们主要考虑全局属性中的动态因素。在我们的方法中，我们==将天气条件和日历特征作为全局属性==，这与交通数据密切相关。==日历特征包括时间、星期几、月份、年份和日期类型（即工作日或假日）==。一个==城市中所有传感器的天气条件和当前日历特征被视为相同==。设 $u_t$ 表示时间槽 t 的属性，则 $u_t = < u_t^{\text{wea}} || u_t^{\text{cal}} >$，其中 $u_t^{\text{wea}}$ 表示天气条件特征，$u_t^{\text{cal}}$ 表示日历特征。

### D.交通流预测

![image-20240303174847254](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202403031748339.png)

*Based on the graph-structured traffic data, the correlations of the traffic data are modeled with a hybrid network with GNN and LSTM, as shown in Fig. 3. The GNN is utilized to model the spatial and semantic correlations and the LSTM is utilized to model the temporal correlations. In addition, to quantify the contributions of various factors, we add the attention mechanism to the attributes of the graph-structured traffic data*

基于图结构化的交通数据，交通数据的相关性通过具有 GNN 和 LSTM 的混合网络进行建模，如图 3 所示。==GNN 用于建模空间和语义相关性，而 LSTM 用于建模时间相关性==。此外，==为了量化各种因素的贡献，我们将注意机制添加到图结构化的交通数据的属性中==。

*When modeling the spatial and semantic correlations, most previous methods are based on spectral-based GNN, i.e., ChebNet [42] and its extensions. These methods are efficient if the structure of the graph is fixed and a global adjacency matrix is predefined. However, if the adjacency matrix needs to be updated frequently, the convolution operation would become inefficient. Moreover, spectral-based GNN cannot handle the flexible edge attributes. Thus, we utilize spatial-based GNN to model the spatial and semantic correlations instead of spectralbased GNN. The spatial-based GNN generalizes and extends previous GNNs, e.g., message-passing neural network [43] and non-local neural network [44], which can learn the correlations of the traffic data and handle flexible edge attributes.*

在建模空间和语义相关性时，大多数先前的方法基于基于谱的 GNN，即 ChebNet [42] 及其扩展。如果图的结构是固定的，并且全局邻接矩阵是预定义的，这些方法是高效的。然而，如果邻接矩阵需要经常更新，卷积操作将变得低效。此外，基于谱的 GNN 无法处理灵活的边属性。因此，我们==利用基于空间的 GNN 来代替基于谱的 GNN 来建模空间和语义相关性==。基于空间的 GNN 泛化并扩展了以前的 GNN，例如消息传递神经网络 [43] 和非局部神经网络 [44]，它可以学习交通数据的相关性并处理灵活的边属性。

*We conduct the convolution on the graph-structured traffic data to model the spatial and semantic correlations as a “graph to graph” process. Generally, the convolution operation is processed by updating the edge attributes, followed by the node attributes, and finally the global attributes, as shown in Fig. 4. The process is implemented via updating functions φ and aggregation functions ρ.*

我们对图结构化的交通数据进行卷积操作，以将空间和语义相关性建模为一个“图到图”的过程。通常，==卷积操作是通过更新边属性，然后是节点属性，最后是全局属性来进行的==，如图 4 所示。==该过程是通过更新函数 $\phi$ 和聚合函数 ρ 实现的==。

![image-20240303175347409](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202403031753491.png)

图结构化的交通数据上的卷积操作：(a) 更新边属性。(b) 更新节点属性。(c) 更新全局属性。



*Let Gt denote the input graph-structured traffic data of the GNN at time slot t. Hereafter, we omit the superscript t for simplicity. Then, the convolution ∗g on the graph-structured traffic data G = (N, E, V, A, u) is defined as:*

让 $G^t$ 表示 GNN 在时间步 t 的输入图结构化交通数据。接下来，为了简洁起见，我们省略 t 上标。那么，在图结构化交通数据 G = (N, E, V, A, u) 上的卷积 ∗g 定义如下：

![image-20240303175859558](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202403031758833.png)

![image-20240303175907762](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202403031759826.png)

其中，$a_{i,j}$ 表示上游节点为 i，下游节点为 j 的边的属性，$v_i$ 和 $v_j$ 分别表示节点 i 和节点 j 的属性，u 表示全局属性。$A^{\prime}_{N(i)} = \{a^{\prime}_{j,i}\}_{j} = i:n_{i}$  表示与节点 i 相连的边的更新属性，$n_i$ 是与节点 i 相连的边的数量。$V^{\prime} = \{v^{\prime}_i\}_{i}=1:n_{se}$ 表示更新后的节点属性，$n_{se}$ 是节点的数量。

A' = ∪r A'r = {a'r}r=1:ned 表示所有边的输出。

==首先，通过 $\phi ^a$ 函数（方程式2）更新边。在这里，$\phi ^a$ 是一个函数，它用于映射所有边，计算每个边的更新属性。更新后的边属性 $a^{\prime}_{i,j}$ 可以看作是从上游节点到下游节点的影响。其次，通过$ρ^{a→v}$ 函数（方程式3）对每个节点来自连接节点的影响进行聚合。该函数将所有连接到某个节点的更新边属性作为输入，并将其减少为一个向量，以表示聚合信息。第三，通过 $\phi ^{v}$ 函数（方程式4）更新每个节点。该函数是聚合影响、当前节点属性和当前全局属性的函数。第四，通过$ρ^{v→u}$ 和 $ρ^{a→u}$ 函数（方程式5 和方程式6）计算表示更新后的边属性和节点属性的聚合信息的元素。最后，通过 $\phi ^ u$ 函数（方程式7）更新全局属性。该函数是当前全局属性和节点属性、边属性的聚合信息的函数。==

*To model the temporal correlations, RNNs are employed in the prediction process. LSTM is a variant of RNNs, which can prevent the gradient from vanishing or exploding problem.*

*Inspired by previous work [14], [16], [36]–[42], we combine GNN with LSTM by replacing the matrix multiplications in the LSTM cell with GNN operation to model the temporal, spatial, and semantic correlations jointly. Let ∗g denote the convolution on the graph-structured traffic data, Gt denote the input graph-structured traffic data of the LSTM at time slot t, then the LSTM model can be modified as:*

为了建模时间上的相关性，预测过程中采用了循环神经网络（RNNs）。LSTM是RNNs的一种变体，可以防止梯度消失或爆炸的问题。

受之前的工作[14]，[16]，[36]–[42]的启发，我们将GNN与LSTM结合起来，通过用GNN操作替换LSTM单元中的矩阵乘法，共同建模时间、空间和语义相关性。让 ∗g 表示对图结构的交通数据进行卷积，$G^t$ 表示LSTM在时间槽t处的输入图结构交通数据，那么LSTM模型可以修改为：

![image-20240303200206549](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202403032002629.png)

其中，$\odot$表示Hadamard积，$\sigma(\cdot)$表示sigmoid函数，i、f和o分别表示输入门、遗忘门和输出门，h是隐藏状态，c是细胞状态，W是权重。LSTM层由图中的所有节点共享。

*The correlations of the traffic data are affected by various factors. However, the contributions of each factor are not equal, and would dynamically vary with sensors and time slots.To quantify the contributions of each factor, we utilize the attention mechanism in our method.*

交通数据的相关性受到各种因素的影响。然而，每个因素的贡献并不相等，并且会随着传感器和时间段的变化而动态变化。为了量化每个因素的贡献，我们在我们的方法中利用了注意力机制。

As defined in Section III-A, the traffic data at a time slot are structured as a graph Gt = (N, E, Vt , At , ut ). With the static and dynamic factors encoded in the graph, each type of attributes consists of multiple feature vectors. Suppose there are nfa types of factors encoded in attribute vt , i.e., vt = {vt k }k=1:nfa , where vt k is the feature vector of the k-th factor at time slot t. The attention mechanism is defined as:

根据第 III-A 节的定义，某个时间段的交通数据被结构化为图 Gt = (N, E, Vt , At , ut )。随着静态和动态因素被编码进图中，每种类型的属性都包含多个特征向量。假设在属性 $v^t$ 中编码了 $n_{fa}$ 种类型的因素，即 vt = {vt k }k=1:nfa ，其中 $v^t_k$ 是时间段 t 的第 k 种因素的特征向量。注意力机制的定义如下：

![image-20240303200901253](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202403032009322.png)

其中，$z_l$、$W_l$、$U_l$ 和 $b_l$ 是可学习的参数，$h^{t-1}$ 是隐藏状态。注意力机制将更高的权重分配给对当前交通状态贡献较大的因素，这些权重也应用于边属性和全局属性。对于边属性，我们将连接节点 i 和 j 的边的隐藏状态定义为相应隐藏状态 $h_i$ 和 $h_j$ 的组合。对于全局属性，我们将隐藏状态定义为所有节点的隐藏状态的总结。

*To obtain the final prediction result, we add a fully connected layer to the output of the LSTM layer. The entire network is trained by minimizing the error of the predicted traffic flow using backpropagation through time. Suppose the output of combined GNN and LSTM layer is Y t+l and the ground truth is Y t+l 0 , then the loss function is defined as:*

为了获得最终的预测结果，我们在 LSTM 层的输出上添加一个全连接层。整个网络通过时间反向传播来最小化预测交通流量的误差。假设联合 GNN 和 LSTM 层的输出是 $Y^{t+l}$，真实值是 $Y^{t+l}_0$，则损失函数定义为：

![image-20240303202712422](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202403032027491.png)

其中，W1、W2是联合GNN和LSTM以及注意力机制中的权重矩阵，λ1、λ2是惩罚项。

## 四、实验

### A.实验设置

+ 数据集：我们在两个真实世界的交通数据集上评估了我们的方法，即PEMS和HZJTD

+ HZJTD数据集：HZJTD数据集来自杭州综合交通研究中心。该数据集包含了杭州市202条道路的传感器交通数据。收集的交通数据时间跨度为2013年10月16日至2014年10月3日。交通数据按照每15分钟进行汇总。

  HZJTD数据集的相关城市数据包括POI数据和地理数据。POI数据来自百度地图API。地理数据包含道路网络的拓扑结构以及每对传感器之间的驾驶距离。与PEMS数据集不同，与HZJTD数据集相关的事故数据和天气数据不可用。

  因此，HZJTD的属性图是由交通数据、POI数据、地理数据和日历特征构建而成的。

+ PEMS数据集是从加州交通部网站上获取的，该网站存储着来自CalTrans Performance Measurement System（PeMS）的数据。PeMS是一个实时的存档数据管理系统，能够实时收集、存储和处理原始的交通数据。在实验中，我们选择了PEMS数据集的两个部分，即PEMS-S和PEMS-L。PEMS-S数据集包含了30个传感器在斯托克顿部署的交通数据，时间跨度是从2017年1月1日到2017年3月31日。交通数据每5分钟汇总一次。PEMS-L数据集包含了在加州第10区部署的362个传感器的交通数据。交通数据也是每5分钟汇总一次，时间跨度同样是从2017年1月1日到2017年3月31日。

  PEMS数据集的相关城市数据包括POI数据、地理数据、道路特性数据、事故数据和天气数据。POI数据来自Google Maps，包含了POI的详细信息，如名称、位置、类别等。地理数据来自OpenStreetMap，包含了道路网络的拓扑结构以及每对传感器之间的驾驶距离。道路特性和事故数据来自PeMS。

  道路特性数据包括道路宽度、道路车道数和道路速限的详细信息。事故数据包括在道路上发生的事故类别和延误时间。天气数据来自加州灌溉管理信息系统（CIMIS），包含太阳辐射、土壤温度、空气温度、相对湿度和降水量等信息。我们根据两个交通数据集所对应的区域和时间段收集数据。

### B.参数

首先，我们经验性地将学习率设置为0.001，批量大小设置为32，训练周期设置为5000。数据集按照时间顺序划分，前70%用于训练，后20%用于测试，另外10%用于验证。其次，我们对AARGNN层数 ∈ {1, 2, 3} 和LSTM隐藏单元数 ∈ {16, 32, 64, 128} 进行网格搜索。最优的AARGNN层数为2，最优的LSTM隐藏单元数为32。

第三，历史时间跨度的长度是模型的一个重要参数。如果将历史时间跨度的长度设置得太短，学习交通数据的相关性将会很困难。如果将历史时间跨度的长度设置得太长，计算复杂度会增加。为了评估历史时间跨度长度的影响，我们选择在{6，12，18，24}个时间槽内设置历史时间跨度的长度，并预测下一个时间槽的交通数据。如图6所示，当历史时间跨度增加时，两个数据集上的RMSE和MAE都会减少。当历史时间跨度的长度大于12个时间槽时，性能变得相对稳定。考虑到计算成本，我们将历史时间跨度的长度设置为12个时间槽作为一种折衷方案。

![image-20240303205700411](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202403032057559.png)

预测间隔也是模型的重要参数。为了调查所提出方法的预测能力，我们将历史时段长度设置为12，并预测未来的交通数据。如图7所示，随着预测间隔从1增加到12，预测误差也增加。当间隔大于4时（对应20分钟），RMSE和MAE显著增加。

![image-20240303205713685](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202403032057762.png)

结果表明，所提出的方法更适合短期预测。

### C.基线方法的比较

![image-20240303205505366](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202403032055538.png)

### D.消融研究

为了进一步研究不同组件的贡献，我们将AARGNN与其变体进行比较，如下所示：

1) AARGNN-nn：仅使用交通流量作为节点属性，忽略其他节点级别的数据。

2) AARGNN-ne：在预测过程中忽略边属性。

3) AARGNN-ng：在预测过程中忽略全局属性。

4) AARGNN-na：忽略各种因素的不同贡献（即去除注意机制）。

PEMS数据集上的结果如表II所示，从中我们可以看到：

1) AARGNN表现优于AARGNN-nn、AARGNN-ne和AARGNN-ng，这验证了用于建模交通数据相关性的所有属性的有效性。

2) 属性的影响可以按照边属性 > 节点属性 > 全局属性的顺序排列。这可能是因为边属性直接与不同传感器的交通数据的相关性有关。此外，全局属性中使用的日历特征主要与时间相关性有关，这也可以从节点属性中使用的交通数据中学习到。

3) AARGNN的性能优于AARGNN-na，这验证了指定不同因素贡献的有效性。

![image-20240303205114748](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202403032051850.png)

在HZJTD上的结果如表III所示。请注意，HZJTD数据集相关的事故数据和天气数据不可用。因此，我们使用速度数据构建节点属性，使用POI数据和地理数据构建边属性，并使用日历特征构建全局属性。因此，我们将AARGNN与AARGNN-na、AARGNN-ne和AARGNN-ng进行比较。可以看出：

1) AARGNN-na的表现与AARGNN相似。其潜在原因可能是这里的注意力机制仅应用于边属性，并且对结果没有太大贡献。

2) AARGNN-ne在变体中表现最差。这个结果表明边属性的影响大于全局属性的影响，与PEMS上的结果一致。

3) AARGNN的表现优于AARGNN-ng和AARGNN-ne，这验证了使用边属性和全局属性构建图的有效性。

![image-20240303205215171](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202403032052248.png)

为了进一步研究注意力机制的效果，我们通过在不同属性上移除注意力组件来比较AARGNN及其变体：

1) AARGNN-w/o-na：移除节点属性上的注意力组件。

2) AARGNN-w/o-ea：移除边属性上的注意力组件。

3) AARGNN-w/o-ga：移除全局属性上的注意力组件。

请注意，与HZJTD数据集相关的事故数据和天气数据不可用，我们仅在PEMS数据集上进行实验。结果如表IV所示，可以看出：

1) AARGNN的表现优于AARGNN-w/o-na、AARGNN-w/o-ea和AARGNN-w/o-ga，这验证了注意力组件的有效性。

2) AARGNN的表现优于AARGNN-w/o-na，但结果类似。潜在原因可能是节点属性中因素的贡献相似。

3) AARGNN-w/o-ea在变体中表现最差。

结果表明，注意力机制在边属性上的作用比节点属性和全局属性上的作用更为重要。这一潜在原因可能在于两个方面：边属性中因素的复杂性以及边属性中每个因素的不同贡献。

![image-20240303205347868](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202403032053954.png)