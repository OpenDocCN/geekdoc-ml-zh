# 第十二章 推荐推荐：使用自然语言理解的推荐系统

> 原文：[`ppml.dev/nlp.html`](https://ppml.dev/nlp.html)

*与 Carlo Lipizzi 合作，系统与企业学院教学助理教授兼项目负责人，史蒂文斯理工学院。*

在第**1**部分，我们介绍了机器学习流程的基础；在第**2**部分，我们讨论了如何创建和维护它们；在第**3**部分，我们提供了涉及的工具和技术简要概述。现在，我们通过讨论 Carlo Lipizzi 为美国国防部构建的一个真实世界机器学习流程的简略版，将这些材料置于上下文中。我们的讨论基于 Lipizzi 等人（Lipizzi 等人 2022）及其中的参考文献。流程代码和配置的改编版本可在以下网址找到：

> [`github.com/pragprogml`](https://github.com/pragprogml) .

我们首先定义流程的范围，并将其置于适当的领域背景下（第 12.1 节）。然后，我们概述涉及的机器学习模型以及我们如何将它们视为数据处理流程（第 12.2 节）。最后，我们概述适合流程运行的硬件和软件基础设施（第 12.3 节）以及从软件工程角度来看最有趣的模块（第 12.4 节）。

## 12.1 领域问题

创建机器学习流程的第一步是定义其范围，从它将尝试解决的问题开始（第 5.3.1 节）。Lipizzi 等人（Lipizzi 等人 2022）将其描述如下：

> “一个确定利益相关者向国防采办社区提供的最相关推荐的系统[...] 从文本中提取用户特定的相关性，并推荐文档的一部分。”

换句话说，我们设想用户将提交一份或几份文档，并且机器学习流程将根据整体相关性对它们进行排序，同时突出显示每份文档中最相关的段落。这将是使命陈述文档（第 8.4 节）的理想开端。

Lipizzi 等人（Lipizzi 等人 2022）关注的领域指标是每份文档的**相关性**，其定义为：

> “通过计算超过阈值（如 0.50）的相似性单词的数量，并将其与每份文档中的单词数量进行归一化，计算出平均相似度指标，该指标展示了整个文档与整个基准之间的相似程度。[...] 相似度指标较高的文档与基准更相关或相似。”

以及*单个段落的相关性*，其定义如下：

> “为了确定每个推荐的相关部分，文档被分成单词段进行查看。[...] 发现，在相似度矩阵的 20 个单词窗口内，实际文档（包括原始文本）会有一个 35 个单词的窗口，这些单词构成了重要且相关的推荐。为了保证高质量的平均移动窗口，平均相似度的阈值设置为 0.75。任何高于该阈值的单词窗口都会追溯到原始文档并突出显示。”

成功的阈值是什么？从领域角度来看，我们希望相关文档的排名始终高于无关文档。

> “模型学习良好的一个良好指标是，控制文档的相似度（0.25）显著低于最差的推荐文档（0.5）。这意味着模型在学习和推荐领域以及找到文档中的相似性方面做得非常准确。”

在统计术语中，我们可以使用任何流行的排名一致性度量来评估管道的性能。如果我们能够访问一组由领域专家标记为相关或不相关的文档，一个简单但有效的方法可能是前\(k\)个文档中的命中比：\[\begin{equation*} \mathrm{HR} = \frac{\text{前$k$个文档中相关文档的数量}}{k}\,. \end{equation*}\] 首先，我们可能假设用户只会查看前\(k\)个文档中的相关结果：毕竟，70%–90%的用户永远不会查看谷歌结果的第二页（Shelton 2017)。因此，从领域角度来看，后续文档的排名准确性并不重要。其次，文档的标记不可避免地会有噪声（第 5.2.1 节）：不同的领域专家会产生不同的排名。希望我们可以使用一组所有专家都同意要么高度相关要么不相关的文档子集来估计 HR。像 Kendall 的\(\tau\)或 Spearman 的\(\rho\)这样的细粒度排名一致性度量可能不会对标签中的噪声具有鲁棒性，这限制了我们对不同机器学习模型性能进行对比的能力。反过来，这可能会影响监控基础设施（第 5.3.6 节）自动触发模型重新训练（第 5.3.4 节）或回滚（第 7.6 节）的能力。

为了使排名定义得更好，我们应该明确“相关”的含义。Lipizzi 等人（Lipizzi et al. 2022）将管道设计基于他们在之前工作中开发的**房间理论**框架（Lipizzi, Borrelli, and de Oliveira Capela 2021）。这个框架背后的关键思想是我们需要让机器学习模型意识到它们所操作的环境，以实现对所分析文档的语义理解。同一个词在不同的领域可能有不同的含义：区分它们及其相关性对于从自然语言处理（NLP）过渡到自然语言理解（NLU）至关重要。Lipizzi 等人（Lipizzi, Borrelli, and de Oliveira Capela 2021）提出，通过让领域专家仔细选择模型将要训练的文档，以形成一个特定主题的知识库来实现 NLU。此外，专家识别领域中的关键术语，并赋予它们权重以编码它们相对的重要性。新文档将与这个知识库进行比较，这是由机器学习模型提炼出来的：它们与知识库中的文档越相似，就越被认为是相关的。显然，如果我们用像维基百科语料库这样的大规模通用数据集来训练我们的模型，这种方法是不起作用的：正如我们在第 5.3.1 节中论证的，确定我们需要收集哪些数据对于管道表现良好至关重要。在 Lipizzi 等人（Lipizzi et al. 2022）的情况下，这些数据是美国国防部采购流程范围内的特定类型商品或服务的文档语料库。该语料库应该足够大，以涵盖该类型商品或服务的所有相关信息：如果它太小，可能不包含所有关键术语和短语，或者可能不允许我们准确建模它们之间的关系。另一方面，如果它太大，可能缺乏焦点，并可能降低模型生成的排名质量。我们应该使用与管道将要排名的文档完全相同的主题的文档来训练管道中的机器学习模型（第 9.1 节）。以这种方式限制管道的焦点也可能有助于防止模型迅速过时（第 5.2.1 节），因为相关的术语会更少，并且它们只会在特定的技术意义上使用。

## 12.2 机器学习模型

从机器学习角度来看，Lipizzi 等人（Lipizzi et al. 2022）的管道相对简单，因为它只包含一个模型。因此，它不易受到模型反馈循环（第 5.2.2 节）的影响，并且对纠正级联（第 5.2.2 节和 9.1.2 节）具有鲁棒性。数据和模型在管道中的交互如下：

1.  编码特定类型商品或服务获取领域知识的文档通过标准 NLP 技术（包括第 3.1.3 节中描述的技术）进行摄取和准备（第 5.3.3 节）。它们是一个用于训练（第 5.3.4 节）的静态数据集。

1.  领域知识通过 word2vec（Rong 2014）从文档中提炼到词嵌入，以及领域专家提供的带有相关权重的关键词列表。这些嵌入代表了 Lipizzi 等人（Lipizzi, Borrelli, and de Oliveira Capela 2021）所说的“房间”，并且是我们机器学习模型的核心。关键词是我们传递给 word2vec 的词汇表。

1.  推理（第 5.3.5 节）涉及用户提交新的文档，这些文档随后以与训练集中相同的方式进行准备。每个文档的相关性是通过计算与“房间”的相似度来衡量的，即通过池化余弦距离并按文档长度进行缩放。因此，机器学习模型输出一个介于 0 和 1 之间的标量数，然后用于对模型进行排序。

1.  同时，该模型按顺序解析每个新的文档，识别出高度相关的词序列并将其突出显示。因此，每个推理请求也会返回用户提交的文档的修改版本。

训练和推理都具有计算效率。训练的时间复杂度（第四章）介于 \(O(N \log(V))\) 和 \(O(NV)\) 之间，其中 \(N\) 是文档数量，\(V\) 是词汇表中的单词数量，这取决于 word2vec 的实现。推理对于估计相关性和突出显示相关文本部分都是 \(O(N)\)，并且可以实现对每个文档的单次遍历。此外，当有新文档可用时（Kaji 和 Kobayashi 2017），可以更新词嵌入：当嵌入变得过时时，无需从头开始重新学习。

实际上，Lipizzi 等人（Lipizzi et al. 2022）通过要求领域专家提供几百个关键术语的列表来限制 \(V\)：由于我们针对的是第 12.1 节中讨论的单种商品或服务，手动编制这样的列表是可行的。由于同样的原因，word2vec 的样本大小要求也大大降低（Dusserre and Padró 2017），因此 \(N\) 和 \(V\) 在实际应用中都是有限的。然而，我们可以通过扩展管道以包括一个模型集合（每个类型一个）来扩大范围，其中适当的模型要么（手动）由用户选择，要么（自动）通过将新文档与最近的“房间”匹配来选择。后一项任务可以重用计算文档相似性的推理模块，因此它具有线性时间复杂度，不应明显影响管道的时间复杂度。

数据摄取、数据准备、模型训练和推理的输入和输出都有明确的特征，这使得基于属性测试（第 9.4 节）构建一系列软件测试变得容易，并且可以监控它们在生产中的行为（第 5.3.6 节）。由于我们可以证明它们在功能上与我们目前使用的等效，因此涉及到的模型和算法很容易用性能更好的新模型替换。特别是：

+   数据摄取以包含文本的 PDF 作为输入，并将其中的单词作为字符串向量输出。

+   数据准备以字符串向量作为输入，执行第 3.1.3 节中讨论的操作，并输出一个包含由领域专家提供的列表中仅有的关键术语的第二个字符串向量。

+   模型训练以数据准备的输出作为输入，并输出词嵌入，这可以是稀疏矩阵或密集矩阵（第 3.2.3 和 4.5.2 节）。

+   推理以词嵌入和一个或多个新预处理文档作为输入，并输出一个相关性得分（一个标量）以及带有高亮的文档作为输出。输出可能按反向相关性排序，也可能不按顺序排序，这取决于它们是否用于程序化使用或仪表板。

我们应该测试数据摄取是否正确处理了格式良好的 PDF，并且在接收到格式不正确的 PDF 或无法解析为文本的结构化数据 PDF 时（例如，表格和方程式）要么直接失败，要么优雅地降级。可选地，我们可以通过位图和 OCR 来增强数据摄取，以尝试恢复此类文档。数据准备应处理与管道范围内的商品或服务相关的文本，删除无关的单词并拒绝非英语语言的文本。我们还应该测试模型训练和推理对于边界和有效输入（例如，没有相关关键词的文档、只有一个相关关键词的文档、所有相关关键词的文档）能够成功完成，而对于无效输入（例如，空文档、`NA`字符串）则失败。最后，我们应该添加集成和系统测试来检查所有输出以及整个管道的稳定性：提交包含具有微小扰动的文本的 PDF（例如，用同义词替换一个单词等）应导致非常相似的相关性分数。我们可以用不变量（如标点符号和大写字母）做同样的事情，这两者都应该在数据准备期间被移除。这些测试和相应的监控设施应设计为涵盖管道的所有部分，尽可能少（第 9.4.6 节），并且足够快，以便进行实时监控。

将这样一套软件测试集成到我们的 CI/CD 和监控设施中，使得我们可以安全地将新的软件和模型插入到管道的不同部分进行升级。然而，我们应该测量并记录升级部分使用的资源数量（第 5.3.6 节）。首先，我们应该确保在第 12.3 节中将要制定硬件基础设施足够运行它们，或者根据需要对其进行扩展（第 2.4 节）。其次，监控设施仍然能够提供实时反馈。毕竟，NLP 模型因其资源密集而臭名昭著（第 9.2.3 节）！对于 Lipizzi 等人（Lipizzi et al. 2022）的特定应用，推理具有低延迟尤为重要，因为我们预计用户会期望文档能够实时排序。

## 12.3 基础设施

管道软件实现应如何划分为模块应该是显而易见的：我们在第 12.2 节中描述的数据处理步骤很好地映射到我们在第 5.3 节中讨论的通用架构。为了支持它，我们应该进行一些容量规划并估计其计算、内存和存储需求（第 2.4 节）。

Lipizzi 等人描述的流程（Lipizzi 等人 2022）在计算能力方面要求不高。由于“房间”对单一类型商品或服务的狭窄关注，我们可以保持我们的训练集较小，并限制我们的存储需求。我们也没有严格的内存要求：由于词汇表中的关键术语数量有限，词嵌入的大小也有限。此外，我们不需要将完整的训练集加载到内存中才能学习它们：我们可以使用较小的批次文档，并逐步学习嵌入（Kaji 和 Kobayashi 2017）。

因此，至少我们需要：

+   用于模型训练的机器学习系统，针对计算和内存进行了优化；

+   一组具有较少内存和计算能力但网络连接良好的系统，用于分配推理负载并保持低延迟；

+   一个存储系统，用于存储用于训练的 PDF 文档、从中提取的预处理文本数据以及包含嵌入的模型存储库；

+   一个独立的系统，用于托管流程编排器、CI/CD 基础设施以及日志和监控的服务器组件（见 5.3.6 节）。

我们还应该注意为所有使用的环境（测试、预生产、生产）分配足够的资源，并尽可能使它们的硬件配置相似。

专门用于模型训练的机器学习系统可能是一个本地系统：这有助于实验，因为远程系统上的可观察性更有限（见 2.3 节）。此外，我们应该为未来做好准备：我们应该配备 GPU 或 TPU，以便能够探索更复杂的 NLP 模型（着眼于采用它们并替换 word2vec，如果它们表现更好），并加速 word2vec，使其达到增加训练集大小随时间推移不再成为问题的程度。因此，存储包含原始和准备数据的存储系统也应该是本地系统，并放置在执行模型训练的同一设施中，以减少模型训练中的数据访问开销。冷存储（见 2.1.2 节）适用于原始数据（PDF 文档）：我们只需要在处理数据摄取和准备时访问它们，而不是用于训练。热存储可能更适合准备数据，再次限制数据访问的开销并提高操作强度（见 2.2 节）。

相比之下，运行推理模块的机器学习系统如果用户遍布全球，比如美国国防人员，那么将其放置在地理上分布的云实例中会更好，这样可以全面降低延迟。我们可能将模型仓库放置在同一云中，以方便模型部署（5.3.5 节和第七章）。

最后，协调器、CI/CD 基础设施以及日志和监控设施应该放置在完全独立的硬件和网络连接上，以确保它们在不受其他模块中任何硬件或软件问题影响的情况下仍然可用和可访问。我们还应该将它们（或如果我们使用 MLOps 平台代替它们）配置为集群模式，以避免单点故障，并追求最大可伸缩性和可靠性。它们将需要将管道恢复到功能状态，例如，通过回滚故障的机器学习模型。将模型注册表保留在云中使得在不同地理区域复制它更容易，从而在不利情况下提高其可用性和可靠性。

我们如何设计备份和灾难恢复策略？这取决于我们如何管理我们的基础设施，以及基础设施是本地、远程还是两者的混合。如果我们可以依赖配置管理，并且我们的基础设施完全以代码形式描述，那么从头开始重新创建它并重新运行 CI/CD 管道可能是更好的选择。仅仅重新运行 CI/CD 管道可能就足以修复一些小问题，比如失败的模块部署。例如，Kubernetes（Kubernetes 作者 2022a）可以备份它管理的任何集群的状态，并在灾难发生时从单个组件恢复到整个集群（Velero 作者 2022b）。如果我们的基础设施没有以代码形式存储，这可能适用于遗留环境，我们只能依赖定期对所有系统进行快照，并在需要时恢复它们。

如果我们的基础设施中有一部分是远程的，我们应该记住云提供商和第三方服务可能会出现故障，并且可能会有一天或两天的停机时间。因此，拥有一个地理上分布的系统集合，其中包含云和本地部署的混合，会更安全。在推理模块的情况下，我们可以确保用户或消费推理输出的服务在出现故障时可以回退到一个正常工作的系统（希望可以透明地处理重试和回退）。

## 12.4 管道架构

我们的目标是开发一个尽可能接近生产级别的管道，用于 Lipizzi 等人（Lipizzi et al. 2022）提出的用例，同时保持其足够简单，以便可以作为我们在第**2**和**3**部分讨论的实践的有用说明。为了使其完全可重复，我们只使用开源组件，作为独立应用程序或从容器镜像（Docker 2022a）安装和执行。此外，我们选择使用 Git（The Git Development Team 2022）单仓库（Section 6.4）来管理整个管道，该仓库包含所有配置、提供、启动和监控其执行的组件。有关管道架构的附加文档以及我们开发环境的软件先决条件列表可以在存储库根目录下的`README.md`文件中找到。

我们将参考第 5.3 节中提出的管道结构作为架构，该结构从高层次概述了一个包含五个架构模块的管道：

+   数据摄取和数据处理（Section 12.4.1）；

+   数据跟踪和版本控制（Section 12.4.2）；

+   模型训练、验证和实验跟踪（Section 12.4.3）；

+   模型打包（Section 12.4.4）；

+   CI/CD 部署和推理（Section 12.4.5）。

![我们的 NLU ML 管道架构](img/27108feb5f2e81c28088058d44055315.png)

图 12.1：我们的 NLU ML 管道架构。

我们选择这种设计，如图 12.1 所示，有两个原因：

+   实际操作中，很少由单一软件解决方案端到端管理管道：在绝大多数情况下，它们由多个平台和组件组成并协同工作。

+   不同的软件有不同的优势和劣势，并且每款软件在特定领域都表现出色：正如我们在第**3**部分所指出的，没有一种适合所有情况的 MLOps 解决方案！使用针对数据工程、模型训练和实验跟踪的独立解决方案，我们可以展示不同的开源工具以及如何将它们接口连接。

### 12.4.1 数据摄取和数据处理

为了确保可重复性，我们决定使用一组可自由访问的文档，而不是 Lipizzi 等人（Lipizzi et al. 2022）最初使用的那些：属于一个相当同质的研究主题的科学文章语料库，同时，该语料库有足够多的出版物。我们选择的语料库包括包含“因果推断”、“因果网络”、“反事实”或“因果推理”等术语的 arXiv 预印本，并且是在 2021 年 8 月 1 日至 2022 年 8 月 31 日期间提交的。生成的查询

```py
date_range:from 2021-08-01 to 2022-08-31;abs:"causal inference" OR
  "causal network" OR "counterfactual" OR "causal reasoning"
```

使用 arXiv 的公共 API（ArXiv 2022）提交，返回包含相关元数据的 1044 篇文章语料库，包括 PDF 文件的 HTTP URL。

![数据摄取和数据准备步骤。](img/8e80f14f4d57f9a9a5ac7ada0ca07bc5.png)

图 12.2：数据摄取和数据准备步骤。

我们使用 Apache Airflow（The Apache Software Foundation 2022a）实现这一部分的管道，我们在第 10.3 节中介绍了它。表示数据摄取和数据准备步骤的 DAG 如图 12.2 所示：每个步骤都实现为一个 Python 函数，并由 Airflow 使用通用的`PythonOperator`和`pythonVirtualenvOperator`接口调用。更详细地说：

1.  *ArXiv 查询*：我们调用 arXiv API 并处理返回的列表以提取 PDF URL。

1.  *文章下载*：我们使用多线程 HTTP Python 客户端下载查询返回的 PDF，并遵守 arXiv 施加的速率限制，并将它们存储在本地文件系统或本地对象存储（使用 MinIO（MinIO 2022）实现）中。

1.  *文本转换*：我们使用众多可用的 Python 库之一，如 PyPDF2（Fenniak 2022）、PdfMiner（Shinyama, Guglielmetti, and Marsman 2022）或 Spacy（Explosion 2021），将 PDF 中的文本提取到纯文本文件中。与之前一样，我们使用线程池并行处理多个文档。

1.  *基本清理*、*n-gramming*、*停用词去除*：我们使用 NLP 库如 NLTK（NLTK Team 2021）、Spacy（Explosion 2021）和 Gensim（Řehůřek and Sojka 2022a）对文本文件进行预处理。特别是，我们执行大小写转换、标点符号和停用词去除、词干提取、词形还原和 n-gramming。

1.  *标记*：剩下的适合在 NLP 和 NLU 应用中进行建模的标记²⁶。

Airflow DAG 的 Python 代码提供了如何实现和链接图 12.1 中块的程序性视图。

```py
with DAG('ingestion', ...) as dag:
 [...]
 get_article_urls = PythonOperator(
 task_id='query_arxiv_archive',
 python_callable=query_arxiv,
 op_kwargs={'query': query}
 )

 download_article = PythonOperator(
 task_id='download_from_arxiv_archive',
 python_callable=download_arxiv,
 op_kwargs={}
 )

 extract_text_from_article = PythonOperator(
 task_id='extract_text',
 python_callable=convert_pdf_to_text
 op_kwargs={},
 )
 [...]
 get_article_urls >> download_article
 download_article >> extract_text_from_article
 [...]
```

我们面临的两个主要挑战是从 PDF 文件中提取文本的可扩展性和软件测试的鲁棒性。我们通过在 Airflow 中调用的 Python 代码中使用多线程来实现可扩展性；我们可以在 Airflow DAG 的级别上使用 Celery（Apache 软件基金会 2022a）或 Kubernetes（Kubernetes 作者 2022a）执行器实现类似的结果，或者完全用 Apache Spark（Apache 软件基金会 2022f）替换 Airflow。至于清理提取的文本，我们开发了一套自定义方法来执行基本的 NLP 清理任务，并为领域因果推理专家识别的关键术语开发了一个自定义 n-gram 方法来检测单语元、双语元和三元语。这两个都是组织在专门的子模块中，并通过单元测试进行补充。n-gram 列表是一个静态资源文件，在 Git 中进行版本控制，并通过环境变量在管道阶段进行引用。

DAG 的输出是我们将使用 word2vec 模型化的标记列表。标记、PDF URL 列表、n-gram 列表以及定义 arXiv 查询的元数据存储在由 DVC（Iterative 2022b）支持的跟踪和版本控制仓库中，以确保可重复性和允许我们跟踪数据来源，如第 5.3.3 节所述。我们可以通过自定义 Airflow 操作符或从 Airflow 内置操作符 `BashOperator` 调用 `dvc` 命令行客户端来集成 Airflow 和 DVC。

Airflow DAG 被配置为将任务日志写入 `stdout`，在那里它们被像 Fluentd（Fluentd 项目 2022）这样的工具收集，并转发到像 Elasticsearch（Elasticsearch 2022）这样的日志数据库。Airflow 还可以配置为将任务执行指标导出到由像 Grafana（GrafanaLabs 2022）这样的工具构建的仪表板。日志本身是 Python Airflow 代码中 `LogRecord` 对象的 JSON 对象表示形式，可以传递给 Python 日志模块。

### 12.4.2 数据跟踪和版本控制

除了以可重复的方式摄取和清理数据外，我们还想跟踪第 12.4.1 节中描述的步骤产生的所有数据集：DAG 可能会被安排每天运行，使用不同的搜索查询来创建如 Lipizzi 等人所述的额外知识域，或者重新训练现有的 word2vec 模型。因此，我们选择将机器学习代码（第 6.5 节）与文本语料库一起进行版本控制。这使我们能够评估不同的 NLP 框架、word2vec 参数的选择以及来自领域专家的 n-gram 集合。

如我们在第 12.4.1 节中提到的，我们选择 DVC 来实现数据版本控制。DVC 还可以执行实验跟踪，但我们将使用 MLflow（在第 10.1 节中与 DVC 一起介绍）在第 12.4.3 节中实现它。我们初始化 Git 仓库以供 DVC 使用，并使用命令 `dvc pull` 从我们存储它们的远程对象存储中拉取 Airflow DAG 生成的标记。这也会拉取相应的元数据，这些元数据是版本化的，并存储在类似下面的 YAML `.dvc` 文件中。

```py
outs:
- md5: 853c9693c5aac78162da1c3b46aec63e
 size: 2190841
 path: causal_inference.txt

meta:
 search_query: "causal inference"
 search_start: 1629410400
 search_end: 1672441200
 [...]
```

`md5` 属性表示内容的哈希值，而 `path` 属性是文件或目录相对于工作目录的路径，默认为文件的位置。然后我们可以开始使用以下类似的发展流程进行实验。

```py
$ git log --oneline
669a39e (HEAD -> master, tag: v0.0.1) - w2v baseline impl.
[...]
$ dvc remote list # list remote storage configured in DVC
exp_bucket   s3://exp_bucket
$ dvc pull # fetch data from remote storage into the project
A       datasets/causal_inference.txt
A       datasets/causal_inference_small.txt
2 files added
$ nvim src/train-cli.py # tune the training code
$ pipenv run src/train-cli.py --dataset=datasets/causal_inference.txt
 ...
$ git status -s
 M src/train.py
$ git add src/train-cli.py
$ git commit -m 'Changed word2vec window size to 4'
$ git tag -a 'v0.0.2' -m 'Changed word2vec window size to 4'
```

### 12.4.3 训练和实验跟踪

我们在第 12.4.1 节中生成的标记，并在第 12.4.2 节中跟踪的标记是 Gensim 中 word2vec 实现的输入，可从 `models.word2vec` 获取，以及领域专家提供的 n-gram 列表（下面代码中的 `vocabulary` 变量）。word2vec 返回一个 `wv` 对象，该对象将每个嵌入（即单词向量）存储在称为 `KeyedVectors` 的结构中，该结构将 n-gram（“键”）映射到向量。`KeyedVectors` 可以用于对向量执行操作，例如计算它们的距离或相似度。

```py
[...]
model = Word2Vec(
 callbacks=[Word2vecCallback()],
 compute_loss=True,
 vector_size=vector_size,
 min_count=min_count,
 window=window,
 workers=workers)
)

model.build_vocab(
 corpus_iterable=vocabulary,
 progress_per=1,
 trim_rule=_rule
)

model.train(
 sentences,
 total_examples=model.corpus_count,
 epochs=epochs,
 report_delay=1.0,
 compute_loss=True,
 callbacks=[Word2vecCallback()],
)

word_vectors = model.wv
[...]
```

我们通过调用 DVC Python API 的 `get_url()` 方法（迭代 2022c）来获取标记，该方法返回 `corpus_path` 在 `path` 中定义的 `revision` 数据集的存储位置的 URL。

```py
import dvc.api
...
corpus_path = dvc.api.get_url(
 path=corpus_path,
 repo=repo_path,
 rev=revision,
 remote=remote
)
...
```

语料库是顺序读取、标记并直接输入到 word2vec 的 `train()` 方法的。我们根据第 5.1 节中的建议使用环境变量设置 `train()` 方法的参数（Řehůřek 和 Sojka 2022b），以方便进行不同组合的多次实验。

+   *vector_size*：单词向量的维度数（默认：100）；

+   *window*：句子中当前单词和预测单词之间的最大距离（默认：5）。

+   *min_count*：一个单词被认为是的最低频率（默认：5）。

+   *workers*：训练模型的工作线程数（默认：3）。

至于实验跟踪，我们使用以下 MLflow 跟踪 API 来实现：

+   `log_param()`：用于跟踪 word2vec 参数和与输入标记相关的元数据，特别是生成它们的 arXiv 查询以及它们被拉取的 DVC 文件路径和哈希；

+   `log_metric()`：用于记录模型生成的嵌入的维度；

+   `log_artifact()`: 用于记录本地文件或目录的名称，例如包含领域专家的 n-gram 和训练模型的词向量等实验成果。

下面是一个简短的例子，说明我们如何在代码中使用这些方法。

```py
from mlflow import (log_metric,log_param,log_artifacts,
 create_experiment,start_run,end_run)
[...]
experiment_id = create_experiment(
 "NLU experiments on causal inference corpus",
 artifact_location=Path.cwd().joinpath("mlruns").as_uri(),
 tags={},
)
start_run(experiment_id=experiment_id)
log_param("query", query)
[...]
log_param("window", window)
log_param("stop_date", stop_data)
[...]
log_metric("wv_size", model.wv.vector_size)
[...]
log_artifact("corpus.txt")
log_artifact("keywords.txt")
log_artifact("vectors.kv")
end_run()
[...]
```

我们使用 MLflow 的`python_function`接口保存模型，该接口支持作为通用 Python 函数实现的自定义模型。具体来说，我们使用 Gensim 函数`save()`将包含在`model.wv`中的学习词向量进行序列化，并在服务模型时使用`KeyedVectors.load()`函数重新加载它们。

### 12.4.4 模型打包

我们在第 11.2 节中介绍了 BentoML（BentoML 2022），它可以导入序列化的 Python 模型或 MLflow 模型，并且它可以以最小的胶水代码将 API 绑定到 RESTful 端点。因此，它是打包和提供 word2vec 模型的一个方便选择。在我们的情况下，计算用户提交的 PDF 文档与用于训练 word2vec 模型的文档之间相似度的分类 API（即我们在第 12.1 节中称为“房间”的内容）公开为`/classify`端点。

下面的代码片段显示了使用 BentoML 提供的 API 和装饰器声明服务的示例。一旦服务启动，API 将在`/classify`处可用：它将接受 PDF 文件作为输入，并返回介于 0 和 1 之间的标量。作为一个未来的增强功能，我们可以构建一个额外的`/rank` API 端点，该端点接受 JSON 格式的 PDF URL 列表作为输入，为每个 URL 调用`/classify` API，并返回带有相关排名和相似度的排序文档列表。

```py
from __future__ import annotations

import io
from typing import Any
import typing

import numpy as np

import bentoml
from bentoml.io import File
from bentoml.io import JSON

nlu_runner = bentoml.picklable_model.get("nlu_exp:v0.0.2).to_runner()
svc = bentoml.Service("pdf_classifier", runners=[nlu_runner])

@svc.api(input=File(), output=JSON())
def classify(input_pdf: io.BytesIO[Any]) -> typing.List[float]:
 return nlu_runner.classify.run(input_pdf)
```

### 12.4.5 部署和推理

使用容器部署和提供模型的一个优点是，它们可以使用 Docker 在本地部署，或者使用 Kubernetes 在目标（可能是远程）环境中部署（见第 7.1.4 节）。这在我们的用例中是一个重要的点：如第 12.3 节所述，我们的管道在本地和远程系统的组合上运行。因此，我们使用`bentoml containerize`命令构建一个包含运行我们第 12.4.4 节中定义的推理 API 所需的所有要求的容器镜像：输出是一个实现了 Python 的无状态 RESTful API 服务器的 Docker 容器。构建容器的命令如下所示。

```py
$ bentoml containerise nlu_exp:v0.0.2
$ docker run -d --rm -p 3000:300 nlu_exp:v0.0.2
```

![BentoML 生成的 OpenAPI 规范。](img/a327ff14abdfde1b2e10ed90daf493a2.png)

图 12.3：BentoML 生成的 OpenAPI 规范。

容器启动后，API 服务器可通过 `http://127.0.0.1:3000` 访达。URL `http://127.0.0.1:3000/classify` 提供了第 12.4.4 节中的 API，而 `http://127.0.0.1:3000/` 显示了一个包含动态生成的 OpenAPI 文档的网页（SmartBear Software 2021）（见图 12.3）。我们还提供了额外的存活性和就绪性 API，以支持在 Kubernetes 上的部署，以及一个返回 Prometheus 格式服务指标的 `/metrics` 端点（Prometheus Authors and The Linux Foundation 2022）。

RESTful 接口设计为可编程使用：我们可以使用 `curl` 或 Postman 等 API 测试工具（Velero Authors 2022a）来访问它。我们还可以在我们的持续集成设置中查询它以运行集成测试，并验证构建过程是否成功创建了容器镜像。然而，RESTful 接口也可以作为后端构建消耗 API 输出并通过仪表板（使用我们在第 11.3 节中讨论的工具）或简单网页界面（使用如 React（Meta Platforms 2022b）或 Vue.js（You 2022）等库或框架）的 Web 应用程序。它们对领域专家来说很有用，可以检查推理输出并验证它们以及生成它们的模型，作为人机交互（见第 5.3.4 和 5.3.6 节）。特别是，它们使得领域专家能够迭代地细化我们用作 word2vec 词汇表的关键术语列表，正如 Lipizzi 等人所设想的那样（Lipizzi et al. 2022）。

要验证 `/classify` API，我们可以使用命令行工具 `curl` 上传（POST）一篇关于因果推理的科学论文的 PDF。

```py
$ curl -H "Content-Type: multipart/form-data" \
 -F 'fileobj=@good-article.pdf;type=application/octet-stream' \
 http://remote:3000/classify
{"value":0.8203434225167339}%
```

以及另一篇关于完全不同主题的文章。

```py
$ curl -H "Content-Type: multipart/form-data" \
 -F 'fileobj=@bad-article.pdf;type=application/octet-stream' \
 http://remote:3000/classify
{"value":0.24675693999330117}%
```

从相关性得分中我们可以看出，“/classify” API 对于相关和不相关的文档都能正确响应（见第 9.4.6 节）。底层的 `classify()` 方法计算 `KeyedVectors` 之间的余弦距离，以浮点数形式返回相似度，并通过 Fluentd 将 PDF 元数据和相关性记录到远程日志数据库。

```py
[...]
# load serialised KeyedVectors from the `knowledge_ww_fp` path
knowledge_wv = KeyedVectors.load(knowledge_ww_fp, mmap="r")
# get the KeyedVectors pairs that match the
# vocabulary word (the keyword list from the expert)
knowledge_v = get_word2vec_vectors(
 word_vectors=knowledge_wv,
 vocabulary=vocab
)
# train on the fly a word2vec model on
# the PDF converted into text
model = word2vec(text, vocab)
document_v = get_word2vec_vectors(
 word_vectors=model.wv,
 vocabulary=vocab
)
[...]
dist = 1 - distance.cosine(document_v.mean(0), knowledge_v.mean(0))
[...]
logger.warning("Classify request with distance %f for %s",
 dist, metadata)
[...]
return dist
```

为 API 提供服务的 Docker 镜像可以使用 Jenkins（Jenkins 2022b）、GitLab CI 或 GitHub Actions 等工具在每次发布新模型时自动重建。我们可以通过应用第 7.2 节中讨论的技术之一将其部署到容器服务或编排器。由于其无状态组合，容器在必要时可以水平扩展（我们只需部署更多实例），因此我们可以随着时间的推移处理不断增加的负载。

### 参考文献

Abid, A.，A. Abdalla，A. Ali，D. Khan，A. Alfozan，和 J. Zou。2022。*Gradio：在野外轻松分享和测试机器学习模型*。[`www.gradio.app/docs/`](https://www.gradio.app/docs/).

Apache 软件基金会。2022a。*Celery 执行器*。[`airflow.apache.org/docs/apache-airflow/stable/executor/celery.html`](https://airflow.apache.org/docs/apache-airflow/stable/executor/celery.html).

ArXiv。2022。*arXiv API 访问*。[`arxiv.org/help/api`](https://arxiv.org/help/api).

BentoML。2022。*统一模型服务框架*。[`docs.bentoml.org/en/latest/`](https://docs.bentoml.org/en/latest/).

Docker。2022a。*Docker*。[`www.docker.com/`](https://www.docker.com/).

Dusserre, E.，和 M. Padró。2017。“更大不一定更好！我们更喜欢特异性。”在 *第 12 届国际计算语义学会议论文集* 中，第 1-6 页。

Elasticsearch。2022。*免费和开源搜索：Elasticsearch、ELK 和 Kibana 的创造者*。[`www.elastic.co/`](https://www.elastic.co/).

爆发。2021。*Spacy：工业级自然语言处理*。[`spacy.io/`](https://spacy.io/).

Fenniak, M.。2022。*PyPDF2 文档*。[`pypdf2.readthedocs.io/en/latest/`](https://pypdf2.readthedocs.io/en/latest/).

GrafanaLabs。2022。*Grafana：开源可观察性平台*。[`grafana.com/`](https://grafana.com/).

迭代。2022b。*DVC：数据版本控制。数据与模型的 Git*。[`github.com/iterative/dvc`](https://github.com/iterative/dvc).

迭代。2022c。*DVC Python API*。[`dvc.org/doc/api-reference`](https://dvc.org/doc/api-reference).

Jenkins。2022b。*Jenkins 用户文档*。[`www.jenkins.io/doc/`](https://www.jenkins.io/doc/).

Kaji, N.，和 H. Kobayashi。2017。“带有负采样的增量 Skip-gram 模型。”在 *2017 年自然语言处理实证方法会议论文集* 中，第 363-371 页。

Lipizzi, C.，H. Behrooz，M. Dressman，A. G. Vishwakumar，和 K. Batra。2022。“获取研究：创造信息变化的协同效应。”在 *第 19 届年度获取研究研讨会论文集* 中，第 242-255 页。

Lipizzi, C.，D. Borrelli，和 F. de Oliveira Capela。2021。*使用“房间理论”实现主观性的计算模型：从文本中检测情感案例*。[`arxiv.org/abs/2005.06059`](https://arxiv.org/abs/2005.06059).

Meta Platforms. 2022b. *React: A JavaScript Library for Building User Interfaces*. [`reactjs.org/`](https://reactjs.org/).

MinIO. 2022\. *MinIO 文档*. [`docs.min.io/docs`](https://docs.min.io/docs).

NLTK 团队. 2021\. *NLTK: 自然语言工具包*. [`www.nltk.org/`](https://www.nltk.org/).

Prometheus 作者，以及 Linux 基金会. 2022\. *Prometheus: 监控系统和时间序列数据库*. [`prometheus.io/`](https://prometheus.io/).

Řehůřek, R. 和 P. Sojka. 2022a. *Gensim 文档*. [`radimrehurek.com/gensim/auto_examples/index.html`](https://radimrehurek.com/gensim/auto_examples/index.html).

Řehůřek, R. 和 P. Sojka. 2022a. *Gensim 文档*. [`radimrehurek.com/gensim/auto_examples/index.html`](https://radimrehurek.com/gensim/auto_examples/index.html).

2022b. *Gensim 文档*. [`radimrehurek.com/gensim/models/word2vec.html`](https://radimrehurek.com/gensim/models/word2vec.html).

Rong, X. 2014\. “Word2vec 参数学习解释.” *arXiv 预印本 arXiv:1411.2738*.

Shelton, K. 2017\. *搜索结果排名的价值*. [`www.forbes.com/sites/forbesagencycouncil/2017/10/30/the-value-of-search-results-rankings/`](https://www.forbes.com/sites/forbesagencycouncil/2017/10/30/the-value-of-search-results-rankings/).

Shinyama, Y., P. Guglielmetti, and P. Marsman. 2022\. *Pdfminer.six 的文档*. [`pdfminersix.readthedocs.io/en/latest/`](https://pdfminersix.readthedocs.io/en/latest/).

SmartBear Software. 2021\. *OpenAPI 规范*. [`swagger.io/specification/`](https://swagger.io/specification/).

Streamlit. 2022\. *Streamlit 文档*. [`docs.streamlit.io/`](https://docs.streamlit.io/).

Apache 软件基金会. 2022a. *Airflow 文档*. [`airflow.apache.org/docs/`](https://airflow.apache.org/docs/).

Apache 软件基金会. 2022f. *Apache Spark 文档*. [`spark.apache.org/docs/latest`](https://spark.apache.org/docs/latest).

The Fluentd 项目. 2022\. *Fluentd: 开源数据收集器*. [`www.fluentd.org/`](https://www.fluentd.org/).

Git 开发团队. 2022\. *Git 源代码镜像*. [`github.com/git/git`](https://github.com/git/git).

Kubernetes 作者. 2022a. *Kubernetes*. [`kubernetes.io/`](https://kubernetes.io/).

Velero 作者. 2022a. *Postman 文档*. [`learning.postman.com/docs`](https://learning.postman.com/docs).

Velero 作者. 2022b. *Velero 文档*. [`velero.io/docs`](https://velero.io/docs).

You, E. 2022\. *Vue.js: 进步的 JavaScript 框架*. [`vuejs.org/`](https://vuejs.org/).

* * *

1.  一系列字符被分组以提供 NLP 处理的语义单元。↩︎
