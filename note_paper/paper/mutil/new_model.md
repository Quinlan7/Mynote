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
    > | 模型\结果         | MAE     | RMSE    | MAPE   |
    > | ----------------- | ------- | ------- | ------ |
    > | DMFFGCN           | 1.5145  | 5.3820  | 0.0454 |
    > | DMFFGCN w/o H     | 1.5949  | 5.5003  | 0.0469 |
    > | DMFFGCN w/o T     | 2.4077  | 8.9624  | 0.0563 |
    > | DMFFGCN w/o C     | 1.6443  | 5.7489  | 0.0527 |
    > | DMFFGCN w/o W     | 1.8200  | 6.7797  | 0.0513 |
    > | DMFFGCN w/o A     | 1.6940  | 6.0739  | 0.0473 |
    > | DMFFGCN-2000epoch | 1.1035  | 3.5433  | 0.0350 |
    > |                   |         |         |        |
    > | 模型              | MAE     | RMSE    | MAPE   |
    > | AGCRN             | 18.7526 | 85.9551 | 0.3256 |
    > | ASTGCN            | 2.7008  | 12.5958 | 0.0834 |
    > | DGCNN             | 4.6817  | 19.2657 | 0.1762 |
    > | FC-LSTM           | 19.5344 | 84.7473 | 0.3871 |
    > | GMAN              | 7.7733  | 22.0913 | 0.2315 |
    > | Graph WaveNet     | 7.2982  | 30.7311 | 0.2789 |
    > | MSTIF-Net         | 7.0468  | 31.4890 | 0.2943 |
    > | STFGNN            | 8.5211  | 40.6801 | 0.3328 |
    > | STGCN             | 19.7796 | 84.9773 | 0.4217 |
    > | VAR               | 6.2802  | 26.9623 | 0.2198 |
    > | MISTAGCN          | 2.7752  | 10.3244 | 0.1357 |
    >
    > 
    >
    > HaiKou-ablation
    >
    > + MFFDSTGCN-850epoch-noMFFmoudle：在这个实验中我们移除掉多模态特征融合模块（图卷积部分），只是把提取完特征的交通流信息和多模态信息叠加到一起
    > + MFFDSTGCN-850epoch-noMImoudle：在这个实验中，我们移除掉专门处理多模态信息的并行模块，我们直接把辅助信息叠加到我们的交通流信息上，来验证这个并行模块的作用
    >
    > | 模型\结果        | MAE    | RMSE   | MAPE   |
    > | ---------------- | ------ | ------ | ------ |
    > | DMFFGCN          | 1.5145 | 5.3820 | 0.0454 |
    > | DMFFGCN w/o DMFF | 1.7550 | 6.1579 | 0.0495 |
    > | DMFFGCN w/o DG   | 2.2108 | 7.7609 | 0.0544 |
    >
    > 
    >
    > 不同预测步长的对比      
    >
    > tp = 3                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           
    >
    > | 模型\结果     | MAE    | RMSE    | MAPE   |
    > | ------------- | ------ | ------- | ------ |
    > | DMFFGCN       | 1.5145 | 5.3820  | 0.0454 |
    > | ASTGCN        | 2.7008 | 12.5958 | 0.0834 |
    > | DGCNN         | 4.6817 | 19.2657 | 0.1762 |
    > | VAR           | 6.2802 | 26.9623 | 0.2198 |
    > | Graph WaveNet | 7.2982 | 30.7311 | 0.2789 |
    > | STFGNN        | 8.5211 | 40.6801 | 0.3328 |
    >
    > 
    >
    > 非对齐多模态特征的实验
    >
    > | 模型\结果       | MAE    | RMSE   | MAPE   |
    > | --------------- | ------ | ------ | ------ |
    > | DMFFGCN (100%)  | 1.5145 | 5.3820 | 0.0454 |
    > | DMFFGCN (0%)    | 2.7948 | 9.3546 | 0.0755 |
    > | DMFFGCN (25.0%) | 1.9052 | 8.0519 | 0.0589 |
    > | DMFFGCN (50.0%) | 1.6977 | 6.2440 | 0.0481 |
    > | DMFFGCN (75.0%) | 1.5529 | 5.6233 | 0.0468 |
  
    
  
    
  
    
  
    
  
    
  
    
  
    > Yellowtrip
    >
    > | 模型\结果     | MAE     | RMSE    | MAPE   |
    > | ------------- | ------- | ------- | ------ |
    > | MFF-850epoch  | 1.5579  | 3.8317  | 0.0411 |
    > | AGCRN         | 14.9889 | 48.5508 | 0.2356 |
    > | ASTGCN        | 3.6524  | 12.2728 | 0.0923 |
    > | DGCNN         | 13.1800 | 28.7546 | 0.1874 |
    > | FC-LSTM       | 17.0592 | 47.7628 | 0.2238 |
    > | GMAN          | 4.4055  | 13.0637 | 0.1087 |
    > | Graph WaveNet | 5.1916  | 16.1632 | 0.1245 |
    > | MSTIF-Net     | 5.9649  | 19.3907 | 0.1467 |
    > | STFGNN        | 5.7983  | 18.9360 | 0.1389 |
    > | STGCN         | 15.3981 | 48.2825 | 0.2412 |
    > | VAR           | 6.6032  | 21.3811 | 0.1578 |
    > | MISTAGCN      | 2.1834  | 6.2657  | 0.0543 |
    >
    > 
    >
    > 消融实验
    >
    > | 模型\结果        | MAE    | RMSE   | MAPE   |
    > | ---------------- | ------ | ------ | ------ |
    > | author           | 2.1834 | 6.2657 | 0.0543 |
    > | DMFFGCN          | 1.5579 | 3.8317 | 0.0411 |
    > | DMFFGCN w/o DMFF | 1.7653 | 4.5872 | 0.0467 |
    > | DMFFGCN w/o DG   | 2.0987 | 5.3421 | 0.0549 |
    >
    > 
    >
    > 多模态特征有效性验证实验
    >
    > | 模型\结果     | MAE    | RMSE   | MAPE   |
    > | ------------- | ------ | ------ | ------ |
    > | DMFFGCN       | 1.5579 | 3.8317 | 0.0411 |
    > | DMFFGCN w/o H | 1.6448 | 3.9223 | 0.0423 |
    > | DMFFGCN w/o T | 2.5334 | 5.6393 | 0.0495 |
    > | DMFFGCN w/o C | 1.7205 | 4.0833 | 0.0458 |
    > | DMFFGCN w/o W | 1.9174 | 4.5443 | 0.0441 |
    > | DMFFGCN w/o A | 1.7072 | 4.2283 | 0.0433 |
    >
    > 
    >
    > 非对齐多模态特征实验
    >
    > | 模型\结果       | MAE    | RMSE   | MAPE   |
    > | --------------- | ------ | ------ | ------ |
    > | DMFFGCN (100%)  | 1.5579 | 3.8317 | 0.0411 |
    > | DMFFGCN (0%)    | 2.7227 | 7.2687 | 0.0682 |
    > | DMFFGCN (25.0%) | 1.9015 | 4.9868 | 0.0507 |
    > | DMFFGCN (50.0%) | 1.7428 | 4.1325 | 0.0462 |
    > | DMFFGCN (75.0%) | 1.6345 | 3.8350 | 0.0424 |
  
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

