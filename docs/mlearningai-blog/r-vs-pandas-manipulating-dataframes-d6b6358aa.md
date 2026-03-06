# r 与熊猫:操纵数据帧

> 原文：<https://medium.com/mlearning-ai/r-vs-pandas-manipulating-dataframes-d6b6358aa?source=collection_archive---------0----------------------->

## 在 R 和 Python 中理解、切片、过滤和操作数据帧

R 和 python pandas 是数据分析和操作任务的行业标准。两种语言提供了几乎相同的功能，那么问题来了，为什么我们必须学习两种语言？数据科学家的角色不仅仅是写代码，还要理解和维护他人的代码。如果你喜欢 R，而你必须维护的代码是熊猫的，反之亦然，那么就会有问题。这篇文章是写给那些了解 R 并想了解熊猫的人的，反之亦然。我已经用两种语言的例子解释了每一点。读完这篇文章后，你会对 R 和熊猫感到非常舒服。我们将涵盖以下主题

*   创造和理解 R 和熊猫中的`dataframes`
*   用 R 和熊猫切`dataframes`
*   用 R 和熊猫过滤`dataframes`

![](img/e4a8add1d8c070dfb479de3863fd1528.png)

Photo by [Maxime Gilbert](https://unsplash.com/@mxm_gilbert?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 资料组

我们将使用托管在 [Kaggle](https://www.kaggle.com/) 上的[数据科学工作薪水](https://www.kaggle.com/datasets/ruchi798/data-science-job-salaries?select=ds_salaries.csv)数据集。数据集有 11 列，下表给出了每列的详细信息。

# 创建数据框架

Dataframe 是一种二维数据结构，由行和列组成，就像 google 工作表或 MS Excel 工作表一样。该数据表也称为“平面”数据。如果您有 MS excel 背景，那么您将会惊讶于 R 和 Pandas 为 excel 用户的日常问题提供了如此强大而优雅的解决方案。

1.  创建并打印空数据框

2.显示数据框的尺寸

3.使用两列创建数据框并打印

4.显示数据框的尺寸

5.创建不同长度的第三列，并尝试使用这三列创建数据框。数据框创建函数将给出错误。

## 稀有

*   `data.frame()`用于创建一个数据框
*   `dim()`用于返回数据框的尺寸
*   `c()`用于创建矢量
*   是 R 编程语言中的赋值操作符

## 计算机编程语言

*   将`numpy` 和`pandas` 库分别导入为`np` 和`pd`
*   `pd.DataFrame()`用于创建一个数据帧，并将其分配给`py_frame`
*   `py_frame.shape`用于返回数据集的维度
*   `[]`用于在 python 中创建列表
*   `{‘key1’: value1, ‘key2’: value2, ...}`是以键值对形式存储数据的字典。
*   `pd.DataFrame(data)`要求`dictionary`创建一个`dataframe`

## 加载并检查数据集

1.  将 GitHub 存储库中的数据读入数据框
2.  从`dataframe` 中删除第一个虚拟列，将其转换为`tibble` 数据结构，并在`r_frame`中更新。
3.  使用`rename(dataset, ...)`功能重命名列
4.  打印数据集的前六行
5.  打印数据集的前三行
6.  打印数据集的最后六行
7.  打印数据集的最后三行
8.  打印数据集的维度
9.  打印数据集的结构
10.  打印`dataset`中的`summary` 。`Currency`、`title`、`type`和`level` 列没有给出足够的信息，因为它们属于`character` 数据类型。
11.  将`currency`、`title`、`type`和`level` 从`character` 数据类型转换为`factor` 数据类型。
12.  打印数据框的`summary`

## 稀有

*   `read.csv(link)`用于从作为参数给出的链接中读取 CSV 文件，并将数据帧分配给`r_frame`。
*   `r_frame[-1]`返回数据帧的所有列，除了第一列是虚拟变量`index` 。
*   `as_tibble()`是用于将`data.frame()`数据结构转换为`tibble()`的函数
*   `rename(dataset, ...)`是用于重命名`tibble`列的功能
*   `head(r_frame)`用于打印`r_frame`的前 6 行。
*   `head(r_frame, n = 3)`是用于打印`r_frame`的前 3 行的功能。
*   `tail(r_frame)`用于打印`r_frame`的底部 6 行。
*   `tail(r_frame, n = 3)`是用于打印`r_frame`底部 3 行的功能。
*   `dim(r_frame)`用于查找`r_frame`的尺寸。
*   `str(r_frame)`用于查找`r_frame`的结构
*   `mutate(dataset, ...)`用于改变不同列的数据类型
*   `summary(r_frame)`用于返回`r_frame` 数据的汇总。

## 计算机编程语言

*   `pd.read_csv(link)`用于从 GitHub 库中加载数据文件
*   `py_frame.drop(columns=[‘Unnamed: 0’])`用于删除虚拟指标变量
*   `py_frame.rename(columns=dictionary)`用于重命名数据集的列
*   `py_frame.head()`用于打印数据集的前 5 行
*   `py_frame.head(n = 3)`用于打印数据集的前 3 行
*   `py_frame.tail()`用于打印最后五行
*   `py_frame.tail(n = 3)`用于打印最后 3 行
*   `py_frame.info()`用于打印数据集的结构
*   `py_frame.describe(include=’all’)`用于打印数据集的基本摘要

## 向数据框架添加行和列

在这段代码中，我们将执行以下任务。

*   创建数据框架
*   更改数据集的列名
*   向数据框架添加一列
*   向数据框架添加一行

## 稀有

*   `data.frame()`用于创建一个数据帧`r_df`
*   更改数据集的列名`r_df`
*   `r_df <- cbind(r_df, new_col)`用于向`r_df`添加一列
*   `r_df <- rbind(r_df, new_row)`用于向`r_df`添加一行

## 计算机编程语言

*   使用`pd.DataFrame()`功能创建数据帧
*   使用`py_df.rename()`功能重命名列
*   使用`py_df[“new_col”] = new_col`命令添加一列
*   使用`py_df.loc[len(py_df)] = new_row`命令添加一行

# 切片数据帧

在这段代码中，我们将在 R 和 Pandas 中执行以下操作。

*   选择数据框的像元值
*   选择数据框的行或列
*   访问数据帧的子集
*   通过数据帧的位置或名称访问列

## 稀有

*   `r_df[row, col]`用于访问数据帧的单元格值
*   将`row` 或`col` 值留空表示全部返回
*   `r_df[2, 1]`用于返回第 2 行第 1 列的值
*   `r_df[3, 2]`用于返回第 3 行第 2 列的值
*   `r_df[2, ]`用于返回数据帧的第 2 行
*   `r_df[, 2]`用于返回数据帧的第 2 列
*   `r_df[2:3, ]`用于选择数据帧的第 2 行和第 3 行
*   `r_df[1:2, ]`用于选择数据帧的第 1 行和第 2 行
*   `r_df[c(1, 2), ]`用于选择数据帧的第一行和第二行
*   `r_df[c(1, 2, 4), 2:3]`用于选择数据帧第 1、2、4 行的第 2、3 列
*   `r_df$student_ages`用于选择数据帧的第 2 列
*   `r_df$student_ages[2]`或`r_df[, 2][2]`用于选择`student_ages` 列的第二个值
*   `r_df[, 1]`用于选择数据帧的第一列

## 计算机编程语言

*   `py_df.iloc[1, 0]`用于访问第 2 行第 1 列的单元格值
*   `py_df.iloc[2, 1]`用于访问第 3 行第 2 列的单元格值
*   `py_df.iloc[1, ]`用于访问数据帧的第 2 行
*   `py_df[[‘student_ages’]]`用于访问数据帧的第 2 列
*   `py_df.iloc[1:3, ]`用于选择数据帧的第 2 行和第 3 行
*   `py_df.iloc[0:2, ]`用于选择数据集的前两行
*   `py_df.iloc[[0, 1], ]`用于选择数据集的前两行
*   `py_df.iloc[[0, 1, 3], 1:3]`用于选择数据帧第 1、2、4 行的第 2、3 列

# 过滤数据帧

在本节中，我们将根据一些标准过滤数据帧。我们有六个关系运算符`>, <, ≤, ≥, ==, !=`分别用于比较值并返回输出一个布尔值`TRUE, FALSE.`

`&`和`|`是用于连接两个或多个逻辑表达式的逻辑运算符

## 稀有

*   `r_df[TRUE, ]`返回`r_df`的所有行
*   `r_df[FALSE, ]`返回虚无
*   如果`marks` 大于`90` ，则`r_df$marks > 90`返回`true` ，否则返回`false`
*   `r_df[r_df$marks > 90, ]`返回分数大于`90`的记录
*   `length(df)`返回数据帧中`columns`的编号
*   `nrow(df)`返回数据帧的`rows` 号
*   `ncol(df)`返回数据帧的`columns` 号
*   `&`是数据集的逻辑`and` 运算
*   `|`是数据集的逻辑`or`运算

## 计算机编程语言

*   创建熊猫数据框
*   选择分数大于 0 的学生
*   选择分数低于 0 的学生
*   如果分数大于 90，返回真，否则返回假
*   `py_df[py_df.marks > 90]`返回分数大于 90 的记录
*   `len(py_df[py_df.marks > 90])`返回分数大于 90 的记录数
*   `py_df.shape[0]`返回数据集中的行数
*   `py_df.shape[1]`返回数据集中的列数
*   `&`用于逻辑`and` 操作
*   `|`用于逻辑`or` 操作

# 结论

在本文中，我们比较了 R 编程语言和 python 熊猫库。我们在 R 和 Pandas 库中理解并实现了 dataframe。然后我们学习了如何在 R 和 Python 熊猫中切片和过滤数据帧。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)