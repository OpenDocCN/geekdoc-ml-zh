# Pytorch & C++ #2:创建模型和数据集

> 原文：<https://medium.com/mlearning-ai/pytorch-c-2-creating-models-and-datasets-bf8c58ee40b1?source=collection_archive---------3----------------------->

![](img/2df193ef8fcb85dc3308b62b246c4add.png)

# 介绍

在本系列中，我将尝试用 Pytorch c++ API 提供示例/实践/项目。如果您是初次接触这个系列，潜水前您可以查看第一个博客。在这个故事中，我们将在 cpp 中练习神经网络模块。所有代码均可在本 Github repo 的[中找到。](https://github.com/EmreOzkose/pytorch_cpp)

# 内容

1.  创建模型
2.  创建自定义数据集(用于[房价数据集](https://www.kaggle.com/competitions/house-prices-advanced-regression-techniques/data))

# 创建模型

我们可以定义一个基本的线性层堆栈，如下所示。`Struct`用于构建模型类。

我们还可以定义一个卷积神经网络。我们应该给层定义一个选项对象，它与 Python 不同。

在编写了一个自定义模型之后，我们也可以定义如下。

让我们为 CNN 运行一个随机输入的基本推断。

模型也可以用预定义的函数初始化。让我们用泽维尔初始化来初始化我们的 CNN 模型。

这很容易，就像在 python 中一样。让我们为房价数据集创建一个自定义数据集，并使用模型(尚未训练)对其进行测试。

# 创建自定义数据集

我们将使用[房价数据集](https://www.kaggle.com/competitions/house-prices-advanced-regression-techniques/data)的一个子集，其中仅使用了*地段面积*和*建成年份*特征。当然，以 *sales_price* 列为目标。我创建了一个 CSV 文件，它只包含上面提到的 3 列，没有标题。此文件看起来像:

```
8450,2003,208500
9600,1976,181500
11250,2001,223500
9550,1915,140000
14260,2000,250000
14115,1993,143000
...
```

其中第一栏为*地段 _ 面积*，第二栏为*建成 _ 年*，第三栏为*销售 _ 价格*。样本数量是 1460。让我们阅读 CSV 文件。这不是本教程目标的重要部分，但我们应该阅读它:。

读取数据集后，我们获得一个包含要素行的矢量对象。我们可以使用以下方式打印所有样本:

现在，我们可以定义如下的自定义数据集。

现在，让我们用我们的 CNN 模型(还没有经过训练)来测试这些定制数据。

预测值 3351.2444 与目标值 208500 相差甚远，但正如我所说，CNN 模型还没有经过训练。我们将在下面的博客中训练一个房价预测模型，并用 JIT 进行测试。

# 结论

在这个博客中，我们用 Pytorch cpp API 练习了 nn 模块。所有代码都可以在[这个 Github Repo](https://github.com/EmreOzkose/pytorch_cpp) 中找到。我们将在本系列的以下博客中继续 torch::nn。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)