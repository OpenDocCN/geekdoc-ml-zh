# 贝叶斯网络——推理模式

> 原文：<https://medium.com/mlearning-ai/bayesian-networks-reasoning-patterns-f238bb234a5f?source=collection_archive---------2----------------------->

在之前的一篇[文章](https://najamogeltoft.medium.com/the-bayesian-network-representation-parametrization-1b08b61dc145)中，我们看到了对贝叶斯网络的简要介绍。其中，我们看到它是一个简单的概率图形模型，通过一个有向无环图来表示一组变量及其条件依赖关系。我们还看到了一种参数化它们的方法，这样我们只需要考虑 n 个参数，而不是 2^n.。在本文中，我们将进一步了解贝叶斯网络中的*推理模式*和 *I-maps* 。

# 贝叶斯网络的因子分解