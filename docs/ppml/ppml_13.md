# 第七章 打包和部署流程

> 原文：[`ppml.dev/deploying-code.html`](https://ppml.dev/deploying-code.html)

将机器学习模型打包成工件是使流程可重复的重要步骤。这也使得模型更容易部署，即，将它们带入生产（或另一个目标）系统并投入使用。选择合适的打包格式和部署策略的组合，确保我们可以构建基于 CI/CD 解决方案（Duvall, Matyas, and Glover 2007）来高效且有效地完成这项工作。我们的最终目标是自信地交付流程，因为我们已经设计（见第五章）、实现（见第六章）、文档化（见第八章）和测试了它（见第九章）。

模型与代码一样，是机器学习流程的一部分，它们以类似传统软件的方式打包（见 7.1 节）和部署（见 7.2 节和 7.3 节）。然而，它们的行为预测性较低（见 5.2 节和 9.2 节）：当它们部署和在生产环境中运行时（见 7.4 节），我们应该对其进行监控。我们还应该为它们失败的情况制定应急计划（见 7.5 节），以便我们可以将流程恢复到功能状态（见 7.6 节）。

## 7.1 模型打包

模型可以存储到不同类型的工件中，正如我们在 5.3.4 节中简要讨论的那样。模型工件可以以不同程度从底层机器学习系统中抽象出来，有多种方式将其集成到流程中。

### 7.1.1 独立打包

最简约的打包形式仅仅是机器学习框架产生的产物，我们用它来训练模型：例如，TensorFlow 的 SavedModel 文件（TensorFlow 2021a）或 ONNX 文件（ONNX 2021）。这样的文件易于提供给第三方，并且方便嵌入到库或（桌面或移动）应用程序中，例如使用 Apple Core ML 框架（Apple 2022）。它们也可以通过通用的工件注册表（如 GitHub 提供的那些）作为独立包分发，例如 GitLab（GitLab 2022b）或 Nexus（Sonatype 2022）。跟踪训练模型的版本、其参数、其配置及其依赖项的任务委托给支持管道的配置管理平台（第 5.3.5 节）。

### 7.1.2 编程语言包管理器

由于成熟的、多功能的框架（如 TensorFlow 2021a 和 PyTorch 2019）的可用性，Python 已经成为机器学习应用中最受欢迎的编程语言（第 6.1 节）。因此，将模型作为 Python 包分发以简化部署过程，并使模型依赖于特定版本的 Python 解释器和那些框架变得越来越普遍。在整个管道中这样做有助于避免多语言编程带来的技术债务（第 5.2.4 节）。在实践中，这涉及到按照 Python 标准（称为“分发包”）分发包、模块和资源文件，使用 Setuptools（Python Packaging Authority 2022）和 Pip（Python Software Foundation 2022a）等工具来安装它们，并且可能将它们上传到中央 Python 包索引以使其易于访问。

### 7.1.3 虚拟机

![类型 1 和类型 2 虚拟化架构。](img/6545b549667e508a3380b4b811c253c6.png)

图 7.1：类型 1 和类型 2 虚拟化架构。

所有现代 CPU（见第 2.1.1 节）都实现了指令集以支持硬件虚拟化：例如，英特尔 CPU 有虚拟化技术（VT-x），而 AMD CPU 有 AMD-V。这使得*虚拟机*（VMs，也称为“客户操作系统”）在本地硬件上成为一个方便的选择，并导致了云实例的广泛可用。虚拟机运行在*虚拟机管理程序*之上，这是一种专门软件，允许多个客户系统共享单个计算系统（宿主硬件）。虚拟机就像一个正常的计算系统：主要区别在于它的 CPU、内存、存储和网络接口通过虚拟机管理程序与底层硬件共享，虚拟机管理程序根据需要将它们分配给客户。vSphere（VMware 2022）、KVM（Open Virtualization Alliance 2022）和 HyperV（Microsoft 2022h）是虚拟机管理程序的一些例子（图 7.1，左侧面板）：它们直接在宿主硬件上运行，要么作为独立的软件组件，要么集成在宿主操作系统内。虚拟机管理程序（图 7.1，右侧面板）如 Virtual box（Oracle 2022）和 VMware Workstation（VMware 2022），另一方面，运行在宿主操作系统之上。这两种类型都限于执行为它们运行的同一类型 CPU 编译的应用程序。

多亏了硬件虚拟化，虚拟机可以在宿主 CPU 上运行，并且可以通过 PCIe 直通（GPU 是一个典型的例子，见第 2.1.1 节）以有限的开销访问宿主的硬件资源。通过从（完全）虚拟化迁移到*半虚拟化*，可以进一步减少开销，这种做法以牺牲客机的完全隔离为代价，换取更好的吞吐量和延迟。现在，客户操作系统已经意识到它正在虚拟化环境中运行，并且可以使用特殊的系统调用集（*超调用*）和 I/O 驱动程序（特别是存储和网络）来直接与虚拟机管理程序通信。

虚拟机（VMs）是我们第 5.3.4 节 5.3.4 中提到的第二种工件。我们可以从头开始创建它们，安装和配置操作系统以及我们需要的所有库，或者我们可以从预先配置了大部分所需软件的*预配置镜像*开始。对于前者，我们有像 Hashicorp Packer（HashiCorp 2022a）或 Vagrant（HashiCorp 2022d）这样的工具，它们可以安装操作系统，以及配置管理软件如 Ansible（Ansible Project 2022），它们可以安装模型以及它们所依赖的软件栈。至于后者，云服务提供商提供了大量的预配置镜像：例如亚马逊机器镜像（AMIs）的目录（Amazon Web Services 2022b）。虚拟机配置和镜像通常存储在标准化的开放格式中，如开放虚拟化格式（OVF）（DMTF 2022）。最后，虚拟机可以通过机器学习管道的编排器通过虚拟机和相关的软件工具自动管理，这些工具可以创建、克隆、快照、启动和停止单个虚拟机。

虚拟机提供三个主要优势：

+   它们易于操作：我们可以在同一主机上运行不同操作系统和不同软件栈的多个实例，使用预配置镜像合并它们的配置，并将它们作为单个实体集中管理。

+   它们也可以轻松地进行扩展以处理峰值负载，无论是通过启动新的虚拟机（*水平扩展*）还是通过增加它们可以访问的硬件资源（*垂直扩展*，第 2.4 节）。

+   它们可以被移动到另一个主机（*可移植性*），并且易于快照，便于在硬件故障的情况下进行灾难恢复。

然而，虚拟机有一个重要的缺点：它们包含整个操作系统，因此需要大量的热存储。因此，虚拟机的部署时间可以从最佳情况下的几十秒到平均情况下的几分钟（Hao, anang, and Kim 2021），这取决于云提供商或本地虚拟机管理程序的配置。

### 7.1.4 容器

相比之下，*容器*更轻量级（Espe et al. 2020），因为它们仅虚拟化操作系统上运行的库和应用程序，而不是整个机器学习系统（图 7.2）。它们不是由虚拟机管理程序管理，而是由*容器运行时*（有时称为“容器引擎”）如 Docker（Docker 2022a）管理，它控制对主机硬件和操作系统的访问。

容器运行时通常建立在一系列 Linux 内核功能之上（Rice 2020）：

+   *命名空间*：一种隔离层，允许每个进程只能看到和访问运行在同一命名空间中的宿主进程、目录和系统资源。

+   *Cgroups（控制组）*：一种资源管理层，为进程集合设置和限制 CPU、内存和网络带宽。

+   *Seccomp（安全计算）*：一种安全层，限制容器只能访问系统调用（内核的 API）的受限子集。

![虚拟化和容器的高级架构](img/f47c9c5be4812ae58329dc5f4d8e69bd.png)

图 7.2：虚拟化和容器的高级架构。

与虚拟机的情况一样，容器可以将机器学习应用程序及其所有相关库、依赖项和工具打包在一个单一的自包含工件中：一个设计上不可变、无状态和短暂的*容器镜像*。¹⁶在 Docker 的情况下，我们通常称之为*Docker 镜像*。容器镜像是由声明性配置文件创建的，也称为`Dockerfile`，它定义了所有必要的命令。每个命令都产生一个不可变的*层*，反映了该命令本身对镜像引入的变化，允许进行增量更改并最小化磁盘空间使用。此过程的起点是提供精简环境（不是完整的操作系统，如预制的虚拟机镜像那样）的*基础镜像*，我们可以向其中添加我们的模型以及补充它们的库、工具和应用程序。

下面是一个创建 FastAPI RESTful 应用程序（创建 Web 服务和 API 的框架）镜像的`Dockerfile`示例。为了保证可重复性，该`Dockerfile`及其引用的`requirements.txt`文件应存储在配置管理平台下的版本控制中（第 6.4 节）。

```py
FROM python:3.10.6-bullseye

WORKDIR /app

COPY requirements.txt .
RUN pip3 install --no-cache-dir  -r requirements.txt

COPY . .

CMD [ "uvicorn", "main:app", "--host=0.0.0.0"]
```

首先，`Dockerfile`明确标识了它生成的镜像的系统依赖项。第一行，“`FROM python:3.10.5-bullseye`”标识了一个基于 Debian GNU/Linux 稳定发布版“Bullseye”和 Python 解释器 3.10.5 版本的基镜像。其次，它标识了我们依赖的 Python 包。第三和第四行，“`COPY requirements.txt .`”和“`RUN pip3 install -r requirements.txt`”，将列出 Python 依赖项的文件`requirements.txt`复制到镜像中，并使用 Python 包管理器（`pip`）进行安装。确保列出所有依赖项并将其固定到我们已测试的确切版本非常重要，以避免累积技术债务（章节 5.2.4 和 6.3）。如果我们升级一个或多个依赖项，相应的容器层将无效。Docker 在创建层时会进行缓存：那些未受我们更改影响的层将从缓存中获取，而不是从头开始重新创建。第二行（“`WORKDIR /app`”）将工作目录更改为包含应用程序文件的目录，第五行（“`COPY . .`”）将它们复制到容器镜像中，最后一行定义了容器启动时运行的命令。

在构建成功后，我们可以将容器存储到诸如 Docker registry（Docker 2022b）或 Harbour（Harbor 2022）这样的*容器注册库*中。容器注册库是提供标准化 API 的服务器应用程序，用于上传（推送）、版本控制（标记）和下载（拉取）容器镜像。注册库的结构组织成仓库（如 Git（Git 开发团队 2022）），每个仓库都保存了特定容器镜像的所有版本。容器的运行时、注册库和镜像规范基于开放容器倡议（OCI）（开放容器倡议 2022），这是 Linux 基金会的一个开放标准，因此可以在不同平台和供应商之间高度可移植。

与任何其他软件工件一样，容器镜像可能存在安全漏洞（Bhupinder 等人 2021），这些漏洞可能来自过时基础镜像中的易受攻击库、不受信任容器注册表中的恶意镜像或易受攻击的 `Dockerfile`。为了识别这些漏洞，我们应该执行 *合规性和安全检查* 以验证 `Dockerfile`，例如使用 Hadolint（The Hadolint Project 2022）工具，以及生成的镜像，使用静态分析和镜像扫描工具，如 Trivy（Aquasecurity 2022）。云服务提供商，如 Amazon AWS（服务 2022）和 Google Cloud（Google 2022b），拥有公共容器注册表，其中包含安全且经过测试的基础镜像，这些镜像从基本的操作系统安装到基于 TensorFlow（TensorFlow 2021a）和 PyTorch（Paszke 等人 2019）预配置的机器学习堆栈不等。

容器运行时与编排器集成，以允许无缝使用容器镜像。编排器负责管理容器集群，包括部署、扩展、网络和安全策略。容器负责提供不同功能模块作为模块化和解耦的服务，通过网络进行通信，可以独立部署，并且具有高度的可见性。本质上，这是微服务架构（Newman 2021）。此外，容器运行时与 CI 集成，以实现可重复的软件测试：基础容器镜像提供了一个干净的环境，确保测试结果不受外部因素的影响（第 9.4 和 10.3 节）。

Kubernetes（The Kubernetes Authors 2022a）在编排器中是事实上的标准。¹⁷ 专注于机器学习管道的编排器将 Kubernetes 与实验跟踪和模型服务集成，以提供完整的 MLOps 解决方案：两个例子是更集成的 Kubeflow（The Kubeflow Authors 2022）和更程序化的 MLflow（Zaharia 和 The Linux Foundation 2022）。容器运行时通过实现从物理主机到容器的 GPU 透传（在 Docker 的情况下使用“`--gpus`”标志）来增强它们。Kubernetes 可以使用此功能来为每个容器应用适当的 *标签选择器*（The Kubernetes Authors 2022b），并在具有适当硬件的机器学习系统上调度训练和推理工作负载（第 2.1.1 节）。

## 7.2 模型部署：策略

*部署策略*或*部署模式*是一种在最小化停机时间和对用户影响的情况下，在生产环境中替换或升级工件或服务的技巧。在这里，我们将关注如何在不影响其消费者（即最终用户和依赖于模型输出的管道模块）的情况下部署机器学习模型（第 5.3.5 节）。显然，这与传统软件的部署有相似之处：我们希望通过 CI/CD 实现自动化和可重复的发布，在大多数情况下使用容器作为工件（第 7.1.4 节）。此外，机器学习管道的部分实际上是传统软件，并以这种方式进行部署。

模型部署可以利用现代软件的渐进式交付策略。通常，管道将包含每个模型（例如，版本`A`）的多个实例，以便能够并行处理多个推理请求和数据准备队列。因此，我们最初可以用一个新模型（例如，版本`B`）替换这些实例中的一个小子集。如果没有出现任何问题，我们随后逐渐替换剩余的实例：新模型已经有效地通过了验收测试（第 9.4.4 节）。如果出现任何问题，我们的日志和监控设施（第 5.3.6 节）将记录我们需要的用于故障排除的信息。我们还可以同时部署多个模型，以比较它们在准确性、吞吐量和延迟方面的性能。因此，渐进式交付加快了模型部署（通过减少预部署测试的数量），降低了部署风险（因为大多数消费者不会受到初始部署中可能出现的问题的影响），并使回滚更容易（第 7.6 节）。

![蓝绿（左侧）、金丝雀和 A/B 测试（右上角）以及影子（右下角）部署策略](img/4aa8111e621b018d7623f2057a25b4bc.png)

图 7.3：蓝绿（左侧）、金丝雀和 A/B 测试（右上角）以及影子（右下角）部署策略。

我们可以使用多种相关的部署策略来实现渐进式交付（Tremel 2017）：

+   蓝绿部署模式（Humble 和 Farley 2011）假设我们正在使用一个路由器（通常是负载均衡器）来将请求分散到一组实例中，这些实例提供机器学习模型版本 `A` 的服务（图 7.3，左侧）。当我们部署模型的新版本 `B` 时，我们创建第二个实例池来提供服务，并将新到达请求的一部分发送到这个新池。如果没有出现任何问题，路由器将逐渐将越来越多的请求发送到提供模型 `B` 的池，而不是提供模型 `A` 的池。正在由模型 `A` 处理的现有请求允许完成，以避免中断。提供模型 `A` 的池最终将不再分配任何请求，并可能随后被退役。如果出现任何问题，回滚很简单：我们可以将所有请求再次发送到提供模型 `A` 的池。将两个池保留在单独的环境或甚至单独的机器学习系统中将进一步降低部署风险。

+   我们已经在 5.3.4 节中提到了 *金丝雀* 部署模式（Humble 和 Farley 2011）：与蓝绿模式的主要区别在于，我们在已经提供模型 `A` 服务的同一池中部署了带有模型 `B` 的实例（图 7.3，顶部右侧）。路由器将一小部分请求重定向到带有模型 `B` 的实例，并注意会话亲和性。¹⁸ 其他请求作为我们的 *对照组*：我们可以在相同的环境中检查和比较两个模型的性能，而没有任何偏见。再次强调，如果没有出现任何问题，我们可以逐渐退役带有模型 `A` 的实例。金丝雀部署通常比其他部署模式慢，因为收集足够关于模型 `B` 性能的数据需要时间。然而，它们提供了一个简单的方法，在真实数据和与现有模型相同的环境中测试新模型。

+   在 *影子* 部署（Microsoft 2022g）中，一个新的模型 `B` 与模型 `A` 并行部署，并且每个请求都发送到这两个模型（图 7.3，右下角）。我们可以通过它们从相同输入产生的输出、延迟以及我们通过日志和监控收集的任何其他指标来比较它们的准确性。实际上，我们可以并行部署多个模型来测试不同的方法，并仅保留表现最好的模型。因此，影子部署要求我们为每个测试的模型设置不同的 API 端点，并分配足够的硬件资源来处理增加的推理工作量。然而，它允许在不干扰操作的情况下测试新模型。

+   在*滚动*或*阶梯式*部署模式中，我们只是在预定的时间表上分批替换模型`A`的实例，直到所有运行的实例都在服务模型`B`。滚动部署既容易安排，也容易回滚。

+   另一种我们在其他地方提到的部署模式（第 5.3.4 节和第 9.4.3 节）是*A/B 测试*（Amazon 2021; Zheng 2015）：路由器随机将请求 50%-50%分配给两个模型`A`和`B`，我们评估每个模型的相应指标，并且只有在模型`B`优于模型`A`的情况下，我们才提升模型`B`。与金丝雀部署的关键区别在于，在后者中，只有一小部分请求被发送到带有模型`B`的实例，以降低部署风险：分配比例是 90%-10%或最多 80%-20%（图 7.3，右上角）。

+   *销毁并重新创建*是最基本的部署策略：我们停止所有带有模型`A`的实例，并从头开始创建一组新的实例，用模型`B`部署以替代它们。结果，管道将不可用，执行多个请求序列的消费者可能会收到不一致的输出。

我们可以通过向我们的模型添加功能标志（第 6.5 节）来整合这些部署模式：然后模型`A`和`B`可以共享大量代码。这样，我们只需切换不同的标志组合，就可以轻松创建新的模型，而无需构建和部署任何新工件。然而，在渐进式交付过程中，这两个模型将同时提供服务：所有消费者都应该支持它们的 API，或者模型`B`应该完全与模型`A`向后兼容。

## 7.3 模型部署：基础设施

在机器学习管道中，模型部署是管道编排的一部分，它使得模型能够在开发、测试和生产环境中部署和提供服务（第 5.3.5 节）。理想情况下，它应该通过 CI/CD 完全自动化，以避免像 Knight Capital (.Seven 2014)那样的灾难性失败，我们在第 5.2.3 节中提到了这一点。

CI/CD 中持续部署部分的性质可能因部署的工件类型（第 7.1 节）和计算系统类型（第 2.4 节）而异。我们的工件可能是封装并通过 API 提供服务的容器镜像：我们可以通过手动调用 Docker 在本地部署它们，或者通过指示 Kubernetes 调用存储在 CI/CD 配置中的自动化脚本来远程部署。在这两种情况下，如果本地不可用，则会在部署时从注册表中获取镜像。我们的工件也可能是虚拟机：在这种情况下，持续部署可以利用配置管理工具如 Ansible（Ansible 项目 2022）来部署和升级它们。在这两种情况下，CI/CD 管道标准化了部署过程，隐藏了本地和云环境之间的差异（第 2.3 节），并将复杂性从粘合代码转移到声明性配置文件（第 5.2.3 节）。这已经将部署过程标准化到几乎与目标编排平台如 Kubernetes（Kubernetes 作者 2022a）和商业提供商如 Amazon AWS ECS 相同的程度。

我们还可以在集成的 MLOps 平台上运行机器学习管道：模型部署完全取决于平台的主张工作流程。例如，Databricks（Databricks 2022）这样的 MLOps 平台通过 MLflow（Zaharia 和 The Linux Foundation 2022）集成了许多开源组件，并通过支持多个部署目标的 API 将它们封装起来。这些 API 提供了一个标准化的接口，类似于 Docker 和 Kubernetes，无论我们选择哪个目标。云供应商的机器学习平台（“机器学习即服务”）如 Azure ML（Microsoft 2022c）或 Amazon AWS SageMaker（Amazon 2022d）提供了更高层次的抽象。一方面，它们对我们如何实现管道以及如何部署模型几乎没有控制权。另一方面，它们对没有技能或预算来管理自己的 CI/CD、监控和日志记录基础设施的团队来说是可访问的。它们还提供了一个实验跟踪的 Web 界面（以及一个用于程序化使用的 API），用于测试新模型以及可视化它们的参数和性能指标。

## 7.4 模型部署：监控和日志记录

我们应该通过我们的日志和监控基础设施跟踪自动化模型部署的所有阶段，以实现我们诊断可能遇到任何问题的可观察性（第 5.3.6 节）。所有持续部署平台都允许这样做：MLflow（Zaharia 和 Linux 基金会 2022）有 MLflow 跟踪，Airflow（Apache 软件基金会 2022a）可以使用 Fluentd（Fluentd 项目 2022），以及像 GitLab 这样的通用 CI/CD 解决方案内置了发布指标和日志事件的机制，以及 Prometheus（Prometheus 作者和 Linux 基金会 2022）的支持。记录每个模块的每个入口和出口点，以及任何重试和管道中所有任务的顺利完成是至关重要的：我们应该能够构建包含全面堆栈跟踪的描述性活动报告。机器学习管道有许多移动部件，可能在许多不同的地方以难以诊断的方式失败（第 9.1、9.2 和 9.3 节）。此外，日志应该自动触发外部通知系统，如 PagerDuty（PagerDuty 2022），以便在部署过程中尽早意识到任何问题。

模型部署后，我们应该检查它是否正在提供服务，是否准备好接受推理请求（就绪性）以及它是否产生正确的结果（活性）。我们用来提供模型服务的软件可能暴露一个健康检查 API（如 Kubernetes 中的就绪性和活性探测（Kubernetes 作者 2022a）），协调器可以使用它来仅将推理请求路由到能够处理它们的模型。模型内部的监控客户端可以通过公开指标来达到相同的目的，以检查性能是否随时间退化。正如我们在第 5.3.6 节中讨论的，我们应该将日志和监控服务器放置在专用系统上，以确保它们不受模型引起的任何问题的干扰，并且可以用来对发生错误的原因进行根本原因分析。

## 7.5 可能出什么问题？

当我们部署一个新的模型时，由于不同的原因，可能会出现许多问题：对部署过程或其目标缺乏控制或可观察性（见第 2.4 节）；手动执行部署前或部署后的操作（见第 5.2.3 节）；或者模型或模块中的关键缺陷在软件测试套件中漏网（见第 9.4 节）。我们可以通过利用 CI/CD（见第五章）和遵循现代开发实践（见第六章）来最小化部署风险，但某些问题可能无法完全解决或甚至无法自动检测。

*硬件资源可能不可用。* 我们部署的环境可能运行在资源不足的机器学习系统上（例如，存储空间或内存不足），硬件故障或网络连接问题（例如，系统本身无法访问，或它们无法访问模型所需的远程第三方资源）。¹⁹这些问题可能出现在本地（本地化）和远程（云）环境中；在后者中，通常通过安排新的部署来解决这些问题，因为底层硬件将发生变化（见第 2.3 节）。

*硬件资源可能无法访问。* 机器学习系统可能没有问题，但存在访问限制，阻止我们使用它们。防火墙可能阻止我们在网络上连接到它们；文件权限可能阻止我们从它们的存储中读取数据和配置。这是云实例和管理服务中常见的问题，因为它们的身份和访问管理（IAM）策略难以编写和理解。实际上，通常只能通过交互式测试控制认证和授权这些服务的配置，这使得它们容易意外中断。因此，已经有许多机器学习工程师移除了过多的访问限制，导致 AWS 上的 S3 存储桶中充满了公开可访问的个人数据（Twilio（The Register 2020）和 Switch（VPNOverview 2022）是近年来两个引人注目的例子）。这显然是不理想的，但可以通过根据*最小权限原则*编写 IAM 策略，通过配置管理工具（见第 11.1 节）跟踪它们，并在应用之前将它们纳入代码审查（见第 6.6 节）来防止这种情况发生。

*人们不互相交流。* 模型部署是我们实际应用我们训练的模型及其支持代码的时候。因此，这也是当领域专家、机器学习专家、软件工程师和用户之间缺乏沟通而产生的缺陷可能暴露出来的时候。范围和设计管道（第 5.3 节）、验证机器学习模型（第 5.3.4 节）和推理输出（第 5.3.6 节）、设计和命名模块及其参数（第 6.2 节）、代码审查（第 6.6 节）以及编写各种形式的文档（第八章）都应该是所有参与和使用管道的人的协作努力。当这种协作不有效时，不同的人将负责管道的不同部分，而由此产生的缺乏协调可能会导致不同责任区域边界的各种问题。机器学习工程师可能会在没有咨询领域专家（“这些模型有意义吗？我们是否有正确的数据来训练它们？”）或软件工程师（“这些模型能在可用的系统上运行并产生足够低的延迟的推理吗？”）的情况下开发模型。领域专家可能无法将他们的专业知识传达给机器学习工程师（“这个模型类别无法表达一些相关的领域事实！”）或软件工程师（“这个变量应该以特定的方式编码才有意义！”）。软件工程师可能会在实现机器学习模型时采取自由行动，改变其统计属性，而机器学习工程师没有注意到（“我可能可以用这个其他的库…或者自己重新实现它可能更快！”）或者以使领域专家难以理解的方式结构化代码（“这个`theta_hat`参数是什么意思？”）。角色的分割是一种应该不惜一切代价避免的组织反模式，而应该支持 DevOps 最初倡导的共享责任和持续共享技能和知识。

*缺失的依赖项。* 模块的部署可能失败，因为其中一个或多个依赖项（在管道内部或外部）缺失或未正常工作。例如，如果模块`A`需要模块`B`的输出作为输入，我们应该在部署模块`A`之前确保模块`B`存在且处于工作状态。在实践中，这需要两个模块的*协调部署*，这是我们努力使模块彼此解耦时的反模式。当然，我们也可以在模块`A`中实施适当的重试策略，使其能够抵御模块`B`暂时离线的情况。在 Kubernetes（Kubernetes 作者 2022a）上，我们可以使用存活性和就绪性探测（第 7.4 节）以及“初始化容器”（在 Pod 中的应用容器之前运行的专用容器）来实现这一目的。

*不完整或不正确的配置管理。* 配置管理工具（第 10.1 节和 11.1 节）促进并自动化模板、环境变量和配置文件的复用。然而，这意味着我们应该小心地将对应不同环境的这些内容分别存储，并始终保持它们干净和完整。在一个由许多模块和环境组成的复杂管道中，很容易错误地使用与我们意图不符的不同环境的配置。在最佳情况下，我们试图做的事情会失败，并记录异常。在最坏的情况下，我们似乎成功地完成了我们试图做的事情，但结果却默默地错误，因为我们访问的资源与我们认为的不同。例如，我们可能无意中通过访问训练数据而不是验证数据导致信息泄露。类似的*配置错误*问题可能涉及管道的任何部分（训练、软件测试、推理等）以及配置管理跟踪的任何实体（数据库引用、机密、模型参数、特征等）。

## 7.6 回滚

当在生产中部署的模型未能达到所需性能和质量标准（第 8.3 节）时，我们有两个选择：要么用仍然适合使用的先前模型替换它（*回滚*），要么用我们专门训练以解决当前模型失败原因的新模型（*向前滚动*）。在以下内容中，我们将重点关注回滚，但我们的讨论对于模型向前滚动同样适用。

模型回滚仅在模型 API 在版本之间向后兼容时才可行。这样，我们的模型任何版本都可以在任何时间点恢复到任何之前的版本，而不会干扰到整个管道的其他部分，因为我们能保证模型提供相同的功能，具有相同的协议规范和相同的签名。实现向后兼容需要大量的软件工程规划和努力。除了将模型封装在一个抽象和标准化的接口容器中，封装其特性和实现外，我们还需要一个实验管理平台，该平台对管道模块、模型及其相应的配置进行版本控制。至少，这样的设置包括一个模型注册表（第 5.3.4 节）和代码的版本控制系统（第 6.5 节）。

有时保持向后兼容性根本不可能：如果我们用一个完全不同模型类的模型替换另一个模型，或者如果模型训练的任务已经改变，API 应该改变以反映新的模型能力和目的。我们可以通过版本控制来在两组不同的 API 之间进行过渡。例如，旧的一组 API 可能从 URL 路径`https://api.mlmodel.local/v1/`提供，而新的一组可能从`https://api.mlmodel.local/v2/`提供，并且旧 API 可能会发出警告来表示它们已被弃用。（OpenAPI 支持弃用 API“操作”（SmartBear Software 2021））。然后我们可以使用在第 7.2 节中讨论的策略部署新的、不兼容的模型，并且管道模块将能够同时访问这两组 API，而不会对它们使用哪个版本有任何混淆。这反过来又使得能够有序地更新单个模块成为可能。

如果一个模型与版本化的 API 一起打包内置配置，那么加载它的函数应该支持旧版本。同样，如果一个模型是状态性的并且需要访问数据库以检索资产和配置，那么访问这些资源的函数应该能够处理不同的数据库模式。我们执行回滚的能力将取决于我们执行数据库迁移的能力。

是否应该手动回滚（即，由人工在环域专家触发）或自动回滚（即，由监控基础设施收集的指标触发的管道编排器）并不是一个简单的决定。从技术角度来看，我们应该评估我们计划使用的部署策略对我们计划使用的部署策略的影响，即管道恢复到完全功能状态所需的时间。从业务角度来看，领域专家在请求回滚之前可能需要更多的确凿证据：在获取更多数据点和更好地理解模型不再准确的根本原因时，他们可能对表现不佳的模型可以接受。机器学习专家可以通过部署具有金丝雀或影子部署策略的替代模型来帮助在此期间调查其性能，并将其与失败模型的性能进行比较。唯一一个自动回滚显然是最好的选择的情况是，模型的性能不佳不是由数据或推理请求的变化引起的，而是由管道下层的硬件和软件基础设施问题引起的。（例如，新部署的模型使用了过多的内存或变得无响应。）即使在这样的情况下，回滚的决定也应该由监控和日志证据支持（第 5.3.6 节）。

### 参考文献

Alquraan, A., H. Takruri, M. Alfatafta, and S. Al-Kiswany. 2018\. “An Analysis of Network-Partitioning Failures in Cloud Systems.” In *13th USENIX Symposium on Operating Systems Design and Implementation (OSDI 18)*, 51–68.

Amazon. 2021\. *使用 Amazon SageMaker MLOps 项目进行机器学习模型的动态 A/B 测试*. [`aws.amazon.com/blogs/machine-learning/dynamic-a-b-testing-for-machine-learning-models-with-amazon-sagemaker-mlops-projects/`](https://aws.amazon.com/blogs/machine-learning/dynamic-a-b-testing-for-machine-learning-models-with-amazon-sagemaker-mlops-projects/).

Amazon. 2022d. *机器学习：Amazon Sagemaker*. [`aws.amazon.com/sagemaker/`](https://aws.amazon.com/sagemaker/).

Amazon Web Services. 2022b. *Amazon Machine Images (AMI)*. [`docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html`](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html).

Ansible Project. 2022\. *Ansible 文档*. [`docs.ansible.com/ansible/latest/index.html`](https://docs.ansible.com/ansible/latest/index.html).

Apple. 2022\. *TensorFlow 2 转换*. [`coremltools.readme.io/docs/tensorflow-2`](https://coremltools.readme.io/docs/tensorflow-2).

Aquasecurity. 2022\. *Trivy 文档*. [`aquasecurity.github.io/trivy/`](https://aquasecurity.github.io/trivy/).

Bhupinder, K., M. Dugré, A. Hanna, and T. Glatard. 2021\. “An Analysis of Security Vulnerabilities in Container Images for Scientific Data Analysis.” *GigaScience* 10 (6): giab025.

Databricks. 2022\. *Databricks 文档*. [`docs.databricks.com/applications/machine-learning/index.html`](https://docs.databricks.com/applications/machine-learning/index.html).

DMTF. 2022\. *开放虚拟化格式*. [`www.dmtf.org/standards/ovf`](https://www.dmtf.org/standards/ovf).

Docker. 2022a. *Docker*. [`www.docker.com/`](https://www.docker.com/).

Docker. 2022b. *Docker 注册库 HTTP API V2 文档*. [`docs.docker.com/registry/spec/api/`](https://docs.docker.com/registry/spec/api/).

Duvall, P. M., S. Matyas, and A. Glover. 2007\. *持续集成：提高软件质量和降低风险*. Addison-Wesley.

Espe, L., A. Jindal, V. Podolskiy, and M. Gerndt. 2020\. “容器运行时性能评估。” In *第 10 届国际云计算与服务科学会议论文集*，273–81.

GitHub. 2022c. *与容器注册库一起工作*. [`docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry`](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry).

GitLab. 2022b. *GitLab 容器注册库*. [`docs.gitlab.com/ee/user/packages/container_registry/`](https://docs.gitlab.com/ee/user/packages/container_registry/).

Google. 2022b. *深度学习容器*. [`cloud.google.com/deep-learning-containers`](https://cloud.google.com/deep-learning-containers).

Hao, J., T. Jiang anang, and K. Kim. 2021\. “公共 IaaS 云中虚拟机启动时间的实证分析：扩展报告。” In *第 14 届 IEEE 国际云计算会议论文集*，398–403.

Harbor. 2022\. *Harbor 文档*. [`goharbor.io/docs/`](https://goharbor.io/docs/).

HashiCorp. 2022a. *Packer 文档*. [`www.packer.io/docs`](https://www.packer.io/docs).

HashiCorp. 2022d. *Vagrant 文档*. [`www.vagrantup.com/docs`](https://www.vagrantup.com/docs).

Humble, J., and D. Farley. 2011\. *持续交付*. Addison Wesley.

Microsoft. 2022c. *Azure 机器学习*. [`azure.microsoft.com/en-us/services/machine-learning/`](https://azure.microsoft.com/en-us/services/machine-learning/).

Microsoft. 2022g. *影子测试*. [`microsoft.github.io/code-with-engineering-playbook/automated-testing/shadow-testing/`](https://microsoft.github.io/code-with-engineering-playbook/automated-testing/shadow-testing/).

Microsoft. 2022h. *虚拟化文档*. [`docs.microsoft.com/en-us/virtualization/`](https://docs.microsoft.com/en-us/virtualization/).

Newman, S. 2021\. *构建微服务：设计细粒度系统*. O’Reilly.

ONNX. 2021\. *开放神经网络交换*. [`github.com/onnx/onnx`](https://github.com/onnx/onnx).

Open Container Initiative. 2022\. *开放容器倡议*. [`opencontainers.org/`](https://opencontainers.org/).

Open Virtualization Alliance. 2022. *文档*. [`www.linux-kvm.org/page/Documents`](https://www.linux-kvm.org/page/Documents).

Oracle. 2022. *Oracle VM Virtualbox*. [`www.virtualbox.org/`](https://www.virtualbox.org/).

PagerDuty. 2022. *PagerDuty：上线即金钱*. [`www.pagerduty.com/`](https://www.pagerduty.com/).

Paszke, A., S. Gross, F. Massa, A. Lerer, J. Bradbury, G. Chanan, T. Killeen, et al. 2019. “PyTorch: 一种命令式风格的、高性能的深度学习库。” In *Advances in Neural Information Processing Systems (Nips)*, 32:8026–37.

Prometheus Authors, and The Linux Foundation. 2022. *Prometheus：监控系统和时间序列数据库*. [`prometheus.io/`](https://prometheus.io/).

Python Packaging Authority. 2022. *使用 Setuptools 构建和分发包*. [`setuptools.pypa.io/en/latest/userguide/index.html`](https://setuptools.pypa.io/en/latest/userguide/index.html).

Python Software Foundation. 2022a. *PyPI：Python 包索引*. [`pypi.org/`](https://pypi.org/).

Rice, L. 2020. *容器安全：保护容器化应用程序的基本技术概念*. O’Reilly.

Services, Amazon Web. 2022. *AWS 深度学习容器*. [`aws.amazon.com/en/machine-learning/containers/`](https://aws.amazon.com/en/machine-learning/containers/).

.Seven, D. 2014. *Knightmare：一个 DevOps 警示故事*. [`dougseven.com/2014/04/17/knightmare-a-devops-cautionary-tale/`](https://dougseven.com/2014/04/17/knightmare-a-devops-cautionary-tale/).

SmartBear Software. 2021. *OpenAPI 规范*. [`swagger.io/specification/`](https://swagger.io/specification/).

Sonatype. 2022. *Nexus 仓库管理器*. [`www.sonatype.com/products/nexus-repository`](https://www.sonatype.com/products/nexus-repository).

TensorFlow. 2021a. *TensorFlow*. [`www.tensorflow.org/overview/`](https://www.tensorflow.org/overview/).

The Apache Software Foundation. 2022a. *Airflow 文档*. [`airflow.apache.org/docs/`](https://airflow.apache.org/docs/).

The Fluentd Project. 2022. *Fluentd: 开源数据收集器*. [`www.fluentd.org/`](https://www.fluentd.org/).

The Git Development Team. 2022. *Git 源代码镜像*. [`github.com/git/git`](https://github.com/git/git).

The Hadolint Project. 2022. *Hadolint: Haskell Dockerfile 检查器文档*. [`github.com/hadolint/hadolint`](https://github.com/hadolint/hadolint).

The Kubeflow Authors. 2022. *所有 Kubeflow 文档*. [`www.kubeflow.org/docs/`](https://www.kubeflow.org/docs/).

The Kubernetes Authors. 2022a. *Kubernetes*. [`kubernetes.io/`](https://kubernetes.io/).

The Kubernetes Authors. 2022b. *Kubernetes 文档：调度 GPU*. [`kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/`](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/).

The Register. 2020\. *Twilio：有人潜入我们未受保护的 AWS S3 存储桶，向我们的客户 JavaScript SDK 添加了可疑代码*. [`www.theregister.com/2020/07/21/twilio_javascript_sdk_code_injection`](https://www.theregister.com/2020/07/21/twilio_javascript_sdk_code_injection).

Tremel, E. 2017\. *Kubernetes 上的部署策略*. [`www.cncf.io/wp-content/uploads/2020/08/CNCF-Presentation-Template-K8s-Deployment.pdf`](https://www.cncf.io/wp-content/uploads/2020/08/CNCF-Presentation-Template-K8s-Deployment.pdf).

VmWare. 2022\. *VMware vSphere 文档*. [`docs.vmware.com/en/VMware-vSphere/index.html`](https://docs.vmware.com/en/VMware-vSphere/index.html).

VMware. 2022\. *VMware Workstation Pro*. [`www.vmware.com/products/workstation-pro.html`](https://www.vmware.com/products/workstation-pro.html).

VPNOverview. 2022\. *金融科技应用切换泄露用户交易和个人身份信息*. [`vpnoverview.com/news/fintech-app-switch-leaks-users-transactions-personal-ids`](https://vpnoverview.com/news/fintech-app-switch-leaks-users-transactions-personal-ids).

Wiggins, A. 2017\. *《十二要素应用》*. [`12factor.net`](https://12factor.net).

Zaharia, M. 和 The Linux Foundation. 2022\. *MLflow 文档*. [`www.mlflow.org/docs/latest/index.html`](https://www.mlflow.org/docs/latest/index.html).

Zheng, A. 2015\. *评估机器学习模型*. O’Reilly.

* * *

1.  容器是*短暂的*，这意味着它们应该被构建在它们可能随时崩溃的预期中。因此，它们应该易于（重新）创建和销毁，并且应该是*无状态的*：它们包含的任何有价值的信息在它们被销毁时将永远丢失。这些特性使它们成为“十二要素应用”（Wiggins 2017）和其他现代软件工程实践中的关键工具。↩︎

1.  在 Kubernetes 文档中，封装一个应用程序的一组一个或多个容器被称为“pod”。↩︎

1.  每个消费者或用户总是被提供相同的模型版本。这在蓝绿部署模式中是隐式发生的，因为每个消费者或用户都被分配到一个池中，并且每个池中的所有实例都提供相同的模型。↩︎

1.  由于网络设备或网络连接故障导致的计算系统、集群或数据中心之间的连接问题也被称为“网络分裂”或“网络分区”（Alquraan 等人 2018)↩︎
