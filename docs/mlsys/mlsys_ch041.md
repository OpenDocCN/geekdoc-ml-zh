# 目标检测

![](img/file582.png)

*DALL·E 提示 - 以 1950 年代动画风格的卡通，展示一个带有传感器的详细板，特别是相机，放在带有图案布料的桌子上。板子后面，一台带有大型背板的计算机展示了 Arduino IDE。IDE 的内容暗示了 LED 引脚分配和用于检测语音命令的机器学习推理。在独立的窗口中，串行监视器显示了“是”和“否”命令的输出。*

## 概述

在关于计算机视觉（CV）和 XIAO ESP32S3 的最后一节中，我们学习了如何使用这款卓越的开发板设置和分类图像。继续我们的 CV 之旅，我们将探索微控制器上的 **目标检测**。

### 目标检测与图像分类的比较

对于图像分类模型来说，主要任务是识别图像上最可能存在的对象类别，例如，在猫和狗之间进行分类，图像中的主要“对象”：

![](img/file583.jpg)

但如果图像中没有主导类别会发生什么呢？

![](img/file584.png)

图像分类模型将上述图像完全错误地识别为“灰桶”，可能是由于色彩色调的原因。

> 之前图像中使用的模型是 MobileNet，它使用大型数据集 *ImageNet* 进行训练，在 Raspberry Pi 上运行。

为了解决这个问题，我们需要另一种类型的模型，其中不仅 **可以找到多个类别**（或标签），而且还可以确定对象在给定图像中的 **位置**。

如我们所想，这样的模型要复杂得多，体积也更大，例如，**MobileNetV2 SSD FPN-Lite 320x320**，使用 COCO 数据集进行训练。这个预训练的目标检测模型旨在在一幅图像中定位多达 10 个对象，并为每个检测到的对象输出一个边界框。下面是这样一个模型在 Raspberry Pi 上运行的结果：

![](img/file585.png)

用于目标检测的模型（如 MobileNet SSD 或 YOLO）通常有数 MB 的大小，这对于与 Raspberry Pi 一起使用是可以的，但用于嵌入式设备则不合适，因为嵌入式设备的 RAM 通常最多只有几 MB，就像 XIAO ESP32S3 的情况一样。

### 目标检测的创新解决方案：FOMO

