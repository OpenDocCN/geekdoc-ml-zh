# 第十章 开发管道的工具

> 原文：[`ppml.dev/development-tools.html`](https://ppml.dev/development-tools.html)

在第**2**部分，我们讨论了如何将软件工程实践应用于机器学习管道的开发。这些实践依赖于我们采用的工具来实现，这将是第**3**部分的重点。

在本章中，我们展示了最新的数据探索、实验跟踪（第 10.1 节）、代码开发（第 10.2 节）以及构建、测试和文档化（第 10.3 节）的工具选择。这些工具共同提供了一个适合创建机器学习管道的开发环境。然后，我们将转向用于在生产中管理和维护管道的工具（第十一章）。我们完全预期，在撰写本文时可用到的工具将随着时间的推移而巩固，正如软件工程和系统管理中其他类型的生产力工具所发生的那样。

## 10.1 数据探索和实验跟踪

机器学习管道中的许多问题都可以追溯到不够干净或结构良好的数据，因此不适合用于训练或推理。早期的探索性分析和领域专家的参与将提高我们对数据的理解，并有助于提高其质量，使其达到我们可以解决第 5.2.1 节和 9.1 节中讨论的问题的程度。在这些探索中编写的代码是初始原型，经过多次打磨和重构后，将成为管道的数据摄取和准备模块（第 5.3.3 节）。使用程序化方法进行数据探索、清理和转换始终是首选，因为它提供了可重复的结果，并使代码版本化（第 6.5 节）。我们应使用基于属性的测试（第 9.4.2 节）来测试我们产生的代码，使用样本数据检查其是否正确工作；相同的测试可以在管道生命周期内作为新数据的质量门重用。我们将在第 10.2 节中建议不同的工具来编写此类代码：如 Jupyter 笔记本（Project Jupyter 2022）；如 RStudio（RStudio 2022a）这样的 IDE；如 NumPy（Harris 等人 2020）和 Pandas（McKinney 2017）这样的 Python 库；如 dplyr（Wickham, François 等人 2022）、tidyr（Wickham, Girlich 和 RStudio 2022）和 janitor（Firke 等人 2022）这样的 R 包，只是其中的一小部分例子。此外，大多数集成 MLOps 工具都包含实验跟踪，因此我们可以将这些探索保存在一个集中式和版本化的存储库中，然后根据我们选择的指标（第 5.3.1 节）对模型进行评估和验证（第 5.3.4 节）。

此外，高级可视化工具可以用于探索和清理数据，这可能有助于涉及那些可能不熟悉编程的领域专家。以下是一些例子：

+   Openrefine (Openrefine 2022)：这是一个开源的客户端-服务器解决方案，它提供了一个协作的 Web 界面来处理数据，以及客户端库，用于通过服务器公开的 API 自动化任务。

+   Trifacta (Trifacta 2022)：这是一个商业解决方案，提供了一个易于使用的界面来处理数据质量、数据转换和一般的数据处理管道。它为非技术用户设计，并支持在所有主要云服务提供商上部署。

+   Tableau Prep Builder（Tableau Software 2022）：提供了一个直观的界面，可以交互式地清理、格式化和可视化来自不同来源的数据。它既可作为本地图形应用程序，也可作为 Web 应用程序使用。

+   类似于 Airtable（Formagrid 2022）这样的 Web 解决方案，它结合了数据库的功能和电子表格的特性，也可能适合以协作方式处理小型数据集。

数据探索是一个迭代的过程：交互式地可视化其输出随时间变化是至关重要的。如果我们使用 Jupyter 或 RMarkdown（Xie, Allaire, and Grolemund 2022）来探索数据，我们可以直接从 Python 和 R 代码中这样做。作为替代方案，我们可以使用 Python、R、Julia 或 Bokeh（Bokeh 2022）代码以及 Jupyter 笔记本，通过 Tableau（Tableau Software 2022）或 Dash（Plotly 2022a）等可视化工具生成一个专门的交互式仪表板。我们将在第 11.3 节中讨论更多工具。

如果数据量太大，无法使用上述工具处理，我们可以使用基于 Hadoop（Apache Software Foundation 2022c）的“大数据”框架来存储，例如 Cloudera（Cloudera 2022）。然后我们可以使用 Apache Pig（Apache Software Foundation 2022e）、Apache Hive（Apache Software Foundation 2022d）、Apache Impala（Apache Software Foundation 2022b）和 Apache Spark（Apache Software Foundation 2022f）等工具来操作它们，并实现我们自己的数据摄取和清洗；或者我们可以使用集成的云解决方案，如 Snowflake（Snowflake 2022）和 Databricks Lakehouse（Databricks 2022）。这些集成解决方案的优势在于，它们处理数据管理的各个方面以及机器学习应用的开发和交付，支持与数据工程、数据科学和机器学习开源项目的集成。

例如，Databricks 包含许多开源组件。其中一个是 Delta Lake（The Delta Lake Project Authors 2022a）：一个用于现有数据湖和对象存储（如 S3）的抽象层，与 Apache Spark 完全兼容，并支持诸如 ACID 事务、模式强制和数据版本化等功能。Databricks 还提供 MLflow（Zaharia and The Linux Foundation 2022）的托管版本，这是一个开源库无关的平台，用于管理机器学习管道，我们将在第 11.2 节中更详细地描述。

DVC（数据版本控制）（迭代 2022b）是一个开源工具，它将 GitOps 原则应用于数据。²⁵ DVC 通过存储在文本文件中的元数据来管理数据和机器学习模型，并使用 Git（Git 开发团队 2022）来对其进行版本控制和跟踪其来源。DVC 既是命令行工具也是库，且与语言和框架无关。

DVC 和 MLflow（Zaharia 和 Linux 基金会 2022）以两种不同的方式实现实验跟踪。DVC 使用提交、分支和标签在 Git 项目中组织实验。它自动跟踪数据依赖、机器学习代码、参数和模型工件：我们可以通过关联的指标使用其命令行或其网页界面来比较不同的实验。MLflow 则提供跟踪服务器和客户端库，这些库可以集成到 Python、R 和 Java 代码中，如第 5.3.6 节所述。跟踪服务器将客户端为每个实验运行收集的元数据、参数、指标和标签存储到文件或数据库中。较大的输出，如数据文件、图像和模型工件，则分别保存，例如，在对象存储中。

此外，还有一些专有 SaaS 提供具有实验跟踪和模型注册功能的解决方案，例如：Neptune（Neptune Labs 2022）、Comet（Comet 2022）和 Weights & Biases（Weights & Biases 2022）；关于此类软件的更多详细信息请参阅第 11.2 节。

## 10.2 代码开发

现代软件开发是一种基于知识共享、持续迭代和持续反馈的协作努力。特别是分布式版本控制系统和 Git（Git 开发团队 2022）通过设定代码版本控制和协作开发的行业标准，并通过支持流行的平台如 GitHub 和 GitLab，使得所有这一切成为可能。我们使用 DevOps 提供和部署软件的能力在很大程度上依赖于 Git 以及语义版本控制（Preston-Werner 2022）和提交标签。因此，Git 是每个软件工程师都应该熟悉的工具。机器学习和数据科学专业人士也应该熟悉它，因为它被用于并影响了像 DVC 这样的软件的设计，这些软件用于处理管道。

选择编写代码的正确工具集是一个关于特定工具的先前经验和个人品味的问题。这可能是由个人开发者或团队、研究小组或开发者所属的公司在标准化预定的软件集时做出的决定。在任何情况下，为了在机器学习管道上高效工作，我们都需要支持：

+   我们将使用的编程语言（第 6.1 节）；

+   强制执行编码标准（第 6.3 节）；

+   自动重构（第 6.7 节）；

+   与源代码版本控制的集成（第 6.5 节）；

+   运行软件测试并总结其结果（第 9.4 节）；

+   交互式调试（第 9.4 节）；

+   管理封装开发环境的容器（第 7.1.4 节），以及在其中远程工作的能力。

确保所有开发者使用类似的工具对于合规性、简化新开发者的培训以及提高可重复性是有用的。（出于同样的原因，我们应该避免在第 5.2.4 节中讨论的多语言编程）。可供选择的各种工具很多，可以分为以下几类：

+   现代且相对轻量级的*代码编辑器*，如 Atom（Atom 2022）和 Visual Studio Code（Microsoft 2022d，也称为 VS Code），可以通过使用第三方扩展来扩展以提供上述功能。

+   *集成开发环境*，如 Eclipse（Eclipse Foundation 2022a）和 JetBrains IntelliJ IDEA（JetBrains 2022a）；

+   *共享交互式计算平台*，如 Jupyter 笔记本（Project Jupyter 2022）；

### 10.2.1 代码编辑器和 IDE；

集成开发环境（IDE）与代码编辑器之间的主要区别在于内置功能和默认设置的配置量。一方面，IDEs 在单个编程语言上提供大多数功能。例如，PyCharm（JetBrains 2022b）提供了代码检查、代码补全、语法高亮、版本控制、调试、重构、测试执行以及容器集成等功能，就像其他主要 IDE 一样，还支持 Python REPL，并为 NumPy 和 Pandas 等科学计算库创建的对象提供内省。R 语言的参考 IDE 是 RStudio（RStudio 2022a），它集成了控制台、支持语法高亮和直接代码执行的编辑器、用于绘图和检查 R 对象的工具以及历史记录、调试和工作区管理。

另一方面，代码编辑器在出厂时功能较为有限，但通过安装和配置第三方扩展，它们可以达到与集成开发环境（IDE）相媲美的功能。例如，VS Code 可以通过使用符合语言服务器协议规范的*Python 语言服务器*（Palantir 2022）来提供与 PyCharm 相似的功能（Microsoft 2022f）。其他替代方案包括 Mypy（The mypy Project 2014）、Pylance（Microsoft 2022a）（基于微软的 Pyright（Pyright 2022）静态类型检查器）、Pytype（Batchelder 和等 2022）（来自 Google）和 Pyre（Meta Platforms 2022a）（来自 Facebook）。通过语言服务器包（Lai 和 Ren 2022）和 VS Code R 扩展（REditorSupport 2022）以及 R 语言，以及 LanguageServer.jl（Julia VS Code 2022a）和 VS Code Julia 扩展（Julia VS Code 2022b）为 Julia 提供相同级别的集成。

代码编辑器和 IDE 也可以作为网络应用程序运行：一个网络浏览器会话连接到一个云实例，该实例复制了一个常见、统一的开发环境。这种方法有两个优点：它减少了因多语言编程而产生的技术债务（第 5.2.4 节）并使得在本地运行过于复杂或资源密集的环境中也能进行开发成为可能（第 5.3.2 节）。像 VS Code 这样的代码编辑器提供了网络界面（Microsoft 2022j），用于导航文件和仓库以及提交小的代码更改，而像 GitHub Codespaces（GitHub 2022a）和 AWS Cloud9（Amazon 2022c）这样的 IDE 则提供了由虚拟机支持的完整云开发环境（第 7.1.3 节）。我们还可以选择使用 Docker（Docker 2022a）和 Kubernetes（The Kubernetes Authors 2022a）来自行托管它们：Eclipse Che（Eclipse Che 2022）、Eclipse Theia（Eclipse Foundation 2022b）和 GitPod（Gitpod 2022）的基础容器镜像都很容易获得。至于 R，RStudio Server（RStudio 2022b）通过连接到远程服务器上运行的 R 会话的基于浏览器的界面，提供了与 RStudio IDE 相同的功能。我们还想指出 DagsHub（DagsHub 2022）作为一个协作平台：它为遵循本书中介绍的开发模式和惯例的数据科学和机器学习项目提供了一个共享的工作环境。它集成了 GitHub、DVC、MLflow、Jenkins（Jenkins 2022b）和许多其他开源工具。

最后，我们想提及最后一组代码编辑器：Vim、Neovim（Neovim 2022）和 Emacs（GNU Project 2022）。它们受到那些喜欢创建符合特定需求模块化开发环境的开发者的青睐。这两个编辑器都通过与各自语言服务器通信的插件，为 R、Julia 和 Python 提供了全面的支持。尽管它们的初始学习曲线很陡峭，但长期来看，它们允许在任意大小的代码库上实现无与伦比的操作速度。

### 10.2.2 笔记本

近年来，笔记本在机器学习项目中得到了广泛的应用，更普遍地说，在科学研究中也得到了应用。它们通常实现为 Jupyter 笔记本（Project Jupyter 2022，由支持*Ju*lia、*Pyt*hon 和*R*的编程语言开发），这是一个理想的交互式开发工具，用于构建概念证明。Jupyter 笔记本旨在快速测试想法，评估不同替代方案的权衡，并使用 Markdown 格式与文档混合共享代码、结果和图表。它们可以直接从 GitHub 和 GitLab、专门的协作平台（如 Google Colaboratory 2022g，也称为 Colab）或 MLOps 平台（如 Amazon SageMaker 2022d)或 Azure Machine Learning 2022c)执行。

