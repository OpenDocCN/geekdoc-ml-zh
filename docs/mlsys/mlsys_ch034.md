# 目标检测

![图片](img/file412.jpg)

*DALL·E 3 提示：以 1940 年或 1950 年代的卡通风格展示宽敞的工业仓库内部。一个传送带突出显示，上面载着玩具轮子和箱子混合物。轮子以其明亮的黄色中心和黑色轮胎而可辨。箱子是白色立方体，涂有交替的黑白图案。在移动传送带的末端站着一个复古风格的机器人，装备有工具和传感器，勤奋地对到达的轮子和箱子进行分类和计数。整体美学让人联想到中世纪动画，线条粗犷，色彩经典。*

## 概述

现在，Nicla Vision 上的图像分类的延续正在探索**目标检测**。

![图片](img/file413.png)

### 目标检测与图像分类

图像分类模型的主要任务是生成一个列表，列出图像上最可能存在的物体类别，例如，在猫吃完饭后识别一只虎斑猫：

![图片](img/file414.png)

但是当猫跳到酒杯附近时会发生什么呢？模型仍然只识别图像上的主要类别，即虎斑猫：

![图片](img/file415.png)

如果图像上没有主导类别会发生什么呢？

![图片](img/file416.png)

模型完全错误地将上述图像识别为“废纸篓”，可能是由于色彩色调的原因。

> 所有先前示例中使用的模型是 MobileNet，它使用了一个大型数据集，*ImageNet*。

为了解决这个问题，我们需要另一种类型的模型，这种模型不仅能够找到**多个类别**（或标签），而且还能确定在给定图像中**物体**的位置。

如我们所想，这样的模型要复杂得多，体积也更大，例如，**MobileNetV2 SSD FPN-Lite 320x320**，使用 COCO 数据集进行训练。这个预训练的目标检测模型旨在在图像中定位多达 10 个物体，并为每个检测到的物体输出一个边界框。下面是这样一个模型在 Raspberry Pi 上运行的结果：

![图片](img/file417.png)

用于目标检测的模型（如 MobileNet SSD 或 YOLO）通常大小为几个 MB，这对于 Raspberry Pi 来说是可以的，但与嵌入式设备不兼容，因为嵌入式设备的 RAM 通常低于 1 兆字节。

### 目标检测的创新解决方案：FOMO

