# 利用脸书预言家预测天然气产量

> 原文：<https://medium.com/mlearning-ai/forecast-using-prophet-canadian-natural-gas-production-dataset-b1f9c57548d8?source=collection_archive---------3----------------------->

在本例中，我们将尝试采用基于纯数据的方法，使用脸书的 Prophet library 预测加拿大的天然气产量。这篇文章的想法是使用一个单变量时间序列数据集，并产生一个最佳拟合模型，使我们能够充满信心地预测未来的产量。我们将使用该模型预测未来 10 年的产量，即直到 2030 年。

**导入数据和库**

> 我们将从导入 Python 中的各种库开始，比如 fbprophet、numpy、pandas…