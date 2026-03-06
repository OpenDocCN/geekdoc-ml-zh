# 强化学习中超参数优化的群体算法

> 原文：<https://medium.com/mlearning-ai/population-based-algorithms-for-hyperparameter-optimization-in-reinforcement-learning-b04ce2165533?source=collection_archive---------2----------------------->

深度学习有望提供精确而强大的自动化系统来执行人类级别的智能任务。但在这场竞赛中，超参数优化是瓶颈。

所有深度学习算法都有一个底层的神经网络架构，由分层堆叠的神经元组成。每个神经元都有一个权重和一个偏差，称为参数。因此，在反向传播过程中，参数被迭代更新，直到我们得到满意的性能指标。但是……