# 预测养猪场的饲料消耗

> 原文：<https://medium.com/mlearning-ai/predicting-the-feed-consumption-of-a-swine-farm-81ce4f703f3a?source=collection_archive---------2----------------------->

## 在 R 中使用时间序列、线性混合模型和一般可加混合模型

在这篇博文中，我将向你展示如何使用几种不同类型的模型来预测养猪场的饲料曲线。当然，通往成功模式的道路不是技术本身，而是研究问题的有效性及其潜在的含义。

让我们看一下这个例子。因为这是商业数据，我不能与你分享这些数据。不过，如果您尝试将代码和工作方式附加到您自己的数据中，这对于学习练习来说总是最好的。任何数据集都可以，只要它有重复的观察。

因此，让我们从加载库和数据开始。

![](img/e57dd7f5b3f316ac3ddff01ed0107ae7.png)

As you can see, quite a nice dataset. It is not that clean, since it is commercial data. Clean data only exist in text-book examples.

![](img/5c4a1a3b3ac10c6cccf90d6787a9a353.png)

Data on three farms, and the stables / pens within each farm.

```
DataExplorer::plot_missing(combined4)
df <- combined4[,colSums(is.na(combined4))<nrow(combined4)]
DataExplorer::plot_missing(df)
table(df$ble_id)
table(df$vbf_id)
table(df$vbf_id, df$lcn_id)
```

![](img/d260392b308a8fe7f5d1dbb16c247edc.png)

Quite some missing data.

现在，在每一个围栏里都有不同的喂食周期。因此，如果我们想要分析日内曲线的函数，我需要通过添加另一个我称为 *newField 的层来进一步取消数据列表。*最后，我们有农场级、笔级和*纽菲尔德*(运行)级*的数据。*

![](img/f8a3ede31b0f570c9136720ab104f144.png)![](img/76c67b099289e793efe3df33ae61f4db.png)![](img/1075b05c8cedc7160e284bf1e5124300.png)

The original data, the run identifier added, and the daily feed provision. The first two plots to the left show the cumulated feed provision. In this post I will show you how to model the data on the right to mimic the data on the left.

让我们继续进行数据探索。千万不要认为这部分过程是浪费时间！它帮助了我很多很多次，通常是在我建立了第一套模型之后，帮助我理解为什么没有一个模型是有意义的。你会经常发现自己在剧情、模型、剧情、模型之间来来回回。

![](img/d4c744d86fc4b2b9af4c682cba251e60.png)![](img/567dd6bc50f1a7a7b647b92e0f242621.png)![](img/a989c2a19e944de638effeaa1c5b605a.png)![](img/83991f8e537abde37159cc2d834e3663.png)![](img/ce9e8ff0f76e3d6c87e5a37a4ad2eafb.png)![](img/6ef055d4214674fcd395a1e2a987ce0f.png)

Feed curves plotted by farm and pen. Either cumulated or not, looking at components or not. There is no shortage of data to say the least.

我对这类数据建模的第一直觉是使用时间序列。除了数据实际上是时间序列的性质之外，还有几种创建时间序列集合的方法可以挑选季节影响。所以，这是我首先尝试做的。找到任何一种时间模式。

![](img/19fb6f15221aab675cc8f604dd5e3a6c.png)

