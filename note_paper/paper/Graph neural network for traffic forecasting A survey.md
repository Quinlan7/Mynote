# Graph neural network for traffic forecasting: A survey



## 摘要

*Traffic forecasting is important for the success of intelligent transportation systems. Deep learning models, including convolution neural networks and recurrent neural networks, have been extensively applied in traffic forecasting problems to model spatial and temporal dependencies. In recent years, to model the graph structures in transportation systems as well as contextual information, graph neural networks have been introduced and have achieved state-of-the-art performance in a series of traffic forecasting problems. In this survey, we review the rapidly growing body of research using different graph neural networks, e.g. graph convolutional and graph attention networks, in various traffic forecasting problems, e.g. road traffic flow and speed forecasting, passenger flow forecasting in urban rail transit systems, and demand forecasting in ridehailing platforms. We also present a comprehensive list of open data and source codes for each problem and identify future research directions. To the best of our knowledge, this paper is the first comprehensive survey that explores the application of graph neural networks for traffic forecasting problems. We have also created a public GitHub repository where the latest papers, open data, and source codes will be updated.*

交通预测对智能交通系统的成功至关重要。深度学习模型，包括卷积神经网络和循环神经网络，已广泛应用于交通预测问题，以建模空间和时间依赖关系。近年来，为了模拟交通系统中的图结构以及上下文信息，引入了图神经网络，并在一系列交通预测问题中取得了最先进的性能。在本综述中，我们回顾了使用不同图神经网络（如图卷积网络和图注意力网络）在各种交通预测问题中的快速增长的研究体系，例如道路交通流和速度预测、城市轨道交通系统中的乘客流预测以及顺风车平台的需求预测。我们还提供了每个问题的开放数据和源代码的全面列表，并确定了未来的研究方向。据我们所知，这篇论文是第一篇全面调查探讨图神经网络在交通预测问题中的应用。我们还创建了一个公共的GitHub仓库，其中将更新最新的论文、开放数据和源代码。





## 一、介绍

*Transportation systems are among the most important infrastructure in modern cities, supporting the daily commuting and traveling of millions of people. With rapid urbanization and population growth, transportation systems have become more complex. Modern transportation systems encompass road vehicles, rail transport, and various shared travel modes that have emerged in recent years, including online ride-hailing, bike-sharing, and e-scooter sharing. Expanding cities face many transportation-related problems, including air pollution and traffic congestion. Early intervention based on traffic forecasting is seen as the key to improving the efficiency of a transportation system and to alleviate transportation-related problems. In the development and operation of smart cities and intelligent transportation systems (ITSs), traffic states are detected by sensors (e.g. loop detectors) installed on roads, subway and bus system transaction records, traffic surveillance videos, and even smartphone GPS (Global Positioning System) data collected in a crowd-sourced fashion. Traffic forecasting is typically based on consideration of historical traffic state data, together with the external factors which affect traffic states, e.g. weather and holidays.*

交通系统是现代城市中最重要的基础设施之一，支持着数百万人的日常通勤和旅行需求。随着城市化的迅速发展和人口的增长，交通系统变得更加复杂。现代交通系统包括道路车辆、铁路交通以及近年来出现的各种共享出行方式，包括在线顺风车、共享单车和电动滑板车共享等。不断扩张的城市面临许多与交通相关的问题，包括空气污染和交通拥堵。基于交通预测的早期干预被视为提高交通系统效率和缓解与交通有关的问题的关键。

在智能城市和智能交通系统（ITS）的发展和运营过程中，通过在道路上安装的传感器（如环形检测器）、地铁和公交系统的交易记录、交通监控视频，甚至是以众包方式收集的智能手机GPS（全球定位系统）数据，可以检测交通状态。交通预测通常基于历史交通状态数据，==结合影响交通状态的外部因素，例如天气和假期==。

*Both short-term and long-term traffic forecasting problems for various transport modes are considered in the literature. This survey focuses on the data-driven approach, which involves forecasting based on historical data. The traffic forecasting problem is more challenging than other time series forecasting problems because it involves large data volumes with high dimensionality, as well as multiple dynamics including emergency situations, e.g. traffic accidents. The traffic state in a specific location has both spatial dependency, which may not be affected only by nearby areas, and temporal dependency, which may be seasonal. Traditional linear time series models, e.g. auto-regressive and integrated moving average (ARIMA) models, cannot handle such spatiotemporal forecasting problems. Machine learning (ML) and deep learning techniques have been introduced in this area to improve forecasting accuracy, for example, by modeling the whole city as a grid and applying a convolutional neural network (CNN) as demonstrated by Jiang and Zhang (2018). However, the CNN-based approach is not optimal for traffic foresting problems that have a graph-based form, e.g. road networks*

文献中考虑了各种交通方式的短期和长期交通预测问题。本综述关注基于数据驱动的方法，即基于历史数据进行预测。交通预测问题比其他时间序列预测问题更具挑战性，因为它涉及大量具有高维度的数据，以及包括紧急情况在内的多种动态因素，例如交通事故。特定位置的交通状态既具有空间依赖性（可能不仅受到附近地区的影响），还具有时间依赖性，可能呈现季节性。传统的线性时间序列模型，例如自回归和移动平均（ARIMA）模型，无法处理这种时空预测问题。为提高预测准确性，机器学习（ML）和深度学习技术已被引入到这一领域，例如通过将整个城市建模为网格，并应用卷积神经网络（CNN），正如江和张（2018）所演示的那样。

然而，基于CNN的方法并不是对于具有图形式的交通预测问题（例如道路网络）最优的。



*In recent years, graph neural networks (GNNs) have become the frontier of deep learning research, showing state-of-the-art performance in various applications (Wu et al, 2020). GNNs are ideally suited to traffic forecasting problems because of their ability to capture spatial dependency, which is represented using non-Euclidean graph structures. For example, a road network is naturally a graph, with road intersections as the nodes and road connections as the edges. With graphs as the input, several GNN-based models have demonstrated superior performance to previous approaches on tasks including road traffic flow and speed forecasting problems. These include, for example, the diffusion convolutional recurrent neural network (DCRNN) (Li, Yu, Shahabi and Liu, 2018) and Graph WaveNet (Wu, Pan, Long, Jiang, & Zhang, 2019) models. The GNN-based approach has also been extended to other transportation modes, utilizing various graph formulations and models.*

近年来，图神经网络（GNNs）已成为深度学习研究的前沿，在各种应用中展现出最先进的性能（吴等，2020）。由于能够捕捉以非欧几里德图结构表示的空间依赖性，GNNs非常适合处理交通预测问题。例如，道路网络自然是一个图，其中道路交叉口是节点，而道路连接是边。以图作为输入，一些基于GNN的模型在道路交通流和速度预测等任务上展示出了优于先前方法的性能。这些模型包括扩散卷积循环神经网络（DCRNN）（李、于、沙哈比、刘，2018）和图WaveNet（吴、潘、龙、江、张，2019）。基于GNN的方法还扩展到其他交通模式，利用各种图形式和模型。

*To the best of the authors’ knowledge, this paper presents the first comprehensive literature survey of GNN-related approaches to traffic forecasting problems. While several relevant traffic forecasting surveys exist (Boukerche, Tao, & Sun, 2020; Boukerche & Wang, 2020a; Fan et al, 2020; George & Santra, 2020; Haghighat et al, 2020; Lee, Eo, Jung, Yoon, & Rhee, 2021; Luca, Barlacchi, Lepri, & Pappalardo, 2020; Manibardo, Laña, & Del Ser, 2021; Pavlyuk, 2019; Shi & Yeung, 2018; Tedjopurnomo, Bao, Zheng, Choudhury, & Qin, 2020; Varghese, Chikaraishi, & Urata, 2020; Xie et al, 2020; Ye, Zhao, Ye, & Xu, 2020a; Yin et al, 2021), most of them are not GNN-focused with only one exception (Ye et al, 2020a). For this survey, we reviewed 212 papers published in the years 2018 to 2020. Additionally, because this is a very rapidly developing research field, we also included preprints that have not yet gone through the traditional peer review process (e.g., arXiv papers) to present the latest progress. Based on these studies, we identify the most frequently considered problems, graph formulations, and models. We also investigate and summarize publicly available useful resources, including datasets, software, and open-sourced code, for GNN-based traffic forecasting research and application. Lastly, we identify the challenges and future directions of applying GNNs to the traffic forecasting problem*

