# MetaVIM: Meta Variationally Intrinsic Motivated Reinforcement Learning for Decentralized Traffic Signal Control

用于分散交通信号控制的元变异内在动机强化学习

*IEEE TRANSACTIONS ON KNOWLEDGE AND DATA ENGINEERING, VOL. 35, NO. 11, NOVEMBER 2023*:

> SCI Q1
>
> 审稿周期很长，近一年

作者：Liwen Zhu , Peixi Peng , Zongqing Lu , and Yonghong Tian , ==Fellow, IEEE==

[实验室官网](https://www.pkuml.org/)

> 这个作者是强化学习的交通方向，但是论文不多，年轻人

![image-20231113103636789](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311131036894.png)

> IEEE fellow

![image-20231113103534750](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311131035831.png)

## 摘要

*Traffic signal control aims to coordinate traffic signals across intersections to improve the traffic efficiency of a district or a city. Deep reinforcement learning (RL) has been applied to traffic signal control recently and demonstrated promising performance where each traffic signal is regarded as an agent. However, there are still several challenges that may limit its large-scale application in the real world. On the one hand, the policy of the current traffic signal is often heavily influenced by its neighbor agents, and the coordination between the agent and its neighbors needs to be considered. Hence, the control of a road network composed of multiple traffic signals is naturally modeled as a multi-agent system, and all agents’ policies need to be optimized simultaneously. On the other hand, once the policy function is conditioned on not only the current agent’s observation but also the neighbors’, the policy function would be closely related to the training scenario and cause poor generalizability because the agents in various scenarios often have heterogeneous neighbors. To make the policy learned from a training scenario generalizable to new unseen scenarios, a novel Meta Variationally Intrinsic Motivated (MetaVIM) RL method is proposed to learn the decentralized policy for each intersection that considers neighbor information in a latent way. Specifically, we formulate the policy learning as a meta-learning problem over a set of related tasks, where each task corresponds to traffic signal control at an intersection whose neighbors are regarded as the unobserved part of the state. Then, a learned latent variable is introduced to represent the task’s specific information and is further brought into the policy for learning. In addition, to make the policy learning stable, a novel intrinsic reward is designed to encourage each agent’s received rewards and observation（也被叫做：State） transition to be predictable only conditioned on its own history. Extensive experiments conducted on CityFlow demonstrate that the proposed method substantially outperforms existing approaches and shows superior generalizability.*



> 状态转移(State Transition)(observation transition)
>
> ![image-20231113131015383](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311131310488.png)



交通信号控制旨在协调交叉口间的交通信号，以提高一个地区或城市的交通效率。近年来，深度强化学习（RL）已应用于交通信号控制，并表现出有望的性能，其中每个交通信号被视为一个智能体。然而，仍然存在一些挑战，可能限制其在现实世界中的大规模应用。

一方面，当前交通信号的策略通常受其邻近智能体的严重影响，需要考虑智能体与其邻居之间的协调。因此，由多个交通信号组成的道路网络的控制自然地建模为多智能体系统，并需要同时优化所有智能体的策略。另一方面，一旦策略函数不仅以当前智能体的观察为条件，还以邻居的观察为条件，策略函数将与训练场景密切相关，并导致通用性差，因为不同场景中的智能体通常具有异质的邻居。

为了使从训练场景中学到的策略能够推广到新的未见场景，提出了一种新颖的Meta Variationally Intrinsic Motivated（MetaVIM）RL方法，用于以潜在方式考虑邻居信息学习每个交叉口的分散策略。具体而言，我们将策略学习视为一种关于一组相关任务的元学习问题，其中每个任务对应于交叉口的交通信号控制，其邻居被视为状态的未观察部分。然后，引入了一个学到的潜在变量来表示任务的特定信息，并进一步引入策略学习中。此外，为了使策略学习稳定，设计了一种新颖的内在奖励，以鼓励每个智能体的奖励获取和状态转移仅在其自己的历史条件下可预测。

在CityFlow上进行的大量实验表明，所提出的方法明显优于现有方法，并显示出更高的通用性。

> 新仿真工具 cityflow [CityFlow](https://zhuanlan.zhihu.com/p/585071751)，这是知乎上的一个使用教程

> 总结：
>
> ​	存在的问题一：交通信号等之间相互影响，建模为多智能体系统，每个智能体是一个信号灯
>
> ​	存在的问题二：**策略函数**在多智能体环境下（考虑邻居智能体的观察），会和当前交通网络产生过拟合（通用性差）

> 总结：
>
> ​	提出的模型：Meta Variationally Intrinsic Motivated（MetaVIM）RL方法
>
> ​	优点一：每个交叉口都会学习这种分散策略（去中心策略）并且会以**潜在方式**考虑邻居信息
>
> ​	优点二：策略学习视为一组相关任务的元学习问题，单个任务就是一个交通信号的控制，**潜在变量**表示邻居信息
>
> ​	优点三：一种新颖的内在奖励，以鼓励每个智能体收到的奖励和状态转移**仅在其自己的历史条件下可预测**。

> 策略函数：决定我们的动作的函数

![image-20231113131211167](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311131312218.png)

## 一、介绍

*TRAFFIC signals that direct traffic movements play an important role in efficient transportation. Most conventional methods aim to control traffic signals by fixed-time plans [1] or hand-crafted heuristics [2]. However, the pre-defined rules cannot adapt to dynamic and uncertain traffic conditions.*

*Recently, deep reinforcement learning (RL) [3], [4], [5], [6], [7], [8], [9], [10], [11] has been applied in Intelligent Transportation Systems (ITS) and demonstrated promising performance. Specifically, recent works [12], [13], [14] use RL for traffic signal control, and provide a promising way to handle dynamic and uncertain traffic conditions, where a RL agent employs a deep neural network to control an intersection and the network is learned by directly interacting with the environment*



指挥交通运动的交通信号在有效的交通中发挥着重要作用。大多数传统方法旨在通过固定的时间计划[1]或手工制定的启发式方法[2]来控制交通信号。然而，预先定义的规则无法适应动态和不确定的交通条件。

最近，深度强化学习（RL）[3]，[4]，[5]，[6]，[7]，[8]，[9]，[10]，[11]已应用于智能交通系统（ITS），并展示了有希望的性能。具体来说，最近的研究[12]，[13]，[14]使用RL来进行交通信号控制，提供了一种处理动态和不确定交通条件的有希望的方法，其中RL代理使用深度神经网络来控制一个交叉口，网络通过与环境的直接交互来学习。





*The most straightforward RL baseline considers each intersection independently and models the task as a single agent RL problem [12]. However, the observation, received reward and dynamics of each traffic signal are closely related to its neighbors, and the coordination between signals should be modeled. Hence, optimizing traffic signal control in a large-scale road network could be modeled as a multi-agent reinforcement learning (MARL) problem. Prior works [15], [16] in this domain resort to centralized training to ensure that agents learn to coordinate, they demonstrate that communication among agents could help coordination.*

*However, as the joint action space grows exponentially with the number of agents, it is infeasible or costly in realistic deployment, and training emergent communication protocols also remains a challenging problem. In addition, once the policy function is conditioned on not only the current agent’s observation but also the neighbors’, the policy function would be closely related to the training scenario and cause poor generalizability because the agents in various scenarios often have heterogeneous neighbors. Therefore, learning decentralized policies opens up a window of new opportunities for advanced MARL research in this area.*



最简单的**强化学习（RL）基线**将每个交叉口视为独立的代理，并将任务建模为单一代理RL问题[12]。然而，每个交通信号的观测、接收奖励和动态与其邻近信号密切相关，信号之间的协调应该被建模。因此，在大规模道路网络中优化交通信号控制可以被建模为多代理强化学习（MARL）问题。先前的研究[15]，[16]在这个领域采用集中式训练，以确保代理学会协调，他们表明代理之间的通信可以帮助协调。

然而，随着代理数量的增加，联合行动空间呈指数增长，在现实部署中变得不可行或昂贵，训练新兴的通信协议仍然是一个具有挑战性的问题。此外，一旦策略函数不仅基于当前代理的观察，而且基于邻居的观察，策略函数将与训练场景密切相关，并且可能导致较差的泛化性能，因为不同场景中的代理通常具有异构的邻居。因此，学习分散的策略为这一领域的先进MARL研究打开了新的机会。

> 总结：
>
> ​	原先的多代理强化学习问题采用中心化的训练方式，可是代理增加算法复杂度指数级增长，不可行。策略如果考虑了邻居的观察，导致过拟合，所以使用去中心化的学习策略



*To learn effective decentralized policies, there are two main challenges. First, it is impractical to learn an individual policy for each intersection in a city or a district containing thousands of intersections. Parameter sharing may help.*

*However, each intersection has a different traffic pattern, and a simple shared policy hardly learns and acts optimally at all intersections. To handle this challenge, we formulate the policy learning in a road network as a meta-learning problem, where traffic signal control at each intersection corresponds to a task, and a policy is learned to adapt to various tasks. Reward function and state transition of these tasks vary but share similarities since they follow the same traffic rules and have similar optimization goals. Therefore, we represent each task as a learned and low-dimensional latent variable obtained by encoding the past trajectory in each task.*

*The latent variable is a part of the input of the policy, which captures task-specific information and helps improve the policy adaption*

 

为了学习有效的分散式策略，存在两个主要挑战。首先，在城市或包含数千个交叉口的区域学习每个交叉口的个体策略是不切实际的。参数共享可能有助于解决这个问题。

然而，每个交叉口的交通模式都不同，一个简单的共享策略很难在所有交叉口上学习和行为都达到最优。为了解决这个挑战，我们将路网中的策略学习形式化为元学习问题，其中每个交叉口的交通信号控制对应一个任务，而一个策略被学习以适应各种任务。这些任务的奖励函数和状态转移不同，但由于它们遵循相同的交通规则并具有类似的优化目标，它们共享相似之处。因此，我们将每个任务表示为一个通过对每个任务的过去轨迹进行编码而获得的学习和低维潜变量。

这个潜变量是策略的一部分，它捕捉任务特定的信息并有助于提高策略的适应性。

> 总结：
>
> ​	挑战一：参数共享 -> 共享策略（所有交叉口都使用），但是一个共享策略很难在所有交叉口上学习和行为都达到最优，策略学习 -> 元学习问题，每个交叉口的交通信号控制对应一个任务，将每个任务表示为一个通过对每个任务的过去轨迹进行编码而获得的可学习的低维潜变量。这个潜变量是策略的一部分，它捕捉任务特定的信息并有助于提高策略的适应性



![image-20231114163246593](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311141632760.png)

*Illustration of the multi-intersection control scenario where policies and rewards are affected by neighboring policies, i.e., the instability of the multi-agent scenario. *

多交叉口控制场景的插图，其中策略和奖励受到邻近策略的影响，即多智能体场景的不稳定性。





*Second, even for a specific task, the received rewards and observations are uncertain to the agent, as illustrated in Fig. 1, which make the policy learning unstable and nonconvergent. Even if the agent performs the same action on the same observation at different timesteps, the agent may receive different rewards and observation transitions because of neighbor agents’ different actions. In this case, the received rewards and observation transitions of the current agent could not be well predicted only conditioned on its own or partial neighbors’ observations and performed actions. To avoid this situation, four decoders are introduced to predict the next observations and rewards without neighbor agents’ policies or with partially neighbor agents, respectively.In addition, an intrinsic reward is designed to reduce the bias among different predictions and enhance learning stability. In other words, the design of the decoders and intrinsic reward is similar to the law of contra-positive.*

*The unstable learning will cause the predicted rewards and observation transitions unstable in a decentralized way, while our decoders and intrinsic reward encourage the prediction convergent. In addition, from the perspective of information theory, the intrinsic reward design makes the policy of each agent robust to neighbours’ polices, which could make the learned policy easy to transfer*



其次，即便针对特定任务，代理接收到的奖励和状态转移也对其存在不确定性，正如图1所示，这使得策略学习变得不稳定且难以收敛。即使代理在不同的时间步上对相同的观察采取相同的行动，由于邻居代理的不同行动，代理可能会获得不同的奖励和状态转移。在这种情况下，仅仅基于其自身或部分邻居的观察和执行的行动条件下，当前代理的获得奖励和状态转移就无法被很好地预测。为避免这种情况，引入了四个解码器，分别用于在没有邻居代理的策略或部分邻居代理的情况下预测下一个观察和奖励。此外，设计了一种内在奖励，以减少不同预测之间的偏差并增强学习稳定性。换句话说，解码器和内在奖励的设计类似于反证法的原理。不稳定的学习会导致以分散方式预测的奖励和状态转移不稳定，而我们的解码器和内在奖励则鼓励预测的收敛。此外，从信息论的角度来看，内在奖励的设计使得每个代理的策略对邻居的策略具有鲁棒性，这有助于学得的策略更容易传递。



> 总结：
>
> ​	挑战二：对于一个交叉口，代理接收到的奖励和状态转移存在不确定性，使得策略学习变得不稳定且难以收敛，所以引入四个解码器，**分别用于在没有邻居代理的策略或部分邻居代理的情况下预测下一个观察和奖励**（用于将邻居的行为全面的考虑）。
>
> ​	创新点：内在奖励，**减少不同预测间的偏差**，增强学习稳定性



*Intrinsic motivation refers to reward functions that allow agents to learn useful behaviors across various tasks. Previous approaches to intrinsic motivation often focus on curiosity [17], imagination [18] or synergy [19], these approaches rely on hand-crafted rewards specific to the environment, or limit to the bimanual manipulation tasks. Such structural constraints make it impossible to achieve independent training of MARL agents across multiple environments.*

*Here, we consider the problem of deriving intrinsic motivation as an exploration bias from other agents in MARL. Our key idea is that a good guiding principle for intrinsic motivation is to make approximations robust to a dynamic environment. Intrinsic motivation is assessed using comparison reasoning: at each timestep, an agent measures the effect of another agent’s policy on its model’s predictions. Neighbor’s policies that lead to a relatively higher change in an agent’s own model estimates are considered highly influential, and because the explicit effect of neighbor’s influence in this task is to increase incoming traffic volumes, which exacerbates the agent’s own traffic load. Therefore, we minimize this influence bias to make the model’s estimations free from interference from others. We show that this inductive bias will drive agents to learn effective decentralized policies.*



*内在动机是指允许代理在各种任务中学习有用行为的奖励函数。先前对内在动机的方法通常侧重于好奇心[17]、想象力[18]或协同效应[19]，这些方法依赖于对环境特定的手工制定奖励，或限制于双手操纵任务。这些结构性约束使得无法实现在多个环境中独立训练多智能体强化学习代理。*

*在这里，我们考虑将内在动机作为来自多智能体强化学习中其他代理的一种探索偏向的问题。我们的关键思想是，内在动机的一个良好的指导原则是使近似对动态环境具有鲁棒性。通过比较推理来评估内在动机：在每个时间步，代理测量另一个代理策略对其模型预测的影响。==导致代理自身模型估计相对较大变化的邻居的策略被认为是高度有影响力的==，因为这项任务中邻居的影响的明确效果是增加流入交通量，这加剧了代理自身的交通负荷。因此，我们最小化这种影响偏差，使模型的估计不受其他代理的干扰。我们展示了这种归纳偏见将驱使代理学习有效的分散策略。*



> 总结：
>
> ​	内在动机是指允许代理在各种任务中学习有用行为的奖励函数。
>
> ​	指导原则：对动态环境具有鲁棒性
>
> ​	最小化邻居的影响



*We conduct extensive experiments on CityFlow [20] in public datasets Hangzhou (China), Jinan (China), New York (USA), and our derived dataset Shenzhen (China) road networks under various traffic patterns, and empirically demonstrate that our proposed method can achieve state-of-theart performances over the above scenarios. Furthermore, our method shows superior adaptivity in transfer experiments. In summary, the main contributions of this paper are concluded as three aspects:*

*1) The traffic signal control is modeled as a meta-learning problem over a set of related tasks, and a novel method MetaVIM is proposed to learn decentralized policy to handle large-scale traffic lights.*

*2) A learnable latent variable is introduced to represent task-specific information by performing approximate inference on the task, and make the policy function shareable across the tasks.*

