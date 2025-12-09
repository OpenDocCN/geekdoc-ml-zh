# 目标检测

![图片](img/file848.jpg)

*DALL·E 提示 - 一张用于树莓派教程中“目标检测”章节的封面图像，设计风格与之前的封面相同，采用 1950 年代电子实验室的风格。场景应突出显示轮子和立方体，类似于用户提供的那些，放置在前景的工作台上。一个连接了摄像头模块的树莓派应该正在捕捉这些对象的图像。周围环绕着经典的实验室工具，如烙铁、电阻和电线。实验室背景应包括示波器和管式收音机等复古设备，保持时代的详细和怀旧感。不应包含任何文本或标志。*

## 概述

在我们探索图像分类的基础上，我们现在将注意力转向一个更高级的计算机视觉任务：目标检测。虽然图像分类将单个标签分配给整个图像，但目标检测通过在单个图像中识别和定位多个对象，更进一步。这种能力为许多新的应用和挑战打开了大门，尤其是在边缘计算和物联网设备（如树莓派）中。

目标检测结合了分类和定位的任务。它不仅确定图像中存在哪些对象，而且通过例如围绕它们绘制边界框等方式，确定它们的位置。这种额外的复杂性使得目标检测成为理解视觉场景的更强大工具，但也需要更复杂的模型和训练技术。

在边缘人工智能领域，我们与受限的计算资源一起工作，实现高效的目标检测模型变得至关重要。我们在图像分类中面临的一些挑战——平衡模型大小、推理速度和准确性——在目标检测中得到了放大。然而，回报也更加显著，因为目标检测能够实现更细致和详细的可视数据分析。

边缘设备上目标检测的一些应用包括：

1.  监控和安全系统

1.  自动驾驶汽车和无人机

1.  工业质量控制

1.  野生动物监测

1.  增强现实应用

当我们着手目标检测时，我们将基于我们在图像分类中探索的概念和技术。我们将检查为效率而设计的流行目标检测架构，例如：

+   单阶段检测器，如 MobileNet 和 EfficientDet，

+   FOMO (更快的目标，更多的目标)，以及

+   YOLO (仅看一次)。