据作者所知，本文是第一篇对交通预测问题中与图神经网络（GNN）相关方法的全面文献综述。尽管存在一些相关的交通预测综述（Boukerche, Tao, & Sun, 2020; Boukerche & Wang, 2020a; Fan et al, 2020; George & Santra, 2020; Haghighat et al, 2020; Lee, Eo, Jung, Yoon, & Rhee, 2021; Luca, Barlacchi, Lepri, & Pappalardo, 2020; Manibardo, Laña, & Del Ser, 2021; Pavlyuk, 2019; Shi & Yeung, 2018; Tedjopurnomo, Bao, Zheng, Choudhury, & Qin, 2020; Varghese, Chikaraishi, & Urata, 2020; Xie et al, 2020; Ye, Zhao, Ye, & Xu, 2020a; Yin et al, 2021)，但其中大多数并非专注于GNN，只有一个例外（Ye et al, 2020a）。对于这个综述，我们回顾了2018年至2020年间发表的212篇论文。此外，由于这是一个发展迅速的研究领域，我们还包括了尚未经过传统同行评审过程的预印本（例如arXiv上的论文），以呈现最新的进展。基于这些研究，我们确定了最经常被考虑的问题、图形式和模型。我们还调查和总结了对GNN-based交通预测研究和应用有用的公开资源，包括数据集、软件和开源代码。最后，我们确定了将GNN应用于交通预测问题所面临的挑战和未来方向。

*Instead of giving a whole picture of traffic forecasting, our aim is to provide a comprehensive summary of GNN-based solutions. This paper is useful for both the new researchers in this field who want to catch up with the progress of applying GNNs and the experienced researchers who are not familiar with these latest graph-based solutions. In addition to this paper, we have created an open GitHub repository on this topic,1 where relevant content will be updated continuously*

与提供交通预测的整体图景不同，我们的目标是提供对基于图神经网络（GNN）解决方案的全面摘要。这篇论文对于这一领域的新研究人员想要了解应用GNN的进展以及对这些最新的基于图的解决方案不太熟悉的经验丰富的研究人员都是有用的。除了这篇论文，我们还创建了一个关于这个主题的开放GitHub存储库1，相关内容将不断更新。 

*Our contributions are summarized as follows: (1) Comprehensive Review: We present the most comprehensive review of graph-based solutions for traffic forecasting problems in the past three years (2018–2020).*

*(2) Resource Collection: We provide the latest comprehensive list of open datasets and code resources for replication and comparison of GNNs in future work.*

*(3) Future Directions: We discuss several challenges and potential future directions for researchers in this field, when using GNNs for traffic forecasting problems.*

*The remainder of this paper is organized as follows. In Section 2, we compare our work with other relevant research surveys. In Section 3, we categorize the traffic forecasting problems that are involved with GNN-based models. In Section 4.1, we summarize the graphs and GNNs used in the reviewed studies. In Section 5, we outline the open resources. Finally, in Section 6, we point out challenges and future directions.*

我们的贡献总结如下：
1. 全面综述：我们提供了过去三年（2018-2020）交通预测问题的基于图的解决方案的最全面综述。

2. 资源收集：我们提供了最新的开放数据集和代码资源的全面列表，以便未来的研究可以进行复制和比较GNN的工作。

3. 未来方向：我们讨论了在使用GNN解决交通预测问题时，这一领域的研究人员可能面临的几个挑战和潜在未来方向。

本文的其余部分组织如下。在第2节中，我们将我们的工作与其他相关研究综述进行比较。在第3节中，我们对涉及GNN模型的交通预测问题进行分类。在第4.1节中，我们总结了在审查的研究中使用的图形和GNNs。在第5节中，我们概述了开放资源。最后，在第6节中，我们指出了挑战和未来方向。



## 二、相关研究调查

*In this section, we introduce the most recent relevant research surveys (most of which were published in 2020). The differences between our study and these existing surveys are pointed out when appropriate.*

*We start with the surveys addressing wider ITS topics, followed by those focusing on traffic prediction problems and GNN application in particular*.

~~在本节中，我们介绍了最近相关的研究综述（其中大部分发表在2020年）。在适当的时候，我们指出了我们的研究与这些现有综述的区别。~~

~~我们首先介绍涵盖更广泛ITS主题的综述，然后介绍专注于交通预测问题和特定GNN应用的综述。~~

*Besides traffic forecasting, machine learning and deep learning methods have been widely used in ITSs as discussed in Fan et al (2020), Haghighat et al (2020) and Luca et al (2020). In Haghighat et al (2020), GNNs are only mentioned in the task of traffic characteristics prediction. Among the major milestones of deep-learning driven traffic prediction (summarized in Figure 2 of Fan et al (2020)), the stateof-the-art models after 2019 are all based on GNNs, indicating that GNNs are indeed the frontier of deep learning-based traffic prediction research.*

~~除了交通预测之外，机器学习和深度学习方法在智能交通系统（ITSs）中得到了广泛应用，这一点在Fan等人（2020）、Haghighat等人（2020）和Luca等人（2020）的研究中有所讨论。在Haghighat等人（2020）的研究中，GNNs仅在交通特征预测任务中被提及。在Fan等人（2020）的图2总结的深度学习驱动交通预测的主要里程碑中，2019年后的最先进模型都是基于GNNs的，这表明GNNs确实是基于深度学习的交通预测研究的前沿。~~

*Roughly speaking, five different types of traffic prediction methods are identified and categorized in previous surveys (George & Santra, 2020; Xie, Li et al, 2020), namely, statistics-based methods, traditional machine learning methods, deep learning-based methods, reinforcement learning-based methods, and transfer learning-based methods.*

*Some comparisons between different categories have been considered, e.g., statistics-based models have better model interpretability, whereas ML-based models are more flexible as discussed in Boukerche et al (2020). Machine learning models for traffic prediction are further categorized in Boukerche and Wang (2020a), which include the regression model, example-based models (e.g., k-nearest neighbors), kernel-based models (e.g. support vector machine and radial basis function), neural network models, and hybrid models. Deep learning models are further categorized into five different generations in Lee et al (2021), in which GCNs are classified as the fourth generation and other advanced techniques that have been considered but are not yet widely applied are merged into the fifth generation. These include transfer learning, meta learning, reinforcement learning, and the attention mechanism. Before these advanced techniques become mature in traffic prediction tasks, GNNs remain the state-of-the-art technique.*

粗略而言，在先前的调查中，已经确定并分类了五种不同类型的交通预测方法（George & Santra, 2020; Xie, Li et al, 2020），即基于统计的方法、传统机器学习方法、基于深度学习的方法、基于强化学习的方法和基于迁移学习的方法。

一些对不同类别之间的比较已经被考虑，例如，基于统计的模型具有更好的模型可解释性，而基于机器学习的模型在Boukerche等人（2020）的讨论中更加灵活。交通预测的机器学习模型在Boukerche和Wang（2020a）中进一步被分类，包括回归模型、基于示例的模型（例如k最近邻）、基于核的模型（例如支持向量机和径向基函数）、神经网络模型和混合模型。深度学习模型在Lee等人（2021）中被进一步分为五个不同的世代，其中图卷积网络（GCNs）被归类为第四代，其他先进技术被合并到第五代。这包括迁移学习、元学习、强化学习和注意机制。在这些先进技术在交通预测任务中变得成熟之前，GNNs仍然是最先进的技术。

*Some of the relevant surveys only focus on the progress of deep learning-based methods (Tedjopurnomo et al, 2020), while the others prefer to compare them with the statistics-based and machine learning methods (Manibardo et al, 2021; Yin et al, 2021). In Tedjopurnomo et al (2020), 37 deep neural networks for traffic prediction are reviewed, categorized, and discussed. The authors conclude that encoder– decoder long short term-memory (LSTM) combined with graph-based methods is the state-of-the-art prediction technique. A detailed explanation of various data types and popular deep neural network architectures is also provided, along with challenges and future directions for traffic prediction. Conversely, it is found that deep learning is not always the best modeling technique in practical applications, where linear models and machine learning techniques with less computational complexity can sometimes be preferable (Manibardo et al, 2021).*