[Edge Impulse 于 2022 年推出，**FOMO**（更快的目标，更多目标）](https://docs.edgeimpulse.com/docs/edge-impulse-studio/learning-blocks/object-detection/fomo-object-detection-for-constrained-devices)，这是一种在嵌入式设备上执行目标检测的新颖解决方案，不仅适用于 Nicla Vision（Cortex M7），还适用于 Cortex M4F CPU（Arduino Nano33 和 OpenMV M4 系列以及 Espressif ESP32 设备（ESP-CAM 和 XIAO ESP32S3 Sense））。

在这个动手实验中，我们将探索使用 FOMO 进行对象检测，不会深入介绍模型本身的细节。要了解更多关于模型如何工作的信息，你可以查看 Edge Impulse 的[官方 FOMO 宣布](https://www.edgeimpulse.com/blog/announcing-fomo-faster-objects-more-objects)，其中 Louis Moreau 和 Mat Kelcey 详细解释了其工作原理。

## 对象检测项目目标

所有机器学习项目都需要从一个详细的目标开始。假设我们在一个工业设施中，必须对**轮子**和特殊的**箱子**进行分类和计数。

![图片](img/file418.jpg)

换句话说，我们应该执行多标签分类，其中每张图像可以有三种类别：

+   背景（无对象）

+   箱子

+   轮子

这里有一些未标记的图像样本，我们将使用它们来检测对象（轮子和箱子）：

![图片](img/file419.jpg)

我们感兴趣的是图像中的哪个对象，其位置（质心），以及我们能在上面找到多少个。与 MobileNet SSD 或 YOLO 一样，FOMO 不检测对象的大小，因为边界框是模型输出之一。

我们将使用 Nicla Vision 进行图像捕捉和模型推理来开发项目。机器学习项目将使用 Edge Impulse Studio 进行开发。但在开始 Studio 中的对象检测项目之前，让我们使用包含要检测的对象的图像创建一个*原始数据集*（未标记）。

## 数据收集

对于图像捕捉，我们可以使用：

+   Web Serial Camera 工具，

+   Edge Impulse Studio,

+   OpenMV IDE,

+   一部智能手机。

在这里，我们将使用**OpenMV IDE**。

### 使用 OpenMV IDE 收集数据集

首先，我们在电脑上创建一个文件夹来保存数据，例如，“data。”然后，在 OpenMV IDE 中，我们转到“工具 > 数据集编辑器”并选择“新建数据集”以开始数据集收集：

![图片](img/file420.jpg)

Edge Impulse 建议对象的大小应该相似，且不要重叠，以获得更好的性能。这在工业设施中是可行的，因为相机应该固定，保持与要检测的对象相同的距离。尽管如此，我们也会尝试使用不同大小和位置来查看结果。

> 我们不会为我们的图像创建单独的文件夹，因为每个文件夹都包含多个标签。

将 Nicla Vision 连接到 OpenMV IDE 并运行 `dataset_capture_script.py`。点击“捕捉图像”按钮将开始捕捉图像：

![图片](img/file421.jpg)

我们建议使用大约 50 张图像来混合对象，并改变场景中每个对象出现的数量。尝试捕捉不同的角度、背景和光照条件。

> 存储的图像使用 QVGA 分辨率 <semantics><mrow><mn>320</mn><mo>×</mo><mn>240</mn></mrow><annotation encoding="application/x-tex">320\times 240</annotation></semantics> 和 RGB565（颜色像素格式）。

在捕捉完你的数据集后，关闭“工具 > 数据集编辑器”中的数据集编辑工具。

## Edge Impulse Studio

### 设置项目

前往 [Edge Impulse Studio](https://www.edgeimpulse.com/)，在 **登录**（或创建账户）处输入您的凭据，并开始一个新项目。

> 在这里，您可以克隆为这个动手实践开发的该项目：[NICLA_Vision_Object_Detection](https://studio.edgeimpulse.com/public/292737/latest)。

在项目 `仪表板` 中，转到 **项目信息** 并选择 **边界框（目标检测）**，然后在页面右上角选择 `目标`，**Arduino Nicla Vision (Cortex-M7)**。

![图片](img/file422.png)

### 上传未标记的数据

在 Studio 中，转到 `数据采集` 选项卡，在 `上传数据` 部分从您的计算机文件上传。

![图片](img/file423.png)

> 您可以离开工作室，让数据自动在训练和测试之间分割，或者手动进行。

![图片](img/file424.png)

所有未标记的图像（51 张）都已上传，但在用作项目中的数据集之前，它们还需要适当标注。工作室有一个用于此目的的工具，您可以在链接 `标注队列（51）` 中找到。

您可以使用两种方式在 Edge Impulse Studio（免费版）上执行人工智能辅助标注：

+   使用 yolov5

+   在帧之间跟踪对象

> Edge Impulse 为企业客户推出了 [自动标注功能](https://docs.edgeimpulse.com/docs/edge-impulse-studio/data-acquisition/auto-labeler)，简化了目标检测项目中的标注任务。

使用现有的 YOLOv5 预训练对象检测模型库（使用 COCO 数据集训练）可以快速识别和标注普通对象。但由于在我们的案例中，对象不是 COCO 数据集的一部分，我们应该选择 `跟踪对象` 选项。使用此选项，一旦您在一个帧中绘制边界框并标注图像，对象将自动从帧到帧跟踪，*部分* 标注新的对象（并非所有都正确标注）。 

> 如果您已经有一个包含边界框的已标记数据集，请使用 EI 上传器导入您的数据。

### 标注数据集

从您未标记的数据的第一张图像开始，使用鼠标拖动一个框来围绕对象添加标签。然后点击 **保存标签** 以进入下一项。

![图片](img/file425.png)

继续此过程，直到队列清空。最后，所有图像都应标注为以下样本中的对象：

![图片](img/file426.jpg)

接下来，在 `数据采集` 选项卡上查看已标记样本。如果其中一个标签错误，可以在样本名称后的 *`三个点`* 菜单中进行编辑：

![图片](img/file427.png)

我们将指导您替换错误的标签并纠正数据集。

![图片](img/file428.jpg)

## 冲量设计

在这个阶段，我们应该定义如何：

+   **预处理**包括将单个图像从 `320 x 240` 调整到 `96 x 96` 并将其压扁（方形形式，不裁剪）。之后，图像从 RGB 转换为灰度。

+   **设计模型**，在这种情况下，“目标检测”。

![图片](img/file429.png)

### 预处理所有数据集

在本节中，选择**颜色深度**为`灰度`，适合与 FOMO 模型一起使用，并保存`参数`。

![图片](img/file430.png)

Studio 会自动移动到下一个部分，即“生成特征”，在这里所有样本都将进行预处理，从而生成包含单个<semantics><mrow><mn>96</mn><mo>×</mo><mn>96</mn><mo>×</mo><mn>1</mn></mrow><annotation encoding="application/x-tex">96\times 96\times 1</annotation></semantics>图像或 9,216 个特征的数据库。

![图片](img/file431.png)

特征探索器显示，在特征生成后，所有样本都显示出良好的分离。

> 其中一个样本（46 号）显然位于错误的空间中，但点击它确认标签是正确的。

## 模型设计、训练和测试

我们将使用 FOMO，这是一个基于 MobileNetV2（alpha 0.35）的对象检测模型，旨在将图像粗略分割成**背景**与**感兴趣对象**（在此处为*框*和*轮子*）的网格。

FOMO 是一个创新的机器学习对象检测模型，与传统模型如 Mobilenet SSD 和 YOLOv5 相比，可以节省高达 30 倍的能量和内存。FOMO 可以在小于 200 KB RAM 的微控制器上运行。这之所以可能，是因为其他模型通过围绕对象绘制一个正方形（边界框）来计算对象的大小，而 FOMO 忽略了图像的大小，仅通过其质心坐标提供有关对象在图像中位置的信息。

### FOMO 是如何工作的？

FOMO 将图像转换为灰度，并使用 8 的因子将其划分为像素块。对于 96x96 的输入，网格将是<semantics><mrow><mn>12</mn><mo>×</mo><mn>12</mn></mrow><annotation encoding="application/x-tex">12\times 12</annotation></semantics> <semantics><mrow><mo stretchy="true" form="prefix">(</mo><mn>96</mn><mi>/</mi><mn>8</mn><mo>=</mo><mn>12</mn><mo stretchy="true" form="postfix">)</mo></mrow><annotation encoding="application/x-tex">(96/8=12)</annotation></semantics>。接下来，FOMO 将对每个像素块运行一个分类器，以计算其中每个像素块包含框或轮子的概率，并随后确定具有最高概率包含对象的区域（如果一个像素块没有对象，它将被分类为*背景*）。从最终区域的重叠部分，FOMO 提供了该区域质心的坐标（与图像尺寸相关）。

![图片](img/file432.png)

对于训练，我们应该选择一个预训练的模型。让我们使用**`FOMO (Faster Objects, More Objects) MobileNetV2 0.35`**。这个模型大约使用 250 KB 的 RAM 和 80 KB 的 ROM（闪存），非常适合我们的板子，因为它有 1 MB 的 RAM 和 ROM。

![图片](img/file433.png)

关于训练超参数，模型将使用以下参数进行训练：

+   迭代次数：60，

+   批处理大小：32

+   学习率：0.001。

在训练期间的验证中，20%的数据集（*validation_dataset*）将被保留。对于剩余的 80%（*train_dataset*），我们将应用数据增强，这会随机翻转、改变图像的大小和亮度，并裁剪它们，人为地增加数据集上的样本数量以供训练。

因此，该模型最终在验证数据上达到约 91%的 F1 分数（验证）和 93%（测试数据）。

> 注意，FOMO 自动将第三个标签背景添加到之前定义的两个标签（*box*和*wheel*）中。

![](img/file434.png)

> 在目标检测任务中，准确率通常不是主要的[评估指标](https://learnopencv.com/mean-average-precision-map-object-detection-model-evaluation-metric/)。目标检测涉及对对象进行分类并在其周围提供边界框，这使得它比简单的分类更复杂。问题是我们没有边界框，只有质心。简而言之，使用准确率作为指标可能是误导性的，并且可能无法完全理解模型的性能。因此，我们将使用 F1 分数。

### 使用“实时分类”测试模型

由于 Edge Impulse 官方支持 Nicla Vision，让我们将其连接到 Studio。为此，请按照以下步骤操作：

+   下载[最新的 EI 固件](https://cdn.edgeimpulse.com/firmware/arduino-nicla-vision.zip)并将其解压。

+   在您的计算机上打开 zip 文件，并选择与您的操作系统相关的上传器。

+   将 Nicla-Vision 置于启动模式，按两次复位按钮。

+   执行针对您操作系统的特定批处理代码，将二进制文件（`arduino-nicla-vision.bin`）上传到您的板子上。

前往 EI Studio 中的`实时分类`部分，并使用*webUSB*连接您的 Nicla Vision：

![](img/file435.png)

连接后，您可以使用 Nicla 捕获实际图像，以便在 Edge Impulse Studio 上由训练模型进行测试。

![](img/file436.png)

需要注意的是，模型可能会产生误报和漏报。这可以通过定义适当的`置信度阈值`（使用`三个点`菜单进行设置）来最小化。尝试使用 0.8 或更高。

## 部署模型

在部署选项卡上选择`OpenMV 固件`并按下 `[构建]`。

![](img/file437.png)

当您再次尝试使用 OpenMV IDE 连接 Nicla 时，它将尝试更新其固件。选择`加载特定固件`选项，或者转到`工具 > 运行引导加载程序（加载固件）`。

![](img/file438.png)

您将在计算机上找到来自 Studio 的 ZIP 文件。打开它：

![](img/file439.png)

将.bin 文件加载到您的板子上：

![](img/file440.png)

下载完成后，将显示一个弹出消息。`按确定`，并打开从 Studio 下载的脚本**ei_object_detection.py**。

> 注意：如果出现弹出窗口表示固件过时，请按 `[NO]`，以升级它。

在运行脚本之前，让我们更改几行。请注意，您可以将窗口定义保留为 <semantics><mrow><mn>240</mn><mo>×</mo><mn>240</mn></mrow><annotation encoding="application/x-tex">240\times 240</annotation></semantics>，并将捕获图像的相机设置为 QVGA/RGB。捕获的图像将由从 Edge Impulse 部署的 FW 预处理。

```py
import sensor
import time
import ml
from ml.utils import NMS
import math
import image

sensor.reset()  # Reset and initialize the sensor.
# Set pixel format (RGB565 or GRAYSCALE)
sensor.set_pixformat(sensor.RGB565)
# Set frame size to QVGA (320x240)
sensor.set_framesize(sensor.QVGA)
sensor.skip_frames(time=2000)  # Let the camera adjust.
```

重新定义最小置信度，例如，设置为 0.8 以最小化误报和漏报。

```py
min_confidence = 0.8
```

如有必要，更改用于显示检测到的物体质心的圆圈颜色，以获得更好的对比度。

```py
threshold_list = [(math.ceil(min_confidence * 255), 255)]

# Load built-in model
model = ml.Model("trained")
print(model)

# Alternatively, models can be loaded from the
# filesystem storage.
# model = ml.Model(
#     '<object_detection_modelwork>.tflite',
#     load_to_fb=True)
# labels = [line.rstrip('\n') for line in open("labels.txt")]

colors = [ # Add more colors if you are detecting more
           # than 7 types of classes at once.
    (255, 255,   0), # background: yellow (not used)
    (  0, 255,   0), # cube: green
    (255,   0,   0), # wheel: red
    (  0,   0, 255), # not used
    (255,   0, 255), # not used
    (  0, 255, 255), # not used
    (255, 255, 255), # not used
]
```

保持剩余代码不变

```py
# FOMO outputs an image per class where each pixel in the
# image is the centroid of the trained object. So, we will
# get those output images and then run find_blobs() on them
# to extract the centroids. We will also run get_stats() on
# the detected blobs to determine their score.
# The Non-Max-Suppression (NMS) object then filters out
# overlapping detections and maps their position in the
# output image back to the original input image. The
# function then returns a list per class which each contain
# a list of (rect, score) tuples representing the detected
# objects.


def fomo_post_process(model, inputs, outputs):
    n, oh, ow, oc = model.output_shape[0]
    nms = NMS(ow, oh, inputs[0].roi)
    for i in range(oc):
        img = image.Image(outputs[0][0, :, :, i] * 255)
        blobs = img.find_blobs(
            threshold_list,
            x_stride=1,
            area_threshold=1,
            pixels_threshold=1,
        )
        for b in blobs:
            rect = b.rect()
            x, y, w, h = rect
            score = (
                img.get_statistics(
                    thresholds=threshold_list, roi=rect
                ).l_mean()
                / 255.0
            )
            nms.add_bounding_box(x, y, x + w, y + h, score, i)
    return nms.get_bounding_boxes()


clock = time.clock()
while True:
    clock.tick()

    img = sensor.snapshot()

    for i, detection_list in enumerate(
        model.predict([img], callback=fomo_post_process)
    ):
        if i == 0:
            continue  # background class
        if len(detection_list) == 0:
            continue  # no detections for this class?

        print("********** %s **********" % model.labels[i])
        for (x, y, w, h), score in detection_list:
            center_x = math.floor(x + (w / 2))
            center_y = math.floor(y + (h / 2))
            print(f"x {center_x}\ty {center_y}\tscore {score}")
            img.draw_circle((center_x, center_y, 12), color=colors[i])

    print(clock.fps(), "fps", end="\n")
```

然后按下 `绿色播放按钮` 运行代码：

![图片](img/file441.png)

从摄像头的视角来看，我们可以看到带有用 12 像素固定圆圈标记的质心的物体（每个圆圈都有独特的颜色，取决于其类别）。在串行终端上，模型显示了检测到的标签及其在图像窗口中的位置 <semantics><mrow><mo stretchy="true" form="prefix">(</mo><mn>240</mn><mo>×</mo><mn>240</mn><mo stretchy="true" form="postfix">)</mo></mrow><annotation encoding="application/x-tex">(240\times 240)</annotation></semantics>。

> 注意，坐标原点位于左上角。

![图片](img/file442.jpg)

注意，每秒帧率约为 8 fps（与我们使用图像分类项目得到的相似）。这是因为 FOMO 是巧妙地建立在 CNN 模型之上，而不是像 SSD MobileNet 或 YOLO 这样的物体检测模型。例如，当在 Raspberry Pi 4 上运行 MobileNetV2 SSD FPN-Lite <semantics><mrow><mn>320</mn><mo>×</mo><mn>320</mn></mrow><annotation encoding="application/x-tex">320\times 320</annotation></semantics> 模型时，延迟大约高 5 倍（大约 1.5 fps）

这里有一个展示推理结果的短视频：[`youtu.be/JbpoqRp3BbM`](https://youtu.be/JbpoqRp3BbM)

## 摘要

如路易斯·莫罗和马特·凯尔西在 2022 年发布时所说，FOMO 是图像处理领域的一个重大飞跃：

> FOMO 是一个突破性的算法，首次将实时物体检测、跟踪和计数引入到微控制器中。

在嵌入式设备上探索物体检测（更确切地说，是计数它们）有多种可能性。这在例如计数蜜蜂的项目中非常有用。

![图片](img/file443.jpg)

## 资源

+   [Edge Impulse 项目](https://studio.edgeimpulse.com/public/292737/latest)
