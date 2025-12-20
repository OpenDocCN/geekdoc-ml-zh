# 第十一章 生产环境中管理流水线的工具

> 原文：[`ppml.dev/production-tools.html`](https://ppml.dev/production-tools.html)

机器学习流水线在生产环境中的组成部分通常比传统软件的更多，而管理这些组成部分的 MLOps 软件是一个广泛且快速发展的领域，拥有许多平台、项目和工具。其底层基础设施可能更加复杂（第 11.1 节），而构成流水线的由数据、代码和模型组成的组合无疑更加异构（第 11.2 节）。除了我们需要管理它们的工具和技术之外，我们还讨论了那些我们可以用来补充流水线的仪表板和报告功能，这些功能在数据科学中很常见（第 11.3 节）。

## 11.1 基础设施管理

在生产环境中成功运行机器学习应用程序不仅限于实现一个流水线：它还涉及到管理不同的本地和远程计算系统，以及集成通过各种 API 相互通信的不同软件组件。令人困惑的是，文献中经常将这两者都称为“系统”，意味着任何需要配置、接收一些输入并产生一些输出的东西。这样的抽象定义使得计算系统、托管我们代码的 GitHub 组织、在云中运行其一部分的 Amazon AWS EC2 实例以及管理我们本地系统资源的 Kubernetes 集群都是系统。考虑到硬件在机器学习应用程序中的突出作用（第二章），我们发现这个定义并不实用，因为它过于抽象，无法推理应用程序本身的架构和性能（第五章）。同样，对于“一切只是 API”的更加抽象的观点也是如此。

在现实世界的流水线中手动或使用一些简单的脚本（这些脚本可以被视为粘合代码，第 5.2.3 节）来管理计算系统和软件通常过于繁重：它们太多了，遵循不同的约定（因为它们是由不同的供应商生产的），它们不向后兼容，并且它们的配置文件使用不同的语言和格式。配置管理是唯一可能的方法，以保持这种复杂性在可控范围内，并确保流水线可重复和可审计。

用于此任务最广泛使用的工具之一是 Terraform（HashiCorp 2022b），它将自己定义为实现“基础设施即代码”的工具。Terraform 本质上是一个广泛的服务的抽象层（HashiCorp 2022c），包括 Amazon AWS、Microsoft Azure、GitHub、GitLab 和 Airflow。每个平台都作为服务“提供者”公开，通过我们控制的 API 进行通信，有效地将我们的基础设施与原始服务的 API 解耦。Terraform 负责在原始服务中初始化资源并配置它们。例如，我们可以用它来创建远程资源，如 Amazon AWS 上的 EC2 实例、Azure 上的对象存储或本地 vSphere（VmWare 2022）上的虚拟机。然而，它不处理操作系统和软件包的安装或配置。

基于 Vagrant（HashiCorp 2022d）和 Packer（HashiCorp 2022a）的云实例、虚拟机（VMs）和开发机器可以使用专门的工具如 Ansible（Ansible Project 2022）、Puppet（Puppet 2022）和 Chef（Progress Software 2022）进行安装和配置。这三个工具都提供了一套完整的配置管理解决方案：我们可以将所有资源和它们的配置定义为代码，并将这些代码存储在版本控制系统（VCS）中。它们还提供了测试配置管理代码的模块，用于在应用特定目标环境之前验证更改，以及用于识别配置文件的手动修改或篡改。因此，它们便于在 CI/CD 管道中集成和自动化。此外，Ansible、Puppet 和 Chef 都可以在 Terraform 创建的实例和 VMs 的首次启动时通过 cloud-init（Canonical 2022a）等软件调用。然而，它们的学习曲线不同，操作它们需要不同的技术技能。Ansible 是用 Python 编写的，使用 YAML 声明性配置文件，并具有无代理架构（即，可以在不安装任何东西的情况下在要配置的实例上运行）。Puppet 和 Chef 使用基于 Ruby 的领域特定语言，并具有主从架构（即，我们在实例上安装“代理”来配置它们）。

对于容器，事实上的标准管理工具是 Kubernetes（Kubernetes 作者 2022a），这是一个开源的编排系统，最初由谷歌开发，现在由云原生计算基金会（CNCF）维护。Kubeflow（Kubeflow 作者 2022）通过将其与流行的机器学习框架（如 Tensorflow）、笔记本（如 Jupyter）和数据管道（如 Pachyderm）集成来扩展 Kubernetes：结果是专门针对管理、开发、部署和扩展机器学习管道的集成平台。Kubeflow 可以部署在管理的 Kubernetes 服务上，如 Amazon EKS（亚马逊网络服务 2022a）、Azure AKS（微软 2022b）或 Google Kubernetes Engine（谷歌 2022c），也可以部署在本地 Kubernetes 集群上。后者虽然运行起来更为复杂，但可以使用 CNCF 认证的开源解决方案（如 Kubernetes Fury Distribution（SIGHUP 2022）和 Typhoon（波塞冬实验室 2022）来设置。这两个解决方案都基于 Terraform 和 Ansible，并与其他 CNCF 组件（如软件定义网络、监控和日志记录）集成（第 5.3.6 节），以促进云和本地部署之间的互操作性。

## 11.2 机器学习软件管理

机器学习应用可以通过集成 MLOps 平台来设计、测试、维护和交付到生产环境中，这些平台融合了 DevOps（第 5.3 节）的工具和实践以及数据处理（第 5.3.3 节）、模型训练和部署（第 5.3.4、5.3.5 和 7.2 节）。在撰写本文时，这是一个非常新的趋势，因此“MLOps 平台”（或“机器学习平台”）这一标签已经附加到了相当多的工具上。在光谱的一端，我们有像 AWS Sagemaker（Amazon 2022d）、Vertex AI（Google 2022f）、Tensorflow Extended（TensorFlow 2022d）、Databricks（Databricks 2022）和 Neptune（Neptune Labs 2022）这样的在线平台。在另一端，我们有更轻量级的解决方案，如 Airflow、MLflow 和 DVC，它们建立在一系列较小的开源工具之上，这些工具并不特定于机器学习应用。除此之外，我们还有像 GitLab 这样的 CI/CD 平台，它们正在开发 MLOps 功能（GitLab 2022c），这些功能与上述平台重叠。我们预计，在 MLOps 平台最终整合成少数几个明确的类别之前，可能还需要几年时间。在此期间，我们正在选择那些尚不成熟且具有不同、不明确的权衡的工具：目前肯定没有一种适合所有情况的解决方案！然而，我们可以安全地提到一个权衡点：集成平台存在局限性，因为它们通常具有偏见（它们使得支持作者设想之外的其他配置和工作流程变得困难）并且它们是透明的（其组件从外部不可见）。在管道的生命周期早期采用它们可能会限制我们在以后改变其架构的能力，可能会阻止我们探索不同的配置以研究它们的权衡，并可能限制我们发展软件工程技能的能力。相比之下，手动集成较小的开源工具给我们提供了更多的自由度，但需要更多的工作，并且需要具备一定程度的软件工程技能。

基于 Kubernetes 的解决方案，例如 Kubeflow 和 Polyaxon（Polyaxon 2022），集成了不同的工具，包括 Jupyter 笔记本；模型训练（在 CPU 或 GPU 上）和 TensorFlow 及其他框架的实验跟踪；以及使用不同解决方案的模型服务，如 TensorFlow Serving（TensorFlow 2022b）、SeldonCore（Seldon Technologies 2022）和 Kserve（The KServe Authors 2022）。Kubeflow 专注于端到端管理机器学习工作流程，而 Polyaxon 通过提供分布式训练、超参数调整和并行任务执行来补充它。Polyaxon 还可以调度和管理 Kubeflow 操作员，跟踪指标、输出、模型和资源使用情况，以比较实验。如果像 Kubeflow 这样的解决方案对于管理我们的管道来说过于复杂，我们还可以考虑用 Argo Workflow（Argo Project 2022）来替换它，这是一个更简单的编排器，可以在 Kubernetes 集群上运行并行作业。

Kubeflow 的架构建立在与 Kubernetes 相同的关键思想之上，特别是操作符和命名空间。实际上，Kubeflow 支持的所有机器学习库（TensorFlow（TensorFlow 2021a）、PyTorch（Paszke et al. 2019）等）都被封装在一个可以运行本地和分布式作业的 Kubernetes 操作符中。管道在单独的命名空间内执行：每个用户都可以利用 Kubernetes 命名空间隔离来防止未经适当授权的其他人访问笔记本、模型或推理端点（第 5.2.2 节）。

Seldon core（Seldon Technologies 2022）和 KServe（KServe 作者 2022）是专门针对 MLOps 的框架，用于在 Kubernetes（Kubernetes 作者 2022a）上打包、部署、监控和管理机器学习模型作为自定义资源。两者都将存储在二进制工件或代码包装器中的模型封装到容器中，通过自动生成的 OpenAPI 规范文件，通过 REST/gRPC API 公开模型的特性。此外，两者都与 Prometheus（Prometheus 作者和 Linux 基金会 2022）和 Grafana（GrafanaLabs 2022）（用于监控指标）以及 Elasticsearch（Elasticsearch 2022）或 Grafana Loki（Grafana Labs 2022）（用于日志记录）以及其他工具（用于检测数据漂移和执行渐进式部署等功能，我们在第 5.2.1 和 7.2 节中讨论过）。具有类似架构的其他两种选择是 BentoML（BentoML 2022）和 MLEM（Iterative 2022d）。前者是一个 Python 框架，具有简单的面向对象接口，用于将模型打包到容器中并创建 HTTP(S) 服务。后者，与 DVC 的作者相同，将模型元数据作为版本化的纯文本文件存储在 Git 仓库中，成为唯一的真相来源。

Tensorflow Extended (TensorFlow 2022d，也称为 TFX) 是一个基于 Tensorflow 的平台，用于托管端到端机器学习管道。TFX 设计用于在多种平台（通过 Vertex AI 的 Google Cloud、Amazon AWS）和编排框架（Apache Airflow、Kubeflow 和 Apache Beam（Apache 软件基金会 2022b））上运行，支持分布式处理（使用 Apache Spark 等框架），并允许使用 TensorBoard（TensorFlow 2022c）和 Jupyter 笔记本进行本地模型和数据探索。TFX 管道高度模块化，并按照我们在第五章讨论的路线组织成不同的组件，所有这些组件通过表示为 DAG 的依赖关系相互连接。用于实验跟踪所需的元数据使用 ML 元数据库（TensorFlow 2022a，也称为 MLMD）保存，以及监控信息和管道的日志，在一个支持关系型数据库的数据存储中。所有这些功能都伴随着复杂性和在某些领域缺乏灵活性的代价：在决定是否采用 TFX 之前，需要仔细评估我们的用例。

与基于 Kubernetes 构建的 Kubeflow 或基于 Tensorflow 构建的 TFX 不同，MLflow（Zaharia 和 Linux 基金会 2022）是一个用 Python 编写的库无关平台，可以通过轻量级 API 与任何机器学习库集成。MLflow 的目标是支持 MLOps，提供以下四个关键特性：

+   以及基于 Conda（Anaconda 2022b）和 Docker（Docker 2022a）构建的项目打包格式，这保证了可重复性，并使得项目易于共享；

+   一个实验跟踪 API，用于记录参数、代码和结果，并带有交互式用户界面，以便在实验之间比较模型和数据；

+   一个模型打包格式和一组 API，用于将模型部署到目标平台，如 Docker、Apache Spark 和 AWS Sagemaker；

+   一个带有图形界面和一组 API 的模型注册库，以便在模型上协作工作。

正如我们之前提到的，我们可以使用通用开源编排器，如 Airflow 和 Luigi（Spotify 2022a）来实现机器学习管道，或者使用更集成的工具，如 Dagster（Elementl 2022）和 Prefect 2.0（Prefect 2022）。Dagster 和 Prefect 2.0 都将管道作为 Python 模块在 DAG 中链接实现，并提供一个网络界面，使得可视化生产中运行的管道、监控其进度和故障排除变得容易。在 Airflow 和 Luigi 中，监控工作外包给了 Prometheus。与 Airflow 和 Luigi 不同，Pachyderm 支持非结构化数据，如视频和图像，以及来自数据仓库的表格数据。此外，它可以根据数据变化、任何类型的数据版本自动触发管道，并自动调整资源（因为它基于容器并在 Kubernetes 上运行）。

我们可以使用比 Kubeflow 更轻量级的工具来实现实验跟踪：两个例子是 MLflow Tracking 和 DVC（与 Gitlab 的或 Jenkins 等 CI/CD 管道集成），这在第 10.1 节中进行了讨论。一个相关的工具是 CML（Iterative 2022a），它是由与 DVC 相同的作者开发的：一个开源的命令行工具，可以轻松集成到任何 CI/CD 管道中，以在每个 pull request/merge request 中添加自动生成的带有模型指标图表的报告。为了做到这一点，CML 监控数据的变化，并自动进行模型训练和评估，以及跨项目迭代的 ML 实验比较。Neptune（Neptune Labs 2022）也是专门为在多个实验中存储和跟踪元数据而设计的。它实现了我们在第 5.3.4 节中提出的实践：特别是，在模型注册表中保存模型工件，并附带相关数据、代码、指标和环境配置的引用。

我们还有另一个选择，即使用如 Sagemaker 和 Vertex AI 这样的托管云平台。它们的优势是与 Amazon AWS 和 Google Cloud 的深度集成，分别使得实施渐进式交付技术、集中日志和监控以及使用 GPU 训练和部署模型变得简单直接。AWS 还提供与 Redshift（Amazon 2022a）和 Databricks 的集成，以访问数据；Vertex AI 同样与 BigQuery（Google 2022a）进行集成，并支持与特征存储一起工作。这两个平台都支持 Jupyter 笔记本进行交互式探索，并且都支持管道：Sagemaker 通过自定义 Python 库，Vertex AI 通过 Kubeflow 和 TFX。此外，Vertex AI 允许我们在 Jupyter 笔记本中开发机器学习模型，部署保存在对象存储桶中的模型，并将它们上传到专用的模型注册表。总之，这两个平台都在追逐彼此的功能，它们非常全面：但正因为如此，可能会让人感到困惑。

最后，特征存储在 MLOps 中的受欢迎程度正在增加，用于存储和编目常用特征，并允许模型间特征重用，从而减少耦合和重复。它们可以从如 Feast（Feast Authors 2022）和 Hopsworks（Hopsworks 2022）等开源工具、Vertex AI 和 Databricks 获得。

## 11.3 仪表板、可视化和报告

数据可视化是数据科学和机器学习的重要组成部分：它有助于解释复杂的数据，并使它们对用户和领域专家可理解，从而让他们能够参与管道的设计和维护（第五章）。正如第 11.2 节中所述，我们可以选择使用一系列解决方案来实现它，从用于数据探索的低级库到创建交互式仪表板和数据报告的更全面的可视化平台。

十年之久的 Matplotlib（Hunter 2022）库是 Python 中最广泛使用的用于基本数据可视化的包，其次是它的后继者 Seaborn（Waskom 2022），它试图解决 Matplotlib 的一些复杂性，同时产生更具现代外观的图形。

在更高层次上，我们有 Python 的 Plotly（Plotly 2022c）、Bokeh（Bokeh 2022）和 Altair（Altair 2022）库，以及 R 的 ggplot2 包（Wickham 2022a）。这些库具有类似的功能和美学，可以创建静态、动画和交互式可视化。Plotly、Bokeh 和 ggplot2 是程序性的；Altair 使用 Vega-Lite（Satyanarayan et al. 2022）语言的声明性 JSON 语法和一组简单的 API 来实现“图形语法”（Wilkinson 2005），这启发了 ggplot2 的设计。ggplot2 有一个名为 Plotnine 的 Python 端口（Kibirige 2022），Altair 有一个基于 reticulate 包（Ushey, Allaire, and Tang 2022）的 R 包装器（Lyttle, Jeppson, and Altair Developers 2022）。

这些包是构建更高级的 Web 仪表板（如 Dash（Plotly 2022b）、Bokeh Server 和 Shiny（Chang et al. 2022））的基础。Dash 提供了 Jupyter 笔记本和多种语言（如 Python、R 和 Julia）的接口，而 Bokeh 仅支持 Python。这两个库都是创建仪表板的良好起点，尽管 Plotly 的学习曲线更快。另一方面，Shiny 由于其与 RStudio 和 R Markdown 的深度集成，成为了在 R 中创建基于 Web 的交互式可视化的实际标准。其他开源选项包括 Voilà（Voilà Dashboards 2022）、Streamlit（Streamlit 2022）和 Panel（Holoviz 2022）。Voilà可以将 Jupyter 笔记本转换为独立的应用程序和仪表板，这在生成快速数据分析报告时很有用。Streamlit 和 Panel 通过组合来自 Plotly、Bokeh 和 Altair 的控件、表格和图形以及可查看的对象和控制来构建与数据交互的 Web 仪表板。与 Streamlit 和 Voilà相比，Panel 对 Jupyter 笔记本的支持更好。

用于视觉分析和商业智能的应用程序，如 Tableau（Tableau Software 2022）和 Microsoft PowerBI（Microsoft 2022e），也适用于创建仪表板，并且对于需要创建自己的仪表板但可能不太熟悉编程的管理人员或领域专家特别有用。Tableau 可以通过 TabPy（Tableau 2022）在运行时执行 Python 代码，并在 Tableau 可视化中显示其输出。另一方面，PowerBI 尚未与 Python 完全集成：它仅允许在 Jupyter 笔记本中放置报告，但笔记本中的数据与 PowerBI 报告之间没有直接连接。

最后，我们可以利用标准的监控和报告工具，如 Prometheus（Prometheus Authors and The Linux Foundation 2022）和 Grafana（GrafanaLabs 2022），来显示与数据、特征和模型相关的指标。我们在第 5.3.6 节中讨论了监控管道的每个部分的重要性：这使得我们很可能会已经使用 Prometheus 和 Grafana 来监控其他事物，我们不妨也用它们来跟踪和比较不同环境中的数据和模型，以及其他指标。这种方法无疑是稳健的：它使用经过高度测试的组件。然而，它需要大量的工程努力来将训练和部署模块与 Prometheus 集成，并将仪表板集成到服务器端基础设施中。我们可以使用 Google Vertex AI 上的 TFX 验证模块构建类似的设置，该模块实现了训练-部署偏差检测（TensorFlow 2022d），或者使用带有其监控功能的 Amazon Sagemaker（Amazon 2022b）。
