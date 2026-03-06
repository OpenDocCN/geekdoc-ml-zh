# 地震数据可视化

> 原文：<https://medium.com/mlearning-ai/earthquakes-data-visualization-5da0d2abeea1?source=collection_archive---------0----------------------->

用 Python 和 Plotly 制作动画

![](img/9fd37e90965d020026363b56fb80657b.png)

Photo by [Cédric Dhaenens](https://unsplash.com/@cedric?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

地震是地壳中储存的能量突然释放产生地震波的结果。根据美国地质勘探局的说法，地震是当地球的两个板块突然滑过彼此时发生的。两个块体滑动的表面称为断层或断层面。美国地质勘探局跟踪危险数据，并每分钟更新一次。数据以`CSV`格式存储。

在这个博客中，我们将学习如何在世界或特定区域的基础上可视化地震数据。我们还将学习基于`time`因素的可视化过程动画。

# 数据源

美国地质调查局是数据的主要来源。它是众所周知的各种灾害管理系统，包括地震，洪水，火山爆发等。地震数据分为四类。

*   [每小时](https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_hour.csv) →每分钟更新一次
*   [每日](https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_day.csv) →每分钟更新
*   [每周](https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_week.csv) →每分钟更新一次
*   [每月](https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_month.csv) →每分钟更新一次

> 对于该项目，我们将只考虑每日、每周和每月的数据。

来源— [USGS 电子表格格式](https://earthquake.usgs.gov/earthquakes/feed/v1.0/csv.php)。在这个链接中，您可以找到更多的详细信息，如列名和列描述。

使用这些实时流数据，我们可以开发一个实时更新的[仪表盘](http://earthquake-tracking-system.herokuapp.com/)(也许我会在下一次讲述)。

# 履行

如果你想得到视频解释，你可以看下面的视频和一些额外的细节。

## `import`套餐

像往常一样，我们从导入对这个项目至关重要的包开始。

## 数据操作

在数据中，几乎有`22`列，其中大部分是`NaN`。只有地震研究者才知道它的重要性。为了可视化，我们将只考虑`5`列。

1.  **时间** →用于显示可视化过程的动画。

*   ***月度*** →如果提取月度数据，我们将从`time`列提取日期。
*   ***每周*** →如果提取每周数据，我们将从`time`列提取工作日。
*   ***每日*** →如果提取每日数据，我们将从`time`列提取小时(数字)。

2.**纬度** →用于在地图上绘制地震位置。

3.**经度** →用于在地图上绘制地震位置。

4. **mag** →用于根据地震的震级表示标记的大小。

5.**放置** →有助于可视化全球或特定区域的数据。

*   `place`栏由`sub area`和`main area`组成。必须对其进行分割，以便可视化与特定区域(主要区域)相关的数据。

## 数据获取和预处理

下面是执行上述操作后获取预处理数据的函数。默认情况下，它获取全球范围内的数据。

该函数采用`3`参数，例如-

*   **方式** →参数值应为`daily`、`weekly`和`monthly`中的一个。默认情况下，该值为`daily`。
*   **bbox** →实际上是一个边界框参数，根据传递的值过滤数据。默认情况下，该值为`Worldwide`。
*   **mag_thresh** →该值是一个幅度阈值，采用整数值或浮点值。基于此，对数据进行过滤。默认为`2`。

## 数据可视化

一旦获取了数据，我们将可视化观察数据模式。数据应绘制在地图上(**雄蕊-地形**)。我们将使用 Plotly 来简化可视化过程。

因此有了下面的函数。

# 全世界的

让我们看看`Worldwide`数据的动画。

## 每天

动画是根据**小时**(数字)创建的。随意放大和缩小地图。

## 每周一次

动画是在**工作日**的基础上创作的。随意放大和缩小地图。

## 每月一次

动画是根据**日期**制作的。随意放大和缩小地图。

# 特定区域

让我们看看基于`Area`的数据的动画。

## 每天

动画是根据**小时**创建的(仅与传递的`bbox`值相关)。随意放大和缩小地图。

## 每周一次

动画是根据**工作日**创建的(仅与传递的`bbox`值相关)。随意放大和缩小地图。

## 每月一次

动画是基于**日期**创建的(仅与传递的`bbox`值相关)。随意放大和缩小地图。

好了，这篇文章就到这里。你可以在下面的链接中找到我的作品。

*   [笔记本](https://jovian.ai/msameeruddin/earthquakes-animations)
*   [卡格尔](https://www.kaggle.com/mohammedsameeruddin/earthquake-animations)

你也可以订阅我的时事通讯来获取这些独家内容。谢谢大家。

**结束**