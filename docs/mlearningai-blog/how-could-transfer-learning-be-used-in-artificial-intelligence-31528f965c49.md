# 迁移学习如何用于人工智能？

> 原文：<https://medium.com/mlearning-ai/how-could-transfer-learning-be-used-in-artificial-intelligence-31528f965c49?source=collection_archive---------4----------------------->

![](img/d4f1e512f583d75e272058a42126cb9a.png)

Photo by [Austin Distel](https://unsplash.com/@austindistel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

机器学习和人工智能正在许多行业中使用，并且它们的使用分别随着系统性能的提高而不断增长。机器学习的一些有趣应用是在自动驾驶汽车和制药行业。随着人工智能和机器学习的使用，每天都有很多最新的有趣应用程序被创建出来。

![](img/944ec7d863f1fd0f20c6a77dd1514652.png)

Photo by [Nick Fewings](https://unsplash.com/@jannerboy62?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

人工智能有一个子集叫做深度学习，其中有复杂的神经元单元，可以在需要时执行计算。需要注意的一点是，当执行这些计算时，在图像分类任务的情况下，在对图像中的某组对象进行分类的过程中，模型将优化它们的权重和偏差。这些权重和偏差在决定图像中存在的不同对象集合时会有影响。换句话说，它们将分别考虑对图像中的对象进行分类很重要的特征。

当将图像分类为不同的对象时，这些权重和偏差集将用于许多其他任务。当执行卷积神经网络操作时，初始层中的权重通常会检测图像中各种边缘的存在。随着我们不断深入网络，由这些权重和前一层的输出执行的卷积将检测到与我们的任务相关的更具体的特征。

每当我们开始一个计算机视觉任务并执行模型的训练时，权重和偏差将被随机初始化。如果将其他任务的权重和偏差作为当前网络的初始权重，并在以后针对我们的任务进行优化，这将是一种更好的方法。因此，这将提高计算的速度和模型根据我们的要求对对象进行分类的准确性。

尽管我们在诸如视频和音频的格式中发现了大量的数据，但是仍然不可能找到特定于该任务的一些图像的大量数据。当执行迁移学习操作时，深度学习算法的开发者减少了所有的时间和精力。这可能会在未来分别执行机器学习和深度学习任务时导致更好的结果。

因此，我们已经看到了迁移学习如何用于图像分类任务，其中已经训练的权重和偏差是从以前的模型中提取的，并为我们的深度学习任务进行初始化。这将减少以合理的精度构建模型实际所需的总时间。希望这篇文章对你有用。请随意分享你的想法。谢谢！