*3) A novel intrinsic reward is designed to tackle the challenge of unstable policy learning in dynamically changing traffic environments. We show that this design will drive agents to learn effective decentralized policies.*

*The paper is structured as follows. We describe the related work in Section 2, and the MARL setting in Section 3.*

*Section 4 introduces the details of the proposed method.*

*Section 5 presents experimental results that empirically demonstrate the efficacy of MetaVIM. Finally, conclusions and future work are discussed in Section 6.*



*我们在公共数据集 CityFlow [20] 上进行了大量实验，包括中国的杭州、济南，美国的纽约以及我们衍生的深圳道路网络，在不同交通模式下进行了测试。实验证明我们提出的方法在上述场景下能够达到最先进的性能。此外，我们的方法在迁移实验中表现出卓越的适应性。总的来说，本文的主要贡献有三个方面：*

*1) 交通信号控制被建模为一组相关任务的元学习问题，提出了一种新颖的方法 MetaVIM，用于学习处理大规模交通信号的分散策略。*

*2) 引入了可学习的潜变量来表示通过对任务进行近似推理而获得的任务特定信息，并使策略函数在任务之间共享。*

*3) 设计了一种新颖的内在奖励，以解决动态变化交通环境中不稳定策略学习的挑战。我们展示了这种设计将驱使代理学习有效的分散策略。*

*文章结构如下。我们在第 2 节介绍相关工作，第 3 节介绍了多智能体强化学习的设置。*

