# 数据工程师:技能指南

> 原文：<https://medium.com/mlearning-ai/a-guide-for-data-engineer-in-2021-34b128100f0f?source=collection_archive---------4----------------------->

数据是当今时代每个公司最重要的资产。近来，数据工程作为一个角色越来越为人所熟悉。本文的目的是分享成为一名成功的数据工程师所需技能的细节。

# **什么是数据工程？**

数据工程是工程领域的一门学科，它专注于处理数据转换过程，为业务团队提供可操作的见解。它涵盖了数据生命周期的方方面面，从原始格式转变为可操作的见解。以下是数据工程团队的主要职责。

*   为各种来源的数据转换构建 ETL 管道
*   有效存储和检索的数据建模
*   用于处理大规模数据集的大数据处理
*   为服务集成与数据 API 交互
*   构建数据集的数据治理相关活动符合法规

# **谁是数据工程师？**

数据工程师主要关注数据的生命周期，负责设计、构建和集成来自各种来源的**数据**，并将其转换为下游用户可以使用的格式，以获取洞察力和智慧。

除此之外，数据工程师还需要与以下人员群体进行互动，以构建有效的解决方案，帮助企业取得成功。

软件工程师——他们构建服务以确保在交易期间收集指标。

**商业智能工程师**——他们根据基础数据构建报告并提出见解

**数据科学家** —他们致力于构建 ML 模型，并使用生成的数据训练模型

**企业所有者**——他们致力于发现新的业务和领导机会。

**法律团队** —处理数据治理和合规相关活动的情况。

因此，对于数据工程师来说，清楚地了解每个角色的范围对于有效的沟通和协作非常重要。

## **1。SQL**

SQL 是任何数据相关角色的基础。它仍然是任何顶级公司中数据相关职位的必备技能。任何有兴趣成为数据工程师的人都应该清楚地了解以下主题(但不限于)

*   基本选择和投影
*   过滤
*   连接类型
*   窗口功能
*   聚集

除了 SQL，清楚地理解数据库设计也很重要。

**表格设计原则**

*   键—主键、外键、代理键、超级键等。
*   指数
*   数据库约束
*   列压缩(适用于基于列的数据库引擎。即。红移)

## **2。ETL 设计**

ETL 设计侧重于从各种来源提取数据，并将其转换成下游系统可以使用的标准格式。以下是 ETL 设计过程中的重要主题。

*   满载
*   增量负载
*   加载类型(合并/覆盖/附加)
*   数据处理类型(批处理/流/实时)
*   文件格式(Avro/Parquet/JSON/CSV)
*   分区(散列/范围/列表)

## **3。数据建模**

数据建模发生在三个层次——物理的、逻辑的和概念的。

*   一个**物理模型**是一个模式或框架，用于描述数据如何物理地存储在数据库中。
*   一个**概念模型**标识了数据的高级用户视图。
*   **逻辑数据模型**位于物理层和概念层之间，允许数据的逻辑表示与其物理存储分离。

数据建模的几个重要概念？

**正常化**

**规范化**是从一个关系或一组关系中最小化**冗余**的过程。关系中的冗余可能导致插入、删除和更新异常。

标准形式:

*   第一范式
*   2NF(第二范式)
*   3NF(第三范式)
*   BCNF(博伊斯-科德范式)

**关系**

*   一对一
*   一对多
*   多对多

**维度建模(DM)** 是一种针对数据仓库中的数据存储而优化的技术。维度建模的目的是优化数据库，以便更快地检索数据。维度建模的概念是由 Ralph Kimball 提出的，由“事实”表和“维度”表组成。

尺寸模型技术将在中详细讨论[。](https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/dimensional-modeling-techniques/)

我也强烈推荐 Kimball 集团出版的数据仓库工具包[书](https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/books/data-warehouse-dw-toolkit/)。

## **4。大数据**

大数据的简单定义—太大或太复杂而无法使用传统数据处理、分析和存储技术进行管理的数据。

以下概念是当今数据工程领域的必备技能。

*   Hadoop 框架——基于 Google 发表的研究论文开发的分布式系统。 [GFS](https://static.googleusercontent.com/media/research.google.com/en/archive/gfs-sosp2003.pdf) 和[地图减少。](https://static.googleusercontent.com/media/research.google.com/en/archive/mapreduce-osdi04.pdf)
*   Apache Spark — [弹性分布式数据集:内存集群计算的容错抽象](http://people.csail.mit.edu/matei/papers/2012/nsdi_spark.pdf)。
*   [Apache Hive](https://cwiki.apache.org/confluence/display/Hive/Home#Home-UserDocumentation) —基于 SQL 的引擎，用于在分布式环境中运行查询。

## **5。编程**

数据工程不再只写 SQL 和 ETL。处理和转换数据集至少需要一种编程技能，这一点很重要。以下是数据工程师最常用的语言

*   Python —简单有效的脚本和 ETL 工作流
*   Scala——对于使用 Apache Spark 开发 ETL 管道非常有效
*   Node JS —对服务器端脚本和 Devops 相关活动有效。CDK 的 AWS Lambda 为 Node JS 提供了本地支持。

## **6。云计算服务**

云计算是按需交付计算服务，从应用程序到存储和处理能力，通常通过互联网并以按需付费的方式进行。对于现代数据工程师来说，接触一种或多种计算服务是很好的技能。但是这些技能对于基础好的人来说可以很快学会。

*   亚马逊网络服务(AWS)
*   微软 Azure
*   谷歌云平台(GCP)
*   数据砖

## **其他重要技能**

*   码头集装箱
*   性能调整
*   无服务器框架
*   CI/CD 管道
*   服务认证和授权框架
*   SAML、OAUTH、OpenID 的基础知识
*   数据治理和数据安全
*   devo PS Automation(cloud formation，CDK，Terraform)

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)