# 借助 Streamlit、SQL 和 Plotly 实现快速商业智能

> 原文：<https://medium.com/mlearning-ai/quick-business-intelligence-with-streamlit-and-sql-cd6a9ba8a48f?source=collection_archive---------3----------------------->

商业智能非常简单，你可以自己编辑源代码。

![](img/577f95a856819888a6b496939269e6c0.png)

Streamlit and Plotly. A match made in heaven.

一旦有一些客户认真使用你的产品，你就需要开始分析了。许多人都想从一个成熟的超集和雪花型现代数据堆栈开始。通常，这是一个很好的解决方案，但往往是矫枉过正。

如果您正在分析中型到大型数据集，在 SQL 中执行转换会快得多，从而最大限度地减少需要通过网络传输的数据量。

Analytics via Streamlit. Video by Author.

随着像 DBT 和 Dagster 这样的工具的开发，调度转换变得非常容易，在您的 BI 工具中不需要繁重或复杂的转换和缓存层。DBT 可以管理转换，编排层可以频繁地将数据具体化为制图所需的结构，以保持事物的性能。

绝大多数图只有一个 X 轴值和一个 Y 轴值，我想简化界面，使第一列始终是 X 输入，第二列始终是 Y 输入。

请注意，这个应用程序的快乐之路很窄。越来越多的融入其中，并记录下我前进的每一步。一旦它成为一个整洁而简单的解决方案，我会把它放在 Docker Hub 和 pypi 上，作为获得高效商业智能的最快途径。

> PyPi 上唯一的低代码商业智能解决方案(也许)

我计划添加保存查询、保存图表配置、集成 DBT、连接到雪花以及通过 API 端点访问保存的查询结果的方法。

如果你对这个作品感兴趣，请鼓掌并跟随。在接下来的几个月里，我将继续创建易于调整和使用的跨分析和机器学习的工具。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)