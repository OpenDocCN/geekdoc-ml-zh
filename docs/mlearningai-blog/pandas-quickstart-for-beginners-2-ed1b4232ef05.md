# 熊猫初学者快速入门 2

> 原文：<https://medium.com/mlearning-ai/pandas-quickstart-for-beginners-2-ed1b4232ef05?source=collection_archive---------5----------------------->

![](img/ae5aaed8c32b5b8361b91112f43f2989.png)

Photo by [Maxwell Nelson](https://unsplash.com/@maxcodes?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

它是我上一篇文章“熊猫初学者快速入门 1”的延续。在这篇文章中，我将介绍一些对数据分析至关重要的工具。这些方法对于第一眼理解数据和快速钻研数据非常有用。

**聚合&分组**

聚合实际上是与数据相关联的特定值的表示，就像它的统计数据一样。其中一些是:

```
# - count()
# - first()
# - last()
# - mean()
# - median()
# - min()
# - max()
# - std()
# - var()
# - sum()
# - pivot table
```

这些函数在“groupby”命令之后使用。我们举个例子来看看。我们继续使用同一个数据集“泰坦尼克号”。我们将计算女性和男性的平均年龄。回想一下，我们使用方括号从数据集中选择一个变量。

```
import pandas as pd
import seaborn as sns
pd.set_option('display.max_columns', None)df = sns.load_dataset("titanic")
df.head()df["age"].mean() #mean of all groupdf.groupby("sex")["age"].mean() #mean of age variable according to sex variable
```

请注意，我们对分类变量应用了 groupby 操作。它根据行在变量中的类别对行进行分组。需要注意的是，如果我们的类较少，分组是有意义的。在接下来的文章中，我将在分析数据时检查这些点的重要性。

实际上，应用 groupby 的模式是:

```
#dataframe_name.groupby(variable1)[variable2].aggregation_function
```

这将在根据分类变量 1 的类别分组后，对变量 2 应用聚合函数。

我们可以计算给定数据的多个描述值。为此，人们可能更喜欢在“agg”命令中使用字典。作为关键字，我们选择数字变量，作为项目，我们给出一个由应用的统计方法组成的列表。一些例子:

```
df.groupby("sex").agg({"age": "mean"}) #calculates means of ages of female and male classesdf.groupby("sex").agg({"age": ["mean", "sum"]}) #calculates sums and means of ages of female and male classes#calculates sums and means of ages of female and male classes, means of survived classes
df.groupby("sex").agg({"age": ["mean", "sum"],
                       "survived": "mean"})
```

**透视表**

数据透视表函数接受一些参数，这些参数详细描述了您希望数据呈现的形状。输出是数据透视表的形式。

模式就像

```
# df.pivot_table(intersection values, index-row values, column values)
# it computes default means of intersection values.df.pivot_table("survived", "sex", "embarked")df.pivot_table("survived", "sex", ["embarked", "class"])
```

我们可以改变我们寻找的统计值:

```
df.pivot_table("survived", "sex", "embarked",aggfunc="std")
```

我们将把数字变量“年龄”转换成分类变量。

```
# converting num to cat
df["new_age"] = pd.cut(df["age"], [0, 10, 18, 25, 40, 90])df.pivot_table("survived", "sex", ["new_age", "class"])
```

不是:“pd.qcut”函数也可以用在这里。“cut”和“qcut”的区别在于，qcut 对变量的值进行排序，并将它们与第一、第二、第三四分位数分开。

**应用&λ**

Apply 将任何给定的函数应用于行或列，而不使用任何循环。

```
df[["age"]].apply(lambda x: x/10).head()df.loc[:, df.columns.str.contains("age")].apply(lambda x: x/10).head()df.loc[:, df.columns.str.contains("age")].apply(lambda x: (x - x.mean()) / x.std()).head()
```

没有 lambda 函数的最后一个命令的另一种方式:

```
def standart_scaler(col_name):
    return (col_name - col_name.mean()) / col_name.std()df.loc[:, df.columns.str.contains("age")].apply(standart_scaler).head()
```

**加入流程**

有两种方法:“concat”(比较常见)和“merge”。

使用 pd.concat 连接:

```
m = np.random.randint(1, 30, size=(5, 3))
df1 = pd.DataFrame(m, columns=["var1", "var2", "var3"])
df2 = df1 + 99pd.concat([df1, df2])pd.concat([df1, df2], ignore_index=True) #to fix indices
```

加入 pd.merge:

```
df1 = pd.DataFrame({'employees': ['john', 'dennis', 'mark', 'maria'],
                    'group': ['accounting', 'engineering', 'engineering', 'hr']})df2 = pd.DataFrame({'employees': ['mark', 'john', 'dennis', 'maria'],
                    'start_date': [2010, 2009, 2014, 2019]})pd.merge(df1, df2) #joins with respect to employee variable
pd.merge(df1, df2, on="employees")#joins with respect to employee variable
```

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)