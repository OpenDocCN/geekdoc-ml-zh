# 💸招数:云上几乎免费的 Jupyter 笔记本！

> 原文：<https://medium.com/mlearning-ai/trick-almost-free-jupyter-notebooks-on-the-cloud-1eabcdda7914?source=collection_archive---------3----------------------->

## 谷歌云提供了一个优秀的 Jupyter 笔记本托管服务，名为 Vertex AI Workbench，价格相对较高。在某些情况下，我建议使用一个可以让你获得高达 91%折扣的老技巧:使用标准模板在计算引擎 Spot 实例上托管 Jupyter 笔记本。这种 ***简单*** 的解决方案产生了与谷歌托管笔记本服务几乎相同的体验，让您同样高效，并大幅降低成本。事实上，对于像 EDA 或模型实验这样的短 ML 任务，它可能是 GCP 上的最佳解决方案。然而，请注意这里的成本-SLA 权衡。

![](img/87dd4849660e393ad5a8d922060e4ecf.png)

Photo by [Emile Perron](https://unsplash.com/@emilep?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# vertex work bench——高价位的绝佳体验！

如果您是数据科学的新手，或者过去五年一直在火星上度过，Jupyter 笔记本是最受欢迎的研究、EDAs 和基本训练模型的编码环境。一些公司，如 DataBricks 和网飞，甚至建议在交互工具之外使用它，并将其集成到一些 ML/Data 管道中。💡

鉴于它们的实用性，谷歌云建立了一个名为 Vertex AI Workbench 的 Jupyter 笔记本托管版本。这项出色的服务将 Jupyter 笔记本电脑的启动时间缩短至 60 秒以下，并提供各种附加功能，让您的工作流程更加轻松。然而，该服务引入了管理费用，并且仅支持按需实例(并且不支持现场实例)，这使得它对于 IMO 的非关键任务来说过于昂贵。

# 深度学习虚拟机—经济高效的替代方案！

幸运的是，谷歌云有一个替代 Vertex AI Workbench 的产品，名为[深度学习虚拟机](https://cloud.google.com/deep-learning-vm)。该产品本质上是预装了 ML/DL 映像的计算引擎虚拟机，这些映像包括流行的数据科学 python 库、R、CUDA 和 TensorFlow。最重要的是，当启动虚拟机时，它会自动运行一个 Jupyter 实验室服务器，可以通过浏览器访问该服务器。

由于 DL-VM 不是托管服务，因此它们更具可定制性，从而为执行成本优化创造了更多机会。当谈到云上的成本优化时，提到 Spot 实例是很重要的——事实上，DL-VMs 支持这个特性！但是，当然，在用户体验和成本优化之间有一个权衡。要了解更多关于这种权衡的信息，[你可以阅读另一篇关于在谷歌上使用不同笔记本服务的优缺点的博客文章](https://gadbenram.com/2022/06/27/what-is-the-best-cloud-jupyter-service-on-google/)。

# 现货笔记本？

因为 DL-VM 本质上是具有映像的计算引擎，所以它们可以选择作为 Spot 实例启动，因此可以享受潜在的 Spot 折扣，并且不会产生额外的服务管理费用！

Spot 实例是计算引擎虚拟机，与按需定价相比，提供高达 **91%** 的折扣。现货与按需等价物具有完全相同的规格，只有一个缺点:它们可能会突然关闭。Spot 实例依赖于特定区域中可用的多余硬件；因此，当其他客户愿意支付全价请求访问时，谷歌会给予他们优先权，并在需求过高时将你从现货机中驱逐出去。使用 Spot 实例时，请记住:

1.  ⚠️当机器**突然关闭**时**突然关闭**。你只能在事情发生前得到一个简短的通知。
2.  ♻️机器关闭后**可以重启**；不需要创建新的。
3.  保存在机器**磁盘**上的🗄️数据不会受到关机的影响——它将保留在那里。当机器关闭时，你也要为这些存储付费。
4.  😱关闭时，RAM 将被清除—因此任何未保存的 ***工作都将丢失*** :/
5.  📈如果您在该地区使用的实例类型有**高需求**，那么**会增加频繁关闭的机会**。

那么为什么不把 Vertex AI Workbench 只与现货机配合使用，获得 91%的优惠呢？嗯，谷歌就是不允许。有道理。Spot 实例并没有从 SLA 中受益，而且它违背了 Google 对可靠用户体验的深刻方法。您不会想到托管服务会因为停机而关闭，对吗？💁‍♂️

那么如何在现货机上使用 Jupyter 笔记本呢？让我们快速浏览一下。

# 教程:启动 Spot 深度学习虚拟机

## **第一步:使用安装了 gcloud 工具的 shell(终端)**

你可以使用电脑的终端并安装`gcloud`命令工具，也可以使用谷歌的托管云外壳。

## **第二步:选择您想要运行的图像**

您可以阅读有关选择图像的内容，但以下是基本信息:

*   跑数据科学？使用干净的**普通**图像
*   跑深度学习？使用**张量流** / **PyTorch** 图像

## **步骤 3:运行创建实例命令**

```
export IMAGE_FAMILY="common-cpu"
export ZONE="us-central1-b"
export INSTANCE_NAME="notebook-vm-spot"
export INSTANCE_TYPE="e2-standard-2"
gcloud compute instances create $INSTANCE_NAME \ 
       --zone=$ZONE \
       --image-family=$IMAGE_FAMILY \  
       --image-project=deeplearning-platform-release \ 
       --maintenance-policy=TERMINATE \
       --machine-type=$INSTANCE_TYPE \
       --boot-disk-size=200GB \
       --provisioning-model=SPOT \
       --instance-termination-action=STOP
```

## **第四步:保护电脑和服务器之间的连接**

为了确保对笔记本的所有访问都得到授权，谷歌屏蔽了与这台电脑的不安全连接。因此，您需要创建一个从您的设备到笔记本服务器的 ssh 隧道。听起来很难？不，只有一行代码。

在本地计算机上，运行以下命令:

```
export PROJECT_ID="my-gcp-project-id"
gcloud compute ssh \
 — project $PROJECT_ID \
 — zone $ZONE \
 $INSTANCE_NAME \
 — -L 8080:localhost:8080
```

让你的终端开着，否则连接会丢失😅。

## **第五步:在本地浏览器中打开 Jupyter Lab**

现在只要到你的浏览器打开 [http://localhost:8080](http://localhost:8080/) 就行了。你可以在你的机器上工作。一定要记得偶尔保存你的工作！

# 提示和高级技巧

1.  **获得现货 GPU 的几率低**。由于硅硬件的持续短缺，GPU 是稀缺资源。如果你使用 TensorFlow，考虑使用 TPU(尤其是 Spot )!
2.  **小机器寿命更长**——你的机器利用了多余的硬件。您的实例使用的内核和内存越多，为全价付费客户服务的机会就越大。
3.  经常保存你的工作！不建议将 Spot 机器用于训练神经网络等长时间运行的任务。但是，如果您这样做了，请想办法保存您的工作，以便在机器重新启动时可以恢复它。例如:[每隔几个时期创建一个回调来保存模型](https://www.tensorflow.org/api_docs/python/tf/keras/callbacks/ModelCheckpoint)。
4.  **定义一个** [**关闭脚本**](https://cloud.google.com/compute/docs/instances/create-use-spot#handle-preemption) — GCP 允许您在实例关闭之前运行一个脚本。它可以让你优雅地完成和备份你的工作。
5.  **使用** [**E2 机器类型**](https://cloud.google.com/blog/products/compute/google-compute-engine-gets-new-e2-vm-machine-types) 用于 EDA 和非计算繁重用例。如果您正在使用大型数据集，请使用 e2-highmem 组来获得每个 CPU 更多的 RAM。
6.  *高级提示:* **使用 GCP 的** [**部署管理器**](https://cloud.google.com/deployment-manager/docs/configuration/templates/create-basic-template) 创建一个模板，并从那里启动实例。

你觉得这篇博文有帮助吗？欢迎随意转发分享！有什么意见吗？请在这里或在我们的 [Twitter](https://twitter.com/gadbenram) 上与我们分享。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)