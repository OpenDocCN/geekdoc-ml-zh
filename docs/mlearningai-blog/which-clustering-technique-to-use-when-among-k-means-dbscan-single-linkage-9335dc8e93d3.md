# 在 K-means，DBScan，Single-Linkage 中使用哪种聚类技术

> 原文：<https://medium.com/mlearning-ai/which-clustering-technique-to-use-when-among-k-means-dbscan-single-linkage-9335dc8e93d3?source=collection_archive---------11----------------------->

当有不同的技术用于同一目的时，选择正确的技术是一个相当大的挑战。因此，让我们简单地讨论一下何时使用各种聚类技术。

K-means: K-means 是一种广泛流行的聚类技术，这是有原因的。
*这是一种**快速**聚类技术。当在大型数据集上实现时，与其他技术相比，性能变化很大。
*但是，您必须为您的数据集找到正确的 k 值。
*对于大型数据集和最佳执行时间，使用 K-means

**DBScan:** DBScan 是基于密度的。当根据密度识别集群时，这是正确的选择。但是，如果在更近的距离上有不同的密度呢？然后，我们可以通过使用分级 DBScan 来解决这个问题。恒星组成的星系。
*其执行时间逐渐增长(y=x)，与数据集的大小无关。

**单链接:**这种技术适用于小数据集。
*对于大型数据集，该技术在计算上变得非常昂贵。因为人们应该手动合并集群。
*这样做的缺点是可能会创建微小的集群，有时很难合并集群。此外，可视化会对更多的集群和大型数据集造成困扰。

演职员表:斯蒂芬·赫博尔德(我的教授。)

干杯！
快乐学习💚

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)