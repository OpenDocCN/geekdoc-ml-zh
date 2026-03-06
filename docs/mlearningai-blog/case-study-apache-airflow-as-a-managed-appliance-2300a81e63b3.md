# 案例研究:Apache Airflow 作为受管理的设备

> 原文：<https://medium.com/mlearning-ai/case-study-apache-airflow-as-a-managed-appliance-2300a81e63b3?source=collection_archive---------6----------------------->

我们把我们的业务赌在我们的客户在他们自己的基础设施上运行我们的软件，而不是建立一个更传统的 SaaS 平台。所有的事情都是数据管理:主要是测试和演示环境(包括 ML、analytics 等)的数据管道化和 PII 移除。)、向利益相关者交付数据、数据库迁移/模式模拟、数据销毁等。简而言之，我们试图将自己定位为构建自动化数据工作流的强大工具包。我们是一家新公司，希望听到大家的意见，但我会就此打住，以免给人留下自私自利的推销印象！

我们认为，将这家公司建设成一个 SaaS 平台会有许多技术上的劣势:将数据传输到云中的数据安全问题、性能、成本开销等。似乎某些技术领域有像钟摆一样来回摆动的诀窍——例如经典的大型机与瘦客户机问题。相对而言，采用受管设备方法的公司并不多(尽管我们确实用我们的云托管 API 来补充我们的本地代理)，所以谁知道呢，也许我们是向另一个方向摆动的钟摆的一部分(对于非常具体的使用案例)或者只是我们的头脑出了问题！

气流和 Kubernetes 的存在确实让我们做出了这个决定，很难想象在没有这些技术的情况下建立这家公司。Airflow 当然可以横向扩展，以管理大量的作业和工作负载，但它以最小的资源占用量运行得如此之好，以及它在非常有限的硬件上对我们的适应性，给我们留下了深刻的印象。对 2.x 中的调度器和 2.3 中的动态任务映射特性的改进是巨大的改进，后者允许从云中远程管理我们的 Dag(稍后将详细介绍)。

我们认为可能会有观众对了解我们的成功策略感兴趣。

# 库伯内特和赫尔姆

我们是 Helm 的忠实粉丝，它是我们的客户如何安装/引导我们的代理软件的关键因素。我们的控制面板为客户构建了工作流配置，以及用于安装和更新代理软件的 helm 安装/升级命令。我们运行我们自己的 Chartmuseum 注册表，这样客户可以采取“如果它没有坏，就不要修复它”的方法，固定他们的版本，这样他们就可以在闲暇时升级(和降级)。用户可以复制和粘贴的单个命令(以及一次性 Docker 注册和 ChartMuseum 认证步骤)非常适合我们构建交钥匙式设备的方法。

受管理的 Kubernetes 服务(EKS、GKE、阿拉斯加等)的普及和普及。)是另一个重要因素，因此我们可以为尚未使用 Kubernetes 的客户提供一个设置此基础架构的简单方法(或平台计划)。

# 数据库认证

