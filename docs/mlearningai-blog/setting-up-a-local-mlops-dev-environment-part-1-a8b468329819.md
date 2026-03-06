# 设置本地 MLOps 开发环境—第 1 部分

> 原文：<https://medium.com/mlearning-ai/setting-up-a-local-mlops-dev-environment-part-1-a8b468329819?source=collection_archive---------1----------------------->

在这个由两篇文章组成的系列中，我分享了在裸机 Ubuntu 工作站上从头开始设置本地 MLOps 开发环境的经验。这一部分是关于在工作站上安装 Kubernetes 集群，在下一部分，我们将看到如何在 Kubernetes 集群上安装 Kubeflow。

![](img/f9092aafef54edf5abf078626371f78f.png)

Photo by [Shane Rounce](https://unsplash.com/@shanerounce?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 在裸机上安装 Kubernetes 集群(Ubuntu 20.04)

# 环境信息

*   Ubuntu Server 20.04.4 LTS
*   三个 HP Z620 塔式工作站，带单个 4 核/ 8vCPU ( 1 个英特尔至强处理器 E5–2643/32GB RAM/1tb 硬盘

三台服务器/工作站应分配如下主机名和 IP:

*   主-192 . 168 . 1 . 207
*   工人 1–192 . 168 . 1 . 199
*   工人 2–192 . 168 . 1 . 208

# 这是怎么回事？

Kubernetes 是一个工具，用于在本地服务器上或跨混合云环境大规模编排和管理容器化的应用程序。对于任何 MLOps 生态系统，Kubernetes 都是托管应用程序的首选方式，因此 ML 应用程序可以扩展并托管在不同的环境中。Kubeadm 是 Kubernetes 提供的一个工具，用于帮助用户安装一个带有隐式验证的生产就绪的 Kubernetes 集群。本教程将演示如何将 Kubernetes 集群安装在一组装有 Kubeadm 的裸机 Ubuntu Server 20.04 工作站上。

要了解更多关于 Kubernetes 的信息，请查看 Kubernetes *官方网站的 [*官方文档。*](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/)*

该实验室包含三台服务器。一个控制或主节点，而另外两个作为工作节点。通过主机名可以很容易地识别它们。

# 步骤 0 准备装有 Ubuntu 操作系统的工作站

用 Ubuntu Server 20.04.4 LTS 操作系统设置工作站。请参考[*链接*](https://ubuntu.com/download/server) 选项 2，即手动安装服务器，在您的工作站上安装操作系统。如果已经安装了操作系统，甚至可以跳过这一步。

用相关的别名和主机名更新三台服务器的/etc/hosts 文件。

```
sudo vi /etc/hosts (make necessary entries for the three workstations)
```

在节点间启用无密码登录。[ **可选**

```
ssh-keygen -t rsa
ssh-copy-id user@<hostname>
```

一旦服务器准备好了，就更新它们。

```
sudo apt update
sudo apt -y full-upgrade
[ -f /var/run/reboot-required ] && sudo reboot -f
```

# 步骤 1 安装 kubelet、kubeadm 和 kubectl

重新启动后，将 Ubuntu 20.04 的 Kubernetes 存储库添加到所有节点。

```
sudo apt -y install curl apt-transport-https
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
```

然后安装所需的软件包。

*注意:对于 MLOps 软件，我将安装与版本 1.22 及更高版本不兼容的 Kubeflow 1.5.0，因此我将安装版本 1.21.10 的 Kubernetes CLI。*

```
sudo apt update
sudo apt -y install vim git curl wget kubelet=1.21.10-00 kubectl=1.21.10-00 kubeadm=1.21.10-00
sudo apt-mark hold kubelet kubeadm kubectl
```

通过检查 kubectl 的版本来确认安装。

```
kubectl version --client && kubeadm version
```

# 禁用交换

关闭交换

```
sudo swapon --show
sudo vi /etc/fstab  (***comment the swap entry***)
sudo swapoff -a
sudo swapon --show
```

如果你想知道为什么在安装 Kubernetes 时需要禁用 swap，请参考本期[](https://github.com/kubernetes/kubernetes/issues/53533)*。*

# *步骤 4 启用内核模块并向 sysctl 添加配置*

*作为 Linux 节点的 *iptables* 正确查看桥接流量的一个要求，您应该确保*net . bridge . bridge-nf-call-iptables*在您的 *sysctl* 配置中设置为 1，例如*

```
****# Enable kernel modules***
sudo modprobe overlay
sudo modprobe br_netfilter ***# Add settings to sysctl***
sudo tee /etc/sysctl.d/kubernetes.conf<<EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
EOF***# Reload sysctl***
sudo sysctl --system*
```

# *步骤 5 安装容器运行时*

*Kubernetes 支持多个容器运行时来运行 pod 中的容器。但是，在这篇文章里，我只讲 Docker。*

```
***# Add repo and Install packages**
sudo apt update
sudo apt install -y curl gnupg2 software-properties-common apt-transport-https ca-certificates
curl -fsSL https:*//download.docker.com/linux/ubuntu/gpg | sudo apt-key add -*
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt update
sudo apt install -y containerd.io docker-ce docker-ce-cli**# Create required directories**
sudo mkdir -p /etc/systemd/system/docker.service.d**# Create daemon json config file**
sudo tee /etc/docker/daemon.json <<EOF
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF**# Start and enable Services**
sudo systemctl daemon-reload 
sudo systemctl restart docker
sudo systemctl enable docker*
```

# *步骤 6 初始化主节点*

*在主节点上，确保加载了 *br_netfilter* 模块。*

```
*lsmod | grep br_netfilter*
```

*启用 kubelet 服务*

```
*sudo systemctl enable kubelet*
```

*接下来，提取容器图像*

```
*sudo kubeadm config images pull*
```

*用 kubeadm 引导集群。请参考[https://kubernetes . io/docs/reference/setup-tools/kube ADM/kube ADM-init/](https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-init/)了解有关 kubeadm init 的有效参数的更多信息。*

```
*sudo kubeadm init --apiserver-advertise-address=192.168.1.207 --upload-certs --pod-network-cidr=192.168.0.0/16 --control-plane-endpoint=control-k8s*
```

*接下来，为普通用户设置集群访问*

```
***mkdir** -p $HOME/.kube
sudo cp -f /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config*
```

*接下来，使用 kubectl 检查集群信息*

```
*kubectl cluster-info*
```

*检查主节点是否处于哪个状态。*

```
*kubectl get nodes -o wide*
```

*可以看出，主设备不处于'*未就绪*状态。*

# *步骤 7 在主节点上安装网络插件*

*接下来，安装一个 Pod 网络插件。在本练习中，我将使用 calico，但请参考 [*支持的网络插件*](https://kubernetes.io/docs/concepts/cluster-administration/addons/) 查看支持的插件。*

```
*kubectl create -f https://docs.projectcalico.org/manifests/tigera-operator.yaml 
kubectl create -f [https://docs.projectcalico.org/manifests/custom-resources.yaml](https://docs.projectcalico.org/manifests/custom-resources.yaml)*
```

*检查所有吊舱是否处于运行状态。*

```
*kubectl get pods -A*
```

*另外，现在检查主节点的状态。*

```
*kubectl get nodes -o wide*
```

# *步骤 8 添加工作节点*

*现在，由于控制平面或主节点都已设置好，可以在“worker”角色中增加更多的节点，这些节点将主要由 Kubernetes 用来调度所有的工作负载。*

*要加入集群，请使用步骤 6 中生成的 join 命令。*

*万岁！！现在，我们已经为自己的本地 Kubernetes 集群设置了 docker 运行时。*

# *结论*

*我们现在已经准备好了一个本地 Kubernetes 集群，并为下一部分做好了一切准备。同时，尽管这看起来是一个简单明了的练习，但是你可能会像我一样面临某些问题。请在您的评论/反馈中与我分享。可能有一些我们可以谈论的共同话题:)。此外，总的来说，请留下您的反馈/评论，让我知道您是否喜欢这篇文章或任何进一步改进的空间。谢谢！*

*第二部分*

*如果 **你想看所有日志&输出的所有动作，参考这个视频。***

*[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)*