*Additional research surveys consider aspects other than model selection. In Pavlyuk (2019), spatiotemporal feature selection and extraction pre-processing methods, which may also be embedded as internal model processes, are reviewed. A meta-analysis of prediction accuracy when applying deep learning methods to transport studies is given in Varghese et al (2020). In this study, apart from the models themselves, additional factors including sample size and prediction time horizon are shown to have a significant influence on prediction accuracy.*

*To the authors’ best knowledge, there are no existing surveys focusing on the application of GNNs for traffic forecasting. Graph-based deep learning architectures are reviewed in Ye et al (2020a), for a series of traffic applications, namely, traffic congestion, travel demand, transportation safety, traffic surveillance, and autonomous driving. Specific and practical guidance for constructing graphs in these applications is provided. The advantages and disadvantages of both GNNs and other deep learning models, e.g. recurrent neural network (RNN), temporal convolutional network (TCN), Seq2Seq, and generative adversarial network (GAN), are examined. While the focus is not limited to traffic prediction problems, the graph construction process is universal in the traffic domain when GNNs are involved.*

一些相关综述只关注基于深度学习方法的进展（Tedjopurnomo等人，2020），而其他人更喜欢将它们与基于统计和机器学习方法进行比较（Manibardo等人，2021；Yin等人，2021）。在Tedjopurnomo等人（2020）的研究中，审查了37种用于交通预测的深度神经网络，并对其进行分类和讨论。作者得出结论，编码器-解码器长短时记忆网络（LSTM）与基于图的方法相结合是最先进的预测技术。此外，还提供了各种数据类型和流行的深度神经网络架构的详细解释，以及关于交通预测的挑战和未来方向。相反，在Manibardo等人（2021）的研究中发现，在实际应用中，深度学习并不总是最佳建模技术，有时具有较低计算复杂性的线性模型和机器学习技术可能更可取。

其他研究综述考虑了模型选择之外的方面。在Pavlyuk（2019）中，审查了可能作为内部模型过程嵌入的时空特征选择和提取预处理方法。在Varghese等人（2020）的研究中，对将深度学习方法应用于交通研究时的预测准确性进行了元分析。在这项研究中，除了模型本身，样本大小和预测时间范围等其他因素被显示对预测准确性有重要影响。

据作者所知，目前尚无关于应用GNNs进行交通预测的综述。在Ye等人（2020a）中审查了基于图的深度学习架构，针对一系列交通应用，包括交通拥堵、出行需求、交通安全、交通监视和自动驾驶。提供了在这些应用中构建图的具体和实用指导。审查了GNNs和其他深度学习模型（例如循环神经网络（RNN）、时间卷积网络（TCN）、Seq2Seq和生成对抗网络（GAN））的优劣势。虽然焦点不仅限于交通预测问题，但在涉及GNNs时，图构建过程在交通领域是通用的。



## 三、问题

*In this section, we discuss and categorize the different types of traffic forecasting problems considered in the literature. Problems are first categorized by the traffic state to be predicted. Traffic flow, speed, and demand problems are considered separately while the remaining types are grouped together under ‘‘other problems’’. Then, the problemtypes are further broken down into levels according to where the traffic states are defined. These include road-level, region-level, and station-level categories.*

~~在本节中，我们讨论并对文献中考虑的不同类型的交通预测问题进行分类。首先，通过要预测的交通状态进行分类。交通流、速度和需求问题被单独考虑，而其余类型被分为“其他问题”组。然后，问题类型根据交通状态的定义进一步细分。这包括道路级别、区域级别和站点级别等类别。~~

*Different problem types have different modeling requirements for representing spatial dependency. For the road-level problems, the traffic data are usually collected from sensors, which are associated with specific road segments, or GPS trajectory data, which are also mapped into the road network with map matching techniques. In this case, the road network topology can be seen as the graph to use, which may contain hundreds or thousands of road segments potentially. The spatial dependency may be described by the road network connectivity or spatial proximity. For the station-level problems, the metro or bus station topology can be taken as the graph to use, which may contain tens or hundreds of stations potentially. The spatial dependency may be described by the metro lines or bus routes. For the region-level problem, the regular or irregular regions are used as the nodes in a graph. The spatial dependency between different regions can be extracted from the land use purposes, e.g., from the points-of-interest data.*

~~不同类型的问题对于表示空间依赖性有不同的建模要求。对于道路级别的问题，交通数据通常是从与特定道路段相关联的传感器收集的，或者是从具有地图匹配技术的GPS轨迹数据映射到道路网络中的。在这种情况下，道路网络拓扑可以被视为要使用的图，其中可能包含数百或数千个潜在的道路段。空间依赖性可以通过道路网络的连接性或空间接近性来描述。对于站点级别的问题，地铁或公交站点拓扑可以被视为要使用的图，其中可能包含数十或数百个站点。空间依赖性可以通过地铁线路或公交路线来描述。对于区域级别的问题，规则或不规则的区域被用作图中的节点。不同区域之间的空间依赖性可以从土地利用目的中提取，例如，从兴趣点数据中提取。~~

*A full list of the traffic forecasting problems considered in the surveyed studies is shown in Table 1. Instead of giving the whole picture of traffic forecasting research, only those problems with GNN-based solutions in the literature are listed in Table 1.*

*Generally speaking, traffic forecasting problems are challenging, not only for the complex temporal dependency, but only for the complex spatial dependency. While many solutions have been proposed for dealing with the time dependency, e.g., recurrent neural networks and temporal convolutional networks, the problem to capture and model the spatial dependency has not been fully solved. The spatial dependency, which refers to the complex and nonlinear relationship between the traffic state in one particular location with other locations.This location could be a road intersection, a subway station, or a city region. The spatial dependency may not be local, e.g., the traffic state may not only be affected by nearby areas, but also those which are far away in the spatial range but connected by a fast transportation tool.The graphs are necessary to capture such kind of spatial information as we would discuss in the next section*

Table 1中列出了在调查研究中考虑的交通预测问题的完整列表。与其呈现交通预测研究的整体画面，表1中只列举了文献中具有基于GNN的解决方案的问题。

总体而言，交通预测问题是具有挑战性的，不仅因为存在复杂的时间依赖性，还因为存在复杂的空间依赖性。虽然已经提出了许多解决方案来处理时间依赖性，例如循环神经网络和时间卷积网络，但捕捉和建模空间依赖性的问题尚未完全解决。空间依赖性指的是在一个特定位置的交通状态与其他位置之间的复杂和非线性关系。这个位置可以是一个道路交叉口、一个地铁站，或者一个城市区域。空间依赖性可能不是局部的，例如，交通状态可能不仅受到附近地区的影响，还受到空间范围内连接的远处地区的影响。

图形对于捕捉这种空间信息是必要的，我们将在下一节中讨论。

*Before the usage of graph theories and GNNs, the spatial information is usually extracted by multivariate time series models or CNNs.Within a multivariate time series model, e.g., vector autoregression, the traffic states collected in different locations or regions are combined together as multivariate time series. However, the multivariate time series models can only extract the linear relationship among different states, which is not enough for modeling the complex and nonlinear spatial dependency. CNNs take a step further by modeling the local spatial information, e.g., the whole spatial range is divided into regular grids as the two-dimensional image format and the convolution operation is performed in the neighbor grids. However, the CNN-based approach is bounded to the case of Euclidean structure data, which cannot model the topological structure of the subway network or the road network*

*Graph neural networks bring new opportunities for solving traffic forecasting problems, because of their strong learning ability to capture the spatial information hidden in the non-Euclidean structure data, which are frequently seen in the traffic domain. Based on graph theories, both nodes and edges have their own attributes, which can be used further in the convolution or aggregation operations. These attributes describe different traffic states, e.g., volume, speed, lane numbers, road level, etc. For the dynamic spatial dependency, dynamic graphs can be learned from the data automatically. For the case of hierarchical traffic problems, the concepts of super-graphs and sub-graphs can be defined and further used.*

