# 使用 ML 的假新闻检测 101

> 原文：<https://medium.com/mlearning-ai/fake-news-detection-101-using-ml-5c5d2631bdb2?source=collection_archive---------5----------------------->

*使用机器学习和深度学习方法检测假新闻的概述和指南，*基于[我同一主题的论文](https://www.researchgate.net/publication/355763923_Study_of_Fake_News_Detection_using_Machine_Learning_and_Deep_Learning_Classification_Methods)。

![](img/1e056c58c6094b59005d4ca8c9325a48.png)

Image via ABGI UK

# 介绍

社交媒体的世界在不断扩大，随之而来的是自由创建、分发和认可任何不需要在现实生活中实际存在的信息的能力。这就是所谓的假新闻。

世界上存在如此多的假新闻，以至于人工核实事实几乎是不可能的。因此，我们开始依赖我们的老朋友**机器学习**。

机器学习模型具有快速可靠地检测假新闻的能力，通常无需繁琐的人工验证。它们已经成为检测假新闻的流行工具。

我计划带你通过更简单和常见的机器学习和 NLP 方法来完成这项任务。

> 欲了解更多深入信息，请点击此处阅读我的全文。

# 我们可以使用哪些数据？

先说**数据集**。

正如我之前提到的，假新闻无处不在。这也是机器学习领域的一个热门话题。因此，存在许多预先存在的标记数据集来帮助我们完成任务。

让我们考虑以下四个流行的数据集:

1.  [coda lab](https://link.springer.com/chapter/10.1007/978-3-030-73696-5_3)——来自推特、脸书等社交媒体网站的新冠肺炎相关假新闻。
2.  [恢复](https://dl.acm.org/doi/10.1145/3340531.3412880) —来自 Twitter 的新闻文章。
3.  [真假新闻(FARN)](https://www.kaggle.com/clmentbisaillon/fake-and-real-news-dataset) —来自路透社等各种知名机构的新闻文章。
4.  各种来源的新闻标题。

每个数据集都有一个共同的关键特征:它们只有两个标签(类):**假新闻(0)或真新闻(1)** 。

> 更多深入的信息，请在这里阅读我的论文全文。

# 这个数据怎么细化？

先说**预处理**。

到目前为止，我们的数据集包含两个有用的列:

1.  文本——真实的新闻文章或标题本身，我们必须对其进行真假分类
2.  标签——一个数字(0，1 ),告诉我们附带的文字实际上是假新闻还是真新闻。

首先，在查看各种文章和标题时，我们可以观察到它们包含许多我们不需要用于机器学习的东西，即标点符号、哈希标签、数字等等。因此，我们可以简单地使用 [regex](https://regex101.com/) 或任何其他工具删除这些不需要的字符。

其次，每篇文章都包含许多无用的词，这些词没有带来关于新闻的真假的信息。常见的例子有“是”、“是”、“上午”、“到”等。这些被称为[停用词](https://byteiota.com/stopwords/)，并被删除。

最后，我们有很多来自同一个家族的词有着相同的含义(比如“跑”、“奔跑”、“奔跑”)。这些单词需要被标准化，以便它们不会被我们的机器学习模型视为不同的单词。于是，我们申请了一个[搬运工斯特梅尔](https://tartarus.org/martin/PorterStemmer/)。

我们的文本现在准备好进行下一步。

> 欲了解更多深入信息，请点击此处阅读我的全文。

# 我们如何利用文本进行机器学习？

先说**特征提取**。

现在，我们有了一个精炼的、基于波特词干的文本语料库，准备好被推入一个奇特的模型以产生一个结果。*但是我们的机器学习模型到底是如何理解文本的*？

他们不知道。这些模型理解数字。因此，我们需要将我们的文本转换成某种数字表示，其中包含每篇文章的特性(列)。这可以通过多种方式实现。我们将使用以下三种方法:

1.  [单词包](https://machinelearningmastery.com/gentleintroduction-bag-words-model/) —文本被描绘成其单词的集合(或包)，不考虑单词的顺序。它关注每篇文章中常见单词或术语(或 n-gram)的频率。
2.  [TF-IDF](http://tfidf.com/) —文本被描绘为被判断为对语料库最重要的单词的集合。
3.  [Word2Vec](http://arxiv.org/abs/1402.3722) —使用单词嵌入将文章表示为多维向量空间。

使用这些算法，我们将文本文章转换成具有许多特征的向量，这些向量现在可以用于我们的模型。

> 要了解更多的信息，请在这里阅读我的全文。

# 如何才能对假新闻做出预测？

我们来谈谈**机器学习分类器**。

恭喜你，你已经通过了预处理，特征提取和其他“必要的邪恶”。我们终于到了最好的部分- *使用最大似然算法预测某事*。

首先，我们需要为**训练**分配一些数据，为**测试**分配一些数据，这样我们就可以恰当地评估我们的模型的熟练程度。我们将数据分成 2:1 的比例，分别用于训练和测试。然后，我们将使用以下使用 [SK-Learn](https://scikit-learn.org/stable/index.html) 的机器学习分类器:

1.  [多项式朴素贝叶斯](https://www.upgrad.com/blog/multinomial-naive-bayes-explained/)(带和不带调整)
2.  [高斯朴素贝叶斯](https://towardsdatascience.com/gaussian-naive-bayes-4d2895d139a)
3.  [被动-主动](https://www.bonaccorso.eu/2017/10/06/ml-algorithms-addendum-passive-aggressive-algorithms/)
4.  [随机森林](https://massivefile.com/randomforestclassifier/)
5.  [逻辑回归](https://www.analyticsvidhya.com/blog/2015/11/beginners-guide-on-logistic-regression-in-r/)
6.  [多层感知器](https://www.techopedia.com/definition/20879/multilayer-perceptron-mlp)
7.  [支持向量机](https://www.edureka.co/blog/support-vector-machine-in-python/)

这些 ML 分类器中的每一个都将与每个数据集上的每个特征提取方法一起使用。我们最终总共有 **80 种组合**。

> 更多深入的信息，请在这里阅读我的全文。

# 我们对假新闻的预测有多准确？

先说**评价指标**。

我们准备了 **80 种假新闻检测组合**。我们如何比较它们来知道哪一个最适合我们的任务？

为了评估我们的模型，我们将使用以下[评估指标](https://www.analyticsvidhya.com/blog/2021/07/metrics-to-evaluate-your-classification-model-to-take-the-right-decisions/):

1.  准确(性)
2.  精确
3.  回忆
4.  f1-分数

通过这些指标，我们可以看到我们的模型正在工作，而且工作得很好！这给我们留下了巨大的 **320 个值**，我们可以用它们来比较分类器和组合！

> 欲了解更多深入信息，请点击此处阅读我的全文。

# 结论

我们从这件事中学到了什么？

我们学习了预处理、特征提取、分类和评估的技术，以便使用 ML 检测假新闻。

此外，我们已经生成了如此多的数据，我们可以相互比较分类器和组合，以获得更多的信息。例如，我们可以得出结论，随机森林和多层感知器一般在所有组合中表现最好。此外，我们可以观察到来自 Kaggle 的 FARN 数据集在所有指标上都给出了最高分。

> 这篇文章是关于这个主题的综述。[完整的信息，包括详细的方法、解释、结果和分析，可以在我的论文中找到。](https://www.researchgate.net/publication/355763923_Study_of_Fake_News_Detection_using_Machine_Learning_and_Deep_Learning_Classification_Methods)读一读吧！

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)