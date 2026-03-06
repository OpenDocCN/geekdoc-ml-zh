# [教程]使用 PYTHON 对 POSITIVES COVID 19 PERU 进行基本数据分析

> 原文：<https://medium.com/mlearning-ai/tutorial-a-basic-data-analysis-with-positives-covid-19-peru-using-python-a9c4a2532282?source=collection_archive---------5----------------------->

## 截至 2010 年 8 月 20 日，covid 19 秘鲁数据的数据分析。使用 Pandas、Matplotlib、Seaborn 等库。

![](img/df63d5131fe1691e1af95540ed4d08c1.png)

我们目前正经历一个无法预料的情况。没错，我指的是 covid 19 疫情。不幸的是，到目前为止，很多国家都有很多人被感染和死亡。

好吧，让我们回到过去。今天是 19-08-20。因此，您在 MINSA(秘鲁卫生部)工作，您的老板让您分析 Covid 19 迄今为止的数据，找出哪些部门、省份和地区的阳性病例数最高，以便提供生物安全措施。

> *数据由 Kaggle 提供，可在此链接找到:* [秘鲁新冠肺炎| Kaggle](https://www.kaggle.com/martinclark/peru-covid19-august-2020)
> 
> *可以在此链接访问* ***我的笔记本****:*[Alex Roman 938/POSITIVES-新冠肺炎-秘鲁-基础-分析:大家好，这里是我用 python 编写的关于基础分析教程的代码。(github.com)](https://github.com/AlexRoman938/POSITIVES-COVID-19-PERU-BASIC-ANALYSIS)

# 步骤 1:让我们看看秘鲁 covid 19 数据框架

首先，你要选择的数据集是 **positivos_covid.csv** ，它会在名为 **df_positives** 的变量中。

接下来，我们来看看 **df_positives 及其信息。**

你能看出哪里有问题吗？您发现“FECHA_RESULTADO”列的格式对您没有帮助。还有，它的 dtype 是 int64，不应该是日期类型吗？。

因此，您需要在下一步处理“FECHA _ result ado”…

# 步骤 2:数据转换

现在，您将从 int64 转换为字符串列“FECHA_RESULTADO”。接下来，您将应用方法 **"pd.to_datetime"** 来转换日期时间数据。

> 最后一个非常重要，因为它将帮助您创建一个新列“月”。

如果你想看到变化，那就看数据框。

# 第三步:数据分析

该是你思考的时候了…你将如何知道哪些部门、省份、地区等。是阳性率最高的。

你可以从基本问题开始。

> Q1:科室十大正面案例是什么？
> 
> Q2:各省的十大正面案例是什么？
> 
> 问题 3:地区十大正面案例是什么？

Q1:部门的十大正面案例是什么？

你可以看到在利马有很多阳性病例。它与其他部门有很大的不同。

巨大的差异可能是因为根据 2007 年的人口普查和 2017 年的人口估计，利马是秘鲁人口最多的城市。

**Q2:各省的十大阳性案例是什么？**

**Q3:地区十大正面案例有哪些？**

你可以看到几乎所有的区都属于利马…这是因为在利马省有很多阳性病例[你在问题 1 中看到]。

目前，您已经回答了 3 个问题。但是，你想知道更多。然后，**你决定查看你的工程量数据，并沿着顶部区域**的时间进行分析。

**年龄分布**

你可以看到 50%的阳性病例年龄都不超过 42 岁。也有超过 100 岁的人被感染。

**性别分布**

男性阳性病例比女性阳性病例多。

**顶区沿时间的分析**

你可以看到每个月的热门地区的阳性病例。

积分在 8 月份下降，因为这个月还有 12 天。

# 结论

在分析期间，你想知道秘鲁的 covid 19 阳性病例的情况。根据调查结果，你可以向你的老板建议:

*   在前 10 个部门和省份实施严厉措施，例如继续隔离、增加宵禁时间等。特别是利马。
*   增加前 10 个区的警察和军队人数。让市民遵守卫生措施。尤其是圣胡安·德·卢里甘乔，6 月至 7 月为最新上升趋势。

# 推荐

这是一个基本的分析，所以我们建议进一步分析数据。多问自己一些问题。

记住数据集截止到 2020 年 8 月 19 日。然后，我们建议您选择当前数据集。

> *最后，感谢您阅读本帖。如果你想联系我。这是我的 LinkedIn:* [*亚历山大丹尼尔罗曼加布里埃尔| LinkedIn*](https://www.linkedin.com/in/alexanderdroman/)

# 参考

[按人口统计的秘鲁地区列表—维基百科](https://en.wikipedia.org/wiki/List_of_regions_of_Peru_by_population)

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)