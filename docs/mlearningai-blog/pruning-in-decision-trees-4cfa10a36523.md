# 决策树中的修剪

> 原文：<https://medium.com/mlearning-ai/pruning-in-decision-trees-4cfa10a36523?source=collection_archive---------2----------------------->

![](img/18bf4bc0d65534e3fe0743d36f8439ff.png)

Photo by [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

[决策树](/mlearning-ai/a-gentle-introduction-to-decision-trees-3f85fb0c451e)是一种基于树的预测方法的分类算法。它们在机器学习的世界中是相当独特的，因为没有理由扩展功能，或者进行任何类型的标准化。

然而，由于在每个节点进行分割的性质，它们易于过度拟合，因此需要某种形式的正则化来降低复杂性。

让我们举个例子:假设 **N** 为数据点的数量， **k** 为特征的数量， **d** 为树的深度
时间复杂度学习决策树的单个节点是通过计算排序和不断更新杂质来完成的。
如果你每次都仔细检查并计算整个数据集的杂质，那就是。

> *O* ( *Nkd*

这意味着它实际上介于*O*(*Nk*log*N*)和*O*(*N*2*k*)之间。

正如你在上面的解释中所看到的，最坏情况下的性能可能会非常糟糕！因此，我们使用一些修剪技术来降低复杂性，从而减少过度拟合的机会

## 预修剪

当使用基于树的方法时，有一种更直接的方法来控制复杂性，这就是决策节点的数量。
一直细分到单个数据点的决策树最大程度上是过度拟合的。这可以直接控制(比如通过限制叶子的数量)，也可以通过设定最大深度来控制。这种想法(当不满足某些标准时，提前停止)被称为预修剪。

## 后期修剪

在这种情况下，我们从一个复杂的树开始，并向后修剪
最简单的方法是减少错误修剪:迭代叶节点，删除那些可以删除的节点，而不会损害保留验证集的预测准确性。

有许多修剪算法；下面是三个剪枝算法的例子。

# 信息增益剪枝

我们可以通过在修剪后和修剪前使用**信息增益**来修剪我们的决策树。在预剪枝中，我们检查特定节点的信息增益是否大于最小增益。在后期修剪中，我们用最少的信息增益来修剪子树，直到我们达到期望的叶子数量。

# 减少错误修剪(REP)

REP 属于修剪后类别。在 **REP** 中，在验证集的帮助下执行修剪。在 REP 中，以自下而上的方式评估所有节点以进行修剪。如果得到的修剪树在验证集上的表现不比原始树差，则节点被修剪。该节点处的子树被替换为分配了最常见类的叶节点。

# 成本复杂性修剪

成本复杂性修剪也属于后修剪类别。**成本复杂度修剪**通过基于子树的残差平方和(RSS)计算*树得分*和作为子树中叶子数量的函数的*树复杂度惩罚*来工作。树复杂性损失补偿了叶子数量的差异。从数字上看，树分数定义如下:

```
TreeScore=RSS+aTTree Score=RSS+aTTreeScore=RSS+aT
```

其中 a (alpha)是我们使用交叉验证找到的超参数，T 是子树中的叶子数。

**快乐修剪！**

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)