在使用图理论和图神经网络（GNNs）之前，空间信息通常是通过多变量时间序列模型或卷积神经网络（CNNs）来提取的。在多变量时间序列模型中，例如向量自回归，收集在不同位置或区域的交通状态被组合为多变量时间序列。然而，多变量时间序列模型只能提取不同状态之间的线性关系，这对于建模复杂和非线性的空间依赖性是不足够的。CNNs通过对局部空间信息建模迈出了一步，例如，整个空间范围被划分为规则的网格，作为二维图像格式，并在相邻网格上执行卷积操作。然而，基于CNN的方法局限于欧几里得结构数据的情况，无法模拟地铁网络或道路网络的拓扑结构。

图神经网络为解决交通预测问题带来了新的机会，因为它们具有强大的学习能力，能够捕捉在交通领域经常出现的非欧几里得结构数据中隐藏的空间信息。基于图理论，节点和边都有自己的属性，可以在卷积或聚合操作中进一步使用。这些属性描述了不同的交通状态，例如，流量、速度、车道数、道路级别等。对于动态空间依赖性，可以从数据中自动学习动态图。对于分层交通问题，可以定义并进一步使用超图和子图的概念。



### 3.1 traffic flow

*Traffic flow is defined as the number of vehicles passing through a spatial unit, such as a road segment or traffic sensor point, in a given time period. An accurate traffic flow prediction is beneficial for a variety of applications, e.g., traffic congestion control, traffic light control, vehicular cloud, etc. (Boukerche & Wang, 2020a). For example, traffic light control can reduce vehicle staying time at the road intersections, optimizing the traffic flow, and reducing traffic congestion and vehicle emission.*

*We consider three levels of traffic flow problems in this survey, namely, road-level flow, region-level flow, and station-level flow.*

*Road-level flow problems are concerned with traffic volumes on a road and include road traffic flow, road origin–destination (OD) Flow, and intersection traffic throughput. In road traffic flow problems, the prediction target is the traffic volume that passes a road sensor or a specific location along the road within a certain time period (e.g. five minutes).*

*In the road OD flow problem, the target is the volume between one location (the origin) and another (the destination) at a single point in time. The intersection traffic throughput problem considers the volume of traffic moving through an intersection.*

*Region-level flow problems consider traffic volume in a region. A city may be divided into regular regions (where the partitioning is grid-based) or irregular regions (e.g. road-based or zip-code-based partitions). These problems are classified by transport mode into regional taxi flow, regional bike flow, regional ride-hailing flow, regional dockless e-scooter flow, regional OD taxi flow, regional OD bike flow, and regional OD ride-hailing flow problems.*

*Station-level flow problems relate to the traffic volume measured at a physical station, for example, a subway or bus station. These problems are divided by station type into station-level subway passenger flow, station-level bus passenger flow, station-level shared vehicle flow, station-level bike flow, and station-level railway passenger flow problems.*

*Road-level traffic flow problems are further divided into cases of unidirectional and bidirectional traffic flow, whereas region-level and station-level traffic flow problems are further divided into the cases of inflow and outflow, based on different problem formulations.*

*While traffic sensors have been successfully used, data collection for traffic flow information is still a challenge when considering the high costs in deployment and maintenance of traffic sensors. Another potential approach is using the pervasive mobile and IoT devices, which have a lower cost generally, e.g., GPS sensors. However, challenges still exist when considering the data quality problems frequently seen in GPS data, e.g., missing data caused by unstable communication links.*

*The traffic light is another source of challenges for various traffic prediction tasks. Short-term traffic flow fluctuation and the spatial relation change between two road segments can be caused by the traffic light. The way of controlling the traffic light may be different in different time periods, causing an inconsistent traffic flow pattern.*

交通流量被定义为通过一个空间单元（如道路段或交通传感器点）的车辆数量，在给定的时间段内。准确的交通流量预测对于各种应用都是有益的，例如交通拥堵控制、交通信号灯控制、车辆云等（Boukerche & Wang, 2020a）。例如，交通信号灯控制可以减少车辆在路口停留的时间，优化交通流，减少交通拥堵和车辆排放。

在本调查中，我们考虑了三个层次的交通流问题，即道路级别流、区域级别流和站点级别流。

道路级别流问题关注的是道路上的交通量，包括道路交通流、道路起点-终点（OD）流和交叉口交通吞吐量。在道路交通流问题中，预测目标是在特定时间段内通过道路传感器或道路上特定位置的交通量（例如，五分钟内）。

在道路OD流问题中，目标是在单一时刻两个位置之间的交通量（起点和终点）。交叉口交通吞吐量问题考虑的是通过交叉口移动的交通量。

区域级别流问题考虑的是区域内的交通量。城市可以被划分为规则区域（基于网格的划分）或不规则区域（例如基于道路或邮政编码的划分）。这些问题根据运输模式分为区域出租车流、区域自行车流、区域约车流、区域无桩电动滑板车流、区域OD出租车流、区域OD自行车流和区域OD约车流问题。

站点级别流问题与在物理站点（例如地铁或公交站）测量到的交通量相关。这些问题根据站点类型分为站点级别地铁客流、站点级别公交客流、站点级别共享车辆流、站点级别自行车流和站点级别铁路客流问题。

道路级别的交通流问题进一步分为单向和双向交通流的情况，而区域级别和站点级别的交通流问题则根据不同的问题制定进一步分为流入和流出的情况。

虽然交通传感器已经被成功使用，但在考虑交通传感器的部署和维护成本较高时，交通流信息的数据收集仍然是一个挑战。另一个潜在的方法是使用无处不在的移动和物联网设备，其成本通常较低，例如GPS传感器。然而，在考虑到GPS数据中经常出现的数据质量问题时，仍然存在挑战，例如由不稳定通信链路引起的缺失数据。

交通信号灯是各种交通预测任务面临的另一个挑战来源。短期交通流波动和两个道路段之间的空间关系变化可能是由交通信号灯引起的。在不同时间段，控制交通信号灯的方式可能不同，导致交通流模式不一致。



### 3.2 交通速度

*Traffic speed is another important indicator of traffic state with potential applications in ITS systems, which is defined as the average speed of vehicles passing through a spatial unit in a given time period.The speed value on the urban road can reflect the crowdedness level of road traffic. For example, Google Maps visualizes this crowdedness level from crowd-sourcing data collected from individual mobile devices and in-vehicle sensors. A better traffic speed prediction is also useful for route navigation and estimation-of-arrival applications.*

交通速度是交通状态的另一个重要指标，具有在智能交通系统（ITS）中潜在应用的可能性。交通速度被定义为在给定时间段内通过空间单元的车辆的平均速度。

城市道路上的速度值可以反映道路交通的拥挤程度。例如，Google Maps通过从个体移动设备和车载传感器收集的众包数据可视化了这种拥挤程度。更好的交通速度预测还对路径导航和到达时间估计应用程序有用。

*We consider two levels of traffic speed problems in this survey, namely, road-level and region-level problems. We also include travel time and congestion predictions in this category because they are closely correlated to traffic speed. Travel time prediction is useful for passengers to plan their commuting time and for drivers to select fast routes, respectively. Traffic congestion is one of the most important and urgent transportation problems in cities, which brings significant time loss, air pollution and energy waste. The congestion prediction results can be used to control the road conditions and optimize vehicle flow, e.g., with traffic signal control. In several studies, traffic congestion is judged by a threshold-based speed inference. The specific roadlevel speed problem categories considered are road traffic speed, road travel time, traffic congestion, and time of arrival problems; while the region-level speed problem considered is regional OD taxi speed.*

在本调查中，我们考虑了两个级别的交通速度问题，即道路级别和区域级别的问题。我们还将旅行时间和拥堵预测包括在此类别中，因为它们与交通速度密切相关。旅行时间预测对乘客规划通勤时间和对驾驶员选择快速路线都很有用。交通拥堵是城市中最重要和紧迫的交通问题之一，它带来了显著的时间损失、空气污染和能源浪费。拥堵预测的结果可以用于控制道路状况并优化车辆流动，例如通过交通信号控制。在一些研究中，交通拥堵是通过基于速度推断的阈值判断的。具体考虑的道路级别速度问题类别包括道路交通速度、道路旅行时间、交通拥堵和到达时间问题；而考虑的区域级别速度问题是区域OD出租车速度。



