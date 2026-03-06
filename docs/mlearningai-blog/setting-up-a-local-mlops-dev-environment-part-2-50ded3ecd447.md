# 设置本地 MLOps 开发环境—第 2 部分

> 原文：<https://medium.com/mlearning-ai/setting-up-a-local-mlops-dev-environment-part-2-50ded3ecd447?source=collection_archive---------4----------------------->

在这个由两篇文章组成的系列中，我分享了在裸机 Ubuntu 工作站上从头开始设置本地 MLOps 开发环境的经验。这一部分是关于在 Kubernetes 集群上安装 Kubeflow 部署，我们已经在 Ubuntu 工作站上安装了 Kubernetes 集群，如 [*第 1 部分*](/@vishalgarg652/setting-up-a-local-mlops-dev-environment-part-1-a8b468329819) 所述。

![](img/55dfe00a2fbe68d23f7aec21d5adf4c1.png)

Photo by [Fletcher Pride](https://unsplash.com/@fletcher_pride?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 设置默认存储类别

作为先决条件，Kubeflow 需要一个带有默认存储类的 Kubernetes 集群。每当 PVC 要求永久卷时，存储类提供永久卷的动态供应。简而言之，它抽象了下划线存储，并为管理员提供了一种描述他们提供的存储类别的方式。参考*[*存储类*](https://kubernetes.io/docs/concepts/storage/storage-classes/) 了解更多。*

出于我们的目的，将 Ceph 作为存储后端的 Rook 应使用 CSI 进行调配。Rook 为 Kubernetes 提供开源的云原生存储，而 Ceph 是一个分布式、可扩展的开源存储解决方案，用于块、对象和共享文件系统存储。想了解更多关于 Rook 和 Ceph 的知识，参考这个 [*链接*](https://rook.io/docs/rook/v1.9/) 。

## 先决条件

*   一个有效的 Kubernetes 集群，我们已经提供了。
*   可以访问 Kubernetes 集群的机器上的 Kubectl 安装，将该集群配置为主集群。
*   能够在安装了 *kubectl* 的机器上克隆 Rook GitHub 库
*   对于测试部署—单节点集群或单个主节点和单个工作节点集群，集群中的每个节点都需要一个已装载的未格式化卷，并且必须安装 LVM，允许主节点上的工作负载:已启用，CNI: Calico

至少需要以下本地存储选项之一:

*   原始设备(没有分区或格式化的文件系统)
*   原始分区(没有格式化的文件系统)
*   数据块模式下存储类中可用的 PVs

确认是否有可用于配置 Ceph 的分区或设备。

```
lsblk
lsblk -f
```

如果 FSTYPE 字段不为空，则在相应的设备上有一个文件系统。在这个例子中，我们可以将 *sdb* 用于 Ceph，而不能使用 *sda* 或其分区。

## 步骤 1 —安装 LVM

为了避免在原始设备上设置 Ceph 时出现任何问题，请通过在所有服务器/工作站上运行以下命令来安装 LVM。

```
sudo apt-get install -y lvm2
```

## 步骤 2-克隆 Rook Github Repo

将 Rook Github repo 克隆到一个空目录中，如下所示。

```
mkdir rook-single-node
cd rook-single-node
git clone --single-branch --branch release-1.5 [https://github.com/rook/rook.git](https://github.com/rook/rook.git)
```

## 步骤 3 —安装车

首先，安装 CRDs

```
cd rook/cluster/examples/kubernetes/ceph
kubectl create -f crds.yaml -f common.yaml -f operator.yaml
```

其次，创建集群。

```
kubectl create -f cluster-test.yaml
```

验证安装。

```
kubectl -n rook-ceph get pod
```

作为这个过程的一部分，将有许多吊舱被部署，这需要时间，所以只需等待一段时间，直到所有吊舱都处于运行状态。

## 步骤 4 —调配存储类

使用 Rook Github repo(Rook/cluster/examples/kubernetes/ceph/CSI/rbd)中提供的 storageclass-test.yaml 为最小安装配置存储类。

```
kubectl apply -f storageclass-test.yaml
```

# 步骤 5 —将存储类别设为默认类别

最后，将存储类设为“默认”。

```
kubectl patch sc rook-ceph-block -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
```

## 步骤 6 —安装 Kubeflow

请参考以下通过清单方法部署 Kubeflow 的先决条件。

## 先决条件

*   `Kubernetes`(最高为`1.21`)，默认为[存储类](https://kubernetes.io/docs/concepts/storage/storage-classes/)。️ Kubeflow 1.5.0 与 1.22 及更高版本不兼容。
*   `kustomize`(版本`3.2.0` ) ( [下载链接](https://github.com/kubernetes-sigs/kustomize/releases/tag/v3.2.0))。Kubeflow 1.5.0 与 kustomize 4.x 的最新版本不兼容。
*   `kubectl`

## 用一个命令安装 Kubeflow

参考 Kubeflow 清单报告中建议的[安装](https://github.com/kubeflow/manifests#installation)方法。基本上，有两种方法，

1.  `apps`和`common`下所有组件的单命令安装
2.  `apps`和`common`的多命令、单个组件安装

选项 1 旨在方便最终用户的部署。
选项 2 针对定制和挑选单个组件的能力。

因为在我的例子中，我将安装 Kubeflow 的所有组件，所以我选择了选项 1，并按如下方式进行。

下载`kustomize.` (版本`3.2.0`)并从[https://github.com/kubeflow/manifests](https://github.com/kubeflow/manifests)克隆清单回购。

从清单目录中执行以下命令。

```
while ! kustomize build example | kubectl apply -f -; do echo "Retrying to apply resources"; sleep 10; done
```

这将需要一些时间来调配所有需要的 K8s 资源。同时保持对 pod、pv、pvc 的监视，因为这将需要相当长的时间来使它们都处于健康和运行状态。

一旦一切安装成功，您就可以通过登录您的集群来访问 Kubeflow 中央仪表盘[。](https://github.com/kubeflow/manifests#connect-to-your-kubeflow-cluster)

如果 **你想看所有日志&输出的所有动作，参考这个视频。**

## 结论

作为这两篇文章的一部分，我们刚刚看到了如何在裸机 Ubuntu 工作站上建立本地 MLOps 开发环境。稍后，我们将讨论如何建立 MLOps 的其他透视图，如特征库、数据管道等。，并从头到尾查看一些示例。请提供您的反馈/意见，并让我知道您的观点。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)