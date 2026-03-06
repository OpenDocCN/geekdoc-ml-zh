# 作为最小二乘问题的线性回归

> 原文：<https://medium.com/mlearning-ai/linear-regression-as-a-least-square-problem-fc13162eb518?source=collection_archive---------3----------------------->

![](img/f347efc2f50503e4758dfdda29c127cd.png)

Photo by [Jess Bailey](https://unsplash.com/@jessbaileydesigns?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 一步一步推导

遵循 [Leo Breiman 的*统计建模:两种文化*](http://staff.pubhealth.ku.dk/~tag/Teaching/share/material/Breiman-two-cultures.pdf) ，我们想找到一个作用于 X 的函数 f 来预测响应 Y:

这里我们着重于推导确定线性回归系数最佳值的方程，不考虑允许统计分析的条件