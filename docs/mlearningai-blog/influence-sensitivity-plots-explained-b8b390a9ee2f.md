# 影响灵敏度图解释:了解特征影响

> 原文：<https://medium.com/mlearning-ai/influence-sensitivity-plots-explained-b8b390a9ee2f?source=collection_archive---------7----------------------->

## 了解如何使用影响敏感度图及其优势！

![](img/81fb50fbb594c8b7b0d62cddf7bc6b96.png)

Image from Unsplash.

本文由 Jisoo Lee 合著。

这个世界正在以指数的速度产生信息，但这可能会带来更多的噪音，或者变得过于昂贵。有了这些数据，模型变得[有用](https://www.lacan.upc.edu/admoreWeb/2018/05/all-models-are-wrong-but-some-are-useful-george-e-p-box/)越来越具有挑战性，即使有效的分析[成为每个企业](https://www.forbes.com/sites/forbestechcouncil/2020/02/14/why-every-company-is-a-data-company/?sh=189ca0a217a4)模型的核心。一个好的模型性能度量标准的选择可以为建模者提供一个他们预测静态数据集结果的能力的现实视图，但是我们周围的世界比以往任何时候都变化得更快。可接受的性能现在可能会以意想不到的方式快速下降，影响灵敏度图是一种有用的工具，可以帮助我们了解和确定我们可以提高模型准确性的领域，不仅是现在，而且是未来。

# 量化输入影响驱动独立销售员的可解释性

可解释性(有时被称为可解释性)是模型质量的基本支柱。可解释性方法通常要么解释模型中的一般趋势(全局解释)，要么解释个体预测的行为(局部解释)。Shapley 值是一种通用工具，有助于将函数的输出归因于其输入，并广泛用于解释 ML 模型做出某种预测的原因。达塔等人为此提出了一个总体框架，称为[量化输入影响(QII)](https://truera.com/wp-content/uploads/2020/08/Quantitative-Input-Influence-2016.pdf) 。

作为复习，这篇关于[Shapley value-based feature influences](https://towardsdatascience.com/the-shapley-value-for-ml-models-f1100bff78d1)的博客解释了 Shapley values 如何采用博弈论方法来捕捉每个特性对模型分数的边际贡献。但是，由于 Shapley 值所具有的特殊属性，一个预测的要素的 Shapley 值与另一个预测的 Shapley 值处于相同的范围内，并且可以直接进行比较。这一特性使得它在聚合单个预测的影响以创建模型行为的全局视图方面非常强大。

一种方法是影响敏感度图(ISP)，它将特征值映射到每个要素的要素影响。这个视图提供了对人工智能模型如何在内部响应某个特征以进行预测的洞察(Shapley values 的 [SHAP](https://arxiv.org/abs/1705.07874) 开源实现也提供了这种可视化的一个版本，称为依赖图)。在这篇文章中，我们将深入探讨什么是影响敏感度图，以及从这个重要的人工智能解释工具中你可以收集到的关于你的模型的各种见解。

# 可解释性的相似可视化方法

在高层次上，我们希望隔离单个特征的影响及其在模型决策中的作用。为了直观地理解这种效应，我们可以考虑各种各样的工具，包括[部分相关性图](https://towardsdatascience.com/finding-and-visualising-non-linear-relationships-4ecd63a43e7e)(PDP)[累积局部效应(ALE)](https://christophm.github.io/interpretable-ml-book/ale.html) 图，以及影响敏感度图(ISP)。这篇文章将谈到 ISP 如何克服 PDP 和 ALEs 面临的一些挑战，并阐述 ISP 可以为您的模型改进工作流带来的价值。

## 部分相关图

通过构建，PDP 改变一个特性，同时独立地随机化其余特性，以便计算给定特性的边际效应。举个简单的例子，这可能会扭曲模型的解释，考虑一个只有三个特征的汽车保险损失预测模型:年龄、每月行驶的英里数和过去的事故数。为了构建过去事故数量的部分相关图，我们将调整其值为过去事故的所有可能值，同时随机化其他特征(在这种情况下，年龄和每月行驶的英里数)。对数据中的每个观察值重复此过程后，下一步是对过去事故和绘图的每个值的观察值进行平均。不幸的是，我们刚刚计算的特征估计包括现实中不太可能的配对(16 岁的新司机，过去有事故)，因此以这种方式有偏差。

[QII 值](https://towardsdatascience.com/the-shapley-value-for-ml-models-f1100bff78d1)也打破了相关性，以了解一个功能的边际影响。然而，他们不是一次做一个特性，而是通过平均特性对其他特性组合的边际影响来考虑特性交互。

## 累积局部效应图

虽然 ALE 图解决了特征独立性的问题，但是它们遇到了不同的障碍。根据定义，ALEs 在局部计算特征扰动的影响，因此不能跨多个区间进行可靠的解释。对于选择本地间隔大小，也没有公认的最佳实践。当选择太多的间隔时，ALEs 可能不稳定(变化很大)，但如果选择太少，可能会很快失去准确性。为麦芽酒寻找最适宜的区域既费时又容易出错。因为 ISP 本质上显示的是数据集中每个点的影响，所以不存在解释可视化所需的邻域概念。

ISP 通过 PDP 和 ALE 图等替代方法解决了许多问题。独立销售员背后的 QII 值说明了给定点和比较组之间的输出差异有多少是由该特征造成的。这种方法是克服上述障碍的关键——我们可以使用 ISP 以不偏不倚的方式理解功能影响，即使是交互功能，我们甚至可以从全局角度解释这些影响。

# 什么是影响敏感度图？

![](img/ae8f653df5a274d10531c6464807cf25.png)

*Influence Sensitivity Plot displaying global and local trends of feature influence. Image from TruEra Diagnostics.*

与 SHAP 的[依赖图](https://shap.readthedocs.io/en/latest/example_notebooks/tabular_examples/tree_based_models/Census%20income%20classification%20with%20LightGBM.html#SHAP-Dependence-Plots)类似，影响敏感度图(ISP)绘制了数据中真实观察值的特征值与 QII 值的关系，以了解特定特征对最终模型得分的影响。以这种方式绘制，我们可以很快获得特征影响的全球趋势的感觉。是积极的还是消极的？单调吗？是线性的吗？

在特征值的给定切片内，我们还可以了解该切片的影响范围。如果范围很窄，我们可以说特征值在孤立时具有更强的影响，而沿 Y 轴的离差是特征交互的良好指示。最后，我们可以单独检查观察结果，以了解被 PDP 隐藏的局部影响，并深入了解跨整个数据集的特征影响的异质性。

![](img/acef0cbf1cda86611bfb3f5254ab1698.png)

*Categorical Influence Sensitivity Plot for a binary feature. Image from TruEra Diagnostics.*

ISP 也可用于分类特征。上面是一个简单的例子，显示了二元变量的特征影响。我们不仅可以观察到影响的差异，还可以观察到这两个值之间范围的差异，这提供了我们能够为连续特征收集的相同洞察力。

# 用于细分市场分析和模型比较的分组独立销售员

像许多常见的可视化技术一样，我们可以使用一种称为分组 ISP 的技术来区分显示的点子集，以传达附加信息。这通常会发现不同的影响模式——对 ISP 进行分组的两种常见方法是对数据进行切片(分段),或者在第二个模型中分层。

![](img/c5725d04bf216d20f2ca52ff5af72d17.png)

*Segmenting based on certain features can often highlight how other features influence the outcome differently. The green points indicate a sub-100k annual income group. Image from TruEra Diagnostics.*

理解数据集中有意义的子集中的要素行为与理解整个数据集中的要素行为同样重要。有不同的方法来识别这些有意义的子集，主题专家通常在头脑中有他们想要关注的子集(或“片段”)。由于不同细分市场之间的性能往往存在差异，因此了解模型如何对待不同的细分市场至关重要，而分组的 ISP 对于比较这些细分市场的特性影响尤其有效。

![](img/2377bdb6b18cd5ec8debc81603a97872.png)

*ISP for comparing different feature values’ influence predictions in different models. Here a logistic regression (green) model and a boosted tree model (yellow) are compared. Image from TruEra Diagnostics.*

分组的 ISP 对于比较不同模型的特性影响也是非常有效的。因为我们可以观察到*单个观察值*的影响，所以除了模型分数如何受整体特征影响的变化之外，我们还可以比较不同模型在处理特定观察值时的差异。

# 用随附的图表丰富 ISP

![](img/1e873cdc806ab29d0ce8d60038535262.png)

*ISP along with an accompanying histogram displaying feature value occurrences on the X-axis and a kernel influence density chart on theY-axis. Image from TruEra Diagnostics.*

影响密度和数据值出现的图表也可以伴随 ISP，以丰富我们对给定特性的理解。这种结合为我们带来了一个动态的三人组来理解我们的模型，确保概念的合理性并进行质量改进！

*如果你有兴趣了解更多关于如何解释、调试和改进你的 ML 模型…*

*   *注册即将到来的* [*人工智能精品课程*](https://go.truera.com/ai-quality-workshop?utm_campaign=workshop-ai-quality-2022-aug&utm_medium=blog&utm_source=tds)
*   *加入*[*AI 品质论坛 Slack 社区*](https://aiqualityforum.slack.com/join/shared_invite/zt-1db75juer-ULg3HInTXjxn9sgnVpisYA#/shared-invite/email)
*   [*观看本讲座*](https://www.youtube.com/watch?v=6KFlcCRja0c) *关于可解释性如何成为提高模型质量和理解的支柱。*

*本文原载于*[*https://truera.com/influence-sensitivity-plots-explained/*](https://truera.com/influence-sensitivity-plots-explained/)

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)