尽管有相当多的编程语言支持以及科学界的广泛采用，但 Jupyter 笔记本在现代开发实践中存在一些缺点（见 6 章，特别是 6.3 节）。Jupyter 笔记本文件格式将代码、输出、图像和 Markdown 文本存储在一个单一的巨大 JSON 对象中，以产生一个自包含、可携带的工件。这种架构选择有四个主要缺点：

1.  正确版本化笔记本是一项挑战，因为随机输出每次运行都会变化。

1.  在“单元格”中表示代码，这些单元格可以按任何顺序执行，这与机器学习软件中使用的编程语言的命令式本质相矛盾。

1.  将代码分成单元格以交错它们的输出和周围文本会影响代码的模块化和代码重用，并降低我们产生良好抽象的能力。我们可以接受单元格之间一定程度的耦合（因为它们在共享的、隐藏的全局环境中运行代码）并添加粘合代码来使事物工作，但这会导致技术债务的增加（见 5.2.3 和 5.2.4 节）。

1.  笔记本没有内置对自动化测试（见 9.4 节）或部署（见 7 章）的支持。虽然这可以接受用于探索数据和原型设计（见 5.3.2 节），但它使它们不适合开发生产级软件。

尤其是按照非线性顺序执行单元格可能会导致结果不一致，因为单元格会相互影响环境，从而实际上创建了一个非常难以追踪的“隐藏状态”。实现可重复性的唯一方法是从笔记本的顶部开始，始终在一个干净的环境中执行所有代码。因此，我们无法直接使用笔记本在生产环境中提供机器学习模型。然而，这种情况在未来可能会改变：目前正在进行开发用于比较和合并（nbtime（Alnæs 和 Project Jupyter 2022）和 nbstripout（Rathgeber 2022））的工具，自动测试（testbook（Nteract Team 2022b）和 nbval（Cortes-Ortuno 等人 2022）），自动化（Papermill（Nteract Team 2022a））和质量保证（nbQA（nbQA Team 2022））。

