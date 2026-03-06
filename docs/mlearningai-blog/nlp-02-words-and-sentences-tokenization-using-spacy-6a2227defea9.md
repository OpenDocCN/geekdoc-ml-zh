# 使用空间的 NLP-02 单词和句子标记化

> 原文：<https://medium.com/mlearning-ai/nlp-02-words-and-sentences-tokenization-using-spacy-6a2227defea9?source=collection_archive---------2----------------------->

![](img/efd18148240aee83e3f5e79b9feb0b69.png)

Photo by [Brett Jordan](https://unsplash.com/@brett_jordan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在 spaCy 中，每个 NLP 应用程序都由几个处理文本的步骤组成。当我们在文本上调用“nlp”时，spaCy 应用一些处理步骤。

第一步是标记化以产生一个 **Doc** 对象。

Doc 对象然后用**标记器**、**解析器**和**实体识别器**进一步处理。这种处理文本的方式被称为语言…