## 二、第二个模型 MFFST-RetNet

![image-20241205093801899](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202412050938999.png)

![image-20241205093825700](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202412050938834.png)

![image-20241205093900127](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202412050939222.png)

**实验结果**

> HaiKou
>
> | 模型\结果         | MAE     | RMSE    | MAPE   |
> | ----------------- | ------- | ------- | ------ |
> | MFFST-RetNet      | 1.1035  | 3.5433  | 0.0488 |
> | DMFFGCN w/o H     | 1.5949  | 5.5003  | 0.0469 |
> | DMFFGCN w/o T     | 2.4077  | 8.9624  | 0.0563 |
> | DMFFGCN w/o C     | 1.6443  | 5.7489  | 0.0527 |
> | DMFFGCN w/o W     | 1.8200  | 6.7797  | 0.0513 |
> | DMFFGCN w/o A     | 1.6940  | 6.0739  | 0.0473 |
> | DMFFGCN-2000epoch | 1.1035  | 3.5433  | 0.0350 |
> |                   |         |         |        |
> | 模型              | MAE     | RMSE    | MAPE   |
> | AGCRN             | 18.7526 | 85.9551 | 0.3256 |
> | ASTGCN            | 2.7008  | 12.5958 | 0.0834 |
> | DGCNN             | 4.6817  | 19.2657 | 0.1762 |
> | FC-LSTM           | 19.5344 | 84.7473 | 0.3871 |
> | GMAN              | 7.7733  | 22.0913 | 0.2315 |
> | Graph WaveNet     | 7.2982  | 30.7311 | 0.2789 |
> | MSTIF-Net         | 7.0468  | 31.4890 | 0.2943 |
> | STFGNN            | 8.5211  | 40.6801 | 0.3328 |
> | STGCN             | 19.7796 | 84.9773 | 0.4217 |
> | VAR               | 6.2802  | 26.9623 | 0.2198 |
> | MISTAGCN          | 2.7752  | 10.3244 | 0.1357 |
>
> 
>
> HaiKou-ablation
>
> + MFFDSTGCN-850epoch-noMFFmoudle：在这个实验中我们移除掉多模态特征融合模块（图卷积部分），只是把提取完特征的交通流信息和多模态信息叠加到一起
> + MFFDSTGCN-850epoch-noMImoudle：在这个实验中，我们移除掉专门处理多模态信息的并行模块，我们直接把辅助信息叠加到我们的交通流信息上，来验证这个并行模块的作用
>
> | 模型\结果                   | MAE    | RMSE   | MAPE   |
> | --------------------------- | ------ | ------ | ------ |
> | MFFST-RetNet                | 1.1035 | 3.5433 | 0.0488 |
> | MFFST-RetNet w/o MF         | 1.3217 | 3.8654 | 0.0592 |
> | MFFST-RetNet w/o MFF-RetNet | 1.4583 | 4.1276 | 0.0627 |
> | MFFST-RetNet w/o ATIGU      | 1.2379 | 3.7891 | 0.0534 |
> | MFFST-RetNet w/o MH-GAT     | 1.2948 | 3.8125 | 0.0556 |
>
> 
>
> 超参数实验
>
> | 模型\结果                              | MAE    | RMSE   | MAPE   |
> | -------------------------------------- | ------ | ------ | ------ |
> | MFFST-RetNet（F=16,$\theta $=15%,L=3） | 1.1035 | 3.5433 | 0.0488 |
> | MFFST-RetNet（F=32,$\theta $=15%,L=3） | 1.1857 | 3.6982 | 0.0516 |
> | MFFST-RetNet（F=48,$\theta $=15%,L=3） | 1.2358 | 3.9567 | 0.0557 |
> | MFFST-RetNet（F=64,$\theta $=15%,L=3） | 1.2135 | 3.8901 | 0.0542 |
> | MFFST-RetNet（F=80,$\theta $=15%,L=3） | 1.2043 | 3.8029 | 0.0529 |
> | MFFST-RetNet（F=16,$\theta $=5%,L=3）  | 1.2981 | 4.2104 | 0.0581 |
> | MFFST-RetNet（F=16,$\theta $=10%,L=3） | 1.1867 | 3.7548 | 0.0519 |
> | MFFST-RetNet（F=16,$\theta $=15%,L=3） | 1.1035 | 3.5433 | 0.0488 |
> | MFFST-RetNet（F=16,$\theta $=20%,L=3） | 1.1279 | 3.6012 | 0.0498 |
> | MFFST-RetNet（F=16,$\theta $=25%,L=3） | 1.1503 | 3.6459 | 0.0512 |
> | MFFST-RetNet（F=16,$\theta $=15%,L=1） | 1.2673 | 4.0315 | 0.0575 |
> | MFFST-RetNet（F=16,$\theta $=15%,L=2） | 1.1892 | 3.6961 | 0.0514 |
> | MFFST-RetNet（F=16,$\theta $=15%,L=3） | 1.1035 | 3.5433 | 0.0488 |
> | MFFST-RetNet（F=16,$\theta $=15%,L=4） | 1.1587 | 3.6872 | 0.0506 |
> | MFFST-RetNet（F=16,$\theta $=15%,L=5） | 1.2054 | 3.7589 | 0.0527 |
>
> 
>
> 不同预测步长的对比      
>
> tp = 3                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           
>
> | 模型\结果     | MAE    | RMSE    | MAPE   |
> | ------------- | ------ | ------- | ------ |
> | DMFFGCN       | 1.5145 | 5.3820  | 0.0454 |
> | ASTGCN        | 2.7008 | 12.5958 | 0.0834 |
> | DGCNN         | 4.6817 | 19.2657 | 0.1762 |
> | VAR           | 6.2802 | 26.9623 | 0.2198 |
> | Graph WaveNet | 7.2982 | 30.7311 | 0.2789 |
> | STFGNN        | 8.5211 | 40.6801 | 0.3328 |
>
> 













