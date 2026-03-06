# Pyspark 中不可或缺的一个转换——vector assembler

> 原文：<https://medium.com/mlearning-ai/one-transformation-we-cannot-model-without-in-pyspark-vectorassembler-a36f58f9ed3b?source=collection_archive---------8----------------------->

![](img/852e68ec3f9fe18368abe576d75d1481.png)

Photo by [Bekky Bekks](https://unsplash.com/@bekkybekks?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

大家好！

> Databricks **为您的所有数据提供一个统一、开放的平台**。它为数据科学家、数据工程师和数据分析师提供了一个简单的协作环境，让他们能够运行交互式的预定数据分析工作负载。你可以得到你自己的一块馅饼来做实验。它让你能够用少数几种编程语言进行分析。

在本帖中，我们将看到一个我们离不开的转变，即 VectorAssembler，首先让我尝试回答为什么需要它:

VectorAssembler 是为了提高效率和规模。向量汇编器将使用 spark vector 等技术有效地表达这些特性，这有助于更好地处理数据和有效地管理内存。这有助于建模算法即使在大型数据列中也能高效运行。

换句话说，VectorAssembler 是一个转换器，它将给定的列列表组合成一个向量列。这对于将原始特征和由不同特征转换器生成的特征组合成单个特征向量以训练 ML 模型是有用的。VectorAssembler 接受以下输入列:

*   全是数字。
*   布尔类型。
*   向量类型。

> 你可以设置你自己的 databricks 版本进行实验，查看细节，在我之前的文章中有详细介绍

[](/mlearning-ai/how-to-use-data-bricks-community-edition-9b899a2c0f40) [## 如何使用数据砖社区版？

### 在这篇文章中，你将学习如何获得你的数据砖社区版！

medium.com](/mlearning-ai/how-to-use-data-bricks-community-edition-9b899a2c0f40) 

让我们先看看基本的

1.  开始导入库

Importing Libraries

2.让我们创建一个虚拟数据集

Creating the dataset for this experiment

3.在这个实验中，id 对于每个用户都是唯一的& clicked 是目标变量，所以我们将转换剩余的 3 个变量。

Applying Transformation

正如我们看到的，已经创建了一个名为 features 的新列，它将所有的特征向量封装到一个列中。

Final output selecting only the transformed features & target Variable(Clicked)

在下一个实验中，我们将转换一个数据集，其中也有分类变量，当我们处理分类变量/特征时，我们必须首先将它们转换为数字并应用一个热编码，为此我们将借助 stringIndexers & OneHotEncoding 等方法，让我们深入了解它们的定义:

*   **frequencyDesc:按标签频率降序排列分配最频繁的标签 0)**
*   **frequencyAsc:按标签频率升序排列(最不频繁的标签分配为 0)**
*   **alphabetDesc:按字母降序排列**
*   **alphabetAsc:升序字母顺序**

*****OneHotEncoding*** :一个一键编码器，将一列类别索引映射到一列二进制向量，每行最多只有一个 1 值，表示输入的类别索引。**

**让我们在一个例子中实现这个过程，同时注意，当我们处理分类变量时，这将是实际向量汇编过程的前奏。**

1.  **创建虚拟数据集**

**Sample dataset creation for Experiment #2**

**2.现在，我们这里有 3 个字符串列，即姓名、资格和性别，快速检查分类列的不同值，给我们以下见解。**

**Checking the available values for variable**

**3.在这个例子中，我们将对限定进行实验。**

**String Indexer for one column**

**需要对每个分类列重复这个过程。为了保持示例的简单性，让我们对 qualification 示例本身实现 onehotencoding，这里我们将使用“qualification_index”作为输入列，qualification_ohe 作为输出。**

**4.对索引列执行一次热编码**

**One Hot Encoding**

**正如我们在上面看到的，必须对所有变量重复两步过程(stringIndexer & OneHotEncoding ),我们尝试了一个称为管道的概念，它将为我们对所有列执行此过程。**

1.  **像往常一样，我们首先创建一个样本数据集**

**Sample Dataset for Experiment #3**

**该数据集是数值列和分类列的混合。**

**2.然后，我们定义希望包含在管道中的阶段，在本例中，管道中需要 3 个步骤**

*   **分类列的字符串索引**
*   **一种用于字符串索引列的热编码**
*   **向量组装(将所有列封装到一个向量列中)**

**3.我们首先定义各个阶段，首先过滤分类列&对它们应用两步过程**

**Handling Categorical Features**

**4.然后我们过滤数字列，将它们与一个热编码的分类列组合起来，并为它们定义向量组装器。**

**5.让我们创建一个管道将所有这些东西放在一起。**

**这个实验的整个笔记本可以在我的 github [这里](https://github.com/rohandawar/Databricks)找到。**

***敬请期待更多此类内容！下一集再见！！快乐学习！如果你喜欢，你读什么关注我* [*linkedIN*](https://www.linkedin.com/in/rohandawar/)**

**参考资料:**

*   **[矢量装配器](https://spark.apache.org/docs/3.1.3/api/python/reference/api/pyspark.ml.feature.VectorAssembler.html)的文档**
*   **[StringIndexer](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.ml.feature.StringIndexer.html) 的文档**
*   **[OneHotEncoding](https://spark.apache.org/docs/3.1.1/api/python/reference/api/pyspark.ml.feature.OneHotEncoder.html) 的文档**

**[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)**