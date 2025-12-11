# 7 探索和清理推土机数据集

> 原文：[`mlbook.explained.ai/bulldozer-intro.html`](https://mlbook.explained.ai/bulldozer-intro.html)

[特伦斯·帕特](http://parrt.cs.usfca.edu) 和 [杰里米·霍华德](http://www.fast.ai/about/#jeremy)

版权所有 © 2018-2019 特伦斯·帕特。保留所有权利。

*请勿在网络上复制或以任何方式重新分发。*

本书由 markup+markdown+python+latex 源代码生成，使用 [Bookish](https://github.com/parrt/bookish)。

您可以通过访问此页面的注释版本来对此页进行**评论或标注**。您会看到现有的标注部分以黄色突出显示。它们是**公开可见的**。或者，您可以直接将评论、建议或修复发送给特伦斯。

目录

+   加载推土机数据

+   初步查看数据

+   基线模型

+   清理数据

+   处理缺失数据

    +   替换缺失的分类值

    +   替换缺失的数值值

+   使用所有特征训练模型

+   总结

我们已经学到了很多关于准备数据、特征工程和训练模型的知识，但公寓租金数据集相对较小，特征较少。在接下来的几章中，我们将探索并构建来自 Kaggle 的[推土机拍卖价格数据集](https://www.kaggle.com/c/bluebook-for-bulldozers)，该数据集有 8 倍多的观测值和 52 个特征。大数据集带来了一系列新的问题，例如确定哪些特征需要关注进行特征工程，以及能够快速训练和测试模型。推土机数据集也存在大量缺失值。尽管如此，在完成我们为该数据集制定的过程后，您的最终模型将在（现已关闭的）推土机竞赛的排行榜上名列前茅。

这个数据集的另一个问题是，记录代表推土机的销售，价格可能会因通货膨胀、金融危机等因素随时间波动。一般来说，我们不能像处理非时间敏感数据那样训练和测试时间敏感数据。袋外（OOB）![图片](img/ec985123b9b52e80981e6500795e8d16.png)的分数通常不合适，因为 OOB 分数只衡量训练数据期间的性能，而不是对未来预测的准确性。按日期排序数据集并取最后，比如说，20%作为保留验证集更为合适。这样，剩下的 80%就作为训练集，我们用它来训练模型。在验证集上评估模型的性能比 OOB 分数更能准确估计模型的一般性。话虽如此，为了逐步解决这个推土机问题，我们将从使用 OOB![图片](img/ec985123b9b52e80981e6500795e8d16.png)分数来衡量模型性能开始，这极大地简化了我们的过程。但请记住，OOB 分数高估了模型性能。

在本章中，我们将检查推土机数据集，对各种值进行归一化和清理，填补缺失值，然后训练一个初始模型。接下来，在第八章**推土机特征工程**中，我们将学习更多关于分类变量的编码，并改进初始模型识别出的重要特征。在我们弄清楚如何准备这个数据集之后，我们将在第九章**训练、验证、测试**中探讨如何正确准备验证集和测试集，以便用于调整模型并获得模型一般性的真实估计。

## 7.1 加载推土机数据

我们的第一步是从 Kaggle 的[推土机蓝皮书](https://www.kaggle.com/c/bluebook-for-bulldozers/data)竞赛中获取推土机数据集。将解压后的`Train.zip`文件（以及解压）、`Valid.csv`和`ValidSolution.csv`下载到您启动 Jupyter 的目录下的`data`目录中。（您必须是注册的 Kaggle 用户并已登录。）解压`Train.zip`后得到的`Train.csv`文件大小为 116M，使用 Pandas 的`read_csv()`函数加载大约需要 35 秒。这种加载时间在盯着屏幕时会让人难以忍受，并且会使我们快速迭代模型变得更加困难。因此，我们将使用[feather 数据格式](https://github.com/wesm/feather)，它允许我们在大约一秒内加载数据。prep-bulldozer.py 脚本（来自本书的 data 目录）加载训练 CSV 数据（第一次也是唯一一次），分割出一个验证集，并使用快速 feather 格式保存它们。该脚本还将`Valid.csv`和`ValidSolution.csv`合并到一个单独的测试数据框中，并保存以供以后使用。在命令行和您的`data`目录中，执行以下命令以创建我们需要的文件。

```py
$ cd data
$ pip install feather-format
$ python prep-bulldozer.py 
Created bulldozer-train-all.feather
Created bulldozer-train.feather
Created bulldozer-valid.feather
Created bulldozer-test.feather
```

1 别忘了[笔记本](https://mlbook.explained.ai/notebooks/)，它们汇总了各章节中的代码片段。

在接下来的三个章节中，我们将使用`bulldozer-train.feather`作为我们的数据起点。为了开始编码过程，在`data`目录的上一级创建一个笔记本，并粘贴我们常用的前言：1

```py
import pandas as pd
import numpy as np
from sklearn.ensemble import RandomForestRegressor
from rfpimp import *  # feature importance plot

```

2 如果您遇到错误“`read_feather() got an unexpected keyword argument 'nthreads'`”，那么请尝试：

`import feather`

`feather.read_dataframe("data/bulldozer-train.feather")`

任何需要数据的新副本时，我们可以这样加载：2

```py
df_raw = pd.read_feather("data/bulldozer-train.feather")
df = df_raw.copy()

```

保留原始数据在`df_raw`中是个好主意，这样我们就可以撤销任何最终证明无用的数据转换。

## 7.2 初步查看数据

当首次检查数据集时，寻找以下关键摘要信息：列名、列数据类型、样本数据元素以及缺失数据量。这里有一个方便的函数可以嗅探数据框并返回一个包含摘要的数据框，其中每一行描述原始数据框中的一个列：

```py
def sniff(df):
    with pd.option_context("display.max_colwidth", 20):
        info = pd.DataFrame()
        info['sample'] = df.iloc[0]
        info['data type'] = df.dtypes
        info['percent missing'] = df.isnull().sum()*100/len(df)
        return info.sort_values('data type')

```

您可以从笔记本中调用`sniff(df)`以获取完整的摘要，但为了节省空间，这里只列出前 14 个条目：

```py
sniff(df).head(14)
```

|   | 样本 | 数据类型 | 缺失百分比 |
| --- | --- | --- | --- |
|  |
| --- |
| SalesID | 1646770 | int64 | 0.0000 |
| SalePrice | 9500 | int64 | 0.0000 |
| MachineID | 1126363 | int64 | 0.0000 |
| ModelID | 8434 | int64 | 0.0000 |
| datasource | 132 | int64 | 0.0000 |
| YearMade | 1974 | int64 | 0.0000 |
| auctioneerID | 18.0000 | float64 | 5.1747 |
| MachineHoursCurrentMeter |  | float64 | 64.7178 |
| saledate | 1989-01-17 00:00:00 | datetime64[ns] | 0.0000 |
| 连接器 |  | 对象 | 46.8269 |
| Tire_Size |  | 对象 | 76.3297 |
| Tip_Control |  | 对象 | 93.6982 |
| Hydraulics | 2 Valve | 对象 | 20.1663 |
| Ripper | None or Unspecified | 对象 | 73.9670 |

只从这次快速嗅探中，我们就能学到很多东西。数据有三种类型：数值、日期时间对象和字符串（`对象`）。有些列是完整的，但其他列有缺失数据，包括`Tip_Control`列，其缺失率高达 94%。一些值只是简单地缺失（在 Python 中表示为`None`对象或“不是一个数字”`np.nan`），但其他“缺失”值实际上是物理存在的字符串，如“`None or Unspecified`”。

例如`SalesID`和`ModelID`这样的列被表示为整数（`int64`），但它们实际上是名义分类变量。型号 8434 并不比型号 8433 大。还有一些列被表示为字符串，但包含数值，如`Hydraulics`（“`2 Valve`”）。其他列被表示为字符串，但实际上是纯数值，带有单位，如英尺或英寸。例如，`Tire_Size`列中的值应转换为英寸数：

```py
print(df['Tire_Size'].unique())
```

[None '14"' 'None or Unspecified' '20.5' '23.5' '26.5' '17.5' '29.5' '13"' '20.5"' '23.5"' '17.5"' '15.5' '15.5"' '7.0"' '23.1"' '10"' '10 inch']

在实验这个数据集时，查看其他列的唯一值集也是一个好主意。我们探索的下一步是训练一个模型。

## 7.3 基线模型

尽管我们只有 52 个总列中的少数几个是数值列，而且其中一些值是缺失的，但在我们的流程早期训练一个模型仍然是一个好主意。首先，它告诉我们训练模型需要多长时间。如果训练时间很重要，我们应该考虑使用数据子集。其次，它给出了数值特征与`SalePrice`目标变量之间关系的初步评估。这个初始模型的 OOB 分数是我们的下限，所以如果它相当不错，我们可以对我们的模型在特征工程后的性能持乐观态度。最后，从模型中得出的特征重要性图有助于我们将清理工作集中在最具预测性的列上。

让我们确定到目前为止表示为数字的特征：

```py
basefeatures = ['SalesID', 'MachineID', 'ModelID',
                'datasource', 'YearMade',
                # some missing values but use anyway:
                'auctioneerID', 'MachineHoursCurrentMeter']

```

我们选择创建包含 50 棵树的随机森林（RF），因为它给出了稳定且准确的分数，同时不需要太多的处理时间。![](img/ec985123b9b52e80981e6500795e8d16.png)

我们可以重用**第六章** *Categorically Speaking* 中的`test()`函数来训练 RF 并打印 OOB 分数：![](img/ec985123b9b52e80981e6500795e8d16.png)

```py
def test(X, y, n_estimators=50):
    rf = RandomForestRegressor(n_estimators=n_estimators, n_jobs=-1, oob_score=True)
    rf.fit(X, y)
    oob = rf.oob_score_
    n = rfnnodes(rf)
    h = np.median(rfmaxdepths(rf))
    print(f"OOB R² {oob:.5f} using {n:,d} tree nodes with {h} median tree height")
    return rf, oob

```

列`auctioneerID`和`MachineHoursCurrentMeter`有一些缺失值，用 Numpy 的`np.nan`表示，但我们可以通过调用`fillna(0)`函数将缺失值转换为零，作为一种权宜之计：

```py
X, y = df[basefeatures], df['SalePrice']
X = X.fillna(0) # flip missing numeric values to zeros
rf, oob_baseline_initial = test(X, y)

```

使用 22,500,374 个树节点和 56.0 的中等树高，OOB R² 为 0.78075

那个 OOB 分数并不糟糕，暗示了在这个数据集中存在一个强大的关系。

不幸的是，通过`fit()`函数训练模型需要大约 25 秒，在我们反复转换数据和重新训练模型的过程中，这似乎像永恒一样。为了减少训练时间，我们可以使用`df.sample(n=100_000)`对数据框进行子采样，如果不是时间敏感数据，我们会这样做。相反，让我们获取最后 100,000 条记录（按日期排序），利用事实，即最近的数据将更好地预测近未来的情况：

```py
df = df.iloc[-100_000:] # take only last 100,000 records

```

在减小数据框的大小后，重复步骤来训练模型：

```py
X, y = df[basefeatures], df['SalePrice']
X = X.fillna(0)
rf, oob_baseline = test(X, y)

```

使用 5,556,904 个树节点和 45.0 的中等树高，OOB R² 为 0.84512

现在训练模型只需要大约 5 秒，而之前需要 25 秒，并且作为额外的好处，OOB 的 0.845 比之前的 0.781 要好得多。

现在，让我们看看模型认为哪些特征最能预测推土机的销售价格：

```py
I = importances(rf, X, y)
plot_importances(I)
```

![](img/bulldozer-intro_sniff_14.svg)

最重要的特征与我们评估车辆价值时预期的相符：它是哪种（型号）推土机，制造时间，使用时间等。

现在我们已经对数据有了很好的理解，并且有了基线模型，让我们开始清理数据。

## 7.4 清理

在清理过程中最容易修复的是像更改列数据类型和删除不可用列这样的小行政细节，所以让我们从这里开始。根据 Kaggle 上的数据描述，`SalesID`是特定交易的唯一标识符。这显然不是预测性的，因为`SalesID`值永远不会再次作为特征出现，所以我们可以移除它。我们还可以移除`MachineID`，因为这个变量有[错误和不一致性](https://www.kaggle.com/c/bluebook-for-bulldozers/discussion/3694)；此外，根据我们的特征重要性图，它也不是强预测性的。我们的第一步是：

```py
del df['MachineID'] # dataset has inconsistencies
del df['SalesID']   # unique sales ID so not generalizer

```

`auctioneerID`列的值看起来像数字，但它们实际上是分类变量，具体来说是没有任何顺序的命名变量：

```py
print(df['auctioneerID'].unique())
```

[nan 5.2.27.1.23.3.4.20.7.8.12.10.6.21.13.9.18.99.16.14.19.28.15.22.25.17.11.24.26.0.]

为了使这一点更清晰，让我们将数据类型更改为字符串：

```py
df['auctioneerID'] = df['auctioneerID'].astype(str)

```

如果我们将其保留为数字，我们下面处理缺失数值的过程将用中值拍卖师 ID 替换缺失的`auctioneerID`值，这显然是没有意义的。

关于数字的问题就这么多，让我们来看看字符串值列。其中一些很整洁，例如：

```py
print(df['ProductGroup'].unique())
```

['TTT' 'TEX' 'WL' 'SSL' 'BL' 'MG']

但其他人有缺失值：

```py
print(df['Drive_System'].unique())
```

[None 'Two Wheel Drive' 'Four Wheel Drive' 'No' 'All Wheel Drive']

以及其他人用物理存在的字符串来表示“缺失：”

```py
print(df['Backhoe_Mounting'].unique())
```

['None or Unspecified' None 'Yes']

列`fiSecondaryDesc`和`fiModelSeries`甚至有“`#NAME?`”的值。这个数据集有多种表示“缺失”或“未指定”的方式，但我们的模型无法自己确定这些等价物。我们需要将这些字符串标准化，以便`None`、`None or Unspecified`和`#NAME?`都表示“缺失”。让我们将这些等价物封装在一个函数中，该函数将转换数据框，使得只有`np.nan`表示缺失：

```py
from pandas.api.types import is_string_dtype, is_object_dtype
def df_normalize_strings(df):
    for col in df.columns:
        if is_string_dtype(df[col]) or is_object_dtype(df[col]):
            df[col] = df[col].str.lower()
            df[col] = df[col].fillna(np.nan) # make None -> np.nan
            df[col] = df[col].replace('none or unspecified', np.nan)
            df[col] = df[col].replace('none', np.nan)
            df[col] = df[col].replace('#name?', np.nan)
            df[col] = df[col].replace('', np.nan)

```

在调用`df_normalize_strings(df)`之后，所有表示“无”的不同方式都被折叠为`np.nan`，并且所有字符串都转换为小写：

```py
df_normalize_strings(df)
print(df['Drive_System'].unique())
print(df['Backhoe_Mounting'].unique())

```

[nan 'two wheel drive' 'four wheel drive' 'no' 'all wheel drive'] [nan 'yes']

一些字符串实际上是数值，但包含单位名称或符号，这迫使数据框将它们视为字符串：

```py
print(df['Tire_Size'].unique())
print(df['Undercarriage_Pad_Width'].unique())

```

[nan '26.5' '20.5' '17.5' '23.5' '14"' '13"' '29.5' '17.5"' '15.5"' '20.5"' '15.5' '23.5"' '7.0"' '10"' '23.1"'] [nan '36 inch' '24 inch' '20 inch' '34 inch' '26 inch' '30 inch' '28 inch' '32 inch' '16 inch' '31 inch' '18 inch' '22 inch' '33 inch' '14 inch' '27 inch' '25 inch' '15 inch']

将这两个列转换为数值很简单，只需去除`"`和`inch`字符即可。这里有一个函数，通过从字符串的前面提取任何整数或浮点数来将字符串列转换为数值列：

```py
def extract_sizes(df, colname):
    df[colname] = df[colname].str.extract(r'([0-9.]*)', expand=True)
    df[colname] = df[colname].replace('', np.nan)
    df[colname] = pd.to_numeric(df[colname])

```

```py
extract_sizes(df, 'Tire_Size')
extract_sizes(df, 'Undercarriage_Pad_Width')
print(df['Tire_Size'].unique())
print(df['Undercarriage_Pad_Width'].unique())

```

[nan 26.5 20.5 17.5 23.5 14. 13. 29.5 15.5 7. 10. 23.1] [nan 36. 24. 20. 34. 26. 30. 28. 32. 16. 31. 18. 22. 33. 14. 27. 25. 15.]

还有另外两列在本质上都是数值，但解析起来会更复杂（因为它们既有英尺也有英寸单位）：

```py
print(df['Blade_Width'].unique())
print(df['Stick_Length'].unique())

```

[nan "12'" "14'" "13'" "16'" "<12'"] [nan '10\' 6"' '9\' 6"' '9\' 7"' '10\' 2"' '12\' 8"' '12\' 10"' '9\' 10"' '9\' 8"' '11\' 0"' '10\' 10"' '8\' 6"' '9\' 5"' '14\' 1"' '11\' 10"' '6\' 3"' '12\' 4"' '8\' 2"' '8\' 10"' '8\' 4"' '15\' 9"' '13\' 10"' '13\' 7"' '15\' 4"' '19\' 8"']

`Blade_Width`列甚至有一个以`<12'`形式表示的范围。最好将它们保留为字符串，在准备数据用于模型时，我们将它们视为分类变量。

## 7.5 处理缺失数据

CSV 文件中的缺失数据通常以物理缺失（如“，，”这样的连续两个逗号）表示，但一些记录使用物理存在的字符串值，如`None`或`Unspecified`。一些文件使用特殊的指示数字来表示缺失的数值，如-1 或 0。Pandas 使用 Numpy 的`np.nan`（“非数字”）来表示内存中数据文件中缺失的值，无论是数值还是字符串数据类型。Pandas 将内存中物理存在的数值和字符串值按原样存储在文件中。关键是缺失的定义是模糊的，取决于数据集。这就是为什么我们在上一节中标准化了字符串，以便只有`np.nan`表示“缺失”。在本节中，我们将对数值指示值做同样的处理。

一旦整个数据框对缺失值有一个统一的定义，我们仍然需要对这些空缺进行一些智能处理。模型无法在“非数字”值上训练。我们处理缺失值的方案如下：对于数值列，我们用该列的中位数替换缺失值，并引入一个新布尔列，当替换缺失值时该列值为真。（统计学家将替换缺失值称为*插补*。）对于非数值列的策略简单来说就是保持原样，使用`np.nan`值。我们的默认字符串/分类变量编码是将它们进行标签编码，这将自动将`np.nan`值替换为零。（标签编码为每个唯一的字符串或类别值分配一个唯一的整数。）处理缺失的非数值值是最简单的，让我们先看看它是如何工作的。

### 7.5.1 替换缺失的分类值

在**第 6.2 节** *编码分类变量*中，我们将字符串`display_address`列转换为数值，通过将列转换为有序分类列，然后替换类别为它们的类别整数代码+1。Pandas 用类别代码-1 表示`np.nan`，所以加一将`np.nan`移到 0，并将所有类别代码移到 1 以上。为了方便，让我们创建两个函数来实现我们的标签编码策略：

```py
from pandas.api.types import is_categorical_dtype
def df_string_to_cat(df):
    for col in df.columns:
        if is_string_dtype(df[col]):
            df[col] = df[col].astype('category').cat.as_ordered()

def df_cat_to_catcode(df):
    for col in df.columns:
        if is_categorical_dtype(df[col]):
            df[col] = df[col].cat.codes + 1

```

让我们在一个玩具数据集上看看这个机制是如何起作用的：

```py
df_toy = pd.DataFrame(data={'Name':['Xue',np.nan,'Tom']})
df_toy
```

|    | 名称 |
| --- | --- |
|   |
| --- |
| 0 | 学 |
| 1 |  |
| 2 | Tom |

将字符串列转换为分类变量意味着 Pandas 将每个字符串替换为唯一的整数表示，我们可以将其包含在数据框中：

```py
df_string_to_cat(df_toy)
df_toy['catcodes'] = df_toy['Name'].cat.codes
df_toy
```

|   | 名称 | catcodes |
| --- | --- | --- |
|   |
| --- |
| 0 | Xue | 1 |
| 1 |  | -1 |
| 2 | Tom | 0 |

Pandas 仍然将`名称`列的值显示为字符串，因为这更有意义，但`名称`列现在是分类的：

```py
print(df_toy.dtypes)
```

名称 类别 catcodes int8 数据类型：对象

要完成标签编码，我们调用第二个函数来将类别值替换为整数代码：

```py
df_cat_to_catcode(df_toy)
df_toy
```

|   | 名称 | catcodes |
| --- | --- | --- |
|   |
| --- |
| 0 | 2 | 1 |
| 1 | 0 | -1 |
| 2 | 1 | 0 |

`名称`列比`catcodes`列多一个，因此缺失值在编码过程的最后成为整数值 0。

要处理所有缺失的非数值和非数值列的标签编码，只需两次函数调用：

```py
df_string_to_cat(df)
df_cat_to_catcode(df)
df.head(2).T.head(10)
```

|   | 289125 | 289126 |
| --- | --- | --- |
|   |
| --- |
| 销售价格 | 8300 | 15500 |
| 模型 ID | 4663 | 11859 |
| datasource | 136 | 132 |
| 拍卖师 ID | 31 | 25 |
| YearMade | 1985 | 1995 |
| MachineHoursCurrentMeter | 0.0000 |  |
| UsageBand | 0 | 0 |
| saledate | 2009-01-23 00:00:00 | 2009-01-23 00:00:00 |
| fiModelDesc | 654 | 3081 |
| fiBaseModel | 211 | 1127 |

我们现在已将所有字符串列转换为数值并处理了缺失的字符串值。

**标签编码类别变量的不合理有效性**

你可能想知道为什么将所有那些无序（名义）的类别变量转换为有序整数是“合法”的。我们确实知道假设类别之间存在顺序是错误的。简短的回答是，RF 模型仍然可以以预测的方式对转换后的类别特征进行分区，这可能会以更复杂的树模型为代价。这绝对不适用于许多模型，例如线性回归模型（它需要所谓的“虚拟”布尔列，每个唯一的类别值一个）。在实践中，我们发现标签编码类别变量非常有效，即使更高级的方法似乎会更好。

### 7.5.2 替换缺失的数值

要处理缺失的数值，我们建议分两步进行：

1.  对于列 *x*，创建一个新的布尔列 *x*`_na`，其中 *x*`[i]` 为真表示 *x*`[i]` 缺失。

1.  将列 *x* 中的缺失值替换为该列中所有 *x* 值的中位数。

这两个步骤在 Python 中有简单直接的等效操作，多亏了 Pandas：

```py
def fix_missing_num(df, colname):
    df[colname+'_na'] = pd.isnull(df[colname])
    df[colname].fillna(df[colname].median(), inplace=True)

```

让我们创建一个包含缺失值的数值列的玩具数据框：

```py
df_toy = pd.DataFrame(data={'YearMade':[1995,2001,np.nan]})
df_toy
```

|   | YearMade |
| --- | --- |
|   |
| --- |
| 0 | 1995.0000 |
| 1 | 2001.0000 |
| 2 |  |

然后将它通过我们的函数来查看其对数据框的影响：

```py
fix_missing_num(df_toy, 'YearMade')
df_toy
```

|   | YearMade | YearMade_na |
| --- | --- | --- |
|   |
| --- |
| 0 | 1995.0000 | False |
| 1 | 2001.0000 | False |
| 2 | 1998.0000 | True |

第三行的缺失值已被替换为 1998 年，即 1995 年和 2001 年的中位数，并且有一个新列`YearMade_na`表示我们进行了替换。

使用中位数的逻辑是，我们必须选择一个数字，所以我们不妨选择一个不会扭曲该列数据分布的数字。我们也不想选择一个极端值，因为模型可能会将其作为预测因素。但是，我们应该在我们的数据集中包含一个列，表明我们已经进行了这种替换，因为有时缺失值具有很强的预测性。例如，制造日期不明的推土机由于不确定性而可能价值较低。这种做法得到了最近学术研究的支持：[关于带有缺失值的监督学习的一致性](https://hal.archives-ouvertes.fr/hal-02024202v2)。

回到完整的数据集，我们之前通过解析英寸数将`Tire_Size`从字符串转换为数值列：

```py
print(f"Values {df['Tire_Size'].unique()}")
print(f"Median {df['Tire_Size'].median()}")

```

值 [ nan 26.5 20.5 17.5 23.5 14\. 13\. 29.5 15.5 7\. 10\. 23.1] 中位数 20.5

这仍然留下了很多缺失值：

```py
df[['Tire_Size']].head(6)
```

|   | Tire_Size |
| --- | --- |
|  |
| --- |
| 289125 |  |
| 289126 |  |
| 289127 |  |
| 289128 |  |
| 289129 | 26.5000 |
| 289130 |  |

应用`fix_missing_num()`后，所有表示缺失值的`np.nan`都已被替换为`Tire_Size`的中位数 20.5：

```py
fix_missing_num(df, 'Tire_Size')
df[['Tire_Size']].head(6)
```

|   | Tire_Size |
| --- | --- |
|  |
| --- |
| 289125 | 20.5000 |
| 289126 | 20.5000 |
| 289127 | 20.5000 |
| 289128 | 20.5000 |
| 289129 | 26.5000 |
| 289130 | 20.5000 |

我们还必须修复我们转换成数字的其他列中的缺失值：

```py
fix_missing_num(df, 'Undercarriage_Pad_Width')

```

并非所有缺失值都由`np.nan`表示。有时人们在数据录入或某些转换过程中使用特殊的指示值来表示缺失值。有两个数值列具有这样的指示值，我们应该修复它们，因为特征重要性图表明它们很重要。

一看`YearMade`与推土机售价之间的关系，我们就知道有问题：

» *由代码生成左侧*

![](img/bulldozer-intro_sniff_40.svg)

```py
df_small = df.sample(n=5_000) # don't draw too many dots
df_small.plot.scatter('YearMade','SalePrice', alpha=0.02, c=bookcolors['blue'])
```

在公元 1000 年人类制造推土机似乎不太可能。要么卖家不想承认推土机的年龄，要么不知道推土机的年龄。不清楚为什么有人选择 1000 作为指示值而不是 0 或-1，但我们可以通过将 1000 替换为`np.nan`来解决这个问题。然后，我们可以应用我们处理缺失数值的标准程序：

```py
# There are some unlikely 1919, 1920 values too
# Assume < 1950 is "unknown"
df.loc[df.YearMade<1950, 'YearMade'] = np.nan
fix_missing_num(df, 'YearMade')

```

![](img/bulldozer-intro_sniff_42.svg)

现在，制造年份与售价看起来要合理得多。

这个列还有一个问题。一些记录表明推土机是在制造之前被出售的，尽管在我们的训练子集的最后 10 万条记录中只有一条：

```py
inverted = df.query("saledate.dt.year < YearMade")[['SalePrice','YearMade','saledate']]
inverted
```

|   | SalePrice | YearMade | saledate |
| --- | --- | --- | --- |
|  |
| --- |
| 344948 | 35000 | 2012.0000 | 2010-05-06 |

通过将`YearMade`设置为销售日期的年份（假设销售日期比制造日期更近且更准确），我们可以轻松解决这个问题：

```py
df.loc[df.eval("saledate.dt.year < YearMade"), 'YearMade'] = df['saledate'].dt.year

```

另一个具有特殊值的数值列是`MachineHoursCurrentMeter`。乍一看，拥有 0 小时机器的推土机似乎只是一台新推土机。让我们筛选出缺失小时数为 0 的记录，并查看`YearMade`的直方图：

» *由代码生成左侧*

![](img/bulldozer-intro_sniff_45.svg)

```py
df.query("MachineHoursCurrentMeter==0")['YearMade'].plot.hist(bins=30)
```

这些制造日期都早于 2009 年，这是我们的数据子集中的第一年销售。不太可能所有这些推土机从制造时间起就闲置，直到多年后的销售日期。从这个角度来看，0 必须表示未知或“你真的不想知道”的机器小时数。让我们将这些零转换为`np.nan`，并调用`fix_missing_num()`：

```py
df.loc[df.eval("MachineHoursCurrentMeter==0"),
       'MachineHoursCurrentMeter'] = np.nan
fix_missing_num(df, 'MachineHoursCurrentMeter')

```

在处理这些缺失的数值后，数据框末尾出现了三个新的列：

```py
df[df.columns[-3:]].head(5)
```

|   | Undercarriage_Pad_Width_na | YearMade_na | MachineHoursCurrentMeter_na |
| --- | --- | --- | --- |
|  |
| --- |
| 289125 | True | False | True |
| 289126 | True | False | True |
| 289127 | True | False | True |
| 289128 | False | False | True |
| 289129 | True | False | True |

到目前为止，所有特征都是数值型的，除了`saledate`，缺失值也已修复。

## 7.6 使用所有特征训练模型

现在我们已经将数据框准备成纯数字，我们可以使用所有特征来训练模型，并将其性能与基线进行比较。唯一的例外是`saledate`仍然是一个时间戳，但我们在下一章中会对它做些特别处理。以下是常规的训练序列：

```py
X, y = df.drop(['SalePrice','saledate'], axis=1), df['SalePrice']
rf, oob_all = test(X, y)

```

使用 5,151,648 个树节点和 43.0 的中位树高，OOB R² 为 0.89871

0.899 这个分数相较于我们的基线分数 0.845 有了很大的提升，但通过特征工程我们可以做得更好，这是下一章的主题。让我们对这个清理过的数据集进行快照，以避免重复同样的过程：

```py
df = df.reset_index(drop=True)
df.to_feather("data/bulldozer-train-clean.feather")

```

![](img/bulldozer-intro_sniff_52.svg)

为了帮助我们集中特征工程的努力，让我们检查特征重要性图（右侧边栏）以查看模型认为哪些特征具有预测性。`YearMade`仍然非常重要，但在该特征上已经没有太多可以做的事情。考虑到它们的重要性，我们应该仔细查看`ProductSize`、`fiProductClassDesc`、`Enclosure`、`Hydraulics_Flow`、`fiSecondaryDesc`等特征。还要注意不重要特征的长期尾部。这些特征可能是真正不重要的，也可能是对于记录的小子集极端重要的。最佳策略是在模型中保留所有特征直到最后，然后逐渐移除它们直到准确率下降。

## 7.7 摘要

在本章中，我们对推土机数据集进行了大量的清理工作，主要与转换列数据类型和处理缺失的数值型和字符串值有关。清理过程的一部分是识别物理上存在的数字或字符串，这些数字或字符串实际上代表缺失值。我们的`df_normalize_strings()`函数将字符串的缺失概念归一化为`np.nan`，但我们必须手动识别缺失值的指示符，例如 1000 年的中世纪销售日期。

本章最重要的教训是如何处理缺失值。由于我们推荐的标签编码过程，缺失的类别值会自动处理：将类别转换为唯一的整数值；缺失值，`np.nan`，变为类别代码 0，而所有其他类别代码为 1 及以上。处理缺失的数值型值需要创建新列并替换`np.nan`：

1.  对于列*x*，创建一个新的布尔列*x*`_na`，其中*x*`[i]`为真表示*x*`[i]`是缺失的。

1.  将列*x*中的缺失值替换为该列所有*x*值的平均值。

到目前为止，你已经掌握了一些良好的数据清洗技能，你知道如何归一化和将字符串列编码为数值型值，也知道如何处理缺失值。这意味着你知道如何为模型训练准备数据集。在下一章，我们将加强你的特征工程技能。
