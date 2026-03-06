# 宣布开源(麻省理工学院许可)气流和 Kubernetes 友好的文件流服务

> 原文：<https://medium.com/mlearning-ai/announcing-an-open-source-mit-licensed-airflow-and-kubernetes-friendly-file-streaming-service-f68d88f548fe?source=collection_archive---------5----------------------->

到今天为止，我们已经通过开源 MIT 许可开源了我们的 [http-nas](https://github.com/Redactics/http-nas) 项目。

如果您已经开发了需要以文件形式跟踪状态的作业/管道/工作流(我们的主要用例是在 Kubernetes 上运行的 Airflow)，您可能会遇到这样的挑战:拥有一个可以在多个容器之间共享的文件系统，或者您的 Airflow DAGs 中的并发任务。用 Kubernetes 的行话来说，缺少的要素是一个`ReadWriteMany`持久卷，根据您的云提供商的不同，这个持久卷通常要么缺少，要么很贵。

在[redic ics](https://www.redactics.com/?utm_source=hashnode&utm_medium=web&utm_campaign=http_nas)我们的[气流控制设备](/p/2300a81e63b3)面临这个问题。使用亚马逊 S3 或类似的网站对我们来说也不是一个好的选择。[redic ics 智能代理](https://www.redactics.com/?utm_source=medium&utm_medium=web&utm_campaign=http_nas#services)使用 Airflow 的`KubernetesPodOperator`来执行我们 Dag 中的许多任务，其中许多任务是并行运行的。这意味着创建了许多短命的 pod，并且不得不多次(有时并发地)上传和下载 pod 所需的相同文件不仅是带宽和性能杀手，而且它打破了我们所有工作在本地运行而数据不被发送到云的安全模型(这包括通过我们探索的基于 [FUSE](https://github.com/s3fs-fuse/s3fs-fuse) 的文件系统安装 S3 桶)。我们还研究了基于 NFS 的解决方案，包括 [NFS Kubernetes 存储类](https://artifacthub.io/packages/helm/kvaps/nfs-server-provisioner)解决方案和[谷歌云文件存储](https://cloud.google.com/filestore)。在我们必须想出如何扩大我们的 NFS 卷之前，我们与 NFS 的合作还算幸运，我们也不喜欢缺乏文件传输的可追溯性。最小的 GCP 文件存储卷大小是 1TB，考虑到我们的文件通常要小得多，这在成本上是不允许的。从业务角度来看，我们将 Redactics 作为一种在您自己的基础架构中运行的设备进行推广，因此找出如何在我们的客户中以某种方式与 S3 存储桶分担成本或让我们的客户设置他们自己的存储桶是非常困难的。

此外，我们希望以尽可能少的内存开销获得尽可能快的性能，这意味着尽可能多地传输文件，而不是将完整的文件读入内存。cURL、pgdump/pgrestore 等工具。可以处理与 Linux 管道耦合的流(稍后将详细介绍)，并且我们希望能够运行代码，比如只依赖于操作系统级的 shell 脚本，所以这对我们来说是理想的。

我们将 repo [称为 http-nas](https://github.com/Redactics/http-nas),`NAS`部分是[网络连接存储](https://en.wikipedia.org/wiki/Network-attached_storage)的简称，因为它本质上是一个基于 http 的 NAS。我们确实考虑到了 Kubernetes 来构建它，但是如果你能为这项技术想出另一个用例，它当然也可以在非 Kubernetes 环境中使用。

Http-nas 是一个基于 Node.js 的 API，它简单地将文件传入和传出一个目录。我们选择 Node 是因为它支持[文件流](https://nodejs.org/api/stream.html)并且通常非常轻量级(Golang 可能是另一个不错的选择)。在 Kubernetes 上，您可以为这个服务附加一个持久的卷声明，根据需要放大它，作为一个 Kubernetes 服务，它可以通过 http 和从服务名派生的自动创建的 DNS 条目来访问。完全并发是受支持的，至少在我们决定测试这一点时是如此。我们对 http 作为一个接口感兴趣，因为我们想通过 Linux 使用 cURL 访问这个 API，而且 [cURL 支持发送和接收分块文件流](https://everything.curl.dev/http/post/chunked)。该服务旨在运行在与文件来源相同的局域网上，所以我们觉得在第一次迭代中不需要认证。将来，我们可能会通过添加某种认证/验证层(一个简单的选项就是 http 基本认证)来接受零信任网络的概念。

完整的文档可以在 repo 中找到，但是为了节省您的行程，这里有一个使用该 API 的两个基本文件操作的示例:

*   将文件流(通过 http/REST post URL)到服务:`cat /path/to/cat.jpg | curl -X POST -H "Transfer-Encoding: chunked" -s -f -T - [http://http-nas:3000/file/cat.jpg](http://http-nas:3000/file/cat.jpg)`
*   附加(通过 http/REST put URL)到文件:`printf "test append" | curl -X PUT -H "Transfer-Encoding: chunked" -s -f -T - [http://http-nas:3000/file/mydata.csv](http://http-nas:3000/file/mydata.csv)`

如果您对这个项目有任何反馈，愿意做出贡献等，请在下面发表评论。(你知道整个开源钻！)我们很想知道如何有效地参与 OSS 社区，所以请让我们知道你的存在！

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)