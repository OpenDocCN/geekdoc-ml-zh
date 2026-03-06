# Python 中的线性回归与弹性网回归

> 原文：<https://medium.com/mlearning-ai/linear-regression-vs-elastic-net-regression-in-python-ea52a11efd7a?source=collection_archive---------4----------------------->

![](img/c4c3389053dcf1c0d5b2883816b58d45.png)

Photo by [Clint Adair](https://unsplash.com/es/@clintadair?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我最近发现了使用 Python 构建模型的 elastic net，所以我想我应该把它写下来，因为它对社区有好处，也是我自己的参考。

弹性网回归是将岭回归和套索回归结合成一个算法。超参数阿尔法和 L1 比率控制着我们对算法施加的惩罚。例如，如果 L1 比率=0，我们有岭回归，如果 L1 比率= 1，我们有套索。

弹性净回归的一个伟大之处在于，它可以通过将弱变量尽可能地变为 0 或接近 0 来删除弱变量。这些是组成弹性网的套索和脊的元素。此外，我喜欢弹性网的一个好处，我想提的是它能够强制变量为正。例如，如果 KPI 的所有驱动因素都是积极的，而线性回归显示其中一个因素有负面影响，那么您可以使用弹性网络强制所有驱动因素都是积极的。

下一步，让我们运行一个线性回归和弹性净回归的例子，看看它们的区别。

1.  让我们导入本练习所需的所有库

```
import numpy as np
import pandas as pd
from sklearn.model_selection import GridSearchCV
from sklearn.linear_model import ElasticNet
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error
```

2.从 UCI 下载数据到熊猫数据框架，并设置我们的 X 和 Y 变量

```
url = ‘[http://archive.ics.uci.edu/ml/machine-learning-databases/wine-quality/winequality-red.csv'](http://archive.ics.uci.edu/ml/machine-learning-databases/wine-quality/winequality-red.csv')
df = pd.read_csv(url, sep=’;’)X=df.loc[:, ~df.columns.isin([‘quality’])]
y=df[‘quality’]
```

3.运行线性回归并报告 MSE

```
regression=LinearRegression()
regression.fit(X,y)
first_model_MSE=(mean_squared_error(y_true=y,y_pred=regression.predict(X)))
print(first_model_MSE)
#0.41676716722140805
```

4.查看线性回归的系数

```
coef_dict_baseline = {}
for coef, feat in zip(regression.coef_,X.columns):
 coef_dict_baseline[feat] = coef
coef_dict_baseline{'fixed acidity': 0.02499055267167325,
 'volatile acidity': -1.0835902586934376,
 'citric acid': -0.18256394841070975,
 'residual sugar': 0.01633126976547429,
 'chlorides': -1.8742251580992035,
 'free sulfur dioxide': 0.004361333309096989,
 'total sulfur dioxide': -0.0032645797030686347,
 'density': -17.881163832495734,
 'pH': -0.41365314382176305,
 'sulphates': 0.9163344127211318,
 'alcohol': 0.27619769922688664}
```

正如我们在上面看到的，我们有 x 中所有变量的系数。它们是否有意义是另一回事。我对葡萄酒质量了解不多，无法判断！

5.让我们用标准化的值建立一个弹性网络。然而，作为第一步，我们需要使用网格搜索来帮助我们找到阿尔法和 L1 比率的可靠值，这将产生最佳模型。根据您的机器，此位需要几秒钟来运行。

```
elastic=ElasticNet(normalize=True)
search=GridSearchCV(estimator=elastic
 ,param_grid={‘alpha’:np.logspace(-5,2,8)
 ,’l1_ratio’:[0,.2,.4,.6,.8,1]}
 ,scoring=’neg_mean_squared_error’,n_jobs=1,refit=True,cv=10)search.fit(X,y)
search.best_params_
#{'alpha': 0.0001, 'l1_ratio': 0.4}
abs(search.best_score_)
#0.43413742198432625
```

6.用网格搜索确定的阿尔法和 L1 比值重新运行一个弹性网

```
elastic=ElasticNet(normalize=True,alpha=0.001,l1_ratio=0.4)
elastic.fit(X,y)
second_model_mse=(mean_squared_error(y_true=y,y_pred=elastic.predict(X)))
print(second_model_mse)
#0.46183127160398396
```

7.让我们回顾一下弹性网的系数，看看它们是如何比较的

```
coef_dict_baseline = {}
for coef, feat in zip(elastic.coef_,X.columns):
 coef_dict_baseline[feat] = coef
coef_dict_baseline{'fixed acidity': 0.014285479621796009,
 'volatile acidity': -0.6103163705349157,
 'citric acid': 0.1826794358574798,
 'residual sugar': 0.002222720304848855,
 'chlorides': -0.8351842130958829,
 'free sulfur dioxide': 0.0,
 'total sulfur dioxide': -0.0014228214139999087,
 'density': -21.637643588877914,
 'pH': -0.04056906676888897,
 'sulphates': 0.451421857492302,
 'alcohol': 0.14341814232976277}
```

你能注意到可变的游离二氧化硫是如何变成 0 的吗？因为它是一个弱变量，所以弹性网把它设为 0。此外，该模型略有不同，因为柠檬酸与线性回归的系数为负，而与弹性网的系数为正。您可能会认为，由于输入的变量及其设置不同，模型会有所不同。

如果你想在回归中强制所有变量为正，你会怎么做？

8.将正=真参数添加到弹性网络函数中，让我们重新运行模型

```
elastic=ElasticNet(normalize=True,alpha=0.001,l1_ratio=0.4, positive=True)
elastic.fit(X,y)
third_model_mse=(mean_squared_error(y_true=y,y_pred=elastic.predict(X)))
print(third_model_mse)
#0.5079920349191247coef_dict_baseline = {}
for coef, feat in zip(elastic.coef_,X.columns):
 coef_dict_baseline[feat] = coef
coef_dict_baseline{'fixed acidity': 0.01306609743058668,
 'volatile acidity': 0.0,
 'citric acid': 0.2824161323218671,
 'residual sugar': 0.0,
 'chlorides': 0.0,
 'free sulfur dioxide': 0.0,
 'total sulfur dioxide': 0.0,
 'density': 0.0,
 'pH': 0.0,
 'sulphates': 0.44743438091280563,
 'alcohol': 0.17087338339714536}
```

所以这里发生了一些有趣的事情。所有的变量都是正的(这是我们想要的),然而，那些不能被建模为正的变量被强制设为 0。游离二氧化硫为 0，因为它是一个弱变量，正如我们之前看到的。值得注意的是，我们使用了以前模型中的阿尔法和 L1 比率。让我们更进一步，通过网格搜索一个弹性网络超参数，强制变量为正，看看它会产生什么。

9.让我们重复第 5、6、7 步，但正值=真

```
elastic=ElasticNet(normalize=True, positive=True)
search=GridSearchCV(estimator=elastic
 ,param_grid={‘alpha’:np.logspace(-5,2,8)
 ,’l1_ratio’:[0,.2,.4,.6,.8,1]}
 ,scoring=’neg_mean_squared_error’,n_jobs=1,refit=True,cv=10)search.fit(X,y)
search.best_params_
#{‘alpha’: 0.00001, ‘l1_ratio’: 0.0}
abs(search.best_score_)
#0.48067437262107227elastic=ElasticNet(normalize=True,alpha=0.00001,l1_ratio=0.0, positive=True)
elastic.fit(X,y)
fourth_model_mse=(mean_squared_error(y_true=y,y_pred=elastic.predict(X)))
print(fourth_model_mse)
#0.46524447657943874coef_dict_baseline = {}
for coef, feat in zip(elastic.coef_,X.columns):
 coef_dict_baseline[feat] = coef
coef_dict_baseline{'fixed acidity': 0.031400560143859536,
 'volatile acidity': 0.0,
 'citric acid': 0.31972354746287335,
 'residual sugar': 0.0,
 'chlorides': 0.0,
 'free sulfur dioxide': 0.0,
 'total sulfur dioxide': 0.0,
 'density': 0.0,
 'pH': 0.0,
 'sulphates': 0.8106956946266386,
 'alcohol': 0.3400753372952517}
```

因此，这产生了一个与步骤 8 中相似的模型，除了系数因超参数而不同。

我希望有些人觉得这很有帮助。我知道我会更经常地使用弹性网，因为我已经遇到了它。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)