RMarkdown 和 Julia 笔记本由于默认情况下从顶部开始在一个干净的环境中执行笔记本中的所有代码，因此它们是完全可重复的，所以我们在单元格中更改代码并重新运行后，不会出现状态不一致。此外，它们比 Jupyter 笔记本更容易进行版本控制和比较，因为它们在编译时将文本、输出和图像存储在单独的 Markdown、PDF 或 HTML 文件中。RMarkdown 笔记本得到了 RStudio 的良好支持，它提供了代码自动完成、代码检查和建议，但它们可以用任何文本编辑器编辑，并通过命令行编译。我们还可以使用 workflowr R 包（Blischak、Carbonetto 和 Stephens 2022）增强它们，该包结合了文献编程（使用 knitr（Xie 2015））和版本控制（使用 git2r（Widgren 和等人 2022））来生成包含时间戳和版本代码块及输出的可共享 HTML 页面。为了实现可重复性，每个分析都在新的 R 会话中运行。

由于 Jupyter 笔记本主要面向原型设计，我们建议它们应集成到现代开发工作流程中，如下所示：

1.  使用笔记本进行实验并构建代码的原型。

1.  当原型完成时，将代码移至新的 Git 仓库，并使用 IDE 或代码编辑器开始重构它，使其模块化和可扩展。同时，添加软件测试。

1.  使用 Jupyter Markdown 中的文本作为基础，为代码添加文档字符串（Goodger 和 Rossum 2022）。

1.  使用 pip（Python 软件基金会 2022a）和 setuptool（Python 打包权威机构 2022）（第 7.1.2 节）打包您的工件，以便以后作为机器学习管道中的模块或将被其他 Jupyter 笔记本导入的库使用。

