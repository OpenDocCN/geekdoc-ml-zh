# 回归梯度提升的直观和数学介绍

> 原文：<https://medium.com/mlearning-ai/an-intuitive-and-mathematical-introduction-to-gradient-boost-for-regression-c19a921f2455?source=collection_archive---------3----------------------->

在本文中，我们将介绍梯度推进算法，专门用于回归。顾名思义，它是一个[提升算法](/mlearning-ai/a-theoretical-introduction-to-boosting-in-machine-learning-240f7ed4d714)，因此假设读者知道这意味着什么。在某些方面，梯度增强类似于 [AdaBoost 算法](/mlearning-ai/a-theoretical-introduction-to-adaboost-3e8d53f37a25)。因此，它可以帮助读者了解梯度增强也熟悉 AdaBoost。

# 重温 AdaBoost 和决策树桩