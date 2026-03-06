# 人工智能是反人类的。等等，什么？

> 原文：<https://medium.com/mlearning-ai/artificial-intelligence-is-against-humanity-wait-what-c1a8bc934146?source=collection_archive---------7----------------------->

我们总是听到机器人或机器对抗人类的故事。你知道这在现实生活中真的发生过吗？要找到答案，请阅读这篇文章。

![](img/85dc2215111275f8e8e537d089f486ab.png)

Figure 1\. A representative of a complex Artificial Neural Network from [unsplash.com](https://unsplash.com/), showing the chaos of not knowing what is happening

我们总是听说机器人或一段代码变得对人类不利的故事，但很少有人告诉我这在现实生活中发生过。从黑盒人工智能模型开始流行的那一天起，它们的使用在行业中变得流行，人工智能决策过程背后的基本原理对用户来说变得不透明。

## 那么，什么是黑箱人工智能模型呢？

黑盒人工智能模型是深度神经网络之类的模型，其中有许多调整过的参数来做出决定。使用大量的参数来做出决策，很难理解模型是基于什么原理做出简单的决策。

## 为什么黑箱模型可以被认为是反人类的？

众所周知，大多数黑盒方法可以在人类无法解决的问题上实现高性能。如前所述，它们在许多问题上可以超越人类，但找到它们做出这些决定背后的原因并不是一件容易的事情。在行业中使用决策时，不了解决策的原因或基本原理会导致不同的问题。例如，认为如果模型的决策是错误的，没有简单的方法找出原因。

## 他们是如何变得反对人类的？

在过去的十年中，使用这些方法在人工智能系统中产生了巨大的问题，例如:

苹果信用卡背后的算法被指责有性别偏见

*   苹果公司的信用卡联合创始人史蒂夫·沃兹尼亚克的限额是他妻子的十倍，尽管账户交易完全相同(对女性有偏见)[1]

2-亚马逊的招聘人员人工智能系统确实更喜欢男性而不是女性被雇佣[2]

3- AI 在预测未来罪犯时对黑人有偏见[3]

*   预测白人和黑人未来犯罪风险的巨大差异

这些都是反人类的人工智能系统的例子。因此，为了解决这一问题，引入了“可解释人工智能(XAI)”的新概念。

## 可解释的人工智能(XAI)

经典的黑盒人工智能方法(特别是深度学习方法)，通常不会解释决策背后的基本原理。为了找到黑盒人工智能方法决策背后的原因或其过程背后的基本原理，引入了新的方法来解释经典的人工智能方法。

## 这些解释方法是什么？

提出了不同的方法来解释黑箱模型的决策(局部解释)或揭示整个决策过程(全局解释)。

我们只是简单总结一下这些方法是什么。要了解更多，有大量的书籍、博客和视频深入讨论了这些方法的功能。

**1-局部可解释的模型不可知解释(LIME):** 这种方法试图使用来自原始模型的一组数据来学习透明模型。透明模型可由人类模拟和分解，并且其算法也易于人类理解。LIME 方法输出用于数据样本决策的特征重要性。(【https://github.com/marcotcr/lime】T2

**2- SHAP:** 这种方法通过尝试平均不同的地层和特征预测的可用性，发现特征的重要性与石灰相同。([https://github.com/slundberg/shap](https://github.com/slundberg/shap))

**三层式相关性传播(LRP)、集成梯度(IG)和深度提升:**这三种方法试图通过分析神经网络中的神经元来发现特征重要性。([https://github.com/albermax/innvestigate](https://github.com/albermax/innvestigate))

**4-累积局部效应(ALE)图:**可视化特征对输出的影响。([https://github.com/DanaJomar/PyALE](https://github.com/DanaJomar/PyALE))

此外，还有更多可用的 XAI 方法。我会留下一些可以在人工智能系统中使用的 XAI 工具箱的链接。

**in investigation**:调查神经网络预测的工具箱。([https://github.com/albermax/innvestigate](https://github.com/albermax/innvestigate))

**Quantus:** Quantus 是一个可解释的 AI 工具包，用于负责任地评估神经网络解释([https://github . com/understand-machine-intelligence-lab/Quantus](https://github.com/understandable-machine-intelligence-lab/Quantus))

**OpenXAI:** 对模型解释进行透明评估([https://github.com/AI4LIFE-GROUP/OpenXAI](https://github.com/AI4LIFE-GROUP/OpenXAI))

## 最后的话

为了正确地对抗人工智能系统，我们需要比它们更好，超越它们，但如今对真正智能的人工智能系统的需求是至关重要的。从自动手术到自动判断系统，人工智能系统必须非常智能，但也必须值得信赖——能够通过了解正在发生的事情来依赖它的决策——对人类来说。为了实现模型的高性能和可信性，有一个解决方案是使用 XAI 方法。

值得注意的是，这些 XAI 方法正在不断发展，需要科学家和开发人员给予更多关注，以评估这些方法并将其用于工业。因此，为了建立一个更好的社区，超越这些人工智能系统，我们需要探索并对它们进行更多的研究。我将鼓励计算机科学家对这些方法进行更多的工作和研究，也鼓励开发者在他们的系统中使用 XAI 方法来建设一个更美好的世界。

## 参考

[1]达菲，克莱尔。2019."苹果联合创始人史蒂夫·沃兹尼亚克称苹果卡歧视他的妻子。" *CNN 商业*。

[2]杰弗里·达斯廷。2018."亚马逊废弃了显示对女性有偏见的秘密人工智能招聘工具."*路透社*

[3]全国各地都有用来预测未来罪犯的软件。而且对黑人有偏见。Julia Angwin、Jeff Larson、Surya Mattu 和 Lauren Kirchner，ProPublica 2016

**想多读书？试用这些资源**

[4]“可解释的机器学习:使黑盒模型可解释的指南”，Christoph Molnar，leanpub，2022 年

[5]应用机器学习的可解释技术:使用莱姆、SHAP 等，使 ML 模型在实际应用中可解释和可信，Aditya Bhattacharya 著，2022 年

[6]“xxAI-超越可解释的人工智能”，Andreas Holzinger，Randy Goebel，Ruth Fong，Taesup Moon，Klaus-Robert Müller，Wojciech Samek 国际研讨会，结合 ICML 2020 举办

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)