### 10.2.3 访问数据和文档

在开发过程中快速访问文档对于处理复杂的代码库来说是无价的。我们可以使用离线文档浏览器，例如 Velocity（Silverlake Software 2022）或 Dash，或者开源的 Zeal（The Delta Lake Project Authors 2022b）。这三个都可以自动下载主要编程语言和机器学习框架的*docsets*（离线使用的 HTML 文档存档），并且可以与领先的 IDE 和代码编辑器集成。总的来说，它们在功能上可以互换。

至于访问数据，像 AWS Amazon S3 这样的对象存储正在成为数据科学和机器学习数据交换的事实标准。因此，将代码编辑器和 IDE 与能够抽象化跨多个云供应商的对象存储桶中数据的列出、下载和上传的库集成是非常有用的。一个流行的例子是 MinIO（MinIO 2022），它与 S3 API 完全兼容，并为多种语言提供开源 SDK。

## 10.3 构建测试和文档工具

使用适当的工具进行构建、测试和执行软件质量保证对于提高人体工程学并减少错误的可能性很重要。此外，我们可能希望在所有环境中（开发者工作站、预生产和生产环境）以及开发的各个阶段使用相同的工具集，以避免不一致并维护所有在管道上工作的人共享的通用环境。

目前，容器（第 7.1.4 节）是机器学习管道模块打包的最常见方式：要么单独使用 Docker，要么使用 Docker Compose（Docker 2022c）或 Podman（The Containers Organization 2022）进行分组，或者作为单节点 Kubernetes 使用 Minikube（The Kubernetes Authors 2022c）或 MicroK8s（Canonical 2022b）。所有这些解决方案都基于 Docker，在架构和功能方面有不同的权衡，因此支持第七章中描述的部署实践。

使用容器的一个要点是将不同的模块和应用程序彼此隔离。我们可以通过使用`pipenv`或`pip`（Python 软件基金会 2022a）加上`virtualenv`（Python 打包权威机构（PyPA）2022）来创建隔离的 Python 环境，并管理对包和 Python 解释器特定版本的依赖（避免与全局安装的版本冲突）。我们可以使用 pyenv（Yamashita, Stephenson, and et al. 2022）或 Poetry（Kalnytskyi 2022a）安装和切换多个 Python 版本，或者使用支持最常用解释器、编译器和开发工具的多个运行时版本的通用工具，如 asdf（Manohar 2022）。如果我们的需求过于复杂，我们可能需要考虑像 Pipenv（Reitz and Python Packaging Authority (PyPA) 2022）和 Conda（Anaconda 2022b）这样的工具。特别是，Conda 对机器学习和数据科学应用的支持很广泛（Anaconda 2022a），但使用起来相当繁琐。Pipenv 在 R 中的对应工具是 packrat 包（Ushey et al. 2022），它也使用本地安装的 R 解释器。

自动化测试是现代软件开发（第 6.5 节）、重构（第 6.7 节）和维护（第 9.4 节）实践中的另一个关键特性。每个测试都应该在一个干净的环境中运行，例如每次运行时都会重新创建的容器：我们希望避免一个测试的执行影响另一个测试的结果。（我们在第七章中讨论的自动化和可重复部署实践是自动化测试的关键推动力！）测试结果应通过 CI/CD 管道以通过/失败的形式包含，以促进代码审查（第 6.6 节）并确保管道始终处于工作状态。有许多框架和库可以用来实现第 9.4 节中描述的测试类型。对于单个模块，我们可以使用 Python 标准库中的 unittest（Python 软件基金会 2022c）和 doctest（Python 软件基金会 2022b）包，R 的 testthat 包（Wickham, RStudio, and R Core Team 2022）以及 Julia 的测试模块。

我们还应该测试整个机器学习管道是否按预期工作。像 Airflow（Apache 软件基金会 2022a）、DVC 和 Pachyderm（Pachyderm 2022）这样的工具使用 DAG 来映射模块之间的依赖关系，以便进行本地、迭代的测试。在 DVC 和 Pachyderm 中，依赖关系在声明性配置文件（例如，DVC 的 `dvc.yaml`）中指定，该文件可以手动编写或使用辅助命令程序化构建。DVC 没有任何内置的测试支持，因此我们应该自己为模块提供工具（用于单元测试）并将整个管道嵌入到测试框架中（用于集成、系统测试和验收测试）。在 Airflow 中，管道用 Python 代码实现，依赖关系编码在专门的 `DAG` 对象中：这使得使用 unittest 测试单个模块变得容易，并可以使用如 Great Expectations（Superconductive 2022）等框架验证数据。至于在 GitHub、GitLab 或 Jenkins 上运行的管道，我们可以使用动作和运行器（Gitlab 2022a；Nektos 2022；Jenkins 2022a），尽管有一些限制，但可以使用容器运行完整的管道或其部分。另一种选择是直接通过迭代提交更改到测试分支并将它们推送到主线分支来强制 CI 运行可能相关的任何测试。Jenkins 还提供用于在配置和管道代码的条件逻辑上实施单元测试的测试框架以及用于检查管道的命令行工具。GitLab 提供用于触发验证和代码检查的 API。

