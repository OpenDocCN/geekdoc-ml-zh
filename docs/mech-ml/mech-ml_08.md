# 8 推土机特征工程

> 原文：[`mlbook.explained.ai/bulldozer-feateng.html`](https://mlbook.explained.ai/bulldozer-feateng.html)

[特伦斯·帕尔](http://parrt.cs.usfca.edu) 和 [杰里米·豪沃德](http://www.fast.ai/about/#jeremy)

版权 © 2018-2019 特伦斯·帕尔。保留所有权利。

*请勿在网络上复制或以任何方式重新分发。*

这本书由 markup+markdown+python+latex 源代码生成，使用 [Bookish](https://github.com/parrt/bookish)。

您可以通过访问此页面的注释版本来**评论或注释**此页。您会看到现有的注释部分以黄色突出显示。它们是**公开可见的**。或者，您可以直接向 特伦斯 发送评论、建议或修正。

目录

+   合成日期相关特征

+   ProductSize 是一个有序变量

+   独热编码 Hydraulics_Flow

+   独热编码 Enclosure

+   拆分 fiProductClassDesc

+   使用对数价格进行训练

+   特征工程对模型性能的影响

+   摘要

在上一章中，我们清理了推土机数据集并修复了缺失值。结果模型的 OOB ![图片](img/ec985123b9b52e80981e6500795e8d16.png) 分数不错，但我们可以通过转换一些现有列和合成其他列来提高这个分数。本章中的技术扩展并改进了我们在**第六章** *按类别说话*中进行的特征工程。要关注的特征来自上一章的特征重要性图，我们的工程过程将如下所示：

1.  将`saledate`特征分解为其组成部分

1.  将 `ProductSize` 转换为有序分类变量

1.  *独热编码* `Hydraulics_Flow` 和 `Enclosure`

1.  将 `fiProductClassDesc` 分解为其组成部分

1.  对目标变量 `SalePrice` 取对数

随着我们的进行，我们将检查模型 OOB ![图片](img/ec985123b9b52e80981e6500795e8d16.png) 分数的改变，并查看特征重要性图中出现的内容。最后，我们将绘制特征转换与模型分数的关系图，以可视化我们手工操作带来的改进。

1 如果您遇到错误“`read_feather() got an unexpected keyword argument 'nthreads'`”，请尝试：

`import feather`

`feather.read_dataframe("data/bulldozer-train.feather")`

让我们从加载我们在上一章中计算过的清理后的数据框开始。我们还将加载原始数据集，以便我们对`ProductSize`、`Hydraulics_Flow`、`fiProductClassDesc`和`Enclosure`应用不同的转换。1

```py
df_raw = pd.read_feather("data/bulldozer-train.feather")
df_raw = df_raw.iloc[-100_000:] # same 100,000 records as before
df = pd.read_feather("data/bulldozer-train-clean.feather")

```

2 不要忘记汇总了各个章节代码片段的[笔记本](https://mlbook.explained.ai/notebooks/)。

我们应该得到一个新的基线 OOB 得分，这次使用森林中有 150 棵树的模型，而不是之前使用的 50 棵。随着树木数量的增加，随机森林的准确性往往会提高，但仅限于某个程度。更重要的是，在评估模型改进时，当我们增加树木数量时，另一个有趣的现象发生了。从更大和更大的模型中得到的分数开始收敛到相同的 OOB 测试集分数，无论运行多少次。随着随机森林平均越来越多的单个树的预测，其综合预测的方差下降，因此整体 OOB 得分的方差也相应下降。这里是如何使用前几章中使用的`test()`函数来获得稳定基线的方法：2

```py
X, y = df.drop(['SalePrice','saledate'], axis=1), df['SalePrice']
rf, oob_clean = test(X, y, n_estimators=150)

```

使用 15,454,126 个树节点，中值树高为 43.0，得到 OOB R² 0.90318

你会注意到，由于森林中树木数量的增加，OOB 得分为 0.903 略高于上一章末尾得到的分数。现在让我们深入探讨特征工程。

**偏差与方差**

统计学家在评估模型复杂度对模型准确性的影响时，会使用“偏差”和“方差”这两个词。如果一个模型系统性地预测的值与已知的真实值存在差异，那么这个模型就被认为是“有偏差”的。差异越大，偏差就越高。高偏差模型不够复杂，无法捕捉特征与目标变量之间的关系——它们是“欠拟合”的。如果模型给出的结果波动很大，取决于测试集，那么模型具有高方差，并且是“过拟合”的。简单来说，这些术语可以这样理解：偏差是准确性，方差是一般性。

以这种方式使用“方差”是不太理想的，尤其是当“过拟合”可用且更明确时，因为方差在许多情况下都会被使用。例如，当讨论增加随机森林中树木数量对效果的影响时，方差这个术语也适用。构建随机森林是一个本质上随机的进程，因此，在完全相同的训练集上训练的多个随机森林模型将会有所不同。这意味着这些模型在完全相同的测试集或 OOB 集上的预测也将不同。将模型预测的变化描述为“方差”是恰当的。

假设我们构建了一个包含 10 棵树的模型，并得到了一个 OOB 分数。重复这个训练和评分过程多次，你会发现分数有很大的变化。现在，将树木数量增加到 100 并重复试验。你会注意到，模型 OOB 分数之间的变化显著低于仅用 10 棵树训练的模型。（当我们平均多个树的预测时，[中心极限定理](https://en.wikipedia.org/wiki/Central_limit_theorem)就会发挥作用。）

因此，有些人比较多个测试集时，将方差视为泛化，同时在同一测试集上使用方差表示预测波动减少，但在更大的森林中。我们建议您避免偏差和方差，而更明确地关注准确性和泛化或欠拟合和过拟合。

## 8.1 合成日期相关特征

数据集中的日期列通常是目标变量的预测指标，例如推土机数据集中的 `saledate`。销售日期和制造年份一起对销售价格有很强的预测性。作为一般规则，我们建议将日期列分解为其组成部分，包括：年、月、日、星期几（1..7）、一年中的天数（1..365），甚至像“季度末”和“月末”这样的东西。Pandas 提供了方便的函数，可以从单个 `datetime64` 实体中提取所有这些信息。提取组件后，将 `datetime64` 转换为自 1970 年以来的秒数（这是常用的 UNIX 时间测量）。以下是一个基本函数，说明了如何合成与日期相关的特征：

```py
def df_split_dates(df,colname):
    df["saleyear"] = df[colname].dt.year
    df["salemonth"] = df[colname].dt.month
    df["saleday"] = df[colname].dt.day
    df["saledayofweek"] = df[colname].dt.dayofweek
    df["saledayofyear"] = df[colname].dt.dayofyear
    df[colname] = df[colname].astype(np.int64) # convert to seconds since 1970

```

使用函数后，我们可以使用 Pandas 的 `filter()` 函数来检查新创建的列：

```py
df_split_dates(df, 'saledate')
df.filter(regex=('sale*')).head(2).T
```

|   | 0 | 1 |
| --- | --- | --- |
|  |
| --- |
| saledate | 1232668800000000000 | 1232668800000000000 |
| saleyear | 2009 | 2009 |
| salemonth | 1 | 1 |
| saleday | 23 | 23 |
| saledayofweek | 4 | 4 |
| saledayofyear | 23 | 23 |

由于我们不知道哪些组件（如果有的话）将是预测性的，因此最好添加您可以从日期中推导出的任何内容。例如，您可能希望添加一个列，表明某天是商业假日，甚至是否有大风暴。除了通常的年/月/日和其他数值组件之外，您合成的新的列将是特定于应用的。RF 模型不会因为额外的列而混淆，我们可以在特征工程完成后删除无用的特征。让我们检查日期拆分对模型准确性的影响：

```py
X, y = df.drop('SalePrice', axis=1), df['SalePrice']
rf, oob_dates = test(X, y, n_estimators=150)

```

使用 14,917,750 个树节点和 43.0 个中值树高，OOB R² 为 0.91315

我们从干净的基线分数 0.903 提高到 0.913，节点数量更少。

现在我们除了 `YearMade` 之外还有 `saleyear` 列，让我们创建一个 `age` 特征，明确说明出售的推土机的年龄。车辆的年龄显然很重要，尽管模型已经可以访问这两个字段，但让模型的生活尽可能简单是个好主意：

```py
df['age'] = df['saleyear'] - df['YearMade']

X, y = df.drop('SalePrice', axis=1), df['SalePrice']
rf, oob_age = test(X, y, n_estimators=150)

```

使用 14,896,494 个树节点和 43.0 个中值树高，OOB R² 为 0.91281

添加 `age` 后，OOB 分数大致相同，但节点数量略小。

观察特征重要性图，除了转换后的 `saledate` 外，我们添加的与日期相关的特征似乎都不重要。`YearMade` 仍然非常重要，但 `age` 似乎并不那么重要。

» *由代码生成左侧*

![bulldozer-feateng](img/bulldozer-feateng_eng_11.svg)

```py
I = importances(rf, X, y)
plot_importances(I.head(15))
```

这种原因微妙，但与所有日期相关特征高度相关的事实有关，这意味着如果我们删除其中一个，其他特征就会“填补”它的空缺。最好将这些日期相关特征视为特征重要性图的元特征：

» 由代码向左生成

![bulldozer-feateng](img/bulldozer-feateng_eng_12.svg)

```py
features = list(df.drop('SalePrice',axis=1).columns)
datefeatures = list(df.filter(regex=("sale*")).columns)
for f in datefeatures:
    features.remove(f)
features.remove('YearMade')
features.remove('age')
features += [['YearMade','age']+datefeatures]
I = importances(rf, X, y, features=features)
plot_importances(I.head(15))
```

虽然`age`可能在重要性图中不会单独突出，但年龄与销售价格的关系图证实了我们的信念，即老旧车辆的平均售价较低。这种相关性（年龄特征与目标价格之间的关系）意味着`age`至少有一定的可预测性。

» 由代码向左生成

![bulldozer-feateng](img/bulldozer-feateng_eng_13.svg)

```py
fig,ax = plt.subplots()
df_small = df.sample(n=5_000) # don't draw too many dots
ax.scatter(df_small['age'], df_small['SalePrice'],
           alpha=0.03, c=bookcolors['blue'])
ax.set_ylabel("SalePrice")
ax.set_xlabel("Age in years")
```

让我们暂时保留所有这些特征，继续进行下一个任务。

## 8.2 ProductSize 是一个有序变量

根据特征重要性图，`ProductSize`特征很重要，因此值得重新审视该特征，看看我们是否可以改进默认的标签编码。为了获得其重要性的证实证据，我们还可以通过使用 Pandas 的`groupby`来查看产品大小与销售价格之间的关系。通过按`ProductSize`对数据进行分组，然后调用`mean()`，我们可以得到产品大小跨度的平均`SalePrice`（和其他列）：

» 由代码向左生成

![bulldozer-feateng](img/bulldozer-feateng_eng_14.svg)

```py
temp = df_raw.fillna('nan') # original dataset
temp = temp.groupby('ProductSize').mean()
temp[['SalePrice']].sort_values('SalePrice').plot.barh()
```

3 当从一个数据框复制列到另一个数据框时，`df_raw`到`df`，使用赋值运算符，Pandas 会根据两个数据框的索引方式静默地执行奇怪的操作。最安全的方法是使用`.values`复制列的 NumPy 版本，就像我们在这里所做的那样。

（在这里，我们使用 Pandas 内置的 matplotlib 条形图快捷方式：`.plot.barh()`。）产品大小与销售价格之间存在明显的相关性，正如我们所预期的那样。由于`Large`比`Small`大，因此`ProductSize`特征是有序的，这意味着我们可以使用序数编码。这仅仅意味着我们根据大小为类别值分配数字。快速网络搜索还显示，`Mini`和`Compact`推土机大小相同，导致以下编码：3

```py
sizes = {None:0, 'Mini':1, 'Compact':1, 'Small':2, 'Medium':3,
         'Large / Medium':4, 'Large':5}
df['ProductSize'] = df_raw['ProductSize'].map(sizes).values
print(df['ProductSize'].unique())
```

[0 2 4 3 1 5]

通过使用序数编码而不是标签编码，我们在 OOB 得分上获得了一小点提升![bulldozer-feateng](img/bulldozer-feateng_eng_11.svg)：

```py
X, y = df.drop('SalePrice', axis=1), df['SalePrice']
rf, oob_ProductSize = test(X, y, n_estimators=150)

```

使用 14,873,450 个树节点和 45.0 的中位树高，OOB R² 为 0.91526

有两个其他重要特征，`Hydraulics_Flow`和`Enclosure`，我们可以比标签编码更结构化地编码。

## 8.3 One-hot encoding Hydraulics_Flow

当不确定时，我们使用标签编码来编码分类变量。然而，正如我们在上一节中看到的，如果我们注意到变量是有序的，我们就使用那种类型的编码。当类别级别数量较少时，比如说 10 个或更少，我们假设类别很重要，对变量进行 one-hot 编码。One-hot 编码产生人们所说的 *虚拟变量*，这是从分类变量派生出来的布尔变量，其中对于给定的记录，恰好有一个虚拟变量为真。每个分类级别都有一个新列。缺失的分类值在每个虚拟变量中产生 0。

**One-hot 编码**

理解 one-hot 编码背后的想法最简单的方法是通过一个简单的例子。想象我们有一个有三个级别（三个学院）的分类变量。我们从一个像这样的数据框开始：

```py
df_toy = pd.DataFrame()
df_toy['Dept'] = ['Math','CS','Physics',np.nan]
df_toy
```

|   | 学院 |
| --- | --- |
|  |
| --- |
| 0 | 数学 |
| 1 | CS |
| 2 | 物理 |
| 3 |  |

Pandas 可以为我们提供该列的虚拟变量，并将它们连接到现有的数据框中：

```py
onehot = pd.get_dummies(df_toy['Dept'])
df_toy = pd.concat([df_toy, onehot], axis=1)
df_toy
```

|   | 学院 | CS | 数学 | 物理 |
| --- | --- | --- | --- | --- |
|  |
| --- |
| 0 | 数学 | 0 | 1 | 0 |
| 1 | CS | 1 | 0 | 0 |
| 2 | 物理 | 0 | 0 | 1 |
| 3 |  | 0 | 0 | 0 |

现在，位置上的“热”值不再是一个数字，而是表示类别。注意缺失的值最终会变成没有“热”值。如果你想知道“热”指的是什么，“one-hot”是电气工程中的一个术语，指的是多个芯片输出，在任何给定时间最多只有一个输出有非零电压。

特征 `Hydraulics_Flow` 有“高流量”和“标准”级别，但绝大多数记录缺失了这个特征的值：

```py
print(df_raw.Hydraulics_Flow.value_counts(dropna=False))
```

NaN 86819 标准 12761 高流量 402 无或未指定 18 名称：Hydraulics_Flow，数据类型：int64

在 one-hot 编码之前，让我们规范化列，使字符串“无或未指定”与缺失值（`np.nan`）相同，然后获取虚拟变量：

```py
df['Hydraulics_Flow'] = df_raw['Hydraulics_Flow'].values
df['Hydraulics_Flow'] = df['Hydraulics_Flow'].replace('None or Unspecified', np.nan)
onehot = pd.get_dummies(df['Hydraulics_Flow'],
                        prefix='Hydraulics_Flow',
                        dtype=bool)

```

接下来，我们将列 `Hydraulics_Flow` 替换为虚拟变量，通过将它们连接到 `df` 数据框并删除未使用的列来实现：

```py
del df['Hydraulics_Flow']
df = pd.concat([df, onehot], axis=1)

```

检查 OOB ![../Images/ec985123b9b52e80981e6500795e8d16.png]，我们看到它与之前差不多，特征重要性图显示最预测性的类别是 `Standard`。（参见图中的特征 `Hydraulics_Flow_Standard`。）

```py
X, y = df.drop('SalePrice', axis=1), df['SalePrice']
rf, oob_Hydraulics_Flow = test(X, y, n_estimators=150)

```

使用 14,872,994 个树节点和 45.0 中值树高，OOB R² 0.91558

![](img/bulldozer-feateng_eng_21.svg)

## 8.4 One-hot 编码 封装

让我们用同样的 one-hot 程序来处理 `Enclosure`，因为它也是一个只有几个级别的分类变量：

```py
print(df_raw.Enclosure.value_counts(dropna=False))
```

OROPS 40904 EROPS w AC 34035 EROPS 24999 NaN 54 EROPS AC 6 NO ROPS 2 名称：Enclosure，数据类型：int64

首先，让我们规范化类别：

```py
df['Enclosure'] = df_raw['Enclosure'].values
df['Enclosure'] = df['Enclosure'].replace('EROPS w AC', 'EROPS AC')
df['Enclosure'] = df['Enclosure'].replace('None or Unspecified', np.nan)
df['Enclosure'] = df['Enclosure'].replace('NO ROPS', np.nan)

```

让我们也看看这个变量与销售价格之间的关系：

» *由代码生成左侧*

![](img/bulldozer-feateng_eng_24.svg)

```py
temp = df.groupby('Enclosure').mean()
temp[['SalePrice']].sort_values('SalePrice').plot.barh()
```

这很有趣，“EROPS AC”的平均价格是其他推土机的两倍。网络搜索显示 ROP 代表“翻滚保护结构”，因此 EROPS 是一个封闭的 ROPS（驾驶室）和 OROP 是一个开放的防护笼。AC 代表“空调”。模型表明，带有封闭驾驶室的推土机价格更高，而带有空调的推土机平均价格最高。不幸的是，OOB 分数略有下降，但根据重要性图，虚拟变量`Enclosure_EROPS AC`很重要。

```py
onehot = pd.get_dummies(df['Enclosure'],
                        prefix='Enclosure',
                        dtype=bool)
del df['Enclosure']
df = pd.concat([df, onehot], axis=1)
X, y = df.drop('SalePrice', axis=1), df['SalePrice']
rf, oob_Enclosure = test(X, y, n_estimators=150)

```

OOB R² 0.91356 使用 14,896,328 个树节点，中值树高为 44.0

» *由代码生成左侧*

![图片](img/d4ca31a8f33d2a20dc751f16aa945cbf.png)(images/bulldozer-feateng/bulldozer-feateng_eng_26.svg)

```py
I = importances(rf, X, y)
plot_importances(I.head(20))
```

还有其他重要的分类变量，例如`fiSecondaryDesc`，但它有 148 个级别，这将创建 148 个新列，这将使数据框中的总列数增加超过三倍。我们建议对这些分类变量进行标签编码。

## 8.5 拆分 fiProductClassDesc

特征`fiProductClassSpec`是字符串变量而不是分类变量。值是产品类别的描述，并且字符串的一些组件似乎与更高的价格相关：

```py
temp = df_raw.groupby('fiProductClassDesc').mean()
temp[['SalePrice']].sort_values('SalePrice').head(15).plot.barh()
```

![图片](img/983a2d4e06390a99351fcbed2edeac57.png)(images/bulldozer-feateng/bulldozer-feateng_eng_27.svg)

操作能力值较高的推土机似乎能卖出更高的价格。当对特征进行标签编码时（根据特征重要性图），模型显然从这一特征中获得了某种预测能力。但是，我们可以通过将描述分为四个部分来使信息更加明确：

![图片](img/5312c2bf456281c7f30846ad789baff0.png)

描述是一个分类变量，从有限类别集合中选择，例如“跳过转向装载机”。下限和上限组件是数值特征，单位是一个类别，例如“马力”或“操作容量（磅）”。我们可以将后三个组件称为“规格”。因为规格有时是“未识别的”，所以规格组件可能缺失。

为了将描述字符串拆分，让我们分两步进行。首先，从`df_raw`中复制原始的非标签编码字符串，并在连字符处拆分它：

```py
# careful when copying between dataframes; use .values
df_split = df_raw.fiProductClassDesc.str.split(' - ',expand=True).values
df['fiProductClassDesc'] = df_split[:,0] 
df['fiProductClassSpec'] = df_split[:,1] # temporary column
print(df['fiProductClassDesc'].unique())
```

['履带式拖拉机，推土机' '液压挖掘机，履带' '轮式装载机' '转向装载机' '后卸式装载机' '平地机']

这就留下了字符串的右侧作为规格字符串：

```py
print(df['fiProductClassSpec'].unique()[:5])
```

['20.0 至 75.0 马力' '12.0 至 14.0 公吨' '14.0 至 16.0 公吨' '33.0 至 40.0 公吨' '225.0 至 250.0 马力']

接下来，使用正则表达式拆分该字符串，以捕获右侧的两个数字和单位：

```py
pattern = r'([0-9.\+]*)(?: to ([0-9.\+]*)|\+) ([a-zA-Z ]*)'
df_split = df['fiProductClassSpec'].str.extract(pattern, expand=True).values
df['fiProductClassSpec_lower'] = pd.to_numeric(df_split[:,0])
df['fiProductClassSpec_upper'] = pd.to_numeric(df_split[:,1])
df['fiProductClassSpec_units'] = df_split[:,2]
del df['fiProductClassSpec'] # remove temporary column
df.filter(regex=('fiProductClassSpec*')).head(3)
```

|   | fiProductClassSpec_lower | fiProductClassSpec_upper | fiProductClassSpec_units |
| --- | --- | --- | --- |
| |
| --- |
| 0 | 20.0000 | 75.0000 | 马力 |
| 1 | 12.0000 | 14.0000 | 公吨 |
| 2 | 14.0000 | 16.0000 | 公吨 |

由于我们引入了可能存在缺失值的列和新分类变量，我们必须按照我们通常的程序准备数据集：

```py
fix_missing_num(df, 'fiProductClassSpec_lower')
fix_missing_num(df, 'fiProductClassSpec_upper')
# label encode fiProductClassDesc fiProductClassSpec_units
df_string_to_cat(df)
df_cat_to_catcode(df)

```

我们看到模型性能因这个特征的转换而略有提升，一个特征重要性图显示我们合成的各个组成部分都很重要。

```py
X, y = df.drop('SalePrice', axis=1), df['SalePrice']
rf, oob_fiProductClassDesc = test(X, y, n_estimators=150)

```

使用 14,873,080 个树节点和 43.0 的中位树高度，OOB R² 为 0.91429

## 8.6 使用 log(price)进行训练

原始的 Kaggle 竞赛是根据价格的对数来衡量模型性能的，因此我们也应该这样做，因为我们将要在下一章中将我们的模型性能与竞赛领先者进行比较。此外，正如我们在**第 5.5 节**“登录，指数出”中讨论的，在处理价格时，通常将目标变量的对数取出来会有所帮助。（我们通常更关心两个价格相差 20%，而不是固定的$20。）转换目标变量只是调用`log()`函数的一个简单问题：

```py
X, y = df.drop('SalePrice', axis=1), df['SalePrice']
y = np.log(y)
rf, oob_log = test(X, y, n_estimators=150)

```

使用 14,865,740 个树节点和 44.0 的中位树高度，OOB R² 为 0.91881

那个 0.919 的评分是一个很好的准确率提升，全部来自于一个简单的数学转换。

## 8.7 特征工程对模型性能的影响

![](img/bulldozer-feateng_eng_35.svg)

我们在本章中做了很多工作来改进提供给 RF 模型的特征，所以让我们比较这些变化对模型性能的影响。以下图放大了从基线到最终日志特征改进的 OOB 评分范围。（此图的代码位于本章的[notebook](https://mlbook.explained.ai/notebooks/bulldozer-feateng/eng.ipynb)中。）

![](img/bulldozer-feateng_eng_36.svg)

总体而言，模型的 OOB 评分从 0.903 提升到 0.919，提高了 16.141%；`(oob_log-oob_clean)*100/(1-oob_clean)`）。

大部分情况下，我们的特征工程努力都得到了回报。然而，`Hydraulics_Flow`和`Enclosure`的一热编码似乎并没有提高模型性能。实际上，`Enclosure`的一热编码似乎损害了性能。但记住，我们是在测量准确率，并使用训练集查看特征重要性图，而不是验证集。结果证明，一热编码`Enclosure`确实在验证集和其他测试集上提高了相关指标。（在撰写下一章时，我们比较了带有和不带一热编码`Enclosure`列的模型的分数。）目前，最好让模型能够访问所有特征。

## 8.8 摘要

让我们总结一下本章学到的技术。

**日期**

作为一般规则，将日期列拆分为日、月、年、星期几、年中日以及与您的应用程序相关的任何其他元素，例如“季度末”或“是否假日”。基于日期合成新列的示例如下：

```py
df["salemonth"] = df[colname].dt.month
```

然后将原始日期列转换为自 1970 年以来的秒数的整数，使用以下方法：

```py
df[colname] = df[colname].astype(np.int64) # convert date to seconds
```

**顺序编码**

对于具有有序元素的分类变量，应使用反映类别级别之间关系的整数进行顺序编码。例如，如果某列具有低、中、高等级，则以下编码方式可行，其中缺失值变为 0：

```py
m = {np.nan:0, 'low':1, 'medium':2, 'high':3}
df[colname] = df_raw[colname].map(m)
```

**独热编码**

对于大约有 10 个或更少级别的非有序（名义）分类变量，可以采用独热编码。以下是替换列以多个虚拟列替换的独热编码列的基本步骤：

```py
onehot = pd.get_dummies(df[colname])
df = pd.concat([df, onehot], axis=1)
del df[colname]
```

**分割字符串编码**

在本章中，我们使用`split()`根据连字符字符拆分字符串：

```py
# get two columns
df_descr_spec_split = df[colname].str.split(' - ',expand=True)
```

然后使用正则表达式通过`extract()`从右侧字符串中提取三个组件：

```py
# get three columns
pattern = r'([0-9.\+]*)(?: to ([0-9.\+]*)|\+) ([a-zA-Z ]*)'
df_split = df['fiProductClassSpec'].str.extract(pattern, expand=True)
```

我们从这种方式提取的列中在`df`中创建了新的列。

在其他数据集中，您可能想要拆分许多其他类型的字符串，例如 URL。您可以创建新的列来指示`https`与`http`、顶级域名、域名、文件名、文件扩展名等。
