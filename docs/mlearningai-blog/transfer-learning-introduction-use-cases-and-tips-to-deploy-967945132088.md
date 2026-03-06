# 迁移学习—简介、使用案例和部署技巧

> 原文：<https://medium.com/mlearning-ai/transfer-learning-introduction-use-cases-and-tips-to-deploy-967945132088?source=collection_archive---------8----------------------->

*作者* [*普拉卡什*](/@prakash_31206)*[*拉吉夫*](/@rajeev_76847)*2021 年 2 月 11 日**

*![](img/8f7ca0f74db6ce1c4b4b334dd5f3e56f.png)*

*[Brain](https://www.maxpixel.net/tag/Brain) [Network](https://www.maxpixel.net/tag/Network) [Artificial Intelligence](https://www.maxpixel.net/tag/Artificial-Intelligence) [Technology](https://www.maxpixel.net/tag/Technology)*

*迁移学习是一种公认的黑客技术，可以用有限的数据集和少得多的计算来构建高性能的深度学习模型。众所周知，深度学习可以产生很好的结果，但它也需要庞大的数据集和大计算能力。即使深度学习是解决问题的最佳工具，大数据集可用性和/或高计算成本也可能是重要的限制因素。在这种情况下，迁移学习可能会有所帮助。*

*迁移学习允许我们在解决相同/相似领域内的另一个问题时获得知识，并将其重新用于解决我们当前的问题。向一个 Java 程序员教授 python 编程应该比向一个没有任何软件编程经验的人教授相对容易。这是一个解释迁移学习如何使我们的深度学习工作变得更容易的恰当比喻。*

# ***用例***

*今天，迁移学习被大量用于解决机器视觉和自然语言处理(NLP)相关的深度学习问题。*

*我仍然记得我是多么惊讶地看到来自 [fast.ai](https://www.fast.ai/) 课程(我几年前学的)的第一个笔记本，它解决了一个猫狗图像分类问题，非常准确，使用了一个小数据集，不到 2 分钟的训练！猜不到奖励——它对一个预先训练好的 ResNet 模型使用了迁移学习。*

*[GPT3](https://en.wikipedia.org/wiki/GPT-3) 大概是最先进的 NLP 服务(不是模型！)曾经建造过。它的 1750 亿个机器学习参数使它能够做“疯狂、具体和相关的事情”，满足你的需求，比如写儿童故事或创建 HTML 网站。你要做的就是，用几个例子来教它。GPT3 的创造者 Openai 将底层模型保密，并通过 API 提供给合作伙伴。最有可能的是，它使用某种转移学习来为每个客户定制模型。*

***“智能即服务”的兴起***

*思考一下“智能即服务”这个术语。这是什么意思？一个有思想的人在解决你的具体问题，听起来是对的。这是否意味着一个活生生的人是将学习从他们的大脑转移到你的用例的先决条件？如果智能被打包在机器学习模型中，它会变得不那么智能吗？只要学习的转移可以有效地解决给定的问题，那么它是来自机器学习模型还是来自活生生的人就应该很重要。*

*我们认为，在未来十年，机器学习作为一种服务将成为 SaaS 领域中一个独特的子群，在人类活动的不同领域有着广泛的应用。在这种情况下，GPT3 apis 似乎指明了前进的方向。如今市场上有很多类似的产品，比如针对流程自动化的 [indico](https://indico.io/) ，针对客户服务自动化的 [ultimate.ai](https://www.ultimate.ai/) 。大多数此类服务公开[披露](https://www.ultimate.ai/blog/ai-automation/transfer-learning-in-customer-service-automation)利用迁移学习。从核心模型开始，迁移学习用于使用特定客户的数据为该客户训练定制模型。随着此类产品的扩展，从以前的培训中获得的经验将使迁移学习对未来的模式更加有效。*

# ***迁移学习—部署生产流水线***

*由于受益于迁移学习的用例不同，在生产中部署迁移学习管道时必须特别小心。概括地说，迁移学习项目可以分为两大类*

***线下培训***

*这是为单个实体开发模型的典型情况。一个组织可能使用迁移学习来开发一个供内部使用的模型，或者一个人工智能顾问可能正在为它的一个客户建立一个模型。通常，这种情况下的工作流程是:*

**构建模型并开始在生产中使用。一旦它的性能下降，重新训练模型并重复。**

*对于这样的场景，一个很好的选择是在像 [AWS Sagemaker](https://aws.amazon.com/sagemaker/) 、 [Azure ML](https://azure.microsoft.com/en-in/free/machine-learning/) 这样的工具上建立训练这样的模型的工作流。这些工具通常使用它们的 SDK apis 来调用。这些调用可以以不同的方式触发——由工程团队按需触发(手动或通过自动化脚本)。*

***在线培训***

*这是使用每个客户的数据集为不同的客户动态生成模型的情况。“智能即服务”机器学习模型就属于这一类。这需要将完整的迁移学习培训管道公开为一个端点，只要客户提供新数据，就可以以编程方式调用该端点。在这些情况下，将您的迁移学习培训代码转换成无服务器的云功能是一种干净、简单且好的方法。无服务器云函数为您提供了巨大的可扩展性和最优的成本，因为您只需为您的函数运行时付费。*

*[AWS Lambda](https://aws.amazon.com/lambda/) 、 [Google Cloud Functions](https://cloud.google.com/functions) 、 [Azure Functions](https://azure.microsoft.com/en-in/services/functions/) 是最流行的无服务器云框架。但是，这是一个很大的但是，大多数这些框架都没有考虑到深度学习培训。他们施加了许多限制，如没有 GPU 支持，只有 15 分钟的执行时间，50MB 的包大小等。AWS lambda 的这些限制的表格可以在[这里](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-limits.html)找到。*

*[Clouderizer Showcase](https://clouderizer.com/) 有助于克服这些限制和挑战。它是在牢记深度学习训练和评分的基础上构建的。它允许将您的 Jupyter 笔记本电脑(无需任何代码更改)直接部署为无服务器云功能。GPU 功能、长时间运行的任务是其他一些突出的功能，这使得 Clouderizer 成为将您的 transfer learning 培训管道部署为无服务器云功能端点的良好选择。你可以[注册一个免费账户](https://showcase.clouderizer.com)试试看。如果您有任何问题，欢迎致电[info@clouderizer.com](mailto:info@clouderizer.com)联系我们*