*第 4 节介绍了所提出方法的详细信息。*

*第 5 节呈现了实验证明 MetaVIM 有效性的实验结果。最后，在第 6 节讨论了结论和未来工作。*

> 总结：
>
> ​	MetaVIM：学习处理大规模交通信号的去中心化策略
>
> ​	潜变量：邻居的信息，使策略共享
>
> ​	内在奖励：使收敛



## 二、相关工作

### 2.1 传统和自适应交通信号控制

*Most conventional traffic signal control methods are designed based on fixed-time signal control [21], actuated control [22] or self-organizing traffic signal control [23].These approaches rely on expert knowledge and often perform unsatisfactorily in complicated real-world situations.*

*To solve this problem, several optimization-based methods have been proposed to optimize average travel time,throughput, etc., which decide the traffic signal plans according to the dynamical observed data. Specifically, the method [2] calculates the difference between the number of upstream and downstream vehicles and then sorts the values to determine the phase order. Besides, several methods [24], [25], [26], [27] consider the scheduling of urban traffic light as the model-based optimization problem and employ search based methods to solve it. Furthermore, the dynamic programming [28], mixed-integer linear programming [29] and non-linear programming models [30] are also used.*

*Please see [31] for more details. Although effective in some situations, these approaches typically rely on prior assumptions which may fail in some cases, or require environment model which is unavailable and unreliable in complex scenarios.*

*大多数传统的交通信号控制方法是基于固定时控制 [21]、感应控制 [22] 或自组织交通信号控制 [23] 设计的。这些方法依赖于专业知识，并且在复杂的实际情况下通常表现不佳。*

*为解决这一问题，提出了几种基于优化的方法来优化平均行驶时间、通行能力等，根据动态观测数据决定交通信号计划。具体来说，方法 [2] 计算上游和下游车辆数量之间的差异，然后对这些值进行排序以确定相位顺序。此外，一些方法 [24]、[25]、[26]、[27] 将城市交通信号的调度视为基于模型的优化问题，并采用基于搜索的方法来解决它。此外，动态规划 [28]、混合整数线性规划 [29] 和非线性规划模型 [30] 也被使用。*

*更多细节请参见 [31]。尽管在某些情况下有效，但这些方法通常依赖于先验假设，可能在某些情况下失败，或者需要在复杂场景中不可用且不可靠的环境模型。*



### 2.2 基于强化学习的交通信号控制

*RL-based traffic signal control methods aim to learn the policy from interactions with the environment. Earlier studies use tabular Q-learning [32], [33], [34], [35] where the states are required to be discretized and low-dimensional. To handle more complex continuous state, recent advances employ deep RL to map the high-dimensional state representations (such as images or feature vectors) into actions.*

*基于强化学习的交通信号控制方法旨在通过与环境的交互学习策略。早期的研究采用表格型 Q 学习 [32]、[33]、[34]、[35]，其中需要对状态进行离散化处理，并且状态的维度较低。为了处理更复杂的连续状态，近期的进展采用深度强化学习，将高维状态表示（例如图像或特征向量）映射到动作空间。*

*Efforts have been made to design strategies that formulate the task as a single agent [12], [14], [36], [37] or some isolated intersections [38], [39], [40], [41], [42], [43] , i.e., each agent makes decision for its own. This type of methods is usually easy to scale, but may have difficulty to achieve global optimal performance due to the lack of collaboration. To address the problem, another way is to jointly model the action among learning agents with centralized optimization [15], [16]. However, as the number of agents increases, joint optimization usually leads to dimensional explosion, which has inhibited the widespread adoption of such methods to a large-scale traffic signal control. To overcome the difficulty, another type of methods are implemented in a decentralized manner.*

*For example, the methods proposed in [32], [44] directly add neighboring information into states, and the neighbors’ hidden features are merged into states in [3], [45], [46], [47]. Compared with them, our method uses neighbor information to form intrinsic motivation rather than as additional input of the policy. It makes our method easy to transfer to a new scenario which may have different neighbour topology with the training scenario. Besides, the neighborhood travel time is optimized in [48] as an additional reward. However, simple concatenation of neighboring information is not reasonable enough because the influence of neighboring intersections is not balanced*

*有人尝试设计将任务表述为单一代理 [12]、[14]、[36]、[37] 或一些孤立的交叉口 [38]、[39]、[40]、[41]、[42]、[43]，即每个代理为自己做决策。这类方法通常易于扩展，但由于缺乏协作，可能难以实现全局最优性能。为了解决这个问题，另一种方式是通过集中优化 [15]、[16] 共同对学习代理的行动进行建模。然而，随着代理数量的增加，联合优化通常导致维度爆炸，这抑制了这类方法在大规模交通信号控制中的广泛应用。为了克服这个困难，另一类方法以分散的方式实施。*

*例如，在 [32]、[44] 中提出的方法直接将邻居信息添加到状态中，而在 [3]、[45]、[46]、[47] 中，邻居的隐藏特征被合并到状态中。与它们相比，我们的方法使用邻居信息形成内在动机，而不是作为策略的额外输入。这使得我们的方法易于转移到可能与训练场景具有不同邻居拓扑结构的新场景。此外，在 [48] 中，邻域行驶时间被优化为额外的奖励。然而，简单地将邻居信息进行连接并不足够合理，因为邻近交叉口的影响不是平衡的。*

*To make the policy transferable, traffic signal control is also modeled as a meta-learning problem in [14], [36], [49].*

*Specifically, the method in [14] performs meta-learning on multiple independent MDPs and ignores the influences of neighbor agents. A data augmentation method is proposed in [49] to generates diverse traffic flows to enhance metaRL, and also regards agents as independent individuals, without explicitly considering neighbors. In addition, a model-based RL method is proposed in [36] for high data efficiency. However it may introduce cumulative errors due to error of the learned environment model and it is hard to achieve the asymptotic performance of model-free methods.*

*Our method both belongs to meta-RL paradigms, the main advantages are two main aspects First, we consider the neighbour information during the meta-learning, which is critical for the multi-agent coordination. Second, our method learns a latent variable to represent task-specific information, which can not only balance exploration and exploitation [50], but also help to learn the shared structures of reward and transition across tasks. As far as we know, our work is the first to propose an intrinsic motivation to enhance the robustness of the policy on traffic signal control. See Appendix F, online available, for a brief overview of the above methods.*

*为了使策略具有可转移性，交通信号控制在 [14]、[36]、[49] 中也被建模为元学习问题。*

*具体来说，在 [14] 中的方法对多个独立的马尔可夫决策过程进行元学习，忽略了邻近代理的影响。在 [49] 中提出了一种数据增强方法，用于生成多样的交通流以增强元强化学习，同时将代理视为独立个体，没有明确考虑邻居的影响。此外，在 [36] 中提出了一种基于模型的强化学习方法，以实现高数据效率。然而，由于学得的环境模型误差，可能引入累积误差，并且难以达到无模型方法的渐近性能。*

*我们的方法同时属于元强化学习范式，主要优势有两个方面。首先，在元学习过程中考虑邻居信息对于多智能体协调至关重要。其次，我们的方法学习了一个潜变量来表示任务特定信息，这不仅可以平衡探索和利用 [50]，还有助于学习跨任务的奖励和转移的共享结构。据我们所知，我们的工作是首次提出使用内在动机来增强交通信号控制策略的鲁棒性。有关上述方法的简要概述，请参见附录 F，在线获取。*



### 2.3 内在奖励

*Intrinsic motivation methods have been widely studied in the literature, such as handling the difficult-to-learn dilemma in sparse reward environments [51] or trading off the exploration and exploitation in non-sparse reward scenarios [50]. Most of the intrinsic reward approaches can be classified into two classes. The first class is counted-based paradigm, where agents are incentivized to reach infrequently visited states by maintaining state visitation counts [52], [53] or density estimators [54], [55]. However, this paradigm is challenging in continuous or high-dimensional state space. The second is curiosity-based paradigm, in which agents are rewarded for high prediction error in a learned reward [17], [56] or inverse dynamics model [55], [57]. The uncertainty of the agent’s assessment of its behavior can be measured as a curiosity for environmental exploration.*

*Besides the above two classes, other intrinsic reward methods are mainly task-oriented and for a specific purpose. For example, the method in [19] uses the discrepancy between the marginal policy and the conditional policy as the intrinsic reward for encouraging agents to have a greater social impact on others. The errors between the joint cooperative behaviors and the individual actions are defined in [58] as an intrinsic reward, which is suitable for agent-pair tasks that rely heavily on collaboration, such as dual-arm robot tasks. Similar with them, the proposed intrinsic reward is specially designed for the traffic signal control task, and we adopt the discrepancy with/without neighbor agent’s policies as a motivation for decentralized control in dynamically changing multi-agent scenarios, making the policy robust to the environment.*

