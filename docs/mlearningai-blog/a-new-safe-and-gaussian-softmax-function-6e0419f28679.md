# 小数据集概率估计的逻辑函数的频率修正

> 原文：<https://medium.com/mlearning-ai/a-new-safe-and-gaussian-softmax-function-6e0419f28679?source=collection_archive---------10----------------------->

![](img/cdf57e03d8298d2e6f3277e3876a902c.png)

Photo by [Riho Kroll](https://unsplash.com/@rihok?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

逻辑函数在机器学习中无处不在——当你远离分类边界时，它假设可能性函数有一个修正的指数尾部(详见[这个](/@alexroberts_50190/a-different-way-to-think-about-logistic-regression-b323f3369979)故事):