## 四、图和图神经网络

*In this section, we summarize the types of graphs and GNNs used in the surveyed studies, focusing on GNNs that are frequently used for traffic forecasting problems. The contributions of this section include an organized approach for classifying the different traffic graphs based on the domain knowledge, and a summary of the common ways for constructing adjacency matrices, which may not be encountered in other neural networks before and would be very helpful for those who would like to use graph neural networks. The different GNN structures already used for traffic forecasting problems are briefly introduced in this section too. For a wider and deeper discussion of GNNs, refer to Wu, Pan, Chen et al (2020), Zhang, Cui and Zhu (2020) and Zhou et al (2020).*

在本节中，我们总结了调查研究中使用的图形类型和图神经网络（GNNs），重点关注经常用于交通预测问题的GNNs。本节的贡献包括一种有组织的方法，根据领域知识对不同的交通图进行分类，以及对构建邻接矩阵的常见方式的总结。这在其他神经网络中可能不会遇到，并且对于那些希望使用图神经网络的人来说将会非常有帮助。本节还简要介绍了已经用于交通预测问题的不同GNN结构。要进行更广泛和深入的GNN讨论，请参阅Wu，Pan，Chen等（2020），Zhang，Cui和Zhu（2020）以及Zhou等（2020）。



### 4.1 交通图

#### 4.1.1 图的构建

*A graph is the basic structure used in GNNs. It is defined as 𝐺 = (𝑉 , 𝐸, 𝐴), where 𝑉 is the set of vertices or nodes, 𝐸 is the set of edges between the nodes, and 𝐴 is the adjacency matrix. Both nodes and edges can be associated with different attributes in different GNN problems. Element 𝑎𝑖𝑗 of 𝐴 represents the ‘‘edge weight’’ between nodes 𝑖 and 𝑗. For a binary connection matrix 𝐴, 𝑎𝑖𝑗 = 1 if there is an edge between nodes 𝑖 and 𝑗 in 𝐸, and 𝑎𝑖𝑗 = 0 otherwise. If 𝐴 is symmetric, the corresponding graph 𝐺 is defined as undirected. Otherwise, 𝐺 is directed, when the edge only exists in one direction between a node pair.*

图是GNN中使用的基本结构，它被定义为 $G = (V, E, A)$，其中 \(V\) 是顶点或节点的集合，\(E\) 是节点之间的边的集合，\(A\) 是邻接矩阵。节点和边在不同的GNN问题中可以关联不同的属性。矩阵 $A$ 的元素 $a_{ij}$ 表示节点 $i$ 和 $j$ 之间的‘‘边权重’’。对于二进制连接矩阵 $A$，如果在 $E$ 中存在节点 $i$ 和 $j$ 之间的边，则 $a_{ij} = 1$，否则 $a_{ij} = 0$。如果 $A$ 是对称的，对应的图 $G$ 被定义为无向图。否则，当边仅在节点对之间的一个方向上存在时，$G$ 是有向的。

*For simplicity, we assume that the traffic state is associated with the nodes. The other case with edges can be derived similarly. In practice, the traffic state is collected or aggregated in discrete time steps, e.g. five minutes or one hour, depending on the specific scenario.*

为简单起见，我们假设交通状态与节点相关联。与边相关的情况可以类似地推导出来。在实际应用中，交通状态是在离散的时间步骤中收集或聚合的，例如五分钟或一小时，具体取决于特定的场景。

*For a single time step 𝑡, we denote the node feature matrix as 𝜒𝑡 ∈ 𝑅𝑁×𝑑 , where 𝑁 is the number of nodes and 𝑑 is the dimension of the node features, i.e., the number of traffic state variables. Now we are ready to give a formal definition of traffic graph.*

在单个时间步（t），我们将节点特征矩阵表示为 $\chi_t \in \mathbb{R}^{N \times d}$，其中 \(N\) 是节点的数量，\(d\) 是节点特征的维度，即交通状态变量的数量。现在我们准备正式定义交通图。

+ *Definition 4.1 (Traffic Graph). A traffic graph (with node features) is defined as a specific type of graph 𝐺 = (𝑉 , 𝐸, 𝐴), where 𝑉 is the node set, 𝐸 is the edge set, and 𝐴 is the adjacency matrix. For a single time step 𝑡, the node feature matrix 𝜒𝑡 ∈ 𝑅𝑁×𝑑 for 𝐺 contains specific traffic states, where 𝑁 is the number of nodes and 𝑑 is the number of traffic state variables.*Then we give a formal definition of graph-based traffic forecasting problem without leveraging external factors firstly
+ 定义4.1（交通图）。一个带有节点特征的交通图被定义为一个特定类型的图 $G = (V, E, A)$，其中 $V$ 是节点集，$E$ 是边集，$A$ 是邻接矩阵。对于单个时间步 $t$，图 $G$ 的节点特征矩阵 $\chi_t \in \mathbb{R}^{N \times d}$ 包含特定的交通状态，其中 $N$ 是节点的数量，$d$ 是交通状态变量的数量。然后，我们首先给出一个不利用外部因素的基于图的交通预测问题的正式定义。
+ *Definition 4.2 (Graph-based Traffic Forecasting). A graph-based traffic forecasting (without external factors) is defined as follows: find a function 𝑓 which generates 𝑦 = 𝑓(𝜒; 𝐺), where 𝑦 is the traffic state to be predicted, 𝜒 = {𝜒1 , 𝜒2 , …, 𝜒𝑇 } is the historical traffic state defined on graph 𝐺, and 𝑇 is the number of time steps in the historical window size.*
+ 定义 4.2（基于图的交通预测）。无外部因素的基于图的交通预测被定义如下：找到一个函数 \(f\)，生成 $y = f(\chi; G)$，其中 $y$ 是要预测的交通状态，$\chi = \{\chi_1, \chi_2, \ldots, \chi_T\}$ 是在图 \(G\) 上定义的历史交通状态，\(T\) 是历史窗口大小中的时间步数。
+ *In single step forecasting, the traffic state in the next time step only is predicted, whereas in multiple step forecasting the traffic state several time steps later is the prediction target. As mentioned in Section 1, traffic states can be highly affected by external factors, e.g. weather and holidays. The forecasting problem formulation, extended to incorporate these external factors, takes the form 𝑦 = 𝑓(𝜒, 𝜀; 𝐺), where 𝜀 represents the external factors. Fig. 1 demonstrates the graph-based traffic forecasting problem, where different color patches represent different traffic variables*
+ 在单步预测中，仅预测下一个时间步的交通状态，而在多步预测中，预测目标是未来若干时间步的交通状态。正如在第1节中提到的，交通状态可能会受到外部因素的影响，例如天气和假期。将预测问题的表述扩展以纳入这些外部因素，形式为 $y = f(\chi, \epsilon; G)$，其中 $\epsilon$ 代表外部因素。图1展示了基于图的交通预测问题，其中不同颜色的区块表示不同的交通变量。



*Various graph structures are used to model traffic forecasting problems depending on both the forecasting problem-type and the traffic datasets available. These graphs can be pre-defined static graphs, or dynamic graphs continuously learned from the data. The static graphs can be divided into two types, namely, natural graphs and similarity graphs. Natural graphs are based on a real-world transportation system, e.g. the road network or subway system; whereas similarity graphs are based solely on the similarity between different node attributes where nodes may be virtual stations or regions.*
根据预测问题类型和可用的交通数据集，可以使用各种图结构来建模交通预测问题。这些图可以是预先定义的静态图，也可以是从数据中持续学习的动态图。静态图可以分为两种类型，即自然图和相似度图。自然图基于真实世界的交通系统，例如道路网络或地铁系统；而相似度图仅基于不同节点属性之间的相似性，其中节点可以是虚拟的站点或区域。

*We categorize the existing traffic graphs into the same three levels used in Section 3, namely, road-level, region-level and station-level graphs.*

