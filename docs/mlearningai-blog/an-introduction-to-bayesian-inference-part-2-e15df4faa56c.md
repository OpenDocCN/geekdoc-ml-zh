# 贝叶斯推理介绍——定义 MAP 估计

> 原文：<https://medium.com/mlearning-ai/an-introduction-to-bayesian-inference-part-2-e15df4faa56c?source=collection_archive---------0----------------------->

在上一篇[文章](https://najamogeltoft.medium.com/an-introduction-to-bayesian-inference-part-1-d5de099b8e73)中，我们看到了频率主义者和贝叶斯推理的区别，以及贝叶斯推理有多少是基于贝叶斯定理的。我们还看到了一些贝叶斯推理的具体例子，例如估计正态分布的平均值和估计硬币的偏差。在本文中，我们将仔细研究贝叶斯推理中的一个中心方法:*【最大后验概率(MAP)】*。

## 重访最大似然估计