> 要了解更多关于目标检测模型的信息，请遵循教程 [使用深度学习轻松入门目标识别](https://machinelearningmastery.com/object-recognition-with-deep-learning/)。

我们将使用

+   TensorFlow Lite 运行时（现在更名为 [LiteRT](https://ai.google.dev/edge/litert)），

+   Edge Impulse Linux Python SDK 和

+   Ultralytics

![图片](img/file849.png)

在整个实验室中，我们将涵盖目标检测的基础知识以及它与图像分类的区别。我们还将学习如何使用从头创建的数据集训练、微调、测试、优化和部署流行的目标检测架构。

### 目标检测基础知识

目标检测建立在图像分类的基础上，但显著扩展了其功能。要理解目标检测，首先识别它与图像分类的关键区别至关重要：

#### 图像分类与目标检测的比较

**图像分类**：

+   为整个图像分配一个标签

+   回答问题：“这张图片的主要物体或场景是什么？”

+   为整个图像输出一个类别预测

**目标检测**：

+   在图像中识别和定位多个物体

+   回答问题：“这张图片中有什么物体，它们在哪里？”

+   输出多个预测，每个预测包括一个类别标签和一个边界框

为了可视化这种差异，让我们考虑一个例子：

![](img/file850.jpg)

此图说明了关键的区别：图像分类为整个图像提供单个标签，而目标检测识别图像中的多个物体、它们的类别和位置。

#### 目标检测的关键组件

目标检测系统通常由两个主要组件组成：

1.  物体定位：此组件识别物体在图像中的位置。它通常输出边界框，即包含每个检测到的物体的矩形区域。

1.  物体分类：此组件确定每个检测到的物体的类别或类别，类似于图像分类，但应用于每个定位区域。

#### 目标检测的挑战

目标检测除了图像分类的挑战之外，还提出了几个挑战：

+   多个物体：一张图片可能包含多个不同类别、大小和位置的物体。

+   变化的尺度：物体可以在图像中以不同的尺寸出现。

+   遮挡：物体可能部分被隐藏或重叠。

+   背景杂乱：从复杂背景中区分物体可能具有挑战性。

+   实时性能：许多应用需要快速的推理时间，尤其是在边缘设备上。

#### 目标检测的方法

目标检测主要有两种方法：

1.  双阶段检测器：这些首先提出感兴趣区域，然后对每个区域进行分类。例如包括 R-CNN 及其变体（Fast R-CNN、Faster R-CNN）。

1.  单阶段检测器：这些在网络的单一前向传递中预测边界框（或质心）和类别概率。例如包括 YOLO（You Only Look Once）、EfficientDet、SSD（Single Shot Detector）和 FOMO（Faster Objects, More Objects）。这些通常更快，更适合像 Raspberry Pi 这样的边缘设备。

#### 评估指标

与图像分类相比，目标检测使用不同的指标：

+   **交并比 (IoU)**: 衡量预测框和真实框的重叠程度。

+   **平均精度 (mAP)**: 结合所有类别和 IoU 阈值的精度和召回率。

+   **每秒帧数 (FPS)**: 衡量检测速度，对于边缘设备上的实时应用至关重要。

## 预训练目标检测模型概述

正如我们在引言中看到的，给定一张图片或视频流，目标检测模型可以识别可能存在的已知对象集合中的哪些对象，并提供它们在图像中的位置信息。

> 你可以通过访问 [Object Detection - MediaPipe Studio](https://mediapipe-studio.webapps.google.com/studio/demo/object_detector) 在线测试一些常见的模型。

在 [Kaggle](https://www.kaggle.com/models?id=298,130,299)，我们可以找到与 Raspi 一起使用的最常用的预训练 tflite 模型，[ssd_mobilenet_v1,](https://www.kaggle.com/models/tensorflow/ssd-mobilenet-v1/tfLite) 和 [EfficientDet](https://www.kaggle.com/models/tensorflow/efficientdet/tfLite)。这些模型是在 COCO (Common Objects in Context) 数据集上训练的，包含 91 个类别中超过 200,000 张标记的图片。去下载这些模型，并将它们上传到 Raspi 的 `./models` 文件夹中。

> 或者[,](https://github.com/Mjrovai/EdgeML-with-Raspberry-Pi/tree/main/OBJ_DETEC/models) 你可以在 [GitHub](https://github.com/Mjrovai/EdgeML-with-Raspberry-Pi/tree/main/OBJ_DETEC/models) 上找到模型和 COCO 标签。

在本实验室的第一部分，我们将专注于一个预训练的 <semantics><mrow><mn>300</mn><mo>×</mo><mn>300</mn></mrow><annotation encoding="application/x-tex">300\times 300</annotation></semantics> SSD-Mobilenet V1 模型，并将其与 <semantics><mrow><mn>320</mn><mo>×</mo><mn>320</mn></mrow><annotation encoding="application/x-tex">320\times 320</annotation></semantics> EfficientDet-lite0 进行比较，后者也是使用 COCO 2017 数据集训练的。这两个模型都转换为 TensorFlow Lite 格式（SSD Mobilenet 为 4.2 MB，EfficientDet 为 4.6 MB）。

> SSD-Mobilenet V2 或 V3 建议用于迁移学习项目，但一旦 V1 TFLite 模型公开可用，我们将使用它进行此概述。

![](img/file851.png)

### 设置 TFLite 环境

我们应该确认在上一节“动手实验室”中完成的步骤，如下所示：

+   更新 Raspberry Pi

+   安装所需库

+   设置虚拟环境（可选但推荐）

```py
source ~/tflite/bin/activate
```

+   安装 TensorFlow Lite 运行时

+   在环境中安装额外的 Python 库

### 创建工作目录：

考虑到我们在上一个实验室中创建了 `Documents/TFLITE` 文件夹，现在让我们为这个目标检测实验室创建特定的文件夹：

```py
cd Documents/TFLITE/
mkdir OBJ_DETECT
cd OBJ_DETECT
mkdir images
mkdir models
cd models
```

### 推理和后处理

让我们开始一个新的 [notebook](https://github.com/Mjrovai/EdgeML-with-Raspberry-Pi/blob/main/OBJ_DETEC/notebooks/SSD_MobileNetV1.ipynb)，以遵循检测图像上的所有步骤：

导入所需的库：

```py
import time
import numpy as np
import matplotlib.pyplot as plt
from PIL import Image
import tflite_runtime.interpreter as tflite
```

加载 TFLite 模型并分配张量：

```py
model_path = "./models/ssd-mobilenet-v1-tflite-default-v1.tflite"
interpreter = tflite.Interpreter(model_path=model_path)
interpreter.allocate_tensors()
```

获取输入和输出张量。

```py
input_details = interpreter.get_input_details()
output_details = interpreter.get_output_details()
```

**输入细节**将告诉我们模型应该如何用图像进行喂养。形状为`(1, 300, 300, 3)`，数据类型为`uint8`告诉我们应该输入一个非归一化（像素值范围从 0 到 255）的图像，其尺寸为<semantics><mrow><mo stretchy="true" form="prefix">(</mo><mn>300</mn><mo>×</mo><mn>300</mn><mo>×</mo><mn>3</mn><mo stretchy="true" form="postfix">)</mo></mrow><annotation encoding="application/x-tex">(300\times 300\times 3)</annotation></semantics>，逐个输入（批处理维度：1）。

**输出细节**不仅包括标签（“类别”）和概率（“分数”），还包括边界框（“boxes”）相对于图像中对象位置的相对窗口位置以及检测到的对象数量（“num_detections”）。输出细节还告诉我们模型可以在图像中检测到**最多 10 个对象**。

![](img/file852.png)

因此，对于上述示例，使用与*Image Classification Lab*相同的猫图像来寻找输出，我们有一个**76%的概率**在由**边界框[0.028011084, 0.020121813, 0.9886069, 0.802299]**定义的区域中找到了一个**类别 ID 为 16**的对象。这四个数字与`ymin`、`xmin`、`ymax`和`xmax`有关，即框坐标。

考虑到**y**从顶部`(ymin)`到底部(`ymax`)，**x**从左(`xmin`)到右(`xmax`)，实际上我们有的是顶部/左上角的坐标和底部/右下角的坐标。有了两边和图片的形状，我们可以在对象周围画一个矩形，如图所示：

![](img/file853.png)

接下来，我们应该找出类别 ID 等于 16 代表什么。打开文件`coco_labels.txt`，作为一个列表，每个元素都有一个关联的索引，检查索引 16，我们得到预期的`cat`。概率是分数返回的值。

现在让我们上传一些带有多个对象的图像进行测试。

```py
img_path = "./images/cat_dog.jpeg"
orig_img = Image.open(img_path)

# Display the image
plt.figure(figsize=(8, 8))
plt.imshow(orig_img)
plt.title("Original Image")
plt.show()
```

![](img/file854.png)

根据输入细节，让我们先对图像进行预处理，改变其形状并扩展其维度：

```py
img = orig_img.resize(
    (input_details[0]["shape"][1], input_details[0]["shape"][2])
)
input_data = np.expand_dims(img, axis=0)
input_data.shape, input_data.dtype
```

新的`input_data`形状为`(1, 300, 300, 3)`，数据类型为`uint8`，这与模型期望的格式兼容。

使用`input_data`，让我们运行解释器，测量延迟，并获取输出：

```py
start_time = time.time()
interpreter.set_tensor(input_details[0]["index"], input_data)
interpreter.invoke()
end_time = time.time()
inference_time = (
    end_time - start_time
) * 1000  # Convert to milliseconds
print("Inference time: {:.1f}ms".format(inference_time))
```

在大约 800 毫秒的延迟下，我们可以获得 4 个不同的输出：

```py
boxes = interpreter.get_tensor(output_details[0]["index"])[0]
classes = interpreter.get_tensor(output_details[1]["index"])[0]
scores = interpreter.get_tensor(output_details[2]["index"])[0]
num_detections = int(
    interpreter.get_tensor(output_details[3]["index"])[0]
)
```

快速检查后，我们可以看到模型检测到了 2 个得分超过 0.5 的对象：

```py
for i in range(num_detections):
    if scores[i] > 0.5:  # Confidence threshold
        print(f"Object {i}:")
        print(f"  Bounding Box: {boxes[i]}")
        print(f"  Confidence: {scores[i]}")
        print(f"  Class: {classes[i]}")
```

![](img/file855.png)

我们还可以可视化结果：

```py
plt.figure(figsize=(12, 8))
plt.imshow(orig_img)
for i in range(num_detections):
    if scores[i] > 0.5:  # Adjust threshold as needed
        ymin, xmin, ymax, xmax = boxes[i]
        (left, right, top, bottom) = (
            xmin * orig_img.width,
            xmax * orig_img.width,
            ymin * orig_img.height,
            ymax * orig_img.height,
        )
        rect = plt.Rectangle(
            (left, top),
            right - left,
            bottom - top,
            fill=False,
            color="red",
            linewidth=2,
        )
        plt.gca().add_patch(rect)
        class_id = int(classes[i])
        class_name = labels[class_id]
        plt.text(
            left,
            top - 10,
            f"{class_name}: {scores[i]:.2f}",
            color="red",
            fontsize=12,
            backgroundcolor="white",
        )
```

![](img/file856.png)

### EfficientDet

EfficientDet 在技术上不是一个 SSD（单次检测器）模型，但它与 SSD 和其他目标检测架构有一些相似之处，并在此基础上构建了想法：

1.  EfficientDet：

    +   由 Google 研究人员于 2019 年开发

    +   使用 EfficientNet 作为骨干网络

    +   采用新颖的双向特征金字塔网络（BiFPN）

    +   它使用复合缩放有效地缩放主干网络和目标检测组件。

1.  与 SSD 的相似之处：

    +   它们都是单阶段检测器，这意味着它们在单次前向传递中执行对象定位和分类。

    +   它们都使用多尺度特征图来检测不同尺度的对象。

1.  关键区别：

    +   主干网络：SSD 通常使用 VGG 或 MobileNet，而 EfficientDet 使用 EfficientNet。

    +   特征融合：SSD 使用简单的特征金字塔，而 EfficientDet 使用更高级的 BiFPN。

    +   缩放方法：EfficientDet 为网络的所有组件引入了复合缩放

1.  EfficientDet 的优势：

    +   通常比 SSD 和许多其他目标检测模型实现更好的精度-效率权衡。

    +   更灵活的缩放允许有一系列具有不同大小-性能权衡的模型。

虽然 EfficientDet 不是一个 SSD 模型，但它可以看作是单阶段检测架构的演变，它结合了更多高级技术以提高效率和准确性。当使用 EfficientDet 时，我们可以期待得到与 SSD 类似的输出结构（例如，边界框和类别分数）。

> 在 GitHub 上，你可以找到另一个[notebook](https://github.com/Mjrovai/EdgeML-with-Raspberry-Pi/blob/main/OBJ_DETEC/notebooks/SSD_EfficientDet.ipynb)，它探索了我们与 SSD MobileNet 一起使用的 EfficientDet 模型。

## 目标检测项目

现在，我们将从数据收集到训练和部署开发一个完整的图像分类项目。就像我们处理图像分类项目一样，训练和转换后的模型将被用于推理。

我们将使用相同的训练 3 个模型：SSD-MobileNet V2、FOMO 和 YOLO。

### 目标

所有机器学习项目都需要从一个目标开始。假设我们在一个工业设施中，必须对**轮子**和特殊的**盒子**进行分类和计数。

![图片](img/file857.jpg)

换句话说，我们应该执行一个多标签分类，其中每张图片可以属于三个类别：

+   背景（没有对象）

+   盒子

+   轮子

### 原始数据收集

一旦我们定义了我们的机器学习项目目标，下一步也是最重要的一步是收集数据集。我们可以使用手机、树莓派或者它们的组合来创建原始数据集（没有标签）。让我们使用树莓派上的简单 Web 应用在浏览器中查看捕获的`QVGA (320 x 240)`图像。

从 GitHub 获取 Python 脚本[get_img_data.py](https://github.com/Mjrovai/EdgeML-with-Raspberry-Pi/blob/main/IMG_CLASS/python_scripts/get_img_data.py)，并在终端中打开它：

```py
python3 get_img_data.py
```

访问 Web 界面：

+   在树莓派本身（如果你有 GUI）：打开 Web 浏览器并访问`http://localhost:5000`

+   在同一网络上的另一台设备上：打开 Web 浏览器并访问`http://<raspberry_pi_ip>:5000`（将`<raspberry_pi_ip>`替换为你的树莓派的 IP 地址）。例如：`http://192.168.4.210:5000/`

![图片](img/file858.png)

Python 脚本创建了一个基于 Web 的界面，用于使用树莓派及其摄像头捕获和组织图像数据集。这对于需要标注图像数据或不需要的机器学习项目都很有用，就像我们在这里的情况一样。

从浏览器访问 Web 界面，输入您想要捕获的图像的通用标签，然后按下`开始捕获`。

![图片](img/file859.png)

> 注意，要捕获的图像将具有多个标签，这些标签将在以后定义。

使用实时预览定位摄像头，然后点击`捕获图像`以保存当前标签下的图像（在这种情况下，`box-wheel`）。

![图片](img/file860.png)

当我们拥有足够的图像时，我们可以按下`停止捕获`。捕获的图像将保存在文件夹 dataset/box-wheel 中：

![图片](img/file861.png)

> 大约获取 60 张图像。尽量捕捉不同的角度、背景和光照条件。Filezilla 可以将创建的原始数据集传输到您的计算机。

### 标注数据

在对象检测项目中下一步是创建一个标注的数据集。我们应该标注原始数据集图像，在每个图片的对象周围创建边界框（箱和轮）。我们可以使用标注工具如[LabelImg](https://pypi.org/project/labelImg/)、[CVAT](https://www.cvat.ai/)、[Roboflow](https://roboflow.com/annotate)或甚至[Edge Impulse Studio](https://edgeimpulse.com/)。一旦我们在其他实验室探索了 Edge Impulse 工具，让我们在这里使用 Roboflow。

> 我们在这里使用 Roboflow（免费版本）有两个主要原因。1) 我们可以拥有自动标注器，2) 标注的数据集以多种格式提供，并且可以在 Edge Impulse Studio（我们将用它来训练 MobileNet V2 和 FOMO）和 CoLab（YOLOv8 训练）上使用，例如。在 Edge Impulse（免费账户）上拥有标注的数据集时，无法在其他平台上用于训练。

我们应该将原始数据集上传到[Roboflow](https://roboflow.com/)。在那里创建一个免费账户并启动一个新项目，例如（“box-versus-wheel”）。

![图片](img/file862.png)

> 一旦许多教程可用，我们不会深入介绍 Roboflow 的过程。

#### 标注

一旦创建项目并上传数据集，您应该使用“自动标注”工具进行标注。请注意，您还可以上传只有背景的图像，这些图像应保存而不进行任何标注。

![图片](img/file863.png)

一旦所有图像都进行了标注，您应该将它们分成训练、验证和测试集。

![图片](img/file864.png)

#### 数据预处理

数据集的最后一步是预处理，以生成用于训练的最终版本。让我们将所有图像调整大小为<semantics><mrow><mn>320</mn><mo>×</mo><mn>320</mn></mrow><annotation encoding="application/x-tex">320\times 320</annotation></semantics>并生成每个图像的增强版本（增强），以从这些新训练示例中创建我们的模型可以学习的新训练示例。

对于增强，我们将旋转图片（+/-15^o），裁剪，并调整亮度和曝光。

![图片](img/file865.png)

处理结束后，我们将有 153 张图片。

![图片](img/file866.png)

现在，您应该导出 Edge Impulse、Ultralytics 和其他框架/工具可以理解的标注数据集格式，例如`YOLOv8`。让我们下载数据集的压缩版本到我们的桌面。

![图片](img/file867.png)

在这里，您可以查看数据集是如何构建的。

![图片](img/file868.png)

有 3 个独立的文件夹，每个文件夹对应一个分割（`train`/`test`/`valid`）。对于每个文件夹，都有 2 个子文件夹，`images`和`labels`。图片存储为**image_id.jpg**和**images_id.txt**，其中“image_id”对于每张图片都是唯一的。

标签文件格式将是`class_id` `边界框坐标`，在我们的例子中，class_id 将为`0`表示`box`，为`1`表示`wheel`。数字 ID（o, 1, 2…）将遵循类名的字母顺序。

`data.yaml`文件包含有关数据集的信息，如类的名称（`names: ['box', 'wheel']`），遵循 YOLO 格式。

就这样！我们准备好开始使用 Edge Impulse Studio（如我们在下一步中将要做的）进行训练，Ultralytics（如我们在讨论 YOLO 时将要做的），甚至可以在 CoLab 上从头开始训练（就像我们在图像分类实验室中对 Cifar-10 数据集所做的那样）。

> 预处理后的数据集可以在[Roboflow 网站](https://universe.roboflow.com/marcelo-rovai-riila/box-versus-wheel-auto-dataset)找到。

## 在 Edge Impulse Studio 上训练 SSD MobileNet 模型

前往[Edge Impulse Studio](https://www.edgeimpulse.com/)，在**登录**处输入您的凭据（或创建一个账户），并开始一个新项目。

> 在这里，您可以克隆为这个动手实验室开发的工程：[Raspi - 目标检测](https://studio.edgeimpulse.com/public/515477/live)。

在项目“仪表板”标签页，向下滚动到**项目信息**，并选择标注方法为“边界框（目标检测）”。

### 上传标注数据

在 Studio 中，转到“数据采集”标签，然后在“上传数据”部分，从您的电脑上传原始数据集。

我们可以使用`选择一个文件夹`选项，例如选择您电脑中的`train`文件夹，它包含两个子文件夹，`images`和`labels`。选择`图像标签格式`，“YOLO TXT”，将其上传到`训练`类别，并按`上传数据`。

![图片](img/file869.png)

为测试数据重复此过程（上传 test 和 validation 两个文件夹）。上传过程结束后，您应该得到一个包含 153 张图片的标注数据集，分为训练/测试（84%/16%）。

> 注意，标签将存储在标签文件`0`和`1`中，它们等同于`box`和`wheel`。

![图片](img/file870.png)

### Impulse 设计

当我们进入“创建脉冲”步骤时，首先要定义的是描述部署的目标设备。将出现一个弹出窗口。我们将选择 Raspberry 4，它是 Raspi-Zero 和 Raspi-5 之间的中间设备。

> 这个选择不会干扰训练；它只会给我们一个关于模型在该特定目标上的延迟的印象。

![](img/file871.png)

在这个阶段，您应该定义如何：

+   **预处理**包括调整单个图像的大小。在我们的案例中，图像在 Roboflow 上进行了预处理，调整为`320x320`，所以让我们保持它。由于图像已经是方形的，所以调整大小在这里不会产生影响。如果您上传的是矩形图像，请将其压扁（方形形式，无需裁剪）。之后，您可以定义图像是否从 RGB 转换为灰度。

+   **设计一个模型**，在这种情况下，是“目标检测”。

![](img/file872.png)

### 预处理整个数据集

在“图像”部分，选择**颜色深度**为`RGB`，然后按“保存参数”。

![](img/file873.png)

Studio 会自动移动到下一个部分，“生成特征”，其中所有样本都将进行预处理，结果生成 480 个对象：207 个框和 273 个轮子。

![](img/file874.png)

特征探索器显示，在特征生成后，所有样本都表现出良好的分离。

### 模型设计、训练和测试

对于训练，我们应该选择一个预训练模型。让我们使用**MobileNetV2 SSD FPN-Lite (320x320 only**)。这是一个预训练的目标检测模型，旨在在图像中定位多达 10 个对象，并为每个检测到的对象输出一个边界框。该模型大小约为 3.7 MB。它支持<semantics><mrow><mn>320</mn><mo>×</mo><mn>320</mn></mrow><annotation encoding="application/x-tex">320\times 320</annotation></semantics> px 的 RGB 输入。

关于训练超参数，模型将使用以下参数进行训练：

+   阶段：25

+   批处理大小：32

+   学习率：0.15。

在训练期间进行验证时，将保留数据集的 20% (*validation_dataset*)。

![](img/file875.png)

因此，该模型的总体精度得分（基于 COCO mAP）为 88.8%，高于使用测试数据时的结果（83.3%）。

### 部署模型

我们有两种方式来部署我们的模型：

+   **TFLite 模型**，它允许将训练好的模型部署为`.tflite`格式，以便 Raspi 使用 Python 运行。

+   **Linux (AARCH64**)，一个适用于 Linux (AARCH64)的二进制文件，实现了 Edge Impulse Linux 协议，使我们能够在任何基于 Linux 的开发板上运行我们的模型，例如，提供 Python 的 SDK。有关更多信息，请参阅文档和[设置说明](https://docs.edgeimpulse.com/docs/edge-impulse-for-linux)。

让我们部署**TFLite 模型**。在“仪表板”选项卡中，转到迁移学习模型（int8 量化）并点击下载图标：

![](img/file876.png)

将模型从您的计算机传输到 Raspi 文件夹`./models`，并捕获或获取一些用于推理的图像，并将它们保存在文件夹`./images`中。

### 推理和后处理

推理可以按照*预训练目标检测模型概述*中讨论的方式进行。让我们开始一个新的[notebook](https://github.com/Mjrovai/EdgeML-with-Raspberry-Pi/blob/main/OBJ_DETEC/notebooks/EI-SSD-MobileNetV2.ipynb)，以遵循所有步骤在图像上检测立方体和轮子。

导入所需的库：

```py
import time
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.patches as patches
from PIL import Image
import tflite_runtime.interpreter as tflite
```

定义模型路径和标签：

```py
model_path = "./models/ei-raspi-object-detection-SSD-\
 MobileNetv2-320x0320-int8.lite"
labels = ["box", "wheel"]
```

> 记住，模型将输出类别 ID 作为值（0 和 1），按照类别名称的字母顺序。

加载模型，分配张量，并获取输入和输出张量详情：

```py
# Load the TFLite model
interpreter = tflite.Interpreter(model_path=model_path)
interpreter.allocate_tensors()

# Get input and output tensors
input_details = interpreter.get_input_details()
output_details = interpreter.get_output_details()
```

需要注意的一个关键区别是，模型输入细节的`dtype`现在是`int8`，这意味着输入值从-128 到+127，而我们的原始图像的每个像素值从 0 到 256。这意味着我们应该预处理图像以匹配它。我们可以在以下位置进行检查：

```py
input_dtype = input_details[0]["dtype"]
input_dtype
```

```py
numpy.int8
```

因此，让我们打开图像并显示它：

```py
# Load the image
img_path = "./images/box_2_wheel_2.jpg"
orig_img = Image.open(img_path)

# Display the image
plt.figure(figsize=(6, 6))
plt.imshow(orig_img)
plt.title("Original Image")
plt.show()
```

![图片](img/file877.png)

并执行预处理：

```py
scale, zero_point = input_details[0]["quantization"]
img = orig_img.resize(
    (input_details[0]["shape"][1], input_details[0]["shape"][2])
)
img_array = np.array(img, dtype=np.float32) / 255.0
img_array = (
    (img_array / scale + zero_point).clip(-128, 127).astype(np.int8)
)
input_data = np.expand_dims(img_array, axis=0)
```

检查输入数据，我们可以验证输入张量与模型期望的兼容性：

```py
input_data.shape, input_data.dtype
```

```py
((1, 320, 320, 3), dtype('int8'))
```

现在，是时候进行推理了。让我们也计算模型的延迟：

```py
# Inference on Raspi-Zero
start_time = time.time()
interpreter.set_tensor(input_details[0]["index"], input_data)
interpreter.invoke()
end_time = time.time()
inference_time = (
    end_time - start_time
) * 1000  # Convert to milliseconds
print("Inference time: {:.1f}ms".format(inference_time))
```

模型在 Raspi-Zero 上执行推理需要大约 600 毫秒，这大约是 Raspi-5 的五倍。

现在，我们可以获取检测到的对象的输出类别、边界框坐标和概率。

```py
boxes = interpreter.get_tensor(output_details[1]["index"])[0]
classes = interpreter.get_tensor(output_details[3]["index"])[0]
scores = interpreter.get_tensor(output_details[0]["index"])[0]
num_detections = int(
    interpreter.get_tensor(output_details[2]["index"])[0]
)
```

```py
for i in range(num_detections):
    if scores[i] > 0.5:  # Confidence threshold
        print(f"Object {i}:")
        print(f"  Bounding Box: {boxes[i]}")
        print(f"  Confidence: {scores[i]}")
        print(f"  Class: {classes[i]}")
```

![图片](img/file878.png)

从结果中，我们可以看到检测到了 4 个对象：两个具有类别 ID 0（`box`）和两个具有类别 ID 1（`wheel`），这是正确的！

让我们可视化`threshold`为 0.5 的结果

```py
threshold = 0.5
plt.figure(figsize=(6, 6))
plt.imshow(orig_img)
for i in range(num_detections):
    if scores[i] > threshold:
        ymin, xmin, ymax, xmax = boxes[i]
        (left, right, top, bottom) = (
            xmin * orig_img.width,
            xmax * orig_img.width,
            ymin * orig_img.height,
            ymax * orig_img.height,
        )
        rect = plt.Rectangle(
            (left, top),
            right - left,
            bottom - top,
            fill=False,
            color="red",
            linewidth=2,
        )
        plt.gca().add_patch(rect)
        class_id = int(classes[i])
        class_name = labels[class_id]
        plt.text(
            left,
            top - 10,
            f"{class_name}: {scores[i]:.2f}",
            color="red",
            fontsize=12,
            backgroundcolor="white",
        )
```

![图片](img/file879.png)

但如果我们把阈值降低到 0.3，比如呢？

![图片](img/file880.png)

我们开始看到假阳性和**多次检测**，其中模型以不同的置信度和略微不同的边界框多次检测到相同的对象。

通常，有时我们需要将阈值调整为更小的值以捕获所有对象，避免假阴性，这会导致多次检测。

为了提高检测结果，我们应该实现**非极大值抑制（NMS**），这有助于消除重叠的边界框，并仅保留最自信的检测。

为了这个目的，让我们创建一个名为`non_max_suppression()`的通用函数，其作用是通过消除冗余和重叠的边界框来细化目标检测结果。它通过迭代选择置信度分数最高的检测，并根据交并比（IoU）阈值移除其他显著重叠的检测来实现这一点。

```py
def non_max_suppression(boxes, scores, threshold):
    # Convert to corner coordinates
    x1 = boxes[:, 0]
    y1 = boxes[:, 1]
    x2 = boxes[:, 2]
    y2 = boxes[:, 3]

    areas = (x2 - x1 + 1) * (y2 - y1 + 1)
    order = scores.argsort()[::-1]

    keep = []
    while order.size > 0:
        i = order[0]
        keep.append(i)
        xx1 = np.maximum(x1[i], x1[order[1:]])
        yy1 = np.maximum(y1[i], y1[order[1:]])
        xx2 = np.minimum(x2[i], x2[order[1:]])
        yy2 = np.minimum(y2[i], y2[order[1:]])

        w = np.maximum(0.0, xx2 - xx1 + 1)
        h = np.maximum(0.0, yy2 - yy1 + 1)
        inter = w * h
        ovr = inter / (areas[i] + areas[order[1:]] - inter)

        inds = np.where(ovr <= threshold)[0]
        order = order[inds + 1]

    return keep
```

它是如何工作的：

1.  排序：首先，根据置信度分数对所有检测进行排序，从高到低。

1.  选择：它选择得分最高的框并将其添加到最终的检测列表中。

1.  比较：这个选定的框与所有剩余得分较低的框进行比较。

1.  消除：任何与选定框显著重叠（超过 IoU 阈值）的框将被消除。

1.  迭代：这个过程会重复进行，直到处理完所有框，使用下一个得分最高的框。

现在，我们可以定义一个更精确的可视化函数，该函数将考虑 IoU 阈值，仅检测由`non_max_suppression`函数选定的对象：

```py
def visualize_detections(
    image, boxes, classes, scores, labels, threshold, iou_threshold
):
    if isinstance(image, Image.Image):
        image_np = np.array(image)
    else:
        image_np = image
    height, width = image_np.shape[:2]
    # Convert normalized coordinates to pixel coordinates
    boxes_pixel = boxes * np.array([height, width, height, width])
    # Apply NMS
    keep = non_max_suppression(boxes_pixel, scores, iou_threshold)
    # Set the figure size to 12x8 inches
    fig, ax = plt.subplots(1, figsize=(12, 8))
    ax.imshow(image_np)
    for i in keep:
        if scores[i] > threshold:
            ymin, xmin, ymax, xmax = boxes[i]
            rect = patches.Rectangle(
                (xmin * width, ymin * height),
                (xmax - xmin) * width,
                (ymax - ymin) * height,
                linewidth=2,
                edgecolor="r",
                facecolor="none",
            )

            ax.add_patch(rect)
            class_name = labels[int(classes[i])]
            ax.text(
                xmin * width,
                ymin * height - 10,
                f"{class_name}: {scores[i]:.2f}",
                color="red",
                fontsize=12,
                backgroundcolor="white",
            )
    plt.show()
```

现在，我们可以创建一个函数来调用其他函数，对任何图像进行推理：

```py
def detect_objects(img_path, conf=0.5, iou=0.5):
    orig_img = Image.open(img_path)
    scale, zero_point = input_details[0]["quantization"]
    img = orig_img.resize(
        (input_details[0]["shape"][1], input_details[0]["shape"][2])
    )
    img_array = np.array(img, dtype=np.float32) / 255.0
    img_array = (
        (img_array / scale + zero_point)
        .clip(-128, 127)
        .astype(np.int8)
    )
    input_data = np.expand_dims(img_array, axis=0)

    # Inference on Raspi-Zero
    start_time = time.time()
    interpreter.set_tensor(input_details[0]["index"], input_data)
    interpreter.invoke()
    end_time = time.time()
    inference_time = (
        end_time - start_time
    ) * 1000  # Convert to milliseconds

    print("Inference time: {:.1f}ms".format(inference_time))

    # Extract the outputs
    boxes = interpreter.get_tensor(output_details[1]["index"])[0]
    classes = interpreter.get_tensor(output_details[3]["index"])[0]
    scores = interpreter.get_tensor(output_details[0]["index"])[0]
    num_detections = int(
        interpreter.get_tensor(output_details[2]["index"])[0]
    )

    visualize_detections(
        orig_img,
        boxes,
        classes,
        scores,
        labels,
        threshold=conf,
        iou_threshold=iou,
    )
```

现在，运行代码，使用相同的图像，但置信度阈值为 0.3，并且有一个小的 IoU：

```py
img_path = "./images/box_2_wheel_2.jpg"
detect_objects(img_path, conf=0.3, iou=0.05)
```

![](img/file881.png)

## 在 Edge Impulse Studio 中训练 FOMO 模型

使用 SSD MobileNet 模型的推理效果良好，但延迟显著较高。在 Raspi-Zero 上的推理时间从 0.5 到 1.3 秒不等，这意味着大约或低于 1 FPS（每秒 1 帧）。加快处理过程的一个替代方案是使用 FOMO（更快的目标，更多目标）。

这种新颖的机器学习算法使我们能够在使用比 MobileNet SSD 或 YOLO 少至<semantics><mrow><mn>30</mn><mo>×</mo></mrow><annotation encoding="application/x-tex">30\times</annotation></semantics>的处理能力和内存的情况下，实时地计数多个对象并找到图像中它们的位置。这之所以成为可能，主要原因是其他模型通过围绕对象绘制一个正方形（边界框）来计算对象的大小，而 FOMO 忽略了图像的大小，仅通过其质心坐标提供有关对象在图像中位置的信息。

### FOMO 是如何工作的？

在典型的目标检测管道中，第一阶段是从输入图像中提取特征。**FOMO 利用 MobileNetV2 来完成这项任务**。MobileNetV2 处理输入图像以产生一个特征图，以计算有效的方式捕获诸如纹理、形状和对象边缘等基本特征。

![](img/file882.png)

一旦提取了这些特征，FOMO 的更简单架构，专注于中心点检测，将解释特征图以确定图像中对象的位置。输出是一个单元格网格，其中每个单元格表示是否检测到对象中心。该模型为每个单元格输出一个或多个置信度分数，表示对象存在的可能性。

让我们看看它在图像上的工作情况。

FOMO 使用 8 的因子将图像划分为像素块。对于输入的<semantics><mrow><mn>96</mn><mo>×</mo><mn>96</mn></mrow><annotation encoding="application/x-tex">96\times 96</annotation></semantics>，网格将是<semantics><mrow><mn>12</mn><mo>×</mo><mn>12</mn></mrow><annotation encoding="application/x-tex">12\times 12</annotation></semantics> <semantics><mrow><mo stretchy="true" form="prefix">(</mo><mn>96</mn><mi>/</mi><mn>8</mn><mo>=</mo><mn>12</mn><mo stretchy="true" form="postfix">)</mo></mrow><annotation encoding="application/x-tex">(96/8=12)</annotation></semantics>。对于<semantics><mrow><mn>160</mn><mo>×</mo><mn>160</mn></mrow><annotation encoding="application/x-tex">160\times 160</annotation></semantics>，网格将是<semantics><mrow><mn>20</mn><mo>×</mo><mn>20</mn></mrow><annotation encoding="application/x-tex">20\times 20</annotation></semantics>，依此类推。接下来，FOMO 将对每个像素块运行一个分类器来计算其中是否存在一个框或轮子的概率，然后确定具有最高概率包含物体的区域（如果一个像素块没有物体，它将被分类为*背景*）。从最终区域的重叠部分，FOMO 提供了该区域质心的坐标（与图像尺寸相关）。

![图片](img/file883.png)

**速度与精度之间的权衡**：

+   **网格分辨率**：FOMO 使用固定分辨率的网格，这意味着每个单元格可以检测图像该部分是否存在物体。虽然它不提供高定位精度，但它通过快速和计算轻量来做出权衡，这对于边缘设备至关重要。

+   **多目标检测**：由于每个单元格是独立的，FOMO 可以通过识别多个中心同时检测图像中的多个物体。

### 冲击设计，新的训练和测试

返回 Edge Impulse Studio，在“实验”选项卡中创建另一个冲击。现在，输入图像应该是<semantics><mrow><mn>160</mn><mo>×</mo><mn>160</mn></mrow><annotation encoding="application/x-tex">160\times 160</annotation></semantics>（这是 MobilenetV2 预期的输入大小）。

![图片](img/file884.png)

在“图像”选项卡上，生成特征并转到“对象检测”选项卡。

我们应该选择一个预训练的模型进行训练。让我们使用**FOMO (Faster Objects, More Objects) MobileNetV2 0.35**。

![图片](img/file885.png)

关于训练超参数，模型将使用以下参数进行训练：

+   迭代次数：30

+   批处理大小：32

+   学习率：0.001。

在训练期间进行验证时，将保留数据集的 20% (*validation_dataset*)。对于剩余的 80% (*train_dataset*)，我们不会应用数据增强，因为我们的数据集在 Roboflow 的标注阶段已经进行了增强。

因此，该模型最终的整体 F1 分数为 93.3%，具有令人印象深刻的延迟 8 毫秒（Raspi-4），大约比我们使用 SSD MovileNetV2 得到的延迟低 <semantics><mrow><mn>60</mn><mo>×</mo></mrow><annotation encoding="application/x-tex">60\times</annotation></semantics>。

![](img/file886.png)

> 注意 FOMO 自动为之前定义的两个标签 *boxes*（0）和 *wheels*（1）添加了第三个标签背景。

在“模型测试”选项卡中，我们可以看到准确率为 94%。以下是其中一个测试样本的结果：

![](img/file887.png)

> 在目标检测任务中，准确率通常不是主要的[评估指标](https://learnopencv.com/mean-average-precision-map-object-detection-model-evaluation-metric/)。目标检测涉及对物体进行分类并在其周围提供边界框，这使得它比简单的分类更复杂。问题是我们没有边界框，只有质心。简而言之，使用准确率作为指标可能是误导性的，并且可能无法完全理解模型的表现。

### 部署模型

如前节所述，我们可以将训练好的模型部署为 TFLite 或 Linux (AARCH64)。现在让我们将其部署为 **Linux (AARCH64**)，这是一个实现 [Edge Impulse Linux](https://docs.edgeimpulse.com/docs/tools/edge-impulse-for-linux) 协议的二进制文件。

Edge Impulse for Linux 模型以 `.eim` 格式提供。这个 [可执行文件](https://docs.edgeimpulse.com/docs/run-inference/linux-eim-executable) 包含我们在 Edge Impulse Studio 中创建的“完整脉冲”。脉冲由信号处理块（s）和任何我们添加并训练的学习和异常块（s）组成。它是针对我们的处理器或 GPU（例如，ARM 内核上的 NEON 指令）进行优化的，并带有简单的 IPC 层（通过 Unix 套接字）。

在“部署”选项卡中，选择`Linux (AARCH64)`、`int8`模型并按“构建”。

![](img/file888.png)

模型将自动下载到您的计算机上。

在我们的 Raspi 上，让我们创建一个新的工作区域：

```py
cd ~
cd Documents
mkdir EI_Linux
cd EI_Linux
mkdir models
mkdir images
```

将模型重命名以便于识别：

例如，将 `raspi-object-detection-linux-aarch64-FOMO-int8.eim` 转移到新的 Raspi 文件夹 `./models` 并捕获或获取一些图像进行推理，并将它们保存在文件夹 `./images` 中。

### 推理和后处理

推理将使用 [Linux Python SDK](https://docs.edgeimpulse.com/docs/tools/edge-impulse-for-linux/linux-python-sdk) 进行。这个库允许我们使用 Python 在 [Linux](https://docs.edgeimpulse.com/docs/tools/edge-impulse-for-linux) 机器上运行机器学习模型并收集传感器数据。SDK 是开源的，托管在 GitHub 上：[edgeimpulse/linux-sdk-python](https://github.com/edgeimpulse/linux-sdk-python)。

让我们为使用 Linux Python SDK 设置一个虚拟环境

```py
python3 -m venv ~/eilinux
source ~/eilinux/bin/activate
```

安装所有需要的库：

```py
sudo apt-get update
sudo apt-get install libatlas-base-dev\
                     libportaudio0 libportaudio2
sudo apt-get installlibportaudiocpp0 portaudio19-dev

pip3 install edge_impulse_linux -i https://pypi.python.org/simple
pip3 install Pillow matplotlib pyaudio opencv-contrib-python

sudo apt-get install portaudio19-dev
pip3 install pyaudio
pip3 install opencv-contrib-python
```

允许我们的模型可执行。

```py
chmod +x raspi-object-detection-linux-aarch64-FOMO-int8.eim
```

在新环境中安装 Jupiter Notebook

```py
pip3 install jupyter
```

在本地运行笔记本（在 Raspi-4 或 5 的桌面上）

```py
jupyter notebook
```

或者在你的电脑浏览器上：

```py
jupyter notebook --ip=192.168.4.210 --no-browser
```

让我们按照所有步骤开始一个新的 [notebook](https://github.com/Mjrovai/EdgeML-with-Raspberry-Pi/blob/main/OBJ_DETEC/notebooks/EI-Linux-FOMO.ipynb)，使用 FOMO 模型和 Edge Impulse Linux Python SDK 在图像上检测立方体和轮子。

导入所需的库：

```py
import sys, time
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.patches as patches
from PIL import Image
import cv2
from edge_impulse_linux.image import ImageImpulseRunner
```

定义模型路径和标签：

```py
model_file = "raspi-object-detection-linux-aarch64-int8.eim"
model_path = "models/" + model_file  # Trained ML model from
# Edge Impulse
labels = ["box", "wheel"]
```

> 记住，模型将以值（0 和 1）的形式输出类别 ID，按照类别名称的字母顺序排列。

加载并初始化模型：

```py
# Load the model file
runner = ImageImpulseRunner(model_path)

# Initialize model
model_info = runner.init()
```

`model_info` 将包含关于我们模型的临界信息。然而，与 TFLite 解释器不同，EI Linux Python SDK 库现在将为推理准备模型。

因此，让我们打开图像并显示它（现在，为了兼容性，我们将使用 OpenCV，EI 内部使用的 CV 库。OpenCV 以 BGR 格式读取图像，因此我们需要将其转换为 RGB：

```py
# Load the image
img_path = "./images/1_box_1_wheel.jpg"
orig_img = cv2.imread(img_path)
img_rgb = cv2.cvtColor(orig_img, cv2.COLOR_BGR2RGB)

# Display the image
plt.imshow(img_rgb)
plt.title("Original Image")
plt.show()
```

![](img/file889.png)

现在，我们将使用 `runner` 获取特征和预处理后的图像（`cropped`）：

```py
features, cropped = (
    runner.get_features_from_image_auto_studio_settings(img_rgb)
)
```

并执行推理。让我们也计算模型的延迟：

```py
res = runner.classify(features)
```

让我们获取检测到的对象的输出类别、它们的边界框质心和概率。

```py
print(
    "Found %d bounding boxes (%d ms.)"
    % (
        len(res["result"]["bounding_boxes"]),
        res["timing"]["dsp"] + res["timing"]["classification"],
    )
)
for bb in res["result"]["bounding_boxes"]:
    print(
        "\t%s (%.2f): x=%d y=%d w=%d h=%d"
        % (
            bb["label"],
            bb["value"],
            bb["x"],
            bb["y"],
            bb["width"],
            bb["height"],
        )
    )
```

```py
Found 2 bounding boxes (29 ms.)
    1 (0.91): x=112 y=40 w=16 h=16
    0 (0.75): x=48 y=56 w=8 h=8
```

结果显示检测到两个对象：一个类别 ID 为 0（`box`）和一个类别 ID 为 1（`wheel`），这是正确的！

让我们可视化结果（`阈值` 为 0.5，这是在 Edge Impulse Studio 上对模型进行测试时设置的默认值）。

```py
print(
    "\tFound %d bounding boxes (latency: %d ms)"
    % (
        len(res["result"]["bounding_boxes"]),
        res["timing"]["dsp"] + res["timing"]["classification"],
    )
)
plt.figure(figsize=(5, 5))
plt.imshow(cropped)

# Go through each of the returned bounding boxes
bboxes = res["result"]["bounding_boxes"]
for bbox in bboxes:

    # Get the corners of the bounding box
    left = bbox["x"]
    top = bbox["y"]
    width = bbox["width"]
    height = bbox["height"]

    # Draw a circle centered on the detection
    circ = plt.Circle(
        (left + width // 2, top + height // 2),
        5,
        fill=False,
        color="red",
        linewidth=3,
    )
    plt.gca().add_patch(circ)
    class_id = int(bbox["label"])
    class_name = labels[class_id]
    plt.text(
        left,
        top - 10,
        f'{class_name}: {bbox["value"]:.2f}',
        color="red",
        fontsize=12,
        backgroundcolor="white",
    )
plt.show()
```

![](img/file890.png)

## 使用 Ultralytics 探索 YOLO 模型

对于这个实验，我们将探索 YOLOv8\. [Ultralytics](https://ultralytics.com/) [YOLOv8](https://github.com/ultralytics/ultralytics) 是一个著名的实时目标检测和图像分割模型 YOLO 的版本。YOLOv8 建立在深度学习和计算机视觉的尖端进步之上，在速度和准确性方面提供了无与伦比的性能。其简化的设计使其适用于各种应用，并且易于适应不同的硬件平台，从边缘设备到云 API。

### 讨论 YOLO 模型

YOLO（你只看一次）模型是一个高效且广泛使用的目标检测算法，以其实时处理能力而闻名。与传统的目标检测系统不同，这些系统将分类器或定位器重新用于执行检测，YOLO 将检测问题作为一个单一的回归任务来处理。这种创新的方法使得 YOLO 能够在单个评估中同时预测多个边界框及其类别概率，从而显著提高了其速度。

#### 关键特性：

1.  **单网络架构**:

    +   YOLO 使用单个神经网络处理整个图像。这个网络将图像划分为一个网格，并为每个网格单元直接预测边界框和相关类别概率。这种端到端训练提高了速度并简化了模型架构。

1.  **实时处理**:

    +   YOLO 的一个突出特点是其实时进行目标检测的能力。根据版本和硬件，YOLO 可以以每秒高帧数（FPS）处理图像。这使得它非常适合需要快速和准确目标检测的应用，如视频监控、自动驾驶和现场体育分析。

1.  **版本演变**：

    +   几年来，YOLO 经历了显著的改进，从 YOLOv1 到最新的 YOLOv10。每一代迭代都在准确性、速度和效率方面引入了改进。例如，YOLOv8 结合了网络架构的进步、改进的训练方法和更好的各种硬件支持，确保了更稳健的性能。

    +   尽管 YOLOv10 是该家族的最新成员，其论文基于令人鼓舞的性能，但它刚刚发布（2024 年 5 月），并且尚未完全集成到 Ultralytics 库中。相反，精确度-召回率曲线分析表明，YOLOv8 通常优于 YOLOv9，在捕获更多真实正例的同时，更有效地最小化了误报（更多详情见此[文章](https://encord.com/blog/performanceyolov9-vs-yolov8-custom-dataset/))。因此，本实验室基于 YOLOv8n。

    ![图片](img/file891.jpg)

1.  **准确性和效率**：

    +   虽然 YOLO 的早期版本在速度上做出了一些牺牲以换取准确性，但最近版本在平衡两者方面取得了重大进展。新模型速度更快、准确性更高，能够检测小物体（如蜜蜂）并在复杂数据集上表现良好。

1.  **广泛的应用范围**：

    +   YOLO 的通用性使其在众多领域得到应用。它被用于交通监控系统检测和计数车辆，安全应用中识别潜在威胁，以及农业技术中监测作物和牲畜。其应用范围扩展到任何需要高效和准确目标检测的领域。

1.  **社区和开发**：

    +   YOLO 持续发展，并得到了强大的开发者和研究者社区的支持（YOLOv8 尤其强大）。开源实现和广泛的文档使其易于定制和集成到各种项目中。流行的深度学习框架如 Darknet、TensorFlow 和 PyTorch 支持 YOLO，进一步扩大了其适用性。

    +   [Ultralytics YOLOv8](https://github.com/ultralytics/ultralytics?tab=readme-ov-file)不仅可以[检测](https://docs.ultralytics.com/tasks/detect)（我们这里的案例）还可以[分割](https://docs.ultralytics.com/tasks/segment)和[姿态](https://docs.ultralytics.com/tasks/pose)模型，这些模型在[COCO](https://docs.ultralytics.com/datasets/detect/coco)数据集上预训练，以及 YOLOv8 在[ImageNet](https://docs.ultralytics.com/datasets/classify/imagenet)数据集上预训练的[分类](https://docs.ultralytics.com/tasks/classify)模型。[跟踪](https://docs.ultralytics.com/modes/track)模式适用于所有检测、分割和姿态模型。

    ![图片](img/file892.png)

    Ultralytics YOLO 支持的任务

### 安装

在我们的 Raspi 上，让我们停用当前环境以创建一个新的工作区域：

```py
deactivate
cd ~
cd Documents/
mkdir YOLO
cd YOLO
mkdir models
mkdir images
```

让我们为使用 Ultralytics YOLOv8 设置一个虚拟环境

```py
python3 -m venv ~/yolo
source ~/yolo/bin/activate
```

并且在 Raspi 上安装 Ultralytics 的本地推理包

1.  更新包列表，安装 pip，并升级到最新版本：

```py
sudo apt update
sudo apt install python3-pip -y
pip install -U pip
```

1.  使用可选依赖项安装`ultralytics` pip 包：

```py
pip install ultralytics[export]
```

1.  重启设备：

```py
sudo reboot
```

### 测试 YOLO

Raspi-Zero 启动后，让我们激活`yolo`环境，进入工作目录，

```py
source ~/yolo/bin/activate
cd /Documents/YOLO
```

并在终端（CLI）上运行从 Ultralytics 网站下载的图像的推理，使用 YOLOV8n 模型（家族中最小的模型）：

```py
yolo predict model='yolov8n' \
     source='https://ultralytics.com/images/bus.jpg'
```

> YOLO 模型家族使用 COCO 数据集预训练。

推理结果将在终端中显示。在图像（bus.jpg）中，检测到 4 个`persons`，1 个`bus`和 1 个`stop signal`：

![图片](img/file893.png)

此外，我们还收到一条消息，`Results saved to runs/detect/predict`。检查该目录，我们可以看到一个新图像已保存（bus.jpg）。让我们从 Raspi-Zero 下载到我们的桌面进行检查：

![图片](img/file894.png)

因此，Ultralytics YOLO 已正确安装在我们的 Raspi 上。但是，在 Raspi-Zero 上，这个问题是推理的高延迟，大约 18 秒，即使是最小巧的模型（YOLOv8n）也是如此。

### 将模型导出为 NCNN 格式

在计算能力有限的边缘设备（如 Raspi-Zero）上部署计算机视觉模型可能会导致延迟问题。一个替代方案是使用优化以获得最佳性能的格式。这确保了即使处理能力有限的设备也能很好地处理高级计算机视觉任务。

在 Ultralytics 支持的所有模型导出格式中，[NCNN](https://docs.ultralytics.com/integrations/ncnn)是一个针对移动平台进行优化的高性能神经网络推理计算框架。从设计之初，NCNN 就深入考虑了在手机上的部署和使用，并且没有第三方依赖。它是跨平台的，运行速度比所有已知的开源框架（如 TFLite）都要快。

NCNN 在处理 Raspberry Pi 设备时提供最佳推理性能。NCNN 针对移动嵌入式平台（如 ARM 架构）进行了高度优化。

因此，让我们转换我们的模型并重新运行推理：

1.  将 YOLOv8n PyTorch 模型导出为 NCNN 格式，创建：‘/yolov8n_ncnn_model’

```py
yolo export model=yolov8n.pt format=ncnn
```

1.  使用导出的模型进行推理（现在源可以是上一次推理下载到当前目录的 bus.jpg 图像）：

```py
yolo predict model='./yolov8n_ncnn_model' source='bus.jpg'
```

> 当模型加载时，第一次推理通常具有很高的延迟（大约 17 秒），但从第二次开始，可以注意到推理时间下降到大约 2 秒。

### 使用 Python 探索 YOLO

首先，让我们调用 Python 解释器，这样我们可以逐行探索 YOLO 模型的工作原理：

```py
python3
```

现在，我们应该从 Ultralytics 调用 YOLO 库并加载模型：

```py
from ultralytics import YOLO

model = YOLO("yolov8n_ncnn_model")
```

接下来，对图像进行推理（让我们再次使用`bus.jpg`）：

```py
img = "bus.jpg"
result = model.predict(img, save=True, imgsz=640, conf=0.5, iou=0.3)
```

![图片](img/file895.png)

我们可以验证，结果几乎与我们在终端级别（CLI）运行推理得到的结果相同，只是没有检测到减少的 NCNN 模型中的公交车站。注意，延迟已经减少。

让我们分析“result”内容。

例如，我们可以看到`result[0].boxes.data`，显示主要推理结果，它是一个形状为（4，6）的张量。每一行是检测到的对象之一，前 4 列是边界框坐标，第 5 列是置信度，第 6 列是类别（在这种情况下，`0: person`和`5: bus`）：

![图片](img/file896.png)

我们可以分别访问几个推理结果，以及推理时间，并以更好的格式打印出来：

```py
inference_time = int(result[0].speed["inference"])
print(f"Inference Time: {inference_time} ms")
```

或者，我们可以得到检测到的对象总数：

```py
print(f"Number of objects: {len (result[0].boxes.cls)}")
```

![图片](img/file897.png)

使用 Python，我们可以创建一个详细输出，满足我们的需求（更多详情请参阅[使用 Ultralytics YOLO 进行模型预测](https://docs.ultralytics.com/modes/predict/)）。让我们运行一个 Python 脚本，而不是像下面那样逐行手动输入到解释器中。让我们使用`nano`作为我们的文本编辑器。首先，我们应该创建一个名为，例如，`yolov8_tests.py`的空 Python 脚本：

```py
nano yolov8_tests.py
```

输入以下代码行：

```py
from ultralytics import YOLO

# Load the YOLOv8 model
model = YOLO("yolov8n_ncnn_model")

# Run inference
img = "bus.jpg"
result = model.predict(img, save=False, imgsz=640, conf=0.5, iou=0.3)

# print the results
inference_time = int(result[0].speed["inference"])
print(f"Inference Time: {inference_time} ms")
print(f"Number of objects: {len (result[0].boxes.cls)}")
```

![图片](img/file898.png)

并使用以下命令进行输入：`[CTRL+O]` + `[ENTER]` + `[CTRL+X]`来保存 Python 脚本。

运行脚本：

```py
python yolov8_tests.py
```

结果与在终端级别（CLI）和内置 Python 解释器中运行推理的结果相同。

> 第一次调用 YOLO 库并加载模型进行推理需要很长时间，但之后的推理将会快得多。例如，第一次单次推理可能需要几秒钟，但之后，推理时间应该减少到不到 1 秒。

### 在自定义数据集上训练 YOLOv8

返回到我们的“箱体与轮子”数据集，该数据集在[Roboflow](https://universe.roboflow.com/marcelo-rovai-riila/box-versus-wheel-auto-dataset)上标记。在“下载数据集”中，而不是为 Edge Impulse Studio 训练而执行的“下载到计算机的 zip 文件”选项，我们将选择“显示下载代码”。此选项将打开一个弹出窗口，其中包含一个代码片段，应将其粘贴到我们的训练笔记本中。

![](img/file899.png)

对于训练，让我们调整 Ultralytics 提供的公共示例之一，并在 Google Colab 上运行它。下面，你可以找到我的示例，你可以将其应用于你的项目：

+   YOLOv8 Box 与 Wheel 数据集训练 [[在 Colab 中打开]](https://colab.research.google.com/github/Mjrovai/EdgeML-with-Raspberry-Pi/blob/main/OBJ_DETEC/notebooks/yolov8_box_vs_wheel.ipynb)

#### 笔记本中的关键点：

1.  使用 GPU 运行它（NVidia T4 是免费的）

1.  使用 PIP 安装 Ultralytics。

    ![](img/file900.png)

1.  现在，你可以导入 YOLO 并将你的数据集上传到 CoLab，粘贴我们从 Roboflow 获得的下载代码。请注意，我们的数据集将挂载在 `/content/datasets/`：

![](img/file901.png)

1.  验证并更改文件 `data.yaml` 中的图像正确路径（在每个 `images` 文件夹中复制路径）是至关重要的。

```py
names:

- box
- wheel
nc: 2
roboflow:
  license: CC BY 4.0
  project: box-versus-wheel-auto-dataset
  url: https://universe.roboflow.com/marcelo-rovai-riila/ \
     box-versus-wheel-auto-dataset/dataset/5
  version: 5
  workspace: marcelo-rovai-riila
test: /content/datasets/Box-versus-Wheel-auto-dataset-5/ \
      test/images
train: /content/datasets/Box-versus-Wheel-auto-dataset-5/ \
       train/images
val: /content/datasets/Box-versus-Wheel-auto-dataset-5/ \
     valid/images
```

1.  定义你想要从默认值更改的主要超参数，例如：

    ```py
    MODEL = 'yolov8n.pt'
    IMG_SIZE = 640
    EPOCHS = 25 # For a final project, you should consider
                # at least 100 epochs
    ```

1.  使用 CLI 运行训练：

    ```py
    !yolo task=detect mode=train model={MODEL} \
      data={dataset.location}/data.yaml \
      epochs={EPOCHS}
      imgsz={IMG_SIZE} plots=True
    ```

![](img/file902.png)

image-20240910111319804

模型训练了几分钟，并取得了优异的结果（mAP50 为 0.995）。训练结束时，所有结果都保存在列出的文件夹中，例如：`/runs/detect/train/`。在那里，你可以找到，例如，混淆矩阵。

![](img/file903.png)

1.  注意，训练好的模型 (`best.pt`) 保存在文件夹 `/runs/detect/train/weights/` 中。现在，你应该使用 `valid/images` 验证训练好的模型。

```py
!yolo task=detect mode=val model={HOME}/runs/detect/train/\
       weights/best.pt data={dataset.location}/data.yaml
```

结果与训练相似。

1.  现在，我们应该对留出的测试图像进行推理

```py
!yolo task=detect mode=predict model={HOME}/runs/detect/train/\
     weights/best.pt conf=0.25 source={dataset.location}/test/\
     images save=True
```

推理结果保存在文件夹 `runs/detect/predict` 中。让我们看看其中的一些：

![](img/file904.png)

1.  建议将训练、验证和测试结果导出到 Google Drive。为此，我们应该挂载驱动器。

    ```py
    from google.colab import drive
    drive.mount('/content/gdrive')
    ```

    并将 `/runs` 文件夹的内容复制到你在 Drive 中创建的文件夹中，例如：

    ```py
    !scp -r /content/runs '/content/gdrive/MyDrive/\
     10_UNIFEI/Box_vs_Wheel_Project'
    ```

### 使用训练的模型，通过 Raspi 进行推理

将训练好的模型 `/runs/detect/train/weights/best.pt` 下载到你的电脑。使用 FileZilla FTP，让我们将 `best.pt` 转移到 Raspi 模型文件夹（在传输之前，你可能需要更改模型名称，例如，`box_wheel_320_yolo.pt`）。

使用 FileZilla FTP，让我们将测试数据集中的几张图像转移到 `.\YOLO\images`：

让我们回到 YOLO 文件夹并使用 Python 解释器：

```py
cd ..
python
```

如前所述，我们将导入 YOLO 库并定义我们的转换模型以检测蜜蜂：

```py
from ultralytics import YOLO

model = YOLO("./models/box_wheel_320_yolo.pt")
```

现在，让我们定义一个图像并调用推理（这次我们将保存图像结果以进行外部验证）：

```py
img = "./images/1_box_1_wheel.jpg"
result = model.predict(img, save=True, imgsz=320, conf=0.5, iou=0.3)
```

让我们对多张图像重复操作。推理结果保存在变量 `result` 中，处理后的图像在 `runs/detect/predict8`

![](img/file905.png)

使用 FileZilla FTP，我们可以将推理结果发送到我们的桌面进行验证：

![](img/file906.png)

我们可以看到推理结果非常出色！该模型是基于 YOLOv8 家族的小型基础模型（YOLOv8n）训练的。问题是延迟，大约 1 秒（或 Raspi-Zero 上的 1 FPS）。当然，我们可以减少这个延迟并将模型转换为 TFLite 或 NCNN。

## 在实时流中进行目标检测

本实验室中探索的所有模型都可以使用摄像头实时检测物体。捕获的图像应该是训练和转换后的模型的输入。对于带有桌面的 Raspi-4 或 5，OpenCV 可以捕获帧并显示推理结果。

然而，使用摄像头创建实时流以检测物体也是可能的。例如，让我们从为图像分类应用程序开发的脚本开始，并对其进行修改以适应*使用 TensorFlow Lite 和 Flask 的实时目标检测 Web 应用程序*。

此应用程序版本适用于所有 TFLite 模型。请验证模型是否位于其正确的文件夹中，例如：

```py
model_path = "./models/ssd-mobilenet-v1-tflite-default-v1.tflite"
```

从[GitHub](https://github.com/Mjrovai/EdgeML-with-Raspberry-Pi/blob/main/OBJ_DETEC/python_scripts/object_detection_app.py)下载 Python 脚本`object_detection_app.py`。

在终端中运行：

```py
python3 object_detection_app.py
```

然后访问 Web 界面：

+   在 Raspberry Pi 本身（如果您有 GUI）：打开网页浏览器，访问`http://localhost:5000`

+   从同一网络中的另一台设备：打开网页浏览器，访问`http://<raspberry_pi_ip>:5000`（将`<raspberry_pi_ip>`替换为您的 Raspberry Pi 的 IP 地址）。例如：`http://192.168.4.210:` `5000/`

这里有一些应用程序在外部桌面运行时的截图

![图片](img/file907.png)

让我们看看在目标检测应用程序中使用的关键模块的技术描述：

1.  **TensorFlow Lite (tflite_runtime)**：

    +   目的：在边缘设备上高效推理机器学习模型。

    +   为什么：与完整的 TensorFlow 相比，TFLite 提供了更小的模型大小和优化的性能，这对于资源受限的设备（如 Raspberry Pi）至关重要。它支持硬件加速和量化，进一步提高了效率。

    +   关键功能：用于加载和运行模型的`Interpreter`，以及用于与模型交互的`get_input_details()`和`get_output_details()`。

1.  **Flask**：

    +   目的：轻量级 Web 框架，用于创建后端服务器。

    +   为什么：Flask 的简单性和灵活性使其成为快速开发和部署 Web 应用程序的理想选择。它比适合边缘设备的较大框架资源消耗更少。

    +   关键组件：用于定义 API 端点的路由装饰器，用于流式传输视频的`Response`对象，以及用于提供动态 HTML 的`render_template_string`。

1.  **Picamera2**：

    +   目的：与 Raspberry Pi 摄像头模块接口。

    +   为什么：Picamera2 是控制 Raspberry Pi 摄像头的最新库，在性能和功能上优于原始的 Picamera 库。

    +   关键功能：`create_preview_configuration()`用于设置摄像头，`capture_file()`用于捕获帧。

1.  **PIL (Python Imaging Library)**：

    +   目的：图像处理和操作。

    +   原因：PIL 提供了广泛的电影处理能力。它在这里用于调整图像大小、绘制边界框和在不同图像格式之间转换。

    +   关键类：`Image` 用于加载和操作图像，`ImageDraw` 用于在图像上绘制形状和文本。

1.  **NumPy**：

    +   目的：高效的数组操作和数值计算。

    +   原因：NumPy 的数组操作比纯 Python 列表快得多，这对于高效处理图像数据和模型输入/输出至关重要。

    +   关键函数：`array()` 用于创建数组，`expand_dims()` 用于向数组添加维度。

1.  **线程**：

    +   目的：任务的并发执行。

    +   原因：线程允许同时捕获帧、对象检测和 Web 服务器操作，这对于保持实时性能至关重要。

    +   关键组件：`Thread` 类创建独立的执行线程，Lock 用于线程同步。

1.  **io.BytesIO**：

    +   目的：内存中的二进制流。

    +   原因：允许在内存中高效处理图像数据，无需临时文件，提高速度并减少 I/O 操作。

1.  **时间**：

    +   目的：时间相关函数。

    +   原因：用于添加延迟（`time.sleep()`）以控制帧率并进行性能测量。

1.  **jQuery（客户端）**：

    +   目的：简化 DOM 操作和 AJAX 请求。

    +   原因：它使得动态更新网页界面并与服务器通信而不需要页面重新加载变得容易。

    +   关键函数：`.get()` 和 `.post()` 用于 AJAX 请求，DOM 操作方法用于更新用户界面。

关于主应用程序系统架构：

1.  **主线程**：运行 Flask 服务器，处理 HTTP 请求并提供服务端界面。

1.  **相机线程**：持续从相机捕获帧。

1.  **检测线程**：通过 TFLite 模型处理帧进行对象检测。

1.  **帧缓冲区**：共享内存空间（由锁保护），存储最新帧和检测结果。

关于应用程序数据流，我们可以简要描述：

1.  摄像头捕获帧 → 帧缓冲区

1.  检测线程从帧缓冲区读取 → 通过 TFLite 模型处理 → 在帧缓冲区更新检测结果

1.  Flask 路由访问帧缓冲区以提供最新帧和检测结果

1.  网络客户端通过 AJAX 接收更新并更新用户界面

这种架构允许在保持资源受限的边缘设备（如 Raspberry Pi）上运行的响应式 Web 界面的同时，进行高效的实时对象检测。线程和高效的库如 TFLite 和 PIL 使系统能够实时处理视频帧，而 Flask 和 jQuery 提供了一种用户友好的方式来与之交互。

您可以使用另一个预处理的模型测试应用程序，例如 EfficientDet，更改应用程序行：

```py
model_path = "./models/lite-model_efficientdet_lite0_\
 detection_metadata_1.tflite"
```

> 如果我们想使用 SSD-MobileNetV2 模型的应用，该模型在 Edge Impulse Studio 上使用“盒子与轮子”数据集进行训练，代码也应根据输入细节进行适配，正如我们在其[notebook](https://github.com/Mjrovai/EdgeML-with-Raspberry-Pi/blob/main/OBJ_DETEC/notebooks/EI-SSD-MobileNetV2.ipynb)中探讨的那样。

## 摘要

本实验室探讨了在 Raspberry Pi 等边缘设备上实现对象检测的实施，展示了在资源受限的硬件上运行高级计算机视觉任务的强大功能和潜力。我们涵盖了几个关键方面：

1.  **模型比较**：我们研究了不同的对象检测模型，包括 SSD-MobileNet、EfficientDet、FOMO 和 YOLO，比较了它们在边缘设备上的性能和权衡。

1.  **训练和部署**：使用在 Roboflow 上标记的盒子和小轮子（自定义数据集），我们介绍了使用 Edge Impulse Studio 和 Ultralytics 训练模型并在 Raspberry Pi 上部署它们的过程。

1.  **优化技术**：为了提高边缘设备上的推理速度，我们探索了各种优化方法，例如模型量化（TFLite int8）和格式转换（例如，转换为 NCNN）。

1.  **实时应用**：实验室展示了实时对象检测网络应用，展示了这些模型如何集成到实际、交互式系统中。

1.  **性能考虑**：在整个实验室中，我们讨论了模型准确性与推理速度之间的平衡，这是边缘 AI 应用的关键考虑因素。

在边缘设备上执行对象检测的能力为各个领域打开了无数的可能性，从精准农业、工业自动化和质量控制到智能家居应用和环境监测。通过本地处理数据，这些系统可以提供降低延迟、提高隐私性以及在有限连接性的环境中运行的能力。

展望未来，进一步探索的潜在领域包括：

+   实现多模型管道以处理更复杂的任务

+   探索 Raspberry Pi 的硬件加速选项

+   将对象检测与其他传感器集成以构建更全面的边缘 AI 系统

+   开发利用本地处理和云资源的边缘到云解决方案

在边缘设备上进行对象检测可以创建智能、响应迅速的系统，将 AI 的力量直接带入物理世界，开辟了我们与环境和理解环境的新领域。

## 资源

+   [数据集（“盒子与轮子”)](https://universe.roboflow.com/marcelo-rovai-riila/box-versus-wheel-auto-dataset)

+   [Raspi 上的 SSD-MobileNet 笔记本](https://github.com/Mjrovai/EdgeML-with-Raspberry-Pi/blob/main/OBJ_DETEC/notebooks/SSD_MobileNetV1.ipynb)

+   [Raspi 上的 EfficientDet 笔记本](https://github.com/Mjrovai/EdgeML-with-Raspberry-Pi/blob/main/OBJ_DETEC/notebooks/SSD_EfficientDet.ipynb)

+   [FOMO - EI Linux 笔记本在 Raspberry Pi 上](https://github.com/Mjrovai/EdgeML-with-Raspberry-Pi/blob/main/OBJ_DETEC/notebooks/EI-Linux-FOMO.ipynb)

+   [在 CoLab 上进行 YOLOv8 Box 与 Wheel 数据集训练](https://github.com/Mjrovai/EdgeML-with-Raspberry-Pi/blob/main/OBJ_DETEC/notebooks/yolov8_box_vs_wheel.ipynb)

+   [Edge Impulse 项目 - SSD MobileNet 和 FOMO](https://studio.edgeimpulse.com/public/515477/live)

+   [Python 脚本](https://github.com/Mjrovai/EdgeML-with-Raspberry-Pi/tree/main/OBJ_DETEC/python_scripts)

+   [模型](https://github.com/Mjrovai/EdgeML-with-Raspberry-Pi/tree/main/OBJ_DETEC/models)
