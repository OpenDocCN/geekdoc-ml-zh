# 如何使用 python 处理缺失值？

> 原文：<https://medium.com/mlearning-ai/how-to-handle-missing-values-using-python-8965e80ecc42?source=collection_archive---------4----------------------->

![](img/d74d17c5a2b26dc1fb964a907b116492.png)

Photo by [Andrea De Santis](https://unsplash.com/@santesson89?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

本文将实现对**缺失值的处理。**

**数据清洗**是**数据科学生命周期**中最重要的部分。如果做得不好，预测将是糟糕的，我们将不得不重新开始整个循环，导致金钱和时间上的损失。本文将讨论缺失值处理和代码实现。使用的数据集是 [**旧金山建筑许可数据**](https://www.kaggle.com/code/aparnashastry/building-permit-data-analysis/data) **。**

```
# modules we'll use
import pandas as pd
import numpy as np

# read in all our data
sf_permits = pd.read_csv("Building_Permits.csv")

# set seed for reproducibility
np.random.seed(0) 

# top 5 rows in the dataframe
sf_permits.head()
```

检查数据框中的缺失值(哪些行，哪些列)。数据的前五行显示有几列缺少值。这些栏是**“街道编号后缀”、“拟建建筑类型”和“现场许可证”**等栏。

## **漏分百分比**

```
 # get the number of missing data points per column
missing_values_count = sf_permits.isnull().sum()

# how many total missing values do we have?
total_cells = np.product(sf_permits.shape)
total_missing = missing_values_count.sum()

# percent of data that is missing
percent_missing = (total_missing/total_cells) * 100
print(percent_missing)
```

**SF _ permissions . is null()。sum()** 用于查找特定列中所有缺失值的数量。 **total_cells** 给出总行数， **total_missing** 给出缺失值的总行数。 **percent_missing** 是缺失值行的百分比。

现在，我们还可以首先尝试探索数据丢失的原因，例如，也许信息应该**不存在**，或者也许数据一开始就被**错误记录**。在数据集的情况下，如果“**街道编号后缀**”列中的一个值丢失，很可能是因为它**不存在**。如果“**邮政编码**”列中的值缺失，则不会被记录。

## **删除缺失值(行)**

**删除包含缺失值的行**。

```
sf_permits.dropna()
```

**删除缺失值(列)**

```
 sf_permits_with_na_dropped = sf_permits.dropna(axis=1)

# calculate number of dropped columns
cols_in_original_dataset = sf_permits.shape[1]
cols_in_na_dropped = sf_permits_with_na_dropped.shape[1]
dropped_columns = cols_in_original_dataset - cols_in_na_dropped
```

这里的想法是删除缺少值的列。**SF _ permissions . dropna(axis = 1)**是删除有缺失值的列。接下来，找到数据帧中的**总列数**和**丢弃列数**。从原始列数中减去删除的列数。

现在让我们用值自动填充缺失的单元格。

```
sf_permits_with_na_imputed = sf_permits.fillna(method='bfill', axis=0).fillna(0)
```

在上面的代码片段中，缺失的单元格( **NaN 值**)被替换为 0。也可以根据问题用均值或中值来代替。**均值或中位数**主要用于**数值**值( **int，float** )。**模式**用于**分类**值。完全取决于问题本身，所属领域，数据来源等。我这里提到的 [**就是**](https://www.kaggle.com/code/abhi2652254/exercise-handling-missing-values/edit) **。**

作为文章的结尾，我想分享一个来自 [**伊努龙**](https://ineuron.ai/) 的课程，请过目。如果你从我的链接中购买这门课程，我将从中赚取一份。

课程设置的**优势**是:

现成可用的端到端数据科学项目，用于**真实业务**用例、**行业级项目**、**案例研究**、**终身**访问、简历构建、**职业指导**、**顶点项目**，以及证书等等。

除了我的会员链接提供的 **10%折扣**之外，还有 **50%年终折扣**。这门课程实际上将帮助你**创建任何基于人工智能的端到端**应用程序，并**为未来将其货币化**。

***披露:*** *本帖部分外部链接为附属链接。*

附属链接:

[https://ineuron . ai/course/Data-Science-Industry-Ready-Projects？campaign = affiliate&coupon _ code = DVFMUEHP](https://ineuron.ai/course/Data-Science-Industry-Ready-Projects?campaign=affiliate&coupon_code=DVFMUEHP)

请查阅我的其他 [**文章**](/@abhi2652254) ，说 [**喜**](https://www.linkedin.com/in/obhinaba17/) **。**

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)