*内在动机方法在文献中得到了广泛研究，例如处理稀疏奖励环境中的难以学习的困境 [51]，或在非稀疏奖励场景中权衡探索和利用 [50]。大多数内在奖励方法可以分为两类。第一类是基于计数的范式，其中通过维护状态访问计数 [52]、[53] 或密度估计器 [54]、[55] 来激励代理达到不经常访问的状态。然而，在连续或高维状态空间中，这种范式具有挑战性。第二类是基于好奇心的范式，代理根据学得奖励 [17]、[56] 或逆动力学模型 [55]、[57] 中的高预测误差而获得奖励。代理对其行为评估的不确定性可以作为对环境探索的好奇心来衡量。*

*除了上述两类之外，其他内在奖励方法主要是面向任务的，用于特定目的。例如，[19] 中的方法使用边际策略和条件策略之间的差异作为鼓励代理对其他人产生更大社会影响的内在奖励。[58] 中定义了联合合作行为与个体行为之间的误差作为内在奖励，适用于严重依赖协作的任务，例如双臂机器人任务。与它们类似，所提出的内在奖励专门为交通信号控制任务设计，我们采用了与/不与邻近代理的策略的差异作为分散控制的动机，在动态变化的多智能体场景中使策略对环境具有鲁棒性。*



### **2.4 RL 中的潜变量**

*A number of prior works have explored how RL can be cast in the framework of variational inference. Latent variable could transform the dynamically updated task-related information such as trajectories into a continuous lowerdimensional space. For example, [59] shows that exploring in latent space can enhance the representation of the environment. In meta-RL, the agent is not given prepared taskspecific data to adapt to, and it must fully explore the environment to collect useful information. Privileged information generated during the learning phase can be used during meta-training to guide exploration, such as expert trajectories [60], dense reward for meta-training but not testing [61], or ground-truth task IDs / descriptions [62], [63].*

*Similar work [64] introduces temporally-extended exploration with latent variable. A series of methods [50], [59], [61] use variational auto-encoders (VAE)[65] structure to help explore the environment. A branch of context-based methods automatically learns to trade-off exploration and exploitation by maximizing average adaptation performance [50], [66]. Differently, we learn a dynamical latent variable for each task to present task-specific information and indicate the correspond agent’s belief.*

*先前的一些工作探讨了如何将强化学习嵌入**变分推理**框架中。潜变量可以将动态更新的与任务相关的信息，如轨迹，转换为一个连续的低维空间。例如，[59] 表明在潜在空间中进行探索可以增强对环境的表示。在元强化学习中，代理没有提前准备好的任务特定数据进行适应，它必须完全探索环境以收集有用的信息。在学习阶段生成的特权信息可以在元训练期间用于引导探索，例如专家轨迹 [60]、用于元训练但不用于测试的密集奖励 [61]，或地面实况任务ID/描述 [62]、[63]。*

*类似的工作 [64] 引入了具有潜变量的时间扩展探索。一系列方法 [50]、[59]、[61] 使用变分自编码器（VAE）[65] 结构来帮助探索环境。一部分基于上下文的方法通过最大化平均适应性能来自动学习权衡探索和利用 [50]、[66]。与之不同的是，我们为每个任务学习一个动态潜变量，以呈现任务特定信息并指示相应代理的信念。*



## 三、问题表述

### 3.1 准备工作

*In this article, we investigate traffic signal control of multiintersection as illustrated in Fig. 6. In order to explain the basic concepts, we use a 4-way intersection as an example, depicted in Fig. 2. Note that the concepts are easily generalized to different intersection structures (e.g., different numbers of entering approaches or phases).*

*在本文中，我们研究了如图 6 所示的多交叉口的交通信号控制。为了解释基本概念，我们以一个4路口为例，如图 2 所示。请注意，这些概念很容易推广到不同的交叉口结构（例如，不同数量的进口道或相位）*。

+ **定义一：进出车道**

+ **定义二：相位**：一个路口合法的红绿灯组合
+ **定义三：平均通行时间**：车辆的行驶时间是指进入特定区域和离开该区域之间的时间差异。评估交通信号控制性能最常用的指标是道路网络中所有车辆的平均行驶时间。

### 3.2 问题定义

*The traffic signal control in a road network is modeled as a multi-agent system, where each signal is controlled by an agent. Before formulating the problem, we firstly design the learning paradigm by analyzing the characteristics of the traffic signal control (TSC). Due to the coordination among different signals, the most direct paradigm may be centralized learning.*

*However, the global state information in TSC is not only highly redundant and difficult to obtain in realistic deployment, but also likely suffers from dimensional explosion. Moreover, once the policy function relies on the global state information or neighbors on execution, it is hard to transfer the policy from the training scenario to other unseen scenarios containing different road networks. Hence, it is natural to resort to the decentralized policy, which controls each signal only conditioned on its own history. However, the fully decentralized learning ignores the coordination. If agents are behaved independently, agents maximize their own rewards and may sacrifice the interests of others, it is difficult for the entire system to reach the optimum. Therefore, we model the task as Decentralized Partially Observable Markov Decision Process (Dec-POMDP) [67]. The neighbors’ information is considered, all agents’ policies are optimized synchronously in training, while only the agent’s observation history is used in the execution*

*在道路网络中，交通信号控制被建模为一个多智能体系统，其中每个信号由一个代理控制。在制定问题之前，我们首先通过分析交通信号控制（TSC）的特性设计学习范式。由于不同信号之间需要协调，最直接的范式可能是集中式学习。*

*然而，在实际部署中，TSC中的全局状态信息不仅高度冗余且难以获取，而且可能受到维度爆炸的影响。此外，一旦策略函数依赖于全局状态信息或执行时的邻居，将策略从训练场景转移到包含不同道路网络的其他未见场景就变得困难。因此，自然而然地转向了分散策略，即仅在其自身历史条件下控制每个信号。然而，完全分散的学习忽略了协调。如果代理行为独立，它们会最大化自己的奖励，并可能牺牲他人的利益，整个系统难以达到最优。因此，我们将任务建模为**分散部分可观察马尔可夫决策过程（Dec-POMDP）**[67]。考虑到邻居的信息，**在训练中所有代理的策略都被同步优化，而在执行时仅使用代理的观察历史**。*



*Specifically, a Dec-POMDP could be denoted by a tuple G ¼ < S; N ; A; O; Z;P; R; H; g > . Each intersection in the scenario is controlled by an agent. S is the state space, and st 2 S denotes the state at time step t. Since the environment is partially observed, each agent i only has access to its local observation oi;t 2 O that is acquired through an observation function ZðsÞ : S!O. At current time, all agents perform joint action a ¼ fai; ... ; aN g; ai 2 A and cause the state transition dynamics Pðstþ1jst; aÞ :¼SAN ! S where stþ1 is the next state. The observation-action history of agent i at time t is denoted as ti;:t. R ¼ fRigN i¼1 is the reward for each agent. As stated in Section 3.3, the reward is calculated by the partial observation (queue length) and the observation transition may be unstable in a multi-agent system. That is, even if the agent performs the same action on the same observation at different timesteps, the agent may receive different observation transitions because neighbor agents may perform different actions. Hence, we define the reward function of each agent as ri;t ¼ Riðoi;t; ai; oi;tþ1Þ. Our goal is to learn the decentralized policy piðai j oi;tÞ to maximize its cumulative rewards: P t gt ri;t where g is the discounted factor. In the traffic signal task, the next observation of each agent not only relies on current agent’s observation and performed action, but also is associated with neighbors actions. Therefore, there exists an observation transition function T iðoi;tþ1 j oi;t; ai; ai Þ for each agent i where ai is the joint action of neighbors.*

*Based on the above formulation, there are two key issues that need to be considered:*

*具体而言，Dec-POMDP 可以用一个元组 G < S; N ; A; O; Z; P; R; H; $\gamma $ > 表示。在场景中，每个交叉口由一个代理控制。S 是状态空间，$s_t \in S$ 表示时间步 t 的状态。由于环境是部分可观测的，每个代理 i 只能访问其局部观察 $o_{i,t} \in O$ ，该观察是通过观察函数 $Z(s):S \to O$ 获取的。在当前时间，所有代理执行联合动作 a 并导致状态转移 ....，其中 .... 是下一个状态。代理 i 在时间 t 的观察-动作历史表示为 ti;:t。R ¼ fRigN i¼1 是每个代理的奖励。正如第 3.3 节所述，奖励是通过部分观察（队列长度）计算的，状态转换在多智能体系统中可能是不稳定的。也就是说，即使代理在不同的时间步上对相同的观察执行相同的动作，由于邻近代理可能执行不同的动作，代理可能会收到不同的观察转换。因此，我们将每个代理的奖励函数定义为 ri;t ¼ Riðoi;t; ai; oi;tþ1Þ。我们的目标是学习分散策略 piðai j oi;tÞ 以最大化其累积奖励：P t gt ri;t 其中 g 是折现因子。在交通信号任务中，每个代理的下一个观察不仅依赖于当前代理的观察和执行的动作，而且与邻居的动作相关联。因此，对于每个代理 i，存在一个观察转移函数 T iðoi;tþ1 j oi;t; ai; ai Þ，其中 ai 是邻居的联合动作。*

