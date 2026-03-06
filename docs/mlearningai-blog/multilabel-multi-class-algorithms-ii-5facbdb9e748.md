# 多标记多类算法-II

> 原文：<https://medium.com/mlearning-ai/multilabel-multi-class-algorithms-ii-5facbdb9e748?source=collection_archive---------9----------------------->

通过输出编码分类器、功率转换、集成方法进行高级预测分析的新指南

![](img/2bca03431e68c0f49ff6940b6d84373b.png)

嗨，这是第一部分 [**链式和多标签算法的延续:通过多标签、多输出&链式**](https://bobrupakroy.medium.com/chained-and-multilabel-algorithms-6b378ec761d3) **的高级预测分析新指南。**

因此，今天我们将学习更多不同的方法来执行多标签多类预测任务，如 ***输出编码分类器、二进制相关性、自适应算法、集成方法和用于深度学习的离线多标签。***

> 所以让我们从 OutputCodeClassifier 开始:

1.  **输出编码分类器**

基于纠错输出码的策略与“一对一”和“一对一”有很大的不同。使用这些策略，每个类都表示在一个欧几里得空间中，其中每个维度只能是 0 或 1。另一种说法是，每个类由一个二进制代码(0 和 1 的数组)表示。跟踪每个类别的位置/代码的矩阵被称为代码簿。代码大小是前述空间的维度。直观地说，每个类别应该用一个尽可能唯一的代码来表示，并且应该设计一个好的代码簿来优化分类精度。在这个实现中，我们简单地使用了 3 中所提倡的随机生成的代码本，尽管将来可能会添加更复杂的方法。

在拟合时，对码本中的每个比特拟合一个二进制分类器。在预测时，分类器用于在类空间中投影新点，并且选择最接近这些点的类。

在 OutputCodeClassifier 中，code_size 属性允许用户控制将要使用的分类器的数量。占班级总数的百分比。

介于 0 和 1 之间的数字需要的分类器比 1 对其余的分类器少。理论上，log2(n_classes) / n_classes 足以明确地表示每个类。然而，在实践中，由于 log2(n_classes)比 n_classes 小得多，因此它可能不会导致良好的准确性。

大于 1 的数字将需要比 1 对其余的分类器更多的分类器。在这种情况下，一些分类器在理论上会纠正其他分类器所犯的错误，因此被称为“纠错”。然而，在实践中，这可能不会发生，因为分类器错误通常是相关的。纠错输出码具有与打包类似的效果。~机器学习精通。该脚本是一个简单的模板，我们可以按照它来应用 **OutputCodeClassifier**

[OutputCodeClassifier.py](https://gist.github.com/rupak-roy/8cc280e335d7792b63f4982932258fe6#file-outputcodeclassifier-py)

接下来，我们有

> **2。问题转化**
> 这种方法可以进一步以三种不同的方式进行:

2.1 .二进制相关性
2.2 .分类器链
2.3 .标签 Powerset

**2.1 二元相关性**
这基本上是把每个标签当作一个单独的单类分类问题。

[binary relevance](https://gist.github.com/rupak-roy/0e5781420be66a1a6e48e2b5e89e884a#file-binary-relevance)

**2.2 分类器链**

我们已经在上一篇文章中看到了它是如何应用的。

下面是链接[https://bobrupakroy . medium . com/chained-and-multi label-algorithms-6b 378 EC 761d 3](https://bobrupakroy.medium.com/chained-and-multilabel-algorithms-6b378ec761d3)

以防万一:

```
from skmultilearn.problem_transform import ClassifierChain
from sklearn.naive_bayes import GaussianNB# initialize classifier chains multi-label classifier
# with a gaussian naive bayes base classifier
classifier = ClassifierChain(GaussianNB())# train
classifier.fit(X_train, y_train)# predict
predictions = classifier.predict(X_test)accuracy_score(y_test,predictions)
```

**2.3 .标注电源组**

在这里，我们将问题转化为多类问题，一个多类分类器在训练数据中找到的所有唯一标签组合上被训练。
标签幂集为训练集中存在的每个可能的标签组合提供唯一的类别。

[Label Powerset](https://gist.github.com/rupak-roy/73b0b45d5a3fd9be45f462d812114b29#file-label-powerset)

接下来，我们有系综方法

> 3.**集合方法**
> 集合方法官方回购:[http://scikit.ml/api/classify.html#ensemble-approaches](http://scikit.ml/api/classify.html#ensemble-approaches)

最后同样重要的是**#深度学习多标签分类**

> 4.**深度学习多标签分类**

深度学习神经网络天生支持多标签分类问题。

```
from numpy import asarray
from sklearn.datasets import make_multilabel_classification
from keras.models import Sequential
from keras.layers import Dense

# get the dataset
def get_dataset():
 X, y = make_multilabel_classification(n_samples=1000, n_features=10, n_classes=3, n_labels=2, random_state=1)
 return X, y

# get the model
def get_model(n_inputs, n_outputs):
 model = Sequential()
 model.add(Dense(20, input_dim=n_inputs, kernel_initializer='he_uniform', activation='relu'))
 model.add(Dense(n_outputs, activation='sigmoid'))
 model.compile(loss='binary_crossentropy', optimizer='adam')
 return model

# load dataset
X, y = get_dataset()
n_inputs, n_outputs = X.shape[1], y.shape[1]# get model
model = get_model(n_inputs, n_outputs)
# fit the model on all data
model.fit(X, y, verbose=0, epochs=100)# make a prediction for new data
row = [3, 3, 6, 7, 8, 2, 11, 11, 1, 3]
newX = asarray([row])
yhat = model.predict(newX)
print('Predicted: %s' % yhat[0])
```

现在，我们也有了执行多标签分类技术的新方法。

如果你喜欢这篇文章，那么我相信你也会喜欢第一部分的[链和多标签算法](https://bobrupakroy.medium.com/chained-and-multilabel-algorithms-6b378ec761d3)文章，这篇文章讨论了更多的方法和技术来执行多标签、多类和链分类。

同样，长话短说，我尝试将它转换成一个更简单的版本 ***，*** 。我将尝试在数据科学领域引入尽可能多的新内容，我希望该软件包在您的工作中会有所帮助。 ***因为我相信机器学习不是取代我们，而是取代同样耗时费力的迭代工作。因此，人们应该来工作，创造创新，而不是被占领在相同的重复无聊的任务。***

*再次感谢您的时间，如果您喜欢这篇短文，我的 medium repo 中有大量关于高级分析、数据科学和机器学习的主题。*[***https://medium.com/@bobrupakroy***](/@bobrupakroy)

**我的一些另类网络存在**[insta gram](https://www.instagram.com/bobrupak/)[Udemy](https://www.udemy.com/course/ai-master-class)，Blogger，Issuu，Slideshare，Scribd 等等。

**Quora 上也有**@[https://www.quora.com/profile/Rupak-Bob-Roy](https://www.quora.com/profile/Rupak-Bob-Roy)

如果你需要什么，请告诉我。稍后再聊。

**Kaggle 实现:**

[https://www . ka ggle . com/rupakroy/multi label-multi-class-algorithms-ii](https://www.kaggle.com/rupakroy/multilabel-multi-class-algorithms-ii)

**Git 回购:**

**OutputCodeClassiifer:**[https://github . com/rupak-Roy/multi label-OutputCodeClassifier](https://github.com/rupak-roy/MultiLabel-OutputCodeClassifier)

**multi label-multi class-Power-Transformation-Approach:**[https://github . com/rupak-Roy/multi label-multi class-Power-Transformation-Approach](https://github.com/rupak-roy/MultiLabel-MultiClass-Power-Transformation-Approach)

![](img/128926ffb05aa46b8f3caa66e88e24ce.png)

Learning to score fish!

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)