# YOLO V5 住宅安全解决方案

> 原文：<https://medium.com/mlearning-ai/residential-safety-solutions-with-yolo-v5-b61801361cd9?source=collection_archive---------8----------------------->

随着社会安全面临越来越多的挑战，未知车辆可能是一个潜在的危险，为了解决这个问题，我试图创建一个安全解决方案，它可以检测车辆的牌照，并将它们与摄像头捕捉到的时间戳一起注册到数据库中。

**项目背后的动机**

居民楼中的未知车辆会给居民带来不便，即使可能有客人在大厅通过身份验证，并得到帮助找到停车位，但未知车辆可能是对安全的潜在威胁，为了解决这一问题，我尝试构建一种住宅安全解决方案，跟踪进出社会的车辆的移动

**入门**

1.  寻找正确的数据

我在 [Kaggle](https://www.kaggle.com/datasets/andrewmvd/car-plate-detection) 上发现了一个很棒的数据源，里面有超过 400 张汽车牌照的注释图片。

2.寻找合适的模型

为了高速、准确、有效地检测车牌，我选择使用 ultralytics 的 YOLOv5 算法，因为 YOLO 算法以其速度和效率而闻名，因为它们只需向前通过模型一次即可检测到对象(顾名思义，您只需查看一次)

3.数据预处理

注释的主要挑战是它是以 Pascal-VOC 格式注释的，这与 YOLO 不兼容，所以我必须使用下面的代码将它们转换成 YOLO 兼容的格式

4.创建训练集、验证集和测试集

由于数据现在与 YOLO 兼容，我们需要使用以下代码创建训练、验证和测试集来训练、调整和评估模型。

5.训练模型

由于数据现在已分区，我们需要训练模型。为此，我们使用 ultralytics yolov5 实现。以下用于定型模型的代码灵感来自于 ultralytics yolov5 关于自定义数据文档的训练。

6.选择最佳模型

由于模型现在已经定型，我们使用下面的代码从最近一次运行中选择模型的最佳版本。

7.创建数据库

既然现在我们已经训练了模型并加载了最佳模型，是时候进行一些检测并存储检测结果了，我们需要在我们首选的数据库软件中创建一个数据库，在这种情况下，我更喜欢使用 MySQL 社区版

现在，要从 python 连接到我们的数据库，我们需要将凭证存储在 config.py 文件中，以便我们可以从 python 连接到它

现在，由于我们已经准备好了数据库，我们可以使用它来存储住宅社区的认证成员和检测结果。

8.为居民注册创建一个 web 用户界面

为了认证住宅协会的成员，我们可以创建一个简单的 web UI，协会秘书可以在认证文档后填写详细信息。为了创建一个 web UI，我使用了 Flask，HTML 模板和 CSS 的选择由用户决定，我在本文末尾提供了该项目的 github 库的链接，这样读者可以参考代码。

9.执行牌照检测

现在，为了检测车牌，我们可以从 torch hub 加载模型，提供自定义权重，并可以使用 OpenCV 从镜头中检测车牌。要从车牌图像中读取文本，我们可以使用各种可用的 OCR 库，我个人更喜欢使用 EasyOCR。实现对象检测的代码如下所示

[Github 库](https://github.com/aayush1036/residential-safety-solution)

Kaggle 笔记本来训练模型

[](https://www.kaggle.com/code/aayusmaanjain/license-plate-detection?kernelSessionId=101824514) [## 牌照检测

### 使用 Kaggle 笔记本探索和运行机器学习代码|使用来自汽车牌照检测的数据

www.kaggle.com](https://www.kaggle.com/code/aayusmaanjain/license-plate-detection?kernelSessionId=101824514) 

我目前从事的项目的未来前景是:

1.  提高执行检测和 OCR 的速度，从而减少检测中的延迟
2.  提高 OCR 的准确性

热烈欢迎建议和指正。

如果你喜欢我的作品，请在 github 上启动知识库，并在 kaggle 上投票和评论

关于我:

我是 Aayushmaan Jain，一名有抱负的数据科学家，正在攻读数据科学学士学位

我对机器学习、深度学习、自然语言处理和计算机视觉领域特别感兴趣。我渴望学习。我相信学习一个特定概念的最好方法是应用它来解决各种现实世界的问题。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)