# Hybrid deep learning models for traffic prediction in large-scale road networks

大规模道路网络交通预测的混合深度学习模型

## 摘要

*Traffic prediction is an important component in Intelligent Transportation Systems(ITSs) for enabling advanced transportation management and services to address worsening traffic congestion problems. The methodology for traffic prediction has evolved significantly over the past decades from simple statistical models to recent complex integration of different deep learning models. In this paper, we focus on evaluating recent hybrid deep learning models in the task of traffic prediction. To this end, we first conducted a review and taxonomize the reviewed models based on their feature extraction methods. We analyze their constituent modules and architectural designs. We select ten models representative of different architectural choices from our taxonomy and conducted a performance comparison study. For this, we reconstruct the selected models and performed a series of comparative experiments under identical conditions with three well-known real-world datasets collected from large-scale road networks. We discuss the findings and insights based on our results, highlighting the differences in the achieved prediction accuracy by models with different design decisions.*

交通预测是智能交通系统（ITS）中的重要组成部分，可通过先进的交通管理和服务来解决日益严重的交通拥堵问题。交通预测的方法学在过去几十年中发生了显著变化，从简单的统计模型演变为最近集成不同深度学习模型的复杂模型。在本文中，我们专注于评估最近在交通预测任务中使用的混合深度学习模型。为此，我们首先进行了一项综述，并根据它们的特征提取方法对所审查的模型进行了分类。我们分析了它们的组成模块和架构设计。我们从我们的分类法中选择了十种代表不同架构选择的模型，并进行了性能比较研究。为此，我们重建了所选的模型，并在与来自大规模道路网络的三个知名真实世界数据集相同的条件下执行了一系列比较实验。我们根据结果讨论了发现和见解，突出了具有不同设计决策的模型所达到的预测准确度的差异。



## 一、介绍

*United Nations reported that currently more than half of world’s population lives in urban area and this is projected to increase to 68% by 2050 [1]. Rapid urbanization has left many countries facing various challenges in meeting the need of increasing urban citizens and sustainable development. Transportation is one such challenge. For instance, Inrix reported on average, a driver lost 134 and 133 h in Bucharest and Bogota each year due to traffic congestion [2]. Intelligent Transportation Systems (ITSs), as an integrated transportation management system, are proposed to arrest the exacerbating traffic situations. ITSs rely on accurate traffic prediction to enable services such as Advanced Traveler Information System (ATIS) to improve traffic conditions [3].*

*One of the earliest work on traffic prediction published in 1979 [4] proposed to apply Auto-Regressive Moving Average (ARMA) for shortterm traffic flow prediction. Since then, many work ensued and the prediction methodology has evolved over time. In this paper, we broadly synthesize the evolution of traffic prediction methodologies into three main stages. In the first (early) stage, statistical methods such as ARMA and its variants [5,6] are proposed with the problem modeled as a pure time series process. These methods are commonly used in small and relatively simple traffic systems. They are not capable of handling traffic data with high dimensions and non-linear relationships.*

联合国报告称，目前超过一半的全球人口居住在城市地区，预计到2050年将增加到68% [1]。快速城市化使许多国家面临满足不断增长的城市居民和可持续发展需求的各种挑战，其中交通是一个重要挑战。例如，Inrix报告称，由于交通拥堵，司机每年在布加勒斯特和波哥大平均失去134和133小时 [2]。智能交通系统（ITS）作为综合交通管理系统，被提出用来缓解不断恶化的交通状况。ITS依赖于准确的交通预测，以实现诸如高级旅行者信息系统（ATIS）等服务，以改善交通状况[3]。

关于交通预测的最早研究之一发表于1979年[4]，提出应用自回归滑动平均（ARMA）进行短期交通流预测。此后，许多工作陆续出现，预测方法随时间演变。在本文中，我们广泛总结了交通预测方法的演变，划分为三个主要阶段。在第一（早期）阶段，提出了统计方法，如ARMA及其变种[5,6]，问题被建模为纯粹的时间序列过程。这些方法通常用于小型和相对简单的交通系统。它们无法处理高维度和非线性关系的交通数据。

*The second stage emerged following the popularity of machine learning models in various fields such as pattern recognition [7,8], image classification [9,10] and natural language processing [11,12]. Machine learning models with non-linear kernels and activation functions such as Support Vector Regression (SVR) [13], 𝐾-Nearest Neighbors (KNN) [14], Artificial Neural Network (ANN) [15] and Bayesian Networks [16] were then used for solving traffic prediction problem. These models mostly treat traffic prediction as multiple classification problem. Following new sensor technologies allowing collection of richer traffic data, [17] found that road traffic has complex spatial– temporal correlation and these machine learning models are shallow and inadequate for analyzing such spatial–temporal relationships.*

第二阶段出现在机器学习模型在各个领域的普及，如模式识别[7,8]、图像分类[9,10]和自然语言处理[11,12]。

具有非线性核和激活函数的机器学习模型，例如支持向量回归（SVR）[13]、𝐾-最近邻（KNN）[14]、人工神经网络（ANN）[15]和贝叶斯网络[16]，随后被用于解决交通预测问题。

这些模型大多将交通预测视为多分类问题。随着新的传感器技术允许收集更丰富的交通数据，[17]发现道路交通具有复杂的时空相关性，而这些机器学习模型对于分析这种时空关系来说显得较为肤浅和不足够。