我们将现有的交通图分类为在第3节中使用的相同三个级别，即道路级别、区域级别和站点级别的图。

![image-20231203131800307](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202312031318536.png)

> Fig. 2. The real-world case and example of road-level graphs. (a) The road network in the Performance Measurement System (PeMS) where each sensor is a node. Source: http://pems.dot.ca.gov/. (b) The road-level graph examples.
>
> Source: Adapted from Ye et al (2020a).
>
> 图2. 道路级别图的实际案例和示例。 (a) Performance Measurement System (PeMS) 中的道路网络，其中每个传感器是一个节点。来源：http://pems.dot.ca.gov/。 (b) 道路级别图的示例。
>
> 来源：改编自Ye等人（2020a）。

+ *Road-level graphs. These include sensor graphs, road segment graphs, road intersection graphs, and road lane graphs. Sensor graphs are based on traffic sensor data (e.g. the PeMS dataset) where each sensor is a node, and the edges are road connections. The other three graphs are based on road networks with the nodes formed by road segments, road intersections, and road lanes, respectively. The real-world case and example of road-level graphs are shown in Fig. 2. In some cases, roadlevel graphs are the most suitable format, e.g., when vehicles can move only through pre-defined roads.*
+ 道路级别的图。这些包括传感器图、道路段图、道路交叉口图和道路车道图。传感器图基于交通传感器数据（例如PeMS数据集），其中每个传感器是一个节点，边是道路连接。其他三个图基于道路网络，其中节点分别由道路段、道路交叉口和道路车道组成。道路级别图的实际案例和示例显示在图2中。在某些情况下，道路级别图是最合适的格式，例如当车辆只能通过预定义的道路移动时。
+ *Region-level graphs. These include irregular region graphs, regular region graphs, and OD graphs. In both irregular and regular region graphs the nodes are regions of the city. Regular region graphs, which have grid-based partitioning, are listed separately because of their natural connection to previous widely used grid-based forecasting using CNNs, in which the grids may be seen as image pixels. Irregular region graphs include all other partitioning approaches, e.g. road based, or zip code based (Ke, Qin et al, 2021). In the OD graph, the nodes are origin region–destination region pairs. In these graphs, the edges are usually defined with a spatial neighborhood or other similarities, e.g., functional similarity derived from point-of-interests (PoI) data. The real-world case and example of region-level graphs are shown in Fig. 3.*
+ 区域级别的图。这包括不规则区域图、规则区域图和OD图。在不规则和规则的区域图中，节点是城市的不同区域。规则区域图具有基于网格的分区，因为它们自然地与先前广泛使用的基于CNN的基于网格的预测相连接，其中这些网格可以看作是图像像素。不规则区域图包括所有其他分区方法，例如基于道路或基于邮政编码的方法（Ke, Qin等，2021）。在OD图中，节点是起始区域 - 目的地区域对。在这些图中，边通常是通过空间邻近性或其他相似性定义的，例如从兴趣点（PoI）数据中派生的功能相似性。区域级别图的实际案例和示例显示在图3中。
+ *Station-level graphs. These include subway station graphs, bus station graphs, bike station graphs, railway station graphs, car-sharing station graphs, parking lot graphs, and parking block graphs. Usually, there are natural links between stations that are used to define the edges, e.g. subway or railway lines, or the road network. The real-world case and example of station-level graphs are shown in Fig. 4.*
+ 站点级别的图。这包括地铁站图、公交车站图、自行车站图、火车站图、拼车站图、停车场图和停车块图。通常，用于定义边的站点之间存在自然的连接，例如地铁或铁路线路，或者道路网络。站点级别图的实际案例和示例显示在图4中。

*A full list of the traffic graphs used in the surveyed studies is shown in Table 2. Sensor graphs and road segment graphs are most frequently used because they are compatible with the available public datasets as discussed in Section 5. It is noted that in some studies multiple graphs are used as simultaneous inputs and then fused to improve the forecasting performance (Lv et al, 2020; Zhu et al, 2019).*

在调查研究中使用的交通图的完整列表显示在表2中。传感器图和道路段图最常用，因为它们与可用的公共数据集兼容，如第5节中讨论的那样。值得注意的是，在一些研究中，使用多个图作为同时输入，然后融合以提高预测性能（Lv等，2020；Zhu等，2019）。



#### 4.1.2 邻接矩阵的构建

*Adjacency matrices are seen as the key to capturing spatial dependency in traffic forecasting (Ye et al, 2020a). While nodes may be fixed by physical constraints, the user typically has control over the design of the adjacency matrix, which can even be dynamically trained from continuously evolving data. We extend the categories of adjacency matrices used in previous studies (Ye et al, 2020a) and divide them into four types, namely, road-based, distance-based, similarity-based, and dynamic matrices.*

邻接矩阵被视为捕捉交通预测中空间依赖性的关键（Ye等，2020a）。虽然节点可能受到物理约束的限制，但用户通常可以控制邻接矩阵的设计，甚至可以从不断演化的数据中动态训练。我们扩展了先前研究中使用的邻接矩阵的分类（Ye等，2020a），将它们分为四种类型，即基于道路的、基于距离的、基于相似性的和动态矩阵。

+ *Road-based Matrix. This type of adjacency matrix relates to the road network and includes connection matrices, transportation connectivity matrices, and direction matrices. A connection matrix is a common way of representing the connectivity between nodes. It has a binary format, with an element value of 1 if connected and 0 otherwise. The transportation connectivity matrix is used where two regions are geographically distant but conveniently reachable by motorway, highway, or subway (Ye et al, 2020a). It also includes cases where the connection is measured by travel time between different nodes, e.g. if a vehicle can travel between two intersections in less than 5 min then there is an edge between the two intersections (Wu, Chen et al, 2018). The less commonly used direction matrix takes the angle between road links into consideration.*
+ 基于道路的矩阵。这种类型的邻接矩阵与道路网络相关，包括连接矩阵、交通连接矩阵和方向矩阵。连接矩阵是表示节点之间连接性的常见方式。它具有二进制格式，元素值为1表示连接，为0表示未连接。交通连接矩阵用于两个地理上相距较远但可以通过高速公路、公路或地铁方便到达的区域（Ye等，2020a）。它还包括通过不同节点之间的行驶时间来衡量连接的情况，例如，如果车辆可以在两个交叉口之间行驶时间不到5分钟，则两个交叉口之间存在一条边（Wu，Chen等，2018）。较少使用的方向矩阵考虑了道路链接之间的角度。
+ *Distance-based Matrix. This widely used matrix-type represents the spatial closeness between nodes. It contains two sub-types, namely, neighbor and distance matrices. In neighbor matrices, the element values are determined by whether the two regions share a common boundary (if connected the value is set to 1, generally, or 1/4 for grids, and 0 otherwise). In distance-based matrices, the element values are a function of geometrical distance between nodes. This distance may be calculated in various ways, e.g. the driving distance between two sensors, the shortest path length along the road (Kang et al, 2019; Lee & Rhee, 2022), or the proximity between locations calculated by the random walk with restart (RWR) algorithm (Zhang, Wang et al, 2019). One flaw of distance-based matrices is that the fail to take into account the similarity of traffic states between long-distance nodes, and the constructed adjacency matrix is static in most cases.*
+ 基于距离的矩阵。这种广泛使用的矩阵类型表示节点之间的空间接近程度。它包含两个子类型，即邻居矩阵和距离矩阵。在邻居矩阵中，元素值取决于两个区域是否共享一个公共边界（如果连接则值通常设置为1，或者对于网格为1/4，否则为0）。在基于距离的矩阵中，元素值是节点之间几何距离的函数。这个距离可以通过各种方式计算，例如两个传感器之间的驾驶距离，沿着道路的最短路径长度（Kang等，2019；Lee＆Rhee，2022），或者通过具有重启（RWR）算法计算的位置之间的接近度（Zhang，Wang等，2019）。基于距离的矩阵的一个缺点是它们未能考虑长距离节点之间的交通状态相似性，并且在大多数情况下构建的邻接矩阵是静态的。
+ *Similarity-based Matrix. This type of matrix is divided into two subtypes, namely, traffic pattern and functional similarity matrices. Traffic pattern similarity matrices represent the correlations between traffic states, e.g. similarities of flow patterns, mutual dependencies between different locations, and traffic demand correlation in different regions.Functional similarity matrices represent, for example, the distribution of different types of PoIs in different regions*
+ 基于相似性的矩阵。这种类型的矩阵分为两个子类型，即交通模式和功能相似性矩阵。交通模式相似性矩阵表示交通状态之间的相关性，例如流量模式的相似性、不同位置之间的相互依赖关系以及不同区域的交通需求相关性。功能相似性矩阵表示不同区域中不同类型的兴趣点（PoIs）的分布，例如不同区域中不同类型的PoIs的分布。
+ *Dynamic Matrix. This type of matrix is used when no pre-defined static matrices are used. Many studies have demonstrated the advantages of using dynamic matrices, instead of a pre-defined adjacency matrix, for various traffic forecasting problems.*
+ 动态矩阵。当没有使用预定义的静态矩阵时，就会使用这种类型的矩阵。许多研究已经证明，在各种交通预测问题中，使用动态矩阵而不是预定义的邻接矩阵具有优势。