> Yellowtrip
>
> | 模型\结果     | MAE     | RMSE    | MAPE   |
> | ------------- | ------- | ------- | ------ |
> | MFFST-RetNet  | 1.1350  | 2.5211  | 0.0441 |
> | AGCRN         | 14.9889 | 48.5508 | 0.2356 |
> | ASTGCN        | 3.6524  | 12.2728 | 0.0923 |
> | DGCNN         | 13.1800 | 28.7546 | 0.1874 |
> | FC-LSTM       | 17.0592 | 47.7628 | 0.2238 |
> | GMAN          | 4.4055  | 13.0637 | 0.1087 |
> | Graph WaveNet | 5.1916  | 16.1632 | 0.1245 |
> | MSTIF-Net     | 5.9649  | 19.3907 | 0.1467 |
> | STFGNN        | 5.7983  | 18.9360 | 0.1389 |
> | STGCN         | 15.3981 | 48.2825 | 0.2412 |
> | VAR           | 6.6032  | 21.3811 | 0.1578 |
> | MISTAGCN      | 2.1834  | 6.2657  | 0.0543 |
>
> 
>
> 消融实验
>
> | 模型\结果                   | MAE    | RMSE   | MAPE   |
> | --------------------------- | ------ | ------ | ------ |
> | MFFST-RetNet                | 1.1350 | 2.5211 | 0.0441 |
> | MFFST-RetNet w/o MF         | 1.2893 | 2.8374 | 0.0516 |
> | MFFST-RetNet w/o MFF-RetNet | 1.3748 | 3.0241 | 0.0558 |
> | MFFST-RetNet w/o ATIGU      | 1.2416 | 2.7620 | 0.0483 |
> | MFFST-RetNet w/o MH-GAT     | 1.3167 | 2.8195 | 0.0502 |
>
> 
>
> 多模态特征有效性验证实验
>
> | 模型\结果     | MAE    | RMSE   | MAPE   |
> | ------------- | ------ | ------ | ------ |
> | DMFFGCN       | 1.5579 | 3.8317 | 0.0411 |
> | DMFFGCN w/o H | 1.6448 | 3.9223 | 0.0423 |
> | DMFFGCN w/o T | 2.5334 | 5.6393 | 0.0495 |
> | DMFFGCN w/o C | 1.7205 | 4.0833 | 0.0458 |
> | DMFFGCN w/o W | 1.9174 | 4.5443 | 0.0441 |
> | DMFFGCN w/o A | 1.7072 | 4.2283 | 0.0433 |
>
> 











