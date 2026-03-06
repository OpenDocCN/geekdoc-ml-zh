# 机器学习中的数据准备

> 原文：<https://medium.com/mlearning-ai/data-preparation-in-machine-learning-2bafe182d7eb?source=collection_archive---------3----------------------->

数据准备是机器学习的一个非常重要的方面。出于各种原因，这对于任何机器学习项目都是至关重要的。我们将讨论这些原因以及为我们的模型准备数据的一些方法。

![](img/7c751defd9c5619bdd1b566cd2e28f32.png)

Photo by [Markus Winkler](https://unsplash.com/@markuswinkler?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 什么是数据准备？

数据准备是我们清理和转换数据的过程，将数据转换成我们的机器学习项目可以使用的形式。在这个过程中，原始数据被转换，以便更好地理解和分析。

## 但是为什么我们需要准备数据呢？

数据准备背后的最大原因是机器学习中使用的算法大多使用数字输入。你的数据中也可能有误差，这可能是简单的观察误差。

此外，很多时候，如果数据中的要素不在同一比例上，算法生成的假设可能会受到很大影响。

让我们假设数据中的两个特征是 X1 和 X2。

X1 = [1，4，2，5，2，5，7，8，9，3]

X2 =[21423412331253653153983170749]

正如我们所见，X1 和 X2 的值相差很大。这可能会影响模型的性能。如果我们将 X1 和 X2 调整到特定范围内的值，将会非常有帮助。

## 数据准备方法

有许多不同的方法可以用来准备您的数据，以便在您的机器学习算法中使用，我们将讨论其中的一些方法以及它们使用 Scikit-Learn 的实现。

本博客中讨论的方法有:

*   最小最大缩放器
*   标准化者
*   标准缩放器
*   二值化器

所有这些方法都可以在 Scikit-learn 中找到。你可以在这里查看。

# 最小最大缩放器

顾名思义，MinMaxScaler 有助于在最小值和最大值的指定范围内缩放数据。

这些值通过使用给定的公式进行缩放:

```
std_x = (x-min(x)) / X_max — X_minscaled_X = std_x * (max - min) + min
```

其中 X_max 和 X_min 分别是数据中 X 的最大值和最小值，min 和 max 是您在算法中指定的范围的最小值和最大值。

让我们以范围(0，1)为例来理解这一点。假设 X 是

X = [6，148，72，35，0，33.6，0.627，50]。

我们可以看到 min(x) = 0，max(x) = 148

rescaled_X = [0.040，1，0.486，0.236，0，0.227，0.004，0.337]。

让我们看看使用 Scikit-learn 实现 MinMaxScaler，我们将使用每日最低温度数据集。数据集可以在这里找到[。](https://www.kaggle.com/paulbrabban/daily-minimum-temperatures-in-melbourne)

```
from pandas import read_csv
from sklearn.preprocessing import MinMaxScalerdataset = read_csv('daily-min-temperatures.csv', header=0, index_col=0)data_values = dataset.values
min_max_scaler = MinMaxScaler(feature_range=(0,1))
scaler = min_max_scaler.fit(values)scaled_X = scaler.transform(values)
orig_X = scaler.inverse_transform(scaled_X)
```

正如你所看到的，我们从使用 pandas 库的`read_csv`函数导入数据开始。你可以在这里阅读`read_csv` [内部传递的参数](https://pandas.pydata.org/docs/reference/api/pandas.read_csv.html)。

然后我们使用 create a instance of`MinMaxScaler`并使用`feature_range`指定输出值的范围。然后`fit`函数计算出最小值和最大值，用于以后的缩放。

然后使用`transform`函数获得 X 的换算值。

但是，如果您希望从重新调整的值中获得 X 的原始值，该怎么办呢？

这就是`inverse_transform`出现的原因，它根据指定的特征范围撤销 X 的缩放。

你可以在这里阅读更多关于 MinMaxScaler [的内容](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html)。

# 标准化者

归一化函数，顾名思义，将输入值归一化为单位范数。

我们可以使用这个函数，通过不同种类的向量范数方法来归一化这些值。你可以在这里阅读不同的向量规范。

我们可以使用 Scikit-learn 实现规范化。我们使用的是皮马印第安人糖尿病数据集，可以在[这里](https://raw.githubusercontent.com/jbrownlee/Datasets/master/pima-indians-diabetes.data.csv)找到。

```
from pandas import read_csv
from sklearn.preprocessing import MinMaxScalerdataset = read_csv('pima-indians-diabetes.csv', header=0, index_col=0)data_values = dataset.valuesX = data_values[:,0:8]
Y = data_values[:,8]scaler = Normalizer().fit(X)
normalizedX = scaler.transform(X)
```

这里，我们首先导入数据集，并从中取出输入要素和输出的值。

然后我们创建了一个 Normalizer 实例，并对数据的输入特征使用了`fit`，然后使用`transform`提取这些特征的归一化值。

我们可以在创建规格化器本身的实例时指定要使用的规格化类型。可以使用的定额的选择有` ***{'l1 '，' l2 '，' max'}。***

你可以在这里阅读更多关于规格化器的内容。

# 标准缩放器

StandardScaler 的工作原理是从 X 中移除平均值，并将其缩放至单位方差。

我们像这样计算 X 的标准分数

```
std_X = (X - mean(X)) / StdDev(X)
```

其中 mean(X)是 X 的均值，StdDev(X)是 X 的标准差。

我们可以实现 Scikit-learn 库的标准缩放器。我们将使用皮马印第安人糖尿病数据集，可以在[这里](https://raw.githubusercontent.com/jbrownlee/Datasets/master/pima-indians-diabetes.data.csv)找到。

```
from pandas import read_csv
from sklearn.preprocessing import StandardScalerdataset = read_csv('pima-indians-diabetes.csv', header=0, index_col=0)data_values = dataset.valuesX = data_values[:,0:8]
Y = data_values[:,8]scaler = StandardScaler().fit(X)
rescaled_X = scaler.transform(X)
```

正如我们所见，`fit`和`tranform`功能与我们上面使用的功能相同。

你可以在这里阅读更多关于 StandardScaler [的内容。](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html)

# 二值化器

该函数根据指定的阈值将数据二进制化为 0 或 1。大于阈值的值映射到 1，否则映射到 0。

我们可以通过一个例子来理解这一点，

```
X = [[123,324,234324,34,234,234,234,123,21,312,312,3]]binarized_X = [[0 1 1 0 0 0 0 0 0 1 1 0]]
```

这些值是在我们将阈值设置为 **235** 时获得的。

让我们看看如何使用 Scikit-Learn 库的内置函数实现二进制化。

```
from sklearn.preprocessing import BinarizerX = [[123,324,234324,34,234,234,234,123,21,312,312,3]]binarizer = Binarizer(threshold=235).fit(X)binaryX = binarizer.transform(X)
```

同样，我们可以看到二进制化器中使用的函数与我们上面讨论的类似。

你可以在这里了解更多关于二值化器[的信息。](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.Binarizer.html)

为您的机器学习算法选择数据准备方法是高度特定于您的数据的，并且不能预先固定。

数据准备是任何机器学习项目中最重要、有时也是最困难的任务之一。

在这篇博客中，我们了解了数据准备的含义，为什么它很重要，以及我们可以用来准备机器学习项目中使用的数据的不同方法。

确保尝试使用不同值的 Python 代码，以便更好地理解它们的工作原理。

## 参考

[](https://machinelearningmastery.com/what-is-data-preparation-in-machine-learning/) [## 什么是机器学习项目中的数据准备——机器学习掌握

### 数据准备可能是任何机器学习项目中最困难的步骤之一。原因是每个…

machinelearningmastery.com](https://machinelearningmastery.com/what-is-data-preparation-in-machine-learning/) [](https://scikit-learn.org/stable/modules/preprocessing.html) [## 6.3.预处理数据-sci kit-学习 0.24.2 文档

### sklearn.preprocessing 包提供了几个常用的实用函数和转换器类来改变 raw…

scikit-learn.org](https://scikit-learn.org/stable/modules/preprocessing.html)