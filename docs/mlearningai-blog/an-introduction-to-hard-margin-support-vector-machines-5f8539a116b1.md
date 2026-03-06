# 硬间隔支持向量机简介

> 原文：<https://medium.com/mlearning-ai/an-introduction-to-hard-margin-support-vector-machines-5f8539a116b1?source=collection_archive---------3----------------------->

在本文中，我们将讨论硬间隔支持向量机。我们将讨论线性和非线性 SVM。因为我们需要考虑非线性 SVM 情形下的内核，所以先阅读下面的文章可能对你有用:[理解内核技巧](/mlearning-ai/understanding-the-kernel-trick-in-machine-learning-part-2-a65e280cb7cf)。我们还将看到支持向量机是如何成为凸学习问题的，这意味着你可能也想阅读下面这篇关于[凸学习问题](/mlearning-ai/an-introduction-to-optimization-for-convex-learning-problems-in-machine-learning-df7fd6453652)的文章。

## 记住线性分类