非线性时间交互：

$\mathcal{X}^{\prime}_{odd} = \phi(\mathcal{X}_{\text{odd}}) \odot \operatorname{Softmax}\left(\frac{\phi(\mathcal{X}_{\text{odd}}) \odot \mathcal{X}_{\text{even}}}{\sqrt{F}} + \sigma(W_{\text{odd}} \cdot \mathcal{X}_{\text{odd}})\right)$

$\mathcal{X}_{even}^{\prime} = \psi (\mathcal{X}_{\text{even}}) \odot \operatorname{Softmax}\left(\frac{\psi(\mathcal{X}_{\text{even}}) \odot \mathcal{X}_{\text{odd}}}{\sqrt{F}} + \sigma(W_{\text{even}} \cdot \mathcal{X}_{\text{even}})\right)$



$$\mathcal{X}^{\prime \prime}_{odd} = \eta (\mathcal{X}_{\text{odd}}^{\prime}) \odot \operatorname{Softmax}\left(\frac{\eta(\mathcal{X}_{\text{odd}}^{\prime}) \odot \mathcal{X}_{\text{even}}^{\prime}}{\sqrt{F}} + \sigma(W_{\text{odd}}^{\prime} \cdot \mathcal{X}_{\text{odd}}^{\prime})\right)$$