如果你使用过下面的 Airflow，你会知道它包括对加密变量的支持，这是一种存储数据库密码等敏感信息的合适方式。这些对 KubernetesPodOperator 来说是不可用的，所以这些需要作为秘密提供。我们创建了一个脚本来运行`airflow connections delete`和`airflow connections add` [命令](https://airflow.apache.org/docs/apache-airflow/stable/cli-and-env-variables-ref.html#connections)来基于通过本地 values.yaml 文件传递给 Helm 的值重新创建这些连接。这样，当新的输入源被定义或密码轮换时，这些将应用于每个头盔升级命令，我们不必让我们的用户输入他们的密码到编辑仪表板。换句话说，它们的连接信息是对由 Redactics 仪表板生成的配置的一种本地补充，仪表板生成一个带有“changemes”的模板配置文件，其中应该提供身份验证信息，它们通过用实际值替换这些“changemes”来创建它们的本地配置文件。

# KubernetesPodOperator，资源管理

因为我们安装在 Kubernetes 上，所以我们可以在一些工作流程步骤中利用 KubernetesPodOperator，特别是那些依赖于 Python 之外的内容的步骤。设置容器资源限制有助于我们坚持我们的目标，即使数据集变大，也要保持极低的资源占用。这在并行运行步骤时特别有用，并且因为我们工作流中的步骤数量是动态的并且依赖于上下文，所以设置`max_active_tasks` DAG 参数以确保您的资源在其分配的足迹内运行是很重要的。否则，您的工作流程可能会非常慢，并且/或者您可能会面临 pod 驱逐。

我们将 Airflow 的舵图设置为跳过安装 web 服务器，因为我们使用`on_failure_callback`回调从文件系统中读取日志，并将它们发送到我们的 API，这样我们的[redac ics 仪表板](https://app.redactics.com/)就是客户与之交互的界面，这样他们就不必在我们的仪表板和 air flow web 服务器之间跳转。我们将 pod exec 命令发送到调度器中的 Airflow CLI，而不是 REST API，以便手动启动工作流。我们的代理软件安装了一些不同的 pod，但是唯一需要的 Airflow pod 是一个单一的计划。它还向我们的 API 发送心跳信号，这样我们就可以在仪表板中提供关于安装状态和运行状况的反馈。

# 动态任务映射

我们急于尝试 Airflow 2.3 的[动态任务映射](https://airflow.apache.org/docs/apache-airflow/stable/concepts/dynamic-task-mapping.html)以及之前介绍的[任务流 API](https://airflow.apache.org/docs/apache-airflow/stable/tutorial/taskflow.html) ，因为没有这些功能，我们只能将预设的工作流配置注入到我们的 Dag 中，这意味着每当我们想要更新我们的工作流配置时，这些 Dag 也必须更新，这意味着需要运行另一个 helm upgrade 命令。现在，我们的 Dag 执行以下操作:

*   从我们的仪表板的 API 中获取工作流配置(我们可以控制 DAG 使用[这个气流变量](https://airflow.apache.org/docs/apache-airflow/stable/configurations-ref.html#worker-refresh-interval)刷新的频率，或者使用 Redis/内存缓存减轻负载
*   几个工作流步骤具有基于这些工作流配置动态生成的`KubernetesPodOperator`命令，这些工作流配置是使用 Python 函数从我们的 API 获取的，如下所示:

```
@task(on_failure_callback=post_logs)
        def gen_table_resets(input_id, schema, **context):
            tables = []
            for input in wf_config["inputs"]:
                if input["id"] == input_id:
                    for table in initial_copies:
                        tables.append(table)
            if len(tables):
                return [["/scripts/table-resets.sh", dag_name, schema, ",".join(tables)]]
            else:
                return []
```

*   每个工作流配置都有自己的 DAG，以便独立地跟踪这项工作。上面的`dag_name`变量，或者一个唯一的配置 ID。这确保了如果您对多个数据库使用编校(正如我们希望的那样)，一个工作流的问题是孤立的，不会影响其他工作流。

# 报告

我们提到了利用`on_failure_callback`回调向我们的 API 报告问题，但我们也利用`on_success_callback`回调来呈现进度条，并通过将它发送到我们的 API 以在我们的仪表板中显示来显示正在运行的工作流的视觉反馈。

我们的 CLI 支持输出关于 pod 健康状况的诊断信息，压缩日志等。但到目前为止，我们的客户还不需要这项功能。我们这样做是为了有可能与我们的客户深入探讨具体问题，但使用 Kubernetes 的一个优势是，当客户已经熟悉它时，他们可以在某种程度上自给自足。例如，我们的代理支持固定到特定的节点，这与您将任何其他 pod 分配到一个节点的方式相同。

# 持久存储，并行任务

我们构建了一个[开源流媒体网络文件系统](https://github.com/Redactics/http-nas)，有点像带有 http 接口的 NAS，这样 pods 就可以轻松地访问和共享文件，包括并行运行的任务的并发使用。我们决定采用这种方法，而不是将文件运送到亚马逊 S3 或类似的地方，部分原因是这更适合我们的“使用您的基础架构”设备方法，而不必让客户导航创建桶以供代理使用(并在这样做时处理可能的数据隐私问题)。当然，性能是评估我们的选项的另一个重要因素，因为它为客户提供了不必打开任何出口网络的选项。我们写了[另一篇关于这个的文章](https://blog.redactics.com/open-source-media-streaming-service-for-kubernetes)，以防你好奇想了解更多，但这绝对是一个挑战，因为`ReadWriteMany` Kubernetes 的持久存储选项不是给定的。

Airflow 支持并行运行任务，这是尽可能提高工作流性能的好方法。

# 摘要

一般来说，气流是工作负载的绝佳选择，但即使是那些需要“超薄气流”极简方案的工作负载也是如此。您唯一的挑战可能是某种共享文件系统。如果你能想出一种方法来简化软件的安装和更新，我想不出任何理由为什么 Redactics 和一个受管理的气流设备不能在一个虚拟机上工作。

我们希望这些信息中有一些是有用的！请务必让我们知道…

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)