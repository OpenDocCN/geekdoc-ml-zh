# 无向图形模型——马尔可夫模型的参数化

> 原文：<https://medium.com/mlearning-ai/undirected-graphical-models-parameterization-of-markov-models-488c5359f5fd?source=collection_archive---------2----------------------->

在以前的文章中，我们已经研究了贝叶斯网络，它是有向的图形模型。我们已经通过[参数化](/mlearning-ai/the-bayesian-network-representation-parametrization-1b08b61dc145)和关于它们的各种主题看了它们的表现。在这些主题中有[推理模式](/mlearning-ai/bayesian-networks-reasoning-patterns-f238bb234a5f)、 [I-maps](/mlearning-ai/bayesian-networks-independencies-and-i-maps-519173977798) 和[d-分离](https://najamogeltoft.medium.com/bayesian-networks-d-separation-e2a8f483b721)。在这一系列文章中，我们将看看无向图形模型，以及它们如何在独立性结构和推理任务方面提供一个不同的、通常更简单的视角。

## 无向图形的动机…