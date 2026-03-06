# 使用 AutoML Vision 在 Google Cloud 上进行机器学习

> 原文：<https://medium.com/mlearning-ai/machine-learning-on-google-cloud-using-automl-vision-88e16abf986e?source=collection_archive---------3----------------------->

## 使用自动视觉在 MNIST 图像上训练和评估分类模型

![](img/6e8b5e9ac39117b3c2a1311f72e605b5.png)

Photo by [Christopher Burns](https://unsplash.com/@christopher__burns?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

这是在谷歌云上执行机器学习的 3 部分系列的第二部分。在这个故事中，我们将重点关注在 MNIST 数据集上使用 AutoML Vision 训练分类模型。为了开始学习，我建议您回顾第一个故事，以确保您满足先决条件，并了解更多关于 MNIST 数据集的信息。 [*第一个故事可以在这里找到*](https://hshirodkar.medium.com/machine-learning-on-google-cloud-using-big-query-ml-a23d41c3b94) *。*

> **AutoML Vision** 使您能够训练机器学习模型，根据您自己定义的标签对您的图像进行分类。
> 
> 1.从标记的图像中训练模型并评估它们的性能。
> 
> 2.对带有未标记图像的数据集利用人工标记服务。
> 
> 3.注册通过 AutoML API 提供服务的训练模型。

[有关 AutoML 视觉系统的更多信息，请点击此处。](https://cloud.google.com/vision/automl/docs)

## 这个故事涵盖了什么？

在这个故事中，我们将在 MNIST 影像的标记数据集上训练一个分类模型，然后评估其性能。然后，我们将使用 AutoML API 注册这个经过训练的模型，并从中提供预测。

## **步骤 1:从 GitHub 下载 MNIST 火车和测试图片。**

继续从本地机器上的 [git 库](https://github.com/crazycoder20/MLonGCP/tree/main/AutoML)下载 AutoML_Training 和 AutoML_Test 目录。

## **第二步:创建云存储桶，上传 MNIST 图片。**

**登录 GCP 控制台**并从账单页面确认您有一笔 **$300 试用信用额**。

从项目仪表板中，复制项目 ID。我们将使用这个项目 ID 来设置一个全球唯一的云存储桶，以上传 MNIST 数据集。

![](img/f31df0c0ca0d9a05234ff433bdeec09a.png)

从导航菜单中，**选择云存储，然后单击浏览器。**

![](img/512066fd829620409444fcf0f8757a83.png)

点击**创建存储桶。**

![](img/8075eba72c5a030fccf0fa158514f8c8.png)

使用 <projectid>-automl 作为您的铲斗名称和离您最近的地区。您可以选择存储类别、访问控制和高级设置的默认值。点击**创建。**</projectid>

![](img/09b6259bf9250ac6c4d59e84a2438ead.png)

创建 CS 存储桶后，点击**上传文件夹**将 AutoML_Training 文件夹从本地机器上传到存储桶。

![](img/5dccb03bf814c47693926f193badb5de.png)![](img/47d44cbe08989c171fde7439e47eb898.png)

上传文件夹后，CS bucket 应该如下所示。不需要上传 AutoML_Test 文件夹。在本教程的后面部分，我们将从您的本地机器直接使用它来生成预测。

![](img/7f0ea99dce5fc63ba531b303cbaee813.png)

## 第三步:数据准备。

> 为了使用 AutoML Vision 训练自定义模型，您需要提供您想要分类的图像类型(输入)的标记示例，以及您想要 ML 系统预测的类别或标签(答案)。

AutoML_Training 文件夹中的 data.csv 文件提供了训练图像及其关联标签的云存储位置。我们只需要替换这个文件中每个记录上的 GS://占位符，就可以使用我们上传训练 MNIST 图像的云存储桶的完整路径。

**从控制台右上角激活云壳**。

![](img/1656a24ae3cd90d12d13524d448f264f.png)![](img/7e601b17fa2edb4e68c0297565f1d308.png)

一旦你点击激活云壳，终端应该在屏幕底部的同一页面上打开。您可以选择使用右下角的图标在新窗口中打开终端。

![](img/86ed835c37ae4ca0c784351079da70f5.png)

我们将在云 shell 终端中执行以下命令。**在执行第一个命令之前，只需在 GCP 仪表板上用您的项目 ID 替换<项目 ID >。**

这些命令将完成以下任务:

*   data.csv 文件首先从云存储本地复制到 shell 存储。
*   data.csv 中的每条记录都会更新，用您的项目 ID 替换 GS://占位符。
*   然后，更新后的 data.csv 文件被复制回云存储。

![](img/9041e608232da0737dadfe8921aa017a.png)

## 步骤 4:启用 AutoML Vision API。

**在产品和资源搜索栏中查找 AutoML Vision** 。点击视觉。

![](img/7d1c79c10a52b7fabfef55443419069e.png)

从仪表板上的**点击图像分类下的**开始。

![](img/2689ce21d2a1b90db7db0045b8af9dfd.png)

**启用 AutoML API。**

![](img/56596346943a4812076a33c92fbcef0c.png)

## 步骤 5: **从云存储中将 MNIST 图像上传到 AutoML 数据集。**

**点击新数据集。**

![](img/b7be99e8ab124a83186d0ecbaf58badb.png)

使用 **mnist** 作为数据集名称，选择**单标签分类**作为模型目标。**点击创建数据集。**

![](img/9a9ba516938df7c3257698bb14ac5bde.png)

选择在云存储上选择 CSV 文件的选项，然后浏览选择云存储的 AutoML_Training 文件夹中的 data.csv 文件。

![](img/c5a7d860a4a985764cfc247dffe4ccb6.png)

点击继续。

![](img/b1faefe9016ff97b458303066cce2814.png)

一旦按下继续，AutoML 将开始从云存储中的 AutoML_Training 文件夹导入图像。单击图像查看导入的图像。

![](img/263944d8e9c1f2e7468604f15ed592d9.png)

完成导入需要几分钟时间。导入后，您应该能够看到图像。从左侧菜单中选择一个标签，浏览每个标签的图像。

![](img/489759cf797eeb1cc8fbbaa5c72700df.png)![](img/fef6d49102ea8d55ba14105204e247e5.png)

现在**点击标签统计**。

![](img/fefe97dde604d03261746579cf32cf89.png)

我们为每个标签导入了 60 张图片。AutoML 会自动将实例分成训练集、验证集和测试集。

![](img/b666deb2a87e405047e491f97426d878.png)

## 第六步:训练模型。

点击培训选项卡，然后开始培训。

![](img/d1b2c83fa6188104a464edc332ad5abc.png)

继续使用模型的默认名称，并选择云托管，以便我们稍后可以实时提供预测。点击继续。

![](img/b2ee51d777a8c3e2fbaf7ca2fbb246a6.png)

将节点小时预算设置为 8 个节点小时，并选中该框以在培训后将模型部署到 1 个节点。点击开始训练。

![](img/3360fcad4cee4ca0313bd67751ddfcd6.png)

培训大约需要 1-2 个小时。培训完成后，您应该会收到一封电子邮件通知。然后，您可以重新登录，继续本教程的剩余部分。

![](img/99255037589aa5d94e02fe51522de6b9.png)

模型训练完成后，您可以单击“评估”选项卡来查看指标。在置信度为 0.5 时，我们的分类器的精度和召回率为 91.67%。您可以通过向左或向右拖动滑块来试验置信度阈值的不同值，并查看它如何影响精度和召回指标。

![](img/eac1d8a130a73c56a61ce47bec7805be.png)

基于混淆矩阵，我们可以观察到该模型在除 2、4 和 5 之外的所有数字上都表现得非常好。如果你想知道更多关于混淆矩阵的信息，请参考第一个故事的 [*。*](https://medium.com/p/a23d41c3b94/edit)

![](img/11f61c88c9c0351f200b539405117d45.png)

> **精度:**就是正面预测的精度(TP/(TP+FP))
> 
> **回忆:**是我们分类器的灵敏度或者说检测率(TP/(TP+FN))

## 第七步:是时候做一些预测了。

现在，**点击测试&使用**并上传本地机器上 AutoML_Test 文件夹中的图像。

![](img/07e169e416d7f59d8586cb69e8b8fe43.png)![](img/dc480dc5fd79aee6eefd1a71fde82bae.png)

一旦图片上传，模型将会给你每张图片的预测。我包括 0 和 5 的样本。如您所见，该模型能够以几乎为 1 的极高置信度预测数字 0，而数字 5 的置信度略低，为 0.85。

![](img/a31efd4c615a8581e35fa1f0cd11c631.png)

## 步骤 8:禁用云 AutoML API。

完成评估后，**在产品搜索栏中查找 Cloud AutoML API** 。从搜索结果中单击 Cloud AutoML API。

![](img/bf1d03e2a8025af7d175283ed8ff3cde.png)

**点击管理**。

![](img/45ed5e2d00ed76743e8c3d8aff2a7eec.png)

继续并**禁用 API** 。如果您忘记禁用该服务，只要该服务启动并运行，您将继续为此付费。

![](img/edd8918a648655bf4b8a387c29786199.png)

## 第 9 步:删除云存储桶

转到云存储控制台，选中 <project-id>-automl bucket 的复选框。**点击删除。**</project-id>

确认该存储桶不再存在。

![](img/58f6c6b63e5f84f626bb2348267bcc16.png)

## 结论:

顾名思义，您可以使用 AutoML Vision 为图像构建 ML 模型，而无需代码和 ML 专业知识。然而，它也有自己的缺点，比如定制和微调模型的选项有限。

再次感谢你坚持到最后。

以下是本系列其他两个故事的链接:

*   [谷歌云上的机器学习使用大查询 ML](https://hshirodkar.medium.com/machine-learning-on-google-cloud-using-big-query-ml-a23d41c3b94)
*   [使用人工智能平台在谷歌云上进行机器学习(定制模式)](https://hshirodkar.medium.com/machine-learning-on-google-cloud-using-the-ai-platform-custom-model-ab0cbc874388)

## 提醒一句:

请小心使用谷歌云上的服务，不要超出 300 美元的试用预算。不要忘记关闭 API，删除任何未使用的实例或在使用后清理云存储桶，以避免向您的帐户收取任何额外费用。