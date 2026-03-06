# 当我 5 岁的时候教我 RANSAC 算法🤖

> 原文：<https://medium.com/mlearning-ai/teach-me-the-ransac-algorithm-like-im-5-d3b1dd6d67a8?source=collection_archive---------5----------------------->

> 当我第一次听说 **OpenCV** 中的 **RANSAC** 算法时，我正在上计算机视觉课，起初我不太明白，尽管老师尽了最大努力，但我想我有点笨😅。因此，我写这篇文章的原因是为了让那些仍在努力解决这个问题的人可以深呼吸一下新鲜空气😊。

**RANSAC** 代表随机样本一致性(Random Sample Consensus)，一种用于处理受离群值影响的数据的有效算法。

**RANSAC** 能够通过将数据点分成两个独立的类别来处理这些类型的数据:

-内侧集合和
-外侧集合

通过这样做， **RANSAC** 消除了您处理异常值的顾虑。离群值被简单地忽略了，现在您只需要处理内层数据点。

让我们看看通过 2D 数据点拟合的回归线。

！[图片说明]([https://www . research gate . net/profile/Mohammad _ Adib _ Khairuddin/publication/317841563/figure/fig 2/AS:512573764505600 @ 1499218516778/mge 13-Linear-Regression-Ln-scatter plot-with-Linear-Regression-Line-of-Ln-Likes-L-I . png](https://www.researchgate.net/profile/Mohammad_Adib_Khairuddin/publication/317841563/figure/fig2/AS:512573764505600@1499218516778/MGE13-Linear-Regression-Ln-Scatterplot-with-Linear-Regression-Line-of-Ln-Likes-L-i.png))

您可能会注意到，一些数据点落在我们的拟合线上或接近拟合线，而另一些则落在外面。现在的问题是，我们如何将内联者与外联者分开。你看到 RANSAC 算法在哪里了吗？😉😜

**RANSAC** 算法迭代地寻找具有最高内联数的最佳拟合。选择具有最高内层数量的拟合点，这就是 **RANSAC** 的基本内容。

**结论** :
随机样本一致性基本上是一个处理异常值的 4 路程序。

-对数据点的一个小子集进行采样，并将其视为内点
-获取潜在的内点，并计算模型参数
-记录支持拟合的数据点数量。
-重复并取最佳拟合分数。😁💃

**RANSAC** 可在图像分析期间应用，通过分析相关摄像机图像确定机器人的位置和方向。

## 我的名字叫 AC Nice，我喜欢计算机视觉🤖

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)