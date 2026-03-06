# ML 算法:基础模型的逻辑回归。

> 原文：<https://medium.com/mlearning-ai/ml-algorithm-logistic-regression-for-a-base-model-35ca5f5029e4?source=collection_archive---------5----------------------->

![](img/01383c9311dabac685f199e4518f9e60.png)

Photo by [Gérôme Bruneau](https://unsplash.com/ja/@geromebruneau?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

通常现实世界中有监督的机器学习问题是分类问题而不是回归问题，这里我们需要预测的*的定性值通常称为 ***类别*** 或 ***类别*** 。比如:预测一个客户是会*流失*还是*而不是*，或者我们需要发现一个肿瘤是*恶性*还是*良性*的问题。*

*回归算法预测数字输出值，而分类算法预测给定实例所属的特定类。预测分类响应值的过程被称为 ***分类***；*

**逻辑回归*被认为是分类问题中基础模型的首选之一。在本文中，您将学习逻辑回归的基础知识，为什么它被命名为回归，以及它的数学实现和一个案例研究的例子。*

*在学习逻辑回归算法之前，让我们先了解为什么我们需要另一种算法来进行预测，以及为什么我们不能使用线性回归。*

*让我们考虑一个机器学习的经典例子；iris-flower 数据集，其中我们需要使用 ***线性回归*** 算法来预测三个物种( *Iris-setosa、Iris-virginica、Iris-versicolor* )中的 ***iris-flower*** 的类别。*

*线性回归算法假设*响应变量* **Y** 是一个数值。因此，让我们将分类变量 Y 值转换成数值响应值，如*鸢尾属的 *1* ，鸢尾属的*2*，鸢尾属的*和 *3* 。*

*在对数据训练模型之后，该算法以这样的方式学习 Y 值之间的关系，即 Iris-setosa 和 Iris-virginica 之间的差异与 Iris-virginica 和 Iris-versicolor 之间的差异相同。但在现实中，没有这样的排序，人们可以选择任何其他数值，这将导致一个具有新关系的新模型，它可能预测不同的测试数据预测值集。*

*如果响应变量值中存在自然排序(例如低、中和高)，那么用具有相同间隙的有序数据对其进行编码将是合理的，但大多数情况下，这是不实际的。*

*用于二元分类(仅用于两类，如真/假或 1/0)。例如，如果我们需要预测客户是否会流失，那么我们可以忽略 y 是离散值的事实来处理分类问题，并使用我们的线性回归算法来预测给定 X 值的输出 y。*

*我们可以预测，如果 y >= 0.5，客户会流失；如果 y 小于 0.5，客户不会流失。但是有时输出很难解释，因为预测值可能大于 1 或小于 0。但实际上 y 必须在 0 和 1 之间。*

*![](img/c292a8b44f3df97ca4a698c5c4e6bedd.png)*

# *表现*

*为了避免这个问题，我们必须使用一个函数，该函数对于 x 的所有值产生范围为 0 和 1 的输出。为此，我们使用一个 ***logit 函数*** 或 ***sigmoid 函数*** ，将其应用于线性回归模型函数。*

*线性回归的假设函数表示为*

*![](img/0315eacde6a411d8fd29650b2295f4e5.png)*

*这里，`theta`表示模型的参数，`x`是输入向量。*

*logit 函数表示如下:*

*![](img/846c7e38ab6d4b56e411b8efee3331b9.png)*

*这里，*

*![](img/63a66cb8a61be8d8be33895d87e32e06.png)*

*在线性回归函数上应用 logit 函数，我们得到了逻辑函数的表示:*

*![](img/3fc6d17ee749c769f8917fc5add59b58.png)*

*这是*逻辑回归*模型的表示。通过计算给定输入 X 的θ值，我们可以计算假设函数的值，然后通过将 logit 函数应用于我们的假设，我们可以决定给定实例的类。*

*logit 函数不产生实际输出(对于二元分类，实际输出是 1 或 0 ),而是计算每个类的概率。*

*![](img/809f8db34154f1f4a71659ab3bb63d3f.png)*

*如果小于 0.5，则标签通常为 0 或负数；否则，如果它大于 0.5，则输出标签为 1 或正。(这是针对二元分类的)*

# *估价*

*逻辑回归的成本函数取决于数据点的实际类别。如前所述，函数的输出是类的概率。假设对于类别为 1 的数据点，logit 函数的输出为 0.75，则该情况的误差或损失为 0.25*(1–0.75)*。但是如果该数据点属于类别 0，那么误差是 0.75。*

*在多类分类中，我们计算每个类的概率，每次选择一个类作为正(1)类，而其他类是负(0)类，然后计算假设。*

# *逻辑回归的假设*

*   *逻辑回归假设因变量`y`本质上必须是绝对的。*
*   *它假设独立要素之间不存在多重共线性。*

# *线性回归和逻辑回归有什么区别？*

*除了对这些算法的因变量`y`的假设。这两种回归模型的主要区别在于*

*   *线性回归的输出是具有单个全局最小值的凸函数，而逻辑回归函数的输出是具有多个局部最小值的非凸函数。*
*   *逻辑回归预测特定产出的概率，而线性回归预测实际产出。*

# *逻辑回归的类型*

*   ****二项式 logistic 回归*** —在二项式 Logistic 回归中，只有两种可能的结果，例如，1 或 0，真或假等。*
*   ****多标逻辑回归*** —在多标逻辑回归中，有两种以上可能的无序结果。例如，鸢尾属花卉品种鸢尾、海滨鸢尾和杂色鸢尾。*
*   ****有序逻辑回归*** —在有序逻辑回归中，有两个以上可能的有序结果，例如，低、中或高。*

# *使用 Scikit-learn 实现*

*让我们使用流行的用于机器学习算法的 Python 库 *scikit-learn* 来实现逻辑回归算法。*

*对于我们的分类示例，我们将使用流失预测数据，您可以从 Kaggle 下载该数据集。*

*我们的目标是使用 scikit-learn 库建立一个逻辑回归模型来预测客户是否会流失。这里，用于训练模型的数据是预处理数据。*

*准备好整个数据集后的第一步是将数据分成训练集和测试集，也可以分成验证集。因此，该模型可以通过在训练数据集上对其进行训练、在验证数据集上进行评估和调整，以及在测试数据集上检查其性能来学习。*

*让我们首先导入必要的库，并使用`read_csv`函数将数据读入熊猫的数据框。*

```
*# Importing libraries
import pandas as pd
import numpy as np

from sklearn.model_selection import train_test_split
from sklearn.feature_extraction import DictVectorizer
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import roc_auc_score

# Load the data
df = pd.read_csv("data.csv")*
```

*现在，让我们将数据分成训练集和测试集。我们将使用一个占整个数据集 30%的测试集。*

```
*# Split the data
df_train, df_test = train_test_split(df, test_size=0.3, random_state=1)

# shape
print("Training shape:: ", df_train.shape)
print("Testing shape:: ", df_test.shape)## Ouput
Training shape:: (4930, 21)
Testing shape:: (2113, 21)*
```

*从输出中，我们可以看到我们的训练数据集有 4930 个训练示例或数据点，而测试数据集有 2113 个示例。下一步，我们将转换数据，并在训练数据集上训练一个 ***逻辑回归*** 模型。*

```
*# create y_train and y_test
y_train = df_train['churn']
y_test = df_test['churn']# delete `churn` attribute from data
del df_train['churn']
del df_test['churn']train_dict = df_train.to_dict(orient='records')
test_dict = df_test.to_dict(orient='records')# Data transformation
dv = DictVectorizer(sparse=False)
X_train = dv.fit_transform(train_dict)
X_test = dv.transform(test_dict)# Train the model
clf = LogisticRegression(max_iter=1000, solver='liblinear')
clf.fit(X_train, y_train)# Prediction
y_preds = clf.predict_proba(X_train)
print(y_preds[0])## Output - [0.8218741 0.1781259]*
```

*这些是训练集中第一个实例的预测概率。`predict_proba`分类器的功能将返回每一类的概率。因为这是一个二进制分类问题，所以上面的输出分别代表了第一个`train`集合的`class 0`和`class 1`的概率数组。*

*下面的代码将选择第 1 类的概率，因为我们想要预测客户流失率。*

```
*# Model evaluation
y_preds = clf.predict_proba(X_train)[:, 1]
auc = roc_auc_score(y_train, y_preds)
print("Train score:: %.3f" % auc)y_preds = clf.predict_proba(X_test)[:, 1]
auc = roc_auc_score(y_test, y_preds)
print("Test score:: %.3f" % auc)## Output - 
Train score:: 0.845
Test score:: 0.858*
```

*从上面的准确度得分，我们可以说我们的逻辑回归模型将以 85%的准确度预测客户是否会流失。*

**在本文中，我们通过一个客户流失预测的例子学习了用于分类的逻辑回归算法，并简要介绍了数学解释。即使逻辑回归是来自线性回归的假设函数和 logit 函数的组合。逻辑回归的成本函数的评估不同于线性回归。可以在这里* 详细学习逻辑回归[](https://en.wikipedia.org/wiki/Logistic_regression)*

***我希望这篇简介能帮助你理解逻辑回归算法的一些基本概念。***

****感谢您的阅读！****

**[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)**