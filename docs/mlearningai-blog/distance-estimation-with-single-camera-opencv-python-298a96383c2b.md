# 使用单摄像头进行距离(网络摄像头)估计📷OpenCV-python

> 原文：<https://medium.com/mlearning-ai/distance-estimation-with-single-camera-opencv-python-298a96383c2b?source=collection_archive---------0----------------------->

> 那么，我们如何通过网络摄像头以相当高的精度实时找到物体摄像头的距离，而完全不需要任何额外的硬件，如立体摄像头或深度传感器？*
> 
> 这篇博文将介绍一个叫做三角形相似度的简单算法的实现，对于物体检测，我们将保持简单，只使用 OpenCV 的人脸检测。

距离估计演示视频

# 代码库

这些代码可以在我的 GitHub 仓库中找到。在这里我将解释一些重要的代码片段，如果你需要帮助，请留下你的评论，我很乐意帮助你😎

## 要求

要求非常简单，你需要 Python、OpenCV 和 [Haar-cascade](https://github.com/opencv/opencv/blob/master/data/haarcascades/haarcascade_frontalface_default.xml) 文件用于**人脸检测**。

在您的机器上安装 python 之后，只需打开终端，将下面的命令粘贴到终端中，安装就完成了。

```
pip install opencv-python # on Linux turn pip to pip3
```

# 捕捉参考图像

参考图像允许我们绘制真实世界(物体平面)，因为当我们在 2D 空间(图像)捕捉物体时，我们失去了物体的深度，否则，我们需要一个特殊的相机，可以捕捉深度，因为我们想使用我们的网络摄像头，参考图像是必需的，你需要注意一些事情，所以精度不会下降，你可以使用我的参考图像，这不会有太大影响，但我会建议你自己捕捉它，以获得更高的精度。

在捕捉参考图像时，您必须注意一些事情。

*   图像(帧)的分辨率必须相同，因为在参考图像中，我一直保持 OpenCV 的默认值(640，480)
*   拍摄参考图像时，尽可能保持相机笔直。
*   测量距离目标摄像机的距离( **KNOWN_DISTANCE** )，记下并拍摄设置为 76.2 厘米的图像。
*   测量物体的宽度( **KNOWN_WIDTH** )，并记下来，我的宽度是 14.3 厘米。

## 重要变量

*   **已知距离:**是物体摄像头的距离📷在现实世界中。
*   **KNOWN_WIDTH:** 现实世界中物体的测量宽度
*   **face_width_in_frame:** 人脸检测器提供的图像(帧)中人脸的宽度，以像素为单位

```
KNOWN_DISTANCE = 76.2  # centimeter
KNOWN_WIDTH = 14.3  # centimeter
```

## 对象检测(人脸)

我在这里使用人脸检测，它对每个物体检测器都不适用，只需按照程序操作，

使用 Opencv python 的人脸检测，这是一个简单的检测人脸并返回人脸宽度的函数，以像素为单位

**面部数据**函数只接受一个参数，即图像。

```
def face_data(image):
face_width = 0
    gray_image = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
    faces = face_detector.detectMultiScale(gray_image, 1.3, 5)
    for (x, y, h, w) in faces:
        cv2.rectangle(image, (x, y), (x+w, y+h), WHITE, 1)
        face_width = wreturn face_width
```

## 焦距取景器

焦距探测器功能涉及三个方面:

1.  Measured_distance 是拍摄参考图像时相机到物体的距离， **Known_distance = 72.2 厘米**
2.  Real_width 它是现实世界中物体的测量宽度，这里我测量的是脸部的宽度，大约是 **Known_width =14.3** 厘米
3.  Width_in_rf_image 是图像/帧中对象的宽度，单位为**像素**

这个函数将返回焦距，这是用来寻找距离，它只是一个映射。

代码如下:

```
def FocalLength(measured_distance, real_width, width_in_rf_image): focal_length = (width_in_rf_image* measured_distance)/ real_width return focal_length
```

## **测距仪**

该函数需要三个参数:

1.  以像素为单位的焦距，它是从**焦距探测器**功能返回的
2.  Real_width 它测量的是现实世界中物体的宽度，这里我测量的是脸部的宽度，大约是 **Known_width =14.3 厘米**
3.  Width_in_rf_image 是图像/帧中对象的宽度，单位为**像素**

测距仪功能将返回以厘米为单位的**距离**

代码如下:

```
def Distance_finder(Focal_Length, real_face_width, face_width_in_frame): distance = (real_face_width * Focal_Length)/face_width_in_frame return distance
```

差不多就是这样了，这里有一个视频教程，非常详细地解释了一切，

这是这个算法的一个更精确的版本，这里是 [GitHub 库](https://github.com/Asadullah-Dal17/QR-detection-and-Distance-Estimation)

还有另一种方法( [Aruco Markers](https://docs.opencv.org/4.x/d5/dae/tutorial_aruco_detection.html) )来找到一个更方便和准确的距离，用一个摄像头(笔记本电脑的普通摄像头)，我们将在接下来的帖子中看看它们。

**参考:**

[](https://www.pyimagesearch.com/2015/01/19/find-distance-camera-objectmarker-using-python-opencv/) [## 使用 Python 和 OpenCV 计算相机到物体的距离

### 最后更新于 2021 年 7 月 8 日。几天前，一位名叫 Cameron 的 PyImageSearch 读者发来电子邮件询问方法…

www.pyimagesearch.com](https://www.pyimagesearch.com/2015/01/19/find-distance-camera-objectmarker-using-python-opencv/) [](https://github.com/Asadullah-Dal17/Distance_measurement_using_single_camera) [## GitHub-Asadullah-dal 17/Distance _ measurement _ using _ single _ camera:使用单摄像头测量…

### 如果你想用简单的网络摄像头估计物体的距离，那么这个算法(三角形相似度)将是…

github.com](https://github.com/Asadullah-Dal17/Distance_measurement_using_single_camera) [](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)