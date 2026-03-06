# 用于强化学习的部分观察马尔可夫决策过程

> 原文：<https://medium.com/mlearning-ai/partially-observed-markov-decision-processes-pomdps-for-reinforcement-learning-rl-437b8ae412dd?source=collection_archive---------3----------------------->

![](img/2fb4d3703ff58eb0f611d2a084ec0cb5.png)

Photo by [Katie Moum](https://unsplash.com/@katiemoum?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/uncertainty?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在强化学习(RL)的初级模型下，你可能学习了马尔可夫决策过程(MDP)。这个模型只有一个主要问题。实际上，代理很少知道所有时间的全部状态。我们研究人员说，状态对代理人来说是部分可观察的。由于大多数人固有的这种缺陷…