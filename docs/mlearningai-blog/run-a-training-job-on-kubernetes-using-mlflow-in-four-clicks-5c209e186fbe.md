# 使用 K3ai 在 Kubernetes 上运行 MLFlow 项目不到 5 分钟

> 原文：<https://medium.com/mlearning-ai/run-a-training-job-on-kubernetes-using-mlflow-in-four-clicks-5c209e186fbe?source=collection_archive---------3----------------------->

*部署整个人工智能基础设施堆栈以学习或运行您的 CI/CD 管道的四个步骤。*

> “傻瓜忽视复杂性。实用主义者深受其害。有些可以避免。天才们会移除它。”艾伦·海瑞泽斯。

![](img/ed9fb9e511f86500369100fee4a674c1.png)

Photo by [Xavi Cabrera](https://unsplash.com/@xavi_cabrera?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/surprise?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

学习人工智能的最大障碍之一是如何使用它的一些工具。

在大多数情况下，您最终会偶然发现一个 Jupiter 笔记本或一个 python 脚本，您最终不得不对您的 Kubernetes 集群执行……*不，等等，反对什么*？

哦，你是说我们忘记了我们这个超级简单的例子有一大堆你从未听说过的需求？

比如:

*   你需要一个 Kubernetes 集群和我们超级简单的清单…
*   下载并配置头盔和运行这个图表…但是不要担心，这将是超级容易的，只要做这 200 步，然后…

是的，你可以使用 minikube，microk8s，kubeadm，或者其他什么……哦，是的，这些都非常简单，对吧，但是我告诉过你需要其他的依赖项吗，比如 *kubectl，helm* 或者在你安装了这些工具之后，简单的了解一下*的所有东西？？另外，你得到了那些的正确版本吗？
你的目标是专注于人工智能，但这种方法意味着甚至在能够部署你感兴趣的人工智能工具之前就学习基础设施的基础知识，显然你会遇到所有可能的错误和失误…*

> 所以，这是个交易:
> 我们能把它变得更简单吗？是的，我们可以

![](img/55ba7884ca485d492d8f959b1eb7df32.png)

Photo by [Jackson Simmer](https://unsplash.com/@simmerdownjpg?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/problem?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 输入使用案例

Alex 是个好奇的人，想学点机器学习。他发现了 MLFlow，并相信这是他想要开始的。通过查看文档，他发现可以在 Conda 或 docker 映像(需要构建)中或者直接在他的笔记本电脑上以 python 库的形式运行它。
那太好了，但 Alex 是一个试图成为 MLOp 的 DevOps，所以他已经知道他的“目标”将是 Kubernetes。
Alex 的理想目标是获得如下所示的流程:

1.  Kubernetes 部署

2.MLFlow 部署(K8s 上？)

3.以 MLFlow 的公共示例为例，针对新部署的 MLFLow 服务器运行它

> **无需担心基础设施、部署、清单等问题..**

第一个问题是，像 minikube 或 microk8s 这样的“本地”解决方案很好，但 Alex 想要更灵活的东西，甚至不一定是本地的。

Alex 知道一个简单、轻量级的工具，允许他进行实验。

**该工具命名为**[**K3ai**](https://k3ai.in)**。**

使用 K3ai，他知道他可以创建一个 Kubernetes 集群，部署选择的 ai 工具，并最终执行用于训练的代码，但更好的是，因为 K3ai 可以在任何上游 k8 上工作，它可以很容易地嵌入到 Alex CI/CD 环境中。

因此，这是每个人都可以遵循的步骤，以完成 Alex 用例。

# 步骤 1 — K3ai 配置

要事第一，对吗？

让我们在我们的 Linux 机器上下载 K3ai:

```
curl -sfL [https://get.k3ai.in](https://get.k3ai.in) | sh -
```

或者干脆看视频:

好了，我们已经安装了 K3ai，现在让我们进入配置。

```
k3ai up
```

或者，让我们再看一遍视频:

# 步骤 2 —部署集群

没有什么比部署完整的 K8s 集群更简单的了

```
k3ai cluster deploy --type k3s --name mycluster
```

是的，你甚至可以在选项中进行选择，请查看:

```
k3ai cluster list --all
```

让我们看一下集群部署:

# 步骤 3-部署 MLFLow

好的，集群已经启动，我们不需要**学习任何东西，如果不是简单地**一个命令**的话，但是让我们在我们新创建的集群上部署 MLFLow。**

```
k3ai plugin deploy --name mlflow --target mycluster
```

至于集群，让我们检查插件列表:

```
k3ai plugin list --all
```

哦，顺便问一下，插件完成了吗？是的，所以你可以登录 **http:// <你的笔记本电脑 IP > :3500**

好了，让我们用一段视频来回顾一下这一步:

# 步骤 4——在上面运行我们的代码

那我们要跑什么？MLFLOW [这里](https://github.com/mlflow/mlflow/tree/master/examples/xgboost)的 xgboost 教程怎么样？

为了让你的生活变得超级简单，K3ai 作为它的快速入门克隆[这里](https://github.com/k3ai/quickstart)

所以让我们简单地输入:

```
k3ai run --source [https://github.com/k3ai/quickstart](https://github.com/k3ai/quickstart) --backend mlflow --target mycluster
```

等一分钟左右，现在担心 K3ai 会告诉你一旦做了一切，并检查你的 MLFLOW 用户界面…你会感谢我，因为亚历克斯做了！

还没被说服让我给你看:

是的，4 个步骤，用你选择的工具训练你的代码，现在有人可能真的会说学习人工智能超级简单！。

> K3ai 是一个由一个人维护的开源项目，所以如果你喜欢你刚刚阅读的内容，你喜欢这个想法，请在这个项目上加一颗星，这对我意义重大！在[https://github.com/k3ai/k3ai](https://github.com/k3ai/k3ai)检查项目