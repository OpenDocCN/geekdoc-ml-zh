# 机器学习的标记数据集与未标记数据集——有什么区别？

> 原文：<https://medium.com/mlearning-ai/labeled-vs-unlabeled-datasets-for-machine-learning-whats-the-difference-e5fce0e68621?source=collection_archive---------9----------------------->

## 人工智能(AI)现在是商业的重要组成部分。机器学习(ML)的使用有助于企业主和制造商通过预测性维护减少流程驱动的损失、增加销售和降低费用。在这里，我们想解释正确的机器学习数据集如何帮助组织使用预测分析。

![](img/89d3c2fde240c19ddc318d008dccd084.png)

Photo by [Pop & Zebra](https://unsplash.com/@popnzebra?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/label?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

ML 通过分析模型的建立自动化数据分析。ML 是人工智能的一个分支，它使用系统来分析数据，识别数据中的模式，并在几乎没有人类干预的情况下做出决策。ML 允许软件应用程序更加精确，ML 中的算法使用各种类型的数据来预测新的值。

# 标记和未标记数据集

ML 需要使用数据，有两种数据集:

*有标签的和无标签的。*

未标记数据集是自然或人造项目的样本。未标记的数据可能包括照片图像、音频和视频记录、文章、推文、医学扫描或新闻。这些项目没有标签或说明；它们仅仅是数据。

与此同时，带标签的数据集使用人类的判断来分类一块未标记的数据。这些标签取决于需要解决的问题，因此，如果一家企业想要预测客户的行为，它就会使用客户的信息来尝试预测消费者是否会完成交易。

# 机器学习模型以及它们如何使用数据

ML 模型可以帮助企业解决诸如预测市场价格等问题，但这完全取决于需要解决的任务。ML 建模使用三个主要组来完成这项工作:

*   监督学习，
*   无监督学习和
*   强化学习。

# 监督学习(使用带标签的数据集)

[监督学习在算法中使用带标签的数据集](https://www.ibm.com/cloud/learn/supervised-learning)对数据进行分类并预测结果。如今，监督学习被用于大多数软件应用程序中；这些包括文本处理和图像识别。它还帮助公司解决现实世界中的问题，例如对 tope 线索进行分类，客户即将取消你的服务，以及在你的电子邮件收件箱中分离垃圾邮件。

![](img/13323eb2639d9ace369705cd3e6ec046.png)

Image by the author — labeled dataset for lead scoring

监督学习中使用的标记数据集将标签添加到观察结果中，这些标签来自该领域的专家或专家的观察结果。这些标记的数据集然后通过分类和回归算法进行预测分析。

当基于给定数据预测类别标签时，使用监督学习中的分类算法。例如，该模型预测或分类客户是否会取消，或者数据是否是摩托车。

[分类预测一个状态](https://www.sciencedirect.com/topics/engineering/classification-algorithm)，比如回答一个肯定或者否定的结果。一些模型使用更大的状态集，其中可能包括以下真实情况:

*   根据使用的词语或给出的评级，对企业、电影或服务的负面或正面评论进行分类；
*   [根据历史行为分类销售线索是否会转换](https://graphite-note.com/how-to-create-a-lead-scoring-model-with-graphite)
*   [根据历史行为对客户是否会取消服务进行分类](https://graphite-note.com/effective-customer-churn-prediction)
*   基于过去的网站交互和用户人口统计预测用户是否会点击链接；
*   基于共同朋友、人口统计、兴趣和用户历史的列表，对社交媒体用户是否会与其他用户成为朋友或互动进行分类。

监督机器学习中使用的另一种方法是回归分析。回归预测数字，如价格、收入、成本等。

回归建模可以根据数据的特征预测数字。回归可用于以下实际情况，以确定:

*   基于位置、房屋面积、房间数量和设施的住房市场价格；
*   某人将在某个产品上花费的金额——基于购买行为和交易历史；
*   患者的预期寿命，使用基于症状、健康史和医疗记录的数据。

# 无监督学习(使用未标记的数据集)

[无监督学习使用无标签数据集](https://www.techtarget.com/searchenterpriseai/definition/unsupervised-learning)和更困难的算法。由于数据集没有被标记，并且收集的信息非常少，因此结果或预测也没有被标记。然而，在无监督学习中使用的未标记数据集仍然可以揭示有用的信息。例如，模型仍然可以通知用户数据是否相似，并且可以仅根据相似性对数据进行分组。

![](img/37077c12788d023a828187fb1dab77d6.png)

Image by the Author: Clustering in Graphite Note

无监督机器学习是处理未标记数据集的分支。无监督学习有两种类型:

*   聚类和
*   降维。

聚类根据相似性对数据进行分组。例如，如果数据中有一组随机动物的图像，它可能会根据图像中动物的数量或动物的颜色对数据进行分组。

下面是一些真实的例子，说明如何在商业中使用聚类:

*   房地产公司可以根据位置、价格或房间数量分割房产；
*   基于购买历史的客户细分
*   基于购买历史的产品细分
*   企业可以根据发件人、主题行中使用的单词或邮件中是否有附件或链接来对未标记的电子邮件进行分类。

[降维简化了数据](https://www.geeksforgeeks.org/dimensionality-reduction/)，用很少的特征来描述。它只关注最重要的特征，并去除使数据集过于复杂的噪声。用于数据的维度越少，模型中的参数就越少。这为模型提供了更大的空间来容纳新数据，新数据可以用于可视化和作为训练模型。

# 强化学习

强化学习不使用数据，而是使用环境和代理来实现环境中的特定目标。然后，环境提供可能的结果，无论是“奖励”还是“惩罚”，然后在未来的情况下指导代理人完成其目标。强化学习侧重于在探索和代理人利用当前知识开发环境之间找到平衡。

强化学习的一个真实例子是预测股票价格。在这个领域使用强化学习允许代理人购买、出售或持有股票。他们还可以评估市场状况，在最有利的时候采取正确的行动。

人工智能可以通过多种方式帮助组织实现目标。收集数据很重要，但这还不够。企业和公司必须能够整理这些数据，为他们的决策提供信息，无论是提高他们的绩效还是提供他们的目标市场正在寻找的服务。

正确的数据集可以帮助组织使用预测分析，并帮助他们做出更具战略性的决策。

*原载于 2022 年 4 月 30 日*[*【https://graphite-note.com】*](https://graphite-note.com/labeled-vs-unlabeled-dataset-for-machine-learning)*。*

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)