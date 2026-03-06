# 解释预测 PySpark 的 Shapley 值

> 原文：<https://medium.com/mlearning-ai/machine-learning-interpretability-shapley-values-with-pyspark-16ffd87227e3?source=collection_archive---------0----------------------->

## 解读隔离森林的预测——不仅仅是

![](img/e15bca0aa6f1e274b2c12a0e63648dd4.png)

Photo by [Joshua Golde](https://unsplash.com/@joshgmit?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 问题是:如何解读《隔离森林》的预测

更具体地说，如何告诉**哪些特征对预测的贡献更大**。因为隔离林**不是典型的决策树**(参见…