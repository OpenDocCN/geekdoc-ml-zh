# 理解马尔可夫网络中的独立性

> 原文：<https://medium.com/mlearning-ai/understanding-independencies-in-markov-networks-de180b0aa6ed?source=collection_archive---------5----------------------->

在前一篇文章中，我们看了一下[马尔可夫网络](https://najamogeltoft.medium.com/undirected-graphical-models-parameterization-of-markov-models-488c5359f5fd)以及它们如何被参数化。在本文中，我们将快速浏览一下作为独立性断言表示的马尔可夫网络。

## 马尔可夫网络中的独立性

与[贝叶斯网络](/mlearning-ai/bayesian-networks-independencies-and-i-maps-519173977798)一样，马尔可夫网络也对独立性假设进行编码。然而，这在概念上更容易在马尔可夫网络中理解。直觉上，概率影响沿着图中的无向路径传播——如果我们…