*基于上述制定，有两个需要考虑的关键问题:*

> 手写笔记

*Since training policy for each realistic scenario is time-consuming even impossible, hence our goal is to learn the decentralized policy piðai j oi;tÞ from the given training scenario (i.e., a road network), which can be generalized to unseen scenarios. Thus the meta-learning framework is employed where the policy learning of each agent corresponds to a task.*

*The reward function Ri and observation transition T i of these tasks vary but also share similarities since they follow the same traffic rules and have similar optimization goals. Therefore, we represent each task as a learned and low dimensional latent variable mi. By incorporating mi, we assume the reward, observation transition and policy functions could be shareable across tasks:*

*由于为每个现实场景训练策略耗时是不可行的，因此我们的目标是从给定的训练场景（即道路网络）中学习分散策略 piðai j oi;tÞ，从而可以推广到未见的场景。因此，采用**元学习框架**，其中每个代理的策略学习对应于一个任务。*

*这些任务的奖励函数 Ri 和观察转移 T i 变化，也具有相似性，因为它们遵循相同的交通规则并具有类似的优化目标。因此，我们将每个任务表示为一个学习得到的低维潜变量 mi。通过引入 mi，我们假设奖励、观察转移和策略函数可以在任务之间共享：*

![image-20231115191707367](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311151917473.png)

> 元学习框架：
>
> ​	元学习框架是一种机器学习方法，其中模型被设计为能够在学习新任务时保持高度的灵活性和适应性。在元学习中，模型经过预先训练，使其能够快速学习新任务或在新环境中适应。这种方法的目标是使模型具有泛化到未见任务的能力，而不仅仅是在训练时见过的任务上表现良好。

> 总结：
>
> ​	学习去中心化的策略以推广到所有场景，采用元学习框架
>
> ​	每个代理的策略学习是一个任务，通过引入一个可学习的低维潜变量（将每个任务表示为一个可学习的低维潜变量 mi），这些任务的奖励、观察转移和策略函数可以共享



*Since Ri not only rely on oi;t and ai but also oi;tþ1 which is generated by T i and related with ai , hence learning pi from Ri may cause learning non-stationary because the agent may receive different rewards and observation transitions for the same action at the same observation. In this case, the received rewards and observation transitions of the current agent could not be well predicted only conditioned on its own observations and performed actions. Conversely, to avoid suffering such non-stationary, we hope the learned decentralized policy could make the observation transition and reward predictable. That is, based on the learned pðaijoi;tÞ, there exists functions T 0 i and R0 i satisfy that:*

*由于 Ri 不仅依赖于 oi;t 和 ai，还依赖于由 T i 生成的 oi;tþ1 并与 ai 相关，因此从 Ri 中学习 pi 可能导致学习非稳态，因为代理在相同的观察下执行相同的动作可能会收到不同的奖励和观察转换。在这种情况下，当前代理的收到奖励和观察转换仅依赖于其自己的观察和执行的动作可能无法很好地预测。相反，为了避免这种非稳态，我们希望学到的分散策略能够使观察转换和奖励可预测。也就是说，基于学到的 p(ai|oi;t)，存在函数 T 0 i 和 R0 i 满足以下条件：*

![image-20231115193911964](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311151939041.png)

> 好像是想说，要考虑其他代理的动作



### 3.3 代理设计

*Each intersection in the system is controlled by an agent, and here we present the detailed definitions of the agent.*

+ *Observation. Each agent has its own local observation, including the number of vehicles on each incoming lane and the current phase of the intersection, where phase is the part of the signal cycle allocated to any combination of traffic movements, as explained in Section 3.1. Observation of agent i is defined by*

*Observation. 每个代理都有自己的局部观察，包括每个进口车道上的车辆数量以及交叉口当前的相位，其中相位是分配给任何组合交通流动的信号周期的一部分，如第 3.1 节所解释的那样。代理 i 的观察定义为：*

![image-20231115201140654](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311152011715.png)

*where M is the total number of incoming lanes and VM means the number of vehicles in the Mth incoming lanes, p is the current phase and represented as a one-hot vector*

*其中，M 是进口车道的总数，VM 表示第 M 条进口车道上的车辆数，p 是当前的相位，并表示为一个独热向量。*

+ *Action. At time t, each agent i chooses a phase p as its action ai, indicating the traffic signal should be set to phase p. Note that the phases may organize in a sequential way in reality, while directly selecting a phase makes the traffic control plan more flexible. In a grid road network, each agent has four phases as described in Section 3.1. For a complex and heterogeneous road network, we unite the phases of all structures together as the full action space, and mask the unavailable phases of each traffic signal as unavailable actions of the agent.*

*Action. 在时间 t，每个代理 i 选择一个相位 p 作为其动作 ai，表示交通信号应设置为相位 p。请注意，相位在现实中可能以顺序方式组织，而直接选择相位使得交通控制计划更加灵活。在网格道路网络中，每个代理有四个相位，如第 3.1 节所述。对于一个复杂且异构的道路网络，我们将所有结构的相位统一在一起作为完整的动作空间，并将每个交通信号的不可用相位标记为代理的不可用动作。*

+ *Reward. We define the reward for agent i as the negative of the queue length on incoming lanes. Note that optimizing queue length has been proved to be equivalent to optimizing average travel time in [38] under certain assumptions. Average travel time is a global criteria which cannot be optimized directly by an agent. Hence, queue length is widely used as reward in traffic signal control. Reward of agent i is defined by*

*Reward. 我们将代理 i 的奖励定义为进口车道上排队长度的负值。请注意，在某些假设下，优化排队长度已被证明等效于优化平均行车时间 [38]。平均行车时间是一个不能直接由代理进行优化的全局标准。因此，在交通信号控制中，排队长度被广泛用作奖励。代理 i 的奖励定义为：*

![image-20231115202239092](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311152022154.png)

*where qm is the queue length on the incoming lane m, and M is the total number of incoming lanes.*

*其中，qm 是进口车道 m 上的排队长度，M 是进口车道的总数。*



## 四、方法

*In this section, we propose Meta Variationally Intrinsic Motivated (MetaVIM) method to achieve Eqs. (1) and (2), as illustrated in Fig. 3. MetaVIM employs latent variable to represent each task to make the reward, observation transition and policy functions shareable. At the same time, MetaVIM makes the approximations and policy robust to neighbors by intrinsic reward design. From now on, we drop the sub- and superscript i to denote the current agent for ease of notation. We start by describing the overall architecture in Section 4.1, and then elaborate the policies with latent variable in Section 4.2 and intrinsic reward design in Section 4.3.*

*在本节中，我们提出了Meta Variationally Intrinsic Motivated（MetaVIM）方法，以实现方程（1）和（2），如图3所示。MetaVIM使用潜变量来表示每个任务，使得奖励、观察转换和策略函数可共享。同时，通过内在奖励设计，MetaVIM使得近似和策略对邻居具有鲁棒性。从现在开始，为了简化表示法，我们省略下标和上标i来表示当前代理。我们首先在第4.1节中描述整体架构，然后在第4.2节详细说明具有潜变量的策略，最后在第4.3节中描述内在奖励设计。*



### 4.1 模型

*MetaVIM consists of a multi-head VAE (mVAE) and a policy network, where the mVAE consists of an encoder and four decoders:*

*MetaVIM包括一个**多头变分自编码器（mVAE）**和一个策略网络，其中mVAE由一个编码器和四个解码器组成。*

> VAE：变分自编码器（详细看一下）

+ *Encoder. The encoder takes the past trajectories t:t up until t as input to calculate the latent variable m in Eq. (1), parameterised by c. The encoder could be:*

![image-20231116095852052](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311160958148.png)

*编码器。编码器以过去的轨迹  $\tau _{:t}$ 直到 t 作为输入，通过参数化为 $\psi$ （是这个编码器参数化为）计算方程（1）中的潜变量 m。编码器可以是：*

> 通过当前代理的过去轨迹的历史，计算潜变量m
>
> **$\tau _{:t}$  ：表示的是时间从0到t，还是只有t**

+ *Decoder. Four decoders are used to predict the environmental dynamics, and parameterised by fr; f~r; fo; f~o respectively, where the former two are used to predict next reward with/without neighbors and the latter two are used to predict next observation with/without neighbors:*

解码器。使用四个解码器来预测环境动态，分别由 fr; f~r; fo; f~o 参数化，其中前两个用于预测带/不带邻居的下一个奖励，而后两个用于预测带/不带邻居的下一个观察：

