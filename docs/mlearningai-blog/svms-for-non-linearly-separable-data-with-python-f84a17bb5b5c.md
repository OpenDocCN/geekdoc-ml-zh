# 用 Python 实现非线性可分数据的支持向量机

> 原文：<https://medium.com/mlearning-ai/svms-for-non-linearly-separable-data-with-python-f84a17bb5b5c?source=collection_archive---------2----------------------->

在我们的前几篇文章中，我们谈到了支持向量机。我们已经考虑了它们的[硬](/mlearning-ai/an-introduction-to-hard-margin-support-vector-machines-5f8539a116b1)和[软](https://najamogeltoft.medium.com/an-introduction-to-soft-margin-support-vector-machines-fec8420981af)余量，以及当我们的数据不是线性可分时，我们如何使用[内核技巧](/mlearning-ai/understanding-the-kernel-trick-in-machine-learning-part-2-a65e280cb7cf)。我们也看到了当数据是线性可分的时，我们如何用 python 实现一个 SVM，有硬边界或软边界。然而，在本文中，我们将只考虑当我们的数据不是线性可分时如何实现 SVM。。我们将使用 Jupyter 笔记本和各种库来实现我们的模型。

## 定义我们的数据