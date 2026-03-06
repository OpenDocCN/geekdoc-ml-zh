# 停止使用肘方法来计算 k-均值聚类中的最佳聚类

> 原文：<https://medium.com/mlearning-ai/stop-using-the-elbow-method-to-compute-optimal-clusters-in-k-means-clustering-1f572c587d2e?source=collection_archive---------0----------------------->

## 轮廓分析基本指南

![](img/e8599681141f60de6a44b5b9202f1755.png)

Image by [Michal Jarmoluk](https://pixabay.com/users/jarmoluk-143740/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=561388) from [Pixabay](https://pixabay.com/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=561388)

聚类是一种机器学习技术，指的是对未标记的数据集进行分组。k-Means 是一种流行的聚类算法，它以这样一种方式对数据集进行分组或聚类，即同一聚类中的数据点彼此相似，而同一聚类中的数据点彼此相似。