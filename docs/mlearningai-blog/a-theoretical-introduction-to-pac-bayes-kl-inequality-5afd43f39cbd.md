# 机器学习中 PAC-Bayes KL 不等式的理论介绍

> 原文：<https://medium.com/mlearning-ai/a-theoretical-introduction-to-pac-bayes-kl-inequality-5afd43f39cbd?source=collection_archive---------0----------------------->

在上一篇文章中，我们看了一下[奥卡姆剃刀](/mlearning-ai/an-introduction-to-occams-razor-bound-in-machine-learning-80ba5456c8dc)——它背后的直觉以及它如何应用于机器学习。在本文中，我们将看看 PAC-Bayesian 分析，在这里我们也需要使用 [KL 不等式](https://najamogeltoft.medium.com/machine-learning-comparing-hoeffdings-generalization-bound-c0443f39bf3f)。因此，我们将定义一个称为 PAC-Bayes KL 不等式的推广界。

## PAC-贝叶斯分析的动机

让我们先理解为什么我们会对 PAC-Bayesian 分析感兴趣，然后再深入研究…