![image-20231116110625791](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311161106847.png)

*where j is any neighbor agent of current intersection, aj is the neighbor action, and m is the latent variable generated by the encoder*

这里，j 是当前交叉口的**任何邻居代理**，aj 是邻居的动作，而 m 是由编码器生成的潜变量。

> m 提供的是当前代理的历史轨迹

+ *Policy. The policy takes local observations ot and latent variable as input, parameterised by u and dependent on c, which is defined by*

*策略。策略以本地观察 ot 和潜变量作为输入，通过参数化为 u 并依赖于 c，其定义为：*

![image-20231116112607448](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311161126513.png)

*where m is derived by encoder. The policy is conditioned on both observation and posterior over m, which is similar to the formulation of Bayesian RL*

*其中 m 是由编码器生成的。该策略是在观察和潜变量 m 的后验上条件化的，这类似于贝叶斯强化学习的表述。*



### 4.2 潜变量

#### 4.2.1 设计

*From the perspective of meta-RL, there exists a true variable to represent a certain task (i.e., an agent). However, we do not have access to this information. Alternatively, we aim to learn variable m from trajectories of the current task.*

*从元强化学习的角度来看，存在一个真实的变量来表示特定的任务（即代理）。然而，我们无法访问这些信息。相反，我们的目标是从当前任务的轨迹中学习变量 m。*

*Considering the uncertainty of the task, we model the task as a Gaussian distribution N ðm; s2Þ, where the mean m and the standard deviation s are generated by the encoder.*

*Specifically, a RNN is employed to track the characteristics of trajectory t:t and calculate m and s respectively. Then, m is randomly sampled from N ðm; s2Þ, that is, m Nðm; s2Þ.Note m Nðm; s2Þ is underivable, we alternatively use the reparameterization technique [65] for backpropagation:*

*考虑到任务的不确定性，我们将任务建模为一个高斯分布 N(μ, σ^2)，其中均值 μ 和标准差 σ 由编码器生成。*

具体来说，我们使用一个循环神经网络（RNN）来追踪轨迹 t:t 的特征并分别计算 μ 和 σ。然后，从 N(μ, σ^2) 中随机采样得到 m，即 m ∼ N(μ, σ^2)。请注意，m ∼ N(μ, σ^2) 无法直接导数，我们使用替代的**反向传播技术 **[65] 进行反向传播：*



![image-20231116123415680](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311161234746.png)

*where m0 is randomly sampled from the standard normal distribution m0  Nðm; IÞ, and I is the identity matrix.*

*其中，m0 是从标准正态分布中随机采样得到的，即 m0 ∼ N(0, I)，其中 I 是单位矩阵。*

> 总结：
>
> ​	潜变量m：表示当前代理的历史轨迹
>
> ​	如何学习到m呢：任务建模为高斯分布，其均值和标准差由编码器（RNN）生成，从这个高斯分布中随机取样m。

#### 4.2.2 分析

*Besides making the meta-learning possible as shown in Eq. (1), the latent variable brings two additional advantages:*

*除了使元学习成为可能，如等式所示。（1），潜在变量带来了两个额外的优势：*



##### 1. Trade-off of Exploration and Exploitation

*Latent variable is an additional input of the policy network except for the observation, which could be regarded as prior knowledge about the task. Previous research [64] has shown that random Gaussian distribution could be regarded as the noise disturbance and provide randomness of the policy. Specifically, the regular policy network outputs action probability based on the current observation only, and the distribution of sampled action is certain once the current observation is given. In contrast, our method takes the latent variable m as the additional input of the policy network besides the observation, where m is randomly sampled from the learnable Gaussian distribution N ðm; d2Þ. Hence, m could bring randomness to the distribution of the sampled action in the learning procedure. Hence, it helps to explore the tasks.*

*Since our Gaussian distribution changes dynamically in training rather than keeps as constant. It not only helps to explore but also helps to trade off the exploration and exploitation [50]. With the converging of the model, the randomness gradually decreases. Specifically, the mean m tends to be stable and the variance d2 tends to zero. Fig. 4 illustrates the adaptation of latent variable. It has a dynamic guiding effect on policy: randomness is the largest at the beginning, the dispersive distribution of m necessitates the agent to perform diverse actions to explore, and then randomness decreases as the posterior distribution N ðm; s2Þ gradually converges. In this phase, the policy network encourages the agent to exploit the environment. In addition, it also makes the final learned policy stable with very little randomness. This can be analogous to the impact of -greedy in DQN on trade-off of the exploration and exploitation. The intuitive idea is: random noise (random latent variable) is equivalent to the fixed  in -greedy, and the learnable Gaussian distribution (learnable latent variable) is equivalent to the adjustable  in -greedy, which has a dynamic guiding effect on the trade-off of the exploration and exploitation.*

*潜变量是策略网络的额外输入，除了观察之外，它可以被视为关于任务的先验知识。先前的研究[64]表明，随机的高斯分布可以被视为噪声干扰，提供策略的随机性。具体而言，常规策略网络仅基于当前观察输出动作概率，一旦给定当前观察，采样动作的分布就是确定的。相反，我们的方法将潜变量 m 作为策略网络的附加输入，除了观察之外，其中 m 是从可学习的高斯分布 N(μ, d^2) 中随机采样的。因此，m 可以为学习过程中采样动作的分布引入随机性，有助于探索任务。*

*由于我们的高斯分布在训练中是动态变化的而不是保持不变的。这不仅有助于探索，还有助于权衡探索与利用[50]。随着模型的收敛，随机性逐渐减小。具体而言，均值 u 趋于稳定，方差 d^2 趋于零。图4展示了潜变量的适应过程。它对策略具有动态引导效果：在开始阶段随机性最大，m 的分散分布要求代理执行多样化的动作以进行探索，然后随着后验分布 N(μ, s^2) 逐渐收敛，随机性减小。在这个阶段，策略网络鼓励代理利用环境。此外，它还使最终学到的策略在很小的随机性下保持稳定。这可以类比于DQN中-greedy对探索和利用的权衡的影响。直观的想法是：随机噪声（随机潜变量）相当于-greedy中的固定  ε，而可学习的高斯分布（可学习潜变量）相当于-greedy中的可调节  ε，它对探索和利用的权衡具有动态引导效果。*

> 潜变量m带来的优势：
>
> ​	提供了随机性，平衡探索和利用。
>
> ​	一开始随机性大然后逐渐收敛



##### 2. Modelling Latent Coordination with Neighbors

*In our method, the neighbor information is available only in training, and the decoders are abandoned in execution. Only using individual observation as the input of policy may ignore the latent neighbors information. As shown in Eq. (1), the observation transition is caused by not only the current agent but also its neighbors. In turn, the latent variable could reflect the latent neighbor’s information.*

*在我们的方法中，邻居信息仅在训练中可用，解码器在执行阶段被放弃。仅使用个体观察作为策略输入可能会忽略潜在的邻居信息。如方程（1）所示，观察转换不仅是由当前代理引起的，还受到其邻居的影响。反过来，潜变量可以反映潜在的邻居信息。*

> 邻居信息仅在训练中可用，解码器在执行阶段被放弃
>
> **这一部分不太明白**
>
> 也许是：m是自己的动作-观察历史，因为观察不止由当前的代理引起还被邻居影响，所以这个潜变量可以反映邻居信息



### 4.3 内在奖励

#### 4.3.1 设计

*As stated in Eq. (2), the non-stationary learning often causes the observation transition and received rewards unpredictable only conditioned on individual observation and action. Conversely, we hope the learned policy makes them be predicted stably. To achieve this goal, we design a novel intrinsic reward based on VAE, as illustrated in Fig. 5, which is the negative of the prediction bias with/without neighbor agents’ policies:*

*正如方程（2）中所述，非平稳学习经常导致观察转换和接收奖励仅在个体观察和动作的条件下难以预测。相反，我们希望学到的策略使它们能够稳定地被预测。为了实现这一目标，我们设计了一种**基于VAE的新颖内在奖励**，如图5所示，它是带/不带邻居代理策略的预测偏差的负值：*

![image-20231116165507336](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311161655431.png)

> VAE：变分自编码器
>
> 基于VAE的**内在奖励**，四个解码器的输出计算而来

*where rtþ1 and r~j;tþ1 are the predicted rewards with/without neighbor agents’ policies respectively, otþ1 and o~j;tþ1 are predicted next observations with/without neighbor agent’s policies respectively. They are generated by the corresponding decoders in Eq. (6).*

*其中，rt+1 和 r~j;t+1 分别是带/不带邻居代理策略的预测奖励，ot+1 和 o~j;t+1 分别是带/不带邻居代理策略的预测下一观察，它们是由方程（6）中相应的解码器生成的。*



#### 4.3.2 分析

*Here, will give an interpretation from the perspective of information theory why the intrinsic reward design can lead to a stable policy robust to neighbors’ policy, which could make the learned policy easy to transfer. From the perspective of the current agent, the rtþ1 is defined as*

