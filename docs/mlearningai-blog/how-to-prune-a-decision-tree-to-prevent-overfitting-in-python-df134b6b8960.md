# 如何修剪决策树以防止 Python 中的过度拟合

> 原文：<https://medium.com/mlearning-ai/how-to-prune-a-decision-tree-to-prevent-overfitting-in-python-df134b6b8960?source=collection_archive---------2----------------------->

![](img/f67cfbb97a6a00c63b7afebd9b0e747c.png)

我很感兴趣地了解到 sklearn 的决策树算法在其编码中有几个防止过拟合的参数。这些参数中的一些是 min_leaf_sample 和 max_depth，它们一起工作以防止在训练数据时过度拟合。成本复杂性修剪提供了另一种选择来控制树的大小。这种修剪技术由成本参数化…