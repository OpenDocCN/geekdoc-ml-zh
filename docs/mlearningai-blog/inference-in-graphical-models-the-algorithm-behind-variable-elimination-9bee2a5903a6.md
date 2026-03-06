# 图形模型中的推理——变量消去背后的算法

> 原文：<https://medium.com/mlearning-ai/inference-in-graphical-models-the-algorithm-behind-variable-elimination-9bee2a5903a6?source=collection_archive---------4----------------------->

在上一篇文章中，我们看到了[变量消去](/p/a6bf9b70e946)背后的一个大概思路。在本文中，我们将继续学习这些知识，并介绍其背后的算法。这些算法可以被看作是对[因子](/p/488c5359f5fd)的操纵，也就是我们在马尔可夫网络中已经了解过的概念。这也意味着我们将能够把变量消去法与马尔可夫网络和贝叶斯网络联系起来。

## 要素边缘化