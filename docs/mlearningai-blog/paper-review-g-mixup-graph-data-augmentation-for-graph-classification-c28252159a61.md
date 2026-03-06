# 论文综述:G-Mixup:用于图分类的图数据扩充

> 原文：<https://medium.com/mlearning-ai/paper-review-g-mixup-graph-data-augmentation-for-graph-classification-c28252159a61?source=collection_archive---------6----------------------->

在这个快速回顾中，我们将谈论[**G-mix**](https://arxiv.org/abs/2202.07179)，这是一种新的数据增强机制，它通过将[*mix*](https://arxiv.org/abs/1710.09412)*应用于估计的* [*图*](https://arxiv.org/pdf/1611.00718.pdf) 来提高[图神经网络](https://distill.pub/2021/gnn-intro/) (GNNs)的泛化能力。

![](img/9b842edd6b7e1f8e954952fc6787158c.png)

Graphs **G** = (**V**, **E**), where **V** is the set of nodes/vertices in the graph, and **E** is the set of edges, or connections, between these nodes. Photo by [JJ Ying](https://unsplash.com/@jjying?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral).

## 概观

**标签:**图形神经网络，数据增强，监督机器学习，图形