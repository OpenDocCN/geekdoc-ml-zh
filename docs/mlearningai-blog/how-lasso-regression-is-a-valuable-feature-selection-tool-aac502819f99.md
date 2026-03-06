# 套索回归如何成为一个有价值的特征选择工具

> 原文：<https://medium.com/mlearning-ai/how-lasso-regression-is-a-valuable-feature-selection-tool-aac502819f99?source=collection_archive---------1----------------------->

![](img/325292a7e37b31cbfe010b07931f9b40.png)

我发现的一件有趣的事情是，在对数据集进行预测时，可以使用线性回归模型 Lasso 来选择要素。这是因为 Lasso 对模型参数的绝对值之和施加了一个约束:该和必须小于一个固定值(上限)。为了做到这一点，该方法应用了收缩(正则化)…