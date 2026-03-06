# 决策树是如何工作的，它是如何选择属性来分割的

> 原文：<https://medium.com/mlearning-ai/how-do-decision-tree-work-and-how-it-choose-attribute-to-split-bea9eb1cfaeb?source=collection_archive---------4----------------------->

![](img/59f8f06967c49cacd9ce65bed0eb413f.png)

[https://unsplash.com/photos/OJo6TXpFGiY](https://unsplash.com/photos/OJo6TXpFGiY)

# 本文旨在介绍决策树，并说明它使用什么算法来拆分数据。

当我第一次在 sklearn 中使用`DecisionTreeClassifier()`时，我想到了一个问题“树如何知道要拆分哪个属性？”选择根节点的标准是什么？