强制执行代码风格和标准，我们在第 6.3 节中讨论过，这是测试的重要补充，以确保我们生产出可维护、可工作的软件。Pylint（Logilab 和 PyCQA 及其贡献者 2022）是 Python 的参考静态代码分析器和代码检查器：它基于 PEP-8（Python 增强提案 8），这是包含有关如何编写 Python 代码的指南和最佳实践的官方文档。另一种选择是 Flake8（Stapleton Cordasco 2022），它基于其他工具，如 pycodestyle（风格指南检查器）、pyflakes（源文件错误检查器）和 mccabe（检查代码复杂性的工具）。Python 代码的全面代码检查步骤应应用以下工具序列：

1.  isort（Crosley 2022）用于按字母顺序排序导入并按类型分隔成部分；

1.  black（Langa 和等 2022）用于格式化代码；

1.  flake8 用于检查代码风格；

1.  作为最后一步运行静态代码分析的 pylint；

The styler 包（Müller 和 Walthert 2022a），它强制执行 tidyverse 风格指南（Wickham 2022b）的合规性，以及 lintr 包（Hester 等 2022），它执行静态代码分析并识别语法错误和可能的语义问题，填补了上述 Python 包在 R 代码中的角色。Both lintr（参见 `vignette("continuous-integration"`））和 styler 支持 CI/CD 集成（Müller 和 Walthert 2022b），接受用户提供的代码风格策略，并与 RStudio 集成。

