# SERP 数据在集成学习中的优势

> 原文：<https://medium.com/mlearning-ai/advantages-of-serp-data-in-ensemble-learning-1ae0d7abb14d?source=collection_archive---------3----------------------->

![](img/3dcc099ef1ad100bcbd285964722f63f.png)

[自动图像分类生成器](https://github.com/serpapi/automatic-images-classifier-generator)允许你对机器学习模型进行自动化数据收集、自动化训练和测试。该工具由 [SerpApi 的 Google Images Scraper API](https://serpapi.com/images-results) 提供支持，绕过了手动数据输入的需要，并通过在其工作流程中提供预处理功能来减少人为错误。

上周，我们讨论了如何使用之前创建的数据集自动捕获机器学习模型的训练和测试过程的数据，并使用 Python 制作一个自动化脚本来收集关于最佳机器学习模型的信息。

本周我们将讨论如何组合多个同类的二进制分类算法，在我们的例子中是 CNN，以创建一个基本的集成学习模型，以及 SERP 数据在集成学习中的优势。

关于这个工具的状态，它是如何创建的，我们使用它的目的是什么，你可以滚动到页面的底部。

# Python 中的集成学习是什么？

集成学习是指在其预测模型中包含至少两种机器学习算法的机器学习模型。集成模型的关键权衡是用不同的神经网络交换 boosting 算法的复杂性。Python 有各种工具、框架和库，专门用于数据科学和机器学习，以创建高级集成学习模型。
你如何用 Python 创建合奏？

有很多专门用于集成学习的库，比如 xgboost。也存在机器学习框架的本地方法，如 scikit-learn 的决策树分类器、BaggingClassifier 或 GradientBoostingRegressor。以后真的很想进入他们。能够仅仅通过调用类似于:
`from sklearn.ensemble import VotingClassifier`或
`from sklearn.ensemble import RandomForestClassifier`或
`from sklearn.model_selection import train_test_split`或
`from sklearn.ensemble import AdaBoostClassifier`
的一行程序来创建集成学习模型，或者简单地用:
`from sklearn.linear_model import LogisticRegression`
调用先前创建的逻辑回归模型，看起来是一种创建具有稀疏训练数据的子集的非常简单的方法，并且使用集成学习技术来涵盖分类问题上的不同模型。然而，我想用简单的软件工程技术来解决这个问题。我们想把它变成一个依赖性更小的客户端工具。因此，如果还能满足目的的话，做定制工作是明智的。

# 什么是集成学习？请举例说明？

在我们开始之前，我想提醒读者，我并不精通集成学习的术语。有不同种类的集成方法和集成模型，如 bagging、bootstrap aggregation、梯度推进决策树等。我真的不知道我对通过分别训练算法并获得最频繁的答案来提升算法的解决方案的主张是什么。我不是数据科学家，但我的最佳猜测是硬投票。

在本教程中，我们将使用二元分类训练 CNN 的多个个体模型，让它们逐个在类标签上进行预测，并收集这些中的每一个，以将多数投票设置为答案。在未来几周，如果我们的二元分类器过度拟合，我们可能会通过考虑二元模型训练的图像数量、验证准确性等来创建某种软投票系统。处理回归问题以创建概率模型。但是我们需要来自这个模型的度量和可靠的分类器，以便将它转换成回归模型。

简单来说，如果你有 3 个美国犬种作为类别标签，分别是`American Hairless Terrier`、`American Malamute`和`American Eskimo Dog`，你将需要 3 个二元分类器:

`american_hairless_terrier_vs_alaskan_malamute`
`alaskan_malamute_vs_american_eskimo_dog`
`alaskan_malamute_vs_american_eskimo_dog`

然后，我们将在这 3 个单独的模型上运行图像，并获得最频繁的答案作为预测。

# 集成学习的优势是什么？

集成学习通过将强学习器模型与弱学习器进行对比来减少强学习器模型的任意坏预测。此外，强大的学习模型可以结合起来，以实现更好的性能。换句话说，集成学习可以用来解决单个机器学习模型无法实现的问题。

想象一下，我们有 3 个分类器的类别标签，这个单一模型的测试集给出了 51%的准确率。这可以被认定为弱学习者。比方说，如果你在两个标签之间进行二元分类，每个模型的准确率大约为 65%。这里的假设是，如果我们组合这些模型中每一个的预测，它们的加权平均值应该高于 51%，这高于单个基础学习者可以达到的水平。你可以把它想象成`max_features = 2`，和`max_depth = 1`决策树。

这里的另一个优点是，在某些情况下，当您有多个模型时，更容易控制数据点。例如，如果作为单一模型的组合的新模型具有一个过度拟合的 CNN，则它可以容易地用新的训练数据集或新的算法重新训练，然后被替换。也许，你甚至可以改变分类的方法，用线性回归、KNN、SVM 或 SVC 等来代替那部分。毕竟，最终目标是对预测进行交叉验证。

# SERP 数据在集成学习中有什么优势？

SERP 数据可用于为机器学习目的创建专门的训练和测试数据集。可以通过按大小、规格、来源等过滤目标结果来减少噪声。对于集成学习，可以用 SERP 数据优化单个机器学习模型，以更好地服务于组合模型。

在我们的例子中，我们能够创建一个数据集，该数据集在我们的单个模型中具有训练项目(x_train)和测试项目(x_test)，这些模型仅由`American Malamute Dog`、`American Eskimo Dog`等组成。，只有正方形的图像才更容易控制内核大小。

例如，您可以从 Kaggle 等地方获取数据集，导入 pandas，并通过在标签文件上使用 read_csv 方法来选择您想要的图像，以专门化您的数据集。但是，使用 SERP 数据，您可以收集这些数据，而不需要浏览带有标签的长 CSV 文件，也不需要使用任何额外的库。您可以简单地指定一次搜索，然后收集您想要的所有数据。

使用 SerpApi 的图像抓取器 Api，如 [SerpApi 的谷歌图像抓取器 API](https://serpapi.com/images-results) 、 [SerpApi 的 Yandex 图像 API](https://serpapi.com/yandex-images-api) 或 [Naver 图像 API](https://serpapi.com/naver-images-api) ，您可以在[游乐场](https://serpapi.com/playground)中通过简单的查询创建专门的图像数据集。

您也可以使用其他形式的数据进行机器学习。请访问[使用 SERP 数据构建机器学习模型页面](https://serpapi.com/use-cases/machine-learning-and-artificial-intelligence)以获得更好的效果。

[立即向 SerpApi 注册，申领免费积分。](https://serpapi.com/playground)

# 集成学习的一个例子

我们使用公式来计算 CNN 中第一个全连接层的输入大小:

再次提醒，这是如果输入是一个正方形图像。

让我们定义一个简单的 CNN，它有两个卷积:

这绝不是一个完全适用的模型。但它会帮助我们构建必要的部分。这个模型的准确率会在%35 到%65 左右。对我们来说不太好。

让我们挑选必要的零件:

这里的 Output size 表示最后一个卷积层的输出大小，image_size 是图像的宽度和高度。

让我们定义我们的标签:

让我们使用标签索引来创建每个可能的唯一配对:

对于那些想知道这个 index_list 将有多少项的人，公式是:
`n_estimators = label_size!/((label_size-2)!*2!)`其中`n_estimators`表示唯一配对(label_combinations)的大小。

让我们开始迭代我们想要在其上建立模型的两个标签，并以此命名模型:

对于每个周期，标签看起来像这样:
`alaskan_malamute_vs_american_eskimo_dog`

让我们计算第一个完全连接的层输入大小:

让我们在每个周期创建训练词典，并将它们添加到列表中:

现在，为了将来的目的，让我们检查已经在特定二元分类上训练的模型:

让我们逐个训练二元分类模型:

注意，如果模型存在，我们跳过训练过程。

此时，我们在`examples`文件夹中有一个名为`malamute_example.jpg`的图像。让我们将其塑造成模型可以识别的形式:

现在我们有了作为张量的图像，让我们逐个调用每个二元分类模型，并将预测收集到一个列表中:

请注意，由于我们对标签张量使用了一个热点向量，因此我们可以获取预测张量中最大值的索引，然后使用它从标签列表中获得答案。

最后，让我们找到最频繁的预测，并通过字符串操作将其作为最终答案:

以下是当您运行之前已经训练好的模型时的输出:

这一预测奏效了。但这并不是长期有效的好指标。CNN 的模式还得用上周的剧本进一步调整，然后换到这里。

[完整代码](https://gist.github.com/kagermanov27/e05391af78d8fdece11e114d8edefa52)

**结论**

我感谢读者的关注，也感谢塞尔帕皮的[杰出人士的支持。在未来几周，我们将讨论如何优化单个二进制分类模型，如何将所有这些模型存储在一个文件中，以及如何最小化训练过程，使其成为命令行工具。](https://serpapi.com/team)

*原载于 2022 年 9 月 8 日*[*https://serpapi.com*](https://serpapi.com/blog/advantages-of-serp-data-in-ensemble-learning/)*。*

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)