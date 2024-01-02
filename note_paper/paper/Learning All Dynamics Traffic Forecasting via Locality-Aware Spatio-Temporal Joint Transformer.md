# Learning All Dynamics: Traffic Forecasting via Locality-Aware Spatio-Temporal Joint Transformer

学习所有动态：通过具有地域感知时空联合Transformer进行交通预测

### 摘要

*Abstract— Forecasting traffic flow and speed in the urban is important for many applications, ranging from the intelligent navigation of map applications to congestion relief of city management systems. Therefore, mining the complex spatio-temporal correlations in the traffic data to accurately predict traffic is essential for the community. However, previous studies that combined the graph convolution network or self-attention mechanism with deep time series models (e.g., the recurrent neural network) can only capture spatial dependencies in each time slot and temporal dependencies in each sensor, ignoring the spatial and temporal correlations across different time slots and sensors.

Besides, the state-of-the-art Transformer architecture used in previous methods is insensitive to local spatio-temporal contexts, which is hard to suit with traffic forecasting. To solve the above two issues, we propose a novel deep learning model for traffic forecasting, named Locality-aware spatio-temporal joint Transformer (Lastjormer), which elaborately designs a spatio-temporal joint attention in the Transformer architecture to capture all dynamic dependencies in the traffic data. Specifically, our model utilizes the dot-product self-attention on sensors across many time slots to extract correlations among them and introduces the linear and convolution self-attention mechanism to reduce the computation needs and incorporate local spatio-temporal information. Experiments on three real-world traffic datasets, England, METR-LA, and PEMS-BAY, demonstrate that our Lastjormer achieves state-of-the-art performances on a variety of challenging traffic forecasting benchmarks.*

摘要— 在城市中，对交通流和速度进行预测对于许多应用至关重要，从地图应用的智能导航到城市管理系统的缓解拥堵。因此，挖掘交通数据中的复杂时空相关性以准确预测交通对社区至关重要。然而，以往的研究将图卷积网络或自注意机制与深度时间序列模型（例如循环神经网络）相结合的方法仅能捕捉每个时间槽内的空间依赖性和每个传感器的时间依赖性，忽略了不同时间槽和传感器之间的时空相关性。

此外，以前方法中使用的最先进的Transformer架构对本地时空背景不敏感，这与交通预测难以适应。为解决上述两个问题，我们提出了一种新颖的交通预测深度学习模型，命名为Locality-aware spatio-temporal joint Transformer（Lastjormer），该模型在Transformer架构中巧妙设计了时空联合注意力，以捕捉交通数据中的所有动态依赖关系。具体而言，我们的模型利用跨多个时间槽对传感器进行点积自注意，提取它们之间的相关性，并引入线性和卷积自注意机制以减少计算需求并整合本地时空信息。在三个真实交通数据集（英格兰、METR-LA和PEMS-BAY）上的实验证明，我们的Lastjormer在各种具有挑战性的交通预测基准上取得了最先进的性能。