*Then, the third stage emerged where deep learning models are applied for solving traffic prediction problems. With increasing computational resource and the development of sophisticated deep learning models, the focus of the problem has also shifted from predicting traffic states at specific road station/segment to large-scale road networks. Expanding the scope of predictions to the entire road network has made the problem more challenging since network-wide traffic data has much more complicated spatial–temporal relationship [18]. Initial deep learning models such as Recurrent Neural Networks (RNN) and its variants (Long-Short Term Memory (LSTM) and Gated Recurrent Unit (GRU)) [19], Convolutional Neural Network (CNN) [20], and Graph Convolutional Neural Network (GCN) [21], are unable to fully analyze such spatial–temporal dependencies of traffic data hidden in large-scale road networks.*

接着，第三阶段出现，深度学习模型被应用于解决交通预测问题。随着计算资源的增加和复杂深度学习模型的发展，问题的重点也从在特定道路站点/段上预测交通状态转移到了大规模道路网络。

将预测范围扩大到整个道路网络使问题变得更加具有挑战性，因为整个网络范围内的交通数据具有更为复杂的时空关系[18]。最初的深度学习模型，如循环神经网络（RNN）及其变种（长短时记忆网络（LSTM）和门控循环单元（GRU））[19]、卷积神经网络（CNN）[20]和图卷积神经网络（GCN）[21]，无法充分分析隐藏在大规模道路网络中的交通数据的这种时空依赖关系。

*Following the above, the evolution of the methodology continues with hybrid deep learning models1 emerging in the latest literature.*

*These models are constructed by combining several deep learning models to further improve prediction accuracy. In this paper, we focus on the performance of these latest hybrid deep learning models rather than the entire evolution of the traffic prediction methodology. We point interested readers to existing surveys on previous evolution. For instance, [22–24] reviewed models focusing on addressing short-term traffic prediction only. On the other hand, [25–27] targeted more classical approaches including statistical-based and machine learning (ML)-based models while [24,28,29] focused on deep learning models.*

*We also note another work [30] which reviewed traffic prediction from the perspective of smart cities and highlighted existing traffic data sources and data models for fusing traffic data in ITSs. Furthermore, [31] explored and explained different GCNs, including graph convolutional and graph attention networks, for solving various traffic prediction problems (i.e., road traffic flow and speed predictions, network-wide traffic flow and speed predictions, and traffic demand prediction) in detail. Meanwhile, [32] surveyed and categorized deep learning models for urban traffic prediction into three: grid-based, graph-based, and multivariate time-series models. The authors provided benchmarks to evaluate the performances of the selected models.*

