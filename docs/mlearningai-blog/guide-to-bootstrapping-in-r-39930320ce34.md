# 如何在 R 中引导数据

> 原文：<https://medium.com/mlearning-ai/guide-to-bootstrapping-in-r-39930320ce34?source=collection_archive---------6----------------------->

![](img/27e25fba8e2128696d9e020872a816bd.png)

Photo by [Carlos Muza](https://unsplash.com/@kmuza?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

**什么是自举？**

Bootstrapping 是一种对单个数据集进行重采样以创建许多重采样样本的方法，允许我们应用通常需要正态假设的统计方法。自举的一个关键特点是重采样总是通过替换来完成。这让我们可以创建置信区间，计算标准误差，并使用假设检验。