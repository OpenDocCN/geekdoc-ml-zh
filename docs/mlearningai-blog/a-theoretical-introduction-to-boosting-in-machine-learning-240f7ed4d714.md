# 机器学习中的助推技术简介

> 原文：<https://medium.com/mlearning-ai/a-theoretical-introduction-to-boosting-in-machine-learning-240f7ed4d714?source=collection_archive---------9----------------------->

在本文中，我们将介绍机器学习中的“助推”概念。我们将试图从直觉和数学上理解这个概念。该理论将作为下两篇文章的基础，我们将讨论两种 boosting 算法:AdaBoost 和 XGBoost。

## 弱学习性和强学习性

我们将首先介绍弱学习者和强学习者的概念，因为 boosting 使用弱学习者的集合。那么，到底什么是弱学习者呢？弱学习者简直…