*在这里，我们将从信息论的角度来解释为什么内在奖励设计可以导致一个稳定的策略，对邻居的策略具有鲁棒性，这可以使学习到的策略易于转移。*

> 解释这个内在奖励为什么好



略





*The last approximation is because the predictions in our implementation are deterministic rather than stochastic. Thus, in expectation, the intrinsic reward is the negative of MI above. As each agent maximizes the long-term cumulative reward, which therefore minimizes MI. As a result, agents become independent. This can be an interpretation from the information-theoretical perspective. Note that the prediction results are only used to form intrinsic rewards, and our method tries to minimize them. That means our method mainly relies on the trend of change of predicted results, not the predicted value. Therefore, we expect our method is resilient to the decoders’ modeling error accumulation*

*
最后的近似是因为我们实现中的预测是确定性的，而不是随机的。

因此，在期望中，内在奖励是上述互信息的负值。由于每个代理都在最大化长期累积奖励，从而最小化互信息。因此，代理变得相互独立。这可以从信息理论的角度进行解释。请注意，预测结果仅用于形成内在奖励，而我们的方法试图将其最小化。这意味着我们的方法主要依赖于预测结果的变化趋势，而不是预测值。因此，我们期望我们的方法对于解码器建模误差的积累具有韧性。*



### 4.4 学习









## 五、实验

*We conduct the experiments on CityFlow [20], an city-level open-source simulation platform for traffic signal control.*

*The simulator is used as the environment to provide state for traffic signal control, the agents execute actions by changing the phase of traffic lights, and the simulator returns feedback. Specifically, we conduct additional experiments on another platform SUMO1 and under different lane configurations to demonstrate the robustness of the method in Appendix D and Appendix C respectively, available in the online supplemental material.*

*我们在 CityFlow [20] 上进行了实验，这是一个用于交通信号控制的城市级开源仿真平台。*

*该仿真器被用作交通信号控制的环境，代理通过改变交通灯的相位来执行动作，仿真器则提供反馈。具体而言，我们在另一个平台 SUMO1 上进行了额外的实验，并在不同的车道配置下进行了实验，以展示该方法的鲁棒性。详细内容可以在在线补充材料的附录 D 和附录 C 中找到。*

### 5.1 数据集

#### 5.1.1 道路网络

*The evaluation scenarios come from four real road network maps of different scales, including Hangzhou (China), Jinan (China), New York (USA) and Shenzhen (China), illustrated in Fig. 6. The road networks and data of Hangzhou, Jinan and New York are from the public datasets.2 The road network map of Shenzhen is made by ourselves which is derived from OpenStreetMap.3 The road networks of Jinan and Hangzhou contain 12 and 16 intersections in 4  3 and 4  4 grids, respectively. The road network of New York includes 48 intersections in 16  3 grid. The road network of Shenzhen contains 33 intersections, which is not grid compared to other three maps, illustrated in Fig. 7. In addition, an additional experiment are conducted on a much larger road network to validate the scalability, more details are listed in Appendix E, available in the online supplemental material.*

*评估场景来自不同规模的四个真实道路网络图，包括杭州（中国）、济南（中国）、纽约（美国）和深圳（中国），如图 6 所示。杭州、济南和纽约的道路网络和数据来自公共数据集。深圳的道路网络地图是我们根据 OpenStreetMap 制作的。济南和杭州的道路网络分别包含 12 个和 16 个十字路口，分别位于 4x3 和 4x4 的网格中。纽约的道路网络包含 48 个交叉口，位于 16x3 的网格中。深圳的道路网络包含 33 个交叉口，与其他三个地图相比，它的布局不是网格状，如图 7 所示。此外，为验证可扩展性，我们还在一个更大的道路网络上进行了额外的实验，详细内容请参见在线补充材料的附录 E。*



#### 5.1.2 交通流量配置

*We run the experiments under three traffic flow configurations: real traffic flow, mixed low traffic flow and mixed high traffic flow. The real traffic flow is real-world hourly statistical data with slight variance in vehicle arrival rates, as shown in Table 1.*

*Since the real-world strategies tend to break down during bottleneck period (peak hour), to better evaluate the performances of traffic light control methods in the flat-peak-flat scenario, we use two synthetic datasets: mixed high and mixed low traffic flow, which have a more dramatic variance in vehicle arrival rates, as shown in Table 2. A detailed description of traffic flow configurations is:*

*我们在三种交通流配置下进行实验：真实交通流、混合低交通流和混合高交通流。真实交通流是真实世界的小时统计数据，车辆到达率略有变化，如表1所示。*

*由于真实世界的策略在瓶颈时期（高峰时段）往往会失效，为了更好地评估交通灯控制方法在平峰时段内的性能，我们使用了两个合成数据集：混合高交通流和混合低交通流，它们在车辆到达率上具有更大的变化，如表2所示。交通流配置的详细描述是：*

![image-20231117172151943](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311171721103.png)

![image-20231117172239575](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311171722659.png)

+ *Real. The traffic flows of Hangzhou (China), Jinan (China) and New York (USA) are from the public datasets,4 which are processed from multiple sources. The traffic flow of Shenzhen (China) is made by ourselves generated based on the traffic trajectories collected from 80 red-light cameras and 16 monitoring cameras in a hour. The data statistics are listed in Tables 1 and 3.*

*真实数据。杭州（中国）、济南（中国）和纽约（美国）的交通流数据来自公共数据集[4]，这些数据集经过多个来源的处理。深圳（中国）的交通流数据由我们根据从80个红绿灯相机和16个监控相机中收集到的交通轨迹生成。数据统计见表1和表3。*

![image-20231117172929560](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311171729623.png)

+ *Mixedl. The mixedl is a mixed low traffic flow with a total flow of 2550 in one hour, to simulate a light peak. The arrival rate changes every 10 minutes, which is used to simulate the uneven traffic flow distribution in the real world, the details of the vehicle arrival rate and cumulative traffic flow are shown in Fig. 8.*

*混合低交通流（Mixedl）是一个在一小时内总流量为2550的混合低交通流，用于模拟交通流的轻峰期。到达率每10分钟变化一次，这用于模拟现实世界中交通流分布的不均匀性，车辆到达率和累积交通流的详细信息见图8。*



![image-20231117173334761](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202311171733860.png)

混流示意图。左图显示车辆到达率，右图显示一小时内累计到达车辆数量

+ *Mixedh. The mixedh is a mixed high traffic flow with a total flow of 4770 in one hour, in order to simulate a heavy peak. The difference from the mixedl setting is that the arrival rate of vehicles during 1200-1800s increased from 0.33 vehicles/s to 4.0 vehicles/s. The data statistics are listed in Table 2.*

*混合高交通流（Mixedh）是一个在一小时内总流量为4770的混合高交通流，以模拟交通流的高峰期。与混合低交通流设置的不同之处在于，在1200-1800秒的时间段内，车辆的到达率从0.33辆/秒增加到4.0辆/秒。数据统计见表2。*





*Following existing studies [13], [14], [40], [41], [46], we use the average travel time to evaluate the performance of different methods for traffic signal control. The average travel time indicates the overall traffic situation in an area over a period of time. For a detailed definition of average travel time, see Section 3.1. Since the number of vehicles and the origin-destination (OD) positions are fixed, better traffic signal control strategies result in less average travel time*

*类似于现有的研究[13]、[14]、[40]、[41]、[46]，我们使用平均行驶时间来评估不同交通信号控制方法的性能。平均行驶时间反映了一定时间内区域内的整体交通状况。有关平均行驶时间的详细定义，请参阅第3.1节。由于车辆数量和起点-终点（OD）位置固定，更好的交通信号控制策略导致较短的平均行驶时间。*



### 5.3 测试模型

*The method is evaluated in two modes: (1) Common Testing Mode: the model trained on one scenario with one traffic flow configuration is tested on the same scenario with the same configuration. It is used to validate the ability of the RL algorithm to find the optimal policy.*

*(2) Meta-Test Mode: we train the model in the Hangzhou road network and transfer the model to the other three networks directly. It is used to validate the generality of the model.*

*该方法在两种模式下进行评估：（1）通用测试模式：在一个场景上训练的模型，使用一个交通流配置，在相同的场景和配置下进行测试。这用于验证强化学习算法寻找最优策略的能力。*

*（2）元测试模式：我们在杭州道路网络中训练模型，然后直接将该模型转移到其他三个网络。这用于验证模型的普适性。*



### 5.4 基准

*Our method is compared with the following two categories of methods: conventional transportation methods and RL methods.5 Note that for a fair comparison all the RL methods are learned without any pre-trained parameters and the methods are evaluated under the same settings. The results are obtained by running the source codes.6 All the baselines are run with three random seeds, and the mean is taken as the final result. The action interval is five seconds for each method, and the horizon is 3600 seconds for each episode.*

