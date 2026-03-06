# AdaBoost 简介

> 原文：<https://medium.com/mlearning-ai/a-theoretical-introduction-to-adaboost-3e8d53f37a25?source=collection_archive---------6----------------------->

在本文中，我们将继续讨论 boosting，我们在下面的[文章](https://najamogeltoft.medium.com/a-theoretical-introduction-to-boosting-in-machine-learning-240f7ed4d714)中已经介绍过了。在本文中，我们将研究 boosting 中的一个特定算法:AdaBoost。我们将试图从直觉和数学上理解它是如何工作的，在另一篇文章中，我们将研究它在 Python 中的实现。

# 重温助推直觉

在介绍 AdaBoost 之前，让我们先快速回顾一下升压的直觉。boosting 背后的想法是权衡用于训练一组…