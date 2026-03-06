# 在 R 中使用 Dplyr 进行数据操作

> 原文：<https://medium.com/mlearning-ai/data-manipulation-using-dplyr-in-r-9f930580f0e3?source=collection_archive---------1----------------------->

数据科学家将数据转化为信息，并将信息转化为洞察力。Dplyr 是数据科学家最重要的工具，这就是为什么它经常被称为数据操作的语法，因为它提供了一组一致而有效的动词，用于解决最常见的数据操作挑战，如排列、汇总、过滤、变异和选择数据。Dplyr 是 tidyverse 库的一部分，tidy verse 库是为数据科学家设计的 R 包的集合，它们共享共同的设计理念、语法和数据结构。在本文中，我们将涵盖以下主题。

*   使用`filter(dataset, ...)`函数过滤数据集
*   使用`select(dataset, ...)`功能选择列
*   使用`mutate(dataset, ... )`功能创建一个新列
*   使用`arrange(dataset, ...)`功能排列数据集
*   使用`summarise(dataset, ...)`函数汇总数据集
*   使用`rename(dataset, ...)`功能重命名列名
*   使用管道操作符将不同的数据操作函数链接到一个命令中。

![](img/1d98ef73eb28d54b44a921092f977de2.png)

Photo by [JESHOOTS.COM](https://unsplash.com/@jeshoots?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 安装和加载软件包

我们将使用以下命令安装并加载`tidyverse` 和`gapminder`包。

*   使用`head(gapminder, n = 5)`功能打印数据集的前五行。
*   使用`tail(gapminder, n = 5)`功能打印数据集的最后五行。
*   使用`summary(gapminder)`功能打印数据集摘要
*   使用`dim(gapminder)`功能打印数据集的尺寸

*   `gapminder` 数据集有`1704` 行和`6`列。
*   前三列是`country`、`continent`和`year`
*   第 4 列是`lifeEx` ，表示`life expectancy at birth`
*   第五列是`pop` ，意思是`total population`
*   第 6 列是`gdpPercap` ，意思是`per-capita GDP`

# 过滤器

`filter(dataframe, col_1 > value_1, col_2 == value_2, ...)`

*   第一个论点是`dataframe`。
*   下一个参数是一个条件或由逗号分隔的条件链。条件的格式是`col_1 > value_1`，其中`col_1` 是列的名称，然后是可以是六个关系符号(`<, >, ≤, ≥, ==, !=`)之一的`>`，最后一部分是列的值`value_1` 。
*   这些条件评估为布尔值(`TRUE, FALSE`)导致该行是否是结果集的一部分。
*   过滤`continent == “Asia”`的记录
*   过滤`continent == “Asia”`和`year == 1992`的记录
*   过滤`continent == “Asia”`和`year == 1992`以及`country == ‘Pakistan’`的记录
*   过滤`continent == “Asia”`和`year == 1992`以及`country == ‘Pakistan’ | country == 'China'`的记录
*   过滤`continent == “Asia”`和`year == 1992`以及`country == ‘Pakistan’ | country == 'China' | country == ‘India’ | country == ‘United States’`的记录

# 挑选

`select(dataframe, col_1, col_2, ...)`

`select` 是一个`dplyr`动词，它将第一个参数作为一个`dataset` 和您想要选择的一列或一系列列。

*   选择`gapminder` 数据集的`country`、`lifeExp` 和`gdpPercap` 列
*   使用范围操作符(`:`)选择从`country` 到`lifeExp`的列
*   使用`everything()`参数选择所有列
*   使用`last_col()`参数选择最后一列
*   使用`starts_with()`参数选择以前缀开头的列
*   使用`ends_with()`参数选择以后缀结尾的列

# 使突变

`mutate(dataset, new_col = col1 * some_value, ...)`

变化动词用于创建新的数据列或改变列的数据类型等。例如，我们有关于`pop` 人口和`gdpPercap`的数据，我们想要计算`gdp`。

*   创建新列`gdp`
*   将`lifeExp` 的数据类型改为整数

# 安排

`arrange(dataset, col_1, desc(col_2))`

它根据所选列的值对数据框的行进行排序。

*   按`pop` 列升序排列`gapminder` 数据集。
*   按照`country` 列降序排列`gapminder` 数据集。
*   按照`country` 列升序和`pop` 列降序排列`gapminder` 数据集。

# 概括

`arrange(dataset, mean(col_01), sd(col_2), ...)`

Summarise 函数用于计算数据集中的常见统计数据。例如

*   你要计算中国的平均值`lifeExp` 和标准差`lifeExp` 。
*   你要计算欧洲人口的`IQR, min, max`和`mean` (`pop`)。

# 重新命名

`rename(dataset, new_col = old_col1, ...)`

重命名谓词用于重命名数据集的列。例如，您有一个`gapminder` 数据集，您想将它的列(如`pop` 重命名为`population` ，将`lifeExp` 重命名为`life_exp`。

# 管道操作员

管道运算符用于链接 tidyverse 动词。例如，您想要对下面给出的`gapminder` 数据集执行三个操作。

*   你想过滤亚洲的数据集
*   仅选择国家/地区和 lifeExp 列
*   按照 lifeExp 升序排列数据集，按照 country 降序排列数据集。

管道操作符用于将这三个动词组合成一个命令。

# 结论

在本文中，我们介绍了 dplyr 包中非常重要的功能，比如过滤、选择、排列、总结、重命名、改变数据集，以及将不同操作链接到单个命令中的管道操作符。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)