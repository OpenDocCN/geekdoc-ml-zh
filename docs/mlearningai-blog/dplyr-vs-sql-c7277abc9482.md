# Dplyr 与 SQL

> 原文：<https://medium.com/mlearning-ai/dplyr-vs-sql-c7277abc9482?source=collection_archive---------4----------------------->

## 使用 SQL 和 Tidyverse 的 Dplyr 包进行数据操作

plyr 是 tidyverse 包的一部分，被称为数据操作语法。结构化查询语言(SQL)是一种专用语言，用于与存储在数据库中的数据进行交互。在本文中，我们将比较 dplyr 和 SQL 的查询结构的异同。SQL 和 dplyr 都是行业标准，在工业界和学术界都同样使用。SQL 中的`SELECT`是一个用于选择列子集的子句，dplyr 中的`select(dataset, col01, col02, ...)`动词用于相同的任务，类似地，`WHERE`子句和`filter(dataset, col01 > val1, ...)`动词分别用于过滤 SQL 和 dplyr 中的数据集。

![](img/01ca99a4d988882dbd6947b06819b8bc.png)

Photo by [Dan Cristian Pădureț](https://unsplash.com/@dancristianpaduret?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 资料组

我们将使用 [mtcars 数据集](https://www.rdocumentation.org/packages/datasets/versions/3.6.2/topics/mtcars)。下面给出了`CSV and batabase`文件。对于数据库文件，我使用 SQLite。您可以在 SQLite 的 [DB 浏览器中打开`.db`文件。](https://sqlitebrowser.org/)

*   [mtcars.csv](https://raw.githubusercontent.com/aimlresearcher/datasets/main/mtcars.csv)
*   [mtcars.db](https://github.com/aimlresearcher/datasets/blob/main/mtcars.db)

# 选择

我们想从 mtcars 数据集中选择英里/(美国)加仑(`mpg`)、气缸数(`cyl`)和总马力(`hp`)。

*   **dplyr** 具有用于返回所需列的`select`动词
*   **SQL** 有`SELECT`子句来选择所需的列

## DPLYR

## 结构化查询语言

# 限制性的

如果您想要返回数据集的前 n 行，您需要使用下面的 dplyr 和 SQL 命令。

*   **dplyr** 有`head(n = k)`功能返回顶部`k`行
*   **SQL** 有`LIMIT k`子句返回顶部`k`行

## DPLYR

## 结构化查询语言

# 包括…在内

要计算数据集中的总行数，您需要应用以下 dplyr 和 SQL 命令

*   dplyr 有`summary(count = n())`函数统计数据集中的所有记录。
*   SQL 有`COUNT(*)`函数返回数据表中的记录总数

## DPLYR

## 结构化查询语言

# 安排

对数据集进行排序是数据科学中一项非常重要的任务。SQL 和 dplyr 有以下函数对数据集进行排序

*   **dplyr** 有`arrange(col1, desc(col2), ...)`动词，用于按`col1`和`col2`等列对数据集进行排序。数据集将按列`col1`升序排序，按列`col2`降序排序。
*   **SQL** 有`ORDER BY`子句对数据集进行排序
*   我们已经给出了三个对`mtcars`数据集进行排序的例子
*   在第一个例子中，我们根据`mpg`列的升序对`mtcars`数据集进行排序。
*   在第二个例子中，我们根据`mpg`列的降序对`mtcars`数据集进行排序。
*   在第三个示例中，我们根据`mpg`列的升序和`model`列的降序对`mtcars`数据集进行排序。

## DPLYR

## 结构化查询语言

# 派生列

例如，您想要将每加仑英里数`mpg,`和重量(1000 磅)`wt` 转换为每加仑千克数`kpl`和`wt_kg`

*   dplyr 具有用于创建新列的`mutate(new_col = old_col*some_val, ...)`动词
*   SQL 在下面给出的`SELECT`子句中创建驱动程序列。

## DPLYR

## 结构化查询语言

# 过滤

您可以按数据集中任何列的升序或降序对数据集进行排序。下面给出了 SQL 和 dplyr 函数

*   dplyr 有`filter(gear == 4)`动词来过滤数据集
*   SQL 有`WHERE`子句来过滤数据库中的记录
*   `gear`的数据类型是`factor` ，我们需要将其转换为`numeric`，因为`less than`运算符不对 factor 数据类型进行操作。
*   我们展示了用 dplyr 和 SQL 过滤数据的三个例子
*   首先，我们用条件`gear == 4`过滤 mtcars 数据集
*   其次，我们使用条件`gear == 4 and cyl <= 6.`过滤 mtcars 数据集
*   其次，我们使用条件`gear == 4 or cyl <= 6.`过滤 mtcars 数据集

## DPLYR

## 结构化查询语言

# 分组

GROUP BY 语句将具有相同值的数据集的行分组到汇总行中，如**“查找每个类别中的齿轮数”**。

GROUP BY 语句通常与聚合函数(COUNT()、MAX()、MIN()、SUM()、AVG())一起使用，按一个或多个列对结果集进行分组。

下面给出了对数据进行分组的 dplyr 和 SQL 代码

## DPLYR

## 结构化查询语言

# 8.平均的

*   dplyr 有`summarise(mean_wt = mean(wt), ...)`动词来计算平均值
*   SQL 有`AVG()`来计算平均值

## DPLYR

## 结构化查询语言

# 差异

*   dplyr 有`summarise(var_wt = var(wt), ...)`动词来计算方差
*   SQL 有`VAR(wt)`来计算`wt` 列的方差

## DPLYR

## 结构化查询语言

# 11.最小值、最大值、总和、范围

*   dplyr 有`summarise(MIN = min(as.numeric(gear)), ...)`动词计算不同的汇总函数。
*   SQL 有`MIN(wt), MAX(wt), SUM(wt), RANGE`来计算`wt` 列的平均值

## DPLYR

## 结构化查询语言

# 结论

在本文中，我们学习了 dplyr 和 SQL 查询的结构。我们学习了如何使用 SQL 和 dplyr 对数据集进行过滤、限制、排序、选择和分组。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)