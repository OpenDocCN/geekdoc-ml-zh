# 使用自动 ARIMA + PROPHET + LightGBM 的时间序列预测

> 原文：<https://medium.com/mlearning-ai/time-series-forecasting-using-auto-arima-prophet-lightgbm-6362ef486c95?source=collection_archive---------0----------------------->

## 使用机器学习预测 Nifty 50 股票的价格

![](img/2e58a93dadfa46ebec5fa237b6c69dc9.png)

Photo by [Nick Chong](https://unsplash.com/@nick604?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在这篇文章中，我展示了一种开始构建可以预测值的机器学习模型的方法，以及它与受监督的机器学习模型有何不同。作为额外的一课，我还向你展示了如何使用 plotly 构建交互式视觉效果。我们开始吧

## 什么是时间序列预测？

顾名思义，时间序列是在一段时间内观察到的有序集合。由于时间序列包含在连续时间段映射的连续数据点，它可以是进行预测的非常重要的工具。它的一些主要应用领域包括股票和金融交易，分析在线和离线零售，以及医疗记录，如心率、EKG、MRI 和 ECG。

数据科学家对时间序列数据集的发展充满热情。它们是处理时间序列问题的许多不同方法。下面的笔记本中探讨了以下模型

*   ARIMA 汽车
*   先知
*   LightGBM

# 目录

1.  [关于数据集](#254a)
2.  [加载数据集预处理](#388d)
3.  [解释性数据分析](#b437)
4.  [特色工程](#c464)
5.  [静止转换](#4f0c)
6.  [ARIMA 和 ARIMA 汽车](#dfcb)
7.  [脸书先知](#9693)
8.  [助推树木](#1a09)
9.  [总结](#bc2a)
10.  [未来工作](#399e)
11.  [参考](#842e)

# №1:关于数据集

[*转到目录*](#546d)

该数据是来自印度 NSE(国家证券交易所)的 NIFTY 50 指数中的 50 只股票的价格历史和交易量。所有数据集都是一天级别的，定价和交易值是分开的。cvs 文件以及一个元数据文件，该文件包含一些关于股票本身的宏观信息。数据跨度从 2000 年 1 月 1 日到 2021 年 4 月 30 日。

该数据集包含以下功能

*   日期—交易日
*   符号—股票名称
*   Prev Close —股票前一天的收盘价
*   开盘——给定日期的开盘价
*   高—给定日期的最高价格
*   Low 给定日期的最低价格
*   最后——给定日期的最后价格
*   收盘——给定日期的收盘价
*   VWAP——全天交易股票的平均价格

因为我们有 50 只股票的数据，所以每次选择一只股票进行分析是有意义的，因此我选择 HDFC 银行进行分析

# №2:加载数据集预处理

[*转到 TOC*](#546d)

我们使用定制的库 opendatasets 从 [Kaggle](https://jovian.ai/outlink?url=https%3A%2F%2Fwww.kaggle.com%2Fdatasets) 加载数据集。

我还导入其他标准导入来执行数据分析、模型构建等

以下是为处理数据集而导入的 python 库列表

要从 Kaggle 下载数据集，您需要从 Kaggle 网站生成 API 密钥。你可以通过这篇[文章](https://jovian.ai/forum/t/what-is-kaggle-api-key-how-to-get-my-kaggle-key/17721)了解这个过程

在我们处理任何分析之前，我们需要检查我们的数据集中是否有任何垃圾数据，如果有，那么就需要清理掉

如上述交易所示，可交割量和可交割量包含空值。让我们设法获得更多的细节

我们在交易、交割量和交割百分比中有缺失值。我选择删除这些列，因为它们可能不会为我们的分析增加重要的值

# №3:解释性数据分析

[*转到目录*](#546d)

## 3.1 绘制 VWAP 随时间的变化

使用 plotly 构建交互式视觉效果

观察

*   股价稳步上升，直至 2011 年 7 月。
*   该股大幅下跌，并在 2020 年 3 月前再次上涨。
*   该股将在 2020 年再次大幅下跌。这可能是因为 covid，让我们检查一下

很明显，股价在 Covid 时代感受到了压力，直到 11 月份才开始回升

## 3.2 绘制移动平均线

当我们分析股票的时候，看一下移动平均线来理解下跌和上涨是一个很好的方法。交易者在打电话买入或卖出股票时更喜欢下列移动平均线

*   50 天 MA
*   100 天 MA
*   200 天 MA

虽然数列与数据框连接，但数列在数据框中的位置不正确。我们需要根据计算移动平均值的天数来移动序列。

pro Technique——虽然通过 matplotlib 绘图更容易，但是如果你想要交互式的视觉效果，你可以使用 plotly

这种分析有助于我们进行以下观察-

*   在 Covid 时间内，股票围绕其 50 天移动平均线运行。
*   股价从其 200 和 100 天移动平均线大幅上涨，表明这是一只优质股票，没有处于超卖区。

## 3.3 了解数据分布

数据不是正态分布的，然而，这正是我们通常对时间序列的期望

## 3.4 最高价、最低价、开盘价和收盘价的单变量分析

**见解**

*   所有参数之间没有太大的偏差
*   有两次下降，一次在 2012 年，另一次在 2020 年。

历年份额量的单变量分析

**见解**

*   从 2000 年到 2018 年，市场份额处于低谷期
*   HDFC 多年来表现出强劲的增长势头，因此在 2012 年之后，数量有所增长。

# №4:以工程为特色

[转到 TOC](#546d)

*计算平均值和标准偏差*

该技术通常应用于时间序列，以消除噪声和展示潜在的因果信号。下面我考虑 3、7、30 天的移动平均值和标准差

# №5:静态转换

[*转到目录*](#546d)

在我们对时间序列数据建立最大似然模型之前。检查数列是否平稳是很重要的。

***什么是静止检查***

平稳序列具有均值、方差和自相关结构不随时间变化的特性。在图片下面，插图给你一个更好的想法

![](img/9d7192f23bac80aae472666c714d704c.png)

有两种方法可以检验时间序列是否平稳

*   通过目视检查——这可以根据上面显示的图像来完成
*   **迪基-富勒检验**
    对原假设和备择假设进行验证。在当前情况下，零假设是时间序列不稳定，而另一个假设是时间序列稳定。

假设是根据应用测试后计算的 p 值确定的

*   p 值小于 0.05，零假设成立，时间序列不是平稳的
*   p 值大于 0.05，零假设成立，时间序列是平稳的

让我们进行这个测试。为了方便起见，python 已经有了一个执行所有底层计算的库

## 5.1 分解时间序列

我们分解时间序列来理解它作为一个加法或乘法时间序列的性质。

**相加时间序列**一般在相加时间序列中，趋势和季节变化随时间相对恒定。加法级数表现出线性行为。

**乘法时间序列**一般来说，趋势和季节变化的幅度会随着时间的推移而增加或减少。乘法时间序列显示指数行为。

这是一个附加的时间序列，因为我们没有看到数量上的指数增长。

## 5.2 **实现换档()**

我们采用移位技术使时间序列平稳。这是一种技术，我们将数据移动一天，用移动后的值减去原始值，并将差异记录在单独的列中。因为我们用今天的值减去昨天的值，所以它会留下一个恒定值，从而使图保持不变。

# №6:ARIMA 和 ARIMA 车展

[*转到 TOC*](#546d)

在高水平上开发 ARIMA 模型时，执行以下步骤

*   使时间序列平稳
*   计算 p、q 和 d 值，其中 p、q 和 d 指的是
*   p —自回归
*   q —移动平均线
*   d-差异

**汽车 ARIMA** 汽车 ARIMA 简化了上述流程。它涵盖了时间序列，以稳定和自动计算的 p，q 和 d 值。

以橙色显示预测值

模型评估可以基于 RMSE 和梅误差进行

# №7:脸书先知

[*转到目录*](#546d)

Prophet 是由脸书开发的开源时间序列模型。2017 年初发布。

据观察，这两个错误在汽车 ARIMA 少于先知。因此，自动 ARIMA 更准确

# №8:助推树

[*转到目录*](#546d)

传统的提升树在 Kaggle 竞争中表现最佳，因为它们似乎将误差降至最低。让我们检查一下

ARIMA 汽车的表现似乎超过了其余两款车型。

# №9:摘要

[*转到目录*](#546d)

下面是我们在进行分析时执行的步骤的总结。

*   从 Kaggle 下载了数据集。
*   执行预处理，如检查空值、删除冗余列、导入所有需要的库。
*   执行解释性数据分析
    关键见解
    1。随着时间的推移，VWAP 稳步增长。自去年以来，该股一直在 50 天移动平均线附近
    3。该股在 2012 年和 2020 年大幅下跌。
*   执行特征工程，计算 3、7 和 30 天内的平均值和标准偏差。
*   对时间序列进行平稳性检查，运行 DK fuller 测试并实施 shift()技术将时间序列转换为平稳性。
*   使用自动 ARIMA、先知和增强树训练模型。
*   在评估 RMSE 分数的时候。ARIMA 汽车似乎表现最好。

# №10:未来的工作

[*转到目录*](#546d)

*   虽然自动 ARIMA 功能非常强大，但也可以实现 ARIMA 来查看输出中的差异。
*   你可以试着训练 XGBoost 的模型，看看它的表现是否比自动 ARIMA 更好。

# №11:参考

[*转到目录*](#546d)

*   [https://www . ka ggle . com/rohan Rao/a-modern-time-series-tutorial/notebook](https://jovian.ai/outlink?url=https%3A%2F%2Fwww.kaggle.com%2Frohanrao%2Fa-modern-time-series-tutorial%2Fnotebook)
*   [https://www . ka ggle . com/yash VI/time-series-analysis-and-forecasting-reliance](https://jovian.ai/outlink?url=https%3A%2F%2Fwww.kaggle.com%2Fyashvi%2Ftime-series-analysis-and-forecasting-reliance)
*   [https://www . ka ggle . com/vikassing 1996/bajaj-stock-price-pred-xgb-f b-prophet-Altair # XGBoost-Modeling-and-Forecasting](https://jovian.ai/outlink?url=https%3A%2F%2Fwww.kaggle.com%2Fvikassingh1996%2Fbajaj-stock-price-pred-xgb-fb-prophet-altair%23XGBoost-Modeling-and-Forecasting)
*   [https://www . ka ggle . com/benroshan/reliance-nifty 50-时间序列-分析#模型-建立-阶段-预测-&-预测](https://jovian.ai/outlink?url=https%3A%2F%2Fwww.kaggle.com%2Fbenroshan%2Freliance-nifty50-time-series-analysis%23Model-building-Phase--Forecasting-%26-Prediction)
*   笔记本链接—[https://jovian . ai/hargurjeet/nifty-50-time-series-forecasting-3](https://jovian.ai/hargurjeet/nifty-50-time-series-forecasting-3)

我真的希望你们能从这篇文章中学到一些东西。如果你喜欢你学到的东西，请随意鼓掌。如果有什么需要我帮忙的，请告诉我。请随时联系 LinkedIn

[](/data-driven-fiction/how-to-submit-5e0808dce313) [## 如何提交？

### 数据驱动的小说

medium.com](/data-driven-fiction/how-to-submit-5e0808dce313)