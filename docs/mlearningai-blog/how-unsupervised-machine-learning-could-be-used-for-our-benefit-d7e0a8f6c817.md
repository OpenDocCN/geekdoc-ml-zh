# 无人监督的机器学习如何为我们服务？

> 原文：<https://medium.com/mlearning-ai/how-unsupervised-machine-learning-could-be-used-for-our-benefit-d7e0a8f6c817?source=collection_archive---------7----------------------->

![](img/964a6432e04461a5a24406c4e1f43143.png)

Photo by [Owen Beard](https://unsplash.com/@owenbeard?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

尽管在**数据科学**领域创造了许多**机器学习**和**深度学习模型**，但仍有一个我们会感兴趣的领域被称为**无监督机器学习**。由于存在各种格式的大量数据，因此很容易得出这样的结论:数据是巨大的，并且可以用于机器学习目的。然而，重要的是要注意，尽管有丰富的数据，但不可能使用这些数据。这是因为数据没有注释。这意味着人类必须花费时间和精力来注释标签**，这些标签可以分别用于机器学习目的。**

![](img/e34183bb45963cc4b546083e47470c79.png)

Photo by [Possessed Photography](https://unsplash.com/@possessedphotography?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

虽然有很多机器学习算法，但重要的是要注意，这些算法只有在数据受到监督时才有效。这意味着需要对输出值进行注释。由于机器学习领域的很大一部分应用是由**监督的**，所以**无监督学习**往往没有得到太多重视。

数据随处可得。但是，它是非结构化的，没有注释。这意味着尽管数据是可用的，但它不是以用于机器学习目的的形式可用的。然而，无监督学习可以用于**分段**和**聚类数据**，以便发现有趣的趋势。

## **什么是无监督机器学习？**

在无监督的机器学习中，没有**预定义的输出标签**，只有训练输入。因此，我们的模型必须能够在没有任何监督的情况下发现模式和隐藏的见解。有各种各样的无人监督的机器学习算法，我们将继续去理解和学习。现在让我们分别回顾一下一些无监督的机器学习模型。

## **K-均值聚类**

![](img/6e13083d0517949b6cf8f342a8ab0e3f.png)

Photo by [Roberto Reposo](https://unsplash.com/@reposo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

顾名思义，它是把整体的训练数据点分成 **K 个簇**。它是一种无监督的机器学习技术，不需要输出标签或因变量。在这种依赖于 K 值的方法中，会有许多**形心**，它们会不断变化，直到达到一个固定点。根据迭代次数和选择的聚类，算法将对数据点进行分类。它将首先从单个数据点开始，然后形成初始聚类，并取这些聚类的平均值来找到质心。这个过程一直持续到我们分别得到想要的输出。这种方法也被称为**病房的方法**。

## **关联规则**

![](img/4c4c58d59b72b1140b1e8c7891f38259.png)

Photo by [Tingey Injury Law Firm](https://unsplash.com/@tingeyinjurylawfirm?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在这种类型的无监督学习中，在执行聚类时要考虑各种规则。在这种方法中，数据根据一组特定的规则被关联到不同的聚类中，并相应地进行分类。因此，基于关联方法，大数据被分成较小的数据和聚类。

## **自动编码器**

![](img/bcddb82d13a8929eca024e45a2d31e70.png)

Photo by [Matheo JBT](https://unsplash.com/@matheo_jbt?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

当我们在机器学习中处理庞大的数据集时，模型要在数据上表现良好，计算成本会很高。因此，它们可能需要额外的资源，如 **GPU** 或 **CPU** 才能有效运行。**自动编码器**在不损失数据完整性的情况下降低数据的维度。所有这些都确保了分配用于训练机器学习模型的资源将在很大程度上减少。

## **无监督机器学习有哪些应用？**

当我们谈论机器学习和数据科学时，我们会非常强调它的监督方面。换句话说，我们所说的算法只有在使用监督进行训练时，才会在测试集上表现良好。尽管如此，仍有各种格式的未标注数据。因此，可以从这些未标记的数据中发现有趣的见解和趋势。下面是一些无监督机器学习的例子。

## **客户细分**

在这种机器学习方法中，我们将根据一组特定的属性和客户特征之间的共性对客户进行分类。因此，根据可以采用的不同销售策略创建了不同的组，这反过来有助于将业务导向这些客户。

## 推荐系统

在这个机器学习问题中，我们将要处理的数据不包含因变量，而只包含自变量。因此，这将是一个无监督的机器学习问题，其中不存在目标或输出变量。我们得到一个数据集，其中包含不同的用户和他们对不同电影的评级。我们得到一个**稀疏矩阵**，因为不是每个用户都会评价所有的电影，电影列表也很大。因此，机器学习模型可以来自无监督的方法，例如 K-means 聚类或关联规则。

## 产品细分

与客户细分的方式类似，产品细分也是在无监督机器学习的帮助下进行的。根据选择的功能，与其他产品相同的产品被分组在一起。在执行多次迭代之后，特别是使用 **k-means** ，它将准确地分割产品信息，以便我们得出关于产品某些属性的结论。因此，客户在做出购买决定之前会考虑彼此相似的产品。

## **DNA 序列分割**

当存在大量 DNA 样本和序列形式的数据时，细胞研究中的科学家和专家有时无法准确识别和分割 DNA 序列。这是因为产生和鉴定的序列数量正在成倍增加。无监督机器学习用于分割图像和理解图像的过程中。因此，这也是无监督机器学习的一个有趣的应用。

## 结论

读完这篇文章后，你可能会很好地理解无监督机器学习是如何工作的，以及它在各种行业中的用途。希望这篇文章对你有所帮助。欢迎分享您的反馈和建议。谢了。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)