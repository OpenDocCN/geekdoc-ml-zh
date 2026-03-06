# 贝叶斯网络——D-分离和 I-等价

> 原文：<https://medium.com/mlearning-ai/bayesian-networks-d-separation-e2a8f483b721?source=collection_archive---------3----------------------->

在以前的文章中，我们一直在研究与贝叶斯网络相关的许多不同概念:如何以一种[紧凑的方式表示它们，](/mlearning-ai/the-bayesian-network-representation-parametrization-1b08b61dc145)它们的[推理模式](/mlearning-ai/bayesian-networks-reasoning-patterns-f238bb234a5f)，以及 [I-Map](https://najamogeltoft.medium.com/bayesian-networks-independencies-and-i-maps-519173977798) 。在本文中，我们将研究 D-分离，并尝试从直观和数学两方面理解它。

## 了解 D 分离

那么，在贝叶斯网络的上下文中，D-分离到底是什么？它能用来做什么？简单地说，这是一个更正式的确定独立性的程序。换句话说，如果…