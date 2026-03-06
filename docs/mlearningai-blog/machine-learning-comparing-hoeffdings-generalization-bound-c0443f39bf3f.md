# 利用 KL 散度放松 Hoeffding 不等式

> 原文：<https://medium.com/mlearning-ai/machine-learning-comparing-hoeffdings-generalization-bound-c0443f39bf3f?source=collection_archive---------3----------------------->

在前一系列文章中，我们讨论了很多关于在机器学习环境中学习的可能性。具体来说，我们讨论了如何确保我们能够从一个假设类中选择一个[假设，并确保它能够很好地推广](/mlearning-ai/machine-learning-and-generalization-error-is-learning-possible-cff8721285e0)。换句话说，我们如何衡量我们的假设在看不见的数据上的表现？我们发现我们可以调整[赫夫丁的不等式](https://najamogeltoft.medium.com/machine-learning-the-intuition-of-hoeffdings-inequality-970a59c2b519)，这样它就可以在关于从一个假设类中找到最佳假设的设置中工作。在这篇文章中，我们不关心如何选择…