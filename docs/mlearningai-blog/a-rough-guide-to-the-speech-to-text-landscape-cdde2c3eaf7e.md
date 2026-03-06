# 语音到文本领域的粗略指南

> 原文：<https://medium.com/mlearning-ai/a-rough-guide-to-the-speech-to-text-landscape-cdde2c3eaf7e?source=collection_archive---------1----------------------->

## 谁在构建自动化转录服务？

![](img/ce94942fdfef045f7da48ffd10afb7d0.png)

Photo by [Jason Leung](https://unsplash.com/@ninjason?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/speech?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

语音到文本(STT)——也称为自动语音识别(ASR)——是一项快速发展的技术，它推动了虚拟助手、自动字幕、笔记等应用的发展。但是，对于那些想利用最新的语音转文本技术开发自己产品的公司来说，有什么选择呢？

将 STT 集成到您自己的产品中有三种主要选择:大型科技公司的云服务、专业语音技术公司，以及构建您自己的技术。要判断哪一个是最好的，通常意味着对一些数据进行测试，这些数据代表了您希望在产品中看到的数据，并将不同的产品与您自己的预算和要求相匹配。

## 大型技术云产品

许多大型科技公司都将 STT 服务作为其云服务的一部分。[亚马逊](https://aws.amazon.com/transcribe/)、[谷歌](https://cloud.google.com/speech-to-text)、[微软](https://azure.microsoft.com/en-us/services/cognitive-services/speech-to-text/)、 [IBM](https://www.ibm.com/uk-en/cloud/watson-speech-to-text) 和[百度](https://intl.cloud.baidu.com/product/speech.html)都有以 API 形式提供的服务，而[苹果](https://developer.apple.com/documentation/speech)的 STT 可以通过其开发者程序获得。这些 API 可以集成到应用程序和其他产品中，以实时流模式工作，或批量上传音频。

有一些有限的定制选项可用，比如定制单词表和发音来处理特定于您的应用程序的词汇。不同的语言是可用的，但是对于 API 背后运行的模型通常没有多少选择。这些服务的优势在于它们易于集成，并且在主流用例中表现良好。它们可以是一种快速而简单的开始方式，因为你不必投入时间和精力来构建，只需用信用卡注册一个帐户，然后自己尝试。

价格是公开的。对于谷歌没有数据记录的标准型号，价格是每 15 秒音频 0.6 美分。亚马逊每月抄写 25 万分钟的价格是每分钟 2.4 美分，量大的话更便宜。从这些价格可以看出，云 STT 服务的目标是呼叫中心或自动字幕等高容量应用。

## 专业语音公司

除了大型科技公司，还有许多中小型企业提供一系列不同的 STT 产品。这些产品都有不同的定价、卖点和特点，因此很难简单概括。值得更深入地检查它们，看看哪一个符合你的目标。一些例子是:

*   [Otter](https://otter.ai/) —丰富的会议、采访笔记&其他对话
*   [钴](https://www.cobaltspeech.com/) —定制的语音技术解决方案
*   [Speechmatics](https://www.speechmatics.com/) —精确且包容的语音识别引擎
*   [Picovoice](https://picovoice.ai/) —边缘设备上的语音技术
*   [Rev](https://www.rev.com/) / [Temi](https://www.temi.com/) —均为手动&跨行业自动转录
*   [Verbit](https://verbit.ai/) —专为法律和教育产品定制
*   [深度图](https://deepgram.com/) —定制 ASR 建模
*   [描述](https://www.descript.com/) —创作者的音视频编辑
*   [SoundHound](https://www.soundhound.com/) —打造定制语音助手和技术
*   肥皂箱实验室——专门为孩子开发语音技术

另一家致力于语音技术的大公司是 Nuance，它成立于 1992 年，长期以来在市场上占有重要地位。2019 年，他们的汽车业务被分拆成一家名为 [Cerence](https://www.cerence.com/) 的独立公司，而 Nuance 随后在 2021 年被微软收购。

## 内部开发

第三种，也是最耗时的一种选择是在内部开发自己的技术。如果现有的解决方案不能满足您的需求，这可能是您的选择。也许你正在为一种不能很好地为之服务的语言而构建，为一个云 API 背后的通用模型不够准确的特定领域而构建，或者为一个不能依赖云连接的设备而构建。

你需要建立一个语音识别系统的成分是大量的数据来训练它，用于训练的计算能力，以及一个或多个机器学习专家来建立和调整模型。值得注意的是，这些技能很难招聘到，而且工资通常很高，因为对机器学习专家的需求很大。

有许多开源的语音工具包可以帮助您入门，并省去完全从头开始的工作。Kaldi 是其中最著名和最常用的一种，但对于语音技术领域的新手来说，它确实有一个陡峭的学习曲线。另一个，最近宣布的，是[言语大脑](https://arxiv.org/pdf/2106.04624.pdf)。对于学习其他可用的资源， [OpenSLR](http://www.openslr.org) 有一个很好的列表。

您可能会花费几周或几个月的时间来构建和部署自己的技术，然后需要投资来维护它。然而，结果将是一个更灵活的系统，调整到您的要求，并深入集成到您的产品。

语音技术的前景一直在变化，随着机器学习研究社区开发出新的想法，新的公司和产品不断出现。这篇文章并不是详尽的总结，而是对当前行业的一个简短概括，供任何对构建语音产品感兴趣的人参考。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)