编写文档并保持其更新是维护机器学习管道随时间持续运行的关键。文档应像代码一样进行版本控制，并尽可能接近其引用的代码。模块、函数和类接口或方法定义的文档可以放置在 Python（Goodger 和 Rossum 2022）和 Julia（Krämer 2022）代码中，使用结构化注释在文档字符串或本地的 Sphinx 格式；然后 Sphinx（Brandl 和 Sphinx 团队 2022）可以通过 Sphinx `autodoc` 扩展将这些注释编译成各种文件格式的文档（参见第 8.2 节的示例）。Sphinx 还可以用于自动（重新）编译文档，使用 CI（“Read the Docs” (Read the Docs 2022）），并将 OpenAPI 规范文件渲染为静态 HTML 页面（Kalnytskyi 2022c）。OpenAPI 规范文件反过来可以自动从文档字符串生成，使用 Sphinx（Kalnytskyi 2022b），Apispec（Loria 和等 2022）或如 FastApi（Ramírez 2022）和 Flask（Pallets 团队 2022）这样的框架。

在 R 中，我们可以使用 Doxygen（van Heesch 2022）格式的注释来达到相同的目的：它们可以被 Roxygen2 包（Wickham, Danenberg 等 2022）解析，以生成各种格式的 R 文档，如第 8.2 节所述。

### 参考文献

Alnæs, M. S. 和 Project Jupyter. 2022. *nbdime – Jupyter Notebooks 的差异和合并*. [`nbdime.readthedocs.io`](https://nbdime.readthedocs.io).

Amazon. 2022c. *AWS Cloud9 文档*. [`docs.aws.amazon.com/cloud9`](https://docs.aws.amazon.com/cloud9).

Amazon. 2022d. *机器学习：Amazon Sagemaker*. [`aws.amazon.com/sagemaker/`](https://aws.amazon.com/sagemaker/).

Anaconda. 2022a. *数据科学家使用的 Conda*. [`docs.conda.io/projects/conda/en/latest/user-guide/concepts/data-science.html`](https://docs.conda.io/projects/conda/en/latest/user-guide/concepts/data-science.html).

Anaconda. 2022b. *任何语言的包、依赖和环境管理*. [`docs.conda.io`](https://docs.conda.io).

Apache 软件基金会. 2022b. *Impala 文档*. [`impala.apache.org/impala-docs.html`](https://impala.apache.org/impala-docs.html).

Atom. 2022\. *21 世纪可黑客攻击的文本编辑器*. [`atom.io/`](https://atom.io/).

Batchelder, N. 和其他作者. 2022\. *Python 代码的静态类型分析器*. [`google.github.io/pytype`](https://google.github.io/pytype).

Blischak, J. D., P. Carbonetto, and M. Stephens. 2022\. *workflowr：一个用于可重复和协作数据科学的框架*. [`cran.r-project.org/web/packages/workflowr`](https://cran.r-project.org/web/packages/workflowr).

Bokeh. 2022\. *Bokeh 文档*. [`docs.bokeh.org/en/latest/`](https://docs.bokeh.org/en/latest/).

Brandl, G. 和 Sphinx 团队. 2022\. *Sphinx：Python 文档生成器*. [`www.sphinx-doc.org/en/master/`](https://www.sphinx-doc.org/en/master/).

Canonical. 2022b. *MicroK8s 文档*. [`microk8s.io/docs`](https://microk8s.io/docs).

Cloudera. 2022\. *Cloudera：混合数据公司*. [`www.cloudera.com/`](https://www.cloudera.com/).

Comet. 2022\. *Comet 文档*. [`www.comet.com/docs/v2`](https://www.comet.com/docs/v2).

Cortes-Ortuno, D., O. Laslett, T. Kluyver, V. Fauske, M. Albert, MinRK, O. Hovorka, and H. Fangohr. 2022\. *为 py.test 进行 IPython Notebook 验证的文档*. [`nbval.readthedocs.io`](https://nbval.readthedocs.io).

Crosley, T. 2022\. *一个用于排序导入的 Python 工具和库*. [`pycqa.github.io/isort/`](https://pycqa.github.io/isort/).

DagsHub. 2022\. *欢迎来到 DagsHub 文档*. [`dagshub.com/docs/`](https://dagshub.com/docs/).

Databricks. 2022\. *Databricks 文档*. [`docs.databricks.com/applications/machine-learning/index.html`](https://docs.databricks.com/applications/machine-learning/index.html).

Docker. 2022a. *Docker*. [`www.docker.com/`](https://www.docker.com/).

Docker. 2022c. *Docker Compose 概述*. [`docs.docker.com/compose`](https://docs.docker.com/compose).

Eclipse Che. 2022\. *在 Kubernetes 上运行您最喜欢的集成开发环境*. [`www.eclipse.org/che/technology/`](https://www.eclipse.org/che/technology/).

Eclipse 基金会. 2022a. *桌面集成开发环境*. [`www.eclipse.org/ide/`](https://www.eclipse.org/ide/).

Eclipse 基金会. 2022b. *Theia：云和桌面集成开发环境*. [`theia-ide.org/docs/`](https://theia-ide.org/docs/).

Firke, S., B. Denney, C. Haid, R. Knight, M. Grosser, 和 J. Zadra. 2022\. *janitor：用于检查和清理脏数据的简单工具*. [`cran.r-project.org/web/packages/janitor`](https://cran.r-project.org/web/packages/janitor).

Formagrid. 2022\. *Airtable 是一个具有数据库功能的现代电子表格平台*. [`airtable.com`](https://airtable.com).

GitHub. 2022a. *GitHub Codespaces*. [`github.com/features/codespaces`](https://github.com/features/codespaces).

Gitlab. 2022a. *GitLab Runner 文档*. [`docs.gitlab.com/runner/`](https://docs.gitlab.com/runner/).

Gitlab. 2022b. *什么是 GitOps?*. [`about.gitlab.com/topics/gitops`](https://about.gitlab.com/topics/gitops).

Gitpod. 2022\. *Gitpod：始终准备编码*. [`www.gitpod.io`](https://www.gitpod.io).

GNU Project. 2022\. *GNU EMacs*. [`www.gnu.org/software/emacs/`](https://www.gnu.org/software/emacs/).

Goodger, D., and G. van Rossum. 2022\. *PEP 257：文档字符串约定*. [https://peps.python.org/pep-0257/\#what-is-a-docstring]](https://peps.python.org/pep-0257/\#what-is-a-docstring%5D).

Google. 2022g. *欢迎使用 Colab!* [`colab.research.google.com`](https://colab.research.google.com).

Harris, C. R., K. J. Millman, Stéfan J. van der Walt, R. Gommers, P. Virtanen, D. Cournapeau, E. Wieser, et al. 2020\. “NumPy 的数组编程.” *自然* 585 (7285): 357–62.

Hester, J., F. Angly, R. Hyde, M. Chirico, K. Ren, and A. Rosenstock. 2022\. *R 代码的代码检查器*. [`cran.r-project.org/web/packages/lintr`](https://cran.r-project.org/web/packages/lintr).

Iterative. 2022b. *DVC: 数据版本控制。数据与模型的 Git*. [`github.com/iterative/dvc`](https://github.com/iterative/dvc).

Jenkins. 2022a. *运行 Jenkinsfile 作为函数的命令行工具*. [`github.com/jenkinsci/jenkinsfile-runner`](https://github.com/jenkinsci/jenkinsfile-runner).

Jenkins. 2022b. *Jenkins 用户文档*. [`www.jenkins.io/doc/`](https://www.jenkins.io/doc/).

JetBrains. 2022a. *IntelliJ IDEA*. [`www.jetbrains.com/idea/`](https://www.jetbrains.com/idea/).

JetBrains. 2022b. *PyCharm*. [`www.jetbrains.com/pycharm/`](https://www.jetbrains.com/pycharm/).

Julia VS Code. 2022a. *Microsoft 语言服务器协议的 Julia 语言实现*. [`juliapackages.com/p/languageserver`](https://juliapackages.com/p/languageserver).

Julia VS Code. 2022b. *Julia for Visual Studio Code*. [`www.julia-vscode.org`](https://www.julia-vscode.org).

Kalnytskyi, I. 2022a. *Poetry 文档*. [`python-poetry.org/docs`](https://python-poetry.org/docs).

Kalnytskyi, I. 2022b. *sphinxcontrib-openapi 是一个用于从 OpenAPI 生成 API 文档的 Sphinx 扩展*. [`sphinxcontrib-openapi.readthedocs.io`](https://sphinxcontrib-openapi.readthedocs.io).

Kalnytskyi, I. 2022c. *使用 ReDoc 渲染 OpenAPI 规范的 Sphinx 扩展*. [`sphinxcontrib-redoc.readthedocs.io/en/stable`](https://sphinxcontrib-redoc.readthedocs.io/en/stable).

Krämer, S. 2022\. *Julia 自动文档*. [https://bastikr.github.io/sphinx-julia/juliaautodoc.html\#julia-autodoc]](https://bastikr.github.io/sphinx-julia/juliaautodoc.html\#julia-autodoc%5D).

Lai, R. 和 K. Ren. 2022. *R 语言服务器协议的实现*. [`cran.r-project.org/web/packages/languageserver`](https://cran.r-project.org/web/packages/languageserver).

Langa 及他人. 2022. *Black：无妥协的代码格式化工具*. [`black.readthedocs.io/en/stable/`](https://black.readthedocs.io/en/stable/).

Logilab 和 PyCQA 及贡献者. 2022. *Pylint 是 Python 2 或 3 的静态代码分析器*. [`pylint.pycqa.org/en/latest/`](https://pylint.pycqa.org/en/latest/).

Loria, S. 及他人. 2022. *可插拔的 API 规范生成器*. [`apispec.readthedocs.io/en/latest`](https://apispec.readthedocs.io/en/latest).

Manohar, A. 2022. *asdf 文档*. [`asdf-vm.com/guide/getting-started.html`](https://asdf-vm.com/guide/getting-started.html).

McKinney, W. 2017. *Python 数据分析*. 2nd ed. O’Reilly.

Meta Platforms. 2022a. *Python 3 的性能型类型检查器*. [`pyre-check.org`](https://pyre-check.org).

微软. 2022a. *VS Code 中性能强大、功能丰富的 Python 语言服务器*. [`marketplace.visualstudio.com/items?itemName=ms-python.vscode-pylance`](https://marketplace.visualstudio.com/items?itemName=ms-python.vscode-pylance).

微软. 2022c. *Azure 机器学习*. [`azure.microsoft.com/en-us/services/machine-learning/`](https://azure.microsoft.com/en-us/services/machine-learning/).

微软. 2022d. *代码编辑，重新定义*. [`code.visualstudio.com/`](https://code.visualstudio.com/).

微软. 2022f. *语言服务器协议*. [`microsoft.github.io/language-server-protocol`](https://microsoft.github.io/language-server-protocol).

微软. 2022j. *VS Code 在网页上*. [`vscode.dev`](https://vscode.dev).

MinIO. 2022. *MinIO 文档*. [`docs.min.io/docs`](https://docs.min.io/docs).

Müller, K. 和 L. Walthert. 2022a. *R 代码的无侵入式美观打印*. [`cran.r-project.org/web/packages/styler`](https://cran.r-project.org/web/packages/styler).

Müller, K. 和 L. Walthert. 2022a. *R 代码的无侵入式美观打印*. [`cran.r-project.org/web/packages/styler`](https://cran.r-project.org/web/packages/styler).

2022b. *第三方集成*. [`styler.r-lib.org/articles/third-party-integrations.html`](https://styler.r-lib.org/articles/third-party-integrations.html).

nbQA 团队. 2022. *在 Jupyter 笔记本上运行 isort, pyupgrade, mypy, pylint, flake8 以及更多*. [`github.com/nbQA-dev/nbQA`](https://github.com/nbQA-dev/nbQA).

Nektos. 2022. *在本地运行 GitHub Actions*. [`github.com/nektos/act`](https://github.com/nektos/act).

Neovim. 2022. *基于 Vim 的超扩展文本编辑器*. [`neovim.io/`](https://neovim.io/).

Neptune Labs. 2022. *Neptune 文档*. [`docs.neptune.ai/`](https://docs.neptune.ai/).

Nteract Team. 2022a. *Papermill 是一个用于参数化和执行 Jupyter 笔记本的工具*. [`papermill.readthedocs.io`](https://papermill.readthedocs.io).

Nteract Team. 2022b. *Testbook*. [`testbook.readthedocs.io/en/latest/`](https://testbook.readthedocs.io/en/latest/).

Openrefine. 2022. *一个免费、开源、强大的用于处理杂乱数据的工具*. [`openrefine.org`](https://openrefine.org).

Pachyderm. 2022. *以数据为中心的管道和数据版本控制*. [`docs.pachyderm.com/latest`](https://docs.pachyderm.com/latest).

Palantir. 2022. *Python 语言服务器*. [`github.com/palantir/python-language-server`](https://github.com/palantir/python-language-server).

Pallets Team. 2022. *Flask 文档*. [`flask.palletsprojects.com/en/latest`](https://flask.palletsprojects.com/en/latest).

Plotly. 2022a. *Python、R、Julia 和 Jupyter 的分析型 Web 应用程序。无需 JavaScript.* [`github.com/plotly/dash`](https://github.com/plotly/dash).

Preston-Werner, T. 2022. *语义版本控制*. [`semver.org/`](https://semver.org/).

Project Jupyter. 2022. *Jupyter*. [`jupyter.org/`](https://jupyter.org/).

Pyright. 2022. *Python 静态类型检查器*. [`github.com/microsoft/pyright`](https://github.com/microsoft/pyright).

Python 打包权威机构. 2022. *使用 Setuptools 构建和分发包*. [`setuptools.pypa.io/en/latest/userguide/index.html`](https://setuptools.pypa.io/en/latest/userguide/index.html).

Python 打包权威机构 (PyPA). 2022. *Virtualenv 文档*. [`virtualenv.pypa.io/en/latest/`](https://virtualenv.pypa.io/en/latest/).

Python 软件基金会. 2022a. *PyPI: Python 包索引*. [`pypi.org/`](https://pypi.org/).

Python 软件基金会. 2022b. *测试交互式 Python 示例*. [`docs.python.org/3/library/doctest.html`](https://docs.python.org/3/library/doctest.html).

Python 软件基金会. 2022c. *unittest: 单元测试框架*. [`docs.python.org/3/library/unittest.html`](https://docs.python.org/3/library/unittest.html).

Ramírez, S. 2022. *FastAPI 框架，高性能，易于学习，快速编码，适用于生产*. [`fastapi.tiangolo.com`](https://fastapi.tiangolo.com).

Rathgeber, F. 2022. *从 Jupyter 和 IPython 笔记本中移除输出*. [`github.com/kynan/nbstripout`](https://github.com/kynan/nbstripout).

Read the Docs. 2022. *Read the Docs: 简化文档*. [`docs.readthedocs.io`](https://docs.readthedocs.io).

REditorSupport. 2022. *Visual Studio Code 中的 R*. [`marketplace.visualstudio.com/items?itemName=REditorSupport.r`](https://marketplace.visualstudio.com/items?itemName=REditorSupport.r).

Reitz, K. 和 Python 打包权威机构 (PyPA). 2022. *Pipenv: 适合人类的 Python 开发工作流程*. [`pipenv.pypa.io`](https://pipenv.pypa.io).

RStudio。2022a。*开源和面向企业的数据科学专业软件*。[`www.rstudio.com`](https://www.rstudio.com)。

RStudio。2022b。*RStudio 服务器*。[`www.rstudio.com/products/rstudio/\#rstudio-server`](https://www.rstudio.com/products/rstudio/\#rstudio-server)。

Silverlake 软件公司。2022。*Velocity：Windows 的文档和 Docset 查看器*。[`velocity.silverlakesoftware.com/`](https://velocity.silverlakesoftware.com/)。

Snowflake。2022。*Snowflake 文档*。[`docs.snowflake.com`](https://docs.snowflake.com)。

Stapleton Cordasco, I. 2022。*Flake8：你的风格指南执行工具*。[`flake8.pycqa.org/en/latest/`](https://flake8.pycqa.org/en/latest/)。

超导技术。2022。*伟大的期望*。[`docs.greatexpectations.io/docs`](https://docs.greatexpectations.io/docs)。

Tableau 软件公司。2022。*Tableau*。[`www.tableau.com/`](https://www.tableau.com/)。

Apache 软件基金会。2022a。*Airflow 文档*。[`airflow.apache.org/docs/`](https://airflow.apache.org/docs/)。

Apache 软件基金会。2022c。*Apache Hadoop*。[`hadoop.apache.org/`](https://hadoop.apache.org/)。

Apache 软件基金会。2022d。*Apache Hive 文档*。[`cwiki.apache.org/confluence/display/Hive`](https://cwiki.apache.org/confluence/display/Hive)。

Apache 软件基金会。2022e。*Apache Pig 文档*。[`pig.apache.org/docs/latest`](https://pig.apache.org/docs/latest)。

Apache 软件基金会。2022f。*Apache Spark 文档*。[`spark.apache.org/docs/latest`](https://spark.apache.org/docs/latest)。

容器组织。2022。*podman*。[`podman.io`](https://podman.io)。

Delta Lake 项目作者。2022a。*Delta Lake 文档*。[`docs.delta.io`](https://docs.delta.io)。

Delta Lake 项目作者。2022b。*Zeal：软件开发者的离线文档浏览器*。[`zealdocs.org`](https://zealdocs.org)。

Git 开发团队。2022。*Git 源代码镜像*。[`github.com/git/git`](https://github.com/git/git)。

Kubernetes 作者。2022a。*Kubernetes*。[`kubernetes.io/`](https://kubernetes.io/)。

Kubernetes 作者。2022c。*minikube*。[`minikube.sigs.k8s.io/docs`](https://minikube.sigs.k8s.io/docs)。

mypy 项目。2014。*mypy：Python 的可选静态类型*。[`mypy-lang.org/`](http://mypy-lang.org/)。

Trifacta。2022。*为分析和机器学习配置、准备和管道数据*。[`www.trifacta.com`](https://www.trifacta.com)。

Ushey, K.，J. McPherson，J. Cheng，A. Atkins，JJ. Allaire，和 T. Allen。2022。*Packrat: R 的可重复包管理*。[`rstudio.github.io/packrat/`](https://rstudio.github.io/packrat/)。

van Heesch, D. 2022。*Doxygen*。[`www.doxygen.nl/index.html`](https://www.doxygen.nl/index.html)。

Weights & Biases. 2022. *Weights & Biases 文档*. [`docs.wandb.ai/`](https://docs.wandb.ai/).

Wickham, H. 2022b. *《tidyverse 风格指南》*. [`style.tidyverse.org/`](https://style.tidyverse.org/).

Wickham, H., P. Danenberg, G. Csárdi, M. Eugster 和 RStudio. 2022. *roxygen2: R 的内联文档*. 

Wickham, H., R. François, L.Henry 和 K. Müller. 2022. *一个快速、一致的工具，用于处理内存中和非内存中的数据框类对象*. [`cloud.r-project.org/web/packages/dplyr`](https://cloud.r-project.org/web/packages/dplyr).

Wickham, H., M. Girlich 和 RStudio. 2022. *tidyr: 整洁混乱数据*. [`cloud.r-project.org/web/packages/tidyr`](https://cloud.r-project.org/web/packages/tidyr).

Wickham, H., RStudio 和 R Core Team. 2022. *R 的单元测试*. [`cloud.r-project.org/web/packages/testthat`](https://cloud.r-project.org/web/packages/testthat).

Widgren, S. 和 等. 2022. *git2r: 提供对 Git 仓库的访问*. [`cran.r-project.org/web/packages/git2r/index.html`](https://cran.r-project.org/web/packages/git2r/index.html).

Xie, Y. 2015. *使用 R 和 knitr 创建动态文档*. 2nd ed. CRC Press.

Xie, Y., J. J. Allaire 和 G. Grolemund. 2022. *R Markdown: 官方指南*. [`bookdown.org/yihui/rmarkdown/`](https://bookdown.org/yihui/rmarkdown/).

Yamashita, Y., S. Stephenson 和 等. 2022. *pyenv: 简单的 Python 版本管理*. [`github.com/pyenv/pyenv`](https://github.com/pyenv/pyenv).

Zaharia, M. 和 Linux 基金会. 2022. *MLflow 文档*. [`www.mlflow.org/docs/latest/index.html`](https://www.mlflow.org/docs/latest/index.html).

* * *

1.  GitOps 是 DevOps 实践（如版本控制、协作、合规性和 CI/CD）的应用，并将它们应用于自动化基础设施管理（Gitlab 2022b)↩︎.