*We have adopted a different approach and focused on hybrid deep learning models. We create a different taxonomy (see Section 3 and further analyze and evaluate representative models of different category of our taxonomy in terms of architecture designs, Mathematical foundations and performances.*

继上述过程之后，方法学的演变在最新的文献中继续出现了混合深度学习模型。

这些模型通过结合几个深度学习模型来进一步提高预测准确性。在本文中，我们关注这些最新混合深度学习模型的性能，而不是整个交通预测方法学的演变。我们将对先前演变的相关内容感兴趣的读者指向现有的调查。例如，[22–24]审查了专注于解决短期交通预测的模型。另一方面，[25–27]针对更经典的方法，包括基于统计和基于机器学习（ML）的模型，而[24,28,29]则专注于深度学习模型。

我们还注意到另一项工作[30]从智能城市的角度审查了交通预测，并突出显示了现有的交通数据来源和在ITS中融合交通数据的数据模型。此外，[31]详细探讨和解释了不同的图神经网络，包括图卷积网络和图注意网络，用于解决各种交通预测问题（即道路交通流和速度预测，全网络交通流和速度预测以及交通需求预测）。同时，[32]对城市交通预测的深度学习模型进行了调查和分类，分为基于网格的、基于图的和多变量时间序列模型。作者提供了用于评估所选模型性能的基准。

我们采用了不同的方法，专注于混合深度学习模型。我们创建了一个不同的分类法（参见第3节），并根据体系结构设计、数学基础和性能对我们分类法中不同类别的代表性模型进行了进一步的分析和评估。

Our work here then are twofold, a specific targeted review of the hybrid deep learning models and a comprehensive performance comparison study. The main contributions of this work are summarized as follows: 

+ We review and taxonomize hybrid deep learning models for traffic prediction in the literature.

+ We analyze the common architectural choices and the constituent sub-models of these hybrid models based on mathematical theories. Ten representative models are then selected for the model architecture analysis in detail based on our taxonomy.

+ We conduct extensive comparative experiments on the selected models. The evaluation results presented in the literature use different settings and datasets. It is then difficult to compare their performance under common conditions. As such, we reconstruct the models and compare their performance fairly under identical experiment setup and datasets. In our experiments, we use datasets with different traffic profiles including both frequent and infrequent congestion from real road networks. We consider both short- and long-term traffic prediction tasks.

+ Furthermore, to facilitate future research, we also collected and summarized publicly available traffic datasets frequently used in the literature

我们在这里的工作有两个方面，一是对混合深度学习模型的具体有针对性的审查，二是一项全面的性能比较研究。这项工作的主要贡献总结如下：

- 我们回顾和分类文献中用于交通预测的混合深度学习模型。
- 我们基于数学理论分析了这些混合模型的常见架构选择和组成子模型。然后，我们根据我们的分类法详细选择了十个代表性模型进行模型架构分析。
- 我们对所选的模型进行了广泛的比较实验。文献中呈现的评估结果使用了不同的设置和数据集。因此，很难在相同的条件下比较它们的性能。因此，在相同的实验设置和数据集下，我们重建了模型并公平比较它们的性能。在我们的实验中，我们使用了包括真实道路网络中的频繁和不频繁拥堵在内的不同交通配置文件的数据集。我们考虑了短期和长期的交通预测任务。
- 此外，为了促进未来的研究，我们还收集并总结了文献中经常使用的公开交通数据集。

*The rest of this paper is organized as follows. First, in Section 2, we present a generalized traffic prediction problem formulation based on existing work. Next, in Section 3, we review the latest hybrid deep learning models from recent literature and create a taxonomy with three main types of models: (1) CNN-based models, (2) GCNbased models and (3) transformer-based models. In this section, we discuss the evolution of traffic prediction models from CNN-based models to GCN-based models. In Section 4, we synthesize the common main modules exploited in recent hybrid deep learning models and review the fundamentals of these modules individually. Then, we select ten representative hybrid deep learning models for deep architecture analysis in Section 5. Section 6 summarizes commonly used public data sources for traffic prediction to help researchers further develop more valuable works while Section 7 presents our comparative performance evaluation experiments in which we reconstructed the selected models and evaluate them under equal conditions using three large traffic datasets from real-world road networks. Finally, we conclude our work and discuss future challenges of traffic prediction problem on large-scale road networks in Section 8.*

本文的其余部分安排如下。首先，在第2节中，我们基于现有工作提出了一个概括的交通预测问题表述。接下来，在第3节中，我们回顾了最新的混合深度学习模型，并创建了一个包括三种主要类型的模型的分类法：（1）基于CNN的模型，（2）基于GCN的模型和（3）基于transformer的模型。在这一节中，我们讨论了交通预测模型从基于CNN的模型到基于GCN的模型的演变。在第4节中，我们综合了最近混合深度学习模型中利用的常见主要模块，并逐个回顾了这些模块的基础知识。然后，在第5节中，我们选择了十个代表性的混合深度学习模型进行深度架构分析。第6节总结了交通预测中常用的公共数据来源，以帮助研究人员进一步开发更有价值的作品，而第7节则呈现了我们的比较性能评估实验，在这些实验中，我们重建了所选的模型，并在相同的条件下使用来自真实道路网络的三个大型交通数据集对它们进行评估。最后，在第8节中，我们总结了我们的工作并讨论了未来在大规模道路网络上进行交通预测问题的挑战。





## 三、分类

*We present a summary of the hybrid deep learning models in Table 1 according to their main constituent models and further segregated chronologically by year of publication. We also show the prediction task (either predicting traffic flow, speed and/or occupancy), the prediction horizon considered and the dataset(s) used in these works. From our review as well as insights from [18,39–41], existing hybrid deep learning models commonly consist of two main modules, respectively for analyzing spatial and temporal dependencies. Furthermore, these models commonly contain CNN or GCN for spatial dependency analysis and LSTM or GRU for temporal dependency analysis. To facilitate the building of the taxonomy, we further define the following:*

我们根据主要组成模型以及按出版年份的先后顺序，在表格1中总结了混合深度学习模型。我们还展示了这些工作中考虑的预测任务（是否预测交通流、速度和/或占用），考虑的预测时段以及使用的数据集。根据我们的审阅以及来自[18,39–41]的见解，现有的混合深度学习模型通常包括两个主要模块，分别用于分析空间和时间依赖性。此外，这些模型通常包含用于空间依赖性分析的CNN或GCN以及用于时间依赖性分析的LSTM或GRU。为了便于建立分类法，我们进一步定义如下：

Definition 1 (Fixed Spatial–Temporal Feature). The fixed spatial– temporal feature does not consider the different influences of:

+ different road segments on the targeted road segment in space domain, and 

+ different previous time intervals on the targeted time interval in time domain.

定义1（固定时空特征）。固定时空特征不考虑以下因素的不同影响：

+ 空间域内不同道路段对目标道路段的影响

+ 时间域内不同先前时间间隔对目标时间间隔的影响。

Definition 2 (Dynamic Spatial–Temporal Feature). The dynamic spatial– temporal feature considers both

+ the different road segments’ contributions to the targeted road segment in space domain, and/or 

+ the different contributions of previous time intervals to the targeted time interval in time domain.

定义2（动态时空特征）。动态时空特征考虑以下两个方面： 

+ 空间域内不同道路段对目标道路段的影响
+ 时间域内不同先前时间间隔对目标时间间隔的影响。

*From the above, we first classify the reviewed models into three categories based on the technologies used for analyzing spatial dependencies: (1) CNN-based models, (2) GCN-based models, and (3) transformer-based models. We then further segregate them based on Definitions 1 and 2. Our taxonomy is shown in Fig. 1. In the following, we detail our taxonomy.*

从上面的内容中，我们首先根据用于分析空间依赖性的技术将研究过的模型分为三类：

（1）基于CNN的模型

（2）基于GCN的模型

（3）基于transformer的模型

然后，我们根据定义1和定义2进一步将它们分开。我们的分类法如图1所示。接下来，我们详细说明我们的分类法。

### 3.1 基于CNN的模型

*The CNN model has been applied in different fields including sentence classification [72], image classification [73], video classification [74] and human action recognition [75]. It extracts local spatial features via its convolutional kernels. CNN was introduced into traffic prediction solutions specifically for processing spatial correlation [76].*

*For example, [77] exploited CNN to predict traffic speed on largescale road networks by representing traffic data as images. However, since CNN focuses on extracting spatial feature, [77] has neglected the importance of temporal features. As such, to supplement CNN, LSTM and GRU are often combined with CNN to form hybrid deep learning models for extracting temporal features (cf. Table 1). These CNN-based hybrid models can be further divided into two main approaches as follows:*

卷积神经网络（CNN）已经应用于不同的领域，包括句子分类[72]、图像分类[73]、视频分类[74]和人体动作识别[75]。它通过卷积核提取局部空间特征。CNN被引入交通预测解决方案中，特别是用于处理空间相关性[76]。

例如，[77]利用CNN预测大规模道路网络上的交通速度，将交通数据表示为图像。然而，由于CNN专注于提取空间特征，[77]忽视了时间特征的重要性。因此，为了补充CNN，通常将LSTM和GRU与CNN结合形成混合深度学习模型以提取时间特征（参见表1）。这些基于CNN的混合模型可以进一步分为两种主要方法：

#### 3.1.1 固定时空特征的模型

*CNN-based hybrid deep learning models based on fixed spatial– temporal feature modeling consider the spatial–temporal dependencies of traffic data being fixed via sharing of parameters. Hence, the traffic states of different neighboring road segments in the space domain are considered to have the same effect on the traffic state of the targeted road segment while, in time domain, traffic data in different previous time intervals also affect traffic state in the future time interval in the same level.*

基于固定时空特征建模的基于CNN的混合深度学习模型通过参数共享考虑交通数据的时空依赖关系是固定的。因此，在空间域内，不同相邻道路段的交通状态被认为对目标道路段的交通状态有相同的影响，而在时间域内，不同的先前时间间隔的交通数据也以相同级别影响未来时间间隔内的交通状态。

*In terms of extracting fixed spatial–temporal features for traffic prediction, [42] used a 1D CNN and two LSTMs to build a hybrid deep learning model named CLTFP. The 1D CNN is used to extract inner spatial dependencies of the road network. One LSTM is used to capture short-term temporal features from previous hours while the other LSTM is utilized to extract periodic features from past days and weeks. The spatial, short-term temporal and periodic features are fused into a feature vector and then sent to a regression layer to perform predicting. Two similar models (named TreNet and U-Net) were developed by [50,57], respectively. Compared to [42,50,57] that extract spatial and temporal features using CNN and LSTM, respectively, [43] combined CNN and LSTM to generate a Conv-LSTM module for spatial–temporal feature modeling and also adopt a Bi-LSTM to analyze periodical features from historical traffic data. Considering the influence of external factors on traffic prediction, such as weather conditions and the physical characteristics of roads, [52] developed the DELA model. This model not only includes an integrated model composed of CNN and LSTM for spatial–temporal feature extraction but also contains a fully-connected layer based embedding component for learning external factors such as route structure, weather conditions and date information.*

在提取固定时空特征以进行交通预测方面，[42] 使用了一维CNN和两个LSTM构建了一个名为CLTFP的混合深度学习模型。一维CNN用于提取道路网络的内部空间依赖关系。一个LSTM用于捕捉先前几小时的短期时间特征，而另一个LSTM则用于提取过去几天和几周的周期特征。空间、短期时间和周期特征被融合成一个特征向量，然后传递到回归层进行预测。[50,57]分别开发了两个类似的模型（名为TreNet和U-Net）。与[42,50,57]分别使用CNN和LSTM提取空间和时间特征不同，[43]结合了CNN和LSTM生成Conv-LSTM模块进行空间-时间特征建模，并采用Bi-LSTM分析历史交通数据的周期特征。考虑到外部因素对交通预测的影响，比如天气条件和道路的物理特性，[52]开发了DELA模型。该模型不仅包括由CNN和LSTM组成的综合模型进行空间-时间特征提取，还包含一个基于全连接层的嵌入组件，用于学习路线结构、天气条件和日期信息等外部因素。

*To analyze and extract more detailed features for improved prediction accuracy, [48,78] proposed models based on CNN and LSTM, namely STRCNs and ALLSCP respectively, for traffic flow prediction based on hourly, daily and weekly spatial–temporal feature extraction.*

*The differences between STRCNs and ALLSCP are that the former model feeds external factors, such as weather conditions, wind and holidays, into a fully-connected layer for external feature extraction while the latter one considers different types of roads. Specifically, ALLSCP differentiates between linear roadways and road intersections and designs different input matrices for those two types roadways when considering that traffic states on linear roadways are affected by the traffic states of upstream and downstream while, for road intersections, the traffic states are affected by the traffic states of the different entrances and exits. Another model that considered the types of roads when predicting traffic states is [45] where it firstly used a Spatial–Temporal Correlation Algorithm (STCA) to identify and extract the critical road sections and then utilized a hybrid deep learning model (named CRS-ConvLSTM NN) based on CNN and LSTM for traffic speed prediction on these critical road sections. Considering the success of CNN in the area of image recognition, some of CNN-based models proposed to convert traffic data to images and then, learn spatial–temporal features from those images. SRCNs [44] and MSTFLN [49] are two examples following this approach. Both models combine CNN and LSTM to analyze spatial– temporal features from generated traffic images. SRCNs uses CNN to learn spatial features and LSTM to learn temporal features. MSTFLN utilizes three ConvLSTM modules composed of CNN and LSTM to learn spatial–temporal features and then concatenate those features to pass to another CNN module for final prediction. Based on the encoder–decoder architecture, [53] developed a stacked autoencoder, including an encoder based on CNNs and a decoder based on BiLSTMs, for traffic flow and speed prediction. Another model (named STCNN) was proposed in [54] for predicting long-term traffic flow.*

*The encoder consists of a ConvLSTM to learn the spatial–temporal traffic dependencies and a Skip-ConvLSTM to learn the periodic traffic patterns. The decoder consists of another ConvLSTM for decoding the spatial–temporal dependencies from the output of the encoder.*

为了分析并提取更详细的特征以提高预测准确性，[48,78]分别提出了基于CNN和LSTM的模型，即STRCNs和ALLSCP，用于基于小时、日和周的空间-时间特征提取的交通流预测。

STRCNs和ALLSCP之间的区别在于前者将外部因素（如天气条件、风和假期）输入到一个全连接层中进行外部特征提取，而后者考虑了不同类型的道路。具体而言，ALLSCP区分了线性道路和道路交叉口，并在考虑到线性道路上的交通状态受到上游和下游交通状态影响的情况下，为这两种类型的道路设计了不同的输入矩阵，对于道路交叉口，交通状态受到不同入口和出口的交通状态影响。另一个在预测交通状态时考虑到道路类型的模型是[45]，它首先使用空间-时间相关算法（STCA）识别和提取关键道路段，然后利用基于CNN和LSTM的混合深度学习模型（命名为CRS-ConvLSTM NN）进行这些关键道路段的交通速度预测。考虑到CNN在图像识别领域的成功，一些基于CNN的模型提出将交通数据转换为图像，然后从这些图像中学习空间-时间特征。SRCNs [44]和MSTFLN [49]是遵循这种方法的两个例子。这两个模型都结合了CNN和LSTM，以从生成的交通图像中分析空间-时间特征。SRCNs使用CNN学习空间特征和LSTM学习时间特征。MSTFLN利用由CNN和LSTM组成的三个ConvLSTM模块学习空间-时间特征，然后将这些特征连接起来传递到另一个CNN模块进行最终预测。基于编码器-解码器架构，[53]开发了一个堆叠的自动编码器，包括一个基于CNN的编码器和一个基于BiLSTM的解码器，用于交通流和速度的预测。另一个模型（名为STCNN）是在[54]中提出的，用于预测长期交通流。编码器包括一个ConvLSTM，用于学习空间-时间交通依赖关系，以及一个Skip-ConvLSTM，用于学习周期性交通模式。解码器包括另一个ConvLSTM，用于从编码器的输出中解码空间-时间依赖关系。



#### 3.1.2 动态时空特征的模型

*In reality, different neighboring road segments impact traffic state on the targeted road segment differently. Similarly, traffic states in different previous time intervals also bring different effects to the traffic state in the future. Due to these, the attention mechanism [79], that represents a major breakthrough in the natural language processing field, was introduced into traffic prediction problems. It defines different weights in space and/or time dimensions for modeling dynamic spatial–temporal dependencies to account for the non-uniform contributions of neighboring road segments and time intervals to the final prediction. CNN-based models, that exploit attention mechanism in CNN and/or LSTM (GRU), are able to analyze dynamic spatial and/or temporal features for traffic prediction.*

在现实中，不同的相邻道路段对目标道路段的交通状态产生不同的影响。同样，不同的先前时间间隔的交通状态也对将来的交通状态产生不同的影响。由于这些原因，注意机制 [79]，在自然语言处理领域取得了重大突破，被引入到交通预测问题中。它在空间和/或时间维度上定义不同的权重，以建模动态的空间-时间依赖关系，以考虑相邻道路段和时间间隔对最终预测的非均匀贡献。基于CNN的模型，利用CNN和/或LSTM（GRU）中的注意机制，能够分析交通预测的动态空间和/或时间特征。

*Dynamic on Temporal Features: Wu et al [46] proposed the Deep Neural Network-Based Traffic Flow prediction (DNN-BTF) model, which uses the attention mechanism to select near-term data from previous time intervals that is highly correlated to the future traffic flow and then compute the corresponding weights to previous traffic flows. The weighted traffic flows in time domain help CNN and GRU to learn spatial and dynamic temporal features, respectively. Similar to [46,80] also built an attention module to generate weighted past traffic data. The weighted traffic data is first sent to CNN module for spatial feature learning and then to an LSTM module for dynamic temporal feature learning. WSTNet [51] is another model that uses the attention mechanism for dynamic temporal feature extraction. It is an end-to-end deep learning model based on wavelet multi-scale analysis for network-wide traffic prediction. WSTNet firstly decomposes original traffic data into multi-level time frequency traffic matrix at different time scales by the discrete wavelet decomposition and then applied CNN for spatial feature extraction before finally sent to the LSTM with attention mechanism for dynamic temporal feature learning.*

*Another work in [81] also joined attention mechanism with LSTM for dynamic temporal feature extraction. Specifically, [81] uses two additional LSTMs for daily and weekly periodicity analysis and this enables the model to have more temporal features for the final prediction*

动态时间特征：Wu等人 [46] 提出了基于深度神经网络的交通流量预测模型（DNN-BTF），该模型使用注意机制从先前的时间间隔中选择与未来交通流高度相关的近期数据，并计算与先前交通流相对应的权重。时间域中加权的交通流可以帮助CNN和GRU分别学习空间和动态时间特征。类似地，[46,80] 也构建了一个注意模块来生成加权的过去交通数据。加权的交通数据首先送入CNN模块进行空间特征学习，然后传送到LSTM模块进行动态时间特征学习。WSTNet [51] 是另一个利用注意机制进行动态时间特征提取的模型。它是基于小波多尺度分析的网络范围交通预测的端到端深度学习模型。WSTNet首先通过离散小波分解将原始交通数据分解为不同时间尺度的多级时间频率交通矩阵，然后应用CNN进行空间特征提取，最后传送到带有注意机制的LSTM进行动态时间特征学习。

[81] 中的另一项工作也将注意机制与LSTM结合，用于动态时间特征的提取。具体而言，[81] 使用两个额外的LSTM进行每日和每周周期性分析，使模型能够具有更多用于最终预测的时间特征。

*Dynamic on Both Spatial and Temporal Features: Yao et al [55] proposed the Spatial–Temporal Dynamic Networks (STDN) to analyze dynamic spatial dependencies by a CNN-based Flow Gating Mechanism (FGM) and dynamic temporal dependencies by integrating the selfattention mechanism into LSTM, for traffic prediction. On the other hand, [82] developed a Convo-Recurrent Attentional Neural Network (CRANN) model for traffic prediction. The idea behind CRANN is the use of classical time-series decomposition in which CRANN consists of several modules to exploit different patterns or characteristics of traffic data and then aggregates them to make predictions. Dynamic spatial and dynamic temporal features are separately extracted by a dedicated attention-based spatial and an attention-based temporal module and then concatenated with exogenous data (i.e. weather condition) via a fully-connected layer for the final prediction.*

同时动态处理时空特征：Yao等人 [55] 提出了时空动态网络（STDN），通过基于CNN的流量门控机制（FGM）分析动态的空间依赖性，同时通过将自注意机制集成到LSTM中来分析动态的时间依赖性，用于交通预测。另一方面，[82] 开发了一个用于交通预测的卷积-循环注意神经网络（CRANN）模型。CRANN的思想是使用经典的时间序列分解，其中CRANN由几个模块组成，用于挖掘交通数据的不同模式或特征，然后将它们聚合以进行预测。通过专用的基于注意力的空间模块和基于注意力的时间模块分别提取动态的空间和动态的时间特征，然后通过一个全连接层与外部数据（即天气条件）连接，用于最终的预测。



### 3.2 基于GCN的模型

*The CNN-based models in Section 3.1 consider road networks as regular grids and traffic data with regular Euclidean structure. In fact, road networks are inherently irregular and traffic data should be treated as non-Euclidean data [83]. Therefore, GCN with the advantage of dealing with non-Euclidean structured data has been introduced into the task of traffic prediction. Similar to CNN-based models, GCN-based models can be also classified into two: fixed and dynamic.*

第3.1节中的基于CNN的模型将道路网络视为规则网格，将交通数据视为具有规则欧几里得结构的数据。实际上，道路网络本质上是不规则的，交通数据应该被视为非欧几里得数据 [83]。因此，具有处理非欧几里得结构数据优势的GCN已被引入到交通预测的任务中。与基于CNN的模型类似，基于GCN的模型也可以分为两类：固定和动态。



#### 3.2.1 固定时空特征的模型

*Zhao et al [36] proposed a Temporal Graph Convolutional Network (T-GCN) model, which combines GCN and GRU for traffic speed prediction. GCN is used to learn complex topological structures from the 𝑘 − ℎ𝑜𝑝 neighborhood matrix for capturing spatial dependencies and GRU is utilized to learn changes of traffic data along the time dimension for capturing temporal dependencies. To enrich traffic information, [35] proposed the Traffic Graph Convolutional Long Short-Term Memory Neural Network (TGC-LSTM) model, based on GCN and LSTM, to learn the interactions of road segments in a large-scale road network from all 𝑘 − ℎ𝑜𝑝 neighborhood matrices. A L1-norm on graph convolution weights and a L2-norm on graph convolution features are added to the loss function for enhancing the interpretability of the model. Following the proposal of the sequence-to-sequence (Seq2Seq) architecture [84] that is capable of dealing with long-term sequence problems, several hybrid deep learning models started to exploit it for analyzing long-term traffic dependency. For example, [33] developed a deep learning framework based on Diffusion Convolutional Recurrent Neural Network (DCRNN) under the Seq2Seq framework for traffic speed prediction. Both the encoder and the decoder inside the Seq2Seq framework consist of GRUs embedded by the diffusion convolutional process. Two other hybrid models exploiting the Seq2Seq framework are [59,60]. The hybrid model in [59] incorporates offline geographical and social attributes, spatial dependencies and online crowd queries with a deep fusion. The model in [60] considers temporal attributes including public holidays, working days, peak hours and off-peak hours for contributing to final prediction in the decoder of the Seq2Seq framework. Pan et al [64] developed a deep-meta-learning model named ST-MetaNet under the Seq2Seq framework to predict networkwide traffic. Both the encoder and the decoder in ST-MetaNet have the same network structure consisting of four components: (1) basic RNN for learning long temporal dependencies, (2) Meta-knowledge learner for learning the meta-knowledge of nodes and edges from node and edge attributes respectively, (3) Meta-GAT for capturing diverse spatial correlations by individually broadcasting locations’ hidden states along edges, and (4) Meta-RNN for capturing diverse temporal correlations associated with the geographical information of locations.*

赵等人 [36] 提出了一种时间图卷积网络（T-GCN）模型，该模型结合了GCN和GRU进行交通速度预测。GCN用于从𝑘-𝑛𝑒𝑖𝑔ℎ𝑏𝑜𝑟ℎ𝑜𝑜𝑑邻域矩阵中学习复杂的拓扑结构，以捕捉空间依赖性，而GRU用于沿时间维度学习交通数据的变化，以捕捉时间依赖性。为了丰富交通信息，[35] 提出了基于GCN和LSTM的交通图卷积长短时记忆神经网络（TGC-LSTM）模型，用于从所有𝑘-𝑛𝑒𝑖𝑔ℎ𝑏𝑜𝑟ℎ𝑜𝑜𝑑邻域矩阵中学习大规模道路网络中道路段之间的交互作用。对图卷积权重进行L1范数和对图卷积特征进行L2范数，以增强模型的可解释性。

在处理长期序列问题的Seq2Seq体系结构 [84] 提出之后，一些混合深度学习模型开始利用它来分析长期交通依赖性。例如，[33] 基于Seq2Seq框架开发了一个基于扩散卷积循环神经网络（DCRNN）的深度学习框架，用于交通速度预测。Seq2Seq框架内部的编码器和解码器都包含由扩散卷积过程嵌套的GRU。利用Seq2Seq框架的另外两个混合模型是 [59,60]。[59] 中的混合模型通过深度融合将离线地理和社会属性、空间依赖性和在线群体查询纳入其中。[60] 中的模型在Seq2Seq框架的解码器中考虑了包括公共假期、工作日、高峰时段和非高峰时段在内的时间属性，用于最终预测。潘等人 [64] 在Seq2Seq框架下开发了一种名为ST-MetaNet的深度元学习模型，用于预测整个网络的交通情况。ST-MetaNet中的编码器和解码器具有相同的网络结构，包括四个组件：（1）基本的RNN用于学习长时间依赖关系，（2）元知识学习器用于分别从节点和边缘属性中学习节点和边缘的元知识，（3）元-GAT用于通过单独广播位置的隐藏状态来捕捉多样的空间相关性，（4）元-RNN用于捕捉与位置的地理信息相关的多样的时间相关性。



#### 3.2.2 动态时空特征的模型

*Fixed GCN-based models address the traffic prediction problem as a fixed spatial–temporal process. Similarly to dynamic CNN-based models, we also find dynamic GCN-based models in the literature that treat the traffic prediction problem as a dynamic spatial–temporal process via introduction of the attention mechanism, due to considering the fact that different neighboring sensors or road segments and different previous time steps individually affect differently the targeted road segment and the future time intervals, respectively.*

固定的GCN模型将交通预测问题视为一个固定的时空过程。类似于动态CNN模型，文献中还存在将交通预测问题视为动态时空过程的动态GCN模型，通过引入注意机制来考虑不同的邻近传感器或道路段以及不同的先前时间步对目标道路段和未来时间间隔的影响。

*Dynamic on Spatial Features: The SAGCN-SST model [69] that is designed for multi-interval network-wide traffic speed prediction falls within this category. It combines the attention mechanism into GCN layers for analyzing dynamic spatial dependencies in different neighborhoods and uses GRU under an encoder–decoder architecture for analyzing temporal features. The Dynamic Graph Convolutional Recurrent Network (DGCRN) model [85] is another model in this category. In DGCRN, the node embedding technology [70] is used to embed the node attributes and then sent to a hyper-network to generate dynamic graph at each time interval. GCN and GRU under an encoder–decoder architecture are then utilized to respectively capture spatial and temporal features based on generated dynamic graphs and their historical traffic data. Meanwhile, [86] built a graph neural network architecture, named Graph WaveNet, to exploit dynamic and hidden spatial features by using a novel adaptive dependency matrix.It captures long-term temporal features by temporal convolution layers consisting of the dilated causal convolution [87].*

GCN模型中的动态空间特征：SAGCN-SST模型 [69] 是专为多时间间隔网络范围内交通速度预测而设计的。该模型将注意机制与GCN层结合起来，以分析不同邻域的动态空间依赖性，并在编码器-解码器架构下使用GRU分析时态特征。动态图卷积循环网络 (DGCRN) 模型 [85] 是此类别中的另一个模型。在DGCRN中，节点嵌入技术 [70] 用于嵌入节点属性，然后发送到一个超网络以在每个时间间隔生成动态图。接着，在编码器-解码器架构下使用GCN和GRU来分别捕捉基于生成的动态图和它们的历史交通数据的时空特征。同时，[86] 构建了一个名为Graph WaveNet的图神经网络架构，通过使用一种新颖的自适应依赖矩阵来开发动态和隐藏的空间特征。它通过由扩张因果卷积 [87] 组成的时态卷积层捕捉长期时态特征。

*Dynamic on Temporal Features: Li et al [63] proposed a graph and attention-based long short-term memory network (named GLA), to capture the spatial–temporal features of traffic flow data. GLA uses GCN to mine the spatial relationships of traffic data, and then the output of GCN is fed to LSTM with the soft attention mechanism for the dynamic temporal feature extraction. Another model, the AGCSeq2Seq-Att [34], also joins the attention mechanism into LSTM for dynamic temporal feature analysis and uses GCN for spatial feature analysis on the adjacent matrix. He et al [88] built a Graph Attention Spatial–Temporal Network (GASTN) model for citywide mobile traffic prediction. Compared to [34,63,88] adopts Dynamic Time Warping (DTW) algorithm [89] to calculate the similarities of traffic data between two nodes, and then clusters nodes into different groups and builds weighted graph based on computed similarities of these groups, before using GCN and attention-based RNN to extract spatial and dynamic temporal features*

GCN模型中的动态时态特征：Li等人 [63] 提出了一种基于图和注意力机制的长短时记忆网络（命名为GLA），用于捕捉交通流数据的时空特征。GLA利用GCN挖掘交通数据的空间关系，然后将GCN的输出馈送到带有软注意力机制的LSTM中进行动态时态特征提取。另一个模型，AGCSeq2Seq-Att [34]，也将注意力机制引入LSTM中进行动态时态特征分析，并在相邻矩阵上使用GCN进行空间特征分析。He等人 [88] 构建了一个城市范围移动交通预测的图注意力时空网络（GASTN）模型。与 [34,63,88] 相比，GASTN采用了动态时间规整（DTW）算法 [89] 来计算两个节点之间交通数据的相似性，然后将节点聚类到不同的组中，并基于这些组的计算相似性构建加权图，最后使用GCN和基于注意力的RNN提取空间和动态时态特征。

*Dynamic on Both Spatial and Temporal Features: Shi et al [90] designed an Attention-based Periodic-Temporal Neural Network (APTN) to learn dynamic spatial and dynamic temporal features for traffic flow prediction. APTN is built based on an encoder–decoder architecture, in which an LSTM-based encoder with the attention mechanism is used to capture dynamic spatial correlations of each node with the entire graph by learning individual weights and the other LSTM-based decoder with the attention mechanism is utilized to decode the encoded information. It also adaptively select the relevant encoder hidden states with individual weights to produce the output. The Spatial and Temporal Attention based Neural Network (STANN) [62] using an encoder–decoder architecture based on CNN on traffic graphs and GRU is another work in this category. The difference of this work compared to [90] is that the spatial attention matrices used to represent the relationships of road segments in a traffic network are generated before using the STANN model with attention mechanism to extract both dynamic spatial and dynamic temporal features. Yin et al [68] proposed a Multi-stage Attention Spatial–Temporal Graph Network (MASTGN) that captures dynamic temporal features via the interactions among multiple time series by an internal attention mechanism and extracts dynamic spatial features by a dynamic neighborhood-based attention mechanism.*

动态时空特征方面：Shi等人 [90] 设计了一种基于注意力的周期时态神经网络（APTN），用于学习交通流预测的动态时空特征。APTN建立在一个编码器-解码器架构之上，其中使用基于LSTM的编码器和注意力机制来捕捉每个节点与整个图的动态空间相关性，通过学习个体权重，并使用基于LSTM的解码器和注意力机制来解码编码信息。它还自适应地选择相关的编码器隐藏状态，以产生输出。基于空间和时间注意力的神经网络（STANN）[62] 使用基于CNN的编码器-解码器架构在交通图上和GRU上进行，是这一类工作中的另一项研究。与 [90] 相比，这项工作的不同之处在于在使用STANN模型和注意机制从交通网络中提取动态时空特征之前，用于表示交通网络中道路段关系的空间注意矩阵是在之前生成的。Yin等人 [68] 提出了一个多阶段注意力时空图网络（MASTGN），通过内部注意力机制在多个时间序列之间捕捉动态时态特征，并通过动态邻域注意力机制提取动态空间特征。



### 3.3 基于 transformer 的模型

*Inspired by the newly proposed transformer in [79] for efficiently modeling long-range dependencies in natural language processing, researchers have introduced the transformer architecture to replace CNN (or GCN) for spatial correlation analysis and LSTM (or GRU) for temporal dependency analysis. Self-attention-based transformer can directly learn dynamic spatial and temporal dependencies by distributing different weights on neighbors in space dimension and on previous time intervals in time dimension for contributing to the targeted road segment. Besides, transformer can exploit more hidden information in traffic data by defining multi-heads and accelerate training phase by paralleling process.*

受到 [79] 中新提出的 transformer 用于高效建模自然语言处理中的长距离依赖关系的启发，研究人员引入了 transformer 架构来替代 CNN（或 GCN）进行空间相关性分析和 LSTM（或 GRU）进行时空依赖分析。基于自注意力的 transformer 可以通过在空间维度上分配不同的权重和在时间维度上分配不同的权重来直接学习动态的时空依赖关系，从而有助于针对性地改善道路段。此外，transformer 可以通过定义多个头部来挖掘交通数据中的更多隐藏信息，并通过并行处理加速训练阶段。

#### 3.3.1 动态时空特征的模型

*Dynamic on Both Spatial and Temporal Features: Xu et al [71] developed a Spatial–Temporal Transformer Networks (STTNs) to improve the accuracy of long-term traffic prediction, which consists of spatial transformer for modeling dynamic spatial dependencies with self-attention mechanism and temporal transformer for modeling dynamic long-range temporal dependencies across previous time intervals. Another transformer-based model (named GMAN) was developed under an encoder–decoder architecture in [70]. Both encoder and decoder consist of multiple spatial–temporal attention blocks to model the impact of the spatial–temporal factors on traffic state. In addition, it also includes a spatial–temporal embedding to encode vertices into vectors using the node2vec approach [91] for the vertex representation learning.*

动态时空特征：徐等人 [71] 开发了一种名为空间-时间 Transformer 网络（STTNs）的模型，以提高长期交通预测的准确性。该模型包括用于建模动态时空依赖关系的带有自注意机制的空间变换器和用于建模跨先前时间间隔的动态长时序依赖关系的时空变换器。另一个基于 Transformer 的模型（名为 GMAN）在 [70] 中以编码器-解码器架构开发。编码器和解码器都包括多个空间-时间注意块，以建模时空因素对交通状态的影响。此外，它还包括一个空间-时间嵌入，使用 node2vec 方法 [91] 对顶点进行编码，以学习顶点表示。