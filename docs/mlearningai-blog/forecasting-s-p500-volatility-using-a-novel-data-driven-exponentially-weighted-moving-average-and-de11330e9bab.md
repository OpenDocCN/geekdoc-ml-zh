# 使用新的数据驱动的指数加权移动平均预测 S&P500 波动率并与 ARMA & GARCH 模型比较

> 原文：<https://medium.com/mlearning-ai/forecasting-s-p500-volatility-using-a-novel-data-driven-exponentially-weighted-moving-average-and-de11330e9bab?source=collection_archive---------1----------------------->

# 介绍

我将介绍一种新的波动预测方法，它可以应用于任何平稳的时间序列。这种新方法首次在“一种使用数据驱动的创新波动的新型算法交易策略”中介绍，用 DD-EWMA 表示。在这个例子中，我们将关注 S&P500 的波动性。DD-EWMA 方法要求给定的时间序列是…