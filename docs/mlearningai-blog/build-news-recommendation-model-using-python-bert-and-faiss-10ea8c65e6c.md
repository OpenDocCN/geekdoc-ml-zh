# 利用 Python、BERT 和 FAISS 构建新闻推荐模型

> 原文：<https://medium.com/mlearning-ai/build-news-recommendation-model-using-python-bert-and-faiss-10ea8c65e6c?source=collection_archive---------0----------------------->

![](img/fdd29c74db2349211bf2d67711c142ef.png)

Photo by [Filip Mishevski](https://unsplash.com/@filipthedesigner?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在这个故事中，我将展示如何建立基于主题的新闻推荐模型，我们将使用微软新闻数据集(MIND)进行实验。

# 提议的解决方案

提议的解决方案是基于内容级别的推荐，这意味着我不会考虑协作推荐(读者谁…