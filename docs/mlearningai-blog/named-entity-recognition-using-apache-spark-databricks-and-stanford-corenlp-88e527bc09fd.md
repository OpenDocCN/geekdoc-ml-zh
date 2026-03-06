# 使用 Apache Spark、Databricks 和 Stanford CoreNLP 进行命名实体识别

> 原文：<https://medium.com/mlearning-ai/named-entity-recognition-using-apache-spark-databricks-and-stanford-corenlp-88e527bc09fd?source=collection_archive---------1----------------------->

在本文中，我将介绍我如何使用 Stanford CoreNLP 库在 Databricks 中使用 Apache spark (Scala)构建命名实体识别器。

![](img/68e3d9191954c5f40184344593e2003b.png)

Photo by [Murat Onder](https://unsplash.com/@muratodr?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 什么是命名实体识别？

命名实体识别(NER)旨在识别和分类人名、地名、组织名等