*A full list of the adjacency matrices applied in the surveyed studies is shown in Table 3. Dynamic matrices are listed at the bottom of the table, with no further subdivisions. The connection and distance matrices are the most frequently used types, because of their simple definition and representation of spatial dependency.*

在调查研究中应用的邻接矩阵的完整列表显示在表3中。动态矩阵位于表的底部，没有进一步的细分。连接和距离矩阵是最常用的类型，因为它们定义简单且能够表示空间依赖性。



### 4.2 图神经网络

*Previous neural networks, e.g. fully-connected neural networks (FNNs), CNNs, and RNNs, could only be applied to Euclidean data (i.e. images, text, and videos). As a type of neural network which directly operates on a graph structure, GNNs have the ability to capture complex relationships between objects and make inferences based on data described by graphs. GNNs have been proven effective in various node-level, edge-level, and graph-level prediction tasks (Jiang, 2022). As mentioned in Section 2, GNNs are currently considered the stateof-the-art techniques for traffic forecasting problems. GNNs can be roughly divided into four types, namely, recurrent GNNs, convolutional GNNs, graph autoencoders, and spatiotemporal GNNs (Wu, Pan, Chen et al, 2020). Because traffic forecasting is a spatiotemporal problem, the GNNs used in this field can all be categorized as the spatiotemporal GNNs. However, certain components of the other types of GNNs have also been applied in the surveyed traffic forecasting studies.*

之前的神经网络，如全连接神经网络（FNNs）、卷积神经网络（CNNs）和循环神经网络（RNNs），只能应用于欧几里得数据（即图像、文本和视频）。作为一种直接在图结构上操作的神经网络，图神经网络（GNNs）具有捕捉对象之间复杂关系并基于图描述的数据进行推理的能力。已经证明，GNNs在各种节点级、边级和图级预测任务中是有效的（Jiang，2022）。

正如在第2节中提到的，目前GNNs被认为是交通预测问题的最先进技术。GNNs大致可以分为四种类型，即循环GNNs、卷积GNNs、图自编码器和时空GNNs（Wu，Pan，Chen等，2020）。由于交通预测是一个时空问题，因此在这个领域使用的GNNs都可以被归类为时空GNNs。然而，在调查的交通预测研究中，其他类型的GNNs的某些组件也被应用了。

*To give the mathematical formulation of GCN, we further introduce some notations. Give a graph 𝐺 = (𝑉 , 𝐸, 𝐴), (𝑣𝑖 ) is defined as the neighbor node set of a single node 𝑣𝑖 . 𝐃 is defined as the degree matrix, of which each element is 𝐃𝑖𝑖 = ‖(𝑣𝑖 )‖. 𝐋 = 𝐃 − 𝐀 is defined as the Laplacian matrix of an undirected graph and 𝐋̃ = 𝐈𝑁 − 𝐃 − 1 2 𝐀𝐃− 1 2 is defined as the normalized Laplacian matrix, where 𝐈𝑁 is the identity matrix with size 𝑁. Without considering the time step index, the node feature matrix of a graph is simplified as 𝐗 ∈ 𝑅𝑁×𝑑 , where 𝑁 is the node number and 𝑑 is the dimension of the node feature vector as before. The basic notations used in this survey is summarized in Table 4.*

为了给出GCN的数学公式，我们进一步引入一些符号。给定一个图 $𝐺 = (𝑉 , 𝐸, 𝐴)$， (𝑣𝑖 ) 被定义为单个节点 𝑣𝑖 的邻居节点集合。𝐃 被定义为度矩阵，其中每个元素为 𝐃𝑖𝑖 = ‖(𝑣𝑖 )‖。𝐋 = 𝐃 − 𝐀 被定义为无向图的拉普拉斯矩阵，𝐋̃ = 𝐈𝑁 − 𝐃 − 1/2 𝐀𝐃− 1/2 被定义为归一化拉普拉斯矩阵，其中 𝐈𝑁 是大小为 𝑁 的单位矩阵。不考虑时间步索引，图的节点特征矩阵简化为 𝐗 ∈ 𝑅𝑁×𝑑，其中 𝑁 是节点数量，𝑑 是节点特征向量的维度，与之前一样。在本调查中使用的基本符号总结在表4中。

*When extending the convolution operation from Euclidean data to non-Euclidean data, the basic idea of GNNs is to learn a function mapping for a node to aggregate its own features and the features of its neighbors to generate a new representation. GCNs are spectral-based convolutional GNNs, in which the graph convolutions are defined by introducing filters from graph signal processing in the spectral domain, e.g., the Fourier domain. The graph Fourier transform is firstly used to transform the graph signal to the spectral domain and the inverse graph Fourier transform is further used to transform the result after the convolution operation back. Several spectral-based GCNs are introduced in the literature. Spectral convoluted neural networking (Bruna, Zaremba, Szlam, & LeCun, 2014) assumes that the filter is a set of learnable parameters and considers graph signals with multiple channels. GNN (Henaff, Bruna, & LeCun, 2015) introduces a parameterization with smooth coefficients and makes the spectral filters spatially localized. Chebyshev’s spectral CNN (ChebNet) (Defferrard, Bresson, & Vandergheynst, 2016) leverages a truncated expansion in terms of Chebyshev polynomials up to 𝐾th order to approximate the diagonal matrix.*

当将卷积操作从欧几里得数据扩展到非欧几里得数据时，GNN的基本思想是学习一个将节点的特征及其邻居的特征聚合起来生成新表示的映射函数。GCNs是基于谱的卷积图神经网络，其中图卷积是通过在频谱域引入图信号处理中的滤波器（例如傅里叶域）来定义的。图傅里叶变换首先用于将图信号转换到频谱域，然后利用逆图傅里叶变换将卷积操作的结果返回。文献中介绍了几种基于谱的GCNs。谱卷积神经网络（Bruna，Zaremba，Szlam和LeCun，2014）假设滤波器是一组可学习的参数，并考虑具有多个通道的图信号。

GNN（Henaff，Bruna和LeCun，2015）引入了一种具有平滑系数的参数化，并使谱滤波器在空间上具有局部性。切比雪夫谱CNN（ChebNet）（Defferrard，Bresson和Vandergheynst，2016）利用切比雪夫多项式的截断展开来近似对角矩阵，直到𝐾阶。







## 五、开源数据集和代码

*In this section, we summarize the open data and source code used in the surveyed papers. These open data are suitable for GNN-related studies with graph structures discussed in Section 4, which can be used to formulate different forecasting problems in Section 3. We also list the GNN-related code resources for those who want to replicate the previous GNN-based solutions as baselines in the follow-up studies.*

在本节中，我们总结了调查论文中使用的开放数据和源代码。这些开放数据适用于与第4节讨论的图结构相关的GNN研究，可以用于制定第3节中讨论的不同预测问题。我们还列出了GNN相关的代码资源，供那些想要在后续研究中将先前基于GNN的解决方案作为基线进行复制的人使用。





