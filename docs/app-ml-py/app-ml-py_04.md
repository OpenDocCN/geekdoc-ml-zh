# 机器学习工作流程构建和编码

> 原文：[`geostatsguy.github.io/MachineLearningDemos_Book/MachineLearning_workflow_construction.html`](https://geostatsguy.github.io/MachineLearningDemos_Book/MachineLearning_workflow_construction.html)

Michael J. Pyrcz，教授，德克萨斯大学奥斯汀分校

[Twitter](https://twitter.com/geostatsguy) | [GitHub](https://github.com/GeostatsGuy) | [网站](http://michaelpyrcz.com) | [Google Scholar](https://scholar.google.com/citations?user=QVZ20eQAAAAJ&hl=en&oi=ao) | [地统计学书籍](https://www.amazon.com/Geostatistical-Reservoir-Modeling-Michael-Pyrcz/dp/0199731446) | [YouTube](https://www.youtube.com/channel/UCLqEr-xV-ceHdXXXrTId5ig) | [Python 应用地统计学电子书](https://geostatsguy.github.io/GeostatsPyDemos_Book/intro.html) | [Python 应用机器学习电子书](https://geostatsguy.github.io/MachineLearningDemos_Book/) | [LinkedIn](https://www.linkedin.com/in/michael-pyrcz-61a648a1)

电子书“《Python 应用机器学习：带代码的实战指南》”的一章。

引用此电子书如下：

Pyrcz, M.J., 2024, *《Python 应用机器学习：带代码的实战指南》* [电子书]. Zenodo. doi:10.5281/zenodo.15169138 ![DOI](https://doi.org/10.5281/zenodo.15169138)

本书中的工作流程及其他工作流程在此处可用：

引用 MachineLearningDemos GitHub 仓库如下：

Pyrcz, M.J., 2024, *MachineLearningDemos: Python Machine Learning Demonstration Workflows Repository* (0.0.3) [软件]. Zenodo. DOI: 10.5281/zenodo.13835312\. GitHub 仓库：[GeostatsGuy/MachineLearningDemos](https://github.com/GeostatsGuy/MachineLearningDemos) ![DOI](https://zenodo.org/doi/10.5281/zenodo.13835312)

由 Michael J. Pyrcz 编写

© 版权所有 2024。

本章是**机器学习工作流程构建和编码**的总结，包括基本概念：

+   学习编码的建议

+   工作流程设计基础

+   工作流程构建模块

**YouTube 讲座**：查看我在以下方面的讲座：

+   [机器学习简介](https://youtu.be/zOUM_AnI1DQ?si=wzWdJ35qJ9n8O6Bl)

+   [工作流程构建和编码](https://youtu.be/45mLZQIu1Gk?si=XjmfF8yrngCvDujR)

+   [地下和空间建模](https://youtu.be/w98m0cPN2Ag?si=y4DYNt9Cr6N8gALT)

## 机器学习工作流程构建和编码的动机

你可以学习数据科学和机器学习的理论。完全理解概率、统计学、推理和预测，超参数调优和模型检查的交叉验证，但为了增加价值，我们必须编写实用、适合目的的工作流程，以便广泛部署和使用。我已经做了很多年，我的目标是分享一些我的经验，帮助你入门或提高你的技能！

如果你还没有编码，请查看我为所有科学家和工程师学习一些编码的理由。我经常在社交媒体和我的课程中分享这个观点。

## 所有科学家和工程师学习一些编程的理由

这里是 Pyrcz 教授对所有科学家和工程师学习一些编程的理由。

1.  **透明度** – 没有编译器会接受挥手！编码迫使你的逻辑暴露出来，以便其他科学家或工程师进行审查。

1.  **可重复性** – 运行它并得到答案，交给同行，他们运行它也会得到相同的答案。这是科学方法的一个原则。

1.  **量化** – 程序需要数字，它推动我们从定性到定量。给程序输入，发现新的看待世界的方法。

1.  **开源** – 利用一个充满智慧的世界。查看包、片段，并惊叹于伟大心灵所免费分享的内容。

1.  **打破障碍** – 不要把它扔过栅栏。与开发者一起坐在桌边，分享更多你的专业知识，以获得更好的部署产品。

1.  **部署** – 与他人分享你的代码，扩大你的影响力。无论是性能指标还是利他主义，你的好工作都会让许多人受益。

1.  **效率** – 最小化工作中无聊的部分。构建一套用于自动化常见任务的脚本，花更多的时间进行科学和工程实践！

1.  **总有再次做它的时间** – 你只做了一次吗？编写和自动化工作流程可能需要 2-4 倍的时间。通常，这是值得的。

1.  **成为我们的一员** – 这将改变你。用户会感到受限，程序员真正利用了他们的应用程序和硬件的力量。

我有数十年的 Basic、Fortran、Visual Basic for Applications (VBA)、C++、R 和 Python 经验。我在中学时第一次编写的代码是在德州仪器的 TI-99/4A 家用电脑上用 Basic 编写的，并存储在磁带上。我做过很多数值原型、工作流程、脚本、仪表板，甚至几年全栈开发。让我这样讲，"预装软件"让我感到被困住，我立刻寻找应用程序编程接口（API），以便我能够逃脱并编写自己的方法、仪表板和可视化。然而，我对编程仍然非常务实和谦逊；因此，我的建议是学习你需要完成工作的内容。以下是一些替代方案：

+   学习如何在预装软件中使用工作流程自动化功能。通常，文档质量很好，这种方法不仅为你的工作流程提供了审计轨迹，而且也符合你组织的流程标准和 IT 部门政策。

+   如果预装软件接受 Python、Java、控制，那么学习这些语言的基础知识以与工作流程中的对象进行交互。这通常会极大地提高你创建增值工作流程的灵活性。

+   许多组织开始使用 Python 等工具原型化和部署工作流程，例如 Jupyter Notebooks 和 Jupyter Lab。如果这样，学习一些 Python 基础知识，开始使用像 Numpy、Pandas、Matplotlib 和 SciPy 这样的基本 Python 包。上网并使用 Google 和 ChatGPT 来帮助您的工作流程开发。有如此多的资源可以帮助，包括这本电子书以及我在本章中分享的编码基础知识。

不要感到害怕，我不希望你（或诅咒你）过上全栈开发的生活。那些是在 C++中寻找内存泄漏和悬挂指针的日子。经历了所有这些之后，我说的是，

使用 Python

我编写的代码更少，但完成的工作更多！

## 建模目的

任何数据科学项目的第一步是提出问题，这项工作的目的是什么？为了解决这个问题，我们必须退后一步并问：

1.  问题是什麼？

1.  别人为了解决这个问题都做了什么？

1.  我们将采取什么措施来解决这个问题？

1.  我们可以部署什么来解决这个问题？

让我们从回顾建模的一些目的开始。现在这些将侧重于我在地下建模方面的经验，但它们可以转移到任何其他类型的数据科学项目中。

### 建立数值模型/通用地球模型

你经常听到我们建模不是为了建模本身。这大多数情况下是正确的，但有时仅仅构建模型也是有价值的。在这种情况下，我们将模型视为一个数值平台，用于整合所有可用信息，建立对问题设置的统一理解。当我们这样做时，会有很多增值的事情发生：

+   确定已知、未知和关键风险

+   发现存在问题。在前期建模是快速失败方法的一部分，这使我们能够在投入过多时间和资源之前消除调查途径。

+   建立一个沟通工具，确保所有数据都是整合和一致的，并且团队中的每个人都对问题有一个共同的理解。以下是一个基于多种不同数据类型整合的地下模型示例。

![](img/e145993c974b277f008b295b9ea78c06.png)

示例地下模型，将多种类型的数据整合到一个模型中（图片来自 https://www.dgi.com/blog/enhancing-geological-modeling-efforts-via-data-integration/）。

### 评估资源和价值

有时模型支持对价值的评估。对于资源公司来说，资源和储量是它们估值的关键部分。这些模型计算资源的总体积、相关的空间分布和资源回收的时间表。

+   当然，在这种情况下，我们包括的不仅仅是数据，还包括与提取方法相关的工程和地球科学以及市场状况的预测。我们的模型通常跨越多个科学学科，甚至可能被应用于组织外部报告价值。

这种类型的评估在地下资源中很常见，例如，采矿、碳氢化合物提取等。

![图片](img/a440cc89ef17bcb743d0a288f0fe8370.png)

Bingham Canyon 矿，肯内科特犹他铜业（图片来自 https://commons.wikimedia.org/wiki/File:Bingham_Canyon_April_2005.jpg）。由 Timjarrett 在英语维基百科上提供，CC BY-SA 3.0 <http:>, 通过维基媒体共享。</http:>

### 量化资源不确定性

再次强调，我们总是需要整合多个来源和多个尺度的数据，但通常还必须包括所有重要的不确定性来源，以模拟可回收资源的不确定性。

+   这类项目报告不确定性和风险，但也可能被用来指导数据收集以减少不确定性，支持开发决策和/或公开披露。

### 导出统计信息

有时我们建模一个事物是为了帮助建模另一个事物！在这种情况下，我们从一个拥有大量良好数据的成熟地点开发出一套稳健的统计信息，目的是将此模型应用于支持在成熟度较低、样本较少的早期开发地点的决策。在这种情况下，我们假设这两个地点是相似的，这在地质统计学中被称为平稳性假设。

+   导出的统计信息示例包括，分布、相关性、训练图像等。

例如，南非 Laingsburg 的露头已被研究作为全球深海储层的类比。

![图片](img/a675b7dbcc2e04b92989d0dc51f77e1d.png)

南非 Laingsburg 的卫星照片和解释（图片来自 Pohl 等人，2022）{引用}`pohl2022`。

模型的用途有很多。

## 常见机器学习工作流程

一旦我们知道了我们的数据科学建模工作的目的，我们就可以利用常见的建模工作流程。例如，对于许多机器学习项目，我们遵循以下步骤：

1.  **数据清洗** - 数据问题在所难免。缺失数据、错误数据、有偏数据等。通常数据清洗是数据科学项目中的 80% - 90%的工作量。这一步骤需要大量的领域专业知识，所以我不能就你的情况提供太多建议，除了，

+   对这一步骤要有耐心，急于求成地进行数据清洗通常会影响到模型精度，并在之后需要大量重复工作。

+   与整个项目团队进行互动，包括所有了解各种数据类型的专家。不要假设你知道所有必要的元数据。

1.  **特征工程** - 是关于将原始特征转化为最佳形式以构建最佳模型的过程。这可能包括特征转换、特征投影到新的更有用的特征，甚至特征选择。我的建议是，

+   不要在自动驾驶模式下工作，只是将所有可用的特征传递到你的机器学习模型中，并希望模型会处理好。这通常会导致可解释性低，模型精度也较低。

1.  **推理机器学习** - 将推理机器学习应用于从有限的样本数据中学习更多关于总体的情况。这可能包括聚类分析，以推断出单独的总体进行单独建模。

+   这是你探索数据以学习新模式并更新下一步计划（预测建模）的机会

1.  **预测机器学习** - 根据所有之前的工作（即，使用干净的数据、工程化特征和关于总体（s）的推理模型）训练和调整预测机器学习模型。

+   这一步骤受到最多的关注，但它通常只占工作量的 3% - 5%。如果你大部分时间都花在这个步骤上，那应该是一个红旗。

1.  **模型检查和文档** - 我们始终，始终检查我们工作流程中的每个步骤，并在我们构建模型后检查它们。你可能会说，“为什么？我们不是已经训练和调整了我们的模型吗？”你说得对，我们确实非常小心，但我们还没有完成。一些想法：

+   使用可解释的 AI 工具，如 Shapley 值来理解我们模型的行为。

+   使用各种插值和外推情况对模型进行压力测试，以确定模型何时会崩溃。

+   现在，整理出优秀的文档、诊断和培训内容（你也可以引用我的电子书和[我的 YouTube 讲座](https://www.youtube.com/@GeostatsGuyLectures)）。这一步骤对于你工作的最广泛部署和最大影响至关重要。

## 适合目的的建模

无论你构建什么数据科学工作流程，你都需要有一个“适合目的”的心态。这意味着什么？

1.  正如我们讨论的那样，数据科学工作流程设计考虑了工作流程的目标

1.  我们还应该考虑工作流程未来的可能需求，即，它是可扩展的吗？它能否快速更新？如果它在你的组织中很受欢迎，你能否跟上？

1.  此外，我们必须始终考虑到我们组织可用的资源、时间和人员（专业知识）。例如，如果你的组织没有很多数据科学专业知识和计算资源，可以部署一个简单的流程

我们不能拥有一切！这张图片在我的职业生涯中非常有帮助，它是好、快、便宜的图，说明了项目权衡，你只能选择两个，不能三者兼得！

![](img/a6818dcbc418024f1e9e7b94df74dea9.png)

好的、快的、便宜的图（图片来自 https://www.purechat.com/blog/fast-cheap-and-good-the-small-business-guide-to-content-creation/）。

## 建模约束

让我们再详细讨论一下建模约束。每个组织都有局限性，这对于资本管理和盈利能力至关重要。以下是一些常见的约束，

1.  **专业时间** - 可用的工作时间受劳动力、项目数量和项目时间表安排的限制。我们的时间是宝贵的，在组织内部，对每个项目团队成员、全职等效（FTE）在一段时间内的维护成本进行核算。从这一点出发，我们可以考虑哪些活动是增值的，以抵消这种成本。

1.  **组织能力** - 一些组织称之为运营能力，它是项目可用的专业人员的技能组合。是的，你可能在项目团队中有一个世界闻名的深度学习专家，但他们的时间可能被分配到多个项目，以及指导和管理责任。

1.  **计算设施** - 这包括硬件的计算存储和处理，以及项目可用的软件。你可能可以访问特定的数值模拟软件，但由于组织内部共享的座位数量有限，因此很难在大量的 3D 模型空间上运行我们的深度学习卷积模型。

1.  **数据收集** - 你不会得到你想要的所有数据用于你的数据科学项目。样本数量通常有限，你可能没有你想要的全部特征。有时甚至可能对数据的移动、存储和使用进行控制，这可能会限制你的工作。我可以提供一些明智的建议吗？对此要非常小心！始终为任何数据的使用和发布获得许可，包括从数据中衍生出的工作。

1.  **总预算** - 如果把这些都加起来，你就得到了最终的限制，即项目可以花费多少。这是之前所有专业时间、计算资源和数据收集的最终限制。

## 建模策略

存在着各种总体建模策略。我这里只提几个。

### 自上而下的建模

我喜欢这种方法，它非常保守，具有快速失败和适合目的的建模元素。它按照以下步骤进行。

1.  从最简单的模型开始，并使用该模型进行预测

1.  逐步向模型中添加细节和复杂性，预测并访问由于添加复杂性而导致的预测变化

1.  迭代，直到额外的细节和复杂性不会影响预测

对于地质建模，这是从简单的网格框架和一些基本的大规模异质性开始的，例如，主要地质不连续性和障碍。应用数值模拟并评估模型性能。然后在大型框架内添加更多中等规模的异质性，并重复数值模拟和评估，重点关注变化。重复此过程，直到额外的复杂性不会影响数值模拟结果。

![图片](img/dd8e1e5c12aed03743a09a97ce040bfb.png)

从 A 到 C，岸坡-台地砂岩的自上而下建模按细节程度增加（图片来自 Sech 等人，2009 年，可在 https://pubs.geoscienceworld.org/aapg/aapgbull/article/93/9/1155/133006/Three-dimensional-modeling-of-a-shoreface-shelf)获取）。

自上而下建模如何应用于机器学习？从某种意义上说，我们已经在做这件事了，当我们通过测试一个简单的机器学习模型并逐步增加复杂性直到测试错误最小化来调整超参数时。我们可以更进一步，通过询问“什么是最简单的模型，可以让我们得到足够准确的模型”，将自上而下建模整合到我们的通用方法中，因为这种模型可能更容易解释和部署。

### 不适的建模

马克·本特利提出了“[舒适建模](https://watermark.silverchair.com/3.pdf?token=AQECAHi208BE49Ooan9kkhW_Ercy7Dm3ZL_9Cf3qfKAc485ysgAAAx0wggMZBgkqhkiG9w0BBwagggMKMIIDBgIBADCCAv8GCSqGSIb3DQEHATAeBglghkgBZQMEAS4wEQQMXkqBT76yzhoCxu0GAgEQgIIC0LwoyDfGdFuErEV3v7xYI_OmFzO9C1-vLUSf1RBl3leUJcUN63voEj-ldlOOSQGu0BiPhTdaHTiQwRedHpbdO1faIuRVfv6f3hAWEcpCSzzmPNJneZ_EIFsxyVeu5opQvMdg44yVEt335OflgyffMqtQKUGiNtuHruTURiNILh0C8KdO3LNwWvIekiwqeq2U5bqWwBkzc8Ymmj-zuCEIdN6-b3EoqYz3PfyldCqO_2twYgcjrn7_3Cka-zH25zvoMmsw-67KBJGfEBZprfLm5wxBlCmGRtqC6Zzn57x11nDmWcm3TnhmssLnUZwKHJzYAEkkWxPvBF02WFB3IG3tNGKCDK85WHu1Lc8BgfMkGyObDTvZbkIFn7aDBgFwbRfrUw4Q9n_k6ebV9BpLV8qx0JtxbIzLERkLbCxYqWPddTYUng8d8jACg43DZDHBpfube_yR6vdFEGi55O6h3Ufz6Q74RJPpwmV3kw9tofurZLOhXsW9W8bhxzTEE2FsWdj3WmSAVNgydJ52wMSzYtRCJgkrThWvo2R0zUrxRcMct7pntorVdo57SlZ22pD0jnOKVR9dteDloTyFMOCwF45ElBxZMO8bqf69qeoMc1s5dCyiMZsm6RZ9r3bLeJkja9nK5yRCKxLGWS8Ox_L_l7HJ4VkbXXMnt1LHlxTBhYNF2PRxM9CDknKlI19-KjPGSO1KA_AThdkYiK5uZVvrEt5k58EMscfJR-dXBEFepgTlFs5-WqDWzvgngGGHOH3JrnX0GZsiISji_MpVkh5YJTx8DgA3ZNQiVrqFcz5T8hF6qpRJmkMwAPikLYfU9-odb_8WVANlfx5PD07ZI9hLcdvwaE2UcWQyZ-P8KRjB6WIwFhZPkfA3gBcpnd80v53KLD08h0Ze96Vfm7-ELR7FgAU8iu2qRB5kF6hPvsliRung25R_-jhhRUQ-5pFb0Ur4pberrw)”的概念，建议模型可能已经变成了验证已经部分或全部做出的决策的工具。为了对抗这种趋势，本特利建议我们进行不适的建模！

+   压力测试我们当前的概念和决策敏感性

本特利指出，通过这样做，我们可以识别剩余的上行潜力，同时确保应对最坏情况。此外，这也是一种识别和缓解我们偏见的好方法。

我把这个称为《神探夏洛克》方法，这是 Jaime 和 Adam 主持的科学和工程电视节目。我的孩子们在观看[Mythbusters](https://en.wikipedia.org/wiki/MythBusters)时长大，我也很喜欢这个节目。他们从电影或历史中取材，进行测试。例如，

+   集中许多抛光的盾牌是否会点燃一艘木制船只？

+   在驾驶时倒车是否会造成变速箱掉落？

+   高音是否能打破玻璃？

我们如何在模型构建中应用不适感建模或 Mythbusters 方法。简单，对你的模型进行压力测试。考虑以下，

+   你的模型预测得非常好，但你能否找到模型不准确的情况

+   你的模型对异常值具有鲁棒性，添加更多异常值并观察这种鲁棒性的极限

+   你的模型在减少问题维度的同时保留信息，添加更多维度

+   你的模型在 2D 中运行得足够快，尝试在 3D 中运行

这相当于 Jaime 和 Adam 没有看到失败并决定增加更多压力、更多速度、更多...直到它失败。

![](img/1bf4066793a3ea4ca42c2463ff0e9d3a.png)

Mythbusters 中的 Jaime 和 Adam 尝试通过集中许多抛光的盾牌来点燃一艘木制船只（图片来自 NPR 关于 Mythbusters 的故事，https://www.npr.org/2010/12/06/131853963/obama-challenges-mythbusters-adam-and-Jamie）。

## 在 Python 中学习编码

我已经教授了数百名工程师和地球科学家数据科学，其中许多人没有编程经验，现在他们正在使用 Python 进行数据科学。这太棒了，我很高兴能提供帮助。为了支持这些学生，我开发了一系列有用的资源，

1.  使用 GeostatsGuy 的 Python 数据科学基础代码讲解

我选择了机器学习工作流程的一些构建块，并构建了现场代码演示（使用 [Rise](https://rise.readthedocs.io/en/latest/) Python 包）并录制了我的讲解，并将其作为 [播放列表](https://www.youtube.com/playlist?list=PLG19vXLQHvSAufDFgZEFAYQEwMJXklnQV)[Pyr24b] 包括在我的 [YouTube 频道](https://www.youtube.com/GeostatsGuyLectures)上。任何人都可以打开这些工作流程（评论中的链接并跟随）。

![](img/7bd1d57ac52f789f71a0e921a914f880.png)

使用 GeostatsGuy 的 Python 数据科学基础代码讲解，YouTube 上的播放列表。

1.  YouTube 上的机器学习课程

我整个机器学习课程都可以在我的[YouTube 频道](https://www.youtube.com/GeostatsGuyLectures)[Pyr24c][Pyr24b]上找到，[播放列表](https://www.youtube.com/playlist?list=PLG19vXLQHvSC2ZKFIkgVpI9fCjkN38kwf)。这不仅包括所有录制的讲座，还包括评论中链接到的所有代码，包括经过良好文档化的工作流程和 Python 中的交互式仪表板。我提供了许多与数据打交道的真实示例，构建机器学习模型，可视化并检查数据和模型等。

1.  Python 中的应用机器学习，动手指南及代码

当然，这本书充满了资源，包括大量的示例代码。实际上，在许多情况下，我添加了额外的注释和描述，以帮助那些刚开始编码的人。

你能行

我看到很多人很快就学会了基本的编码技能来构建机器学习工作流程。

## 评论

这是对机器学习工作流程构建和编码的基本描述。还有更多可以做的和讨论的，我有很多更多的资源。查看我的[共享资源清单](https://michaelpyrcz.com/my-resources)以及本章开头提供的 YouTube 讲座链接，视频描述中包含资源链接。

希望这有所帮助，

*迈克尔*

## 关于作者

![](img/eb709b2c0a0c715da01ae0165efdf3b2.png)

迈克尔·皮尔茨教授在德克萨斯大学奥斯汀分校 40 英亩校园的办公室。

迈克尔·皮尔茨（Michael Pyrcz）是德克萨斯大学奥斯汀分校[科克雷尔工程学院](https://cockrell.utexas.edu/faculty-directory/alphabetical/p)和[杰克逊地球科学学院](https://www.jsg.utexas.edu/researcher/michael_pyrcz/)的教授，他在那里研究并教授地下、空间数据分析、地统计学和机器学习。迈克尔还是，

+   [能源分析](https://fri.cns.utexas.edu/energy-analytics)新生研究项目的负责人，德克萨斯大学奥斯汀分校自然科学院机器学习实验室的核心教员

+   [计算机与地球科学](https://www.sciencedirect.com/journal/computers-and-geosciences/about/editorial-board)的副编辑，以及国际数学地球科学协会[数学地球科学](https://link.springer.com/journal/11004/editorial-board)的董事会成员。

迈克尔已经撰写了超过 70 篇[同行评审的出版物](https://scholar.google.com/citations?user=QVZ20eQAAAAJ&hl=en)，一个用于空间数据分析的[Python 包](https://pypi.org/project/geostatspy/)，合著了一本关于空间数据分析的教科书《[地统计学储层建模](https://www.amazon.com/Geostatistical-Reservoir-Modeling-Michael-Pyrcz/dp/0199731446)》，并且是两本近期发布的电子书的作者，分别是《[Python 应用地统计学：GeostatsPy 实践指南](https://geostatsguy.github.io/GeostatsPyDemos_Book/intro.html)》和《[Python 应用机器学习：代码实践指南](https://geostatsguy.github.io/MachineLearningDemos_Book/intro.html)》。

迈克尔的所有大学讲座都可以在他的[YouTube 频道](https://www.youtube.com/@GeostatsGuyLectures)上找到，其中包含 100 多个 Python 交互式仪表板和 40 多个存储库中的详细记录工作流程，这些存储库位于他的[GitHub 账户](https://github.com/GeostatsGuy)，以支持任何有兴趣的学生和在职专业人士，提供持续更新的内容。了解更多关于迈克尔的工作和共享教育资源，请访问他的网站。

## 想要一起工作吗？

我希望这些内容对那些想了解更多关于地下建模、数据分析和机器学习的人有所帮助。学生和在职专业人士都欢迎参加。

+   想邀请我到贵公司进行培训、辅导、项目审查、工作流程设计和/或咨询吗？我很乐意拜访并与您合作！

+   感兴趣合作，支持我的研究生研究或我的地下数据分析与机器学习联盟（共同负责人包括 Foster 教授、Torres-Verdin 教授和 van Oort 教授）吗？我的研究将数据分析、随机建模和机器学习理论与实践相结合，以开发新的方法和工作流程，增加价值。我们正在解决具有挑战性的地下问题！

+   您可以通过 mpyrcz@austin.utexas.edu 联系我。

我总是很高兴讨论，

*迈克尔*

迈克尔·皮尔奇，博士，P.Eng. 教授，德克萨斯大学奥斯汀分校 Cockrell 工程学院和 Jackson 地球科学学院

更多资源可在以下位置获取：[Twitter](https://twitter.com/geostatsguy) | [GitHub](https://github.com/GeostatsGuy) | [网站](http://michaelpyrcz.com) | [Google Scholar](https://scholar.google.com/citations?user=QVZ20eQAAAAJ&hl=en&oi=ao) | [地统计学书籍](https://www.amazon.com/Geostatistical-Reservoir-Modeling-Michael-Pyrcz/dp/0199731446) | [Python 应用地统计学电子书](https://geostatsguy.github.io/GeostatsPyDemos_Book/intro.html) | [Python 应用机器学习电子书](https://geostatsguy.github.io/MachineLearningDemos_Book/) | [LinkedIn](https://www.linkedin.com/in/michael-pyrcz-61a648a1)

## 构建机器学习工作流程和编码的动机

你可以学习数据科学和机器学习的理论。完全理解概率、统计学、推理和预测，超参数调优和模型检查的交叉验证，但为了增加价值，我们必须编写实用、适合目的的工作流程，以便广泛部署和使用。我已经做了很多年，我的目标是分享一些我的经验，帮助你开始，或者磨练你的技能！

如果你还没有开始编程，请阅读我为什么认为所有科学家和工程师都应该学习一些编程的理由。我经常在社交媒体和我的课程中分享这些内容。

## 所有科学家和工程师学习一些编程的理由

这是皮尔茨教授对所有科学家和工程师学习一些编程的理由。

1.  **透明度** – 没有编译器会接受挥手！编程迫使你的逻辑被任何其他科学家或工程师审查。

1.  **可重复性** – 运行它并得到答案，交给一个同行，他们运行它并得到相同的答案。这是科学方法的一个原则。

1.  **量化** – 程序需要数字，并推动我们从定性转向定量。给程序输入，发现新的看待世界的方法。

1.  **开源** – 利用世界的智慧。查看包、片段，并惊叹于伟大思想者免费分享的内容。

1.  **打破壁垒** – 不要把它扔过篱笆。和开发者坐在一起，分享更多你的专业知识，以获得更好的部署产品。

1.  **部署** – 与他人分享你的代码，扩大你的影响力。性能指标或利他主义，你的好工作使许多人受益。

1.  **效率** – 最小化工作中无聊的部分。为常见任务自动化构建脚本集，花更多时间进行科学和工程！

1.  **总是有再次做它的时间**！你只做了一次吗？脚本和自动化工作流程可能需要 2-4 倍的时间。通常，这是值得的。

1.  **像我们一样** – 这将改变你。用户会感到受限，程序员真正利用了他们的应用程序和硬件的力量。

我有数十年的 Basic、Fortran、Visual Basic for Applications (VBA)、C++、R 和 Python 经验。我在中学时第一次编写的代码是在德州仪器的 TI-99/4A 家用电脑上用 Basic 编写的，并存储在磁带上。我做过很多数值原型、工作流程、脚本、仪表盘，甚至几年的全栈开发。让我这样来说，"现成的软件"让我感到被困住，我立刻寻找应用程序编程接口 (API)，以便我可以逃脱并编写自己的方法、仪表盘和可视化。然而，我对编程仍然非常实际和谦逊；因此，我的建议是学习你需要完成工作的内容。以下是一些替代方案：

+   学习在现成软件中使用工作流程自动化功能。通常，文档都相当不错，这种方法不仅为你的工作流程提供了审计跟踪，而且也符合你组织的流程标准和 IT 部门政策。

+   如果现成软件接受 Python、Java、控制语言，那么学习这些语言的基本知识以与工作流程中的对象交互。这通常会大大提高你创建增值工作流程的灵活性。

+   许多组织开始使用 Jupyter Notebooks 和 Jupyter Lab 等工具在 Python 中原型设计和部署工作流程。如果这样，学习一些 Python 基础知识，开始使用像 Numpy、Pandas、Matplotlib 和 SciPy 这样的基本 Python 包。上网并使用 Google 搜索，让 ChatGPT 帮助你开发工作流程。有如此多的资源可以帮助你，包括这本电子书以及我在本章中分享的编码基础知识。

不要感到害怕，我不希望你（或诅咒你）过上全栈开发的生活。那些是搜索 C++中的内存泄漏和悬挂指针的日子。在经历了所有这些经历之后，我说的是：

使用 Python

我编写的代码更少，但完成的工作更多！

## 建模目的

任何数据科学项目的第一步是提出问题，这项工作的目的是什么？为了解决这个问题，我们必须退后一步并问：

1.  问题是什

1.  别人为了解决这个问题都做了什么？

1.  我们将如何解决这个问题？

1.  我们可以部署什么来解决这个问题？

让我们从回顾一些建模的目的开始。现在这些将侧重于我在地下建模方面的经验，但它们可以转移到任何其他类型的数据科学项目中。

### 建立数值模型/通用地球模型

你经常听到我们建模不是为了建模本身。这大多数情况下是正确的，但有时仅仅构建模型也是有价值的。在这种情况下，我们将模型视为一个数值平台，用于整合所有可用信息，建立对问题设置的统一理解。当我们这样做时，会有很多增值的事情发生：

+   确定已知、未知和关键风险

+   发现存在问题。建模是快速失败方法的一部分，它允许我们在投入太多时间和资源之前消除调查途径。

+   构建一个沟通工具，确保所有数据都是整合和一致的，并且团队中的每个人都对问题有一个共同的理解。以下是一个基于多种不同数据类型整合的地下模型示例。

![](img/e145993c974b277f008b295b9ea78c06.png)

示例地下模型，将多种类型的数据整合到一个模型中（图片来自 https://www.dgi.com/blog/enhancing-geological-modeling-efforts-via-data-integration/）。

### 评估资源和价值

有时模型支持对价值的评估。对于资源公司来说，资源和储量是它们估值的关键部分。这些模型计算资源的大致体积、相关的空间分布和资源回收的时间表。

+   当然，在这种情况下，我们包括的不仅仅是数据，还包括与提取方法相关的工程和地球科学以及市场状况的预测。我们的模型通常跨越多个科学学科，甚至可能被应用于报告我们组织之外的价值。

这种类型的评估在地下资源中很常见，例如，采矿、碳氢化合物提取等。

![](img/a440cc89ef17bcb743d0a288f0fe8370.png)

Bingham Canyon Mine，肯内科特犹他铜矿（图片来自 https://commons.wikimedia.org/wiki/File:Bingham_Canyon_April_2005.jpg）。由 Timjarrett 在英语维基百科上，CC BY-SA 3.0 <http:>, 通过维基媒体共享。</http:>

### 量化资源不确定性

再次强调，我们必须始终整合多个来源和多个尺度的数据，但通常我们还需要包括所有重要的不确定性来源，以模拟可回收资源的不确定性。

+   这类项目报告不确定性和风险，但也可用于指导数据收集以减少不确定性，支持开发决策和/或公开披露

### 导出统计数据

有时我们建模一个东西是为了帮助建模另一个东西！在这种情况下，我们从一个数据丰富的成熟地点开发出一套稳健的统计数据，目的是将此模型应用于支持在成熟度较低、样本较少的早期开发地点的决策。在这种情况下，我们假设这两个地点是相似的，这在地质统计学中被称为平稳性假设。

+   导出的统计数据示例包括，分布、相关性、训练图像等。

例如，南非 Laingsburg 的露头已被研究作为全球深水储层的类比。

![](img/a675b7dbcc2e04b92989d0dc51f77e1d.png)

南非 Laingsburg 露头的卫星照片和解释（图片来自 Pohl 等人，2022）{引用}`pohl2022`。

建模有许多其他目的。

### 建立数值模型/通用地球模型

你经常听到我们建模不是为了建模。这大多数情况下是正确的，但有时仅仅构建模型也是有价值的。在这种情况下，我们将模型视为一个数值平台，用于整合所有可用信息，构建对问题设置的统一理解。当我们这样做时，会有很多增值的事情发生：

+   确定已知、未知和关键风险

+   发现存在问题。在前期建模是快速失败方法的一部分，这使我们能够在投入过多时间和资源之前消除调查途径。

+   建立一个沟通工具，确保所有数据都是整合和一致的，并且团队中的每个人都对问题有一个共同的理解。以下是一个基于多种不同数据类型整合的地下模型示例。

![图片](img/e145993c974b277f008b295b9ea78c06.png)

示例：将多种类型的数据整合到一个模型中的地下模型（图片来自 https://www.dgi.com/blog/enhancing-geological-modeling-efforts-via-data-integration/）。

### 评估资源和价值

有时模型支持价值的评估。对于资源公司来说，资源和储量是它们估值的关键部分。这些模型计算资源的总体积、相关的空间分布和资源回收的时间表。

+   当然，在这种情况下，我们包括的数据远不止这些，还包括与提取方法相关的工程和地球科学以及市场状况的预测。我们的模型通常跨越多个科学学科，甚至可能被应用于报告我们组织之外的价值。

这些类型的评估在地下资源中很常见，例如，采矿、碳氢化合物提取等。

![图片](img/a440cc89ef17bcb743d0a288f0fe8370.png)

Bingham Canyon Mine，肯内科特犹他铜矿（图片来自 https://commons.wikimedia.org/wiki/File:Bingham_Canyon_April_2005.jpg）。由 Timjarrett 在英语维基百科上，CC BY-SA 3.0 <http:>, 通过维基媒体共享。</http:>

### 量化资源的不确定性

再次强调，我们必须始终整合多个来源和多个尺度的数据，但通常还必须包括所有重要的不确定性来源，以对可回收资源的不确定性进行建模。

+   这些类型的项目报告不确定性和风险，但也可能用于指导数据收集以减少不确定性，支持开发决策和/或公开披露

### 导出统计数据

有时我们建模一个事物是为了帮助建模另一个事物！在这种情况下，我们从一个拥有大量良好数据的成熟地点开发出一套稳健的统计数据，目的是将此模型应用于支持在成熟度较低、样本较少的早期开发地点的决策。在这种情况下，我们假设这两个地点是相似的，这在地质统计学中被称为平稳性假设。

+   导出的统计示例包括分布、相关性、训练图像等。

例如，南非 Laingsburg 的露头已被研究作为全球深水储层的类比。

![图片](img/a675b7dbcc2e04b92989d0dc51f77e1d.png)

南非 Laingsburg 露头的卫星照片和解释（图片来自 Pohl 等人，2022 年）{引用}`pohl2022`。

模型的用途有很多。

## 常见的机器学习工作流程

一旦我们知道了我们的数据科学建模工作的目的，我们就可以利用常见的建模工作流程。例如，对于许多机器学习项目，我们遵循以下步骤：

1.  **数据清洗** - 数据问题在所难免。缺失数据、错误数据、有偏数据等。通常，数据清洗是数据科学项目中 80% - 90%的工作量。这一步骤需要大量的领域专业知识，所以我不能就你的情况提供太多建议，除了，

+   对这一步骤要有耐心，急于进行数据清洗通常会影响到模型精度，并在以后需要大量重复工作。

+   与整个项目团队进行合作，包括所有了解各种数据类型的专家。不要假设你知道所有必要的元数据。

1.  **特征工程** - 是关于将原始特征转化为最佳形式，以构建最佳模型。这可能包括特征转换、特征投影到新的更有用的特征，甚至特征选择。我的建议是，

+   不要在自动驾驶模式下工作，只是将所有可用的特征传递到你的机器学习模型中，并希望模型能将其整理好。这通常会导致可解释性低，模型精度降低。

1.  **推断机器学习** - 应用推断机器学习从有限的样本数据中学习更多关于总体的信息。这可能包括聚类分析，以推断出单独的总体来分别建模。

+   这是你探索数据以学习新模式并更新下一步计划（预测建模）的机会

1.  **预测机器学习** - 基于所有之前的工作，即使用干净的数据、工程化特征和关于总体（们）的推断模型来训练和调整预测机器学习模型。

+   这一步骤受到最多的关注，但它通常只占工作量的 3% - 5%。如果你大部分时间都花在这一步骤上，那应该是一个红旗。

1.  **模型检查和文档** - 我们总是，总是检查我们工作流程中的每一个步骤，并在我们构建模型后检查它们。你可能会说，“为什么？我们不是已经训练和调整了我们的模型吗？”你说得对，我们确实非常小心，但我们还没有完成。一些想法：

+   使用可解释的 AI 工具，如 Shapley 值，来理解我们模型的行为。

+   使用各种插值和外推案例来测试你的模型，以确定模型何时会崩溃。

+   现在，整理出优秀的文档、诊断和培训内容（你也可以引用我的电子书和[我的 YouTube 讲座](https://www.youtube.com/@GeostatsGuyLectures)）。这一步骤对于你工作的最广泛部署和最大影响至关重要。

## 适合目的的建模

无论你构建什么数据科学工作流程，你都需要有一个“适合目的”的心态。这意味着什么？

1.  正如我们讨论的那样，数据科学工作流程设计考虑了工作流程的目标

1.  我们还应该考虑工作流程未来的可能需求，即它是否可扩展？能否快速更新？如果它在你的组织中成为热门，你是否能够跟上？

1.  此外，我们必须始终考虑我们组织可用的资源、时间和人员（专业知识）。例如，如果你的组织没有很多数据科学专业知识和计算资源，可以部署一个简单的流程。

我们不可能拥有一切！这幅图在我的职业生涯中非常有帮助，它是好、快、便宜的图示，说明了项目权衡，你只能选择两个，不能三者兼得！

![](img/a6818dcbc418024f1e9e7b94df74dea9.png)

好的、快的、便宜的图示（图片来自 https://www.purechat.com/blog/fast-cheap-and-good-the-small-business-guide-to-content-creation/）。

## 建模约束

让我们再深入讨论一下建模约束。每个组织都有其局限性，这对于资本管理和盈利能力至关重要。以下是一些常见的约束条件，

1.  **专业时间** - 可用的工作时间受劳动力、项目数量和项目时间表安排的限制。我们的时间是宝贵的，在组织内部，每个项目团队成员（全职等效，FTE）在一定时期内的维护成本是有所记录的。从这个角度来看，我们可以思考哪些活动能够增加价值以抵消这些成本。

1.  **组织能力** - 一些组织称之为运营能力，它是指可供项目使用的专业人员的技能组合。是的，你可能在项目团队中有一个世界知名的深度学习专家，但他们的时间可能被分配在多个项目上，同时还要承担导师和领导职责。

1.  **计算设施** - 这包括硬件的计算存储和处理，以及项目可用的软件。你可能有权访问特定的数值模拟软件，但组织内部共享的座位数量有限，因此很难在巨大的 3D 模型空间上运行我们的深度学习卷积模型。

1.  **数据收集** - 你可能无法获得你想要的所有数据用于你的数据科学项目。样本数量通常有限，你可能没有你想要的全部特征。有时甚至可能对数据的移动、存储和使用有控制，这可能会限制你的工作。我可以提供一些明智的建议吗？对此要非常小心！始终确保任何数据的使用和发布都有权限，包括从数据中衍生出的工作。

1.  **总预算** - 如果将这些因素全部加起来，你得到的是项目的最终限制，即项目可以花费多少。这是对之前所有因素（专业时间、计算资源、数据收集）的最终限制。

## 建模策略

存在着各种全面的建模策略。在这里，我只提几个。

### 自上而下的建模

我喜欢这种方法，它非常保守，具有快速失败和适合目的的建模元素。它按照以下步骤进行。

1.  从最简单的模型开始，并使用模型进行预测

1.  逐步增加模型的细节和复杂性，预测并评估由于增加的复杂性导致的预测变化

1.  迭代直到增加的细节和复杂性不会影响预测

对于地质建模，这是通过从简单的网格框架和一些基本的大规模异质性开始应用的，例如，主要地质不连续性和障碍。应用数值模拟并评估模型性能。然后在大型框架内添加更多中等规模的异质性，并重复数值模拟和评估，重点关注变化。重复此过程，直到增加的复杂性不会影响数值模拟结果。

![图片](img/dd8e1e5c12aed03743a09a97ce040bfb.png)

从 A 到 C 的水平向建模，沙岸-平台砂岩比例，随着细节级别的增加（图片来自 Sech 等人，2009 年，可在 https://pubs.geoscienceworld.org/aapg/aapgbull/article/93/9/1155/133006/Three-dimensional-modeling-of-a-shoreface-shelf 上找到）。

顶向下建模如何应用于机器学习？以一种我们已经做到的方式，当我们通过测试一个简单的机器学习模型并逐步增加复杂性直到测试错误最小化来调整超参数时。我们可以更进一步，通过询问“什么是最简单的模型，它能让我们得到足够准确的模型”，因为这样的模型可能更容易解释并且更易于部署。

### 不适感建模

马克·本特利提出了“[不适感建模](https://watermark.silverchair.com/3.pdf?token=AQECAHi208BE49Ooan9kkhW_Ercy7Dm3ZL_9Cf3qfKAc485ysgAAAx0wggMZBgkqhkiG9w0BBwagggMKMIIDBgIBADCCAv8GCSqGSIb3DQEHATAeBglghkgBZQMEAS4wEQQMXkqBT76yzhoCxu0GAgEQgIIC0LwoyDfGdFuErEV3v7xYI_OmFzO9C1-vLUSf1RBl3leUJcUN63voEj-ldlOOSQGu0BiPhTdaHTiQwRedHpbdO1faIuRVfv6f3hAWEcpCSzzmPNJneZ_EIFsxyVeu5opQvMdg44yVEt335OflgyffMqtQKUGiNtuHruTURiNILh0C8KdO3LNwWvIekiwqeq2U5bqWwBkzc8Ymmj-zuCEIdN6-b3EoqYz3PfyldCqO_2twYgcjrn7_3Cka-zH25zvoMmsw-67KBJGfEBZprfLm5wxBlCmGRtqC6Zzn57x11nDmWcm3TnhmssLnUZwKHJzYAEkkWxPvBF02WFB3IG3tNGKCDK85WHu1Lc8BgfMkGyObDTvZbkIFn7aDBgFwbRfrUw4Q9n_k6ebV9BpLV8qx0JtxbIzLERkLbCxYqWPddTYUng8d8jACg43DZDHBpfube_yR6vdFEGi55O6h3Ufz6Q74RJPpwmV3kw9tofurZLOhXsW9W8bhxzTEE2FsWdj3WmSAVNgydJ52wMSzYtRCJgkrThWvo2R0zUrxRcMct7pntorVdo57SlZ22pD0jnOKVR9dteDloTyFMOCwF45ElBxZMO8bqf69qeoMc1s5dCyiMZsm6RZ9r3bLeJkja9nK5yRCKxLGWS8Ox_L_l7HJ4VkbXXMnt1LHlxTBhYNF2PRxM9CDknKlI19-KjPGSO1KA_AThdkYiK5uZVvrEt5k58EMscfJR-dXBEFepgTlFs5-WqDWzvgngGGHOH3JrnX0GZsiISji_MpVkh5YJTx8DgA3ZNQiVrqFcz5T8hF6qpRJmkMwAPikLYfU9-odb_8WVANlfx5PD07ZI9hLcdvwaE2UcWQyZ-P8KRjB6WIwFhZPkfA3gBcpnd80v53KLD08h0Ze96Vfm7-ELR7FgAU8iu2qRB5kF6hPvsliRung25R_-jhhRUQ-5pFb0Ur4pberrw)”的概念，建议模型可能已经成为验证已经部分或全部做出的决策的工具。为了对抗这种趋势，本特利建议我们进行不适感建模！

+   压力测试我们当前的概念和决策敏感性

本特利表示，通过这样做，我们可以识别剩余的潜在优势，同时确保应对最坏情况。此外，这也是识别和减轻我们偏见的一个很好的方法。

我将这种方法称为《神探夏洛克》方法，这个名字来源于 Jaime 和 Adam 参与的科学与工程电视节目。我的孩子们在观看[《神探夏洛克》](https://en.wikipedia.org/wiki/MythBusters)时长大，我也很喜欢这个节目。他们从电影或历史中取材进行测试。例如，

+   集中许多抛光的盾牌能否点燃一艘木船

+   在驾驶时将汽车挂入倒挡会导致变速箱掉落吗？

+   高音能打破玻璃吗？

我们如何在模型构建中应用建模以应对不适或《神探夏洛克》的方法。简单来说，就是对你的模型进行压力测试。考虑以下情况，

+   你的模型预测得非常好，但你能否找到模型不准确的情况

+   你的模型对异常值具有鲁棒性，添加更多异常值并观察这种鲁棒性的极限

+   在减少问题维度的同时，你的模型保留了信息，添加更多维度

+   你的模型在 2D 中运行得足够快，尝试在 3D 中运行

这相当于 Jaime 和 Adam 未能看到失败并决定增加更多压力、更多速度、更多...直到它失败。

![图片](img/1bf4066793a3ea4ca42c2463ff0e9d3a.png)

来自《神探夏洛克》的杰伊姆和亚当试图通过集中许多抛光的盾牌来点燃一艘木船（图片来自 NPR 关于《神探夏洛克》的故事，链接：https://www.npr.org/2010/12/06/131853963/obama-challenges-mythbusters-adam-and-Jamie）。

### 自上而下的建模

我喜欢这种方法，它非常保守，具有快速失败和适合目的的建模元素。它按照以下步骤进行。

1.  从最简单的模型开始，并使用该模型进行预测

1.  逐步增加模型的细节和复杂性，预测并评估由于增加的复杂性导致的预测变化

1.  迭代直到增加的细节和复杂性不会影响预测

对于地质建模，这是通过从简单的网格框架和一些基本的大规模异质性开始来应用的，例如，主要地质不连续性和障碍物。应用数值模拟并评估模型性能。然后在大型框架内添加更多中等规模的异质性，并重复数值模拟和评估，重点关注变化。重复此过程，直到增加的复杂性不会影响数值模拟结果。

![图片](img/dd8e1e5c12aed03743a09a97ce040bfb.png)

从 A 到 C，以不断增加的细节水平对顶面-平台砂岩进行自上而下的建模（图片来自 Sech 等人，2009 年的研究，链接：https://pubs.geoscienceworld.org/aapg/aapgbull/article/93/9/1155/133006/Three-dimensional-modeling-of-a-shoreface-shelf）。

自上而下的建模如何应用于机器学习？从某种意义上说，我们已经在做了，当我们通过测试一个简单的机器学习模型并逐步增加复杂性直到测试错误最小化来调整超参数。我们可以更进一步，通过询问“什么是最简单的模型，它能让我们得到足够准确的模型”，将自上而下的建模整合到我们的通用方法中，因为这种模型可能更容易解释并且更易于部署。

### 建模不适

马克·本特利提出了“[建模舒适](https://watermark.silverchair.com/3.pdf?token=AQECAHi208BE49Ooan9kkhW_Ercy7Dm3ZL_9Cf3qfKAc485ysgAAAx0wggMZBgkqhkiG9w0BBwagggMKMIIDBgIBADCCAv8GCSqGSIb3DQEHATAeBglghkgBZQMEAS4wEQQMXkqBT76yzhoCxu0GAgEQgIIC0LwoyDfGdFuErEV3v7xYI_OmFzO9C1-vLUSf1RBl3leUJcUN63voEj-ldlOOSQGu0BiPhTdaHTiQwRedHpbdO1faIuRVfv6f3hAWEcpCSzzmPNJneZ_EIFsxyVeu5opQvMdg44yVEt335OflgyffMqtQKUGiNtuHruTURiNILh0C8KdO3LNwWvIekiwqeq2U5bqWwBkzc8Ymmj-zuCEIdN6-b3EoqYz3PfyldCqO_2twYgcjrn7_3Cka-zH25zvoMmsw-67KBJGfEBZprfLm5wxBlCmGRtqC6Zzn57x11nDmWcm3TnhmssLnUZwKHJzYAEkkWxPvBF02WFB3IG3tNGKCDK85WHu1Lc8BgfMkGyObDTvZbkIFn7aDBgFwbRfrUw4Q9n_k6ebV9BpLV8qx0JtxbIzLERkLbCxYqWPddTYUng8d8jACg43DZDHBpfube_yR6vdFEGi55O6h3Ufz6Q74RJPpwmV3kw9tofurZLOhXsW9W8bhxzTEE2FsWdj3WmSAVNgydJ52wMSzYtRCJgkrThWvo2R0zUrxRcMct7pntorVdo57SlZ22pD0jnOKVR9dteDloTyFMOCwF45ElBxZMO8bqf69qeoMc1s5dCyiMZsm6RZ9r3bLeJkja9nK5yRCKxLGWS8Ox_L_l7HJ4VkbXXMnt1LHlxTBhYNF2PRxM9CDknKlI19-KjPGSO1KA_AThdkYiK5uZVvrEt5k58EMscfJR-dXBEFepgTlFs5-WqDWzvgngGGHOH3JrnX0GZsiISji_MpVkh5YJTx8DgA3ZNQiVrqFcz5T8hF6qpRJmkMwAPikLYfU9-odb_8WVANlfx5PD07ZI9hLcdvwaE2UcWQyZ-P8KRjB6WIwFhZPkfA3gBcpnd80v53KLD08h0Ze96Vfm7-ELR7FgAU8iu2qRB5kF6hPvsliRung25R_-jhhRUQ-5pFb0Ur4pberrw)”的概念，建议模型可能已经变成了验证已经部分或全部做出的决策的工具。为了对抗这种趋势，本特利建议我们进行建模不适！

+   测试我们当前的概念和决策的敏感性

本特利指出，通过这样做，我们可以识别剩余的潜在优势，同时确保应对最坏情况。这也是一种识别和减轻我们偏见的好方法。

我把这个称为《神探夏洛克》方法，这是 Jaime 和 Adam 主持的科学和工程电视节目。我的孩子们在观看[《神探夏洛克》](https://en.wikipedia.org/wiki/MythBusters)时长大，我也很喜欢这个节目。他们从电影或历史中取材，进行测试。例如，

+   集中许多磨光的盾牌会点燃一艘木船吗？

+   将汽车倒车时挂入倒挡会导致变速箱掉出来吗？

+   高音能打破玻璃吗？

我们如何在模型构建中应用建模不适或《神探夏洛克》方法。简单来说，就是对你的模型进行压力测试。考虑以下，

+   你的模型预测得非常好，但你能否找到一些模型不准确的情况？

+   你的模型对异常值有鲁棒性，添加更多异常值并观察这种鲁棒性的极限

+   如果你的模型在减少问题维度的同时保留了信息，添加更多维度

+   如果你的模型在 2D 中运行得足够快，尝试在 3D 中运行它

这就像是 Jaime 和 Adam 没有看到失败，决定增加更多压力、更多速度、更多……直到它失败。

![图片](img/1bf4066793a3ea4ca42c2463ff0e9d3a.png)

来自《迷雾探秘》的 Jaime 和 Adam 试图通过集中许多抛光的盾牌来点燃一艘木船（图片来自 NPR 关于《迷雾探秘》的故事，https://www.npr.org/2010/12/06/131853963/obama-challenges-mythbusters-adam-and-Jamie）。

## 在 Python 中学习编程

我已经教授了数百名工程师和地球科学家数据科学，其中许多人之前没有编程经验，现在正在使用 Python 进行数据科学。这太棒了，我很高兴能帮助他们。为了支持这些学生，我开发了一系列有用的资源，

1.  与 GeostatsGuy 一起在 Python 中进行数据科学基础代码讲解

我选择了机器学习工作流程的一些基本模块，并构建了现场代码演示（使用[Rise](https://rise.readthedocs.io/en/latest/) Python 包）并录制了我的讲解，将其作为[播放列表](https://www.youtube.com/playlist?list=PLG19vXLQHvSAufDFgZEFAYQEwMJXklnQV)[Pyr24b]发布在我的[YouTube 频道](https://www.youtube.com/GeostatsGuyLectures)。任何人都可以打开这些工作流程（评论中的链接），并跟随学习。

![图片](img/7bd1d57ac52f789f71a0e921a914f880.png)

与 GeostatsGuy 一起在 Python 中进行数据科学基础代码讲解的播放列表。

1.  YouTube 上的机器学习课程

我的整个机器学习课程都可以在我的[YouTube 频道](https://www.youtube.com/GeostatsGuyLectures)[Pyr24c]上的[播放列表](https://www.youtube.com/playlist?list=PLG19vXLQHvSC2ZKFIkgVpI9fCjkN38kwf)[Pyr24b]找到。这不仅包括所有录制的讲座，还包括评论中的所有代码链接，包括经过良好文档化的工作流程和 Python 中的交互式仪表板。我提供了许多与数据交互、构建机器学习模型、可视化并检查数据和模型等现实世界的示例。

1.  《Python 应用机器学习：动手实践指南及代码》

当然，这本书充满了资源，包括大量的示例代码。实际上，在许多情况下，我添加了额外的注释和描述，以帮助那些编程新手。

你能行

我看到很多人很快就学会了基本的编码技能来构建机器学习工作流程。

## 评论

这是对机器学习工作流程构建和编码的基本描述。可以做和讨论的还有很多，我有很多更多的资源。查看我的[共享资源清单](https://michaelpyrcz.com/my-resources)以及本章开头带有资源链接的视频讲座 YouTube 链接。

我希望这有所帮助，

*迈克尔*

## 关于作者

![](img/eb709b2c0a0c715da01ae0165efdf3b2.png)

迈克尔·皮尔奇教授在德克萨斯大学奥斯汀分校 40 英亩校园的办公室。

迈克尔·皮尔奇（Michael Pyrcz）是[德克萨斯大学奥斯汀分校的 Cockrell 工程学院](https://cockrell.utexas.edu/faculty-directory/alphabetical/p)和[德克萨斯大学奥斯汀分校的 Jackson 地球科学学院](https://www.jsg.utexas.edu/researcher/michael_pyrcz/)的教授，在那里他研究并教授地下、空间数据分析、地质统计学和机器学习。迈克尔还是，

+   [能源分析](https://fri.cns.utexas.edu/energy-analytics)新生研究项目的首席研究员，以及德克萨斯大学奥斯汀分校自然科学学院机器学习实验室的核心教员。

+   [《计算机与地球科学》](https://www.sciencedirect.com/journal/computers-and-geosciences/about/editorial-board)的副编辑，以及国际数学地球科学协会[《数学地球科学》](https://link.springer.com/journal/11004/editorial-board)的董事会成员。

迈克尔·皮尔奇（Michael Pyrcz）撰写了超过 70 篇[同行评审的出版物](https://scholar.google.com/citations?user=QVZ20eQAAAAJ&hl=en)，一个用于空间数据分析的[Python 包](https://pypi.org/project/geostatspy/)，合著了一本关于空间数据分析的教科书《[地质统计学储层建模](https://www.amazon.com/Geostatistical-Reservoir-Modeling-Michael-Pyrcz/dp/0199731446)》，并是两本最近发布的电子书的作者，分别是《[Python 中的应用地质统计学：GeostatsPy 实践指南](https://geostatsguy.github.io/GeostatsPyDemos_Book/intro.html)》和《[Python 中的应用机器学习：带代码的实践指南](https://geostatsguy.github.io/MachineLearningDemos_Book/intro.html)》。

迈克尔的所有大学讲座都可在他的[YouTube 频道](https://www.youtube.com/@GeostatsGuyLectures)上找到，其中包含 100 多个 Python 交互式仪表板和 40 多个存储库中的详细工作流程链接，这些存储库位于他的[GitHub 账户](https://github.com/GeostatsGuy)，以支持任何感兴趣的学生和在职专业人士，提供持续更新的内容。要了解更多关于迈克尔的工作和共享教育资源，请访问他的网站。

## 想一起工作吗？

我希望这些内容对那些想了解更多关于地下建模、数据分析和学习机器的人来说有帮助。学生和在职专业人士都欢迎参与。

+   想邀请我到贵公司进行培训、辅导、项目审查、工作流程设计以及/或咨询吗？我很乐意拜访并与您合作！

+   感兴趣合作、支持我的研究生研究或我的地下数据分析与机器学习联盟（共同负责人包括 Foster 教授、Torres-Verdin 教授和 van Oort 教授）吗？我的研究结合数据分析、随机建模和机器学习理论及实践，开发新颖的方法和工作流程以增加价值。我们正在解决具有挑战性的地下问题！

+   您可以通过 mpyrcz@austin.utexas.edu 联系到我。

我总是很高兴讨论，

*迈克尔*

迈克尔·皮尔茨，博士，注册工程师，德克萨斯大学奥斯汀分校 Cockrell 工程学院和 Jackson 地球科学学院教授

更多资源请访问：[Twitter](https://twitter.com/geostatsguy) | [GitHub](https://github.com/GeostatsGuy) | [网站](http://michaelpyrcz.com) | [Google Scholar](https://scholar.google.com/citations?user=QVZ20eQAAAAJ&hl=en&oi=ao) | [地统计学书籍](https://www.amazon.com/Geostatistical-Reservoir-Modeling-Michael-Pyrcz/dp/0199731446) | [YouTube](https://www.youtube.com/channel/UCLqEr-xV-ceHdXXXrTId5ig) | [Python 中应用地统计学电子书](https://geostatsguy.github.io/GeostatsPyDemos_Book/intro.html) | [Python 中应用机器学习电子书](https://geostatsguy.github.io/MachineLearningDemos_Book/) | [LinkedIn](https://www.linkedin.com/in/michael-pyrcz-61a648a1)
