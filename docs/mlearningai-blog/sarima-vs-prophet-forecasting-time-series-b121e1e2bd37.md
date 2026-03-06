# 先知 vs 萨里玛—时间序列预测

> 原文：<https://medium.com/mlearning-ai/sarima-vs-prophet-forecasting-time-series-b121e1e2bd37?source=collection_archive---------0----------------------->

在分别写了一篇关于 Prophet 和 SARIMA 的文章后，我认为通过在同一个数据集上构建两个模型来比较预测会很有趣。在这篇文章中，我将尝试预测美国工业生产指数，这是一个衡量美国制造业、采矿业、电力和天然气公用事业所有设施实际产出的经济指标。两个模型的输出将使用 R 平方值(相关强度)和平均绝对误差(一组预测中误差的平均大小)和均方根值进行比较