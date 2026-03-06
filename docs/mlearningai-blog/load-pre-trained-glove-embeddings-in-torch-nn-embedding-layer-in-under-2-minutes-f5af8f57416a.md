# 将预先训练好的手套嵌入物装入 torch.nn.Embedding layer…不到两分钟！🏎🔥

> 原文：<https://medium.com/mlearning-ai/load-pre-trained-glove-embeddings-in-torch-nn-embedding-layer-in-under-2-minutes-f5af8f57416a?source=collection_archive---------0----------------------->

## 一个直接从[其官方项目页面](https://nlp.stanford.edu/projects/glove/)获取的将预先训练好的手套单词嵌入到`torch.nn.Embedding`层的实用教程

![](img/19895d9a062b02fa5e18309df84c22eb.png)

Photo by [Traf](https://unsplash.com/@traf?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/fast?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

**步骤 1:** 下载嵌入

选择适合您的嵌入。我选择[维基百科 2014 + Gigaword 5](http://nlp.stanford.edu/data/glove.6B.zip) 变种。

您可以在 Jupyter 环境(例如 Google Colab)中执行这些代码。如果不可用，请在 shell 中单独执行这些操作。

**步骤 2:** 解析词汇表和嵌入

我将加载 50 维嵌入。您也可以从 100、200 或 300 维选项中选择。

1.  `vocab`:单词列表，将在文本到令牌 id 转换步骤中有用
2.  `embeddings`:嵌入列表，这将用于初始化嵌入层

**步骤 3:** 将词汇表和嵌入转换成 numpy 数组

1.  `vocab_npa`:numpy 形状数组(vocab 大小，)
2.  `embs_npa`:形状的 numpy 数组(vocab 大小，嵌入维数)

**步骤 4:** 将*填充*和*未知*令牌添加到`vocab`和`embeddings`数组中

我选择了:(1)填充符标记嵌入为零，以及(2)未知的*标记嵌入作为所有其他嵌入的平均值。*

**步骤 5:** 使用下载的嵌入初始化嵌入层

**第六步(可选):**保存`vocab_npa`和`embs_npa`数组，以后可以轻松加载。

## **下一步**

太好了！现在你知道如何使用手套嵌入的任何变体来初始化你的嵌入层。

通常，在接下来的步骤中，您需要:

1.  定义一个`torch.nn.Module`来设计自己的模型。该模型将包含本教程中初始化的`torch.nn.Embedding`层。
2.  定义一个接受文本样本的`torch.utils.data.Dataset`,并将其转换成`torch.nn.Embedding`层可以理解的形式。

我将在接下来的文章中介绍这两个步骤！我还在我的一个 github 项目的笔记本中讲述了这些步骤。请随意查看！