# 如何在你的关键词研究中使用监督模型的机器学习来提高你的搜索引擎优化🚀

> 原文：<https://medium.com/mlearning-ai/how-to-use-machine-learning-with-a-supervised-model-in-your-keyword-research-to-boost-your-seo-43d09860d851?source=collection_archive---------1----------------------->

![](img/5042059dedff3fa9cbd8a7172a43ee38.png)

自年以来，我们一直使用关键字工具，如 [Ahrefs](https://ahrefs.com/) 、 [Semrush](https://www.semrush.com/) 、 [Google keyword planner](https://ads.google.com/intl/en_uk/home/tools/keyword-planner/) 、 [Spyfu](https://www.spyfu.com) [和 Kwfinder](https://kwfinder.com/) 我个人使用、尝试和测试了所有主要的关键字研究工具，以使用 SEO 对内容进行排名，并确定最佳的绩效营销关键字。

这些工具可以帮助您识别正确的关键词，以及竞争情报。但它的缺点是需要无止境的测试来找出正确的组合，因为一个关键词的排名涉及多个因素，如内部链接，反向链接，网站速度，移动响应，内容的新鲜度，权威，域评级，模式，元标签，元描述等等。为什么一个博客排名和为什么一个博客不排名有无数的因素。

# 可预见性:

我们已经了解了在销售、客户支持、金融和许多其他领域最常用的可预测性。

例如，当客户支持不佳时，即当客户对产品不满意/即使在多次投诉后问题仍持续出现时，您可以预测客户流失。

**输入=投诉，行动=已解决/未解决，新输入=上报，最终结果=不满意的客户**

同样的可预测性可以应用于使用数据和机器学习模型的多个用例

![](img/bffb0297ceae668af6191d8319d6fcbd.png)

> 我很容易接受趋势，无论是使用 phantombuster 这样的工具收集数据，还是使用 [Mongodb 进行数据建模，根据 CRM 数据确定合适的 ICP，从而](https://www.mongodb.com/docs/manual/core/data-modeling-introduction/)[启动 ABM 试点活动](/@gvschaitanya/the-abm-execution-template-for-early-stage-companies-cee839ac22c4)

**机器学习趋势:**

自从机器学习和人工智能成为 saas 生态系统中的时髦词汇以来，我一直在关注由 Open AI 开发的 [GPT3 AI](https://openai.com/blog/gpt-3-apps/) 等趋势，它是我们每天使用的 100 个 saas 应用程序的主干，如 **Github copilot、Duolingo、keeper tax，语法上包括**都是使用 GPT3 构建的

你可以在这里阅读关于 GPT3 人工智能的研究，这里有大量开源的人工智能模型，你可以用来构建工具和解决方案

![](img/f6ff465d4694342d4694c3cea22781bc.png)

使用机器学习解决营销问题的逐步指南

在我们深入研究这种方法之前，让我们先了解一下基本的机器学习术语

***训练数据*** :训练数据是你用来训练一个算法或者机器学习模型来预测想要的结果的数据

***预测建模*** :预测建模是通过分析输入模式来预测结果的一系列统计步骤的执行

***统计建模*** :统计建模是利用数学模型，通过样本数据得出对现实世界的预测(例如天气预报)

***数据集*** :数据集是可以单独或组合访问或作为一个整体实体管理的项目的集合，数据集可以包含值、数字和其他信息的组合。

***信心得分:*** 一个信心得分是**一个 0 到 100 之间的数字**。100 分可能是完全匹配，而 0 分意味着没有匹配。置信度得分越大，输出越好。

![](img/93dd9397cbe3a501045a4ff906413cf9.png)

Confidence scoring

**逐步执行步骤**

U 唱机器学习在营销中理解关键词数据的意图，并对置信度进行评分

用例

让我们假设你想预测你想要排名的关键词的意图，这将有助于锁定正确的用户群，并为你的网站带来正确的访问者。

为了解释上述用例，我将使用[www.wayleadr.com](http://www.wayleadr.com)

[wayledr 是一种技术解决方案，通过连接智能车辆和总部位于都柏林的建筑，重新想象您旅程的最后一英里]我想预测其中一个关键词集群“停车管理”的意图

第一步:获取该关键词排名最高的博客的网址

(应该在前 10 位)

步骤 2 —我们将使用 Semrush 工具来获取关键字数据

步骤 3 -在 semrush 中设置位置跟踪

![](img/2e57151ecd5569c22e0ba3d7d2908f27.png)

第四步-我在这里为[https://wayleadr.com/blog/parking-management-strategy](https://wayleadr.com/blog/parking-management-strategy/)选出了前 20 名

![](img/dcac9e7f54b334897440ef58d5d3b60a.png)

第 5 步导出谷歌表单中的关键词列表

![](img/d20a705b1923c213dadc7754e1c42815.png)

步骤 6 在[Bigml.com](https://bigml.com/)上注册

![](img/b5f0a9984fd5c3c06a2c4c0f5c3ed477.png)

第七步:上传 CSV 文件到 bigml.com 确保你使用修剪功能修剪单元格中的空白

![](img/d44defda8c7bf440a7dc744220334916.png)

Trimming keywords

步骤 8:将表单上传到 bigml.com

![](img/ee394b4e05f069cb860ccec931a46d09.png)

第 9 步，配置源并映射关键字类别如果您有多行，如果您想进行更高级的操作，如将关键字映射到类别并预测意图，您可以根据输入映射字段

如果您正在执行一个操作，其中您预测关键字的意图并将它们分类到子组中，那么您想要接收输出的输出字段应该被标记为类别 1 和 2。

![](img/3d3f5bedbb6dd2b19939481c02ef94f5.png)

步骤 10:配置好数据源后，创建一个数据集

![](img/b725eb4073b80bff9200005ff67b756f.png)

步骤 11 训练数据集

![](img/77f516422d8ea4cf47b429a8c99aaaab.png)

步骤 12-根据你的目标选择监督学习的培训目标，这里我们的目标是识别关键词意图并为意图建立一个置信度分数

![](img/6ab2b8860d65a6e74b2563b22293c9dc.png)

步骤 12:你可以进一步钻取并执行主题建模，以确定你的副主题，这可以用来建立支柱网页

![](img/9243acbb12ddefd2dfa5ddc3b1256156.png)

Topic clustering

![](img/221eb5442853d81540f1da81a7721320.png)![](img/d767645b5c9a9450b224dea2e908b165.png)![](img/187cfec2813a9e1af385c451c7eb97d2.png)

一个支柱页的例子

![](img/aad72be0dcc3e72837b74c6fc1e8389f.png)[](https://www.hotjar.com/heatmaps/) [## 什么是热图？热图指南/如何使用它们| Hotjar

### 了解/指南/热图指南返回指南热图是了解用户在您的网站上做什么的有力方式…

www.hotjar.com](https://www.hotjar.com/heatmaps/) 

支柱页面:支柱页面是一个高层次的内容，概括了一个核心主题，并链接到关于特定子主题的深入文章。

**关键词 SEO 的主题聚类**

![](img/eeb0f96cc9c176715e8246fbc244becd.png)

**意向关键词置信度评分**

访问 https://bigml.com/account/apikey[并生成您的 bigml API 密钥](https://bigml.com/account/apikey)

为 google sheets 安装 bigml 插件

使用我们的 API 密钥和用户名认证 bigml。

![](img/218aba5c27d9e683df0fa7aa64b3b160.png)

选择您已经创建的数据集，然后选择预测

![](img/e70a898d516a813df0b20dc6a3bb0621.png)

你对关键词的意图很快就会被归类为机密

![](img/719a996084bce2583c6ea84196be7198.png)

我们可以更进一步，建立一个模型，为所有自定义参数的关键字确定搜索活动中最便宜的最佳执行关键字，如

消除停车关键词

搜索量=400

中国共产党（the Communist Party of China）

谷歌趋势低于 0.9

![](img/bfde0b4049a94bd77b59c89e5f811dd0.png)

我们可以在现有数据集上执行的其他用例有

预测正确的关键词，让你的内容排名比以往任何时候都快

预测关键词位置

搜索广告的 CPC

我们可以驾驶的交通流量

针对一组相似关键字的竞争

![](img/ee601ff57eeb1607150792a063bede1d.png)

keyword competition

![](img/50057993adc3040041e63b42598453f9.png)

keyword intent

![](img/ee601ff57eeb1607150792a063bede1d.png)

Traffic acquisition

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)