[Edge Impulse 于 2022 年推出，**FOMO**（更快的目标，更多目标）](https://docs.edgeimpulse.com/docs/edge-impulse-studio/learning-blocks/object-detection/fomo-object-detection-for-constrained-devices)，这是一种在嵌入式设备上执行目标检测的新颖解决方案，例如 Nicla Vision 和 Portenta（Cortex M7），在 Cortex M4F CPU（Arduino Nano33 和 OpenMV M4 系列）以及 Espressif ESP32 设备（ESP-CAM、ESP-EYE 和 XIAO ESP32S3 Sense）上。

在这个动手项目中，我们将探索使用 FOMO 进行目标检测。

> 要了解有关 FOMO 的更多信息，您可以访问 Edge Impulse 的 [官方 FOMO 宣布](https://www.edgeimpulse.com/blog/announcing-fomo-faster-objects-more-objects)，其中 Louis Moreau 和 Mat Kelcey 详细解释了它是如何工作的。

## 对象检测项目目标

所有机器学习项目都需要从详细的目标开始。让我们假设我们在一个工业或农村设施中，必须对 **橙子（水果）** 和特定的 **青蛙（虫子）** 进行分类和计数。

![](img/file586.png)

换句话说，我们应该执行多标签分类，其中每张图像可以有三个类别：

+   背景（无对象）

+   水果

+   虫子

这里有一些未标记的图像样本，我们应该使用它们来检测对象（水果和虫子）：

![](img/file587.jpg)

我们对图像中的哪个对象感兴趣，它的位置（质心），以及我们能在上面找到多少个。与 MobileNet SSD 或 YOLO 一样，FOMO 不检测对象的大小，其中边界框是模型输出之一。

我们将使用 XIAO ESP32S3 开发项目以进行图像捕获和模型推理。机器学习项目将使用 Edge Impulse Studio 开发。但在 Studio 中开始对象检测项目之前，让我们创建一个 *原始数据集*（未标记）包含要检测的对象的图像。

## 数据收集

您可以使用 XIAO、您的手机或其他设备捕获图像。在这里，我们将使用 XIAO 和来自 Arduino IDE ESP32 库的代码。

### 使用 XIAO ESP32S3 收集数据集

打开 Arduino IDE 并选择 XIAO_ESP32S3 板（以及它连接的端口）。在 `文件 > 示例 > ESP32 > Camera` 中选择 `CameraWebServer`。

在 `板管理器` 面板中，确认您已安装最新的“稳定”包。

> ⚠️ **注意**
> 
> Alpha 版本（例如，3.x-alpha）与 XIAO 和 Edge Impulse 不兼容。请使用最后一个稳定版本（例如，2.0.11）。

您还应该注释掉所有相机模型，除了 XIAO 模型引脚：

`#define CAMERA_MODEL_XIAO_ESP32S3 // 有 PSRAM`

在 `工具` 中启用 PSRAM。输入您的 Wi-Fi 凭据并将代码上传到设备：

![](img/file588.jpg)

如果代码执行正确，你应该在串行监视器中看到地址：

![](img/file589.png)

在浏览器中复制地址并等待页面上传。选择相机分辨率（例如，QVGA）并选择 `[开始流]`。根据您的连接等待几秒钟/几分钟。您可以使用 `[保存]` 按钮将图像保存到您的计算机下载区域。

![](img/file590.jpg)

Edge Impulse 建议对象应该大小相似且不重叠，以便获得更好的性能。在工业设施中这是可以的，因为相机应该固定，保持与要检测的对象相同的距离。尽管如此，我们也会尝试使用不同大小和位置来查看结果。

> 我们不需要为我们的图片创建单独的文件夹，因为每个都包含多个标签。

我们建议使用大约 50 张图片来混合物体并改变场景中每个物体出现的数量。尽量捕捉不同的角度、背景和光照条件。

> 存储的图片使用 QVGA 帧大小<semantics><mrow><mn>320</mn><mo>×</mo><mn>240</mn></mrow><annotation encoding="application/x-tex">320\times 240</annotation></semantics>和 RGB565（颜色像素格式）。

在捕获您的数据集后，点击[停止流]并将您的图片移动到一个文件夹中。

## Edge Impulse Studio

### 设置项目

前往[Edge Impulse Studio](https://www.edgeimpulse.com/)，在**登录**处输入您的凭据（或创建一个账户），并开始一个新的项目。

![](img/file591.png)

> 在这里，您可以克隆为这个动手实践开发的工程：[XIAO-ESP32S3-Sense-Object_Detection](https://studio.edgeimpulse.com/public/315759/latest)

在您的项目仪表板上，向下滚动到**项目信息**，并选择**边界框（目标检测）**和**Espressif ESP-EYE**（与我们板最相似）作为您的目标设备：

![](img/file592.png)

### 上传未标记的数据

在工作室中，转到“数据采集”标签，在“上传数据”部分，从您的计算机上传作为文件夹捕获的文件。

![](img/file593.png)

> 您可以让工作室自动在训练和测试之间分割您的数据，或者手动进行。我们将上传所有这些作为训练数据。

![](img/file594.png)

所有未标记的图片（47 张）已上传，但在用作项目数据集之前必须适当标记。工作室有一个用于此目的的工具，您可以在链接“标记队列（47）”中找到。

您可以使用两种方式在 Edge Impulse Studio（免费版）上执行 AI 辅助标记：

+   使用 yolov5

+   跟踪帧间物体

> Edge Impulse 为企业客户推出了[自动标记功能](https://docs.edgeimpulse.com/docs/edge-impulse-studio/data-acquisition/auto-labeler)，简化了目标检测项目中的标记任务。

普通物体可以快速识别和标记，使用 YOLOv5（使用 COCO 数据集训练）现有的预训练目标检测模型库。但由于在我们的情况下，物体不是 COCO 数据集的一部分，我们应该选择跟踪物体的选项。使用此选项，一旦在一个帧中绘制边界框并标记图片，物体将自动从帧到帧跟踪，*部分*标记新的物体（并非所有都正确标记）。

> 如果您已经有一个包含边界框的标记数据集，可以使用[EI 上传器](https://docs.edgeimpulse.com/docs/tools/edge-impulse-cli/cli-uploader#bounding-boxes)导入您的数据。

### 标记数据集

从您未标记数据的第一个图像开始，使用鼠标拖动一个框围绕一个对象以添加标签。然后点击**保存标签**以进入下一个项目。

![](img/file595.png)

继续此过程，直到队列清空。最后，所有图像都应该像以下样本那样标记了对象：

![图片](img/file596.jpg)

接下来，在“数据采集”标签页上查看标记的样本。如果其中一个标签错误，您可以在样本名称后面的*三个点*菜单中编辑它：

![图片](img/file597.png)

您将被引导替换错误的标签并纠正数据集。

![图片](img/file598.jpg)

### 平衡数据集和分割训练/测试

标记所有数据后，发现水果类别比虫子类别的样本多得多。因此，收集了 11 张新的额外虫子图像（总计 58 张）。标记它们后，是时候选择一些图像并将它们移动到测试数据集了。您可以在图像名称后面的三个点菜单中完成此操作。我选择了 6 张图像，占总数据集的 13%。

![图片](img/file599.png)

## 冲击设计

在这个阶段，您应该定义如何：

+   **预处理**包括将单个图像从<semantics><mrow><mn>320</mn><mo>×</mo><mn>240</mn></mrow><annotation encoding="application/x-tex">320\times 240</annotation></semantics>调整大小到<semantics><mrow><mn>96</mn><mo>×</mo><mn>96</mn></mrow><annotation encoding="application/x-tex">96\times 96</annotation></semantics>并压缩它们（平方形式，不裁剪）。之后，图像从 RGB 转换为灰度。

+   **设计一个模型**，在这种情况下，“对象检测”。

![图片](img/file600.png)

### 预处理整个数据集

在本节中，将**颜色深度**选为灰度，适用于与 FOMO 模型一起使用并保存参数。

![图片](img/file601.png)

Studio 会自动移动到下一部分，生成特征，其中所有样本都将进行预处理，生成一个包含单个<semantics><mrow><mn>96</mn><mo>×</mo><mn>96</mn><mo>×</mo><mn>1</mn></mrow><annotation encoding="application/x-tex">96\times 96\times 1</annotation></semantics>图像或 9,216 个特征的数据库。

![图片](img/file602.png)

特征探索器显示，在特征生成后，所有样本都表现出良好的分离。

> 一些样本似乎位于错误的空间中，但点击它们可以确认正确的标记。

## 模型设计、训练和测试

我们将使用 FOMO，这是一个基于 MobileNetV2（alpha 0.35）的对象检测模型，旨在将图像粗略分割为**背景**与**感兴趣对象**（在此处为*框*和*轮*）的网格。

FOMO 是一种创新的机器学习对象检测模型，与传统模型如 Mobilenet SSD 和 YOLOv5 相比，可以节省多达 30 倍的能量和内存。FOMO 可以在小于 200 KB RAM 的微控制器上运行。这之所以可能，是因为其他模型通过围绕对象绘制一个正方形（边界框）来计算对象的大小，而 FOMO 忽略了图像的大小，仅通过其质心坐标提供有关对象在图像中位置的信息。

### FOMO 是如何工作的？

FOMO 将图像转换为灰度并使用 8 的因子将其划分为像素块。对于输入的<semantics><mrow><mn>96</mn><mo>×</mo><mn>96</mn></mrow><annotation encoding="application/x-tex">96\times 96</annotation></semantics>，网格将是<semantics><mrow><mn>12</mn><mo>×</mo><mn>12</mn></mrow><annotation encoding="application/x-tex">12\times 12</annotation></semantics> <semantics><mrow><mo stretchy="true" form="prefix">(</mo><mn>96</mn><mi>/</mi><mn>8</mn><mo>=</mo><mn>12</mn><mo stretchy="true" form="postfix">)</mo></mrow><annotation encoding="application/x-tex">(96/8=12)</annotation></semantics>。接下来，FOMO 将对每个像素块运行一个分类器，以计算其中每个像素块包含一个框或轮子的概率，并随后确定具有最高概率包含对象的区域（如果一个像素块没有对象，它将被分类为*背景*）。从最终区域的重叠部分，FOMO 提供了该区域质心的坐标（与图像尺寸相关）。

![图片](img/file603.png)

对于训练，我们应该选择一个预训练的模型。让我们使用**FOMO（更快的目标，更多目标）MobileNetV2 0.35**。此模型大约使用 250 KB 的 RAM 和 80 KB 的 ROM（闪存），非常适合我们的板。

![图片](img/file604.png)

关于训练超参数，模型将使用以下参数进行训练：

+   迭代次数：60

+   批处理大小：32

+   学习率：0.001。

在训练期间的验证中，我们将保留 20%的数据集（*validation_dataset*）。对于剩余的 80%（*train_dataset*），我们将应用数据增强，这会随机翻转、改变图像的大小和亮度，并裁剪它们，从而在训练数据集上人工增加样本数量。

因此，该模型最终的整体 F1 分数为 85%，与使用测试数据（83%）的结果相似。

> 注意，FOMO 自动为之前定义的两个标签（*框*和*轮子*）添加了第三个标签*背景*。

![图片](img/file605.png)

> 在目标检测任务中，准确率通常不是主要的[评估指标](https://learnopencv.com/mean-average-precision-map-object-detection-model-evaluation-metric/)。目标检测涉及对对象进行分类并在其周围提供边界框，这使得它比简单的分类更复杂。问题是我们没有边界框，只有质心。简而言之，使用准确率作为指标可能是误导性的，并且可能无法完全理解模型的性能。因此，我们将使用 F1 分数。

### 使用“实时分类”测试模型

一旦我们的模型训练完成，我们可以使用实时分类工具对其进行测试。在相应的部分，点击连接开发板图标（一个小型 MCU）并使用您的手机扫描二维码。

![图片](img/file606.png)

连接后，您可以使用智能手机捕获实际图像，以便在 Edge Impulse Studio 上由训练模型进行测试。

![](img/file607.png)

需要注意的是，该模型可能会产生误报和漏报。这可以通过定义合适的置信度阈值（使用三个点菜单进行设置）来最小化。尝试使用 0.8 或更高。

## 模型部署（Arduino IDE）

选择 Arduino 库和量化（int8）模型，在部署选项卡上启用 EON 编译器，然后按 `[构建]`。

![](img/file608.png)

打开您的 Arduino IDE，然后在“草图”下，转到“包含库”并添加.ZIP 库。选择您从 Edge Impulse Studio 下载的文件，然后完成操作！

![](img/file609.png)

在 Arduino IDE 的“示例”选项卡下，您应该在您的项目名称下找到名为 (`esp32 > esp32_camera`) 的草图代码。

![](img/file610.png)

您应该更改 32 到 75 行，这些行定义了摄像头模型和引脚，使用与我们的模型相关的数据。复制并粘贴以下行，替换 32-75 行：

```py
#define PWDN_GPIO_NUM -1
#define RESET_GPIO_NUM -1
#define XCLK_GPIO_NUM 10
#define SIOD_GPIO_NUM 40
#define SIOC_GPIO_NUM 39
#define Y9_GPIO_NUM 48
#define Y8_GPIO_NUM 11
#define Y7_GPIO_NUM 12
#define Y6_GPIO_NUM 14
#define Y5_GPIO_NUM 16
#define Y4_GPIO_NUM 18
#define Y3_GPIO_NUM 17
#define Y2_GPIO_NUM 15
#define VSYNC_GPIO_NUM 38
#define HREF_GPIO_NUM 47
#define PCLK_GPIO_NUM 13
```

这里您可以查看生成的代码：

![](img/file611.png)

将代码上传到您的 XIAO ESP32S3 Sense，然后您应该可以开始检测水果和昆虫。您可以在串行监视器中检查结果。

### 背景

![](img/file612.png)

### 水果

![](img/file613.png)

### 故障

![](img/file614.png)

注意，模型的延迟为 143 毫秒，每秒帧率约为 7 fps（与我们从图像分类项目中获得的类似）。这是因为 FOMO 是巧妙地建立在 CNN 模型之上，而不是像 SSD MobileNet 这样的目标检测模型。例如，当在 Raspberry Pi 4 上运行 MobileNetV2 SSD FPN-Lite <semantics><mrow><mn>320</mn><mo>×</mo><mn>320</mn></mrow><annotation encoding="application/x-tex">320\times 320</annotation></semantics> 模型时，延迟大约高五倍（大约 1.5 fps）。

## 模型部署（SenseCraft-Web-Toolkit）

如同在图像分类章节中讨论的那样，在 Arduino IDE 上验证图像模型的推理非常具有挑战性，因为我们无法看到摄像头聚焦在哪里。再次，让我们使用 **SenseCraft-Web Toolkit**。

按照以下步骤启动 SenseCraft-Web-Toolkit：

1.  打开 [SenseCraft-Web-Toolkit 网站](https://seeed-studio.github.io/SenseCraft-Web-Toolkit/#/setup/process)。

1.  将 XIAO 连接到您的电脑：

+   连接好 XIAO 后，选择如下：

![](img/file615.jpg)

+   选择设备/端口，然后按 `[连接]`：

![](img/file616.jpg)

> 您可以尝试之前由 Seeed Studio 上传的几个计算机视觉模型。尝试它们并享受乐趣！

在我们的情况下，我们将使用页面底部的蓝色按钮：`[上传自定义 AI 模型]`。

但首先，我们必须从 Edge Impulse Studio 下载我们的 **量化 .tflite** 模型。

1.  前往 Edge Impulse Studio 中的您的项目，或克隆此项目：

+   [XIAO-ESP32S3-CAM-Fruits-vs-Veggies-v1-ESP-NN](https://studio.edgeimpulse.com/public/228516/live)

1.  在 `仪表板` 上，下载模型（“块输出”）: `Object Detection model - TensorFlow Lite (int8 quantized)`

![](img/file617.jpg)

1.  在 SenseCraft-Web-Toolkit 上，使用页面底部的蓝色按钮：`[上传自定义 AI 模型]`。一个窗口将弹出。将你从 Edge Impulse Studio 下载到电脑上的模型文件输入，选择一个模型名称，并输入标签（ID: Object）：

![](img/file618.jpg)

> 注意，你应该使用在 EI Studio 训练的标签，并按字母顺序输入它们（在我们的情况下，背景，虫子，水果）。

几秒钟（或几分钟）后，模型将上传到你的设备，相机图像将实时出现在预览区域：

![](img/file619.jpg)

被检测到的对象将被标记（质心）。你可以选择你推理光标的置信度 `Confidence` 和 `IoU`，这是用来评估预测边界框与真实边界框准确性的。

点击顶部按钮（设备日志），你可以打开一个串行监视器来跟踪推理，就像我们在 Arduino IDE 中做的那样。

![](img/file620.png)

在设备日志中，你将获得如下信息：

+   预处理时间（图像捕获和裁剪）：3 毫秒，

+   推理时间（模型延迟）：115 毫秒，

+   后处理时间（显示图像和标记对象）：1 毫秒。

+   输出张量（框），例如，其中一个框：[[30,150, 20, 20,97, 2]]；其中 30,150, 20, 20 是框的坐标（围绕质心）；97 是推理结果，2 是类别（在这种情况下 2：水果）。

> 注意，在上面的例子中，我们得到了 5 个框，因为没有任何水果有 3 个质心。一个解决方案将是后处理，我们可以将接近的质心聚合为一个。

这里是其他截图：

![](img/file621.jpg)

## 摘要

FOMO 是图像处理空间中的一个重大飞跃，正如路易斯·莫罗和马特·凯利在 2022 年发布时所说：

> FOMO 是一个突破性的算法，首次将实时目标检测、跟踪和计数带到微控制器。

在嵌入式设备上探索目标检测（以及更精确地，计数它们）存在多种可能性。

## 资源

+   [Edge Impulse 项目](https://studio.edgeimpulse.com/public/315759/latest)