*Specifically, the compared methods contain:*

*我们的方法与以下两类方法进行比较：传统交通方法和强化学习方法。请注意，为了进行公平比较，所有强化学习方法都是在没有任何预训练参数的情况下学习的，并且这些方法是在相同的设置下进行评估的。结果是通过运行源代码获得的。**所有基准方法都使用三个随机种子运行**，取平均值作为最终结果。每种方法的动作间隔为五秒，每个episode的时间跨度为3600秒。*

*具体而言，比较的方法包括：*

> 每5s调整动作，每个episode一小时

#### 5.4.1 传统方法

+ Random selects available phase randomly.

+ MaxPressure [2] is a leading conventional method, which greedily chooses the phase with the maximum pressure. The pressure is defined as the difference of vehicle density between the incoming lane and the outgoing lane.

+ Fixedtime [1] with random offset [72] executes each phase in a phase loop with a pre-defined span of phase duration, which is widely used for steady traffic.

+ FixedtimeOffset [1] where multiple intersections use the same synchronized fix-time plan. The offset is the time interval between the start time of the green light among intersections.

+ SlidingFormula [72] is designed based on the expert experience, which dynamically divides the duration of each phase according to the number of vehicle arriving.

+ SOTL [23] specifies a pre-defined threshold for the number of waiting vehicles on approaching lanes.Once the waiting vehicles exceeds the threshold, it will switch to the next phase.

- **Random**: 随机选择可用相位。

- **MaxPressure [2]**: MaxPressure 是一种领先的传统方法，它贪婪地选择具有最大压力的相位。这里压力定义为进口车道和出口车道之间的车辆密度差异。

- **Fixedtime [1] with random offset [72]**: Fixedtime 是一个带有随机偏移的固定时长的方法，它在一个相位循环中执行每个相位，这种方法在稳定交通流中广泛使用。

- **FixedtimeOffset [1]**: 多个交叉口使用相同的同步固定时间计划，偏移是交叉口之间绿灯开始时间的时间间隔。

- **SlidingFormula [72]**: SlidingFormula 是根据专家经验设计的，根据到达的车辆数量动态划分每个相位的持续时间。

- **SOTL [23]**: SOTL 指定了逼近车道上等待车辆数量的预定义阈值。一旦等待的车辆超过阈值，它将切换到下一阶段。

#### 5.4.1 基于强化学习方法

+ Individual RL [12] where each intersection is controlled by one agent. The replay buffer and network parameters are not shared, and the model update is independent. There is no information transfer between agents.

+ MetaLight [14] is a value-based meta RL method via parameter initialization based on MAML [73]. MetaLight is originally a single-agent approach for meta-learning on multiple separate tasks. Here we extend it to a multi-agent scenario without considering neighbor information.

+ PressLight [13] combines the traditional traffic method MaxPressure [2] with RL together, and optimizes the pressure of each intersection.

+ CoLight [46] uses graph convolution and attention mechanism to model the neighbor information, and then further uses this neighbor information to optimize the queue length.

+ GeneraLight [49] is a meta RL method which uses generative adversarial network to generate diverse traffic flows and uses them to build training environments.

- **Individual RL [12]**: 在这种方法中，每个交叉口由一个代理控制。回放缓冲区和网络参数不共享，模型更新是独立的。代理之间没有信息传递。

- **MetaLight [14]**: MetaLight 是一种基于 MAML [73] 的参数初始化的值基元强化学习方法。MetaLight 最初是一种用于在多个独立任务上进行元学习的单一代理方法。在这里，我们将其扩展到了一个考虑邻居信息的多代理场景。

- **PressLight [13]**: PressLight 结合了传统的交通方法 MaxPressure [2] 和强化学习，优化了每个交叉口的压力。

- **CoLight [46]**: CoLight 使用图卷积和注意力机制建模邻居信息，然后进一步利用这些邻居信息来优化队列长度。

- **GeneraLight [49]**: GeneraLight 是一种使用生成对抗网络生成多样化交通流并使用它们构建训练环境的元强化学习方法。

> 元强化学习



### 5.5 性能比较

#### 5.5.1 通用测试下的评估

这个模型good

#### 5.5.2 元测试下的评估

这个模型good

#### 5.5.3 消融实验

*To better validate the contribution of each component, four variants of MetaVIM are evaluated on the common testing mode in the Shenzhen road network, including:*

+ Base only keeps the policy network and removes the mVAE and latent variable m.

+ Base+m where the encoder and m are introduced.Base+m contains policy network and encoder, which keeps latent variable but discards intrinsic rewards.

+ Base+m+tran_RS contains policy network, encoder and transition decoders but discards reward decoders.Transition decoders pf0 ðotþ1Þ and pfo~ðo~tþ1Þ are used and only kotþ1  o~j;tþ1k is remained in Eq. (17) to form the intrinsic reward.

+ Base+m+rew_RS contains policy network, encoder and reward decoders but discards transition decoders. Reward encoders pfrðrtþ1Þ and pf~r ðr~tþ1Þ are used and only krtþ1  r~j;tþ1k is remained in Eq. (17) to form the intrinsic reward

*为了更好地验证每个组件的贡献，我们在深圳道路网络上评估了 MetaVIM 的四个变体，包括：*

+ **Base only**: 保留策略网络，移除了 mVAE 和潜变量 m。

+ **Base+m**: 引入了编码器和潜变量 m 的 Base+m。Base+m 包含策略网络和编码器，保留了潜变量但舍弃了内在奖励。

+ **Base+m+tran_RS**: 包含策略网络、编码器和转换解码器，但舍弃了奖励解码器。使用转换解码器 pf0 ðotþ1Þ 和 pfo~ðo~tþ1Þ，并且在方程（17）中仅保留 kotþ1  o~j;tþ1k 以形成内在奖励。

+ **Base+m+rew_RS**: 包含策略网络、编码器和奖励解码器，但舍弃了转换解码器。使用奖励解码器 pfrðrtþ1Þ 和 pf~r ðr~tþ1Þ，并且在方程（17）中仅保留 krtþ1  r~j;tþ1k 以形成内在奖励。

*The qualitative evaluation results are listed in Table 4 and the learning curves are shown in Appendix A, available in the online supplemental material. We can obtain the following findings: 1) Among these 5 models, the performance of Baseline is the worst. The reason is that it is hard to learn the effective decentralized policy independently in the multi-agent traffic signal control task, where one agent’s reward and transition are affected by its neighbors. 2) Compared with the baseline, the improvement of Baseline + m demonstrates the effectiveness of the latent variable m. The latent variable not only identifies the POMDP-specific information and helps to learn POMDP-shared policy network, but also trades off the exploration and exploitation during the RL procedure. 3) The tran_RS and rew_RS are both effective because each of them encourages the policy learning stable. Compared to them, the superiority of MetaVIM indicates tran_RS and rew_RS are complementary to each other.*

*4) Overall, all of the proposed components contribute positively to the final model.*

定性评估结果列在表4中，学习曲线显示在附录A中，可以在在线补充材料中找到。我们可以得出以下发现：
1) 在这5个模型中，Baseline的性能最差。原因在于在多代理交通信号控制任务中，很难独立学习有效的分散策略，其中一个代理的奖励和转换受其邻居的影响。
2) 与基线相比，Baseline + m 的改进证明了潜变量 m 的有效性。潜变量不仅识别了POMDP特定的信息并有助于学习POMDP共享的策略网络，还在强化学习过程中权衡了探索和开发。
3) tran_RS 和 rew_RS 都是有效的，因为它们各自鼓励策略学习的稳定性。与它们相比，MetaVIM的优越性表明 tran_RS 和 rew_RS 互补。
4) 总体而言，所有提出的组件都对最终模型产生了积极的贡献。



## 六、总结与未来方向

*In this paper, we propose a novel Meta RL method MetaVIM for multi-intersection traffic signal control, which can make the policy learned from a training scenario generalizable to new unseen scenarios. MetaVIM learns the decentralized policy for each intersection which considers neighbor information in a latent way. We conduct extensive experiments and demonstrate the superior performance of our method over the state-of-the-art. We have collected and released more complex scenarios containing different structures,7 and will improve the method based on these scenarios in the future. In addition, the utilization of latent variable in model-based RL for traffic signal control will also be explored to improve sample efficiency.*

在本文中，我们提出了一种新颖的元强化学习方法 MetaVIM，用于多交叉口交通信号控制，该方法可以使从训练场景中学到的策略推广到新的未见场景。MetaVIM 学习了每个交叉口的分散策略，以潜在方式考虑邻居信息。我们进行了大量实验，展示了我们的方法在最新技术上的卓越性能。我们已经收集并发布了包含不同结构的更复杂的场景[7]，并将基于这些场景改进该方法。此外，将探索在基于模型的强化学习中利用潜在变量来提高交通信号控制的样本效率。
