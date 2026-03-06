# 为什么 k 倍交叉验证在金融学中是失败的？

> 原文：<https://medium.com/mlearning-ai/why-k-fold-cross-validation-is-failing-in-finance-65c895e83fdf?source=collection_archive---------8----------------------->

## 清除和禁运

![](img/0b3fceb3a2f6a1e2c29d93db9763f9cc.png)

Photo by [Shubham Dhage](https://unsplash.com/@theshubhamdhage?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

> Marcos López de Prado 的《金融机器学习的进展》中描述了这种方法。

## 什么是交叉验证？

当我们想要训练机器学习模型时，通常我们必须将数据集分成三个部分:

*   火车