# Scikit-learn 中机器学习工作流程的最佳实践

> 原文：<https://medium.com/mlearning-ai/best-practices-for-your-machine-learning-workflow-in-scikit-learn-2fe857bcb962?source=collection_archive---------2----------------------->

![](img/f38edb1eeefde644cf4faab960b558ca.png)

Photo Credit: Kevin Wu via Unsplash

在我学习数据科学的过程中，我接触了大量的 Scikit-learn。Scikit learn 是 Python 最流行的机器学习库之一。在本文中，我将讨论在使用 Scikit-learn 库构建有效模型时需要记住的一些重要实践。

# 保持数据预处理的一致性

Scikit-learn 提供了许多用于进行数据转换的库和方法。为模型定型时使用的任何数据集转换也必须用于测试数据或部署到生产系统中。例如，如果您使用他们的 StandardScaler 来缩放 X train 以适合您的模型，则在获取预测或模型度量时，您还必须在 X_test 上使用 StandardScaler。不应用一致的处理步骤将导致意外结果，并导致模型无法有效执行。

以下代码块由 Sci-kit learn 的文档提供，显示了对测试数据进行预测的**错误的**方式，而没有以与用于拟合模型的训练数据相同的方式对其进行处理

```
**>>> from** **sklearn.metrics** **import** mean_squared_error**>>> from** **sklearn.linear_model** **import** LinearRegression**>>> from** **sklearn.preprocessing** **import** StandardScaler**>>>** scaler = StandardScaler()**>>>** X_train_transformed = scaler.fit_transform(X_train)**>>>** model = LinearRegression().fit(X_train_transformed, y_train)**>>>** mean_squared_error(y_test, model.predict(X_test))62.80...
```

下面的代码块显示了**正确的**方式

```
**>>>** X_test_transformed = scaler.transform(X_test)**>>>** mean_squared_error(y_test, model.predict(X_test_transformed))0.90...
```

# 避免数据泄露

> 数据泄漏是指在模型训练过程中使用了在预测时预计不可用的信息，导致预测分数(指标)过高估计了模型在生产环境中运行时的效用。

数据泄漏将导致您的模型表现得比预期的要好。

以下是使用 Scikit-learn 时为避免数据泄露需要考虑的一些事项:

从不符合测试数据。

当执行特征选择算法时，仅使用训练数据。训练，先测试分割你的数据，然后对训练数据做特征选择。

如果您使用一些平均值或其他统计数据来对数据进行特征工程、估算、缩放或其他预处理，则应仅使用基于训练子集的统计数据，否则来自训练子集的信息将会影响模型。

注意/避免使用泄漏预测器作为特征。你不应该使用在目标实现后更新的变量。例如，如果您正在构建一个模型来预测人们是否生病，您不应该使用人们已经服药的事实来拟合该模型。人们吃药是因为生病，而不是相反(通常情况下)。在看不见的数据上部署该模型会导致不准确的结果！

**错了**

```
**>>>** *# Incorrect preprocessing: the entire data is transformed***>>>** X_selected = SelectKBest(k=25).fit_transform(X, y)**>>>** X_train, X_test, y_train, y_test = train_test_split(**…** X_selected, y, random_state=42)
```

**右**

```
**>>>** X_train, X_test, y_train, y_test = train_test_split(**... **    X, y, random_state=42)**>>>** select = SelectKBest(k=25)**>>>** X_train_selected = select.fit_transform(X_train, y_train)
```

交叉验证测试和训练折叠也应遵循上述指南

# **管道**

Scikit-learn 有一个名为 Pipelines 的对象，它将帮助您避免许多来自坏代码的上述陷阱。一位核心开发人员自己建议每个人在使用 Scikit 时都应该使用管道。

> 管道允许您封装所有预处理步骤、特征选择、缩放、变量编码等等，以及您通常在单个估计器中拥有的最终监督模型。- [Andreas Muller](https://datascience.columbia.edu/andreas-mueller) ，Scikit-learn 的核心开发者

管道在进行交叉验证时特别有用，因为它可以防止从训练到测试折叠的数据泄漏，并允许交叉验证评分提供更准确的模型性能图。

# 结论

现在你有了一些在使用 Scikit-learn 时应该遵循的重要实践，这样你的模型就可以有效地处理看不见的数据！

# 来源

https://scikit-learn.org/stable/common_pitfalls.html

[https://scikit-learn . org/stable/modules/compose . html # pipeline](https://scikit-learn.org/stable/modules/compose.html#pipeline)

[https://drgabriel Harris . medium . com/python-how-sci kit-learn-0-20-optimal-pipeline-and-best-practices-DC 4d d94 D2 c 09](https://drgabrielharris.medium.com/python-how-scikit-learn-0-20-optimal-pipeline-and-best-practices-dc4dd94d2c09)

[https://towards data science . com/want-to-true-master-scikit-learn-2-essential-tips-from-the-official-developer-self-dada 6 ff 56 b 99](https://towardsdatascience.com/want-to-truly-master-scikit-learn-2-essential-tips-from-the-official-developer-himself-dada6ff56b99)

[https://en . Wikipedia . org/wiki/Leakage _(机器学习)](https://en.wikipedia.org/wiki/Leakage_(machine_learning))

[https://www.kaggle.com/dansbecker/data-leakage](https://www.kaggle.com/dansbecker/data-leakage)