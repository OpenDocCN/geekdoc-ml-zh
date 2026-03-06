# 贝叶斯推理介绍——定义最小均方误差和 LLMS 估计

> 原文：<https://medium.com/mlearning-ai/an-introduction-to-bayesian-inference-part-3-3109dd7abd3a?source=collection_archive---------2----------------------->

在上一篇[文章](https://najamogeltoft.medium.com/an-introduction-to-bayesian-inference-part-2-e15df4faa56c)中，我们谈到了用于选择最优参数θ*以最大化*后验*分布的 MAP 估计。我们还看到了两个如何做到这一点的例子，一个是已知方差的正态分布的均值，另一个是估计给定硬币的偏差。在本文中，我们将更多地讨论*贝叶斯最小均方估计*和*贝叶斯线性最小均方估计【LLMS】*。

## 重新审视条件期望