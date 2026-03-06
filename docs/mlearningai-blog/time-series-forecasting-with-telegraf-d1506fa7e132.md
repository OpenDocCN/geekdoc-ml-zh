# 用 Telegraf 进行时间序列预测

> 原文：<https://medium.com/mlearning-ai/time-series-forecasting-with-telegraf-d1506fa7e132?source=collection_archive---------1----------------------->

如果您熟悉 Telegraf，您会知道您可以使用一个 TOML 配置文件轻松配置这个轻量级收集代理，从超过 [180 个输入](https://github.com/influxdata/telegraf/tree/master/plugins/inputs)中收集指标，并将数据写入各种不同的输出和/或平台。您可能还知道 Telegraf 可以充当处理器、聚合器、解析器和串行化器。例如，你甚至可能熟悉 [Starlark 处理器插件](https://www.influxdata.com/blog/how-use-starlark-telegraf/)，它让你能够在 Telegraf 中执行各种数学运算。