# 多类与多标签分类:有什么区别？

> 原文：<https://medium.com/mlearning-ai/multi-class-vs-multi-label-classification-whats-the-difference-2840a87c8dfe?source=collection_archive---------4----------------------->

![](img/6d6e19ddf89c62a6065f51a35c3487bb.png)

Photo by [Markus Winkler](https://unsplash.com/es/@markuswinkler?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

机器学习是一个复杂的话题，理解所有涉及的概念可能是一项挑战。初学者最困惑的一个方面就是多类和多标签分类的区别。这篇博文将解释这两个概念之间的区别，并帮助您理解哪一个适合您的数据。

# 多类分类

在我们假设的世界里只有三种动物:猫、狗或小鸡。我们有许多动物的图片，我们想根据动物把它们分成三个不同的类别。

如果我们能正确识别每张图片中的动物，我们就能把它归入正确的类别。例如，如果一张图片包含一只猫，我们会把它放入“猫”类别。然而，如果一张图片包含一只狗，我们会把它归入“狗”类别。同样，如果一张图片包含一只小鸡，我们会把它放入“小鸡”类别。通过对每张图片中的动物进行正确分类，我们可以帮助确保每种动物都被归入正确的类别。

正如我刚才向你介绍的，这个问题是一个多类分类问题。我们有样本(图片)，每个样本属于一个类。我们假设一张图片要么是猫，要么是狗，要么是小鸡，而不是它们的组合！

![](img/6655f4ad37af36a0e1c10e94f5d7361f.png)

Photo by [kevin turcios](https://unsplash.com/@kevin_turcios?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 多标签分类

假设我们现在有一个不同的问题。我们想对动物的图片进行分类，但是这次每个图片中可以有不止一个动物！

例如，一张图片可能同时包含一只猫和一只狗。我们会把图像分为“猫”和“狗”两类。类似地，一张图片可能包含一只猫、一只狗和一只小鸡。在这种情况下，我们会将图像放入“猫”类别、“狗”类别和“小鸡”类别。

这个问题是一个多标签分类问题。我们有样本(图像)，每个样本可以属于多个类。我们假设一张图片可以包含动物的任意组合！

# 摘要

那么，多类和多标签分类有什么区别呢？在多类分类中，每个样本属于且仅属于一个类。相比之下，在多标签分类中，每个样本可以属于多个类别。

感谢阅读:)

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)