Time-series models do not like (some actually throw a temper tantrum) when you have missing data. So, I used the Kalman method of the [**imputeTS**](https://cran.r-project.org/web/packages/imputeTS/index.html) package to fill in the blanks.

![](img/8c3a3ad3b1fd781d32407b5bacb67c99.png)

Plotting a [**tsibble**](https://tsibble.tidyverts.org/).

现在，让我们使用这个 tsibble 并释放几个时间序列模型，包括一个[集合](https://blog.devgenius.io/ensemble-models-using-caret-in-r-d54e4e646968)。

![](img/048ebb48d75384abaf1e05beefaab0ed.png)

And then THIS is what happens. To be honest, I still have not figured out why it produces NULL models, even if I ask it to only run on feed curves that have at least 30m data points included. There should also be no missing data, but it just does not work. If you try to run forecasts on this list of models it will stop immediately since the first model on the list is actually not there. It is a list with holes.

最终，经过一番调整，我放弃了，求助于另一套模型——[混合模型](/mlearning-ai/introduction-to-mixed-models-in-r-9c017fd83a63)。我非常喜欢混合模型，因为它能够处理缺失数据(在某些假设下)，并且能够在嵌套的数据框架中对纵向数据进行建模。这正是我们现在所拥有的。

![](img/96ea4108e11123ce24313675e30b0c0a.png)

ANOVA to find the best possible model.

![](img/959af043dc63a795c18a1546982d3171.png)

Comparing five models. Model 4 has some serious issues despite having the lowest AIC. Just using the AIC to determine which model to use is not the best strategy as overfitting may easily take place.

让我们更仔细地看看模型 4 和模型 1。

![](img/faae1a37b775f4b9593744d69df64a7e.png)![](img/472eb27cf1a934e49603d7dfacf3e32b.png)![](img/0e2cdd94fe89dbf2c0e42514a62e3dfa.png)![](img/cad8b27d58afe687427fd5a9f9056eeb.png)![](img/acb79e3b3dc574ddcd71ff4ba8e3e901.png)![](img/994960ea1933f4c5f32083eb1431dec3.png)![](img/180d9a45b1fbcd53894590583a4a33ba.png)

Model 4 is not too bad to be honest, but I still do not like the standard error of the coefficient estimates.

![](img/1293276907675c4be9e579e9a27695f7.png)![](img/f000c89de45e35aa12294cb8e11a3126.png)

Negative predicted values of the cumulative feed provision is not what I want to look at. Random effects look okay.

![](img/ad67450ebb7a68007faf10461dbbcf54.png)![](img/6f98fd9f5467017ce457d4052de461c5.png)![](img/71081863c8551276b3a63fb13b798d62.png)

Residuals — total, across feed run (**newField**) and pen. Residuals by pen shows some funny values. These value may easily disturb the model.

![](img/8a6091494317a457c6b6800778e858fe.png)![](img/371580281221fb6db73ffebc615ce491.png)![](img/9c247514e47c22372e6f8670fca37c13.png)

Calibration plot to the left, residuals by time, and QQ-plot by feed run (**newField**). The calibration plot is not that bad actually, but the residuals by time shows some weird values. As if the **curve_day** variable itself is not really correct.

模型系数如下表所示。

![](img/18a72db31cbca4f29f7ec13355f1a222.png)

我已经给你们展示了一个校准图。让我们再看几个。老实说，校准图在使用和比较模型时并不方便，显示模型间残差的密度图会更好。然而，当我到这里的时候，我已经下定决心，线性混合模型不是使用的模型集。

![](img/5890d17d80332c3f4e2770c93d1cf1a3.png)![](img/d774bf621996610ea35195cedb3c4e9e.png)![](img/8c6e7890bd9687dea69486fc13a170ca.png)![](img/af748e8b84164c38a34e2143a0493414.png)

Different models reveal different colors which reveals different levels of accuracy for the model fit. It does not matter anymore to be honest, since I wanted to shift to a different set of models by this time anyhow.

我在上面做的是给线性混合模型添加样条——这是一个我已经在不同物种间采用了无数次的的[过程。同样，](/mlearning-ai/growth-modeling-in-ruminants-fbeb08fa58ab)[馈送数据](https://blog.devgenius.io/modelling-feed-intake-data-in-swine-1af3d72f263d)的建模也不是新的。但是这一次我想采用不同的建模方式，在混合模型格式中使用[通用附加模型](https://blog.devgenius.io/dose-response-response-surface-designs-and-splines-using-sas-42535db1302e)。此外，我想使用训练和测试来分割数据，以便更好地理解模型。

![](img/4b76fe82b7e9ef2cd891ec4c980db2b9.png)![](img/fd18f9c0f3756986062b9b9ccddc21c4.png)

Train and test data of selected pen’s. THIS is the level at which I will be modelling using GAMs in a Mixed Model structure.

![](img/fea552ea80bdbf2878c0e2ac1568ca56.png)

The model output.

![](img/a4c3c995f57f93d33b91527ce7d3ed23.png)

And the functions for each of the predictors included to model the cumulative feed curve.

![](img/1a52e7cc65274a0902e4d29a0a3cbc26.png)![](img/066487c9080ebfdb0b76580fb5702927.png)![](img/c0d2df20f2a1973c2ccd0fa4ea6ddef0.png)

Calibration plots. For some pens, the model performs much better then for other pens.

![](img/1fc97cdf14e30dac6abcfc00853084f3.png)![](img/daa2c52c6d616840413f33ea4fd879ff.png)

Residuals as density plots, and predictions per feed run (**newField**).

![](img/fd58705cf026755eb7e188cb84360c85.png)![](img/fedb992c60a7021a6579efa8e0f7f284.png)

Calibration plot, overall, per feed run. To the right, the observed and predicted feed curve per feed run. The GAMs do a very nice job, but there is some intermittent demend / provision going on that the GAMs do not directly pick up. I need to keep a careful eye on that.

因此，上述练习是针对提供的总累积饲料。但是，如果我们按成分来估计饲料供给呢？如你所见，我创建的所有模型都是在个体层面上运行的，然后将预测汇总，形成一条累积供给曲线。这似乎起了作用。它并不完美，但没有一个模型是完美的。

让我们试试组件 1 的情况。

![](img/b531e0c9b96150ba5754d8522b458dc2.png)![](img/adfa0c782120988e33bc3e09de34f32e.png)

Predictors and predictions. Not too bad.

![](img/a766f64d44dff0d151618f6c7990a1f8.png)![](img/a4e7cd6d19acf23d16451122a203d961.png)

Predictions on the individual and the cumulative level. Easy to see how the trained data is better then than the test data, but the test data is really not that bad. Especially if you will deploy this model and re-estimate predictions daily.

在组件 2 上。

![](img/47964d925b87ce283e9a35a448bce8aa.png)![](img/51053bf539e18ea8269153c91561a392.png)![](img/92008a30ca6aef294b2600e42ee5eccb.png)![](img/f3e976aec886d169030546bb33079ed2.png)

A bit more tricky, but it does seem to do the trick. Empty oberserved values are true empty values of component 2\. The model keeps predicting though because component 1 is a predictor of component 2.

现在，让我们把成分 3 加入到混合物中，把所有的东西加在一起，看看预测结果如何。

![](img/c1f19271e5da95229fdf6a64e1e1a153.png)

Component 1 and 2 together is really not that bad!

![](img/af730ad9e7c7afd44e165ed3391c4ad9.png)![](img/2e7dc5056af415984dde24e51aecd7f8.png)

Not bad at all! Okay, it is not perfect, but we are able to do it!

所以，这是一个小例子，说明如何使用一般的加性混合模型来估计养猪场的饲料曲线。继续，因为这只是一个很小的开始！

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) 

🔵 [**成为作家**](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)