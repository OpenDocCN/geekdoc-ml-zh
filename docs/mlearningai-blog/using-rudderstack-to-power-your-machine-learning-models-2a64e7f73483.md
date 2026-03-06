# 使用方向舵堆栈为您的机器学习模型提供动力

> 原文：<https://medium.com/mlearning-ai/using-rudderstack-to-power-your-machine-learning-models-2a64e7f73483?source=collection_archive---------3----------------------->

![](img/993fcdec0eb90ffb3b834b7b3309f231.png)

在 RudderStack，我们经常谈论数据、分析、技术工具，偶尔还会谈论足球和板球。我们自称是数据迷，没有什么比机器学习这个话题更能让我们兴奋了。对我们来说，机器学习将工程、编程和分析等关键科学与直觉、经验、语言和行为心理学等创造性(有些人可能认为是黑暗的)艺术结合在一起。但是，建立一个利用和集成人工智能或机器学习的系统是很难的，有时从编程和数据收集的角度来看，都感觉势不可挡。

毫不奇怪，我们在这些贡献领域中有许多专家，因此能够原型化一些机器学习模型，并支持我们的许多客户构建他们自己的模型。我们在 RudderStack 的目标之一是大大降低希望在其组织内应用机器学习的团队的准入门槛。这里有几个例子来说明如何使用 RudderStack 来利用自己的数据释放 ML 的威力。

# 使用谷歌的情感分析人工智能丰富 Zendesk

RudderStack 的团队构建了一个示例项目，概述了如何使用谷歌的自然语言 API 工具，通过使用[谷歌的情感分析](https://cloud.google.com/natural-language/docs/analyzing-sentiment)方法来丰富 Zendesk 的入站门票。Google 情感 API 允许您访问现成的、经过预先训练的自然语言处理模型，为您的组织轻松执行情感分析。您可以通过 RudderStack 的[转换](https://docs.rudderstack.com/adding-a-new-user-transformation-in-rudderstack)实时利用该 API 来丰富入站跟踪和识别呼叫。我们将很快进行一次深入的演练，所以如果你想对我们正在整理的分步指南有所了解，请给我们发送一个 slack(在此加入)或在 [Twitter](https://twitter.com/RudderStack) 上联系我们！

# 预测永利赌场游戏应用的客户流失

我们最近写了一篇博文，讲述了 RudderStack 如何帮助鱼雷实验室预测永利赌场应用的玩家流失。鱼雷使用 Rudderstack 来捕获玩家事件数据，并将其转发给 BigQuery。在 BigQuery 中，使用 Tracks 表中的数据和定制的事件评分来训练和测试标准线性回归模型。最终结果是客户流失率大幅降低，日收入和玩家参与度增加。

# LoveHolidays 酒店建议

[LoveHolidays](https://www.loveholidays.com/) 是一家英国在线旅游预订网站，使用 RudderStack 从其网站和移动应用程序向谷歌 BigQuery 传输用户活动数据。他们用 Python 建立了一个机器学习算法，根据个人旅行者之前的搜索历史来预测哪个度假酒店最适合他或她。该模型每 24 小时重新训练一次，训练结果通过 RudderStack 仓库操作输入 Redis 数据库。当用户返回网站或应用程序时，可用属性的建议可以立即得到推广和/或改进，从而提供更相关的用户体验，而无需直接搜索。对于 Loveholidays，结果是更高的每用户搜索预订率和更低的每用户搜索率。

如果你或你的团队想了解更多关于 RudderStack 如何支持你的 AI/ML 需求，或者想看 RudderStack 的演示，请订阅我们下面的产品简讯，或者通过 [Slack](https://resources.rudderstack.com/join-rudderstack-slack) 与我们联系，以确保你了解我们正在做的事情。

这篇博客最初发表于:
[https://rudder stack . com/blog/using-rudder stack-to-power-your-machine-learning-models](https://rudderstack.com/blog/using-rudderstack-to-power-your-machine-learning-models)