## 六、挑战和未来方向

*In this section, we discuss general challenges for traffic prediction problems as well as specific new challenges when GNNs are involved.

While GNNs achieve a better forecasting performance, they are not the panacea. Some existing challenges from the border topic of traffic forecasting remain unsolved in current graph-based studies. Based on these challenges, we discuss possible future directions as well as early attempts in these directions. Some of these future directions are inspired from the border traffic forecasting research and remain insightful for the graph-based modeling approach. We would also highlight the special opportunities with GNNs.*

在这一部分，我们讨论交通预测问题的一般挑战，以及当涉及图神经网络（GNNs）时出现的一些特定新挑战。

虽然GNNs在提高预测性能方面取得了更好的效果，但它们并非包治百病。一些来自交通预测边缘主题的现有挑战在当前基于图的研究中仍未解决。基于这些挑战，我们讨论了可能的未来方向以及在这些方向上的早期尝试。其中一些未来方向受到了边缘交通预测研究的启发，并对基于图的建模方法仍具有洞见。我们还将强调使用GNNs的特殊机会。

### 6.1 挑战

#### 6.1.1 异构数据

*Traffic prediction problems involve both spatiotemporal data and external factors, e.g., weather and calendar information. Heterogeneous data fusion is a challenge that is not limited to the traffic domain.

GNNs have enabled significant progress by taking the underlying graph structures into consideration. However, some challenges remain; for example, geographically close nodes may not be the most influential, both for CNN-based and GNN-based approaches. Another special challenge for GNNs is that the underlying graph information may not be correct or up to date. For example, the road topology data of OpenStreetMap, an online map services, are collected in a crowd-sourced approach, which may be inaccurate or lagged behind the real road network. The spatial dependency relationship extracted by GNNs with these inaccurate data may decrease the forecasting accuracy.*

交通预测问题涉及时空数据和外部因素（如天气和日历信息）。异构数据融合是一个挑战，不仅限于交通领域。

通过考虑底层的图结构，GNNs已经在这方面取得了显著的进展。然而，仍然存在一些挑战；例如，对于基于CNN和GNN的方法，地理上相邻的节点可能不一定是最具影响力的。对于GNNs的另一个特殊挑战是底层图信息可能不正确或不及时。例如，OpenStreetMap（在线地图服务）的道路拓扑数据是通过众包方法收集的，这可能是不准确的或者滞后于实际道路网络。由于GNNs使用这些不准确的数据提取的空间依赖关系可能会降低预测的准确性。

*Data quality concerns present an additional challenge with problems such as missing data, sparse data and noise potentially compromising forecasting results. Most of the surveyed models are only evaluated with processed high-quality datasets. A few studies do, however, take data quality related problems into consideration, e.g., using the Kalman filter to deal with the sensor data bias and noise (Chen, Chen et al, 2020), infilling missing data with moving average filters (Hasanzadeh et al, 2019) or linear interpolation (Agafonov, 2020; Chen, Chen et al, 2020). Missing data problem could be more common in GNNs, with the potential missing phenomena happening with historical traffic data or underlying graph information, e.g., GCNs are proposed to fill data gaps in missing OD flow problems (Yao et al, 2020).*

数据质量方面的担忧提出了一个额外的挑战，例如缺失数据、稀疏数据和噪声可能威胁到预测结果。大多数调查的模型仅使用处理过的高质量数据集进行评估。然而，一些研究考虑到了与数据质量相关的问题，例如使用卡尔曼滤波器处理传感器数据的偏差和噪声（Chen, Chen等，2020），使用移动平均滤波器填充缺失数据（Hasanzadeh等，2019），或者使用线性插值（Agafonov, 2020; Chen, Chen等，2020）。在GNNs中，缺失数据问题可能更为常见，有可能在历史交通数据或底层图信息中发生缺失现象，例如GCNs被提出来填补缺失的OD流问题（Yao等，2020）。

*Traffic anomalies (e.g., congestion) are an important external factor that may affect prediction accuracy and it has been proven that under congested traffic conditions a deep neural network may not perform as well as under normal traffic conditions (Mena-Oreja & Gozalvez, 2020). However, it remains a challenge to collect enough anomaly data to train deep learning models (including GNNs) in both normal and anomalous situations. The same concern applies for social events, public holidays, etc.*

交通异常（例如，拥堵）是一个重要的外部因素，可能会影响预测的准确性，并且已经证明，在交通拥堵条件下，深度神经网络的性能可能不如在正常交通条件下（Mena-Oreja & Gozalvez, 2020）。

然而，收集足够的异常数据以在正常和异常情况下训练深度学习模型（包括GNNs）仍然是一个挑战。同样的问题也适用于社会事件、公共假期等情况。

*Challenges also exist for data privacy in the transportation domain. As discussed in Section 5.1, many open data are collected from individual mobile devices in a crowd sourcing approach. The data administrator must guarantee the privacy of individuals who contribute their personal traffic data, as the basis for encouraging a further contribution. Different techniques may be used, e.g., privacy-preserving data publishing techniques and privacy-aware data structures without personal identities.*

在交通领域，数据隐私也存在挑战。如第5.1节所讨论的，许多开放数据是通过众包方法从个体移动设备中收集的。数据管理员必须保证为鼓励进一步的贡献而提供个人交通数据的个体的隐私。可以使用不透露个人身份的隐私保护数据发布技术和隐私感知数据结构等不同技术。

> 总结：
>
> 挑战：
>
> 1. 异构数据融合
> 2. 数据质量
> 3. 考虑外部因素的影响

#### 6.1.2 多任务性能

*For the public service operation of ITSs, a multi-task framework is necessary to incorporate all the traffic information and predict the demand of multiple transportation modes simultaneously. For example, knowledge adaption is proposed to adapt the relevant knowledge from an information-intensive source to information-sparse sources for demand prediction (Li, Bai, Liu, Yao and Waller, 2020). Related challenges lie in data format incompatibilities as well as the inherent differences in spatial or temporal patterns. While some of the surveyed models can be used for multiple tasks, e.g., traffic flow and traffic speed prediction on the same road segment, most can only be trained for a single task at one time. Multi-task forecasting is a bigger challenge in graph-based modeling because different tasks may use different graph structures, e.g., roadlevel and station-level problems use different graphs and thus are difficult to be solved with a single GNN model. Some efforts that have been made in GNN-based models for multi-task prediction include taxi departure flow and arrival flow (Chen, Zhao et al, 2020), region-flow and transition-flow (Wang, Xu et al, 2020), crowd flows, and OD of the flows (Wang, Miao et al, 2020). However, most of the existing attempts are based on the same graph with multiple outputs generated by feed forward layers. Nonetheless, GNN-based multi-task prediction for different types of traffic forecasting problems is a research direction requiring significant further development, especially those requiring multiple graph structures.*

对于智能交通系统（ITS）的公共服务运营，使用一个多任务框架是必要的，以同时整合所有的交通信息并预测多种交通方式的需求。例如，提出了知识适应（knowledge adaption）的方法，用于将信息密集源的相关知识调整到信息稀疏的源头，用于需求预测（Li, Bai, Liu, Yao和Waller, 2020）。相关挑战在于数据格式的不兼容性以及空间或时间模式固有的差异。虽然一些调查过的模型可以用于多个任务，例如，在同一道路段上进行交通流量和交通速度的预测，但大多数只能一次训练一个单一任务。

在基于图的建模中，多任务预测是一个更大的挑战，因为不同的任务可能使用不同的图结构，例如，道路级别和站点级别的问题使用不同的图，因此很难用单个GNN模型解决。一些在基于GNN的多任务预测中所做的努力包括出租车出发流量和到达流量（Chen, Zhao等，2020），区域流量和转移流量（Wang, Xu等，2020），人群流动和流量的OD（Wang, Miao等，2020）。然而，大多数现有尝试是基于相同图形的，通过前馈层生成多个输出。尽管如此，基于GNN的针对不同类型交通预测问题的多任务预测仍是需要重要进一步发展的研究方向，特别是那些需要多种图结构的情况。

#### 6.1.3 实际实施、

#### 6.1.4 模型解释

### 6.2 未来方向

