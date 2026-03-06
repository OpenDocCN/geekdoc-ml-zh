# 必须阅读关于认知雷达的论文

> 原文：<https://medium.com/mlearning-ai/must-read-papers-on-cognitive-radars-851de235acb?source=collection_archive---------2----------------------->

![](img/3dc800c41eae3165fc36965cd9d57800.png)

Photo by [Brent Olson](https://unsplash.com/@helixgames?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/competition?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

1.  **受约束的在线学习减轻脉冲捷变认知雷达中的失真效应(**[**【arXiv】**](https://arxiv.org/pdf/2010.15698)**)**

**作者:** [查尔斯·e·桑顿](https://arxiv.org/search/?searchtype=author&query=Thornton%2C+C+E)，[r·迈克尔·布勒](https://arxiv.org/search/?searchtype=author&query=Buehrer%2C+R+M)，[安东尼·f·马通](https://arxiv.org/search/?searchtype=author&query=Martone%2C+A+F)

**摘要:**脉冲捷变雷达系统在动态电磁环境中表现出良好的性能。然而，当使用脉冲多普勒处理时，在雷达的相干处理间隔内使用不相同的波形可能导致有害的失真效应。本文提出了一个在线学习框架，以优化检测性能，同时减轻有害的旁瓣电平。雷达波形选择过程被公式化为线性上下文 bandit 问题，在该问题中，超过预期失真的可容忍水平的波形适应被消除。通过在雷达-通信共存场景和存在故意自适应干扰的情况下的仿真，证明了约束在线学习方法是有效的和计算上可行的。这种方法适用于随机和敌对的上下文土匪学习模型和检测性能的动态场景进行了评估

**2。通过上下文 Thompson 采样(**[**【arXiv】**](http://This paper describes a sequential, or online, learning scheme for adaptive radar transmissions that facilitate spectrum sharing with a non-cooperative cellular network. First, the interference channel between the radar and a spatially distant cellular network is modeled. Then, a linear Contextual Bandit (CB) learning framework is applied to drive the radar's behavior. The fundamental trade-off between exploration and exploitation is balanced by a proposed Thompson Sampling (TS) algorithm, a pseudo-Bayesian approach which selects waveform parameters based on the posterior probability that a specific waveform is optimal, given discounted channel information as context. It is shown that the contextual TS approach converges more rapidly to behavior that minimizes mutual interference and maximizes spectrum utilization than comparable contextual bandit algorithms. Additionally, we show that the TS learning scheme results in a favorable SINR distribution compared to other online learning algorithms. Finally, the proposed TS algorithm is compared to a deep reinforcement learning model. We show that the TS algorithm maintains competitive performance with a more complex Deep Q-Network (DQN))**)**)进行认知雷达-细胞共存的高效在线学习

**作者:** [查尔斯·e·桑顿](https://arxiv.org/search/?searchtype=author&query=Thornton%2C+C+E)，[r·迈克尔·布勒](https://arxiv.org/search/?searchtype=author&query=Buehrer%2C+R+M)，[安东尼·f·马通](https://arxiv.org/search/?searchtype=author&query=Martone%2C+A+F)

**摘要:**本文描述了一种用于自适应雷达传输的顺序或在线学习方案，该方案促进了与非合作蜂窝网络的频谱共享。首先，对雷达和空间距离较远的蜂窝网络之间的干扰信道进行建模。然后，应用线性上下文 Bandit (CB)学习框架来驱动雷达的行为。勘探和开发之间的基本权衡通过所提出的 Thompson 采样(TS)算法来平衡，该算法是一种伪贝叶斯方法，其基于特定波形是最优的后验概率来选择波形参数，给定折扣信道信息作为上下文。已经表明，与可比较的上下文 bandit 算法相比，上下文 TS 方法更快地收敛到最小化相互干扰和最大化频谱利用的行为。此外，我们表明，与其他在线学习算法相比，TS 学习方案产生了有利的 SINR 分布。最后，将提出的 TS 算法与深度强化学习模型进行比较。我们证明了 TS 算法在更复杂的深度 Q 网络(DQN)中保持了竞争性能

**3。对抗性雷达推断。从逆跟踪到认知雷达的逆强化学习(arXiv)**

**作者:** [克里希那穆提](https://arxiv.org/search/?searchtype=author&query=Krishnamurthy%2C+V)

**摘要:**认知感知(Cognitive sensing)是指一种可重构的传感器，通过使用随机控制来动态适应其感知机制，以优化其感知资源。例如，认知雷达是复杂的动力系统；他们使用随机控制来感知环境，从中学习关于目标和背景的相关信息，然后调整雷达传感器以满足其任务的需要。过去二十年见证了认知/自适应雷达的深入研究。本文讨论解决下一个逻辑步骤，即逆认知传感。通过实时观察传感器(如雷达或一般的受控随机动态系统)的发射，我们如何检测传感器是否是认知的(理性效用最大化器)以及我们如何预测其未来的行为？科学挑战包括将贝叶斯滤波、逆向强化学习和动态系统的随机优化扩展到数据驱动的对抗环境。我们的方法超越了经典的统计信号处理(传感和估计/检测理论)，以解决如何从传感推断策略的更深层次的问题。生成模型、对抗推理算法和相关的数学分析将推动对复杂的自适应传感器(如认知雷达)如何工作的理解。

**4。认知雷达利用强化学习在汽车上的应用(**[**【arXiv】**](https://arxiv.org/abs/1904.10739)**)**

**作者:**[黄天耀](https://arxiv.org/search/?searchtype=author&query=Huang%2C+T)[陆宇翔](https://arxiv.org/search/?searchtype=author&query=Lu%2C+Y)

**摘要:**认知雷达(CR)的概念使得雷达系统能够通过从接收机到发射机的反馈设施来实现对可变环境的智能适应。然而，在快速变化的环境中实施 CR 通常需要一个众所周知的环境模型。在我们的工作中，我们使用 CR 和强化学习(RL)的结合来强调 CR 在未知环境中的学习能力，称为 RL-CR。需要更少或不需要环境模型。我们还将一般的 RL-CR 应用于汽车雷达频谱分配的具体问题，以减轻相互干扰。使用 RL-CR，每个车辆可以根据自己对环境的观察自主选择频率子带。由于与环境的整体信息相比，雷达的单次观测非常有限，因此利用了长短期记忆(LSTM)网络，以便雷达可以通过聚合其随时间的观测来决定下一个发射子带。与集中式频谱分配方法相比，我们的方法具有减少车辆和控制中心之间的通信的优点。在某些情况下，它在减少干扰方面也优于其他一些分布式频率子带选择策略。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)