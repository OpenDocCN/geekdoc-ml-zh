# 十分钟后的熊猫——第一部分

> 原文：<https://medium.com/mlearning-ai/pandas-cheat-sheet-part-1-6816991aac65?source=collection_archive---------4----------------------->

![](img/e5ff02f6fb796d7d11f6197ecee29f65.png)

image by [Ilona Froehlich](https://unsplash.com/@julilona) on unsplash

**Pandas** 是一款快速、强大、灵活且易于使用的开源数据分析和操作工具，构建于 [Python](https://www.python.org/) 编程语言之上。

Pandas 构建在 NumPy 和 Matplolib python 库之上。

**安装熊猫**

如果您的系统中安装了 Anaconda，那么您可以简单地从终端或命令提示符下安装:

```
conda install pandas
```

否则，如果您的系统中安装了 pip，那么您可以使用以下命令从终端或命令提示符安装它:

```
pip install pandas
```

**进口熊猫**

```
import pandas as pd
```

而不是写“熊猫”在熊猫体内使用这个方法，我们可以简单地写“pd”。因此，我们将其作为“pd”导入。

## **熊猫的数据结构**

***系列***

它是一种一维数据结构，非常类似于数组。

这是 python 中的一个列表

```
a = [3, 5, 2.71, -9.4, 8.432]
print(type(a))
a
```

![](img/8f9bab373488a9b64f40dae84e70aabc.png)

*创建系列*

1.

```
s = Series(a)
print(type(s))
s
```

![](img/ec6600659fff0c830d5b6bdd70186ebd.png)

2.在系列中，指数可以是任何值。

```
idx = ['a', 'd', 'f', 'h', 'i', 't']
idx
```

![](img/27bc4716762cee16d5eeb182bdadef55.png)

```
s1 = Series(a, idx)   #Series(data, index)
s1
```

![](img/d3d13b2815216b45f31a61f287339ecc.png)

3.使用字典

```
dic = {"a" : 6, "b": 7, "c": "Disha", "d" : 30}
dic
```

![](img/3b27a59af5ff02340118283ad47d0d09.png)

```
s2 = Series(dic)
s2
```

![](img/7d3ee907cf29b7fe698fcc056546019c.png)

访问系列的元素

```
s[3]
```

![](img/0a1debe54ce16b1e9c8f421dba7a66aa.png)

```
s2[“name”]
```

![](img/3439a7edae0d44561fc5b9d4024358c4.png)

级数的算术运算

```
s1 + s2
```

![](img/9d8b8f61a4134c63873291bb1dbb892b.png)

```
s2 + s2
```

![](img/e4769fc0ba44ffdabf3952e1d0fddda6.png)

```
s1 — s2
```

![](img/fac6e6fbec0511609321c46a8dcf9096.png)

当熊猫找不到匹配时，它输出 NaN。

```
s — s
```

![](img/5125a82d577521f068c2d726b0dbd2f8.png)

```
s * s
```

![](img/16f3e8a8032f0ba3f580a9345b229c32.png)

```
s / s
```

![](img/f1ca062ab7a89ce759083d74cbd0c6a1.png)

***数据帧***

数据框是一种二维数据结构，其中数据以表格形式排列。

熊猫中最常用的数据结构

**创建数据结构**

```
import numpy as np #another most important python library
df = pd.DataFrame(np.random.randn(5, 2), columns = list('AB'))
df
```

![](img/bce94f0a0048c095338f2d5e16f83656.png)

```
sample = {'name' : ['Riya', 'Sandy', 'Tonny', 'Alex'
          'age' : [14, 24, 30, 38],
    sample = {'name' : ['Riya', 'Sandy', 'Tonny', 'Alex'],
          'age' : [14, 24, 30, 38],
          'country' : ['India', 'New Zealand', 'Russia', 'Bangladesh']}
#creating a dataframe using dictionary
df1 = pd.DataFrame(sample)
df1
```

![](img/c34a0e9f47ff8d808f38bc8ac444c87e.png)

**从数据帧中选择列**

```
df1['name']
```

![](img/65ab6e8990616b3e41f4e2fe5fa38fda.png)

```
df1['name', 'age']
```

![](img/eebc63d8e9ba519791fd787351042b2f.png)

**添加列**

```
df1['year'] = [2006, 1996, 1990, 1982]
df1
```

![](img/8c8b09f4ab4c8458598743e497ddee59.png)

**移除或删除一列**

```
df1 = df1.drop(‘year’, axis = 1)
df1
```

![](img/02085d6e974f8297644d0708627bd48d.png)

要删除多个列，我们可以将多个列放在一个列表中，如下所示:

df1 = df1 . drop([列 1，列 2，轴= 1)

**移除或删除行**

```
df1 =df1.drop(3)
df1
```

![](img/67ef9ed39c02467900ebe6160289d259.png)

axis = 1 表示列，axis = 0 表示行。默认情况下，axis 值为“0”。

**访问元素**

1.  访问一列

```
df1['name']
```

![](img/4d1c9fcd831531353f78f1da044394b6.png)

2.访问多个列

```
df1[['name', 'age']]
```

![](img/846a58d2f454edb3084c9c28b00114b1.png)

3.基于特定条件访问数据帧的列

```
df1[df1['age'] > 18] # df1[condition]
```

![](img/78aa352d8d83ee9bac7e2248c6280404.png)[](https://varshithagudimalla.medium.com/pandas-in-10-minutes-part2-59782f7bfe20) [## 十分钟后的熊猫——第二部分

varshithagudimalla.medium.com](https://varshithagudimalla.medium.com/pandas-in-10-minutes-part2-59782f7bfe20)