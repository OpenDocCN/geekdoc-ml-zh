# 马尔可夫网络和贝叶斯网络之间的转换

> 原文：<https://medium.com/mlearning-ai/conversion-between-markov-and-bayesian-networks-5402f707212a?source=collection_archive---------1----------------------->

在过去的几篇文章中，我们已经研究了关于[贝叶斯](/mlearning-ai/bayesian-networks-d-separation-e2a8f483b721)和[马尔可夫网络](/mlearning-ai/undirected-graphical-models-parameterization-of-markov-models-488c5359f5fd)的不同主题。我们已经看到了它们是如何各自代表独立约束，而另一个却不能。然而，在这篇文章中，我们想要检查从一个到另一个是否以及如何可能。

# 从贝叶斯网络到马尔可夫网络

我们可以从考虑如何从贝叶斯网络到马尔可夫网络开始。我们可以从记住贝叶斯网络是一个有向…