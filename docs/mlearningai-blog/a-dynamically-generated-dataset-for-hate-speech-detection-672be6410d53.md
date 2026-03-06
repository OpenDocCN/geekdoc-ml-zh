# 用于仇恨言论检测的动态生成数据集

> 原文：<https://medium.com/mlearning-ai/a-dynamically-generated-dataset-for-hate-speech-detection-672be6410d53?source=collection_archive---------4----------------------->

![](img/03f56f72ea8e8eba91169bbee51faf4c.png)

Photo by [Guillaume de Germain](https://unsplash.com/@guillaumedegermain?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在本帖中，我们描述了一个为训练和改进仇恨言论检测模型而动态生成的数据集。一组训练有素的注释者生成并标记了具有挑战性的例子，这样仇恨言论模型就可以被欺骗，从而得到改进。这个数据集包含大约 40，000 个例子，其中 54%被标记为仇恨言论。它也是仇恨言论的目标，包括弱势群体、边缘化群体和受歧视群体。总的来说，这是一个平衡的数据集，这使得它不同于你在网上已经可以找到的仇恨言论数据集。

这个数据集出现在 2021 年发表的文章[从最坏的情况中学习:动态生成的数据集，以改善在线仇恨检测](https://aclanthology.org/2021.acl-long.132.pdf)中。本文描述了生成和注释数据的过程。此外，它还描述了他们如何使用生成的数据来训练和改进仇恨言论检测模型。

大多数可用的仇恨言论数据集并不平衡(即它们包含的仇恨言论实例较少)。由于内容节制，很难从社交网络和在线论坛收集仇恨言论的例子。因此，本文的作者做了很大的努力来动态生成一个数据集，该数据集对于检测仇恨言论的模型来说是平衡的和具有挑战性的。

# 数据集是如何生成的

数据集是经过四轮生成的。它包含大约 40，000 个条目，其中 54%被标记为仇恨言论。每个仇恨言论条目不仅如此标注，而且还标注了目标群体。

在每一轮中，他们执行以下过程:

1.  使用上一轮生成的数据集训练分类模型。在第一轮中，他们收集了几个已经可以在网上找到的仇恨言论数据集。
2.  标注器创建新的训练示例，这些示例可以欺骗上一步中训练的模型。目标是误导模型，使其更好地检测仇恨言论。
3.  使用所有数据(步骤 1 中使用的数据和步骤 2 中生成的数据)训练新模型，并评估其性能。

在最后一轮中，注释者从现实世界的在线论坛中获得了仇恨言论的例子。他们为模型寻找具有挑战性的内容，并根据这些内容写出例子。

# 优点和缺点

以动态方式创建数据集带来了一些好处，例如拥有更具挑战性的示例，使我们能够训练更好的模型。数据问题可以在此过程中解决。例如，您可以细化生成仇恨言论的有效示例的准则，以提高模型的有效性。此外，这个过程生成了一个平衡的数据集，这使得它在训练模型时非常有用。然而，这是一个昂贵的过程，需要仇恨言论专家和其他专业人员的领域专业知识，以便这些例子不会有偏见。

# 资源

*   **原文:** [从最坏的情况中学习:动态生成数据集以改进在线仇恨检测](https://aclanthology.org/2021.acl-long.132.pdf)。
*   **作者的** [**GitHub 知识库**](https://github.com/bvidgen/Dynamically-Generated-Hate-Speech-Dataset) 中有模型的数据和代码。
*   Kaggle 中的 [**数据集。**](https://www.kaggle.com/datasets/usharengaraju/dynamically-generated-hate-speech-dataset)

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)