$$\mathcal{X}^{\prime \prime}_{even} = \eta (\mathcal{X}_{\text{even}}^{\prime}) \odot \operatorname{Softmax}\left(\frac{\eta(\mathcal{X}_{\text{even}}^{\prime}) \odot \mathcal{X}_{\text{odd}}^{\prime}}{\sqrt{F}} + \sigma(W_{\text{even}}^{\prime} \cdot \mathcal{X}_{\text{even}}^{\prime})\right)$$



$$\begin{array}{c}
Q=\left(\mathcal{H}^{f} W_{Q}\right) \odot \Theta^{f}, \quad K_{j}=\left(\mathcal{H}^{j} W_{K}^{j}\right) \odot \bar{\Theta}^{j}, \quad V_{j}=\mathcal{H}^{j} W_{V}^{j} \quad \forall j \in  \mathbb{J} \\
\Theta_{n}^{j}=e^{i n \theta} \forall n=1, \ldots, N_{j} \\
D_{n m}=\left\{\begin{array}{ll}
\gamma^{n-m}, & \text{ if }\  n \geq m, \\
0, & \text { if }\  n<m,
\end{array} \quad \forall n, m=1, \ldots, N_{f}\right.
\end{array}$$





$$\begin{array}{c}
H^{j}=\chi^{j} W_{j} \forall j \in \mathbf{J} \\
\mathcal{H}^{h j}=H^{j}[h d:(h+1) d] \forall h \in \mathbf{h}, \forall j \in \mathbf{J} \\
\gamma_{h}=1-2^{-5-\operatorname{arange}(0,|\mathbf{h}|)} \in \mathbb{R}^{h} \forall h \in \mathbf{h} \\
\text { head }_{h}={\operatorname{Retention}\left(\left\{\mathcal{H}^{h j}\right\}_{\forall j \in \mathbf{J}}, \gamma_{h}\right) \forall h \in \mathbf{h}} \\
{\Gamma=\operatorname{GroupNorm}_{|\mathbf{h}|}\left(\operatorname{Concat}\left(\operatorname{head}_{1}, \ldots, \text { head }_{h}\right)\right)}\\
{\operatorname{MSR}\left(\left\{\boldsymbol{H}^{j}\right\}_{\forall j \in \mathbf{J}}\right)=\left(\operatorname{swish}\left(H^{1} W_{G}\right) \odot Y\right) W_{O}}
\end{array}$$





$$\begin{array}{c}
Y^{l}=M S R\left(L N\left(\left\{H_{l}^{j}\right\}_{\forall j \in \mathbb{J}}\right)\right)+H_{l}^{f} \\
H_{l+1}^{f}=F F N\left(L N\left(Y^{l}\right)\right)+Y^{l} \\
F F N(X)=\operatorname{gelu}\left(X W_{6}\right) W_{7}
\end{array}$$



$$A_{adt}=SoftMax(Relu(E_1E_2^T))$$





$$\lambda = \sigma (A_{adt}\mathcal{X}_{fm}W_{a})$$





$$\mathcal{X}_{t}=\sigma  (ATI\ Block(\mathcal{X}_{1}))\odot (ATI\ Block(\mathcal{X}_{2}))$$



$$ a_{ij}=\text{LeakyReLU}(\mathbf{a}^{\top}[\mathbf{W}{x}_{i}\| \mathbf{W}{x}_{j}])$$



$$\alpha_{i j}=\frac{\exp \left(a_{i j}\right)}{\sum_{k \in \mathcal{N}_{i}} \exp \left(a_{i k}\right)}$$



$$x_{i}^{\prime}=\|_{k=1}^{k} \sigma\left(\sum_{j \in N_{i}} \alpha_{i j} \mathbf{W}_{j}^{k} x_{j}\right)$$



$$\mathcal{X}_{s}=GAT(\mathcal{X}^{f},A_{t} ) $$



$$\hat{\mathbf{X}}=(1-\lambda) \odot \mathcal{X}_{t}+\lambda \odot \varphi\left(\mathcal{X}_{s}\right)$$







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

4. 两个章节中相同的处理（预处理部分）要不要写两遍呢







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

### 多模态特征的降维处理

在本模型中，为了有效减少特征维度，降低冗余信息对模型训练的干扰，同时保留特征中最重要的统计信息，我们采用主成分分析（PCA, Principal Component Analysis）对多模态特征进行了降维处理。具体而言，温度（TT）、天气状况（CC）、风力（WW）和空气质量（AA）作为主要的多模态输入特征，往往存在较高的相关性和冗余性。如果直接输入模型，不仅会增加计算复杂度，还可能导致模型的过拟合问题。因此，降维操作对于提高模型的效率和稳定性至关重要。

#### PCA 降维流程

1. **数据归一化**：
    由于各特征的量纲和取值范围不同（例如，温度的数值范围与空气质量指数的范围差异显著），为了保证降维效果，我们首先对每个特征进行了归一化处理。具体地，将各特征值转化为均值为 0、方差为 1 的标准正态分布：

   x′=x−μσx' = \frac{x - \mu}{\sigma}

   其中，μ\mu 和 σ\sigma 分别为对应特征的均值和标准差。

2. **协方差矩阵计算**：
    对归一化后的特征数据构建协方差矩阵，以衡量各特征之间的线性相关性。协方差矩阵的元素表示不同特征之间的协方差，定义为：

   Cov(X,Y)=1n−1∑i=1n(Xi−Xˉ)(Yi−Yˉ)\text{Cov}(X, Y) = \frac{1}{n-1} \sum_{i=1}^n (X_i - \bar{X})(Y_i - \bar{Y})

3. **特征分解与主成分选择**：
    对协方差矩阵进行特征值分解，获取特征值及对应的特征向量。特征值的大小反映了每个主成分解释原始特征数据的方差比例。我们按照特征值从大到小的顺序排列，并选择累计解释方差比例达到 95% 的主成分，以确保尽可能多地保留原始信息。

4. **特征映射**：
    最终，将归一化后的特征数据投影到选定的主成分方向上，形成降维后的紧凑特征表示。通过这一过程，温度、天气状况、风力和空气质量这四个高维特征被压缩为少量的低维特征输入，从而减轻了模型计算负担。

#### 降维效果

通过 PCA 的降维处理，不仅有效去除了冗余信息，还显著降低了模型的输入维度。实验表明，降维后的多模态特征在保证信息量的同时，进一步提升了模型在特征提取和融合阶段的效率与性能。这为后续的时空依赖性建模提供了更加简洁和优化的输入。





## 四、latex公式

### 第二章

$$u=\frac{dx}{dt} =\lim_{t_{2} \to t_{1} \to 0} \frac{x_{2} - x_{1}}{t_{2}-t_{1}} $$



$$\bar{u}_{t} = \frac{1}{N}\sum_{i=1}^{N}u_{i}   $$



$$\bar{u}_{s} = \frac{D }{\frac{1}{N}\sum_{i=1}^{N}t_{i}   } $$



$$e_{ti}=score(s_{i},h_{i})$$



$$\alpha _{ti}=\frac{exp(e_{ti})}{\sum_{k=1}^{n}exp(e_{tk}) } $$



$$c_{t}=\sum_{i=1}^{n}a_{ti}h_{i} $$



$$s_{t+1} = f(s_{t},y_{t-1},c_{t})$$



$$q_{i} = W^{Q}x_{i},k_{i} = W^{K}x_{i},v_{i} = W^{V}x_{i}$$



$$e_{ij}=\frac{q_{i}\cdot k_{j}}{\sqrt{d_{k}} } $$



$$\alpha _{ij}=\frac{exp(e_{ij})}{\sum_{j=1}^{n}exp(e_{ij}) } $$



$$a_{i}=\sum_{j=1}^{n} \alpha _{ij}v_{j} $$



$$head_{i}=Attention(Q_{i},K_{i},V_{i})=softmax(\frac{Q_{i}K_{i}^{\top }}{\sqrt{d_{k}} } )V_{i}$$



$$\mathbf{Z} =\mathbf{HW} ^{O}$$



$$e_{ij}=LeakyReLU(\mathbf{a}^{\top }  [\mathbf{h}_{i}||\mathbf{h}_{j}])$$



$$\mathbf{h}_{i}^{\prime}=\sigma (\sum_{j \in \mathcal{N}(i) }\alpha _{ij}\mathbf{h}_{j}^{\prime}  ) $$



$$\mathbf{h}_{i}^{\prime}=\|_{k=1}^{k} \sigma\left(\sum_{j \in N_{i}} \alpha^{k}_{i j} \mathbf{W}_{j}^{k} \mathbf{h}_{j}\right)$$



$$\begin{array}{c}
Q=\left(X W_{Q}\right) \odot \Theta, \quad K=\left(X W_{K}\right) \odot \bar{\Theta}, \quad V=X W_{V} \\
\Theta_{n}=e^{i n \theta}, \quad D_{n m}=\left\{\begin{array}{ll}
\gamma^{n-m}, & n \geq m \\
0, & n<m
\end{array}\right. \\
\operatorname{Retention}(X)=\left(Q K^{\boldsymbol{\top}} \odot D\right) V
\end{array}$$



$$\begin{array}{l}
S_{n}=\gamma S_{n-1}+K_{n}^{\top} V_{n} \\
\operatorname{Retention}\left(X_{n}\right)=Q_{n} S_{n}, \quad n=1, \cdots,|x|
\end{array}$$



$$\begin{array}{l} 
Q_{[i]}=Q_{B i: B(i+1)}, \quad K_{[i]}=K_{B i: B(i+1)}, \quad V_{[i]}=V_{B i: B(i+1)} \\
R_{i}=K_{[i]}^{\top}\left(V_{[i]} \odot \zeta\right)+\gamma^{B} R_{i-1}, \quad \zeta_{i j}=\gamma^{B-i-1} \\
\operatorname{Retention}\left(X_{[i]}\right)=\underbrace{\left(Q_{[i]} K_{[i]}^{\top} \odot D\right) V_{[i]}}_{\text {Inner-Chunk }}+\underbrace{\left(Q_{[i]} R_{i-1}\right) \odot \xi}_{\text {Cross-Chunk }}, \quad \xi_{i j}=\gamma^{i+1}
\end{array}$$



$$\begin{array}{c}
\gamma=1-2^{-5-\operatorname{arange}(0,{h})} \in \mathbb{R}^{h}  \\
\text { head }_{i}={\operatorname{Retention}\left(X, \gamma_{i}\right) } \\
{Y=\operatorname{GroupNorm}_{{h}}\left(\operatorname{Concat}\left(\operatorname{head}_{1}, \ldots, \text { head }_{h}\right)\right)}\\
{\operatorname{MSR}\left(X\right)=\left(\operatorname{swish}\left(X W_{G}\right) \odot Y\right) W_{O}}
\end{array}$$



$$\begin{array}{c}
Y^{l}=M S R\left(L N\left(X^{l}\right)\right)+X^{l} \\
X^{l+1}=F F N\left(L N\left(Y^{l}\right)\right)+Y^{l} \\
F F N(X)=\operatorname{gelu}\left(X W_{1}\right) W_{2}
\end{array}$$