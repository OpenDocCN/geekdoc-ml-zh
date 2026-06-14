

# Python 与 OpenCV3

- 计算机视觉
- 图像处理
- 机器学习与目标检测

![](img/9de0cb7cd12c1232044968590f57e471_0_0.png)

Richard Stallman

## OpenCV3 与 Python

### 目录

## 第1章：OpenCV3 与 Python

1. 计算机视觉
   1.1 计算机视觉
   1.2 应用领域
2. OpenCV3 简介与安装
   2.1 OpenCV3 简介
   2.2 在 Windows 上安装
   2.3 在 Mac 上安装
   2.4 在 Ubuntu 上安装
   2.5 在树莓派上安装
3. 基本接口
   3.1 图像与视频输入/输出
   3.2 绘图
   3.3 窗口管理
   3.4 事件处理
4. NumPy 与 Matplotlib
   4.1 NumPy
   4.2 Matplotlib
5. 基本图像处理
   5.1 感兴趣区域 (ROI)
   5.2 颜色空间
   5.3 阈值处理
   5.4 图像算术
   5.5 直方图
   5.6 实践练习
   5.7 人脸合成
   5.8 运动检测监控
6. 几何变换
   6.1 平移、缩放、旋转
   6.2 图像扭曲
   6.3 镜头畸变
7. 实践练习
   - 人脸马赛克
   - 液化工具
   - 畸变相机
8. 滤波器
   8.1 卷积滤波器
   8.2 模糊处理
   8.3 边缘检测
   8.4 形态学操作
   8.5 图像金字塔
   8.6 实践练习
   - 马赛克2
   - 卡通化
9. 图像分割
   9.1 轮廓
   9.2 霍夫变换
   9.3 连通域
   9.4 实践练习
   - 形状识别
   - 文档扫描仪
   - 硬币计数器
10. 匹配与跟踪
    10.1 使用相似图片进行匹配
    10.2 特征与关键点
    10.3 描述符提取器
    10.4 特征匹配
    10.5 跟踪
    10.6 实践练习
    - 全景图制作
    - 书籍封面搜索器
11. 机器学习与目标检测
    11.1 k-均值聚类
    11.2 k-近邻算法
    11.3 支持向量机与方向梯度直方图
    11.4 级联分类器
    11.5 实践练习
    - 人脸马赛克
    - 汉尼拔面具 (Snapchat 滤镜)
    - 人脸扭曲 (Snapchat 滤镜)

##### 计算机视觉

1. **计算机视觉**
2. 应用领域

#### 计算机视觉领域

- 图像处理
  - 使用计算机获取更高质量视频的过程
  - 接收视频后输出新视频
    - 图像增强
    - 图像复原
    - 图像合成
    - 图像分割
- 计算机视觉 (CV)
  - 从视频中获取有意义的信息
  - 从接收的视频中输出数据
    - 目标检测
    - 目标识别
    - 目标跟踪
- 计算机图形学 (CG)
  - 生成不存在的视频
  - 从接收的数据中输出新数据

##### 图像处理案例

- 改善图像

![](img/9de0cb7cd12c1232044968590f57e471_6_0.png)

###### ❖ 图像处理案例

- 改善图像

![](img/9de0cb7cd12c1232044968590f57e471_7_0.png)

- 复原图像

![](img/9de0cb7cd12c1232044968590f57e471_7_1.png)

###### ❖ 图像处理案例

- 合成图像

![](img/9de0cb7cd12c1232044968590f57e471_8_0.png)

- 分割图像

![](img/9de0cb7cd12c1232044968590f57e471_8_1.png)

###### ❖ 计算机视觉案例

- 检测目标

![](img/9de0cb7cd12c1232044968590f57e471_9_0.png)

- 识别目标

![](img/9de0cb7cd12c1232044968590f57e471_9_1.png)

![](img/9de0cb7cd12c1232044968590f57e471_9_2.png)

###### ❖ 计算机视觉案例

- 跟踪目标

![](img/9de0cb7cd12c1232044968590f57e471_10_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_10_1.png)

![](img/9de0cb7cd12c1232044968590f57e471_10_2.png)

![](img/9de0cb7cd12c1232044968590f57e471_10_3.png)

![](img/9de0cb7cd12c1232044968590f57e471_10_4.png)

![](img/9de0cb7cd12c1232044968590f57e471_10_5.png)

##### 计算机视觉

1. 计算机视觉
2. **<u>应用领域</u>**

###### 领域

- 移动设备
  - 相机滤镜，图像通讯应用
- 视频、广播
  - 扭曲，变形，换脸，
  - 色度键，字幕
- 医疗
  - 超声波，X光，CT，MRI 分析诊断
- 体育
  - 姿态分析，视频解读
- 汽车
  - 全景视图，自动驾驶汽车
- 安全、安防、交通
  - 入侵报警，追踪嫌疑人，防护，识别事故类型，门禁控制
- 工业
  - 流程检验，缺陷检测，感知商场运营
- 机器人
  - 识别物体和人脸

####### 移动设备

- Snapchat

![](img/9de0cb7cd12c1232044968590f57e471_12_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_12_1.png)

######## ❖ 移动设备

- Snow 相机

![](img/9de0cb7cd12c1232044968590f57e471_13_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_13_1.png)

######## ❖ 电影、影片

- 扭曲
- 变形

![](img/9de0cb7cd12c1232044968590f57e471_13_2.png)

![](img/9de0cb7cd12c1232044968590f57e471_13_3.png)

######## ❖ 电影、影片

- 换脸

![](img/9de0cb7cd12c1232044968590f57e471_14_0.png)

- 色度键

![](img/9de0cb7cd12c1232044968590f57e471_14_1.png)

######## ❖ 电影、影片

- 色度键

![](img/9de0cb7cd12c1232044968590f57e471_15_0.png)

- 字幕

![](img/9de0cb7cd12c1232044968590f57e471_15_1.png)

####### 医疗诊断

![](img/9de0cb7cd12c1232044968590f57e471_16_0.png)

####### 体育

![](img/9de0cb7cd12c1232044968590f57e471_16_1.png)

######## ❖ 汽车

- 全景视图

![](img/9de0cb7cd12c1232044968590f57e471_17_0.png)

- 自动驾驶汽车

![](img/9de0cb7cd12c1232044968590f57e471_17_1.png)

####### 安全、交通

![](img/9de0cb7cd12c1232044968590f57e471_18_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_18_1.png)

####### 安全、交通

![](img/9de0cb7cd12c1232044968590f57e471_19_0.png)

####### 工业

- 流程管理

![](img/9de0cb7cd12c1232044968590f57e471_19_1.png)

######## ❖ 工业

- 质量、缺陷检测

![](img/9de0cb7cd12c1232044968590f57e471_20_0.png)

######## ❖ 机器人

![](img/9de0cb7cd12c1232044968590f57e471_20_1.png)

##### OpenCV 与 Python

1. **OpenCV3 简介**
2. 在 Windows 上安装
3. 在 Mac 上安装
4. 在 Ubuntu 上安装
5. 在树莓派上安装

#### ❖ OpenCV

- http://opencv.org
- 开源计算机视觉库
- 官方支持
  - 原生语言：C/C++
  - 绑定语言：Python, Java
  - 支持平台：Windows, Linux, Mac, iOS, Android
- 支持 OpenCL 激活硬件加速
- Intel 俄罗斯团队
  - 源于部分 CPU 密集型应用性能增强研究

![](img/9de0cb7cd12c1232044968590f57e471_22_0.png)

#### ❖ 历史

- 发布：1999年1月，基于 IPL（图像处理库），C 语言
- 首个 alpha 版本：2000年
- 5个 Beta 版本：2001 ~ 2005
- 版本 1.0：2006年10月
  - 包含预编译的 Python 模块
  - 添加了 Borland C++ makefiles
- 版本 2.0：2009年9月
  - 新的 C++ API，基于 STL
  - 新的 Python 接口
- 版本 2.2：2010年12月
  - 重新组织包结构（核心、imgproc、features2d、ml、contrib 等）
  - 新的 Python 绑定，需要 NumPy
- 版本 2.3：2011年7月
  - 移植到 Android
  - 用于 OpenCV 2.x 的 Python 模块 cv2
- 版本 3.4：2017年12月，最新版本

#### ❖ OpenCV

- 官方网站：https://opencv.org
- 源代码仓库
  - 主仓库
    - https://github.com/opencv/opencv
    - 官方发行版的代码存储库
  - 附加仓库
    - https://github.com/opencv/opencv_contrib
    - 包含不成熟或未普及的细节
    - 当完成度提高时，会移至主仓库
    - 包含 SIFT、SURF 等有专利问题的代码
    - BSD 许可证
  - 免费
    - 无论用于研究还是工业
    - 不包括附加仓库
  - 无需开源义务
- OpenCV-Python：同时支持 Python 2 和 3（无差异）
  - 2.7, 3.4+

#### ❖ 环境

| 平台 | 版本 | 硬件 |
| :--- | :--- | :--- |
| Windows | Windows 7 或更高版本 | 网络摄像头 |
| Mac | OS X Sierra 或更高版本 | 网络摄像头 |
| Ubuntu | 16.04 LTS | 网络摄像头 |
| 树莓派 | 树莓派3 B 型号<br>Rasbian Stretch | SD 卡 16GB<br>PiCamera 或 USB 网络摄像头 (UVC) |

#### ❖ 前置要求

- Python 3.4+
  - 本课程不支持 Python 2.7
- NumPy 1.8+
- OpenCV-Python 3.4+，包含附加 (contrib) 模块

##### OpenCV 与 Python

1. OpenCV3 简介
2. **在 Windows 上安装**
3. 在 Mac 上安装
4. 在 Ubuntu 上安装
5. 在树莓派上安装

####### 安装 Python

- 仅 Python 3
  - Python 3.4+
- 仅 Python 2
  - Python 2.7
- Python 3 + Python 2
  - 安装顺序很重要
  - 1. 安装 Python 3
  - 2. 安装 Python 2

![](img/9de0cb7cd12c1232044968590f57e471_25_0.png)

##### 安装 Python 3

- https://www.python.org/downloads/
- 下载 Python 3.4+
  - NumPy 不支持 Python 3.3 及以下版本

####### 安装 Python 3.x

- 启动安装向导
- 勾选 "Add Python 3.x to PATH"
- 点击 "Customize installation"

![](img/9de0cb7cd12c1232044968590f57e471_25_1.png)

####### 安装 Python 3.x

- 可选功能

![](img/9de0cb7cd12c1232044968590f57e471_26_0.png)

####### 安装 Python 3.x

- 更改“安装位置”
  - c:\Python36
  - 非必需

![](img/9de0cb7cd12c1232044968590f57e471_26_1.png)

####### 安装 Python 3.x

- 安装完成

![](img/9de0cb7cd12c1232044968590f57e471_27_0.png)

####### 安装 Python 2.7

- [https://www.python.org/downloads/](https://www.python.org/downloads/)
- 下载 Python 2.7

![](img/9de0cb7cd12c1232044968590f57e471_27_1.png)

####### 安装 Python 2.7

- 启动“安装向导”
- 选择安装用户
  - “为所有用户安装”

![](img/9de0cb7cd12c1232044968590f57e471_28_0.png)

####### 安装 Python 2.7

- 选择目标目录
  - 默认路径

![](img/9de0cb7cd12c1232044968590f57e471_28_1.png)

####### 安装 Python 2.7

- 启用“将 python.exe 添加到路径”选项

![](img/9de0cb7cd12c1232044968590f57e471_29_0.png)

####### 安装 Python 2.7

- 安装完成

![](img/9de0cb7cd12c1232044968590f57e471_29_1.png)

####### Python 命令设置

- 创建链接
  - 以管理员身份打开 Windows 命令行
  - mklink c:\windows\python2.exe c:\python27\python.exe
  - mklink c:\windows\python3.exe c:\python36\python.exe
- 检查 Python 版本
  - Python 2
    - python --version
    - python2 --version
  - Python 3
    - python3 --version
- 检查 Pip 版本
  - Python 2
    - pip --version
    - pip2 --version
  - Python 3
    - pip3 --version

![](img/9de0cb7cd12c1232044968590f57e471_30_0.png)

####### Python 命令设置

![](img/9de0cb7cd12c1232044968590f57e471_30_1.png)

####### 安装依赖库

- Numpy
  - 安装
    - pip install numpy
    - pip3 install numpy
  - 在 Python 中检查
    - import numpy as np
    - np.\_\_version\_\_

```
C:\Users\wsw>pip install numpy
Collecting numpy
  Downloading numpy-1.14.0-cp27-none-win32.whl (9.8MB)
    100% |###########################| 9.8MB 75kB/s
Installing collected packages: numpy
Successfully installed numpy-1.14.0

C:\Users\wsw>pip3 install numpy
Collecting numpy
  Downloading numpy-1.14.0-cp36-none-win32.whl (9.8MB)
    100% |###########################| 9.8MB 88kB/s
Installing collected packages: numpy
Successfully installed numpy-1.14.0

>>> import numpy as np
>>> np.__version__
'1.14.0'

>>> import numpy
>>> numpy.__version__
'1.14.0'
```

####### 安装依赖库

- Matplotlib
  - 安装
    - pip install matplotlib
    - pip3 install matplotlib
  - 在 Python 中检查
    - import matplotlib
    - matplotlib.\_\_version\_\_

```
C:\Users\wsw>pip3 install matplotlib
Collecting matplotlib
  Downloading matplotlib-2.1.2-cp36-cp36m-win32.whl (8.5MB)
    100% |###########################| 8.5MB 59kB/s
Collecting pytz (from matplotlib)
  Using cached pytz-2017.3-py2.py3-none-any.whl

C:\Users\wsw>pip install matplotlib
Collecting matplotlib
  Downloading matplotlib-2.1.2-cp27-cp27m-win32.whl (8.2MB)
    100% |###########################| 8.3MB 53kB/s
Requirement already satisfied: numpy>=1.7.1 in c:\python27\lib\site-packages
Collecting pyparsing!=2.0.4,!=2.1.2,!=2.1.6,>=2.0.1 (from matplotlib)
  Downloading pyparsing-2.2.0-py2.py3-none-any.whl (56kB)
    100% |###########################| 61kB 190kB/s
Collecting backports.functools-lru-cache (from matplotlib)
```

```
>>> import matplotlib
>>> matplotlib.__version__
'2.1.2'
```

##### 安装 OpenCV-Python

- 3种方法
  - OpenCV 预编译库（手动）
    - 仅限 Windows
    - 仅限 Python 2.7
    - 易于选择 OpenCV 版本
      - OpenCV 2.4, 3.4
    - 不包含 contrib 包
  - 使用 pip 的 PyPi 预编译包
    - 几乎支持所有平台
    - 支持 Python 2, 3
    - 仅限 OpenCV 3.x
    - contrib 包单独提供
  - 源码编译
    - VisualStudio / MinGW
    - CMake
    - https://docs.opencv.org/master/d3/d52/tutorial_windows_install.html

####### OpenCV 预编译库

- 下载 Windows 包
  - https://opencv.org/releases.html
    - 3.4 , 2.4

![](img/9de0cb7cd12c1232044968590f57e471_32_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_32_1.png)

####### OpenCV 预编译库

- 自解压归档文件

![](img/9de0cb7cd12c1232044968590f57e471_33_0.png)

####### OpenCV 预编译库

- 将库复制到 site-packages
  - 从
    - opencv/build/bin/python/2.7/x64/cv2.pyd
    - opencv/build/bin/python/2.7/x86/cv2.pyd
  - 到
    - c:\Python27\Lib/site-packages

![](img/9de0cb7cd12c1232044968590f57e471_33_1.png)

####### OpenCV 预编译库

- 在 Python 2 中检查已安装的 OpenCV 版本
  - import cv2
  - cv2.\_\_version\_\_
- 不支持 Python 3

```
>>> import cv2
>>> cv2.__version__
'3.4.0'
>>> import cv2
>>> cv2.__version__
'2.4.13.5'
```

```
C:\Users\sw>python3
Python 3.6.4 (v3.6.4:d48eceb, Dec 19 2017, 06:04:45) [MSC v.1900 32 bit (Intel)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import cv2
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ImportError: Module use of python27.dll conflicts with this version of Python
```

####### OpenCV-Python 安装 - PyPi

- Python 的非官方 OpenCV 包
- OpenCV-Python（最新版本）
  - https://pypi.python.org/pypi/opencv-python
  - 核心模块，不含额外（contrib）模块
  - 安装 OpenCV-Python
    - pip install opencv-python
    - pip3 install opencv-python

```
C:\Users\sw>pip install opencv-python
Collecting opencv-python
  Downloading opencv_python-3.4.0.12-cp27-cp27m-win32.whl (22.8MB)
    100% |###########################| 22.8MB 32kB/s
Requirement already satisfied: numpy>=1.11.1 in c:\python27\lib\site-packages (from opencv-python)
Installing collected packages: opencv-python
Successfully installed opencv-python-3.4.0.12
```

```
C:\Users\sw>pip3 install opencv-python
Collecting opencv-python
  Downloading opencv_python-3.4.0.12-cp36-cp36m-win32.whl (22.8MB)
    100% |###########################| 22.8MB 31kB/s
Requirement already satisfied: numpy>=1.11.3 in c:\python36\lib\site-packages (from opencv-python)
Installing collected packages: opencv-python
Successfully installed opencv-python-3.4.0.12
```

####### ❖ OpenCV-Python 安装 - PyPi

- Python 的非官方 OpenCV 包
- OpenCV-Contrib-Python（最新版本）
  - https://pypi.python.org/pypi/opencv-contrib-python
  - 包含 contrib 模块
  - 安装 OpenCV-Contrib-Python
    - pip install opencv-contrib-python
    - pip3 install opencv-contrib-python

```
C:\Users\sw>pip install opencv-contrib-python
Collecting opencv-contrib-python
  Downloading opencv_contrib_python-3.4.0.12-cp27-cp27m-win32.whl (27.5MB)
    100% |###########################| 27.5MB 24kB/s
Requirement already satisfied: numpy>=1.11.1 in c:\python27\lib\site-packages (from opencv-contrib-python)
Installing collected packages: opencv-contrib-python
Successfully installed opencv-contrib-python-3.4.0.12

C:\Users\sw>pip3 install opencv-contrib-python
Collecting opencv-contrib-python
  Downloading opencv_contrib_python-3.4.0.12-cp36-cp36m-win32.whl (27.5MB)
    100% |###########################| 27.5MB 13kB/s
Requirement already satisfied: numpy>=1.11.3 in c:\python36\lib\site-packages (from opencv-contrib-python)
Installing collected packages: opencv-contrib-python
Successfully installed opencv-contrib-python-3.4.0.12
```

####### ❖ OpenCV-Python 安装 - PyPi

- 指定版本安装
  - 列出 OpenCV 版本（仅限 3.x）
    - pip install opencv-python==?
  - 安装特定版本（示例）
    - pip install opencv-python==3.1.x.x

```
C:\Users\sw>pip install opencv-python==?
Collecting opencv-python==?
  Could not find a version that satisfies the requirement opencv-python==? (from versions: 3.1.0.0, 3.1.0.1, 3.1.0.2, 3.1.0.3, 3.1.0.5, 3.2.0.6, 3.2.0.7, 3.2.0.8, 3.3.0.9, 3.3.0.10, 3.3.1.11, 3.4.0.12)
No matching distribution found for opencv-python==?
```

####### ❖ OpenCV-Python 安装 - PyPi

- 检查已安装版本
  - import cv2
  - cv2.\_\_version\_\_

```
C:\Users\sw>python
Python 2.7.14 (v2.7.14:84471935ed
(Intel)] on win32
Type "help", "copyright", "credits
>>> import cv2
>>> cv2.__version__
'3.4.0'

C:\Users\sw>python3
Python 3.6.4 (v3.6.4:d48eceb, Dec 19 2
l)] on win32
Type "help", "copyright", "credits" or
>>> import cv2
>>> cv2.__version__
'3.4.0'
>>>
```

- 测试代码

```
import cv2

img_file = "../img/model.jpg"
img = cv2.imread(img_file)

if not img is None:
    cv2.imshow('IMG', img)
    while True:
        key = cv2.waitKey(0) #& 0xFF
        if key == 27:
            cv2.destroyAllWindows()
            break
else:
    print('No image file.')
```

![](img/9de0cb7cd12c1232044968590f57e471_36_0.png)

##### 使用Python进行OpenCV开发

- 1. OpenCV3简介
- 2. 在Windows上安装
- 3. **在Mac上安装**
- 4. 在Ubuntu上安装
- 5. 在树莓派上安装

###### 安装Python

- 预装的Python不支持pip
- 仅Python 3
  - Python 3.4+
- 仅Python 2
  - Python 2.7
- Python 3 + Python 2
  - 安装顺序无关紧要
  - 1. 安装Python 3
  - 2. 安装Python 2

####### 安装Python 3

- https://www.python.org/downloads/
- 下载Python 3.4+
  - Numpy不支持Python 3.3

![](img/9de0cb7cd12c1232044968590f57e471_38_0.png)

######## 安装Python 3.x

- 启动安装向导

![](img/9de0cb7cd12c1232044968590f57e471_39_0.png)

- 许可协议

![](img/9de0cb7cd12c1232044968590f57e471_39_1.png)

######### ❖ 安装Python 3.x

- 标准安装

![](img/9de0cb7cd12c1232044968590f57e471_40_0.png)

- 安装完成

![](img/9de0cb7cd12c1232044968590f57e471_40_1.png)

######### ❖ 检查Python 3

- python3 --version

######### ❖ 检查Pip版本

- pip3 --version

```
rainer@swMBA:~$ python3 --version
Python 3.6.4
rainer@swMBA:~$ pip3 --version
pip 9.0.1 from /Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/site-packages (python 3.6)
```

## ❖ 安装Python 2.7

- https://www.python.org/downloads/
- 下载Python 2.7

![](img/9de0cb7cd12c1232044968590f57e471_41_0.png)

######## 安装Python 2.7

- 启动“安装向导”

![](img/9de0cb7cd12c1232044968590f57e471_42_0.png)

- 重要信息

![](img/9de0cb7cd12c1232044968590f57e471_42_1.png)

######## 安装Python 2.7

- 许可协议

![](img/9de0cb7cd12c1232044968590f57e471_43_0.png)

- 标准安装

![](img/9de0cb7cd12c1232044968590f57e471_43_1.png)

######## 安装Python 2.7

- 安装完成

![](img/9de0cb7cd12c1232044968590f57e471_44_0.png)

######## 检查Python 2

- python --version

######## 检查Pip版本

- pip --version

```
rainer@swMBA:~$ python --version
Python 2.7.14
rainer@swMBA:~$ pip --version
pip 9.0.1 from /Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/site-packages (python 2.7)
```

####### 安装依赖库

- 安装Numpy
  - pip install numpy
  - pip3 install numpy

```
rainer@swMBA:~$ pip install numpy
Collecting numpy
  Downloading numpy-1.14.0-cp27-cp27m-macosx_10_6_intel.macosx_10_9_intel.macosx_10_9_x86_64.macosx_10_10_intel.macosx_10_10_x86_64.whl (4.7MB)
    100% |████████████████████████████████| 4.7MB 233kB/s 
Installing collected packages: numpy
Successfully installed numpy-1.14.0
rainer@swMBA:~$ pip3 install numpy
Collecting numpy
  Downloading numpy-1.14.0-cp36-cp36m-macosx_10_6_intel.macosx_10_9_intel.macosx_10_9_x86_64.macosx_10_10_intel.macosx_10_10_x86_64.whl (4.7MB)
    100% |████████████████████████████████| 4.7MB 68kB/s 
Installing collected packages: numpy
Successfully installed numpy-1.14.0
```

- 在Python中检查Numpy
  - import numpy as np
  - np.__version__

```
rainer@swMBA:~$ python
Python 2.7.14 (v2.7.14:84471935ed, Sep 16 2017, 12:01:12) 
[GCC 4.2.1 (Apple Inc. build 5666) (dot 3)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import numpy
>>> numpy.__version__
'1.14.0'
rainer@swMBA:~$ python3
Python 3.6.4 (v3.6.4:d48ecebad5, Dec 18 2017, 21:07:28) 
[GCC 4.2.1 (Apple Inc. build 5666) (dot 3)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import numpy
>>> numpy.__version__
'1.14.0'
```

####### 安装依赖库

- 安装Matplotlib
  - pip install matplotlib
  - pip3 install matplotlib

```
rainer@swMBA:~$ pip install matplotlib
Collecting matplotlib
  Downloading matplotlib-2.1.2-cp27-cp27m-macosx_10_6_intel.macosx_10_9_intel.macosx_10_9_x86_64.macosx_10_10_intel.macosx_10_10_x86_64.whl (13.2MB)
    100% |████████████████████████████████| 13.2MB 60kB/s 
Requirement already satisfied: numpy>=1.7.1 in /Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/site-packages (from matplotlib)
```

```
rainer@swMBA:~$ pip3 install matplotlib
Collecting matplotlib
  Downloading matplotlib-2.1.2-cp36-cp36m-macosx_10_6_intel.macosx_10_9_intel.macosx_10_9_x86_64.macosx_10_10_intel.macosx_10_10_x86_64.whl (13.2MB)
    100% |████████████████████████████████| 13.2MB 95kB/s 
Requirement already satisfied: pytz in /Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/site-packages (from matplotlib)
```

- 在Python中检查Matplotlib
  - import matplotlib
  - matplotlib.__version__

```
rainer@swMBA:~$ python
Python 2.7.14 (v2.7.14:84471935ed, Sep 16 2017, 12:01:12) 
[GCC 4.2.1 (Apple Inc. build 5666) (dot 3)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import matplotlib
>>> matplotlib.__version__
'2.1.2'
```

```
rainer@swMBA:~$ python3
Python 3.6.4 (v3.6.4:d48ecebad5, Dec 18 2017, 21:07:28) 
[GCC 4.2.1 (Apple Inc. build 5666) (dot 3)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import matplotlib
>>> matplotlib.__version__
'2.1.2'
```

####### ❖ OpenCV-Python安装 - PyPi

- Python的非官方OpenCV包
- OpenCV-Python（最新版本）
  - https://pypi.python.org/pypi/opencv-python
  - 核心模块，不包含contrib模块
  - 安装OpenCV-Python
    - pip install opencv-python
    - pip3 install opencv-python

```
rainer@swMBA:~$ pip install opencv-python
Collecting opencv-python
  Downloading opencv_python-3.4.0.12-cp27-cp27m-macosx_10_6_intel.macosx_10_9_intel.macosx_10_9_x86_64.macosx_10_10_intel.macosx_10_10_x86_64.whl (41.2MB)
    100% |████████████████████████████████| 41.2MB 25kB/s 
Requirement already satisfied: numpy>=1.11.1 in /Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/site-packages (from opencv-python)
Installing collected packages: opencv-python
Successfully installed opencv-python-3.4.0.12
```

```
rainer@swMBA:~$ pip3 install opencv-python
Collecting opencv-python
  Downloading opencv_python-3.4.0.12-cp36-cp36m-macosx_10_6_intel.macosx_10_9_intel.macosx_10_9_x86_64.macosx_10_10_intel.macosx_10_10_x86_64.whl (41.2MB)
    100% |████████████████████████████████| 41.2MB 24kB/s 
Requirement already satisfied: numpy>=1.11.1 in /Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/site-packages (from opencv-python)
Installing collected packages: opencv-python
Successfully installed opencv-python-3.4.0.12
```

- Python的非官方OpenCV包
- OpenCV-Contrib-Python（最新版本）
  - https://pypi.python.org/pypi/opencv-contrib-python
  - 包含contrib模块
  - 安装OpenCV-Contrib-Python
    - pip install opencv-contrib-python
    - pip3 install opencv-contrib-python

```
rainer@swMBA:~$ pip install opencv-contrib-python
Collecting opencv-contrib-python
  Downloading opencv_contrib_python-3.4.0.12-cp27-cp27m-macosx_10_6_intel.macosx_10_9_intel.macosx_10_9_x86_64.macosx_10_10_intel.macosx_10_10_x86_64.whl (46.4MB)
    100% |████████████████████████████████| 46.4MB 25kB/s
Requirement already satisfied: numpy>=1.11.1 in /Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/site-packages (from opencv-contrib-python)
Installing collected packages: opencv-contrib-python
Successfully installed opencv-contrib-python-3.4.0.12
```

```
rainer@swMBA:~$ pip3 install opencv-contrib-python
Collecting opencv-contrib-python
  Downloading opencv_contrib_python-3.4.0.12-cp36-cp36m-macosx_10_6_intel.macosx_10_9_intel.macosx_10_9_x86_64.macosx_10_10_intel.macosx_10_10_x86_64.whl (46.4MB)
    100% |████████████████████████████████| 46.4MB 27kB/s
Requirement already satisfied: numpy>=1.11.1 in /Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/site-packages (from opencv-contrib-python)
Installing collected packages: opencv-contrib-python
Successfully installed opencv-contrib-python-3.4.0.12
```

####### ❖ OpenCV-Python安装 - PyPi

- 指定版本安装
  - 列出OpenCV版本（仅限3.x）
    - pip install opencv-python==?
  - 安装特定版本（示例）
    - pip install opencv-python==3.1.x.x

```
rainer@swMBA:~$ pip install opencv-python==?
Collecting opencv-python==?
  Could not find a version that satisfies the requirement opencv-python==? (from versions: 3.1.0.0, 3.1.0.1, 3.1.0.2, 3.1.0.3, 3.1.0.4, 3.1.0.5, 3.2.0.6, 3.2.0.7, 3.2.0.8, 3.3.0.9, 3.3.0.10, 3.3.1.11, 3.4.0.12)
No matching distribution found for opencv-python==?
```

####### OpenCV-Python 安装 - PyPi

- 查看已安装版本
  - import cv2
  - cv2.__version__

```
rainer@swMBA:~$ python
Python 2.7.14 (v2.7.14:84471935ed, Sep 16 2017, 12:01:12)
[GCC 4.2.1 (Apple Inc. build 5666) (dot 3)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import cv2
>>> cv2.__version__
'3.4.0'

rainer@swMBA:~$ python3
Python 3.6.4 (v3.6.4:d48ecebad5, Dec 18 2017, 21:07:28)
[GCC 4.2.1 (Apple Inc. build 5666) (dot 3)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import cv2
>>> cv2.__version__
'3.4.0'
```

###### 测试代码

```
import cv2

img_file = "./img/girl.jpg" # Path to image
img = cv2.imread(img_file)

if not img is None:
    cv2.imshow('IMG', img)
    cv2.waitKey(0)
    cv2.destroyAllWindows()
else:
    print('No image file.')
```

![](img/9de0cb7cd12c1232044968590f57e471_49_0.png)

##### OpenCV 与 Python

1. OpenCV3 简介
2. 在 Windows 上安装
3. 在 Mac 上安装
4. **在 Ubuntu 上安装**
5. 在树莓派上安装

####### 安装 Python

- Ubuntu 16.04 LTS
- 检查是否已安装
  - python --version
  - python3 --version
- 如果未安装 Python
  - 安装 Python2
    - sudo apt-get install python
  - 安装 Python 3
    - sudo apt-get install python3
- 安装并检查 Pip
  - 用于 python 的 pip
    - sudo apt-get install python-pip
    - pip --version

```
lee@vbox-opencv:~$ python --version
Python 2.7.12
lee@vbox-opencv:~$ python3 --version
Python 3.5.2
lee@vbox-opencv:~$
```

```
lee@vbox-opencv:~$ sudo apt-get install python-pip
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following packages were automatically installed and are no longer required:
  libllvm4.0 libpython3-dev libpython3.5-dev python3-dev python3-wheel
  python3.5-dev
Use 'sudo apt autoremove' to remove them.
The following NEW packages will be installed:
  python-pip
```

```
lee@vbox-opencv:~$ pip --version
pip 8.1.1 from /usr/lib/python2.7/dist-packages (python 2.7)
```

####### 安装 Python

- 安装并检查 Pip
  - 用于 python3 的 pip3
    - sudo apt-get install python3-pip
    - pip3 --version

```
lee@vbox-opencv:~$ sudo apt-get install python3-pip
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following package was automatically installed and is no longer required:
  libllvm4.0
Use 'sudo apt autoremove' to remove it.
The following NEW packages will be installed:
  python3-pip
0 upgraded, 1 newly installed, 0 to remove and 1 not upgraded.
Need to get 175 kB of archives.
After this operation, 747 kB of additional disk space will be used.
Get:1 http://us.archive.ubuntu.com/ubuntu artful/universe amd64 python3-pip all 9.0.1-1 [175 kB]
Fetched 175 kB in 0s (433 kB/s)
Selecting previously unselected package python3-pip.
(Reading database ... 284415 files and directories currently installed.)
Preparing to unpack .../python3-pip_9.0.1-1_all.deb ...
Unpacking python3-pip (9.0.1-1) ...
Setting up python3-pip (9.0.1-1) ...
Processing triggers for man-db (2.7.6-1) ...
```

```
lee@vbox-opencv:~$ pip3 --version
pip 8.1.1 from /usr/lib/python3/dist-packages (python 3.5)
```

####### 安装依赖库

- 安装 Numpy
  - pip install numpy
  - pip3 install numpy

```
lee@vbox-opencv:~$ pip install numpy
Collecting numpy
  Using cached numpy-1.14.0-cp27-cp27mu-manylinux1_x86_64.whl
Installing collected packages: numpy
Successfully installed numpy-1.14.0
```

```
lee@vbox-opencv:~$ pip3 install numpy
Collecting numpy
  Using cached numpy-1.14.0-cp35-cp35m-manylinux1_x86_64.whl
Installing collected packages: numpy
Successfully installed numpy-1.14.0
```

- 在 Python 中检查 Numpy
  - import numpy
  - numpy.__version__

```
lee@vbox-opencv:~$ python
Python 2.7.12 (default, Nov 20 2017, 18:23:56)
[GCC 5.4.0 20160609] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import numpy
>>> numpy.__version__
'1.14.0'
```

```
lee@vbox-opencv:~$ python3
Python 3.5.2 (default, Nov 23 2017, 16:37:01)
[GCC 5.4.0 20160609] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import numpy
>>> numpy.__version__
'1.14.0'
```

####### 安装依赖库

- 安装 Matplotlib
  - pip install matplotlib
  - pip3 install matplotlib

```
lee@vbox-opencv:~$ pip install matplotlib
Collecting matplotlib
  Downloading matplotlib-2.1.2-cp27-cp27mu-manylinux1_x86_64.whl (15.0MB)
    100% |████████████████████████████████| 15.0MB 64kB/s
Collecting numpy>=1.7.1 (from matplotlib)
  Using cached numpy-1.14.0-cp27-cp27mu-manylinux1_x86_64.whl
```

```
lee@vbox-opencv:~$ pip3 install matplotlib
Collecting matplotlib
  Downloading matplotlib-2.1.2-cp35-cp35m-manylinux1_x86_64.whl (15.0MB)
    100% |████████████████████████████████| 15.0MB 85kB/s
Collecting six>=1.10 (from matplotlib)
  Using cached six-1.11.0-py2.py3-none-any.whl
```

- 在 Python 中检查 Matplotlib
  - import matplotlib
  - matplotlib.__version__

```
lee@vbox-opencv:~$ python
Python 2.7.12 (default, Nov 20 2017, 18:23:56)
[GCC 5.4.0 20160609] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import matplotlib
>>> matplotlib.__version__
'2.1.2'
```

```
lee@vbox-opencv:~$ python3
Python 3.5.2 (default, Nov 23 2017, 16:37:01)
[GCC 5.4.0 20160609] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import matplotlib
>>> matplotlib.__version__
'2.1.2'
```

####### ❖ OpenCV-Python 安装 - PyPi

- Python 的非官方 OpenCV 包
- OpenCV-Python（最新版本）
  - https://pypi.python.org/pypi/opencv-python
  - 不含 contrib 模块的核心模块
  - 安装 OpenCV-Python
    - pip install opencv-python
    - pip3 install opencv-python

```
lee@vbox-opencv:~$ pip install opencv-python
Collecting opencv-python
  Using cached opencv_python-3.4.0.12-cp27-cp27mu-manylinux1_x86_64.whl
Collecting numpy>=1.11.1 (from opencv-python)
  Using cached numpy-1.14.0-cp27-cp27mu-manylinux1_x86_64.whl
Installing collected packages: numpy, opencv-python
Successfully installed numpy-1.14.0 opencv-python-3.4.0.12
```

```
lee@vbox-opencv:~$ pip3 install opencv-python
Collecting opencv-python
  Downloading opencv_python-3.4.0.12-cp35-cp35m-manylinux1_x86_64.whl (24.9MB)
    100% |████████████████████████████████| 24.9MB 58kB/s
Collecting numpy>=1.11.1 (from opencv-python)
  Using cached numpy-1.14.0-cp35-cp35m-manylinux1_x86_64.whl
Installing collected packages: numpy, opencv-python
Successfully installed numpy-1.14.0 opencv-python-3.4.0.12
```

- Python 的非官方 OpenCV 包
- OpenCV-Contrib-Python（最新版本）
  - https://pypi.python.org/pypi/opencv-contrib-python
  - 包含 contrib 模块
  - 安装 OpenCV-Contrib-Python
    - pip install opencv-contrib-python
    - pip3 install opencv-contrib-python

```
lee@vbox-opencv:~$ pip install opencv-contrib-python
Collecting opencv-contrib-python
  Downloading opencv_contrib_python-3.4.0.12-cp27-cp27mu-manylinux1_x86_64.whl (30.5MB)
    100% |████████████████████████████████| 30.5MB 34kB/s
Collecting numpy>=1.11.1 (from opencv-contrib-python)
  Using cached numpy-1.14.0-cp27-cp27mu-manylinux1_x86_64.whl
Installing collected packages: numpy, opencv-contrib-python
Successfully installed numpy-1.14.0 opencv-contrib-python-3.3.1.11
```

```
lee@vbox-opencv:~$ pip3 install opencv-contrib-python
Collecting opencv-contrib-python
  Using cached opencv_contrib_python-3.4.0.12-cp35-cp35m-manylinux1
Collecting numpy>=1.11.1 (from opencv-contrib-python)
  Using cached numpy-1.14.0-cp35-cp35m-manylinux1_x86_64.whl
Installing collected packages: numpy, opencv-contrib-python
Successfully installed numpy-1.14.0 opencv-contrib-python-3.4.0.12
```

####### ❖ OpenCV-Python 安装 - PyPi

- 指定版本安装
  - 列出 OpenCV 版本（仅限 3.x）
    - pip install opencv-python==?
  - 安装特定版本（示例）
    - pip install opencv-python=3.1.x.x

```
lee@vbox-opencv:~$ pip3 install opencv-contrib-python==?
Collecting opencv-contrib-python==?
Could not find a version that satisfies the requirement opencv-contrib-python=
==? (from versions: 3.2.0.7, 3.2.0.8, 3.3.0.9, 3.3.0.10, 3.3.1.11, 3.4.0.12)
No matching distribution found for opencv-contrib-python==?
```

- 查看已安装版本
  - import cv2
  - cv2.__version__

```
lee@vbox-opencv:~$ python
Python 2.7.12 (default, Nov 20 2017, 18:23:56)
[GCC 5.4.0 20160609] on linux2
Type "help", "copyright", "credits" or "license" for more informa
>>> import cv2
>>> cv2.__version__
'3.4.0'
```

```
lee@vbox-opencv:~$ python3
Python 3.5.2 (default, Nov 23 2017, 16:37:01)
[GCC 5.4.0 20160609] on linux
Type "help", "copyright", "credits" or "license" for more infor
>>> import cv2
>>> cv2.__version__
'3.4.0'
```

###### 测试代码

```python
import cv2

img_file = "img/model.jpg"
img = cv2.imread(img_file)

if img:
    cv2.imshow('model', img)
    cv2.waitKey(0)
    cv2.destroyAllWindows()
```

![](img/9de0cb7cd12c1232044968590f57e471_56_0.png)

##### 使用Python的OpenCV

- 1. OpenCV3简介
- 2. 在Windows上安装
- 3. 在Mac上安装
- 4. 在Ubuntu上安装
- 5. 在树莓派上安装

###### 安装Python和pip

- Raspbian Stretch版本
- 检查默认安装
  - python --version
  - python3 --version
  - pip --version
  - pip3 --version

```
pi@raspberry:~ $ python --version
Python 2.7.13
pi@raspberry:~ $ python3 --version
Python 3.5.3
pi@raspberry:~ $ pip --version
pip 9.0.1 from /usr/lib/python2.7/dist-packages (python 2.7)
pi@raspberry:~ $ pip3 --version
pip 9.0.1 from /usr/lib/python3/dist-packages (python 3.5)
```

####### 安装Numpy

- pip不支持树莓派的numpy
- 使用APT安装
  - sudo apt-get install python-numpy
  - sudo apt-get install python3-numpy

```
pi@raspberrypi:~ $ sudo apt-get install python-numpy
Reading package lists... Done
Building dependency tree
Reading state information... Done
```

```
pi@raspberrypi:~ $ sudo apt-get install python3-numpy
Reading package lists... Done
Building dependency tree
Reading state information... Done
```

####### 安装Numpy

- 检查numpy版本
  - import numpy
  - numpy.__version__

```
pi@raspberrypi:~ $ python
Python 2.7.13 (default, Jan 19 2017, 14:48:08)
[GCC 6.3.0 20170124] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import numpy
>>> numpy.__version__
'1.12.1'

pi@raspberrypi:~ $ python3
Python 3.5.3 (default, Jan 19 2017, 14:11:04)
[GCC 6.3.0 20170124] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import numpy
>>> numpy.__version__
'1.12.1'
```

###### 安装OpenCV

- pip不支持树莓派的OpenCV-Python
  - pip默认仓库
    - https://pypi.python.org/
    - pypi不支持armv7l的OpenCV-Python
  - piwheels仓库
    - https://www.piwheels.hostedpi.com/
    - 树莓派专用仓库
    - /etc/pip.conf (rasbian stretch)
    - 无法正常工作 (2018年1月)
- 安装OpenCV-Python的3种方法
  - 从APT仓库安装
  - 从源码编译安装
  - 使用预编译的Debian包安装
    - 本课程提供

![](img/9de0cb7cd12c1232044968590f57e471_59_0.png)

树莓派的Python Wheels
piwheels是一个Python包仓库，专门为树莓派提供ARM平台的wheels（预编译的二进制Python包）。这些包使用Mythic Beasts Pi云在树莓派3硬件上原生编译。

| 包数量 | 104,642 |
| Wheels数量 | 745,847 |
| 下载量（过去30天） | 366,260 |

配置
Raspbian Stretch默认配置了pip使用piwheels。如果你使用其他发行版（或旧版本的Raspbian），可以通过在`/etc/pip.conf`中添加以下行来使用piwheels：

```
[global]
extra-index-url=https://www.piwheels.hostedpi.com/simple
```

####### 从APT仓库安装OpenCV

- 仅支持Python2，OpenCV 2.4
- 在树莓派上安装OpenCV-Python最简单的方法
- 不支持Python3，OpenCV 3+
- 安装
  - sudo apt-get install python-opencv

```
pi@raspberrypi:~ $ sudo apt-get install python-opencv
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following additional packages will be installed:
  libopencv-contrib2.4v5 libopencv-legacy2.4v5 libopencv-ml2.4v5
  libopencv-photo2.4v5
The following NEW packages will be installed:
  libopencv-contrib2.4v5 libopencv-legacy2.4v5 libopencv-ml2.4v5
  libopencv-photo2.4v5 python-opencv
```

- 检查版本
  - import cv2
  - cv2.__version__

```
pi@raspberrypi:~ $ python
Python 2.7.13 (default, Jan 19 2017, 14:48:08)
[GCC 6.3.0 20170124] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import cv2
>>> cv2.__version__
'2.4.9.1'
```

####### 从源码编译安装OpenCV

- 安装编译过程所需的工具
  - sudo apt install build-essential cmake pkg-config
- 安装依赖库
  - 图像格式库
    - sudo apt-get install -y libjpeg-dev libtiff5-dev libjasper-dev libpng12-dev
  - 视频编解码库
    - sudo apt-get install -y libavcodec-dev libavformat-dev libswscale-dev libxvidcore-dev libx264-dev libxine2-dev
  - Linux标准视频设备库
    - sudo apt-get install -y libv4l-dev v4l-utils
  - 视频流库
    - sudo apt-get install -y libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev
  - GUI窗口库
    - sudo apt-get install libqt4-dev
- 安装依赖库（续）
  - OpenGL库
    - sudo apt-get install -y mesa-utils libgl1-mesa-dri libqt4-opengl-dev libatlas-base-dev gfortran libeigen3-dev
  - Python库
    - sudo apt-get install -y python-dev python3-dev
  - Python Numpy
    - sudo apt-get install -y python-numpy python3-numpy
- 创建目录 "~/opencv"
  - ~ $ mkdir opencv
  - ~ $ cd opencv
- 下载OpenCV源代码
  - https://github.com/opencv/opencv/releases
  - https://github.com/opencv/opencv_contrib/releases

![](img/9de0cb7cd12c1232044968590f57e471_62_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_62_1.png)

####### 从源码编译安装OpenCV

- 下载OpenCV源代码
  - ~/opencv$ wget -O opencv-3.4.tar.gz https://github.com/opencv/opencv/archive/3.4.0.tar.gz

```
pi@raspberrypi:~/opencv $ wget -O opencv-3.4.tar.gz https://github.com/opencv/opencv/archive/3.4.0.tar.gz
--2018-02-03 08:50:44--  https://github.com/opencv/opencv/archive/3.4.0.tar.gz
Resolving github.com (github.com)... 192.30.255.112, 192.30.255.113
Connecting to github.com (github.com)|192.30.255.112|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://codeload.github.com/opencv/opencv/tar.gz/3.4.0 [following]
--2018-02-03 08:50:45--  https://codeload.github.com/opencv/opencv/tar.gz/3.4.0
Resolving codeload.github.com (codeload.github.com)... 192.30.255.120, 192.30.255.121
Connecting to codeload.github.com (codeload.github.com)|192.30.255.120|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 87169544 (83M) [application/x-gzip]
Saving to: 'opencv-3.4.tar.gz'

opencv-3.4.tar.gz  100%[===================>]  83.13M  2.37MB/s    in 91s
```

####### 从源码编译安装OpenCV

- 下载OpenCV源代码
  - ~/opencv$ wget -O opencv-3.4-contrib.tar.gz https://github.com/opencv/opencv_contrib/archive/3.4.0.tar.gz

```
pi@raspberrypi:~/opencv $ wget -O opencv-contrib-3.4.tar.gz https://github.com/opencv/opencv_contrib/archive/3.4.0.tar.gz
--2018-02-03 09:03:32--  https://github.com/opencv/opencv_contrib/archive/3.4.0.tar.gz
Resolving github.com (github.com)... 192.30.255.113, 192.30.255.112
Connecting to github.com (github.com)|192.30.255.113|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://codeload.github.com/opencv/opencv_contrib/tar.gz/3.4.0 [following]
--2018-02-03 09:03:33--  https://codeload.github.com/opencv/opencv_contrib/tar.gz/3.4.0
Resolving codeload.github.com (codeload.github.com)... 192.30.255.121, 192.30.255.120
Connecting to codeload.github.com (codeload.github.com)|192.30.255.121|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 55978680 (53M) [application/x-gzip]
Saving to: 'opencv-contrib-3.4.tar.gz'

opencv-contrib-3.4. 100%[===================>]  53.38M  2.64MB/s    in 52s
```

####### 从源码编译安装OpenCV

- 解压
  - ~/opencv $ tar -xvf opencv-3.4.tar.gz
  - ~/opencv $ tar -xvf opencv-contrib-3.4.tar.gz
- 创建编译目录
  - ~/opencv $ mkdir -p ./opencv-3.4.0/build
  - ~/opencv $ cd ./opencv-3.4.0/build

```
pi@raspberrypi:~/opencv $ ls
opencv-3.4.0        opencv-contrib-3.4.tar.gz
opencv-3.4.tar.gz   opencv_contrib-3.4.0
pi@raspberrypi:~/opencv $ mkdir -p ./opencv-3.4.0/build
pi@raspberrypi:~/opencv $ cd ./opencv-3.4.0/build/
pi@raspberrypi:~/opencv/opencv-3.4.0/build $
```

####### 从源码构建安装 OpenCV

- CMake

```
~/opencv/opencv3.4.0/build $ cmake -D CMAKE_BUILD_TYPE=RELEASE \
  -D CMAKE_INSTALL_PREFIX=/usr/local \
  -D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib-3.4.0/modules \
  -D BUILD_DOCS=ON \
  -D BUILD_EXAMPLES=ON \
  -D ENABLE_NEON=ON \
  -D WITH_QT=ON \
  -D WITH_OPENGL=ON \
  -D WITH_XINE=ON ../
```

- 交换内存
  - 内存检查
    - free -h
  - 交换空间检查
    - swapon -s
  - 创建 1GB 交换文件
    - sudo fallocate -l 1024M /var/swap_temp
  - 制作交换空间
    - sudo mkswap /var/swap_temp
  - 启用交换空间
    - sudo swapon /var/swap_temp
  - 移除交换空间
    - sudo swapoff -v /var/swap_temp
  - 删除交换文件
    - rm -f /var/swap_temp

####### 从源码构建安装 OpenCV

- make

```
~/opencv/opencv3.4.0/build $ make
```

```
~/opencv/opencv3.4.0/build $ time make -j4
```

```
[ 99%] Building CXX object modules/optflow/CMakeFiles/opencv_optflow.dir/src/pcaflow.cpp.o
[ 99%] Building CXX object modules/optflow/CMakeFiles/opencv_optflow.dir/src/simpleflow.cpp.o
[100%] Building CXX object modules/optflow/CMakeFiles/opencv_optflow.dir/src/sparse_matching_gpc.cpp.o
[100%] Building CXX object modules/optflow/CMakeFiles/opencv_optflow.dir/src/sparsetodenseflow.cpp.o
[100%] Building CXX object modules/optflow/CMakeFiles/opencv_optflow.dir/src/variational_refinement.cpp.o
[100%] Building CXX object modules/optflow/CMakeFiles/opencv_optflow.dir/opencl_kernels_optflow.cpp.o
[100%] Linking CXX shared library ../../lib/libopencv_optflow.so
[100%] Built target opencv_optflow
[100%] Generating pyopencv_generated_include.h, pyopencv_generated_funcs.h, pyopencv_generated_types.h, pyopencv_generated_type_reg.h, pyopencv_generated_ns_reg.h
[100%] Generating pyopencv_generated_include.h, pyopencv_generated_funcs.h, pyopencv_generated_types.h, pyopencv_generated_type_reg.h, pyopencv_generated_ns_reg.h
Scanning dependencies of target opencv_python3
Scanning dependencies of target opencv_python2
[100%] Building CXX object modules/python2/CMakeFiles/opencv_python2.dir/__/src2/cv2.cpp.o
[100%] Building CXX object modules/python3/CMakeFiles/opencv_python3.dir/__/src2/cv2.cpp.o
[100%] Linking CXX shared module ../../lib/python3/cv2.cpython-34m.so
[100%] Linking CXX shared module ../../lib/cv2.so
[100%] Built target opencv_python2
[100%] Built target opencv_python3

real    45m16.204s
user    152m25.580s
sys     3m26.660s
```

####### 从源码构建安装 OpenCV

- htop

![](img/9de0cb7cd12c1232044968590f57e471_66_0.png)

- make install

```
bash
~/opencv/opencv3.4.0/build $ sudo make install
```

- 检查版本

```
bash
~ $ pkg-config -modversion opencv
```

![](img/9de0cb7cd12c1232044968590f57e471_66_1.png)

####### 从源码构建安装 OpenCV

- 检查 Python 2, 3 的 cv2 模块版本

```
import cv2
cv2.  version
```

![](img/9de0cb7cd12c1232044968590f57e471_67_0.png)

####### 使用预构建的 Debian 包安装 OpenCV

- 由本课程非官方提供
- 从 github 下载 deb 包文件
  - https://github.com/dltpdn/opencv-for-rpi
    - opencv-main
    - opencv-libs
    - opencv-dev
    - opencv-python

![](img/9de0cb7cd12c1232044968590f57e471_67_1.png)

####### 使用预构建的 Debian 包安装 OpenCV

- git 下载

```
~ $ git clone https://github.com/dltpdn/opencv-for-rpi.git
~ $ cd ./opencv-for-rpi/stretch/3.4.0
```

- 目录

- <debian_version> / <opencv_version>

![](img/9de0cb7cd12c1232044968590f57e471_68_0.png)

- 安装

```
~/opencv-for-rpi/stretch/3.4.0 $ sudo apt -y install ./OpenCV*.dev
```

![](img/9de0cb7cd12c1232044968590f57e471_68_1.png)

####### 使用预构建的 Debian 包安装 OpenCV

- 检查结果

```
~ $ pkg-config -modversion opencv
```

![](img/9de0cb7cd12c1232044968590f57e471_69_0.png)

###### 安装 Python Matplotlib

- pip 不支持树莓派的 matplotlib
- 从 APT 仓库安装

```
sudo apt-get install python-matplotlib
sudo apt-get install python3-matplotlib

sudo apt-get install python-gi-cairo
sudo apt-get install python3-gi-cairo

sudo apt-get install python-cairocffi
sudo apt-get install python3-cairocffi
```

- 在 Python 中检查 Matplotlib
  - import matplotlib
  - matplotlib.__version__

![](img/9de0cb7cd12c1232044968590f57e471_69_1.png)

###### ❖ 测试代码

```
import cv2

img_file = "img/model.jpg"
img = cv2.imread(img_file)

if img:
    cv2.imshow('model', img)
    cv2.waitKey(0)
    cv2.destroyAllWindows()
```

![](img/9de0cb7cd12c1232044968590f57e471_70_0.png)

### 基本接口

1. **图像与视频 I/O**
2. 绘图
3. 窗口管理
4. 事件处理

#### ❖ 图像显示

- img = cv2.imread(file_name [,mod_flag]): 从文件读取图像
  - file_name: 图像路径，字符串
  - mode_flag: 定义读取模式标志
    - cv2.IMREAD_UNCHANGED: 颜色 (BGR) 缩放，基础值 -1
    - cv2.IMREAD_GRAYSCALE: 灰度，0
    - cv2.IMREAD_COLOR: 按文件保存的缩放，1
  - img: 读取的图像，NumPy ndarray
- cv2.imshow(title, img): 显示图像
  - title : 窗口名称，字符串
  - img : 要显示的图像，NumPy ndarray
- key = cv2.waitKey( [delay]): 键盘输入等待
  - delay : 等待时间，毫秒
  - key : 输入的键盘值，-1: 超时
- cv2.destroyAllWindows() : 关闭所有打开的窗口

#### ❖ 图像显示

```
import cv2

img_file = "../img/model.jpg"
img = cv2.imread(img_file)

if img:
    cv2.imshow('model', img)
    cv2.waitKey(0)
    cv2.destroyAllWindows()
```

![](img/9de0cb7cd12c1232044968590f57e471_72_0.png)

#### 以灰度读取

```
import cv2

img_file = "../img/model.jpg"
img = cv2.imread(img_file,
    cv2.IMREAD_GRAYSCALE)

if not img is None:
    cv2.imshow(img_file, img)
    cv2.waitKey(0)
    cv2.destroyAllWindows()
else:
    print("no file:", img_file)
```

![](img/9de0cb7cd12c1232044968590f57e471_73_0.png)

#### 图像写入

- cv2.imwrite(file_name, img)
  - file_name: 要保存的文件名，字符串
  - img: 要保存的图像 (NumPy 数组)

```
import cv2

img_file = "../img/model.jpg"
img = cv2.imread(img_file, cv2.IMREAD_GRAYSCALE)
cv2.imshow("my darling daughter", img)

key = cv2.waitKey(0) & 0xFF
if key == 27: #esc
    cv2.destroyAllWindows()
elif key == ord('s'):
    print('saving file.');
    cv2.imwrite("./gray.jpg", img)
    cv2.destroyAllWindows()
```

#### ❖ 视频捕获

- cap = cv2.VideoCapture(file_path or index): 视频捕获对象生成器
  - file_path: 图像文件路径，字符串
  - index: 摄像头设备号，整数，从 0 开始递增（即：0,1,2,...）
- ret = cap.isOpened(): 检查连接初始化
  - ret : 检查是否成功，布尔值
- cap.set()/get() : 属性更改和确认
  - cv2.CAP_PROP_FRAME_WIDTH(3), cv2.CAP_PROP_FRAME_HEIGHT(4)
  - cv2.CAP_PROP_FPS: 每秒帧数
  - cv2.CAP_PROP_POS_MSEC: 视频文件的帧位置（毫秒）
  - cv2.CAP_PROP_POS_AVI_RATIO: 视频文件的位置（0-开始，1-结束）
  - cv2.CAP_PROP_FOURCC: 视频文件编解码器字符串
  - cv2.CAP_PROP_AUTOFOCUS: 摄像头自动对焦
  - cv2.CAP_PROP_ZOOM: 摄像头变焦
- ret, img = cap.read(): 读取下一帧
  - ret : 检查是否成功，布尔值
  - img: 图像帧数据（numpy 数组）

#### ❖ 视频文件播放

```
import cv2
video_file = "../img/big_buck.avi"
cap = cv2.VideoCapture(video_file) #0 or -1
while cap.isOpened():
    ret, img = cap.read()
    if ret:
        cv2.imshow(video_file, img)
        if cv2.waitKey(1) & 0xFF == 27:
            break
    else:
        print('no more frame.')
        break
else:
    print('can't open video.')
cap.release()
cv2.destroyAllWindows()
```

![](img/9de0cb7cd12c1232044968590f57e471_74_0.png)

#### ❖ 带 FPS 的视频文件播放

- delay = 1000/FPS

```
import cv2

video_file = "../img/big_buck.avi"
cap = cv2.VideoCapture(video_file) #0 or -1
fps = cap.get(cv2.CAP_PROP_FPS)
delay = int(1000/fps)
print("FPS: %f, Delay: %dms" %(fps, delay))

while cap.isOpened():
    ret, img = cap.read()
    if ret:
        cv2.imshow(video_file, img)
        if cv2.waitKey(delay) & 0xFF == 27:
            break
    else:
        print('no more frame.')
        break
else:
    print('can't open video.')
cap.release()
cv2.destroyAllWindows()
```

![](img/9de0cb7cd12c1232044968590f57e471_75_0.png)

#### ❖ 摄像头帧

```python
import cv2
cap = cv2.VideoCapture(0) #0 or -1
while cap.isOpened():
    ret, img = cap.read()
    if ret:
        cv2.imshow('camera-0', img)
        if cv2.waitKey(1) & 0xFF == 27: #esc
            break
    else:
        print('no camera!')
        break
cap.release()
cv2.destroyAllWindows()
```

![](img/9de0cb7cd12c1232044968590f57e471_76_0.png)

#### ❖ 摄像头帧尺寸调整

```python
import cv2

cap = cv2.VideoCapture(0) #0 or -1
print("width: %d, height:%d" % (cap.get(3),
cap.get(4)) )
cap.set(cv2.CAP_PROP_FRAME_WIDTH, 320)
cap.set(cv2.CAP_PROP_FRAME_HEIGHT, 240)
print("resized width: %d, height:%d" % (cap.get(3),
cap.get(4)) )
while cap.isOpened():
    ret, img = cap.read()
    if ret:
        cv2.imshow('camera-0', img)
        if cv2.waitKey(1) & 0xFF == 27: #esc
            break
    else:
        print('no camera!')
        break
cap.release()
cv2.destroyAllWindows()
```

![](img/9de0cb7cd12c1232044968590f57e471_76_1.png)

#### ❖ 摄像头帧翻转

- cv2.flip(img, flipCode)
  - 翻转图像
  - flipcode
    - 0 : x轴（行）
    - 1 : y轴（列）
    - -1 : x,y轴（行，列）

$$dst_{ij} = \begin{cases} src_{src.rows-i-1,j} & \text{if flipCode = 0} \\ src_{i,src.cols-j-1} & \text{if flipCode > 0} \\ src_{src.rows-i-1,src.cols-j-1} & \text{if flipCode < 0} \end{cases}$$

```python
import cv2
cap = cv2.VideoCapture(0)
while True:
    ret, frame = cap.read()
    if ret==True:
        frame = cv2.flip(frame,1)
        cv2.imshow('frame',frame)
        if cv2.waitKey(1) & 0xFF == 27: #esc
            break
    else:
        break
cap.release()
cv2.destroyAllWindows()
```

![](img/9de0cb7cd12c1232044968590f57e471_77_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_77_1.png)

#### 保存摄像头帧

```python
import cv2

cap = cv2.VideoCapture(0)                    # Connect camera #0
if cap.isOpened() :
    while True:
        ret, frame = cap.read()               # Read camera frame
        if ret:
            cv2.imshow('camera',frame)        # Display frame display
            if cv2.waitKey(1) != -1 :         # Select any key to
                cv2.imwrite('photo.jpg', frame) # Save frame as 'photo.jpg'
                break
        else:
            print('no frame!')
            break
else:
    print('no camera!')
cap.release()
cv2.destroyAllWindows()
```

#### 使用摄像头录制视频

- writer = cv2.VideoWriter(file_path, fourcc, fps, (width, height) )
  - file_path : 保存路径
  - fourcc : 保存格式，4个字母
    - cv2.VideoWriter_fourcc(*'ABCD')
      - ABCD : 'Mjpg' , 'DIVX'
    - ord('D') + (ord('I') <<8) + (ord('V') <<16) + (ord('X') <<24)
  - fps : 帧率，浮点数
  - (width, height) : 帧宽度，高度，元组
- writer.write(frame)
  - frame : numpy数组
- writer.set()/get()
  - 属性更改和检查
  - cv2.CAP_PROP_*
    - FRAME_WIDTH(3),
    - FRAME_HEIGHT(4)

```python
import cv2

cap = cv2.VideoCapture(0)    # Connect #0 camera
file_path = './record.avi'
fps = 40                     # FPS, Frame per sec
fourcc = cv2.VideoWriter_fourcc(*'DIVX') # Encoding format letter
size = (int(cap.get(cv2.CAP_PROP_FRAME_WIDTH)), \
        int(cap.get(cv2.CAP_PROP_FRAME_HEIGHT)))        # Frame Size
out = cv2.VideoWriter(file_path, fourcc, fps, size) # Generate VideoWriter Objects
while True:
    ret, frame = cap.read()
    if ret:
        cv2.imshow('camera-recording',frame)
        out.write(frame)                    # Save File
        if cv2.waitKey(1) != -1:    break
    else: break
else:
    print("can't open camera!")
out.release()                                # Close File
cap.release()
cv2.destroyAllWindows()
```

### 基础接口

1. 图像与视频输入/输出
2. **绘图**
3. 窗口管理
4. 事件处理

#### ❖ 线段

- cv2.line(img, start, end, color [, thickness, lineType]): 绘制直线
  - img : 要绘制的图像，Numpy数组
  - start : 起点坐标，元组 (x,y)
  - end : 终点坐标，元组 (x, y)
  - color : 线条颜色，元组 (Blue, Green, Red)，0~255
  - thickness : 线条粗细
  - lineType : 线条绘制类型，
    - cv2.LINE_4 : 4-连通线算法
    - cv2.LINE_8 : 8-连通线算法
    - cv2.LINE_AA : 抗锯齿（无阶梯效应）

```python
import cv2

img = cv2.imread('./img/blank_500.jpg')

cv2.line(img, (50, 50), (150, 50), (255,0,0)) # Blue 1 pixel line
cv2.line(img, (200, 50), (300, 50), (0,255,0)) # Green 1 pixel line
cv2.line(img, (350, 50), (450, 50), (0,0,255)) # Red 1 pixel line
cv2.line(img, (100, 100), (400, 100), (255,255,0), 10)
cv2.line(img, (100, 150), (400, 150), (255,0,255), 10)
cv2.line(img, (100, 200), (400, 200), (0,255,255), 10)
cv2.line(img, (100, 250), (400, 250), (200,200,200), 10)
cv2.line(img, (100, 300), (400, 300), (0,0,0), 10)
cv2.line(img, (100, 350), (400, 350), (0,0,255), 20, cv2.LINE_4 )
cv2.line(img, (100, 400), (400, 400), (0,0,255), 20, cv2.LINE_8)
cv2.line(img, (100, 450), (400, 450), (0,0,255), 20, cv2.LINE_AA)
cv2.line(img, (0,0), (500,500), (0,0,255))   # Diagonal line across the image

cv2.imshow('lines', img)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

#### ❖ 线段

![](img/9de0cb7cd12c1232044968590f57e471_82_0.png)

#### ❖ 矩形

- cv2.rectangle(img, start, end, color[, thickness, lineType]): 绘制四边形
  - img: 要绘制的图像
  - start: 四边形起始顶点，元组 (x, y)
  - end: 四边形结束顶点，元组 (x, y)
  - color: 颜色，元组 (Blue, Green, Red)
  - thickness: 线条粗细，-1: 填充
  - lineType: 线条类型，
    - cv2.LINE_4
    - cv2.LINE_8
    - cv2.LINE_AA

#### ❖ 矩形

```python
import cv2

img = cv2.imread('./img/blank_500.jpg')

cv2.rectangle(img, (50, 50), (150, 150), (255,0,0) )
cv2.rectangle(img, (300, 300), (100, 100), (0,255,0), 10 )
cv2.rectangle(img, (450, 200), (200, 450), (0,0,255), -1 )

cv2.imshow('rectangle', img)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

![](img/9de0cb7cd12c1232044968590f57e471_83_0.png)

#### 多边形

- cv2.polylines(img, points, isClosed, color[, thickness, lineType]): 绘制多边形
  - img: 要绘制的图像
  - points: 顶点坐标，Numpy数组的列表
  - isClosed: 图形是否闭合，布尔值
  - color: 颜色，元组 (Blue, Green, Red)
  - thickness: 线条粗细
  - lineType : 线条类型
    - cv2.LINE_4
    - cv2.LINE_8
    - cv2.LINE_AA

```python
import cv2
import numpy as np

img = cv2.imread('./img/blank_500.jpg')

pts1 = np.array([[50,50], [150,150], [100,140],[200,240]],
                dtype=np.int32)
pts2 = np.array([[350,50], [250,200], [450,200]], dtype=np.int32)
pts3 = np.array([[150,300], [50,450], [250,450]], dtype=np.int32)
pts4 = np.array([[350,250], [450,350], [400,450],[300,450],
                [250,350]], \
                dtype=np.int32)

cv2.polylines(img, [pts1], False, (255,0,0))
cv2.polylines(img, [pts2], False, (0,0,0), 10)
cv2.polylines(img, [pts3], True, (0,0,255), 10)
cv2.polylines(img, [pts4], True, (0,0,0))

cv2.imshow('polyline', img)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

#### 多边形

![](img/9de0cb7cd12c1232044968590f57e471_85_0.png)

#### 圆形、椭圆

- cv2.circle(img, center, radius, color [, thickness, lineType]): 圆形绘制函数
  - img : 要绘制的图像
  - center: 原点，元组 (x, y)
  - radius
  - color : 元组 (Blue, Green, Red)
  - thickness : 线条粗细，-1: 填充
  - lineType : 线条类型，cv2.LINE_4, cv2.LINE_8, cv2.LINE_AA

- cv2.ellipse(img, center, axes, angle, from, to, color[, thickness, lineType])
  - img: 要绘制的图像
  - center : 原点，元组 (x, y)
  - axes: 基轴长度
  - angle: 基轴角度
  - from, to: 线条起始和结束点的角度
  - color: 元组 (Blue, Green, Red)
  - thickness : 线条粗细，-1: 填充
  - lineType: 线条类型，cv2.LINE_4, cv2.LINE_8, cv2.LINE_AA

#### ❖ 椭圆示例

- cv2.ellipse(img, center, axes, angle, from, to, color[, thickness, lineType])

```python
ellipse(img, (256,256), (50,100), 45, 180,360, 1)
```

![](img/9de0cb7cd12c1232044968590f57e471_86_0.png)

#### 圆与椭圆

```python
import cv2
img = cv2.imread('./img/blank_500.jpg')

cv2.circle(img, (150, 150), 100, (255,0,0))
cv2.circle(img, (300, 150), 70, (0,255,0), 5)
cv2.circle(img, (400, 150), 50, (0,0,255), -1)

cv2.ellipse(img, (50, 300), (50, 50), 0, 0, 360, (0,0,255))
cv2.ellipse(img, (150, 300), (50, 50), 0, 0, 180, (255,0,0))
cv2.ellipse(img, (200, 300), (50, 50), 0, 181, 360, (0,0,255))
cv2.ellipse(img, (325, 300), (75, 50), 0, 0, 360, (0,255,0))
cv2.ellipse(img, (450, 300), (50, 75), 0, 0, 360, (255,0,255))
cv2.ellipse(img, (50, 425), (50, 75), 15, 0, 360, (0,0,0))
cv2.ellipse(img, (200, 425), (50, 75), 45, 0, 360, (0,0,0))
cv2.ellipse(img, (350, 425), (50, 75), 45, 0, 180, (0,0,255))
cv2.ellipse(img, (400, 425), (50, 75), 45, 181, 360, (255,0,0))

cv2.imshow('circle', img)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

![](img/9de0cb7cd12c1232044968590f57e471_87_0.png)

#### ❖ 文本

- `cv2.putText(img, text, point, fontFace, fontSize, color [, thickness, lineType])`
  - img: 要显示字母的图像
  - text: 要显示的字符串
  - point: 显示字母的坐标（左下角基准点），元组 (x,y)
  - fontFace: 字体
    - cv2.FONT_HERSHEY_PLAIN: 无衬线小字体
    - cv2.FONT_HERSHEY_SIMPLEX: 无衬线常规字体
    - cv2.FONT_HERSHEY_DUPLEX: 无衬线粗体
    - cv2.FONT_HERSHEY_COMPLEX_SMALL: 衬线小字体
    - cv2.FONT_HERSHEY_COMPLEX: 衬线常规字体
    - cv2.FONT_HERSHEY_TRIPLEX: 衬线粗体
    - cv2.FONT_HERSHEY_SCRIPT_SIMPLEX: 手写无衬线字体
    - cv2.FONT_HERSHEY_SCRIPT_COMPLEX: 手写无衬线字体
    - cv2.FONT_ITALIC: 斜体标志
  - fontSize: 字体大小
  - color: 元组 (蓝, 绿, 红)
  - thickness: 线条粗细，-1: 填充
  - lineType: 线条类型，cv2.LINE_4, cv2.LINE_8, cv2.LINE_AA

#### ❖ 文本 (1/2)

```python
import cv2

img = cv2.imread('./img/blank_500.jpg')

cv2.putText(img, "Plain", (50, 30), cv2.FONT_HERSHEY_PLAIN, 1, (0, 0,0))
cv2.putText(img, "Simplex", (50, 70), cv2.FONT_HERSHEY_SIMPLEX, 1, (0, 0,0))
cv2.putText(img, "Duplex", (50, 110), cv2.FONT_HERSHEY_DUPLEX, 1, (0, 0,0))
cv2.putText(img, "Simplex", (200, 110), cv2.FONT_HERSHEY_SIMPLEX, 2,
            (0,0,250))
cv2.putText(img, "Complex Small", (50, 180), cv2.FONT_HERSHEY_COMPLEX_SMALL,
            1, (0, 0,0))
```

#### ❖ 文本 (2/2)

```python
cv2.putText(img, "Complex", (50, 220), cv2.FONT_HERSHEY_COMPLEX, 1, (0, 0,0))
cv2.putText(img, "Triplex", (50, 260), cv2.FONT_HERSHEY_TRIPLEX, 1, (0, 0,0))
cv2.putText(img, "Complex", (200, 260), cv2.FONT_HERSHEY_TRIPLEX, 2,
            (0,0,255))

cv2.putText(img, "Script Simplex", (50, 330),
            cv2.FONT_HERSHEY_SCRIPT_SIMPLEX, 1, (0, 0,0))
cv2.putText(img, "Script Complex", (50, 370),
            cv2.FONT_HERSHEY_SCRIPT_COMPLEX, 1, (0, 0,0))

cv2.putText(img, "Plain Italic", (50, 430), cv2.FONT_HERSHEY_PLAIN |
            cv2.FONT_ITALIC, 1, (0, 0,0))
cv2.putText(img, "Complex Italic", (50, 470), cv2.FONT_HERSHEY_COMPLEX |
            cv2.FONT_ITALIC, 1, (0, 0,0))

cv2.imshow('draw text', img)
cv2.waitKey()
cv2.destroyAllWindows()
```

#### ❖ 文本

![](img/9de0cb7cd12c1232044968590f57e471_89_0.png)

### 基础接口

1. 图像与视频 I/O
2. 绘图
3. **窗口管理**
4. 事件处理

#### ❖ 窗口管理

- `cv2.namedWindow(title [, option])`: 打开一个带标题的窗口
  - title: 窗口名称，显示在标题栏
  - option: 窗口选项，以 "cv2.WINDOW_" 开头
    - cv2.WINDOW_NORMAL: 临时尺寸，可以调整窗口大小
    - cv2.WINDOW_AUTOSIZE: 尺寸与窗口相同，无法重新调整窗口大小
- `cv2.moveWindow(title, x, y)`: 移动窗口位置
  - title: 要更改位置的窗口标题
  - x, y: 要移动到的窗口位置
- `cv2.resizeWindow(title, width, height)`: 更改窗口大小
  - title: 要更改大小的窗口名称
  - width, height: 要更改的窗口宽度和高度
- `cv2.destroyWindow(title)`: 关闭窗口
  - title: 要关闭的窗口标题
- `cv2.destroyAllWindows()`: 关闭所有打开的窗口

```python
import cv2

file_path = './img/girl.jpg'
img = cv2.imread(file_path)
img_gray = cv2.imread(file_path, cv2.IMREAD_GRAYSCALE)
cv2.namedWindow('origin', cv2.WINDOW_AUTOSIZE)
cv2.namedWindow('gray', cv2.WINDOW_NORMAL)

cv2.imshow('origin', img)
cv2.imshow('gray', img_gray)
cv2.moveWindow('origin', 0, 0)
cv2.moveWindow('gray', 100, 100)
cv2.waitKey(0)
cv2.resizeWindow('origin', 200, 200)
cv2.resizeWindow('gray', 100, 100)
cv2.waitKey(0)
cv2.destroyWindow("gray")
cv2.waitKey(0)
cv2.destroyAllWindows()
```

#### ❖ 窗口管理

![](img/9de0cb7cd12c1232044968590f57e471_92_0.png)

### 基础接口

1. 图像与视频 I/O
2. 绘图
3. 窗口管理
4. **事件处理**

#### 键盘事件

- `val = cv2.waitKey([ delay ])`: 键盘输入待机，仅移动活动窗口
  - delay=0: 待机时间，毫秒
    - delay <= 0: 无限等待
  - val: 按下键的代码
    - -1: 超时
- 比较按键代码的字符串，使用 Python 内置函数
  - `ord(c)`: 将字符 c 转换为其映射的代码
    - 例如: `ord('a')` #97
  - `chr(code)`: 将代码转换为其映射的字符串
    - 例如: `chr(97)` # 'a'
  - `cv2.waitKey() == ord('a')`
- ASCII(8位)代码溢出
  - 在某些64位环境中会转换为32位整数，需要初始化超过8位的位掩码
  - `key = cv2.waitKey(0) & 0xFF`

```python
import cv2
img_file = "./img/girl.jpg"
img = cv2.imread(img_file)
title = 'IMG'                # 窗口标题
x, y = 100, 100              # 初始坐标
while True:
    cv2.imshow(title, img)
    cv2.moveWindow(title, x, y)
    key = cv2.waitKey(0) & 0xFF # 无限等待键盘输入，8位掩码有效
    print(key, chr(key))        # 键盘输入值，打印字符串值
    if key == ord('h'):         # 按 'h' 向左移动
        x -= 10
    elif key == ord('j'):       # 按 'j' 向下移动
        y += 10
    elif key == ord('k'):       # 按 'k' 向上移动
        y -= 10
    elif key == ord('l'):       # 按 'l' 向右移动
        x += 10
    elif key == ord('q') or key == 27: # 按 'q' 或 'esc' 结束
        break
    cv2.destroyAllWindows()
    cv2.moveWindow(title, x, y)
```

#### 鼠标事件

- `cv2.setMouseCallback(win_name, onMouse [, param])`: 注册 onMouse 函数
  - win_name: 要注册事件的窗口标题
  - onMouse: 为事件预先声明的回调函数对象
  - param: 根据需要传递给 onMouse 函数的参数
- `MouseCallback(event, x, y, flags, param)`: 回调函数声明
  - event: 鼠标事件类型，所有12种以 cv2.EVENT_ 开头的整数
  - x, y: 鼠标坐标
  - flags: 鼠标移动时发生的额外状态，所有6种以 cv2.EVENT_FLAG_ 开头的整数
  - param: 从 cv2.setMouseCallback() 函数传递的参数

```python
def onMouse(event, x, y, flags, param):
    # 根据鼠标事件进行的草稿工作。
    pass

cv2.setMouseCallback('title', onMouse)
```

#### 鼠标事件与标志类型

- `cv2.EVENT_*`

```python
import cv2

events = [i for i in dir(cv2) if 'EVENT_' in i]
print( len(events) ,"events" )
print( events )
```

```
18 events
['EVENT_FLAG_ALTKEY', 'EVENT_FLAG_CTRLKEY', 'EVENT_FLAG_LBUTTON',
'EVENT_FLAG_MBUTTON', 'EVENT_FLAG_RBUTTON', 'EVENT_FLAG_SHIFTKEY',
'EVENT_LBUTTONDBLCLK', 'EVENT_LBUTTONDOWN', 'EVENT_LBUTTONUP',
'EVENT_MBUTTONDBLCLK', 'EVENT_MBUTTONDOWN', 'EVENT_MBUTTONUP',
'EVENT_MOUSEHWHEEL', 'EVENT_MOUSEMOVE', 'EVENT_MOUSEWHEEL',
'EVENT_RBUTTONDBLCLK', 'EVENT_RBUTTONDOWN', 'EVENT_RBUTTONUP']
```

#### 鼠标事件类型

- cv2.EVENT_MOUSEMOVE: 移动鼠标
- cv2.EVENT_LBUTTONDOWN: 按下左键
- cv2.EVENT_RBUTTONDOWN: 按下右键
- cv2.EVENT_MBUTTONDOWN: 按下中键
- cv2.EVENT_LBUTTONUP: 释放左键
- cv2.EVENT_RBUTTONUP: 释放右键
- cv2.EVENT_MBUTTONUP: 释放中键
- cv2.EVENT_LBUTTONDBLCLK: 左键双击
- cv2.EVENT_RBUTTONDBLCLK: 右键双击
- cv2.EVENT_MBUTTONDBLCLK: 中键双击
- cv2.EVENT_MOUSEWHEEL: 滚轮
- cv2.EVENT_MOUSEHWHEEL: 水平滚动滚轮

#### 鼠标事件标志类型

- cv2.EVENT_FLAG_LBUTTON (1): 按下左键
- cv2.EVENT_FLAG_RBUTTON (2): 按下右键
- cv2.EVENT_FLAG_MBUTTON (4): 按下中键
- cv2.EVENT_FLAG_CTRLKEY (8): 按下 Ctrl
- cv2.EVENT_FLAG_SHIFTKEY (16): 按下 Shift
- cv2.EVENT_FLAG_ALTKEY (32): 按下 Alt

#### 鼠标事件

```python
import cv2

title = 'mouse event'
img = cv2.imread('./img/blank_500.jpg')
cv2.imshow(title, img)

def onMouse(event, x, y, flags, param):
    print(event, x, y, )
    if event == cv2.EVENT_LBUTTONDOWN:
        cv2.circle(img, (x,y), 30, (0,0,0), -1)
        cv2.imshow(title, img)
cv2.setMouseCallback(title, onMouse)
while True:
    if cv2.waitKey(0) & 0xFF == 27:    # End with esc
        break
cv2.destroyAllWindows()
```

![](img/9de0cb7cd12c1232044968590f57e471_97_0.png)

#### 鼠标事件标志示例

```python
import cv2
title = 'mouse event'
img = cv2.imread('./img/blank_500.jpg')
cv2.imshow(title, img)
colors = {'black':(0,0,0),'red': (0,0,255),'blue':(255,0,0), 'green': (0,255,0) }

def onMouse(event, x, y, flags, param):
    print(event, x, y, flags)
    color = colors['black']
    if event == cv2.EVENT_LBUTTONDOWN:
        if flags & cv2.EVENT_FLAG_CTRLKEY and flags & cv2.EVENT_FLAG_SHIFTKEY:
            color = colors['green']
        elif flags & cv2.EVENT_FLAG_SHIFTKEY :
            color = colors['blue']
        elif flags & cv2.EVENT_FLAG_CTRLKEY :
            color = colors['red']
        cv2.circle(img, (x,y), 30, color, -1)
        cv2.imshow(title, img)
cv2.setMouseCallback(title, onMouse)
while True:
    if cv2.waitKey(0) & 0xFF == 27:
        break
cv2.destroyAllWindows()
```

![](img/9de0cb7cd12c1232044968590f57e471_97_1.png)

#### 鼠标事件

- 使用鼠标和按键事件绘图 (1/3)

```python
import cv2
import numpy as np

drawing = False # true if mouse is pressed
color = (0,0,255) # red as default
ix,iy = -1,-1

def draw_circle(event,x,y,flags,param):
    global ix,iy,drawing,mode
    if event == cv2.EVENT_LBUTTONDOWN :
        drawing = True
        ix,iy = x,y
        if event == cv2.EVENT_LBUTTONDOWN:
            mode = True
        else:
            mode = False
    elif event == cv2.EVENT_MOUSEMOVE:
        if drawing:
            if flags == cv2.EVENT_FLAG_LBUTTON:
                cv2.circle(img, (x,y), 5, color, -1)
            elif flags == (cv2.EVENT_FLAG_SHIFTKEY + cv2.EVENT_FLAG_LBUTTON):
                cv2.circle(img,(x,y),5, (0,0,0),-1)
    elif event == cv2.EVENT_LBUTTONUP or event == cv2.EVENT_RBUTTONUP:
        drawing = False
img = np.zeros((512,512,3), np.uint8)
cv2.namedWindow('paint')
cv2.setMouseCallback('paint',draw_circle)

while(1):
    cv2.imshow('paint',img)
    k = cv2.waitKey(1) & 0xFF
    if k == ord('r'):
        color = (0,0,255)
    elif k == ord('g'):
        color = (0,255,0)
    elif k == ord('b'):
        color = (255,0,0)
    elif k == 27:
        break
cv2.destroyAllWindows()
```

#### 鼠标事件

- 使用鼠标和按键事件绘图

![](img/9de0cb7cd12c1232044968590f57e471_99_0.png)

#### 滑动条

- cv2.createTrackbar(name, win_name, value, count, onChange): 创建滑动条
  - name: 滑动条名称
  - win_name: 显示滑动条的窗口名称
  - value: 滑动条首次出现时的初始值，范围在 0 ~ count 之间
  - count: 滑动条的刻度数量，即滑动条的最大值
  - onChange : TrackbarCallback，滑动条事件处理函数
- TrackbarCallback(value): 滑动条事件回调函数
  - value: 滑动条移动到的位置值
- pos = cv2.getTrackbarPos(name, win_name)
  - name: 要查询的滑动条名称
  - win_name: 包含滑动条的窗口名称
  - pos: 滑动条的位置值

#### ❖ 滑动条

- 使用滑动条调整颜色

```python
import cv2 , numpy as np

win_name = 'Trackbar'
img = cv2.imread('./img/blank_500.jpg')
cv2.imshow(win_name,img)

def onChange(x):
    r = cv2.getTrackbarPos('R',win_name)
    g = cv2.getTrackbarPos('G',win_name)
    b = cv2.getTrackbarPos('B',win_name)
    print(x, r, g, b)
    img[:] = [b,g,r]
    cv2.imshow(win_name, img)
cv2.createTrackbar('R', win_name, 255, 255, onChange)
cv2.createTrackbar('G', win_name, 255, 255, onChange)
cv2.createTrackbar('B', win_name, 255, 255, onChange)
while True:
    if cv2.waitKey(1) & 0xFF == 27:
        break
cv2.destroyAllWindows()
```

![](img/9de0cb7cd12c1232044968590f57e471_100_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_100_1.png)

### Numpy 和 Matplotlib

1. **Numpy**
2. Matplotlib

#### ❖ Numpy

- http://www.numpy.org/
- https://docs.scipy.org/doc/numpy/genindex.html
- Python 科学计算的基础包
  - 强大的 N 维数组对象
  - 精确的广播功能
  - C/C++、Fortran 集成工具
  - 实用的线性代数、傅里叶变换、随机数生成函数
- OpenCV-Python 版本 2.3+
  - 之前使用其内部数据结构
  - 现在使用 NumPy 作为图像数据结构
  - 更改为 cv2 包
  - import cv2
  - import numpy as np

![](img/9de0cb7cd12c1232044968590f57e471_102_0.png)

#### ❖ NumPy 与图像

- ndarray
  - ndim: n 维（轴）
  - shape: 每个维度的大小，元组
  - size : 所有元素的数量，shape 各维度的乘积
  - dtype : 元素的数据类型
  - itemsize : 每个元素的字节大小
  - data : 包含元素的缓冲区，实际中不常用

![](img/9de0cb7cd12c1232044968590f57e471_102_1.png)

```python
import numpy as np
import cv2
img = cv2.imread('../img/blank_500.jpg')
type(img) # <class 'numpy.ndarray'>
print(img.ndim)  #3
print(img.shape) #(500,500,3,)
print(img.size) #750000
print(img.dtype) # 'uint8'
print(img.itemsize) # 1, Size of each item
```

#### ❖ Numpy

- 生成
  - 作为值生成
    - array()
  - 作为初始值生成
    - empty(), zeros(), ones(), full()
  - 作为现有数组生成
    - empty_like(), zeros_like(), ones_like(), full_like()
  - 作为连续值生成
    - arange()
  - 作为随机数生成
    - random.rand(), random.randn()
- 作为值生成
  - numpy.array(list [, dtype]) : 使用指定值生成 NumPy 数组
    - list: 用于生成数组的 Python 列表对象
    - dtype: 数据类型，如果省略，则根据值自动决定
      - int8, int16, int32, int64 : 有符号整数
      - uint8, uint16, uint32, uint64 : 无符号整数
      - float16, float32, float64, float128 : 浮点实数
      - complex64, complex128, complex256 : 浮点复数
      - bool : 布尔值
- 作为值生成

```python
import numpy as np
a = np.array([1,2,3,4])
b = np.array([[1,2,3,4],
              [5,6,7,8]])
c = np.array([1,2,3.14, 4])
d = np.array([1,2,3,4], dtype=np.float64)

print(a, a.dtype, a.shape) # [1 2 3 4] int64 (4,)
print(b, b.dtype, b.shape) # [[1 2 3 4] [5 6 7 8]] int64 (2, 4)
print(c, c.dtype, c.shape) # [ 1.  2.  3.14  4.] float64 (4,)
print(d, d.dtype, d.shape) # [ 1.  2.  3.  4.] float64 (4,)
```

#### ❖ Numpy

- 作为大小和初始值生成
  - np.empty ( tuple [, dtype])
    - 所有元素未初始化，元组大小
    - np.empty( (2,3))
    - np.empty( (2,3), dtype=np.int16)
  - np.zeros( tuple [, dtype])
    - 所有元素为 0，元组大小
    - np.zeros( (3,4))
    - np.zeros( (2,3,4) , dtype=np.int16)
  - np.ones ( tuple [, dtype])
    - 所有元素为 1，元组大小
    - np.ones( (3,4) )
    - np.ones( (2,3,4) , dtype=np.int16)
  - np.full(shape, fill_value [, dtype])
    - 生成用 fill_value 初始化的数组
- 作为大小生成

```python
import numpy as np

a = np.zeros( (2,3))
b = np.zeros( (2,3,4), dtype=np.uint8)

c = np.ones( (2,3), dtype=np.float32)
d = np.empty( (2,3))  #not initialized, garbage value
e = np.empty( (2,3), dtype=np.int16)
f = np.full(2,3, 255)

print(a, a.dtype, a.shape)
print(b, b.dtype, b.shape)
print(c, c.dtype, c.shape)
print(d, d.dtype, d.shape)
print(f, f.dtype, f.shape)
```

#### ❖ Numpy

- 作为现有数组生成
  - np.empty_like(array [, dtype] )
    - 生成未初始化的、具有相同 dtype 和形状的数组
  - zeros_like(array [, dtype])
    - 生成用 0（零）初始化的、具有相同 dtype 和形状的数组
  - ones_like(array [, dtype])
    - 生成用 1 初始化的、具有相同 dtype 和形状的数组
  - full_like(array, fill_value [, dtype])
    - 生成用 fill_value 初始化的、具有相同 dtype 和形状的数组

- 作为现有数组生成

```python
import numpy as np
img = cv2.imread('../img/girl.jpg')

a = np.empty_like(img)
b = np.zeros_like(img)
c = np.ones_like(img)
d = np.full_like(img, 255)

print(a, a.dtype, a.shape)
print(b, b.dtype, b.shape)
print(c, c.dtype, c.shape)
print(d, d.dtype, d.shape)
```

- 作为序列和随机数生成
  - numpy.arange([start=0, ] stop [, step=1, dtype=float64])
    - start : 起始值
    - stop : 停止值，范围内的数字不包括 stop 本身，即 stop-1
    - step : 递增值
    - dtype : 数据类型
  - numpy.random.rand([d0 [, d1 [..., dn]]]) :
    - d0, d1..dn : 形状，如果省略，则返回一个随机数
    - 0 到 1 之间的随机数
  - numpy.random.randn([d0 [, d1 [..., dn]]]) :

#### Numpy

- 均值=0，分布=1，遵循标准分布的随机数

##### 生成序列和随机数

```python
import numpy as np

a = np.arange(5)
b = np.arange(5.0)
c = np.arange(3,9,2)

e = np.random.rand()
f = np.random.randn()
g = np.random.rand(2,3)
h = np.random.randn(2,3)
```

##### 更改数据类型

- a.astype('typename'), a.astype(np.dtype)
- np.uintXX(), np.intXX(), np.floatXX(), np.complexXX()

```python
a = np.arange(10)
print(a, a.dtype)

b = a.astype('float32')
print(b, b.dtype)

c = np.uint8(b)
print(c, c.dtype)

d = c.astype(np.float64)
print(d, d.dtype)
```

##### 更改维度

- np.reshape(array, newshape [, order])
- arr.reshape(newshape [, order])
    - array: 要重新排列的对象数组
    - newshape: 新数组的大小，元组
        - -1: 列大小未确定
    - order: {'C', 'F', 'A'} C: 类C顺序，F: 类Fortran顺序，A: F或C
- np.ravel(array)
    - 更改为1维
- arr.T: 转置数组

```python
a=np.arange(6) #[0 1 2 3 4 5] (6,)
b=a.reshape(2,3) #[[0 1 2] [3 4 5]] (2, 3)
c= np.arange(24).reshape(2,3,4)
#[[[ 0  1  2  3]...
 [[12 13 14 15]...
  [20 21 22 23]]] (2, 3, 4)

d = np.arange(100).reshape(-1, 5)
# [[0 1 2 3 4]
   [5 6 7 8 9]
      ...
   [95 96 97 98 99]]

e = np.arange(100).reshape(2, -1)
#[[0 1 2 3 ... 49]
   [50 51 52 ...99]]

f = np.ravel(c) #[0 1 2 3 4 5 6 7 8 .... 21 22 23]
```

##### 广播计算

- 标量与数组计算
- 数组与数组计算，两个数组应具有相同的形状

```python
a = np.array([10,20,30,40])
b = np.arange(1,5)

c = a + b
d = a - b
e = a * b
f = a / b

g = a + 5
h = a * 2
i = b ** 2
```

```
[10 20 30 40]
[1 2 3 4]

[11 22 33 44]
[ 9 18 27 36]
[ 10  40  90 160]
[ 10.  10.  10.  10.]

[15 25 35 45]
[20 40 60 80]
[ 1  4  9 16]
```

##### 矩阵

- 矩阵加法：+
- 矩阵乘法：np.dot(a, b), a.dot(b)
- 转置矩阵：a.T, a.transpose()

```python
a = np.arange(6).reshape(2,3)
b = np.array([1,2,3])
c = a + b

e = np.dot(a , b)
f = a.T

a.dot(b)
```

```
#[[0 1 2]
# [3 4 5]]
#[1 2 3]
#[[1 3 5]
#[4 6 8]]
#[ 8 26]
#[[0 3]
#[1 4]
#[2 5]]
#[ 8 26]
```

##### 索引/切片

- 1维
    - a[0]: 特定参数
    - a[1 : 5]: 第1到第4个参数
    - a[5:]: 第5个到末尾
    - a[:5]: 开头到第4个
    - a[ :]: 所有参数
- 2维
    - a[2, 3]: 第2列第3行
    - a[2:4, 3]: 第2到第3列，第3行
    - a[2, 1:4]: 第2列，第1到第3行
    - a[2, :]: 第2列，所有行
    - a[:, 2]: 所有列，第2行
    - a[1:3, :]: 第1到第2列，所有行

```python
a = np.arange(10) # [0 1 2 3 4 5 6 7 8 9]

print(a[5])  # 5
print(a[1:5])# [1 2 3 4]
print(a[5:]) # [5 6 7 8 9]
print(a[:5]) # [0 1 2 3 4]
print(a[:]). # [0 1 2 3 4 5 6 7 8 9]

b = np.arange(16).reshape(4,-1)

print(b)
print(b[2, 2])
print(b[2, :])
print(b[:, 2])
print(b[1:3, :])
```

##### 花式索引

- 生成符合特定条件的数据或初始化
- 条件计算：>, >=, <, <=, ==, !=
- 布尔类型（True/False）
- a[a > 4]
- a[a > 4] = 0

```python
a = np.arange(10) #[0 1 2 3 4 5 6 7 8 9]
b = a > 4
print(b) #[False False False False False  True True True True True]
print(a[b]) #[5 6 7 8 9]
print(a[a>4]) #[5 6 7 8 9]

a[a>4] = 0
print(a) # [0 1 2 3 4 0 0 0 0 0]

names = np.array(['John', 'Tom', 'Lee', 'Tom'])
scores = np.random.rand(4,4) * 50 + 50
print(scores)
print(names=='Tom')
print(scores[names=='Tom', :]) #仅提取Tom的分数
```

##### 合并

- numpy.hstack(arrays): 水平合并数组
- numpy.vstack(arrays): 垂直合并数组
- numpy.concatenate(arrays, axis=0): 沿指定轴合并数组
- numpy.stack(arrays, axis=0): 沿新轴合并数组
    - arrays: 合并对象，元组

```python
>>> a = np.arange(4).reshape(2,2)
>>> a
array([[0, 1],
       [2, 3]])
>>> b = np.arange(10, 14).reshape(2,2)
>>> b
array([[10, 11],
       [12, 13]])
>>> np.vstack( (a,b) )
array([[ 0,  1],
       [ 2,  3],
       [10, 11],
       [12, 13]])
>>> np.hstack( (a,b) )
array([[ 0,  1, 10, 11],
       [ 2,  3, 12, 13]])
>>> np.concatenate((a,b), 0)
array([[ 0,  1],
       [ 2,  3],
       [10, 11],
       [12, 13]])
>>> np.concatenate((a,b), 1)
array([[ 0,  1, 10, 11],
       [ 2,  3, 12, 13]])
>>> a = np.arange(12).reshape(4,3)
>>> b = np.arange(10, 130, 10).reshape(4,3)
>>> a
array([[ 0,  1,  2],
       [ 3,  4,  5],
       [ 6,  7,  8],
       [ 9, 10, 11]])
>>> b
array([[ 10,  20,  30],
       [ 40,  50,  60],
       [ 70,  80,  90],
       [100, 110, 120]])
>>> c = np.stack( (a,b), 0)
>>> c.shape
(2, 4, 3)
>>> c
array([[[  0,   1,   2],
        [  3,   4,   5],
        [  6,   7,   8],
        [  9,  10,  11]],
       [[ 10,  20,  30],
        [ 40,  50,  60],
        [ 70,  80,  90],
        [100, 110, 120]]])
```

##### 分割

- np.hsplit(array, indice) = np.split(axis=1)
- np.vsplit(array, indice) = np.split(axis=0)
- np.split(array, indice, axis=0)

```python
a = np.arange(12) # [ 0  1  2  3  4  5  6  7  8  9 10 11]

b = np.hsplit(a, 3)
# [array([0, 1, 2, 3]), array([4, 5, 6, 7]), array([ 8,  9, 10, 11])]

c = np.hsplit(a, (3,6))
# [array([0, 1, 2]), array([3, 4, 5]), array([ 6,  7,  8,  9, 10, 11])]
```

#### Numpy 搜索

- numpy.where(condition [, t, f]): 查找符合特定条件的元素
    - 返回值：元组，包含符合搜索条件的索引或更改后的值的数组
    - condition: 用于搜索的条件
    - t, f: 根据条件设置的值或数组，如果是数组，形状应与条件相同
        - t: 符合条件的值或数组
        - f: 不符合条件的值或数组
- numpy.nonzero(array): 返回数组参数中非0元素的索引，元组
- numpy.all(array [, axis]): 搜索数组所有参数是否为真
    - array: 搜索对象的数组
    - axis: 搜索的轴，如果省略，搜索所有轴，如果定义，按轴数返回结果
- numpy.any(array [, axis]): 搜索数组中是否存在真值

##### 搜索

```python
>>> a = np.arange(10, 20)
>>> a
array([10, 11, 12, 13, 14, 15, 16, 17, 18, 19])
>>> np.where(a > 15)
(array([6, 7, 8, 9]),)
>>> np.where(a > 15, 1, 0)
array([0, 0, 0, 0, 0, 0, 1, 1, 1, 1])
>>> a
array([10, 11, 12, 13, 14, 15, 16, 17, 18, 19])
>>> np.where(a>15, 99, a)
array([10, 11, 12, 13, 14, 15, 99, 99, 99, 99])
>>> np.where(a>15, a, 0)
array([ 0,  0,  0,  0,  0,  0, 16, 17, 18, 19])
>>> a
array([10, 11, 12, 13, 14, 15, 16, 17, 18, 19])
>>> b = np.arange(12).reshape(3,4)
>>> b
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11]])
>>> coords = np.where(b>6)
>>> coords
(array([1, 2, 2, 2, 2]), array([3, 0, 1, 2, 3]))
>>> np.stack( (coords[0], coords[1]), -1)
array([[1, 3],
       [2, 0],
       [2, 1],
       [2, 2],
       [2, 3]])
>>> z = np.array([0,1,2,0,1,2])
>>> np.nonzero(z)
(array([1, 2, 4, 5]),)
```

```python
>>> a = np.arange(10)
>>> b = np.arange(10)
>>> a
array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
>>> b
array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
>>> a==b
array([ True,  True,  True,  True,  True,  True,  True,
       True,  True,  True])
>>> np.all(a==b)
True
>>> b[5] = -1
>>> a
array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
>>> b
array([ 0,  1,  2,  3,  4, -1,  6,  7,  8,  9])
>>> np.all(a==b)
False
>>> np.where(a==b)
(array([0, 1, 2, 3, 4, 6, 7, 8, 9]),)
>>> np.where(a!=b)
(array([5]),)
>>>
```

##### 基本统计函数

- numpy.sum(array [, axis]): 数组求和
- numpy.mean(array [, axis]): 数组平均值
- numpy.amin(array [, axis]): 数组最小值
- numpy.min(array [, axis]): 与numpy.amin()相同
- numpy.amax(array [, axis]): 数组最大值
- numpy.max(array [, axis]): 与numpy.amax()相同
    - array: 计算对象的数组
    - axis: 计算的基准轴，如果未定义，所有参数都参与计算

```python
a = np.arange(12).reshape(-1,4)

print(np.sum(a)) #np.sum(-1)
print(np.sum(a, 0))
print(np.sum(a, 1))
print(np.sum(a, (0,)))

print(np.min(a))
print(np.min(a, 0))
print(np.min(a, 1))

print(np.max(a))
print(np.max(a, 0))
print(np.max(a, 1))

print(np.mean(a))
print(np.mean(a, 0))
print(np.mean(a, 1))
```

#### ❖ Numpy

- 创建灰度图像
  - n x m
  - n : 高度，行数
  - m : 宽度，列数
  - 像素值 : 0~255 (dtype=np.uint8)
- 创建彩色图像
  - n x m x 3
  - n : 高度，行数
  - m : 宽度，列数
  - 3 : 颜色通道，默认顺序为 [蓝色, 绿色, 红色]
  - 像素值 : 0~255 (dtype=np.uint8)
- Numpy 图像示例

```
img = np.zeros( (120,120), dtype=np.uint8)
img[25:35, :] = 45
img[55:65, :] = 115
img[85:95, :] = 160
img[:, 35:45] = 205
img[:, 75:85] = 255
cv2.imshow('Gray', img)

img2 = np.zeros( (120,120,3), dtype=np.uint8)
img2[25:35, : ] = [255,0,0]
img2[55:65, : ] = [0,255,0]
img2[85:95, : ] = [0,0,255]
img2[ : , 35:45] = [255,255,0]
img2[ : , 75:85] = [255,0,255]
cv2.imshow('Color', img2)
```

![](img/9de0cb7cd12c1232044968590f57e471_116_0.png)

### Numpy 与 Matplotlib

1. Numpy
2. **Matplotlib**

#### Matplotlib

![](img/9de0cb7cd12c1232044968590f57e471_118_0.png)

- https://matplotlib.org/
- Python 2D 绘图库
- 绘制各种图形、图表
- 在一个显示窗口中展示多张图像
- 安装
    - pip install matplotlib 或者 pip3 install matplotlib
    - sudo apt-get install python3-matplotlib
- 检查版本
    - import matplotlib
    - matplotlib.__version__
- 导入 pyplot
    - import matplotlib.pyplot as plt

![](img/9de0cb7cd12c1232044968590f57e471_118_1.png)

- plt.plot(x, y)
- plt.show()

```
import matplotlib.pyplot as plt
import numpy as np

x = np.arange(10)
y = x * 5
x, y
plt.plot(x,y)
plt.show()
```

![](img/9de0cb7cd12c1232044968590f57e471_118_2.png)

#### Matplotlib

- 颜色 : plt.plot(x, y, 'r')
  - b : 蓝色
  - g : 绿色
  - r : 红色
  - c : 青色
  - m : 品红色
  - y : 黄色
  - k : 黑色
  - w : 白色
- 线型 : plt.plot(x, y, '--g')

```
import matplotlib.pyplot as plt
import numpy as np

x = np.arange(10)
y = x**2
plt.plot(x,y,'r')
plt.show()
```

![](img/9de0cb7cd12c1232044968590f57e471_119_0.png)

| 字符 | 线型 | 字符 | 线型 |
|---|---|---|---|
| - | 实线(*) | -- | 虚线 |
| -. | 点划线 | : | 点线 |
| . | 点 | , | 像素 |
| o | 圆形 | v | 向下三角 |
| ^ | 向上三角 | < | 向左三角 |
| > | 向右三角 | 1 | 小向下三角 |
| 2 | 小向上三角 | 3 | 小向左三角 |
| 4 | 小向右三角 | s | 正方形 |
| p | 五边形 | * | 星形 |
| h | 六边形 | + | 加号 |
| D | 菱形 | x | 叉号 |

```
import matplotlib.pyplot as plt
import numpy as np

x = np.arange(10)
y = x**2
plt.plot(x,y,'--g')
plt.show()
```

![](img/9de0cb7cd12c1232044968590f57e471_119_1.png)

#### Matplotlib

- 标签
    - plt.xlabel('text')
    - plt.ylabel('text')
    - plt.title('text')

```
import matplotlib.pyplot as plt
import numpy as np

x = np.arange(10)
y = x **2

plt.plot(x,y)
plt.xlabel('time')
plt.ylabel('money')
```

![](img/9de0cb7cd12c1232044968590f57e471_120_0.png)

- 坐标轴范围
    - 仅显示所有数据中的特定区域
    - plt.xlim(x,y)
    - plt.ylim(x,y)

```
import matplotlib.pyplot as plt
import numpy as np

x = np.arange(10)
y = x **2
plt.plot(x,y)
plt.xlim(2,5)
plt.ylim(5,20)
plt.show()
```

![](img/9de0cb7cd12c1232044968590f57e471_120_1.png)

#### Matplotlib

- 图例
    - plt.plot(x, y, 'b', label='text')
    - plt.legend(loc='upper right')
        - upper left
        - upper center
        - upper right
        - center left
        - center
        - center right
        - lower left
        - lower center
        - lower right
        - best
        - right

![](img/9de0cb7cd12c1232044968590f57e471_121_0.png)

```
import matplotlib.pyplot as plt
import numpy as np

x = np.arange(10)
y = x ** 2
y2 = x*5 + 2

plt.plot(x,y, 'b', label='first')
plt.plot(x,y2, 'r', label='second')
plt.legend(loc='upper right')
plt.show()
```

- 注释
    - plt.annotate('text')

```
import matplotlib.pyplot as plt
import numpy as np

x = np.arange(10)
y = x**2

plt.plot(x,y)
plt.annotate('here', xy=(4,16), xytext=(5,20),
             arrowprops={'color':'green'})
plt.show()
```

![](img/9de0cb7cd12c1232044968590f57e471_121_1.png)

#### Matplotlib

- 散点图
  - plt.scatter(x, y, s=None, c=None, marker=None)

```
import matplotlib.pyplot as plt
import numpy as np
x = np.arange(1,5)
y = np.arange(2, 6)
x2 = np.arange(1.5,5.5)
y2 = np.arange(2.5,6.5)
plt.scatter(x, y)
plt.scatter(x2, y2, s=80, c='r', marker='^')
plt.show()
```

![](img/9de0cb7cd12c1232044968590f57e471_122_0.png)

- 子图
  - plt.subplot(r,c,idx)
  - plt.subplot(3位数字)

```
import matplotlib.pyplot as plt
import numpy as np

x = np.arange(10)

plt.subplot(2,2,1)
plt.plot(x,x**2)
plt.subplot(2,2,2)
plt.plot(x,x*5)
plt.subplot(223)
plt.plot(x, np.sin(x))
plt.subplot(224)
plt.plot(x,np.cos(x))
plt.show()
```

![](img/9de0cb7cd12c1232044968590f57e471_122_1.png)

#### Matplotlib

- 子图
  - plt.subplot(r,c,idx)
  - plt.subplot(3位数字)

```
import matplotlib.pyplot as plt
import numpy as np

x = np.arange(10)

plt.subplot(2,2,1)
plt.plot(x,x**2)
plt.subplot(2,2,2)
plt.plot(x,x*5)
plt.subplot(223)
plt.plot(x, np.sin(x))
plt.subplot(224)
plt.plot(x,np.cos(x))
plt.show()
```

![](img/9de0cb7cd12c1232044968590f57e471_123_0.png)

- OpenCV 图像
  - plt.imshow(img)
  - OpenCV 使用 BGR 格式，Matplotlib 使用 RGB 格式

```
import cv2
from matplotlib import pyplot as plt

img = cv2.imread('../img/model.jpg')
plt.imshow(img)
#plt.xticks([])
#plt.yticks([])
plt.show()
```

![](img/9de0cb7cd12c1232044968590f57e471_123_1.png)

#### Matplotlib

- OpenCV 图像
    - 将 BGR 转换为 RGB
        - 使用 numpy 切片
        - 使用 cv2.split() 和 cv2.merge()
        - 使用 cv2.cvtColor(img, cv2.COLOR_BGR2RGB)

```
img1 = cv2.imread('../img/model.jpg')
img2 = cv2.imread('../img/model2.jpg')
img3 = cv2.imread('../img/model3.jpg')

img1_rgb = np.zeros(img1.shape, dtype=np.uint8)
img1_rgb[:,:,2],img1_rgb[:,:,1], img1_rgb[:,:,0] = img1[:,:,0], \
    img1[:,:,1], img1[:,:,2]
#img1_rgb = img1[:,:, ::-1] 或 img1_rgb = img1[:,:,(2,1,0)]

b,g,r = cv2.split(img2)
img2_rgb = cv2.merge([r,g,b])
```

- OpenCV 图像
    - 将 BGR 转换为 RGB

![](img/9de0cb7cd12c1232044968590f57e471_124_0.png)

#### ❖ Matplotlib

- 直方图
  - plt.hist(img, bins, range)
  - https://matplotlib.org/devdocs/api/_as_gen/matplotlib.pyplot.hist.html#matplotlib.pyplot.hist

```
import matplotlib.pylab as plt
import cv2

img = cv2.imread('../img/model2.jpg', cv2.IMREAD_GRAYSCALE)
d1 = img.ravel()

plt.subplot(1,2,1)
plt.title('img')
plt.imshow(img, cmap='gray')
plt.subplot(122)
plt.title('hist')
plt.hist(d1, 255)
plt.show()
```

- 直方图

![](img/9de0cb7cd12c1232044968590f57e471_125_0.png)

### 图像处理基础

1. **ROI（感兴趣区域）**
2. 颜色空间
3. 阈值
4. 图像算术运算
5. 直方图
6. 实践工作坊

#### ❖ ROI

- 感兴趣区域
  - 感兴趣的区域
  - 图像编辑的主体区域，并非所有像素
  - 提高计算速度
- 使用 Numpy 切片设置特定区域
  - img[h:h+y, w:w+x]
    - 切片对于区域像素操作非常有效
  - img.item(h, w, c), img.itemset( (h, w, c), val)
    - Item() 函数对于单个像素操作非常有效
  - 注意原始引用
    - 与 Python 列表不同。
    - 如果编辑了 ROI，原始图像也会改变。
    - 如果编辑了原始图像，ROI 也会改变。
    - 如果不想修改原始图像，请复制后使用 (img.copy())

#### ❖ ROI（感兴趣区域）

- 设置 ROI

```
import cv2
import numpy as np

img = cv2.imread('./img/sunset.jpg')

x=320; y=150; w=50; h=50       # roi 坐标
roi = img[y:y+h, x:x+w]        # 设置 ROI

print(roi.shape)                # roi 形状, (50,50,3)
cv2.rectangle(roi, (0,0), (h-1, w-1), (0,255,0))
cv2.imshow("img", img)

cv2.waitKey(0)
cv2.destroyAllWindows()
```

#### ❖ ROI（感兴趣区域）

- 设置 ROI

![](img/9de0cb7cd12c1232044968590f57e471_128_0.png)

#### ❖ ROI 复制与新窗口显示

```
import cv2
import numpy as np

img = cv2.imread('./img/sunset.jpg')

x=320; y=150; w=50; h=50
roi = img[y:y+h, x:x+w]      # 设置 roi
img2 = roi.copy()             # 复制 roi 数组---①

img[y:y+h, x+w:x+w+w] = roi
cv2.rectangle(img, (x,y), (x+w+w, y+h), (0,255,0))

cv2.imshow("img", img)        # 显示原始图像
cv2.imshow("roi", img2)       # 仅显示 roi

cv2.waitKey(0)
cv2.destroyAllWindows()
```

#### ❖ ROI 复制并打开新窗口

![](img/9de0cb7cd12c1232044968590f57e471_129_0.png)

#### ❖ 用鼠标选择 ROI (1/2)

```python
import cv2
import numpy as np

isDragging = False
x0, y0, w, h = -1,-1,-1,-1
blue, red = (255,0,0),(0,0,255)

def onMouse(event,x,y,flags,param):
    global isDragging, x0, y0, img
    if event == cv2.EVENT_LBUTTONDOWN:
        isDragging = True
        x0 = x
        y0 = y
    elif event == cv2.EVENT_MOUSEMOVE:
        if isDragging:
            img_draw = img.copy()
            cv2.rectangle(img_draw, (x0, y0), (x, y), blue, 2)
            cv2.imshow('img', img_draw)
    elif event == cv2.EVENT_LBUTTONUP:
        if isDragging:
            isDragging = False
            w = x - x0
            h = y - y0
            print("x:%d, y:%d, w:%d, h:%d" % (x0, y0, w, h))
            if w > 0 and h > 0:
                img_draw = img.copy()
                cv2.rectangle(img_draw, (x0, y0), (x, y), red, 2)
                cv2.imshow('img', img_draw)
                roi = img[y0:y0+h, x0:x0+w]
                cv2.imshow('cropped', roi)
                cv2.moveWindow('cropped', 0, 0)
                cv2.imwrite('./cropped.jpg', roi)
                print("croped.")
        else:
            cv2.imshow('img', img)
            print("Drag area from left top to right bottom.")
img = cv2.imread('./img/sunset.jpg')
cv2.imshow('img', img)
cv2.setMouseCallback('img', onMouse)
cv2.waitKey()
cv2.destroyAllWindows()
```

#### ❖ 用鼠标选择 ROI

```
x:193, y:174, w:-56, h:-77
Drag area from left top to right bottom
x:314, y:148, w:64, h:54
croped.
```

![](img/9de0cb7cd12c1232044968590f57e471_131_0.png)

#### ❖ 选择 ROI

- 简单的鼠标 ROI 选择
- `ret=cv2.selectROI([win_name,] img[, showCrossHair, fromCenter])`
  - `win_name` : 用于 ROI 选择的窗口名称，字符串类型
  - `img` : 用于 ROI 选择的图像，NumPy ndarray 类型
  - `showCrossHair=True` : 是否在区域中心显示十字线
  - `fromCenter=False` : 将鼠标起始点周围设置为区域中心
  - `ret` : 所选区域的大小和坐标 (x, y, w, h)，当选择取消时，所有值为 0
- 空格键或回车键 : 结束选择
- 键盘 'C' : 取消选择

```python
import cv2, numpy as np

img = cv2.imread('../img/sunset.jpg')

x,y,w,h = cv2.selectROI('img', img, False)
if w and h:
    roi = img[y:y+h, x:x+w]
    cv2.imshow('cropped', roi)
    cv2.moveWindow('cropped', 0, 0)
    cv2.imwrite('./cropped2.jpg', roi)

cv2.imshow('img', img)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

### 图像处理基础

1. ROI (感兴趣区域)
2. **<u>色彩空间</u>**
3. 阈值
4. 图像算术
5. 直方图
6. 实践

#### ❖ 数字图像

- 二值图像
  - 每像素 1 位 (白色或黑色)，
  - 通过点的密度显示明暗
- 灰度图像
  - 每像素 1 字节 = 8 位
  - 显示 256 级明暗
- 彩色图像
  - 每像素 3 字节 = 24 位
  - 16,777,216 种颜色
  - 多种显示方式

![](img/9de0cb7cd12c1232044968590f57e471_134_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_134_1.png)

![](img/9de0cb7cd12c1232044968590f57e471_134_2.png)

![](img/9de0cb7cd12c1232044968590f57e471_134_3.png)

#### ❖ RGB, BGR, RGBA

- RGB
  - 最常用的色彩空间
  - 行 x 列 x 通道
  - 每种颜色值从 0 ~ 255
- BGR
  - 在 OpenCV 中 RGB 顺序相反
- RGBA
  - RGB + Alpha
  - Alpha : 透明度
  - 文件为 RGBA 格式时，使用 `cv2.IMREAD_UNCHANGED` 标志 => BGRA

![](img/9de0cb7cd12c1232044968590f57e471_135_0.png)

#### ❖ BGR, BGRA, Alpha

```python
import cv2
import numpy as np

img = cv2.imread('../img/opencv_logo.png')
bgr = cv2.imread('../img/opencv_logo.png', cv2.IMREAD_COLOR)
bgra = cv2.imread('../img/opencv_logo.png', cv2.IMREAD_UNCHANGED)

print("default:", img.shape, "color:", bgr.shape, "unchanged:",
bgra.shape)
cv2.imshow('bgr', bgr)
cv2.imshow('bgra', bgra)
cv2.imshow('alpha', bgra[:,:,3])
cv2.waitKey(0)
cv2.destroyAllWindows()
```

![](img/9de0cb7cd12c1232044968590f57e471_135_1.png)

#### 颜色转换函数

- `out = cv2.cvtColor(img, flag)`
  - `img` : Numpy 数组，要转换的图像
  - `flag` : 要转换的色彩空间，以 `cv2.COLOR_` 开头的名称，共 274 种
    - `cv2.COLOR_BGR2GRAY` : 将 BGR 彩色图像转换为灰度图
    - `cv2.COLOR_GRAY2BGR` : 将灰度图像转换为 BGR 彩色图像
    - `cv2.COLOR_BGR2RGB` : 将 BGR 彩色图像转换为 RGB 图像
    - `cv2.COLOR_BGR2HSV` : 将 BGR 彩色图像转换为 HSV 图像
    - `cv2.COLOR_HSV2BGR` : 将 HSV 彩色图像转换为 BGR 图像
    - `cv2.COLOR_BGR2YUV` : 将 BGR 彩色图像转换为 YUV
    - `cv2.COLOR_YUV2BGR` : 将 YUV 彩色图像转换为 BGR
    - `[i for i in dir(cv2) if i.startswith('COLOR_')]`
  - `out` : 转换结果 (NumPy 数组)

#### BGR 转灰度

- `cv2.COLOR_BGR2GRAY`

```python
import cv2
import numpy as np

img = cv2.imread('../img/girl.jpg')
img2 = img.astype(np.uint16)
b,g,r = cv2.split(img2)
gray1 = ((b + g + r)/3).astype(np.uint8)
gray2 = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
cv2.imshow('original', img)
cv2.imshow('gray1', gray1)
cv2.imshow('gray2', gray2)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

![](img/9de0cb7cd12c1232044968590f57e471_136_0.png)

#### HSV, HSI, HSL

- 区分颜色的三个特征
- 以圆柱系统通道表示
- 色相 (Hue) : 0~179
- 饱和度 (Saturation) : 0~255
- 明度 (Value) : 0~255
- 颜色范围
  - 红色 : 165~15, 0 ~ 15
  - 绿色 : 45~75
  - 蓝色 : 90~120
- `cv2.COLOR_BGR2HSV`

![](img/9de0cb7cd12c1232044968590f57e471_137_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_137_1.png)

#### BGR 转 HSV

```python
import cv2
import numpy as np

red_bgr = np.array([[[0,0,255]]], dtype=np.uint8)
green_bgr = np.array([[[0,255,0]]], dtype=np.uint8)
blue_bgr = np.array([[[255,0,0]]], dtype=np.uint8)
yellow_bgr = np.array([[[0,255,255]]], dtype=np.uint8)

red_hsv = cv2.cvtColor(red_bgr, cv2.COLOR_BGR2HSV);
green_hsv = cv2.cvtColor(green_bgr, cv2.COLOR_BGR2HSV);
blue_hsv = cv2.cvtColor(blue_bgr, cv2.COLOR_BGR2HSV);
yellow_hsv = cv2.cvtColor(yellow_bgr, cv2.COLOR_BGR2HSV);

print("red:",red_hsv)
print("green:", green_hsv)
print("blue", blue_hsv)
print("yellow", yellow_hsv)
```

```
red: [[[  0 255 255]]]
green: [[[ 60 255 255]]]
blue [[[120 255 255]]]
```

#### YUV, YCbCr

- 人眼对亮度敏感，对颜色不敏感
- Y : 亮度
- U (色度蓝, Cb) : 亮度与蓝色之间的色差
- V (色度红, Cr) : 亮度与红色之间的色差
- 数据压缩效果
  - 为 Y 分配更多位，为 UV 分配更少位
- `cv2.COLOR_BGR2YUV`
- `cv2.COLOR_BGR2YCrCb`

![](img/9de0cb7cd12c1232044968590f57e471_138_0.png)

#### BGR 转 YUV

```python
import cv2
import numpy as np

dark = np.array([[[0,0,0]]], dtype=np.uint8)
middle = np.array([[[127,127,127]]], dtype=np.uint8)
bright = np.array([[[255,255,255]]], dtype=np.uint8)
dark_yuv = cv2.cvtColor(dark, cv2.COLOR_BGR2YUV)
middle_yuv = cv2.cvtColor(middle, cv2.COLOR_BGR2YUV)
bright_yuv = cv2.cvtColor(bright, cv2.COLOR_BGR2YUV)
print("dark:",dark_yuv)
print("middle:", middle_yuv)
print("bright", bright_yuv)
```

```
dark: [[[  0 128 128]]]
middle: [[[127 128 128]]]
bright [[[255 128 128]]]
```

#### CMYK

- 青色 (Cyan)、品红色 (Magenta)、黄色 (Yellow)、黑色 (Key/Black)
- 减色混合：原色 + 黑色
- 在现实中难以显示纯色
- 用于印刷领域

![](img/9de0cb7cd12c1232044968590f57e471_139_0.png)

### 图像处理基础

1. ROI (感兴趣区域)
2. 色彩空间
3. **阈值**
4. 图像算术
5. 直方图
6. 实践

#### ❖ 二值化

- 两类分类
- 字母与纸张
- 背景与前景

#### ❖ 全局固定阈值

- 对一张图像使用一个阈值
- 确定最佳阈值
- 简单阈值法：试错法
- Otsu 方法

#### ❖ 局部自适应阈值

- 对每个像素使用不同的阈值
- 当每个区域具有不同光照水平时

#### ❖ 简单全局固定阈值

- 设置临时阈值 (边界) 值
- numpy 掩码
  - `dst[ src > thresh ] =255`
- `ret, dst = cv.threshold(src, thresh, maxval, flag)`
  - `src` : 输入图像
  - `thresh` : 阈值
  - `value` : 应用于符合阈值像素的值
  - `flag` : `cv2.THRESH_*`
    - `BINARY` : 像素 > 阈值 ? 值 : 0
    - `BINARY_INV` : 像素 > 阈值 ? 0 : 值
    - `TRUNC` : 像素 > 阈值 ? 值 : 像素
    - `TOZERO` : 像素 > 阈值 ? 像素 : 0
    - `TOZERO_INV` : 像素 > 阈值 ? 0 : 像素

#### 简单全局固定阈值

- 示例

```python
import cv2
import numpy as np
import matplotlib.pylab as plt

img = cv2.imread('../img/gray-gradient.jpg', cv2.IMREAD_GRAYSCALE)

t_np = np.zeros_like(img)
t_np[ img > 127] = 255

ret, t_bin = cv2.threshold(img, 127, 255, cv2.THRESH_BINARY)
ret, t_bininv = cv2.threshold(img, 127, 255,
cv2.THRESH_BINARY_INV)
ret, t_truc = cv2.threshold(img, 127, 255, cv2.THRESH_TRUNC)
ret, t_2zr = cv2.threshold(img, 127, 255, cv2.THRESH_TOZERO)
ret, t_2zrinv = cv2.threshold(img, 127, 255,
cv2.THRESH_TOZERO_INV)

titles = ['origin', 'Numpy', 'BINARY', 'BINARY_INV',
          'TRUNC', 'TOZERO', 'TOZERO_INV']
imgs = [img, t_np, t_bin, t_bininv, t_truc, t_2zr, t_2zrinv]

for i in range(len(imgs)):
    plt.subplot(2,4, i+1)
    plt.imshow(imgs[i], cmap='gray')
    plt.title(titles[i])
    plt.xticks([])
    plt.yticks([])

plt.show()
```

#### 简单全局固定阈值

- 示例 < 结果 >

![](img/9de0cb7cd12c1232044968590f57e471_143_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_143_1.png)

![](img/9de0cb7cd12c1232044968590f57e471_143_2.png)

![](img/9de0cb7cd12c1232044968590f57e471_143_3.png)

![](img/9de0cb7cd12c1232044968590f57e471_143_4.png)

![](img/9de0cb7cd12c1232044968590f57e471_143_5.png)

![](img/9de0cb7cd12c1232044968590f57e471_143_6.png)

#### 大津二值化法

- 寻找最佳阈值

![](img/9de0cb7cd12c1232044968590f57e471_143_7.png)

![](img/9de0cb7cd12c1232044968590f57e471_143_8.png)

![](img/9de0cb7cd12c1232044968590f57e471_143_9.png)

![](img/9de0cb7cd12c1232044968590f57e471_143_10.png)

![](img/9de0cb7cd12c1232044968590f57e471_143_11.png)

![](img/9de0cb7cd12c1232044968590f57e471_143_12.png)

![](img/9de0cb7cd12c1232044968590f57e471_143_13.png)

![](img/9de0cb7cd12c1232044968590f57e471_143_14.png)

#### 大津二值化法

- 大津展之，1979年
- 根据亮度将像素分为两类
- 中点作为阈值
- 如何操作
  - 使用阈值 t 将图像像素分为两类
    - t：0~255
    - 每类的比例：$\omega_1, \omega_2,$
    - 每类的平均亮度：$\mu1, \mu2$
    - 每类的分布：$\sigma_1^2, \sigma_2^2$
    - 类内方差：$\sigma_{\omega}^2(t) = \omega_1(t)\sigma_1^2(t) + \omega_2(t)\sigma_2^2(t)$
      - 最小 t 值
      - 最大 t 值
    - 类间方差：$\sigma_b^2(t) = \omega_1(t)\omega_2(t) [\mu_1(t) - \mu_2(t)]^2$
- cv2.threshold
  - 标志：cv2.THRESH_OTSU，返回值：T 值
  - 应用高斯滤波后有效

![](img/9de0cb7cd12c1232044968590f57e471_144_0.png)

#### 大津二值化法

```python
import cv2
import numpy as np
import matplotlib.pylab as plt

img = cv2.imread('../img/scaned_paper.jpg', cv2.IMREAD_GRAYSCALE)
_, t_130 = cv2.threshold(img, 130, 255, cv2.THRESH_BINARY)
t, t_otsu = cv2.threshold(img, 0, 255, cv2.THRESH_BINARY | cv2.THRESH_OTSU)
print('otsu threshold:', t)

imgs = {'Original': img, 't:130':t_130, 'otsu:%d'%t: t_otsu}
for i , (key, value) in enumerate(imgs.items()):
    plt.subplot(1, 3, i+1)
    plt.title(key)
    plt.imshow(value, cmap='gray')
    plt.xticks([]); plt.yticks([])

plt.show()
```

![](img/9de0cb7cd12c1232044968590f57e471_145_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_145_1.png)

![](img/9de0cb7cd12c1232044968590f57e471_145_2.png)

#### 自适应阈值

- 根据周围分辨率值设置阈值
- 采用并响应周围环境
- cv2.adaptiveThreshold(src, maxVal, method, type, blockSize, C)
  - src：灰度图像
  - maxVal：当像素满足阈值时应用的值
  - method：决定阈值的方法
    - cv2.ADAPTIVE_THRESH_*
    - MEAN_C：通过邻近像素的平均值决定
    - GAUSSIAN_C：通过高斯分布的加权和决定
  - type：阈值类型
  - blockSize：邻近区域的大小
  - C：通过在计算结果上加减来决定阈值

$$T(x,y)=\frac{1}{n^2} \sum_{x_i} \sum_{y_i} I(x+x_i,y+y_j) - C$$

- 数独扫描示例 < 1/2 >

```python
import cv2
import numpy as np
from matplotlib import pyplot as plt

file_name = '../img/sudoku.png'
img = cv2.imread(file_name, cv2.IMREAD_GRAYSCALE)
print(img.shape)
ret, th1 = cv2.threshold(img,127,255,cv2.THRESH_BINARY)
th2 = cv2.adaptiveThreshold(img,255,cv2.ADAPTIVE_THRESH_MEAN_C,
            cv2.THRESH_BINARY,11,7)
th3 = cv2.adaptiveThreshold(img,255,cv2.ADAPTIVE_THRESH_GAUSSIAN_C,
            cv2.THRESH_BINARY,11,7)
```

#### 自适应阈值

- 数独扫描示例 < 2/2 >

```python
titles = ['Original', 'Global', 'Mean', 'Gaussian']
images = [img, th1, th2, th3]

for i in range(4):
    plt.subplot(2,2,i+1)
    plt.imshow(images[i], 'gray')
    plt.title(titles[i])
    plt.xticks([]), plt.yticks([])

plt.show()
```

- 数独扫描示例 < 结果 >

![](img/9de0cb7cd12c1232044968590f57e471_147_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_147_1.png)

![](img/9de0cb7cd12c1232044968590f57e471_147_2.png)

![](img/9de0cb7cd12c1232044968590f57e471_147_3.png)

### 图像处理基础

1. 感兴趣区域
2. 颜色空间
3. 阈值
4. **图像算术**
5. 直方图
6. 工作坊

#### 图像算术

- NumPy 广播（ + , - , * , /）
  - 0 > 计算结果 > 255
- OpenCV 函数
  - cv2.add(src1, src2[, dest, mask, dtype])
    - src1：输入图像1或数字
    - src2：输入图像2或数字
    - out：输出图像
    - mask：计算非零像素
    - dtype：输出数据类型
  - cv2.subtract(src1, src2[, dest, mask, dtype])
    - 与 cv2.add() 相同
  - cv2.multiply(src1, src2[, dest, scale, dtype])
    - scale：添加到计算结果的值
  - cv2.divide(src1, src2[, dest, scale, dtype])
    - 与 cv2.multiply() 相同

#### 图像加法

```python
import cv2
import numpy as np
a = np.uint8([[200, 50]])
b = np.uint8([[100, 100]])
add1 = a + b
sub1 = a - b
mult1 = a * 2
div1 = a / 3
add2 = cv2.add(a, b)
sub2 = cv2.subtract(a, b)
mult2 = cv2.multiply(a , 2)
div2 = cv2.divide(a, 3)
print(add1, add2)
print(sub1, sub2)
print(mult1, mult2)
print(div1, div2)
```

```
[ 44 150]] [[255 150]]
[[100 206]] [[100   0]]
[[144 100]] [[255 100]]
[[66.66666667 16.66666667]] [[67 17]]
```

#### ❖ 带掩码的图像加法

```python
import cv2
import numpy as np

#---① 创建用于运算的数组
a = np.array([[1, 2]], dtype=np.uint8)
b = np.array([[10, 20]], dtype=np.uint8)
#---② 创建第二个元素为0的掩码数组
mask = np.array([[1, 0]], dtype=np.uint8)

#---③ 与累积赋值的比较运算
c1 = cv2.add( a, b , None, mask)
print(c1)
c2 = cv2.add( a, b , b, mask)
print(c2)
```

```
[[11  0]]
[[11 20]]
```

#### ❖ 图像混合

```python
a = np.array([250], dtype=np.uint8)
b = np.array([10], dtype=np.uint8)
c = a + b
print(a, b)
print( a+b )  # 250 + 10 = 260, 260 - 256 = 4
print( cv2.add(a, b)) #over 255

img1 = cv2.imread('../img/wingwall.jpg')
img2 = cv2.imread('../img/yate.jpg')

cv2.imshow('img1', img1)
cv2.imshow('img2', img2)
cv2.imshow('img1+img2', img1+img2)
cv2.imshow('cv2.add(img1,img2)', cv2.add(img1,img2))

cv2.waitKey(0)
cv2.destroyAllWindows()
```

#### ❖ 图像混合

![](img/9de0cb7cd12c1232044968590f57e471_151_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_151_1.png)

![](img/9de0cb7cd12c1232044968590f57e471_151_2.png)

![](img/9de0cb7cd12c1232044968590f57e471_151_3.png)

#### ❖ Alpha 混合

- 混合两张图像
- 决定权重，alpha : beta，alpha + beta = 1
- a*alpha + b*beta <= 255
- 使用 numpy 实现
  $g(x) = (1 - \alpha)f_0(x) + \alpha f_1(x)$
- cv2.addWeighted(img1, alpha, img2, beta, gamma)
  - img1, img2：要混合的两张图像
  - alpha：img1 的权重（alpha 值）
  - beta：img2 的权重，通常为 (1- alpha)
  - gamma：可以添加到计算结果的整数，通常为 0（零）
  $dst = \alpha \cdot img1 + \beta \cdot img2 + \gamma$

#### Alpha 混合

```python
alpha = 0.3

img1 = cv2.imread('../img/wingwall.jpg')
img2 = cv2.imread('../img/yate.jpg')

blended = img1 *alpha + img2 * (1-alpha)
blended = blended.astype(np.uint8)
cv2.imshow('img1 * 0.3 + img2 * 0.7', blended)

dst = cv2.addWeighted(img1, alpha, img2, (1-alpha), 0)
cv2.imshow('cv2.addWeighted', dst)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

![](img/9de0cb7cd12c1232044968590f57e471_152_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_152_1.png)

#### ❖ 图像混合 – 轨迹条

```python
import cv2
import numpy as np

win_name = 'Alpha blending'    # 窗口名称
trackbar_name = 'fade'         # 轨迹条名称

def onChange(x):
    alpha = x/100
    dst = cv2.addWeighted(img1, 1-alpha, img2, alpha, 0)
    cv2.imshow(win_name, dst)

img1 = cv2.imread('../img/man_face.jpg')
img2 = cv2.imread('../img/lion_face.jpg')
cv2.imshow(win_name, img1)
cv2.createTrackbar(trackbar_name, win_name, 0, 100, onChange)

cv2.waitKey()
cv2.destroyAllWindows()
```

![](img/9de0cb7cd12c1232044968590f57e471_153_0.png)

#### ❖ 位运算

- 计算图像2每个像素的位运算
- 用于删除背景、分离背景和前景以及混合
  - cv2.bitwise_and(img1, img2, mask=None)
  - cv2.bitwise_or(img1, img2, mask=None)
  - cv2.bitwise_xor(img1, img2, mask=None)
  - cv2.bitwise_not(img1, mask=None)
    - mask（可选）
      - 仅计算掩码值不为0的像素，掩码值为0的像素使用掩码值
      - 二值图像（黑色图像）
    - 掩码的img1、img2的行和列必须相同

#### ❖ 图像位运算

```python
import numpy as np, cv2
import matplotlib.pylab as plt
img1 = np.zeros( ( 200,400), dtype=np.uint8)
img2 = np.zeros( ( 200,400), dtype=np.uint8)
img1[:, :200] = 255           # 左侧为黑色(0)，右侧为白色(255)
img2[100:200, :] = 255        # 上侧为黑色(0)，下侧为白色(255)

bitAnd = cv2.bitwise_and(img1, img2)
bitOr = cv2.bitwise_or(img1, img2)
bitXor = cv2.bitwise_xor(img1, img2)
bitNot = cv2.bitwise_not(img1)

imgs = {'img1':img1, 'img2':img2, 'and':bitAnd,
        'or':bitOr, 'xor':bitXor, 'not(img1)':bitNot}
for i, (title, img) in enumerate(imgs.items()):
    plt.subplot(3,2,i+1)
    plt.title(title)
    plt.imshow(img, 'gray')
    plt.xticks([]); plt.yticks([])

plt.show()
```

#### 图像位运算

- 位运算示例

![](img/9de0cb7cd12c1232044968590f57e471_155_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_155_1.png)

![](img/9de0cb7cd12c1232044968590f57e471_155_2.png)

![](img/9de0cb7cd12c1232044968590f57e471_155_3.png)

![](img/9de0cb7cd12c1232044968590f57e471_155_4.png)

![](img/9de0cb7cd12c1232044968590f57e471_155_5.png)

#### 使用位运算进行掩码处理

```python
import numpy as np, cv2
import matplotlib.pylab as plt
img = cv2.imread('../img/girl.jpg')
mask = np.zeros_like(img)
cv2.circle(mask, (150,140), 100, (255,255,255), -1)
masked = cv2.bitwise_and(img, mask)

cv2.imshow('original', img)
cv2.imshow('mask', mask)
cv2.imshow('masked', masked)
cv2.waitKey()
cv2.destroyAllWindows()
mask = np.zeros(img.shape[:2], dtype=np.uint8)
cv2.circle(mask, (150,140), 100, (255), -1)
masked = cv2.bitwise_and(img, img, mask=mask)
```

#### 使用位运算进行掩码处理

![](img/9de0cb7cd12c1232044968590f57e471_156_0.png)

#### 差值图像

- 从图像中提取图像
- 查找差异
- diff = cv2.absdiff(img1, img2)
  - img1, img2：输入图像
  - diff：两幅图像之间的绝对差值

#### 差值图像（查找地图差异）

```python
import numpy as np, cv2

img1 = cv2.imread('../img/robot_arm1.jpg')
img2 = cv2.imread('../img/robot_arm2.jpg')
img1_gray = cv2.cvtColor(img1, cv2.COLOR_BGR2GRAY)
img2_gray = cv2.cvtColor(img2, cv2.COLOR_BGR2GRAY)
diff = cv2.absdiff(img1_gray, img2_gray)
_, diff = cv2.threshold(diff, 1, 255, cv2.THRESH_BINARY)
diff_red = cv2.cvtColor(diff, cv2.COLOR_GRAY2BGR)
diff_red[:,:,2] = 0
spot = cv2.bitwise_xor(img2, diff_red)
cv2.imshow('img1', img1)
cv2.imshow('img2', img2)
cv2.imshow('diff', diff)
cv2.imshow('spot', spot)
cv2.waitKey()
cv2.destroyAllWindows()
```

#### ❖ 差值图像（查找地图差异）

![](img/9de0cb7cd12c1232044968590f57e471_157_0.png)

#### ❖ 使用Alpha通道（RGBA PNG）进行混合

- 将标志混合到图像中

![](img/9de0cb7cd12c1232044968590f57e471_157_1.png)

#### HSV颜色掩码

- 使用HSV颜色空间的颜色掩码
- dst=cv2.inRange(src, lowerb, upperb)
  - src：原始组织
  - lowerb和upperb：范围
  - dst：将原始范围外的因子标记为零(0)

![](img/9de0cb7cd12c1232044968590f57e471_158_0.png)

- 按颜色区分 <1/2>

```python
import cv2
import numpy as np
import matplotlib.pylab as plt
img = cv2.imread("../img/cube.jpg")
hsv = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
blue1 = np.array([90, 50, 50])
blue2 = np.array([120, 255,255])
green1 = np.array([45, 50,50])
green2 = np.array([75, 255,255])
red1 = np.array([0, 50,50])
red2 = np.array([15, 255,255])
red3 = np.array([165, 50,50])
red4 = np.array([180, 255,255])
yellow1 = np.array([20, 50,50])
yellow2 = np.array([35, 255,255])
```

#### HSV颜色掩码

- 按颜色区分 <2/2>

```python
mask_blue = cv2.inRange(hsv, blue1, blue2)
mask_green = cv2.inRange(hsv, green1, green2)
mask_red = cv2.inRange(hsv, red1, red2)
mask_red2 = cv2.inRange(hsv, red3, red4)
mask_yellow = cv2.inRange(hsv, yellow1, yellow2)
res_blue = cv2.bitwise_and(img, img, mask=mask_blue)
res_green = cv2.bitwise_and(img, img, mask=mask_green)
res_red1 = cv2.bitwise_and(img, img, mask=mask_red)
res_red2 = cv2.bitwise_and(img, img, mask=mask_red2)
res_red = cv2.bitwise_or(res_red1, res_red2)
res_yellow = cv2.bitwise_and(img, img, mask=mask_yellow)
imgs = {'original': img, 'blue':res_blue, 'green':res_green,
        'red':res_red, 'yellow':res_yellow}
for i, (k, v) in enumerate(imgs.items()):
    plt.subplot(2,3, i+1)
    plt.title(k)
    plt.imshow(v[:,:,::-1])
    plt.xticks([]); plt.yticks([])
plt.show()
```

- 按颜色区分 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_159_0.png)

#### ❖ 色度键掩码与混合

- 色度键混合

```python
import cv2
import numpy as np
import matplotlib.pylab as plt

img1 = cv2.imread('../img/man_chromakey.jpg')
img2 = cv2.imread('../img/street.jpg')

height1, width1 = img1.shape[:2]
height2, width2 = img2.shape[:2]
x = (width2 - width1)//2
y = height2 - height1
w = x + width1
h = y + height1

chromakey = img1[:10, :10, :]
offset = 20

hsv_chroma = cv2.cvtColor(chromakey, cv2.COLOR_BGR2HSV)
hsv_img = cv2.cvtColor(img1, cv2.COLOR_BGR2HSV)
chroma_h = hsv_chroma[0]
lower = np.array([chroma_h.min()-offset, 100, 100])
upper = np.array([chroma_h.max()+offset, 255, 255])
mask = cv2.inRange(hsv_img, lower, upper)
mask_inv = cv2.bitwise_not(mask)
roi = img2[y:h, x:w]
fg = cv2.bitwise_and(img1, img1, mask=mask_inv)
bg = cv2.bitwise_and(roi, roi, mask=mask)
img2[y:h, x:w] = fg + bg
cv2.imshow('chromakey', img1)
cv2.imshow('added', img2)
cv2.waitKey()
cv2.destroyAllWindows()
```

#### ❖ 色度键掩码与混合

- 色度键混合 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_161_0.png)

#### ❖ 无缝克隆

- dst = cv2.seamlessClone(src, dst, mask, coords, flags[, output])
  - src：输入图像，通常为前景
  - dst：目标图像，通常为背景
  - mask：掩码，在src中要混合的区域为255，其余为0
  - coords：希望放置src的DST坐标（中心点）
  - flags：混合类型
    - cv2.NORMAL_CLONE：保持原始输入
    - cv2.MIXED_CLONE：混合输入和主体
  - output：混合结果
  - dst：混合结果

#### ❖ 无缝克隆混合

```python
import cv2
import numpy as np
import matplotlib.pylab as plt
img1 = cv2.imread("../img/drawing.jpg")
img2= cv2.imread("../img/my_hand.jpg")
mask = np.full_like(img1, 255)
height, width = img2.shape[:2]
center = (width//2, height//2)
normal = cv2.seamlessClone(img1, img2, mask, center, cv2.NORMAL_CLONE)
mixed = cv2.seamlessClone(img1, img2, mask, center, cv2.MIXED_CLONE)
cv2.imshow('normal', normal)
cv2.imshow('mixed', mixed)
cv2.waitKey()
cv2.destroyAllWindows()
```

![](img/9de0cb7cd12c1232044968590f57e471_162_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_162_1.png)

![](img/9de0cb7cd12c1232044968590f57e471_162_2.png)

![](img/9de0cb7cd12c1232044968590f57e471_162_3.png)

### 图像处理基础

1. 感兴趣区域（ROI）
2. 颜色空间
3. 阈值
4. 图像算术
5. **直方图**
6. 实践

#### 直方图

- 显示像素颜色值的频率
  - 1通道：灰度颜色
  - 2通道：交叉两个值
  - 3通道：难以显示
- 对比度均衡化，各种应用如背景删除
- 支持所有OpenCV、Numpy、MatPlotlib
  - cv2.calcHist()
  - np.histogram()
  - plt.hist()

![](img/9de0cb7cd12c1232044968590f57e471_164_0.png)

- cv2.calcHist(img, channel, mask, histSize, ranges)
  - img：输入图像，绑定在列表中如[img]
  - channel：要处理的通道，绑定在列表中
    - 1通道：[0]，2通道：[0,1]，3通道：[0,1,2]
  - mask：仅计算掩码中设置的像素的直方图
  - histSize：级别（bin）的数量，根据通道数量以列表形式显示
    - 1通道：[256]，2通道：[256, 256]，3通道：[256,256,256]
  - ranges：每个像素可以具有的值的范围，对于RGB为[0, 256]

#### ❖ 灰度单通道直方图

```python
import cv2
import numpy as np
import matplotlib.pylab as plt

img = cv2.imread('../img/mountain.jpg', cv2.IMREAD_GRAYSCALE)
cv2.imshow('img', img)

hist = cv2.calcHist([img], [0], None, [256], [0,256])
plt.plot(hist)

print(hist.shape)
print(hist.sum(), img.shape)
plt.show()
```

```
hist.shape: (256, 1)
hist.sum(): 270000.0 img.shape: (450, 600)
```

![](img/9de0cb7cd12c1232044968590f57e471_165_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_165_1.png)

#### 彩色直方图

```python
import cv2
import numpy as np
import matplotlib.pylab as plt

img = cv2.imread('../img/mountain.jpg')
cv2.imshow('img', img)

channels = cv2.split(img)
colors = ('b', 'g', 'r')
for (ch, color) in zip(channels, colors):
    hist = cv2.calcHist([ch], [0], None, [256], [0, 256])
    plt.plot(hist, color=color)
plt.show()
```

![](img/9de0cb7cd12c1232044968590f57e471_166_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_166_1.png)

#### ❖ 归一化

- 均匀分布像素
- 如果像素分布集中在某个区域，图像质量会变差。
    - 改善图像质量

- $I_N = (I - Min)\frac{newMax - newMin}{Max - Min} + newMin$
    - $I$ : 归一化前的值
    - $Min, Max$ : 归一化前范围的最小值、最大值
    - $newMin, newMax$ : 归一化后的最小值、最大值
    - $I_N$: 归一化后的值

![](img/9de0cb7cd12c1232044968590f57e471_167_0.png)

- dst = cv2.normalize(src, dst, alpha, beta, type_flag)
    - src : 归一化前的原始数据
    - dst : 归一化后的数据
    - alpha : 归一化区域 1
    - beta : 归一化区域 2，非分段归一化时不使用
    - type_flag : 为归一化算法选择的标志整数
        - cv2.NORM_MINMAX : 通过 alpha & beta 区域进行归一化
        - cv2.NORM_L1 : 除以总和，alpha = 所有归一化值的总和
        - cv2.NORM_L2 : 归一化单位向量
        - cv2.NORM_INF : 除以最大值

#### 直方图归一化

```python
import cv2
import numpy as np
import matplotlib.pylab as plt

img = cv2.imread('../img/abnormal.jpg', cv2.IMREAD_GRAYSCALE)

img_f = img.astype(np.float32)
img_norm = ((img_f - img_f.min()) * (255) / (img_f.max() - img_f.min())).astype(np.uint8)

img_norm2 = cv2.normalize(img, None, 0, 255, cv2.NORM_MINMAX)

hist = cv2.calcHist([img], [0], None, [256], [0, 256])
hist_norm = cv2.calcHist([img_norm], [0], None, [256], [0, 256])
hist_norm2 = cv2.calcHist([img_norm2], [0], None, [256], [0, 256])

cv2.imshow('Before', img)
cv2.imshow('Manual', img_norm)
cv2.imshow('cv2.normalize()', img_norm2)

hists = {'Before': hist, 'Manual': hist_norm,
         'cv2.normalize()': hist_norm2}
for i, (k, v) in enumerate(hists.items()):
    plt.subplot(1,3,i+1)
    plt.title(k)
    plt.plot(v)
plt.show()
```

#### 直方图归一化

![](img/9de0cb7cd12c1232044968590f57e471_169_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_169_1.png)

![](img/9de0cb7cd12c1232044968590f57e471_169_2.png)

![](img/9de0cb7cd12c1232044968590f57e471_169_3.png)

![](img/9de0cb7cd12c1232044968590f57e471_169_4.png)

![](img/9de0cb7cd12c1232044968590f57e471_169_5.png)

#### 均衡化

- 控制像素分布的高度而非宽度
- 对光照有效
- dst = cv.equalizeHist(src[, dst])
    - src : 目标图像，8位单通道
    - dst : 结果图像

> $H'(v) = round\left(\frac{cdf(v) - cdf_{min}}{(M \times N) - cdf_{min}} \times (L - 1)\right)$

- $cdf(v)$ : 直方图累积和
- $cdf_{min}$ : 累积最小值，1
- $M \times N$ : 像素数量，宽度 x 高度
- $L$ : 分布区域，256
- $round(v)$ : 四舍五入
- $H'(v)$ : 均衡化后的直方图值

#### ❖ 灰度均衡化

```python
import cv2
import numpy as np
import matplotlib.pylab as plt

img = cv2.imread('../img/yate.jpg', cv2.IMREAD_GRAYSCALE)
rows, cols = img.shape[:2]

hist = cv2.calcHist([img], [0], None, [256], [0, 256])
cdf = hist.cumsum()
cdf_m = np.ma.masked_equal(cdf, 0)
cdf_m = (cdf_m - cdf_m.min()) / (rows * cols) * 255
cdf = np.ma.filled(cdf_m, 0).astype('uint8')
img2 = cdf[img]
img3 = cv2.equalizeHist(img)

hist2 = cv2.calcHist([img2], [0], None, [256], [0, 256])
hist3 = cv2.calcHist([img3], [0], None, [256], [0, 256])

cv2.imshow('Before', img)
cv2.imshow('Manual', img2)
cv2.imshow('cv2.equalizeHist()', img3)
hists = {'Before': hist, 'Manual': hist2,
         'cv2.equalizeHist()': hist3}
for i, (k, v) in enumerate(hists.items()):
    plt.subplot(1,3,i+1)
    plt.title(k)
    plt.plot(v)
plt.show()
```

#### ❖ 灰度均衡化

![](img/9de0cb7cd12c1232044968590f57e471_171_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_171_1.png)

![](img/9de0cb7cd12c1232044968590f57e471_171_2.png)

![](img/9de0cb7cd12c1232044968590f57e471_171_3.png)

![](img/9de0cb7cd12c1232044968590f57e471_171_4.png)

![](img/9de0cb7cd12c1232044968590f57e471_171_5.png)

#### ❖ 彩色(YUV)均衡化

```python
import numpy as np, cv2

img = cv2.imread('../img/yate.jpg')

img_yuv = cv2.cvtColor(img, cv2.COLOR_BGR2YUV)

img_yuv[:,:,0] = cv2.equalizeHist(img_yuv[:,:,0])

img2 = cv2.cvtColor(img_yuv, cv2.COLOR_YUV2BGR)
cv2.imshow('Before', img)
cv2.imshow('After', img2)
cv2.waitKey()
cv2.destroyAllWindows()
```

#### ❖ 彩色(YUV)均衡化

![](img/9de0cb7cd12c1232044968590f57e471_172_0.png)

#### ❖ CLAHE

- 对比度受限自适应直方图均衡化
- 应用均衡器时，尽可能防止亮区过曝
- 如果超过直方图的限制值，则将其分配到其他尺度
- cv2.createCLAHE(clipLimit, tileGridSize) : 生成 CLAHE 对象
    - clipLimit : 对比度限制边界值，默认 40.0
    - tileGridSize : 区域大小，默认 8x8
- CLAHE : CLAHE 算法对象
    - apply(src) : 应用 CLAHE
        - src : 输入图像

![](img/9de0cb7cd12c1232044968590f57e471_172_1.png)

#### ❖ 使用 CLAHE 提升图像质量

```python
import cv2
import numpy as np
import matplotlib.pylab as plt

img = cv2.imread('../img/bright.jpg')
img_yuv = cv2.cvtColor(img, cv2.COLOR_BGR2YUV)

img_eq = img_yuv.copy()
img_eq[:,:,0] = cv2.equalizeHist(img_eq[:,:,0])
img_eq = cv2.cvtColor(img_eq, cv2.COLOR_YUV2BGR)

img_clahe = img_yuv.copy()
clahe = cv2.createCLAHE(clipLimit=3.0, tileGridSize=(8,8))
##### 生成 CLAHE 对象
img_clahe[:,:,0] = clahe.apply(img_clahe[:,:,0]) # 应用 CLAHE
img_clahe = cv2.cvtColor(img_clahe, cv2.COLOR_YUV2BGR)

cv2.imshow('Before', img)
cv2.imshow('CLAHE', img_clahe)
cv2.imshow('equalizeHist', img_eq)
cv2.waitKey()
cv2.destroyAllWindows()
```

![](img/9de0cb7cd12c1232044968590f57e471_173_0.png)

#### ❖ 2D 直方图

```python
import cv2, matplotlib.pylab as plt

plt.style.use('classic')            # 使用 1.x 风格以获得颜色样式
img = cv2.imread('../img/mountain.jpg')
plt.subplot(131)
hist = cv2.calcHist([img], [0,1], None, [32,32], [0,256,0,256])
p = plt.imshow(hist)
plt.title('Blue and Green')
plt.colorbar(p)

plt.subplot(132)
hist = cv2.calcHist([img], [1,2], None, [32,32], [0,256,0,256])
p = plt.imshow(hist)
plt.title('Green and Red')
plt.colorbar(p)

plt.subplot(133)
hist = cv2.calcHist([img], [0,2], None, [32,32], [0,256,0,256])
p = plt.imshow(hist)
plt.title('Blue and Red')
plt.colorbar(p)
plt.show()
```

![](img/9de0cb7cd12c1232044968590f57e471_174_0.png)

#### ❖ 反向投影

- HSV 色彩空间中的 HS 直方图
- ROI/全部比例
- 未选中区域的比例大多为 0
- 用比例映射原始像素
- 未选中颜色的像素为 0
- 适合作为掩码使用
- cv2.calcBackProject(img, channel, hist, ranges, scale)
    - img : 输入图像，绑定为列表，如 [img]
    - channel : 要处理的通道，绑定为列表，如
        - 单通道: [0], 双通道:[0,1], 三通道:[0,1,2]
    - hist : 用于反向投影的直方图
    - ranges : 每个像素的值范围
    - scale : 将应用于结果的缩放系数
- 通过 HSV 反向投影进行掩码 <1/2>

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

win_name = 'back_projection'
img = cv2.imread('../img/pump_horse.jpg')
hsv_img = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
draw = img.copy()

def masking(bp, win_name):
    disc = cv2.getStructuringElement(cv2.MORPH_ELLIPSE,(5,5))
    cv2.filter2D(bp,-1,disc,bp)
    _, mask = cv2.threshold(bp, 1, 255, cv2.THRESH_BINARY)
    result = cv2.bitwise_and(img, img, mask=mask)
    cv2.imshow(win_name, result)
def backProject_manual(hist_roi):
    hist_img = cv2.calcHist([hsv_img], [0,1], None,[180,256],
    [0,180,0,256])
```

#### 反向投影

- 通过HSV反向投影进行掩膜 <2/2>

```python
hist_rate = hist_roi/ (hist_img + 1)
h,s,v = cv2.split(hsv_img)
bp = hist_rate[h.ravel(), s.ravel()]
bp = np.minimum(bp, 1)
bp = bp.reshape(hsv_img.shape[:2])
cv2.normalize(bp,bp, 0, 255, cv2.NORM_MINMAX)
bp = bp.astype(np.uint8)
masking(bp,'result_manual')
def backProject_cv(hist_roi):
    bp = cv2.calcBackProject([hsv_img], [0, 1], hist_roi, [0, 180, 0, 256], 1)
    masking(bp,'result_cv')
(x,y,w,h) = cv2.selectROI(win_name, img, False)
if w > 0 and h > 0:
    roi = draw[y:y+h, x:x+w]
    cv2.rectangle(draw, (x, y), (x+w, y+h), (0,0,255), 2)
    hsv_roi = cv2.cvtColor(roi, cv2.COLOR_BGR2HSV)
    hist_roi = cv2.calcHist([hsv_roi],[0, 1], None, [180, 256], [0, 180, 0, 256] )
    backProject_manual(hist_roi)
    backProject_cv(hist_roi)
cv2.imshow(win_name, draw)
cv2.waitKey()
cv2.destroyAllWindows()
```

![](img/9de0cb7cd12c1232044968590f57e471_176_0.png)

#### 比较直方图

- 比较直方图以找出图像间的相似性
- cv2.compareHist(hist1, hist2, method)
  - hist1, hist2 : 要比较的两个直方图，大小和维度应相同。
  - method : 用于比较算法的标志整数
    - cv2.HISTCMP_CORREL : 相关性
      - 1 : 完全匹配，-1 : 最大不匹配，0: 无关联
    - cv2.HISTCMP_CHISQR : 卡方
      - 0 : 完全匹配，大值（未知）: 最大不匹配
    - cv2.HISTCMP_INTERSECT : 交叉
      - 1 : 完全匹配，0 : 最大不匹配（当归一化为1时）
    - cv2.HISTCMP_BHATTACHARYYA : 巴塔查里亚
      - 0: 完全匹配，1 : 最大不匹配
    - cv2.HISTCMP_HELLINGER : 与HISTCMP_BHATTACHARYYA相同

#### ❖ 比较直方图 <1/2>

```python
import cv2, numpy as np
import matplotlib.pylab as plt
img1 = cv2.imread('../img/taekwonv1.jpg')
img2 = cv2.imread('../img/taekwonv2.jpg')
img3 = cv2.imread('../img/taekwonv3.jpg')
img4 = cv2.imread('../img/dr_ochanomizu.jpg')
cv2.imshow('query', img1)
imgs = [img1, img2, img3, img4]
hists = []
for i, img in enumerate(imgs) :
    plt.subplot(1,len(imgs),i+1)
    plt.title('img%d'% (i+1))
    plt.axis('off')
    plt.imshow(img[:,:,::-1])
    hsv = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
    hist = cv2.calcHist([hsv], [0,1], None, [180,256], [0,180,0, 256])
    cv2.normalize(hist, hist, 0, 1, cv2.NORM_MINMAX)
    hists.append(hist)

query = hists[0]
methods = {'CORREL' :cv2.HISTCMP_CORREL,
           'CHISQR':cv2.HISTCMP_CHISQR,
           'INTERSECT':cv2.HISTCMP_INTERSECT,
           'BHATTACHARYYA':cv2.HISTCMP_BHATTACHARYYA}
for j, (name, flag) in enumerate(methods.items()):
    print('%-10s'%name, end='\t')
    for i, (hist, img) in enumerate(zip(hists, imgs)):
        ret = cv2.compareHist(query, hist, flag)
        if flag == cv2.HISTCMP_INTERSECT:
            ret = ret/np.sum(query)
        print("img%d:%7.2f"% (i+1 , ret), end='\t')
    print()
plt.show()
```

![](img/9de0cb7cd12c1232044968590f57e471_178_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_178_1.png)

![](img/9de0cb7cd12c1232044968590f57e471_178_2.png)

![](img/9de0cb7cd12c1232044968590f57e471_178_3.png)

![](img/9de0cb7cd12c1232044968590f57e471_178_4.png)

| 方法 | img1 | img2 | img3 | img4 |
|---|---|---|---|---|
| CORREL | 1.00 | 0.70 | 0.56 | 0.23 |
| CHISQR | 0.00 | 67.33 | 35.71 | 1129.49 |
| INTERSECT | 1.00 | 0.54 | 0.40 | 0.18 |
| BHATTACHARYYA | 0.00 | 0.48 | 0.47 | 0.79 |

### 图像处理基础

1. 感兴趣区域
2. 颜色空间
3. 阈值
4. 图像算术
5. 直方图
6. 实践环节

#### 怪物人脸合成

- 将两张人脸自然地融合成一张。
- 使用文件：
  - img/man_face.jpg
  - img/skull.jpg
- 结果示例

![](img/9de0cb7cd12c1232044968590f57e471_180_0.png)

- 提示
  - 对两张人脸的匹配区域进行0%~50%、50%~0%的Alpha混合

#### 运动检测监控

- 使摄像头能够检测运动。
- 结果示例

#### 运动检测监控

- 使用摄像头检测运动物体或人，并标记区域。
- 当检测到运动时，用红色打印“检测到运动”消息。
- 将一个屏幕分成左右两部分，在一个屏幕上显示标记了运动区域的屏幕，在另一个屏幕上仅显示相应的像素；将它们显示在一个屏幕上。
- 提示
  - 获取差分图像以寻找差异。
  - 不能仅通过前一帧/当前帧的差异来解决。
  - 当A和B有差异且B和C有差异时，将其定义为运动。
  - 可设置的两个因素：
    - 像素差异的大小
    - 像素差异的数量

![](img/9de0cb7cd12c1232044968590f57e471_181_0.png)

### 几何变换

1. **平移、缩放、旋转**
2. 畸变
3. 镜头畸变
4. 实践环节

#### ❖ 平移

- 移动图像
  - 要移动的坐标 = 原始坐标 + 移动距离
- 公式
  - $x' = x + d_x$
  - $y' = y + d_y$
- 行列式
  - $\begin{bmatrix} x' \\ y' \end{bmatrix} = \begin{bmatrix} 1 & 0 & d_x \\ 0 & 1 & d_y \end{bmatrix} \begin{bmatrix} x \\ y \\ 1 \end{bmatrix} \quad \begin{bmatrix} x' \\ y' \end{bmatrix} = \begin{bmatrix} x + d_x \\ y + d_y \end{bmatrix} = \begin{bmatrix} 1x + 0y + 1d_x \\ 0x + 1y + 1d_y \end{bmatrix}$

![](img/9de0cb7cd12c1232044968590f57e471_183_0.png)

#### ❖ 平移

- dst = cv2.warpAffine(src, mtrx, dsize [, dst, flags, borderMode, borderValue])
  - src: 原始图像，NumPy数组
  - mtrx: 2 x 3 变换矩阵，NumPy数组，dtype=float32
  - dsize: 结果图像大小，元组(宽度, 高度)
  - flags: 插值算法选择标志
    - cv2.INTER_LINEAR: 基本值，使用最近4个像素的距离权重
    - cv2.INTER_NEAREST: 使用最近像素值
    - cv2.INTER_AREA: 使用像素面积关系进行重采样
    - cv2.INTER_CUBIC: 使用最近16个像素的距离权重
    - cv2.INTER_LANCZOS4: 使用最近8个像素的Lanco算法
  - borderMode : 边界区域修正标志
    - cv2.BORDER_CONSTANT : 固定颜色值 (999|12345|999)
    - cv2.BORDER_REPLICATE : 复制边界 (111|12345|555)
    - cv2.BORDER_WRAP : 重复 (345|12345|123)
    - cv2.BORDER_REFLECT : 反射 (321|12345|543)
  - borderValue : 用于cv2.BORDER_CONSTANT的颜色值，基本值 = 0
  - dst : 结果图像，NumPy数组

#### ❖ 平移

```python
import cv2
import numpy as np
img = cv2.imread('../img/fish.jpg')
rows,cols = img.shape[0:2]  # 图像大小

dx, dy = 100, 50            # 像素移动距离

mtrx = np.float32([[1, 0, dx],
                   [0, 1, dy]])
dst = cv2.warpAffine(img, mtrx, (cols+dx, rows+dy))
dst2 = cv2.warpAffine(img, mtrx, (cols+dx, rows+dy), None,
                       cv2.INTER_LINEAR, cv2.BORDER_CONSTANT, (255,0,0) )
dst3 = cv2.warpAffine(img, mtrx, (cols+dx, rows+dy), None,
                       cv2.INTER_LINEAR, cv2.BORDER_REFLECT)

cv2.imshow('original', img)
cv2.imshow('trans',dst)
cv2.imshow('BORDER_CONSTANT', dst2)
cv2.imshow('BORDER_REFLECT', dst3)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

#### ❖ 平移

![](img/9de0cb7cd12c1232044968590f57e471_185_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_185_1.png)

![](img/9de0cb7cd12c1232044968590f57e471_185_2.png)

![](img/9de0cb7cd12c1232044968590f57e471_185_3.png)

#### ❖ 缩放

- 缩放：α · β 变换矩阵
  - $\begin{bmatrix} x' \\ y' \end{bmatrix} = \begin{bmatrix} \alpha & 0 & 0 \\ 0 & \beta & 0 \end{bmatrix} \begin{bmatrix} x \\ y \\ 1 \end{bmatrix}$
- Dst=cv2.resize(src, dsize, fx, fy, interpolation)
  - Src: 输入图像
  - Dsize : 输出图像大小 (w,h)
  - Fx, fy : 比例，如果省略则应用dsize
  - 插值
    - cv2.INTER_LINEAR :默认
    - cv2.INTER_NEAREST
    - cv2.INTER_AREA
    - cv2.INTER_CUBIC
    - cv2.INTER_LANCZOS4
  - dst : 结果图像

![](img/9de0cb7cd12c1232044968590f57e471_186_0.png)

#### 缩放

- 变换矩阵示例

```python
img = cv2.imread('../img/fish.jpg')
height, width = img.shape[:2]
m_shrink = np.float32([[0.5, 0, 0],
                       [0, 0.5, 0]])  # 矩阵
m_zoom = np.float32([[1.5, 0, 0],
                     [0, 1.5, 0]])  # 矩阵

shrink = cv2.warpAffine(img, m_shrink, (int(width*0.5),
                                        int(height*0.5)) )
zoom = cv2.warpAffine(img, m_zoom, (int(width*1.5),
                                    int(height*1.5)) )

cv2.imshow("original", img)
cv2.imshow("shrink", shrink)
cv2.imshow("zoom", zoom)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

#### 缩放

- 变换矩阵示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_187_0.png)

#### 缩放

- cv2.resize 函数示例

```python
import cv2
import numpy as np
img = cv2.imread('../img/fish.jpg')
height, width = img.shape[:2]
shrink = cv2.resize(img,None,fx=0.5, fy=0.5,
                    interpolation = cv2.INTER_AREA)
zoom = cv2.resize(img,(int(1.5*width), int(1.5*height)),
                  interpolation = cv2.INTER_CUBIC)
cv2.imshow("original", img)
cv2.imshow("shrink", shrink)
cv2.imshow("zoom", zoom)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

#### 缩放

- cv2.resize 函数示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_188_0.png)

- 旋转

  - 旋转变换矩阵

```
$$\begin{bmatrix} x' \\ y' \end{bmatrix} = \begin{bmatrix} \cos\theta & -\sin\theta & 0 \\ \sin\theta & \cos\theta & 0 \end{bmatrix} \begin{bmatrix} x \\ y \\ 1 \end{bmatrix}$$
```

![](img/9de0cb7cd12c1232044968590f57e471_188_1.png)

![](img/9de0cb7cd12c1232044968590f57e471_188_2.png)

#### 旋转

- 变换矩阵

```python
import cv2, numpy as np

img = cv2.imread('../img/fish.jpg')
rows,cols = img.shape[0:2]

d45 = 45.0 * np.pi / 180    # 45度
d90 = 90.0 * np.pi / 180    # 90度
m45 = np.float32( [[ np.cos(d45), -1* np.sin(d45), rows//2],
                   [np.sin(d45), np.cos(d45),    -1*cols//4]])
m90 = np.float32( [[ np.cos(d90), -1* np.sin(d90), rows],
                   [np.sin(d90), np.cos(d90), 0]])

r45 = cv2.warpAffine(img,m45,(cols,rows))
r90 = cv2.warpAffine(img,m90,(rows,cols))
cv2.imshow("origin", img)
cv2.imshow("45", r45)
cv2.imshow("90", r90)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

#### 旋转

- 变换矩阵

![](img/9de0cb7cd12c1232044968590f57e471_189_0.png)

#### 旋转

- 用于旋转的逆变换矩阵
- 可以设置旋转函数、旋转轴、缩放比例
- mtrx = cv2.getRotationMatrix2D(center, angle, scale)
  - center : 旋转轴中心，坐标，元组 (x, y)
  - angle : 旋转角度，六十进制
  - scale : 缩放比例

$$\begin{bmatrix} \alpha & \beta & (1-\alpha) \cdot center.x - \beta \cdot center.y \\ -\beta & \alpha & \beta \cdot center.x + (1-\alpha) \cdot center.y \end{bmatrix}$$

$$\alpha = scale \cdot \cos \theta,$$
$$\beta = scale \cdot \sin \theta$$

- 旋转函数示例

```python
img = cv2.imread('../img/fish.jpg')
rows,cols = img.shape[0:2]

matrix_45 = cv2.getRotationMatrix2D((cols/2,rows/2),45,1)
matrix_90 = cv2.getRotationMatrix2D((cols/2,rows/2),90,0.5)

print(matrix_45)
r45 = cv2.warpAffine(img, matrix_45,(cols,rows))
r90 = cv2.warpAffine(img, matrix_90,(cols,rows))

cv2.imshow('origin',img)
cv2.imshow("45", r45)
cv2.imshow("90", r90)

cv2.waitKey(0)
cv2.destroyAllWindows()
```

#### 旋转

- 旋转函数示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_191_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_191_1.png)

![](img/9de0cb7cd12c1232044968590f57e471_191_2.png)

### 几何变换

1. 平移、缩放、旋转
2. **变形**
3. 镜头畸变
4. 实践

#### 仿射变换

- 保持线条的平行性，图像变换
- 包括所有移动、放大、缩放
- 保持线条、长度比例和平行性的变换
- 将原始图像中的3个坐标映射到结果图像
- matrix = cv2.getAffineTransform(pts1, pts2)
  - pts1: 变换前的3个坐标，3 x 2 NumPy数组(float32)
  - pts2: 变换后的3个坐标，与pts1相同
  - matrix: 逆变换数组，2 x 3 数组

![](img/9de0cb7cd12c1232044968590f57e471_193_0.png)

```python
import cv2
import numpy as np
from matplotlib import pyplot as plt

file_name = '../img/fish.jpg'
img = cv2.imread(file_name)
rows, cols = img.shape[:2]
pts1 = np.float32([[100, 50], [200, 50], [100, 200]])
pts2 = np.float32([[80, 70], [210, 60], [250, 120]])
cv2.circle(img, (100,50), 5, (255,0), -1)
cv2.circle(img, (200,50), 5, (0,255,0), -1)
cv2.circle(img, (100,200), 5, (0,0,255), -1)
mtrx = cv2.getAffineTransform(pts1, pts2)
dst = cv2.warpAffine(img, mtrx, (int(cols*1.5), rows))
cv2.imshow('origin',img)
cv2.imshow('affin', dst)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

#### ❖ 透视变换

![](img/9de0cb7cd12c1232044968590f57e471_194_0.png)

- 实现扫描效果 < 1/2 >

```python
import cv2
import numpy as np

win_name = "scanning"
img = cv2.imread("../img/paper.jpg")
rows, cols = img.shape[:2]
draw = img.copy()
pts_cnt = 0
pts = np.zeros((4,2), dtype=np.float32)

def onMouse(event, x, y, flags, param):
    global pts_cnt
    if event == cv2.EVENT_LBUTTONDOWN:
        cv2.circle(draw, (x,y), 10, (0,255,0), -1)
        cv2.imshow(win_name, draw)

        pts[pts_cnt] = [x,y]
        pts_cnt+=1
        if pts_cnt == 4:
            sm = pts.sum(axis=1)
            diff = np.diff(pts, axis = 1)
            topLeft = pts[np.argmin(sm)]
            bottomRight = pts[np.argmax(sm)]
            topRight = pts[np.argmin(diff)]
```

#### ❖ 透视变换

- 实现扫描效果 < 2/2 >

```python
bottomLeft = pts[np.argmax(diff)]
pts1 = np.float32([topLeft, topRight, bottomRight , bottomLeft])
w1 = abs(bottomRight[0] - bottomLeft[0])    # 上方左右坐标间的距离
w2 = abs(topRight[0] - topLeft[0])            # 下方左右坐标间的距离
h1 = abs(topRight[1] - bottomRight[1])        # 右侧上下坐标间的距离
h2 = abs(topLeft[1] - bottomLeft[1])          # 左侧上下坐标间的距离
width = max([w1, w2])
height = max([h1, h2])
pts2 = np.float32([[0,0], [width-1,0], [width-1,height-1],
[0,height-1]])
mtrx = cv2.getPerspectiveTransform(pts1, pts2)
result = cv2.warpPerspective(img, mtrx, (width, height))
cv2.imshow('scanned', result)
cv2.imshow(win_name, img)
cv2.setMouseCallback(win_name, onMouse)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

- 实现扫描效果 < 结果 >

![](img/9de0cb7cd12c1232044968590f57e471_195_0.png)

#### ❖ 三角仿射变换

- 图像是矩形
- 需要三角仿射变换
  - 图像变形
  - 图像变形
  - 液化

![](img/9de0cb7cd12c1232044968590f57e471_196_0.png)

#### ❖ 三角仿射变换

- OpenCV未提供函数
- 三角仿射变换的步骤
  1. 选择变换前三角形的3个匹配坐标
  2. 选择变换后三角形的3个匹配坐标
  3. 计算包围步骤1三角形坐标的边界四边形坐标。
  4. 选择步骤3的四边形区域作为感兴趣区域。
  5. 将步骤4的感兴趣区域作为输入，使用步骤1和步骤2的坐标计算变换矩阵并进行仿射变换。
  6. 从步骤5变换后的感兴趣区域中，仅掩码步骤2的三角形坐标。
  7. 使用步骤6的掩码，将其与原始图像或其他图像合成。

- x,y,w,h = cv2.boundingRect(pts) pts : 多边形坐标，需要步骤3
  - x, y, w, h : 边界四边形的宽度和高度
- cv2.fillConvexPoly(img, points, color [, lineType]) img : 输入图像，需要步骤6
  - points : 多边形顶点
  - color : 用于填充的颜色
  - lineType : 线绘制算法的选择标志
- 三角仿射变换 < 1/2>

```python
import cv2
import numpy as np

img = cv2.imread("../img/taekwonv1.jpg")
img2 = img.copy()
draw = img.copy()

pts1 = np.float32([[188,14], [85,202], [294,216]])
pts2 = np.float32([[128,40], [85,307], [306,167]])

x1,y1,w1,h1 = cv2.boundingRect(pts1)
x2,y2,w2,h2 = cv2.boundingRect(pts2)

roi1 = img[y1:y1+h1, x1:x1+w1]
roi2 = img2[y2:y2+h2, x2:x2+w2]

offset1 = np.zeros((3,2), dtype=np.float32)
offset2 = np.zeros((3,2), dtype=np.float32)
for i in range(3):
    offset1[i][0], offset1[i][1] = pts1[i][0]-x1, pts1[i][1]-y1
    offset2[i][0], offset2[i][1] = pts2[i][0]-x2, pts2[i][1]-y2
```

#### 三角仿射变换 <2/2>

```python
mtrx = cv2.getAffineTransform(offset1, offset2)
warped = cv2.warpAffine( roi1, mtrx, (w2, h2), None,
    cv2.INTER_LINEAR, cv2.BORDER_REFLECT_101 )

mask = np.zeros((h2, w2), dtype = np.uint8)
cv2.fillConvexPoly(mask, np.int32(offset2), (255))

warped_masked = cv2.bitwise_and(warped, warped, mask=mask)
roi2_masked = cv2.bitwise_and(roi2, roi2,
    mask=cv2.bitwise_not(mask))
roi2_masked = roi2_masked + warped_masked
img2[y2:y2+h2, x2:x2+w2] = roi2_masked

cv2.rectangle(draw, (x1, y1), (x1+w1, y1+h1), (0,255,0), 1)
cv2.polylines(draw, [pts1.astype(np.int32)], True, (255,0,0), 1)
cv2.rectangle(img2, (x2, y2), (x2+w2, y2+h2), (0,255,0), 1)

cv2.imshow('origin', draw)
cv2.imshow('warped triangle', img2)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

![](img/9de0cb7cd12c1232044968590f57e471_198_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_198_1.png)

### 几何变换

1.  平移、缩放、旋转
2.  扭曲
3.  **镜头畸变**
4.  实践环节

#### 重映射

-   无法用行列式表示的非线性变换
-   `dst = cv2.remap(src, mapx, mapy, interpolation [, dst, borderMode, borderValue])`
    -   `src`：输入图像
    -   `mapx`, `mapy`：将要移动到 x 和 y 轴的坐标（索引）
        -   与 `src` 大小相同，`dtype=float32`
    -   其他参数与 `cv2.warpAffine()` 相同
    -   `dst`：结果图像
-   `mapy, mapx = np.indices( (rows, cols), dtype=np.float32)`
    -   生成带有索引值的数组
    -   使用 `cv2.remap()` 函数初始化数组

#### 翻转作为重映射

-   方程
    -   $x' = cols - x - 1$
    -   $y' = rows - y - 1$
-   行列式
    -   $\begin{bmatrix} x' \\ y' \end{bmatrix} = \begin{bmatrix} -1 & 0 & cols - 1 \\ 0 & -1 & rows - 1 \end{bmatrix} \begin{bmatrix} x \\ y \\ 1 \end{bmatrix}$
-   可以用行列式表示的线性变换效率较低

```python
import cv2
import numpy as np
import time

img = cv2.imread('../img/girl.jpg')
rows, cols = img.shape[:2]
st = time.time()
mflip = np.float32([ [-1, 0, cols-1],[0, -1, rows-1]])
fliped1 = cv2.warpAffine(img, mflip, (cols, rows))
print('matrix:', time.time()-st)

st2 = time.time()
mapy, mapx = np.indices((rows, cols),dtype=np.float32)
mapx = cols - mapx -1
mapy = rows - mapy -1
fliped2 = cv2.remap(img,mapx,mapy,cv2.INTER_LINEAR)
print('remap:', time.time()-st2)

cv2.imshow('origin', img)
cv2.imshow('fliped1',fliped1)
cv2.imshow('fliped2',fliped2)
cv2.waitKey()
cv2.destroyAllWindows()
```

matrix: 0.0009329319000244141
remap: 0.00399327278137207

![](img/9de0cb7cd12c1232044968590f57e471_201_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_201_1.png)

![](img/9de0cb7cd12c1232044968590f57e471_201_2.png)

#### ❖ 三角函数非线性重映射

```python
import cv2
import numpy as np

l = 20       # 波长
amp = 15     # 振幅
img = cv2.imread('../img/taekwonv1.jpg')
rows, cols = img.shape[:2]

mapy, mapx = np.indices((rows, cols),dtype=np.float32)
sinx = mapx + amp * np.sin(mapy/l)
cosy = mapy + amp * np.cos(mapx/l)

img_sinx=cv2.remap(img, sinx, mapy, cv2.INTER_LINEAR) # 仅对 x 应用正弦曲线
img_cosy=cv2.remap(img, mapx, cosy, cv2.INTER_LINEAR) # 仅对 y 应用余弦曲线
img_both=cv2.remap(img, sinx, cosy, cv2.INTER_LINEAR, None, cv2.BORDER_REPLICATE)

cv2.imshow('origin', img)
cv2.imshow('sin x', img_sinx)
cv2.imshow('cos y', img_cosy)
cv2.imshow('sin cos', img_both)
cv2.waitKey()
cv2.destroyAllWindows()
```

![](img/9de0cb7cd12c1232044968590f57e471_202_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_202_1.png)

![](img/9de0cb7cd12c1232044968590f57e471_202_2.png)

![](img/9de0cb7cd12c1232044968590f57e471_202_3.png)

#### ❖ 凸透镜与凹透镜畸变

-   坐标系
    -   直角坐标 → 极坐标：$\theta = \arctan(x, y)$，$r = \sqrt{x^2 + y^2}$
    -   极坐标 → 直角坐标：$x = r \cos(\theta)$，$y = r \sin(\theta)$

-   `r, theta = cv.cartToPolar( x, y )`：直角坐标系 → 极坐标系
-   `x, y = cv.polarToCart(r, theta)`：极坐标系 → 直角坐标系
    -   `x, y`：x, y 数组
    -   `r`：到原点的距离
    -   `theta`：角度值

![](img/9de0cb7cd12c1232044968590f57e471_203_0.png)

```python
import cv2, numpy as np

img = cv2.imread('../img/taekwonv1.jpg')
rows, cols = img.shape[:2]
exp = 0.5        # 凸透镜，凹透镜（凹透镜：0.1 ~ 1，凸透镜：1.1~）
scale = 1        # 变换大小（0 ~ 1）

mapy, mapx = np.indices((rows, cols),dtype=np.float32)
mapx = (2*mapx - cols)/cols
mapy = (2*mapy - rows)/rows

r, theta = cv2.cartToPolar(mapx, mapy)
r[r< scale] = r[r<scale] **exp
mapx, mapy = cv2.polarToCart(r, theta)

mapx = ((mapx + 1)*cols)/2
mapy = ((mapy + 1)*rows)/2
distorted = cv2.remap(img,mapx,mapy,cv2.INTER_LINEAR)

cv2.imshow('origin', img)
cv2.imshow('distorted', distorted)
cv2.waitKey()
cv2.destroyAllWindows()
```

![](img/9de0cb7cd12c1232044968590f57e471_204_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_204_1.png)

![](img/9de0cb7cd12c1232044968590f57e471_204_2.png)

#### ❖ 径向畸变

-   圆形相机镜头产生的四边形图像
-   桶形畸变：边缘凸起的图像畸变
-   桶形畸变方程：
    -   取决于系数，枕形畸变，桶形畸变

$r_d = r_u(1 + k_1r_u^2 + k_2r_u^4 + k_3r_u^6)$
-   $r_d$：畸变变换后
-   $r_u$：畸变变换前
-   $k_1, k_2, k_3$：畸变系数

![](img/9de0cb7cd12c1232044968590f57e471_205_0.png)

```python
import cv2
import numpy as np
k1, k2, k3 = 0.5, 0.2, 0.0 # 桶形畸变
#k1, k2, k3 = -0.3, 0, 0    # 枕形畸变
img = cv2.imread('../img/girl.jpg')
rows, cols = img.shape[:2]
mapy, mapx = np.indices((rows, cols),dtype=np.float32)
mapx = (2*mapx - cols)/cols
mapy = (2*mapy - rows)/rows
r, theta = cv2.cartToPolar(mapx, mapy)
ru = r*(1+k1*(r**2) + k2*(r**4) + k3*(r**6))
mapx, mapy = cv2.polarToCart(ru, theta)
mapx = ((mapx + 1)*cols)/2
mapy = ((mapy + 1)*rows)/2
distored = cv2.remap(img,mapx,mapy,cv2.INTER_LINEAR)

cv2.imshow('original', img)
cv2.imshow('distored', distored)
cv2.waitKey()
cv2.destroyAllWindows()
```

![](img/9de0cb7cd12c1232044968590f57e471_206_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_206_1.png)

![](img/9de0cb7cd12c1232044968590f57e471_206_2.png)

-   消除桶形畸变的 OpenCV 函数
-   `dst = cv2.undistort(src, cameraMtrix, distCoeffs)`
    -   `src`：原始输入图像
    -   `cameraMatrix`：相机矩阵
    -   `distCoeffs`：畸变系数。最少 4 个，或 5, 8, 12, 14 个
        -   `(k1, k2, p1, p2[,k3])`

$$\begin{bmatrix} f_x & 0 & c_x \\ 0 & f_y & c_y \\ 0 & 0 & 1 \end{bmatrix}$$

-   `cv2.undistort()`

```python
import numpy as np
import cv2

img = np.full((300,400,3), 255, np.uint8)
img[::10, :, :] = 0
img[:, ::10, :] = 0
width  = img.shape[1]
height = img.shape[0]

k1, k2, p1, p2 = 0.001, 0, 0, 0     # 桶形畸变
#k1, k2, p1, p2 = -0.0005, 0, 0, 0  # 枕形畸变
distCoeff = np.float64([k1, k2, p1, p2])

fx, fy = 10, 10
cx, cy = width/2, height/2
camMtx = np.float32([[fx,0, cx],
                      [0, fy, cy],
                      [0 ,0 ,1]])

dst = cv2.undistort(img,camMtx,distCoeff)
cv2.imshow('original', img)
cv2.imshow('dst',dst)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

![](img/9de0cb7cd12c1232044968590f57e471_207_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_207_1.png)

![](img/9de0cb7cd12c1232044968590f57e471_207_2.png)

### 几何变换

1.  平移、缩放、旋转
2.  扭曲
3.  镜头畸变
4.  实践环节

#### ❖ 马赛克

-   模糊选定区域。
-   结果示例

![](img/9de0cb7cd12c1232044968590f57e471_209_0.png)

-   提示
    -   如果选定区域被最小化然后放大，清晰度会变差。
    -   如果在插值期间选择 `cv2.INTER_AREA`，则会生成棋盘格时间模糊。

#### ❖ 液化工具

-   编写一个类似于 Photoshop 中液化工具的程序。
-   仅编辑选定区域。
-   结果示例

![](img/9de0cb7cd12c1232044968590f57e471_209_1.png)

-   提示
    -   基于鼠标坐标创建相同大小的四边形。
    -   使用四边形的中心创建 4 个三角形。
    -   计算鼠标拖动后从原始中心点移动的中心点。
    -   使用新三角形的大小，对三角形进行仿射变换。

![](img/9de0cb7cd12c1232044968590f57e471_210_0.png)

#### ❖ 畸变相机

-   制作具有魔镜效果的畸变相机。
-   6 种效果
    -   原图、左右对称、上下对称
    -   波浪、凸起、凹陷
-   结果示例

![](img/9de0cb7cd12c1232044968590f57e471_210_1.png)

-   提示
    -   参考镜头畸变示例，将 6 个映射结果合并为一张图像。
    -   只创建一个重映射坐标，并将该坐标应用于每一帧。

### 滤波器

1.  **卷积滤波器**
2.  模糊
3.  边缘检测
4.  形态学
5.  图像金字塔
6.  实践环节

### 滤波器

- 滤除不需要的值，仅获取所需的值。
- LPF（低通滤波器）：模糊化，消除噪声
- HPF（高通滤波器）：锐化，边缘检测
- 域
  - 空间域滤波器
    - 使用周围像素值进行计算，而非单个像素
    - 卷积计算
  - 频率域滤波器
    - 通过将像素值差异转换为频率进行计算
    - 傅里叶变换

#### 卷积

- 空间域滤波器的核心计算
- 将输入像素值与卷积核的每个因子相乘并求和
- 卷积核：选择用于计算的周围像素
  - 名称可能不同，如窗口、掩码、滤波器等
  - 卷积核大小：n x n
  - n：通常为奇数

![](img/9de0cb7cd12c1232044968590f57e471_213_0.png)

#### ❖ 用于计算的卷积函数

- dst = cv2.filter2D(src, ddepth, kernel[, dst, anchor, delta, borderType])
  - src：输入图像，NumPy 数组
  - ddepth：输出图像的数据类型，
    - -1：与输入图像相同
    - CV_8U, CV_16U/CV_16S, CV_32F, CV_64F
  - kernel：卷积核，float32 类型
  - dst：结果图像，NumPy 数组
  - anchor：卷积核的锚点，默认值：中心点(-1,-1)
  - delta：在滤波器结果值上要添加的值
  - borderType：指定边界像素校正方法

### 滤波器

1. 卷积滤波器
2. **模糊化**
3. 边缘检测
4. 形态学
5. 图像金字塔
6. 实践

#### ❖ 模糊化

- 使用滤波器模糊图像
- 适用于噪声去除
- 模糊类型
  - 均值模糊
    - cv2.blur, cv2.boxFilter
  - 高斯模糊
    - cv2.GaussianBlur
  - 中值模糊
    - cv2.medianBlur
  - 双边滤波器
    - cv2.bilateralFilter

#### ❖ 均值模糊

- 将像素值更改为周围因子的平均值
- 确定周围因子的域
  - 生成卷积核矩阵
  - 卷积核越大，图像越模糊
- 方法
  - cv2.filter2D()
  - cv2.boxFilter()
  - cv2.blur()
- 将周围像素应用于平均像素
- 平均特征值以产生模糊效果
- 5 X 5 均值滤波器

![](img/9de0cb7cd12c1232044968590f57e471_216_0.png)

#### ❖ 均值模糊

cv2.filter2D 示例

```python
import cv2, numpy as np

img = cv2.imread('../img/girl.jpg')
...
##### 5x5 평균 필터 커널 생성
kernel = np.array([[0.04, 0.04, 0.04, 0.04, 0.04],
                   [0.04, 0.04, 0.04, 0.04, 0.04],
                   [0.04, 0.04, 0.04, 0.04, 0.04],
                   [0.04, 0.04, 0.04, 0.04, 0.04],
                   [0.04, 0.04, 0.04, 0.04, 0.04]])
...
kernel = np.ones((5,5))/5*5
blured = cv2.filter2D(img, -1, kernel)

cv2.imshow('origin', img)
cv2.imshow('avrg blur', blured)
cv2.waitKey()
cv2.destroyAllWindows()
```

示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_217_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_217_1.png)

#### ❖ 均值模糊

- 无需定义卷积核即可应用模糊
- dst = cv2.blur(src, ksize[, dst, anchor, borderType])
  - src：输入图像，NumPy 数组
  - ksize：卷积核大小
  - 其他参数与 cv2.filter2D() 相同
- dst = cv2.boxFilter(src, ddepth, ksize[, dst, anchor, normalize, borderType])
  - src：输入图像，NumPy 数组
  - ddepth：输出图像的数据类型，-1：与输入图像相同
  - normalize：是否根据卷积核大小进行归一化，布尔值
  - 其他参数与 cv2.filter2D() 相同
- 模糊特定函数示例

```python
import cv2
import numpy as np

file_name = '../img/taekwonv1.jpg'
img = cv2.imread(file_name)

##### blur()
blur1 = cv2.blur(img, (10,10))

##### boxFilter()
blur2 = cv2.boxFilter(img, -1, (10,10))

merged = np.hstack( (img, blur1, blur2))
cv2.imshow('blur', merged)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

#### ❖ 均值模糊

- 模糊特定函数结果

![](img/9de0cb7cd12c1232044968590f57e471_219_0.png)

#### ❖ 高斯模糊

- 使用应用高斯函数的卷积核
- 对白噪声有效
- cv2.GaussianBlur(src, ksize, sigmaX[, sigmaY, border])
  - src：输入图像
  - ksize：卷积核大小
  - sigmaX：X 方向标准差，0 = 自动
    - σ = 0.3 * ((ksize - 1) * 0.5 - 1) + 0.8
  - sigmaY：Y 方向标准差，默认值 = sigmaX
  - border：边界修正
- kernel = cv2.getGaussianKernel(ksize, sigma)
  - ksize：卷积核大小
  - sigma：标准差
  - kernel * kernel.T

![](img/9de0cb7cd12c1232044968590f57e471_219_1.png)

| 1 | 4 | 7 | 4 | 1 |
|---|---|---|---|---|
| 4 | 16 | 26 | 16 | 4 |
| 7 | 26 | 41 | 26 | 7 |
| 4 | 16 | 26 | 16 | 4 |
| 1 | 4 | 7 | 4 | 1 |

$$\frac{1}{273}$$

#### 高斯模糊

- 示例

```python
import cv2
import numpy as np

img = cv2.imread('../img/gaussian_noise.jpg')

k1 = np.array([[1, 2, 1],
               [2, 4, 2],
               [1, 2, 1]]) * (1/16)
blur1 = cv2.filter2D(img, -1, k1)

k2 = cv2.getGaussianKernel(3, 0)
blur2 = cv2.filter2D(img, -1, k2*k2.T)

blur3 = cv2.GaussianBlur(img, (3, 3), 0)

print('k1:', k1)
print('k2:', k2*k2.T)
merged = np.hstack((img,blur1,blur2,blur3))
cv2.imshow('gaussian blur', merged)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

- 示例 <执行结果>

![](img/9de0cb7cd12c1232044968590f57e471_220_0.png)

#### ❖ 中值模糊

- 应用卷积核域内像素值的中值
- 对椒盐噪声有效
- 重用现有像素值，而非新值
- dst = cv2.medianBlur(src, ksize)
  - src：输入图像，NumPy 数组
  - ksize：卷积核大小

```python
import cv2
import numpy as np

img = cv2.imread("../img/salt_pepper_noise.jpg")

blur = cv2.medianBlur(img, 5)

merged = np.hstack((img,blur))
cv2.imshow('media', merged)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

![](img/9de0cb7cd12c1232044968590f57e471_221_0.png)

#### ❖ 双边滤波器模糊

- 双向滤波器，使用两个滤波器
  - 高斯滤波器 + 边界滤波器
- 模糊结果会模糊边缘，此方法改善了这一问题
- 速度较慢，因为计算时间长
- dst = cv2.bilateralFilter(src, d, sigmaColor, sigmaSpace[, dst, borderType])
  - src：输入图像，NumPy 数组
  - d：滤波器直径，如果大于 5，速度会非常慢
  - sigmaColor：颜色域滤波器的 Sigma 值
  - sigmaSpace：坐标域的 Sigma 值
    - 建议为 Sigma Color 和 Sigma Space 使用相同的值以简化使用
    - 建议值在 10 ~ 150 之间
- 示例

```python
import cv2
import numpy as np

img = cv2.imread("../img/gaussian_noise.jpg")

blur1 = cv2.GaussianBlur(img, (5,5), 0)

blur2 = cv2.bilateralFilter(img, 5, 75, 75)

merged = np.hstack((img, blur1, blur2))
cv2.imshow('bilateral', merged)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

![](img/9de0cb7cd12c1232044968590f57e471_222_0.png)

### 滤波器

1. 卷积滤波器
2. 模糊化
3. **边缘检测**
4. 形态学
5. 图像金字塔
6. 实践

#### 边缘检测

- 区分前景图像和背景图像
- 锐化：通过检测强调边缘
- 边缘：像素值变化大的点
- 微分
  - 分辨率变化的比率
  - 图像的斜率

连续函数：

$$\frac{\partial f(x, y)}{\partial x} = \lim_{h \to 0} \frac{f(x+h, y) - f(x, y)}{h}$$

$$\frac{\partial f(x, y)}{\partial y} = \lim_{h \to 0} \frac{f(x, y+h) - f(x, y)}{h}$$

#### 基本微分滤波器

- 简化微分方程
  - 非连续空间
  - 通过微分计算近似值
  - 下一个像素 – 当前像素
  - 双向卷积核
    - $Gx = [-1 \quad 1]$
    - $Gy = \begin{bmatrix} -1 \\ 1 \end{bmatrix}$

$$Gx = \frac{\partial f(x, y)}{\partial x} \approx f_{x+1, y} - f_{x, y}$$

$$Gy = \frac{\partial f(x, y)}{\partial y} \approx f_{x, y+1} - f_{x, y}$$

![](img/9de0cb7cd12c1232044968590f57e471_224_0.png)

| 像素值 | 6 | 6 | 6 | 5 | 4 | 3 | 2 | 1 | 1 | 1 | 6 | 6 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 一阶微分 | 0 | 0 | 0 | -1 | -1 | -1 | -1 | 0 | 0 | 5 | 0 | |
| 二阶微分 | 0 | 0 | 0 | -1 | 0 | 0 | 1 | 0 | 5 | -5 | 0 | |

#### 基本微分滤波器

卷积核 [-1, 1] 示例

```python
import cv2
import numpy as np

img = cv2.imread("../img/sudoku.jpg")

gx_kernel = np.array([[ -1, 1]])
gy_kernel = np.array([[ -1],[ 1]])

edge_gx = cv2.filter2D(img, -1, gx_kernel)
edge_gy = cv2.filter2D(img, -1, gy_kernel)

merged = np.hstack((img, edge_gx, edge_gy))
cv2.imshow('edge', merged)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

卷积核 [-1, 1] 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_225_0.png)

#### 边缘的方向与大小

- 梯度
  - 大小：变化的强度
  - 方向：值变化的方向
    - 垂直于边缘
- 用于图像的矢量化

$$
\begin{aligned}
\text{大小} &= \sqrt{G_x^2 + G_y^2} \\
\text{方向}(\theta) &= \arctan\left(\frac{G_y}{G_x}\right)
\end{aligned}
$$

![](img/9de0cb7cd12c1232044968590f57e471_226_0.png)

#### Roberts 交叉算子

- 由 Lawrence Roberts (1963) 提出
- 所有改进的核微分算子中最古老的一种
- 在对角线方向放置 1 和 -1
- 与基本微分掩模相比，具有更好的对角线检测效果
- 对噪声敏感
- 边缘强度较弱

$$
Gx = \begin{bmatrix} +1 & 0 \\ 0 & -1 \end{bmatrix}
$$

$$
Gy = \begin{bmatrix} 0 & +1 \\ -1 & 0 \end{bmatrix}
$$

##### 示例

```python
import cv2
import numpy as np

img = cv2.imread("../img/sudoku.jpg")

gx_kernel = np.array([[1,0], [0,-1]])
gy_kernel = np.array([[0, 1],[-1,0]])

edge_gx = cv2.filter2D(img, -1, gx_kernel)
edge_gy = cv2.filter2D(img, -1, gy_kernel)

merged = np.hstack((img, edge_gx, edge_gy, edge_gx+edge_gy))
cv2.imshow('roberts cross', merged)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

##### 示例 < 结果 >

![](img/9de0cb7cd12c1232044968590f57e471_227_0.png)

#### Prewitt 算子

- 由 Judith M. S. Prewitt 提出
- 每个方向 3 个差分
- 边缘强度强
- 对垂直和平行边缘效果相同
- 对角线检测能力弱

$$
Gx = \begin{bmatrix} -1 & 0 & +1 \\ -1 & 0 & +1 \\ -1 & 0 & +1 \end{bmatrix}
$$

$$
Gy = \begin{bmatrix} -1 & -1 & -1 \\ 0 & 0 & 0 \\ +1 & +1 & +1 \end{bmatrix}
$$

##### 示例

```python
import cv2
import numpy as np
file_name = "../img/sudoku.jpg"
img = cv2.imread(file_name)
gx_k = np.array([[-1,0,1], [-1,0,1],[-1,0,1]])
gy_k = np.array([[-1,-1,-1],[0,0,0], [1,1,1]])
edge_gx = cv2.filter2D(img, -1, gx_k)
edge_gy = cv2.filter2D(img, -1, gy_k)
merged = np.hstack((img, edge_gx, edge_gy, edge_gx+edge_gy))
cv2.imshow('prewitt', merged)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

##### 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_228_0.png)

#### Sobel 算子

- 由 Irwin Sobel (1968) 提出
- 初级微分算子中最具代表性的一种
- 中心分辨率差分的权重加倍
- 对所有平行、垂直和对角线边缘都有很强的检测能力

$$
Gx = \begin{bmatrix} -1 & 0 & +1 \\ -2 & 0 & +2 \\ -1 & 0 & +1 \end{bmatrix}
$$

$$
Gy = \begin{bmatrix} -1 & -2 & -1 \\ 0 & 0 & 0 \\ +1 & +2 & +1 \end{bmatrix}
$$

- dst = cv2.Sobel(src, ddepth, dx, dy[, dst, ksize, scale, delta, borderType])
  - src : 输入图像，NumPy 数组
  - ddepth : 输出图像的数据类型，-1 : 与输入图像相同
  - dx, dy : 微分阶数，选择 0,1,2 中的一个，不能同时为 0
  - ksize : 核大小，选择 1,3,5,7
  - scale : 微分中使用的系数
  - delta : 将添加到计算结果的值

##### 示例

```python
import cv2
import numpy as np

img = cv2.imread("../img/sudoku.jpg")

gx_k = np.array([[-1,0,1], [-2,0,2],[-1,0,1]])
gy_k = np.array([[-1,-2,-1],[0,0,0], [1,2,1]])

edge_gx = cv2.filter2D(img, -1, gx_k)
edge_gy = cv2.filter2D(img, -1, gy_k)

sobelx = cv2.Sobel(img, -1, 1, 0, ksize=3)
sobely = cv2.Sobel(img, -1, 0, 1, ksize=3)

merged1 = np.hstack((img, edge_gx, edge_gy, edge_gx+edge_gy))
merged2 = np.hstack((img, sobelx, sobely, sobelx+sobely))
merged = np.vstack((merged1, merged2))
cv2.imshow('sobel', merged)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

##### 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_230_0.png)

#### Scharr 算子

- Sobel 的问题
  - 即使核大小很大或很小，随着距离中心的增加，精度也会降低

- 改进的 Sobel

$$
Gx = \begin{bmatrix} -3 & 0 & +3 \\ -10 & 0 & +10 \\ -3 & 0 & +3 \end{bmatrix}
$$

$$
Gy = \begin{bmatrix} -3 & -10 & -3 \\ 0 & 0 & 0 \\ +3 & +10 & +3 \end{bmatrix}
$$

- dst = cv2.Scharr(src, ddepth, dx, dy[, dst, scale, delta, borderType])
  - 函数与 cv2.Sobel() 相同，只是没有 ksize 参数

##### 示例

```python
import cv2
import numpy as np

img = cv2.imread("../img/sudoku.jpg")
gx_k = np.array([[-3,0,3], [-10,0,10],[-3,0,3]])
gy_k = np.array([[-3,-10,-3],[0,0,0], [3,10,3]])
edge_gx = cv2.filter2D(img, -1, gx_k)
edge_gy = cv2.filter2D(img, -1, gy_k)
scharrx = cv2.Scharr(img, -1, 1, 0)
scharry = cv2.Scharr(img, -1, 0, 1)
merged1 = np.hstack((img, edge_gx, edge_gy))
merged2 = np.hstack((img, scharrx, scharry))
merged = np.vstack((merged1, merged2))
cv2.imshow('Scharr', merged)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

##### 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_231_0.png)

#### 拉普拉斯算子

- Pierre-Simon de Laplace (1749-1827)
- 代表性的二阶微分算子
- 解决初级微分中渐变边缘检测的问题

$$
\frac{d^2f}{dy^2} = \frac{df(x,y+1)}{dy} - \frac{df(x,y)}{dy}
$$

$$
= [f(x,y+1) - f(x,y)] - [f(x,y) - f(x,y-1)]
$$

$$
= f(x,y+1) - 2f(x,y) + f(x,y-1)
$$

$$
\text{核} = \begin{bmatrix} 0 & 1 & 0 \\ 1 & -4 & 1 \\ 0 & 1 & 0 \end{bmatrix}
$$

- cv2.Laplacian(src, ddepth)
  - 函数参数与 cv2.Sobel() 相同

##### 示例

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt
img = cv2.imread("../img/sudoku.jpg")
edge = cv2.Laplacian(img, -1)
merged = np.hstack((img, edge))
cv2.imshow('Laplacian', merged)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

##### 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_232_0.png)

#### Canny 边缘检测器

- 由 John F Canny 提出 (1986)
- 4 步算法，而非一步
  1. 噪声抑制
     - 5x5 高斯滤波器
  2. 边缘梯度检测
     - 使用 Sobel 计算梯度方向角
  3. 非极大值抑制
     - 从梯度方向中选择最大值，丢弃其余值
  4. 滞后阈值处理
     - 设置最大和最小阈值
     - 检查阈值区域内的边缘
     - 连接到大于最大值的值，如果没有这样的值，则丢弃

![](img/9de0cb7cd12c1232044968590f57e471_233_0.png)

- edges = cv2.Canny(img, threshold1, threshold2 [,edges, apertureSize, L2gradient])
  - img : 输入图像，NumPy 数组
  - threshold1, threshold2 : 阈值处理的最大和最小值
  - apertureSize : Sobel 掩模中使用的核大小
  - L2gradient : 指定用于计算梯度大小的标志
    - True : $\sqrt{G_x^2 + G_y^2}$
    - False : $|G_x| + |G_y|$
  - edges : 包含边缘结果值的二维数组

##### 示例

```python
import cv2
import numpy as np

img = cv2.imread("../img/sudoku.jpg")

edges = cv2.Canny(img,100,200)

cv2.imshow('Original', img)
cv2.imshow('Canny', edges)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

##### 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_234_0.png)

### 滤波器

1. 卷积滤波器
2. 模糊
3. 边缘检测
4. **形态学**
5. 图像金字塔
6. 实践

#### 形态学操作

- 用于灰度图像
- 消除噪声、简化图像，用于分离部分或填充孔洞
- 结构元素
  - 应用于原始图像的核
  - 四边形、椭圆、十字形
  - np.ones( ) : 四边形
  - cv2.getStructuringElements(shape, ksize[, anchor]) : 四边形以外的形状
    - shape: cv2.MORPH_*
      - RECT, ELLIPSE, CROSS
    - ksize : 核大小
    - anchor : 结构元素的基准点，除十字形外无意义
- 操作类型
  - 腐蚀
  - 膨胀
  - 开运算
  - 闭运算

#### 腐蚀

- 应用结构元素，如果至少有一个像素，则删除
  - 如果无法放置核，则删除
- 对于消除小物体（主要是噪声）有效
- cv2.erode(src, kernel [, anchor, iteration])
  - src : 输入图像
  - kernel : 结构元素
  - anchor : 核的中心点，默认(-1, -1)
  - iteration : 重复次数，默认(1)

![](img/9de0cb7cd12c1232044968590f57e471_236_0.png)

#### ❖ 腐蚀

- 示例

```python
import cv2
import numpy as np

img = cv2.imread('../img/morph_dot.png')

k = cv2.getStructuringElement(cv2.MORPH_RECT, (3,3))
erosion = cv2.erode(img, k)

merged = np.hstack((img, erosion))
cv2.imshow('Erode', merged)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

![](img/9de0cb7cd12c1232044968590f57e471_237_0.png)

#### ❖ 膨胀

- 使用结构元素填充0像素。
- 填充放置核后未覆盖的区域。
- 连接断开部分，对填充孔洞有效。
- cv2.dilation(src, kernel [, anchor, iteration])
    - src : 输入图像
    - kernel : 结构元素
    - anchor : 核的中心点，默认值为(-1, -1)
    - iteration : 重复次数，默认值为(1)

![](img/9de0cb7cd12c1232044968590f57e471_237_1.png)

#### 膨胀

- 示例

```python
import cv2
import numpy as np

img = cv2.imread('../img/morph_hole.png')
k = cv2.getStructuringElement(cv2.MORPH_RECT, (3, 3))
dst = cv2.dilate(img, k)
merged = np.hstack((img, dst))
cv2.imshow('Dilation', merged)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

![](img/9de0cb7cd12c1232044968590f57e471_238_0.png)

#### 开运算与闭运算

- 腐蚀与膨胀的组合
- 开运算：腐蚀 + 膨胀
    - 对消除较亮的噪声有效
    - 分离相邻的独立对象
- 闭运算：膨胀 + 腐蚀
    - 对消除较暗的噪声有效
    - 连接断开部分
- cv2.morphologyEx(src, op, kernel)
    - src : 输入图像
    - op : 操作类型
        - cv2.MORPH_OPEN
        - cv2.MORPH_CLOSE
        - cv2.MORPH_GRADIENT : 膨胀与腐蚀的差值
        - cv2.MORPH_TOPHAT : 开运算与原图的差值
        - cv2.MORPH_BLACKHAT : 闭运算与原图的差值

![](img/9de0cb7cd12c1232044968590f57e471_238_1.png)

![](img/9de0cb7cd12c1232044968590f57e471_238_2.png)

#### ❖ 开运算与闭运算

- 示例

```python
import cv2
import numpy as np

img1 = cv2.imread('../img/morph_dot.png', cv2.IMREAD_GRAYSCALE)
img2 = cv2.imread('../img/morph_hole.png', cv2.IMREAD_GRAYSCALE)

k = cv2.getStructuringElement(cv2.MORPH_RECT, (5,5))
opening = cv2.morphologyEx(img1, cv2.MORPH_OPEN, k)
closing = cv2.morphologyEx(img2, cv2.MORPH_CLOSE, k)

merged1 = np.hstack((img1, opening))
merged2 = np.hstack((img2, closing))
merged3 = np.vstack((merged1, merged2))
cv2.imshow('opening, closing', merged3)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

- 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_239_0.png)

#### 形态学梯度、顶帽、黑帽

- 梯度 = 膨胀 – 腐蚀
    - 仅保留边界线
- 顶帽 = 原图 – 开运算
    - 消除特别亮的区域
- 黑帽 = 闭运算 – 原图
    - 强调较暗的区域

![](img/9de0cb7cd12c1232044968590f57e471_240_0.png)

- 示例

```python
import cv2
import numpy as np

img = cv2.imread('../img/morphological.png')

k = cv2.getStructuringElement(cv2.MORPH_RECT, (3,3))
gradient = cv2.morphologyEx(img, cv2.MORPH_GRADIENT, k)

merged = np.hstack((img, gradient))
cv2.imshow('gradient', merged)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

![](img/9de0cb7cd12c1232044968590f57e471_240_1.png)

#### ❖ 顶帽、黑帽

- 示例

```python
import cv2
import numpy as np

img = cv2.imread('../img/moon_gray.jpg')

k = cv2.getStructuringElement(cv2.MORPH_RECT, (9,9))
tophat = cv2.morphologyEx(img, cv2.MORPH_TOPHAT, k)
blackhat = cv2.morphologyEx(img, cv2.MORPH_BLACKHAT, k)

merged = np.hstack((img, tophat, blackhat))
cv2.imshow('tophat blackhat', merged)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

- 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_241_0.png)

### 滤波器

1. 卷积滤波器
2. 模糊
3. 边缘检测
4. 形态学
5. **图像金字塔**
6. 实践练习

#### 图像金字塔

- 通过逐步缩小和放大图像，像金字塔一样堆叠。
- 速度与精度
    - 使用小图像进行快速分析，并对下一层图像进行详细分析。
- 通过尺寸补偿结果差异。
- 高斯金字塔
    - 下采样/缩放
    - 缩小图像
- 拉普拉斯金字塔
    - 上采样/缩放
    - 放大图像

![](img/9de0cb7cd12c1232044968590f57e471_243_0.png)

#### 高斯金字塔

- cv2.pyrDown(src [, dstsize, borderType])
    - src : 原始图像
    - dstsize : 输出图像的大小
    - 生成金字塔的上层图像
    - 应用高斯核后，删除奇数列和行
    - 缩小到1/4大小
- cv2.pyrUp()
    - 生成金字塔的下层图像
    - 用0填充列和行，与指定滤波器进行卷积并设置
    - 放大到4倍。
    - 变得模糊

- 示例

```python
import cv2

img = cv2.imread('../img/girl.jpg')

smaller = cv2.pyrDown(img) # img x 1/4
bigger = cv2.pyrUp(img) # img x 4

cv2.imshow('img', img)
cv2.imshow('pyrDown', smaller)
cv2.imshow('pyrUp', bigger)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

![](img/9de0cb7cd12c1232044968590f57e471_244_0.png)

#### ❖ 拉普拉斯金字塔

- 小层级放大了原始图像与高斯金字塔之间的差异
- Li = Gi – pyrUp(Gi+1)
- 用于从小金字塔恢复原始图像
- 除边缘外，大部分像素为0（零）

```python
import cv2
import numpy as np

img = cv2.imread('../img/taekwonv1.jpg')

smaller = cv2.pyrDown(img)
bigger = cv2.pyrUp(smaller)

laplacian = cv2.subtract(img, bigger)
restored = bigger + laplacian
merged = np.hstack((img, laplacian, bigger, restored))
cv2.imshow('Laplacian Pyramid', merged)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

#### ❖ 拉普拉斯金字塔

![](img/9de0cb7cd12c1232044968590f57e471_245_0.png)

### 滤波器

1. 卷积滤波器
2. 模糊
3. 边缘检测
4. 形态学
5. 图像金字塔
6. **实践练习**

#### ❖ 马赛克2

- 选择要马赛克的区域
- 结果示例

![](img/9de0cb7cd12c1232044968590f57e471_247_0.png)

- 提示
    - 对选定区域进行均值模糊

#### ❖ 卡通化相机

- 为图像添加卡通或水彩效果。
- 同时输出素描图像和水彩图像。
- 结果示例
- 提示
    - 素描图像：反转为灰度图以获取边缘
        - cv2.Laplacian()
        - cv2.GaussianBlur()
    - 水彩：对彩色图像应用模糊，并将其与素描图像结合
        - cv2.blur()
        - cv2.bitwise_and()

![](img/9de0cb7cd12c1232044968590f57e471_247_1.png)

### 图像分割

1. **轮廓**
2. 霍夫变换
3. 连通组件
4. 实践练习

#### 轮廓

- 连接相同颜色或亮度点的曲线。
- 对图形分析和物体识别有效。
- 为获得更好的准确性，最好进行二值化处理。
    - 阈值，Canny边缘检测
    - 从黑色背景中查找白色物体

![](img/9de0cb7cd12c1232044968590f57e471_249_0.png)

#### 轮廓

- dst, contours, hierarchy = cv2.findContours(img, mode, method)
    - img : 输入图像
    - mode : 结果轮廓模式
        - cv2.RETR_EXTERNAL : 外部边界线
        - cv2.RETR_LIST : 所有线，无层次结构
        - cv2.RETR_CCOMP : 所有线作为2层层次结构
        - cv2.RETR_TREE : 所有线，所有层次信息。树状结构
    - method : 近似方法
        - cv2.CHAIN_APPROX_NONE : 保存所有坐标
        - cv2.CHAIN_APPROX_SIMPLE : 仅保存顶点坐标
        - cv2.CHAIN_APPROX_TC89_L1 : Teh-Chin近似算法
        - cv2.CHAIN_APPROX_TC89_KC0S : Teh-Chin近似算法
    - dst : 编辑后的图像，OpenCV3.2+（在之前的版本中原始图像会被损坏）
    - contours : 检测到的轮廓坐标，Python列表
    - hierarchy : 层次信息。
        - Next, Prev, FirstChild, Parent
        - -1 : 不适用

#### 轮廓

- 沿给定轮廓绘制线条（无论数量多少，显示所有线条）
- cv2.drawContours(img, contours, contourIdx, color, thickness)
    - img : 输入图像
    - contours : Python列表
    - contourIdx : 绘制主体索引，-1：全部
    - color
    - thickness: 线条粗细，0 : 填充
- 示例 < 1/2 >

```python
import cv2
import numpy as np

img = cv2.imread('../img/shapes.png')
img2 = img.copy()
imggray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
ret, imthres = cv2.threshold(imggray, 127, 255, cv2.THRESH_BINARY_INV)

im2, contour, hierarchy = cv2.findContours(imthres, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_NONE)
im2, contour2, hierarchy = cv2.findContours(imthres, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
print('# of figures: %d(%d)' % (len(contour), len(contour2)))
```

#### 轮廓

- 示例 < 2/2 >

```python
cv2.drawContours(img, contour, -1, (0,255,0), 4)
cv2.drawContours(img2, contour2, -1, (0,255,0), 4)

for i in contour:
    for j in i:
        cv2.circle(img, tuple(j[0]), 1, (255,0,0), -1)
for i in contour2:
    for j in i:
        cv2.circle(img2, tuple(j[0]), 1, (255,0,0), -1)

cv2.imshow('CHAIN_APPROX_NONE', img)
cv2.imshow('CHAIN_APPROX_SIMPLE', img2)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

- 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_251_0.png)

#### 轮廓层级

- 示例

```python
import cv2
import numpy as np

img = cv2.imread('../img/shapes_donut.png')
img2 = img.copy()
imggray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
ret, imthres = cv2.threshold(imggray, 127, 255,
cv2.THRESH_BINARY_INV)

im2, contour, hierarchy = cv2.findContours(imthres,
cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_NONE)
print(len(contour), hierarchy)

im2, contour2, hierarchy = cv2.findContours(imthres, cv2.RETR_TREE,
cv2.CHAIN_APPROX_SIMPLE)
print(len(contour2), hierarchy)

cv2.drawContours(img, contour, -1, (0,255,0), 3)
for idx, cont in enumerate(contour2):
    color = [int(i) for i in np.random.randint(0,255, 3)]
    cv2.drawContours(img2, contour2, idx, color, 3)
    cv2.putText(img2, str(idx), tuple(cont[0][0]),
cv2.FONT_HERSHEY_PLAIN, 1, (0,0,255))

cv2.imshow('RETR_EXTERNAL', img)
cv2.imshow('RETR_TREE', img2)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

- 示例 <结果>

```
3 [[[ 1 -1 -1 -1]
  [ 2  0 -1 -1]
  [-1  1 -1 -1]]]
6 [[[ 2 -1  1 -1]
  [-1 -1 -1  0]
  [ 4  0  3 -1]
  [-1 -1 -1  2]
  [-1  2  5 -1]
  [-1 -1 -1  4]]]
```

![](img/9de0cb7cd12c1232044968590f57e471_253_0.png)

#### 图像矩

- 矩
  - 物理学中，力矩的术语
- 图像矩
  - 显示物体的数量
    - $m_{p,q} = \sum_{x} \sum_{y} f(x, y) x^p y^q$
  - 轮廓所包围的面积；x,y坐标像素值与坐标指数p,q的乘积之和
  - 二值图像：如果不是0，则为1
- p, q 阶：通过从0到3变化计算，获取有意义的信息
- m00：轮廓的面积
  - 所有数字的0阶为1，通过乘以1得到面积数量
- 中心矩
  - $\mu_{p,q} = \sum_{x} \sum_{y} f(x,y)(x - \bar{x})^p(y - \bar{y})^q$
    - $\bar{x} = \frac{m_{10}}{m_{00}}$
    - $\bar{y} = \frac{m_{01}}{m_{00}}$

##### ❖ 图像矩

- moment = cv2.moments(contour)
  - contour：计算矩的坐标对象
  - moment：结果矩，Python字典
    - m00, m01, m10, m11, m02, m12, m20, m21, m03, m30：空间矩
    - mu20, mu11, mu02, mu30, mu21, mu12, mu03：中心矩
    - nu20, nu11, nu02, nu30, nu21, nu03：归一化中心矩
- retval = cv.contourArea(contour[, oriented=False])：计算轮廓面积
  - contour：要计算面积的轮廓
  - oriented：轮廓方向标志
    - True：根据轮廓方向返回负值
    - False：返回绝对值
  - retval：轮廓的面积
- retval = cv.arcLength( curve, closed )：计算周长
  - curve：要计算的轮廓
  - closed：是否闭合标志
  - retval：轮廓周长值

##### ❖ 图像矩

- 中心点、面积、周长示例 < 1/2 >

```python
import cv2
import numpy as np
img = cv2.imread("../img/shapes.png")
imggray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
ret, th = cv2.threshold(imggray, 127, 255, cv2.THRESH_BINARY_INV)
img2, contours, hierachy = cv2.findContours(th, cv2.RETR_EXTERNAL,
cv2.CHAIN_APPROX_SIMPLE)
for c in contours:
    mmt = cv2.moments(c)
    cx = int(mmt['m10']/mmt['m00'])
    cy = int(mmt['m01']/mmt['m00'])
    a = mmt['m00']
    l = cv2.arcLength(c, True)
    cv2.circle(img, (cx, cy), 5, (0, 255, 255), -1)
    cv2.putText(img, "A:%.0f"%a, (cx, cy+20) ,
cv2.FONT_HERSHEY_PLAIN, 1, (0,0,255))
    cv2.putText(img, "L:%.2f"%l, tuple(c[0][0]),
cv2.FONT_HERSHEY_PLAIN, 1, (255,0,0))
    print("area:%.2f"%cv2.contourArea(c, False))
cv2.imshow('center', img)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

- 中心点、面积、周长示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_255_0.png)

##### ❖ 边界形状

- x,y,w,h = cv2.boundingRect(contour)：计算包围四边形的坐标
  - x, y：四边形左上角坐标
  - w, h：宽度、高度
- rotateRect = cv2.minAreaRect(contour)：最小包围四边形的坐标
  - rotateRect：旋转四边形的坐标
    - center：中心点 (x,y)
    - size：尺寸 (w, h)
    - angle：旋转角度，正值：顺时针，负值：逆时针
- vertex = cv2.boxPoints(rotateRect)：从rotateRect获取顶点坐标
  - vertex：4个顶点，包含小数，需要转换为整数
- center, radius = cv2.minEnclosingCircle(contour)：
  - 最小包围圆的坐标
  - center：原始坐标 (x, y)，元组
  - radius：半径
- area, triangle = cv.minEnclosingTriangle(points)：最小包围三角形的坐标
  - area：面积
  - triangle：3个顶点的坐标
- ellipse = cv.fitEllipse( points )：最小包围椭圆的坐标
  - ellipse
    - center：中心坐标 (x,y)，元组
    - axes：轴长 (x, y)，元组
    - angle：旋转角度
- line = cv.fitLine( points, distType, param, reps, aeps[, line] )：计算通过中心点的直线
  - distType：距离计算方式
    - cv2.DIST_L2, cv2.DIST_L1, cv2.DIST_L12, cv2.DIST_FAIR, cv2.DIST_WELSCH, cv2.DIST_HUBER
  - param：传递给distType的参数，0=选择最优值
  - reps：半径精度，直线与原始坐标之间的距离，建议0.01
  - aeps：角度精度，建议0.01
  - line
    - vx, vy：归一化单位向量，直线的斜率，元组
    - x0, y0：中心点，坐标，元组

##### ❖ 边界形状

- 示例 < 1/2 >

```python
import cv2
import numpy as np

img = cv2.imread("../img/lightning.png")
imggray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
ret, th = cv2.threshold(imggray, 127,255,cv2.THRESH_BINARY_INV)

im, contours, hr = cv2.findContours(th, cv2.RETR_EXTERNAL,
cv2.CHAIN_APPROX_SIMPLE)
contr = contours[0]

x,y,w,h = cv2.boundingRect(contr)
cv2.rectangle(img, (x,y), (x+w, y+h), (0,0,0), 3)

rect = cv2.minAreaRect(contr)
box = cv2.boxPoints(rect)    # 将中心点和角度转换为4个顶点坐标
box = np.int0(box)           # 转换为整数
cv2.drawContours(img, [box], -1, (0,255,0), 3)
```

- 示例 < 2/2 >

```python
(x,y), radius = cv2.minEnclosingCircle(contr)
cv2.circle(img, (int(x), int(y)), int(radius), (255,0,0), 2)

ret, tri = cv2.minEnclosingTriangle(contr)
cv2.polylines(img, [np.int32(tri)], True, (255,0,255), 2)

ellipse = cv2.fitEllipse(contr)
cv2.ellipse(img, ellipse, (0,255,255), 3)

[vx,vy,x,y] = cv2.fitLine(contr, cv2.DIST_L2,0,0.01,0.01)
cols,rows = img.shape[:2]
cv2.line(img,(0, 0-x*(vy/vx) + y), (cols-1, (cols-x)*(vy/vx) + y),(0,0,255),2)

cv2.imshow('Bound Fit shapes', img)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

- 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_258_0.png)

#### 轮廓近似

- 当精确轮廓不准确时
  - 噪声、腐蚀
- Douglas-Peucker算法
  - 设置epsilon值：轮廓到近似轮廓的最大距离
- approx = cv2.approxPolyDP(contour, epsilon, closed)
  - contour：轮廓坐标
  - epsilon：近似值的精度，误差范围
  - closed：轮廓是否闭合
  - approx：近似轮廓的坐标

![](img/9de0cb7cd12c1232044968590f57e471_259_0.png)

- 示例

```python
import cv2
import numpy as np

img = cv2.imread('../img/bad_rect.png')
img2 = img.copy()
imggray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
ret, th = cv2.threshold(imggray, 127, 255, cv2.THRESH_BINARY)
temp, contours, hierachy = cv2.findContours(th, cv2.RETR_EXTERNAL,
    cv2.CHAIN_APPROX_SIMPLE)
contour = contours[0]
epsilon = 0.05 * cv2.arcLength(contour, True)
approx = cv2.approxPolyDP(contour, epsilon, True)
cv2.drawContours(img, [contour], -1, (0,255,0), 3)
cv2.drawContours(img2, [approx], -1, (0,255,0), 3)
cv2.imshow('contour', img)
cv2.imshow('approx', img2)
cv2.waitKey()
cv2.destroyAllWindows()
```

#### 轮廓近似

- 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_260_0.png)

#### 凸包

- 凸包：没有凹陷部分，由凸曲线构成的物体
- 物体边界区域的坐标
- 在寻找物体的实际面积和中心点方面很有效
- `hull = cv2.convexHull(points, hull, clockwise, returnPoints)`
  - `points`：轮廓
  - `hull`：输出，避免使用
  - `clockwise`：设置方向，True/False
  - `returnPoints`：
    - True*：凸包点的环绕坐标
    - False：给定轮廓索引的环绕凸包
- `ret = cv2.isContourConvex(contour)`：True/False
  - `ret`：检查是否为凸形
- `defects = cv2.convexityDefects(contour, convexhull)`：寻找凸包的缺陷
  - `contour`：输入轮廓
  - `convexhull`：凸形物体的轮廓索引
  - `defects`：带有凸包缺陷的轮廓数组索引，N x 1 x 4 数组
  - `[start, end, farthest, distance]`
    - `start`：凸角起始点的轮廓索引
    - `end`：凸角结束点的轮廓索引
    - `farthest`：距离凸包最远点的轮廓索引
    - `distance`：最远点到凸包的距离
      - 8位定点小数（距离/256.0）

#### ❖ 凸包

- 示例

```python
import cv2
import numpy as np

img = cv2.imread('../img/hand.jpg')
img2 = img.copy()
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
ret, th = cv2.threshold(gray, 127, 255, cv2.THRESH_BINARY_INV)

temp, contours, heiarcy = cv2.findContours(th, cv2.RETR_EXTERNAL,
    cv2.CHAIN_APPROX_SIMPLE)
cntr = contours[0]
cv2.drawContours(img, [cntr], -1, (0, 255,0), 1)

hull = cv2.convexHull(cntr)
cv2.drawContours(img2, [hull], -1, (0,255,0), 1)
print(cv2.isContourConvex(cntr), cv2.isContourConvex(hull))
hull2 = cv2.convexHull(cntr, returnPoints=False)
defects = cv2.convexityDefects(cntr, hull2)
for i in range(defects.shape[0]):
    startP, endP, farthestP, distance = defects[i, 0]
    farthest = tuple(cntr[farthestP][0])
    dist = distance/256.0
    if dist > 1 :
        cv2.circle(img2, farthest, 3, (0,0,255), -1)
cv2.imshow('contour', img)
cv2.imshow('convex hull', img2)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

#### 凸包

- 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_262_0.png)

#### 点多边形测试

- 计算图像中某点到轮廓的距离
  - `cv2.pointPolygonTest(img, pt, measureDist)`
    - `img`：输入图像
    - `pt`：(x,y)，测量点
    - `measureDist`：True/False
      - True：距离测量
      - False：仅显示内部和外部（-1,0,1）
  - 返回值
    - 正数：内部
    - 负数：外部

#### 点多边形测试

- 示例

```python
img = cv2.imread('../img/4star.jpg')
imggray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
ret, th = cv2.threshold(imggray, 127,255, cv2.THRESH_BINARY_INV)
_, contours, _ = cv2.findContours(th, cv2.RETR_EXTERNAL,
                                cv2.CHAIN_APPROX_SIMPLE)

contour = contours[0]
cv2.drawContours(img, [contour], 0, (0,255,0),2)
p1 = (100,100)
p2 = (200,200)
cv2.circle(img, p1, 3, (255,0,0), -1)
cv2.circle(img, p2, 3, (255,0,255), -1)
dist1 = cv2.pointPolygonTest(contour, p1, True)
dist2 = cv2.pointPolygonTest(contour, p2, True)
cv2.putText(img, '%f'%dist1, p1, cv2.FONT_HERSHEY_PLAIN, 1,
            (0,0,255), 1)
cv2.putText(img, '%f'%dist2, p2, cv2.FONT_HERSHEY_PLAIN, 1,
            (0,0,255), 1)
cv2.imshow('Convex defects', img)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

- 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_263_0.png)

#### ❖ 形状匹配

- 比较两个不同的轮廓
- `ret = cv2.matchShape(cntr1, cntr2, method, param)`
  - `cntr1, cntr2`：要比较的轮廓
  - `method`：Hu矩比较算法
    - `cv2.CONTOURS_MATCH_I1`
    - `cv2.CONTOURS_MATCH_I2`
    - `cv2.CONTOURS_MATCH_I3`
  - `param`：传递给算法的参数，0.0（不支持）
  - `ret`：
    - 相似度：值越小，越相似，0=相同
- 示例 <1/2>

```python
import cv2
import numpy as np

target = cv2.imread('../img/4star.jpg') # 매칭 대상
shapes = cv2.imread('../img/shapestomatch.jpg') # 여러 도형
targetGray = cv2.cvtColor(target, cv2.COLOR_BGR2GRAY)
shapesGray = cv2.cvtColor(shapes, cv2.COLOR_BGR2GRAY)
ret, targetTh = cv2.threshold(targetGray, 127, 255,
cv2.THRESH_BINARY_INV)
ret, shapesTh = cv2.threshold(shapesGray, 127, 255,
cv2.THRESH_BINARY_INV)

_, cntrs_target, _ = cv2.findContours(targetTh, cv2.RETR_EXTERNAL,
cv2.CHAIN_APPROX_SIMPLE)
_, cntrs_shapes, _ = cv2.findContours(shapesTh, cv2.RETR_EXTERNAL,
cv2.CHAIN_APPROX_SIMPLE)
```

#### 形状匹配

- 示例 <2/2>

```python
matchs = [] # List to save contour and matching points
for contr in cntrs_shapes:
    match = cv2.matchShapes(cntrs_target[0], contr,
    cv2.CONTOURS_MATCH_I2, 0.0)
    matchs.append( (match, contr) )
    cv2.putText(shapes, '%.2f'%match, tuple(contr[0][0]),
    cv2.FONT_HERSHEY_PLAIN, 1,(0,0,255),1 )
matchs.sort(key=lambda x : x[0])

cv2.drawContours(shapes, [matchs[0][1]], -1, (0,255,0), 3)
cv2.imshow('target', target)
cv2.imshow('Match Shape', shapes)
cv2.waitKey()
cv2.destroyAllWindows()
```

- 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_265_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_265_1.png)

### 图像分割

1. 轮廓
2. **霍夫变换**
3. 连通组件
4. 工作坊

#### 霍夫变换

- 区分直线、圆形等简单形状
- 从寻找直线的方法扩展到各种图形
- Paul Hough 首次发现（气泡室），1962年
- Richard Duda 和 Peter Hart 推广，1972年
- 通过 Dana H. Ballard 的《计算机视觉》普及，1981年
- 霍夫直线变换
- 霍夫圆变换
- 从图像中众多点中检测直线
- r = cos θx + sin θy
- 对于一个点 (r, θ)
- 连接所有具有相同 (r, θ) 的点的直线

![](img/9de0cb7cd12c1232044968590f57e471_267_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_267_1.png)

#### ❖ 霍夫直线变换

- `lines = cv2.HoughLines(img, rho, theta, threshold[, lines, srn=0, stn=0, min_theta, max_theta])`
  - `img`：输入图像，1通道二值图像
  - `rho`：距离分辨率，0~1
  - `theta`：角度分辨率，弧度单位（np.pi/0~180）
  - `threshold`：判断为直线的最小匹配数
    - 小值：精度下降，但检测到的直线数量增加
    - 大值：精度提高，但检测到的直线数量减少
  - `lines`：检测结果，N x 1 x 2 数组 (r,θ)
  - `srn, stn`：用于多尺度霍夫变换，检测中不使用
  - `min_theta, max_theta`：用于检测的最大和最小角度
- 示例

```python
import cv2
import numpy as np

img = cv2.imread('../img/sudoku.jpg')
img2 = img.copy()
h, w = img.shape[:2]
imggray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
edges = cv2.Canny(imggray, 100, 200 )
lines = cv2.HoughLines(edges, 1, np.pi/180, 130)
for line in lines: # Tour all detected lines
    r,theta = line[0] # Distance and angle
    tx, ty = np.cos(theta), np.sin(theta) # Trigonometric ration to x, y
    x0, y0 = gx*r, gy*r  #Coordination based on x, y
    cv2.circle(img2, (abs(x0), abs(y0)), 3, (0,0,255), -1)
    x1, y1 = int(x0 + w*(-ty)), int(y0 + h * tx)
    x2, y2 = int(x0 - w*(-ty)), int(y0 - h * tx)
    cv2.line(img2, (x1, y1), (x2, y2), (0,255,0), 1)
merged = np.hstack((img, img2))
cv2.imshow('hough line', merged)
cv2.waitKey()
cv2.destroyAllWindows()
```

#### ❖ 霍夫直线变换

- 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_269_0.png)

#### ❖ 概率霍夫直线变换

- 避免计算所有像素的低效性
- 随机选择像素
- `lines = cv2.HoughLineP(src, rho, theta, threshold, minLineLen, maxLineGap)`
  - `src`：输入图像
  - `rho`：距离精度（0~1）
  - `theta`：角度精度，弧度（np.pi/0~180）
  - `threshold`：找到直线的最小匹配值
  - `minLineLen`：如果长度小于此值，则丢弃
  - `maxLineGap`：如果距离大于此值，则视为不同的直线
  - `lines`：检测到的直线坐标
    - 用于直线中两条线的 nx1x4 数组 (x1, y1, x2, y2)

#### ❖ 概率霍夫线变换

- 示例

```python
import cv2
import numpy as np

img = cv2.imread('../img/sudoku.jpg')
img2 = img.copy()
imgray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
edges = cv2.Canny(imgray, 100, 200)

lines = cv2.HoughLinesP(edges, 1, np.pi/180, 50, 5, 10)
for line in lines:
    x1, y1, x2, y2 = line[0]
    cv2.line(img, (x1, y1), (x2, y2), (0, 255, 0), 1)

cv2.imshow('Probability hough line', img)
cv2.waitKey()
cv2.destroyAllWindows()
```

- 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_270_0.png)

#### ❖ 霍夫圆变换

- 将直角坐标系转换为极坐标系
- 能够使用直线变换算法检测圆形
- 计算量过大，速度慢

- 霍夫梯度法
  - OpenCV 操作
  - 使用 Canny 检测边缘
  - 使用 Sobel 计算梯度方向
  - 计算圆心和半径，使用 3D 累加器
  - 由于使用 Sobel 获取梯度方向，因此不准确
  - 难以检测重叠的同心圆

- `circles = cv2.HoughCircles(src, method, dp, minDst, circles, param1, param2, minRds, maxRds)`
  - `src` : 输入图像
  - `method` : `cv2.HOUGH_GRADIENT`，唯一可用方法
    - `cv.HOUGH_STANDARD`
    - `cv.HOUGH_PROBABILISTIC`
    - `cv.HOUGH_MULTI_SCALE`
    - `cv.HOUGH_GRADIENT`
  - `dp` : 累加器与输入图像之间的分辨率反比
    - 1: 相同，2: ½，小值：提高精度，
    - 大值：增加检测数量
  - `minDst` : 圆心之间的最小值，0=错误，无法检测同心圆
  - `circles` : 圆形结果，N x 1 x 3 (x, y, r)
  - `param1` : 用于 Canny 边缘检测的最大值
  - `param2` : HOUGH_GRADIENT 阈值，
    - 小值：增加检测数量，降低精度
  - `minRadius`, `maxRadius`：圆的最小半径，最大半径（如果为 0，则为图像大小）

#### ❖ 霍夫圆变换

- 示例

```python
import cv2
import numpy as np

img = cv2.imread('../img/coins_spread1.jpg')
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
blur = cv2.GaussianBlur(gray, (3, 3), 0)
circles = cv2.HoughCircles(blur, cv2.HOUGH_GRADIENT, 1.5, 30, None, 200)
if circles is not None:
    circles = np.uint16(np.around(circles))
    for i in circles[0, :]:
        cv2.circle(img, (i[0], i[1]), i[2], (0, 255, 0), 2)
        cv2.circle(img, (i[0], i[1]), 2, (0, 0, 255), 5)

cv2.imshow('hough circle', img)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

- 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_272_0.png)

### 图像分割

1. 轮廓
2. 霍夫变换
3. **连通域**
4. 实践

#### 距离变换

- 寻找物体的中心点
- 距离边界最远
- 适用于二值图像
- 从像素值为 0 的位置开始，随着距离 0 的增加而增加 1
- `cv2.distanceTransform(src, distanceType, maskSize)`
  - `src` : 输入图像，二值图像
  - `distanceType` : 选择距离计算类型
    - `cv2.DIST_L2`, `cv2.DIST_L1`, `cv2.DIST_L12`, `cv2.DIST_FAIR`, `cv2.DIST_WELSCH`, `cv2.DIST_HUBER`
  - `maskSize` : 距离变换核大小

![](img/9de0cb7cd12c1232044968590f57e471_274_0.png)

- 示例

```python
import cv2
import numpy as np

img = cv2.imread('../img/full_body.jpg', cv2.IMREAD_GRAYSCALE)
_, biimg = cv2.threshold(img, 127, 255, cv2.THRESH_BINARY_INV)

dst = cv2.distanceTransform(biimg, cv2.DIST_L2, 5)
dst = (dst / (dst.max() - dst.min()) * 255).astype(np.uint8)
skeleton = cv2.adaptiveThreshold(dst, 255,
    cv2.ADAPTIVE_THRESH_GAUSSIAN_C, cv2.THRESH_BINARY, 7, -3)
cv2.imshow('origin', img)
cv2.imshow('dist', dst)
cv2.imshow('skel', skeleton)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

- 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_275_0.png)

#### ❖ 连通域标记

- 分离相连的区域
- 对于像素值未被 0 打断的区域，赋予相同的标签值
- `retval, labels = cv.connectedComponents(src[, labels, connectivity=8, ltype])` : 返回连通域标记和数量
  - `src` : 输入图像，二值图像
  - `labels` : 与输入图像大小相同的数组，用于存储标记结果
  - `connectivity` : 连通性检查的方向数，可选 4 或 8
  - `ltype` : 结果标签数组的数据类型
  - `retval` : 标签数量
- `retval, labels, stats, centroids = cv.connectedComponentsWithStats(src[, labels, stats, centroids, connectivity, ltype])` : 返回标记和各种状态信息
  - `stats` : N x 5 矩阵（N：标签数量）
    - [x坐标, y坐标, 宽度, 高度, 面积]
  - `centroids` : 每个标签的中心点坐标，N x 2 矩阵（N：标签数量）

![](img/9de0cb7cd12c1232044968590f57e471_275_1.png)

#### ❖ 连通域标记

- 示例

```python
import cv2
import numpy as np

img = cv2.imread('../img/shapes_donut.png')
img2 = np.zeros_like(img)
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
_, th = cv2.threshold(gray, 127, 255, cv2.THRESH_BINARY)

cnt, labels = cv2.connectedComponents(th)
##### retval, labels, stats, cent = cv2.connectedComponentsWithStats(th)
##### ---②

for i in range(cnt):
    img2[labels == i] = [int(j) for j in np.random.randint(0, 255, 3)]

cv2.imshow('origin', img)
cv2.imshow('labeled', img2)
```

- 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_276_0.png)

#### ❖ 漫水填充

- 为连续区域填充相同颜色
- `retval, img, mask, rect = cv2.floodFill(img, mask, seed, newVal[, loDiff, upDiff, flags])`
  - `img` : 输入图像，1 或 3 通道
  - `mask` : 比输入图像大 2x2 像素，如果遇到非 0 区域则停止填充
  - `seed` : 开始填充的坐标
  - `newVal` : 用于填充的颜色值
  - `loDiff`, `upDiff` : 用于决定是否填充的最小和最大值
  - `flags` : 填充类型的标志，
    - 4 或 8 距离填充
    - `cv2.FLOODFILL_MASK_ONLY` : 仅填充掩码，不填充图像
      - 需要包含 8-16 位用于填充的值
    - `cv2.FLOODFILL_FIXED_RANGE`：与种子像素比较，而不是相邻像素
- `retval`：填充的像素数量
- `rect` : 包围填充区域的矩形

- 示例

```python
import cv2
import numpy as np

img = cv2.imread('../img/taekwonv1.jpg')
rows, cols = img.shape[:2]
mask = np.zeros((rows + 2, cols + 2), np.uint8)
newVal = (255, 255, 255)
loDiff, upDiff = (10, 10, 10), (10, 10, 10)

def onMouse(event, x, y, flags, param):
    global mask, img
    if event == cv2.EVENT_LBUTTONDOWN:
        seed = (x, y)
        retval = cv2.floodFill(img, mask, seed, newVal, loDiff, upDiff)
        cv2.imshow('img', img)

cv2.imshow('img', img)
cv2.setMouseCallback('img', onMouse)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

> - $src(x', y') - loDiff \leq src(x, y) \leq src(x', y') + upDiff$
>   - $src(x, y)$ : 当前像素
>   - $src(x', y')$ : 相邻像素

#### ❖ 漫水填充

- 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_278_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_278_1.png)

#### ❖ 分水岭算法

- 通过将边缘平衡区域视为山谷来分割主体
- 由用户（或算法）指定标记以进行分割
- 向每个不同的标记区域注水，当它们相遇时停止。
- 标记
  - 为每个不同的对象应用 ID（1~），标记主体以进行区分
  - 0 : 未知
  - 例如）前景图像=1，背景=2
  - 例如）橙子=1，青柠=2，背景=3
- `markers = cv2.watershed(img, markers)`
  - `img` : 输入图像
  - `markers`
    - 标记边界设置为 -1，与输入图像大小相同

#### ❖ 分水岭算法

- 示例

```python
import cv2
import numpy as np

img = cv2.imread('../img/taekwonv1.jpg')
rows, cols = img.shape[:2]
img_draw = img.copy()
marker = np.zeros((rows, cols), np.int32)
markerId = 1
colors = []
isDragging = False

def onMouse(event, x, y, flags, param):
    global img_draw, marker, markerId, isDragging
    if event == cv2.EVENT_LBUTTONDOWN:  # 按下鼠标左键，开始拖动
        isDragging = True
        colors.append((markerId, img[y, x]))
    elif event == cv2.EVENT_MOUSEMOVE:  # 鼠标移动
        if isDragging:            # 正在拖动
            marker[y, x] = markerId
            cv2.circle(img_draw, (x, y), 3, (0, 0, 255), -1)
            cv2.imshow('watershed', img_draw)
    elif event == cv2.EVENT_LBUTTONUP:  # 释放鼠标左键
        if isDragging:
            isDragging = False    # 停止拖动
            markerId += 1
    elif event == cv2.EVENT_RBUTTONDOWN: # 点击鼠标右键
        cv2.watershed(img, marker)
        img_draw[marker == -1] = (0, 255, 0)
        for mid, color in colors: # 根据标记 ID 数量重复
            img_draw[marker == mid] = color
        cv2.imshow('watershed', img_draw) # 打印标记结果

cv2.imshow('watershed', img)
cv2.setMouseCallback('watershed', onMouse)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

#### ❖ 分水岭算法

- 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_280_0.png)

#### ❖ Grabcut 算法

- 基于图割算法进行扩展
- 假设前景主体的颜色分布，以分离前景图像
- 在具有相同标签的连通区域中，分离前景图像和背景
- `mask, bgdModel, fgdModel = cv2.grabCut(img, mask, rect, bgdModel, fgdModel, iterCount[, mode])`
- `img`：输入图像
- `mask`：单通道数组，与输入图像大小相同，用于保存区分背景和前景的值
    - `cv2.GC_BGD`：绝对背景 (0)
    - `cv2.GC_FGD`：绝对前景 (1)
    - `cv2.GC_PR_BGD`：可能是背景 (2)
    - `cv2.GC_PR_FGD`：可能是前景 (3)
- `rect`：假设前景区域的矩形坐标，元组 (x1, y1, x2, y2)
- `bgdModel`, `fgdModel`：函数内部使用的临时数组缓冲区，重复使用时请勿编辑
- `iterCount`：迭代次数
- `mode`：
    - `cv2.GC_INIT_WITH_RECT`：基于 `rect` 中设置的坐标进行 Grabcut
    - `cv2.GC_INIT_WITH_MASK`：基于 `mask` 中设置的坐标进行 Grabcut
    - `cv2.GC_EVAL`：重新尝试

#### Grabcut

- 示例 <1/2>

```python
import cv2
import numpy as np
img = cv2.imread('../img/taekwonv1.jpg')
img_draw = img.copy()
mask = np.zeros(img.shape[:2], dtype=np.uint8)
rect = [0,0,0,0]
mode = cv2.GC_EVAL
bgdmodel = np.zeros((1,65),np.float64)
fgdmodel = np.zeros((1,65),np.float64)
def onMouse(event, x, y, flags, param):
    global mouse_mode, rect, mask, mode
    if event == cv2.EVENT_LBUTTONDOWN :
        if flags <= 1: # If no key input
            mode = cv2.GC_INIT_WITH_RECT
            rect[:2] = x, y
    elif event == cv2.EVENT_MOUSEMOVE and flags &
    cv2.EVENT_FLAG_LBUTTON :
        if mode == cv2.GC_INIT_WITH_RECT:
            img_temp = img.copy()
            cv2.rectangle(img_temp, (rect[0], rect[1]), (x, y), (0,255,0), 2)
            cv2.imshow('img', img_temp)
        elif flags > 1:
            mode = cv2.GC_INIT_WITH_MASK
            if flags & cv2.EVENT_FLAG_CTRLKEY :
                cv2.circle(img_draw,(x,y),3, (255,255,255),-1)
                cv2.circle(mask,(x,y),3, cv2.GC_FGD,-1)
```

#### ❖ Grabcut

- 示例 <2/2>

```python
if flags & cv2.EVENT_FLAG_SHIFTKEY :
    cv2.circle(img_draw,(x,y),3, (0,0,0),-1)
    cv2.circle(mask,(x,y),3, cv2.GC_BGD,-1)
    cv2.imshow('img', img_draw)
elif event == cv2.EVENT_LBUTTONUP:
    if mode == cv2.GC_INIT_WITH_RECT :
        rect[2:] =x, y
    cv2.rectangle(img_draw, (rect[0], rect[1]), (x, y), (255,0,0), 2)
    cv2.imshow('img', img_draw)
cv2.grabCut(img, mask, tuple(rect), bgdmodel, fgdmodel, 1, mode)
    img2 = img.copy()
    img2[(mask==cv2.GC_BGD) | (mask==cv2.GC_PR_BGD)] = 0
    cv2.imshow('grabcut', img2)
    mode = cv2.GC_EVAL
cv2.imshow('img', img)
cv2.setMouseCallback('img', onMouse)
while True:
    if cv2.waitKey(0) & 0xFF == 27 : # esc
        break
cv2.destroyAllWindows()
```

- 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_282_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_282_1.png)

#### ❖ 均值漂移滤波器

- 它是一个具有特定边界大小的核窗口，将中心移动到平均像素值
- 通过将起点和终点绑定为一个来查找连通区域
- 将连通区域的颜色值更改为最频繁的颜色值
- `dst = cv.pyrMeanShiftFiltering(src, sp, sr[, dst, maxLevel, termcrit])`
    - `src`：输入图像
    - `sp`：空间窗口半径
    - `sr`：颜色窗口半径
    - `maxLevel`：图像金字塔的最大级别
    - `termcrit`：均值漂移终止标准
- 示例

```python
import cv2
import numpy as np

img = cv2.imread('../img/taekwonv1.jpg')
def onChange(x):
    sp = cv2.getTrackbarPos('sp', 'img')
    sr = cv2.getTrackbarPos('sr', 'img')
    lv = cv2.getTrackbarPos('lv', 'img')
    mean = cv2.pyrMeanShiftFiltering(img, sp, sr, None, lv)
    cv2.imshow('img', np.hstack((img, mean)))

cv2.imshow('img', np.hstack((img, img)))
cv2.createTrackbar('sp', 'img', 0,100, onChange)
cv2.createTrackbar('sr', 'img', 0,100, onChange)
cv2.createTrackbar('lv', 'img', 0,5, onChange)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

#### ❖ 均值漂移滤波器

- 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_284_0.png)

### 图像分割

1. 轮廓
2. 霍夫变换
3. 连通域
4. **实践工作坊**

#### 形状识别

- 编写一个程序，获取包含五个图形的图像中图形的名称。
- 结果示例

![](img/9de0cb7cd12c1232044968590f57e471_286_0.png)

- 提示
    - 查找轮廓，并通过将其简化为近似值来计算顶点数量

#### 文档扫描器

- 起草一个无需鼠标输入即可产生扫描效果的程序
- 结果示例

![](img/9de0cb7cd12c1232044968590f57e471_286_1.png)

- 提示
    - 获取 4 个顶点
        - 使用 canny 边缘检测检测边界
            - 使用 findContour() 查找最大轮廓，并使用 approxPolyDP() 进行简化

#### ❖ 硬币计数器

- 编写一个分离并计数硬币的程序。
- 结果示例

![](img/9de0cb7cd12c1232044968590f57e471_287_0.png)

- 提示
    - pyrMeanShiftFiltering() 模糊硬币表面
    - Otsu 阈值
    - distanceTransform() 硬币中心点
    - 查找局部最大值
    - floodfill() 将局部最大值作为种子
    - 使用 distanceTransform 确定硬币的绝对前景
    - 使用 distanceTransform 确定硬币的绝对背景
    - 绝对背景 – 绝对前景 = 未知区域
    - connectComponents() 标记
    - watershed()
    - 使用 findContour(), boundingRect() 分离硬币区域

### 匹配与跟踪

1. **相似图像匹配**
2. 特征与关键点
3. 描述符提取器
4. 特征匹配
5. 跟踪
6. 实践工作坊

#### 平均哈希匹配

- 适用于理解相似图像
- 平均哈希
    - 通过计算平均值将图像转换为一个数字
        - 无论宽高比如何，将图像缩小到特定尺寸
        - 计算所有像素的平均值并进行比较
        - `px > Average? 1 : 0`
        - 将其转换为一行，并将其转换为一个二进制数
        - 可以转换为十六进制
- 相似距离
    - 测量两个值之间的距离，如果数字小，则相似度高
    - 欧几里得距离
        - 将数字之间的差异作为距离
        - 例如) 5:3 = 2, 5:8=3
    - 汉明距离
        - 显示两个数字之间不同位数的数量
            - 12345:12354=2, 12345:92345=1
- 示例

```python
import cv2

img = cv2.imread('../img/pistol.jpg')
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
gray = cv2.resize(gray, (8,8))
avg = gray.mean()
bin = 1 * (gray > avg)
print(bin)
dhash = []
for row in bin.tolist():
    s = ''.join([str(i) for i in row])
    dhash.append('%02x'%(int(s,2)))
dhash = ''.join(dhash)
print(dhash)
cv2.imshow('pistol', img)
cv2.waitKey(0)
```

#### 平均哈希匹配

```
[[1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1]
 [1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0]
 [1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0]
 [1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0]
 [1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0]
 [1 0 0 0 0 0 0 1 0 0 1 1 1 1 1 1]
 [1 1 0 0 0 0 0 1 1 1 1 1 1 1 1 1]
 [1 1 0 0 0 0 0 1 1 1 1 1 1 1 1 1]
 [1 1 0 0 0 0 0 0 0 1 1 1 1 1 1 1]
 [1 1 0 0 0 0 1 1 1 1 1 1 1 1 1 1]
 [1 1 0 0 0 1 1 1 1 1 1 1 1 1 1 1]
 [1 1 0 0 0 1 1 1 1 1 1 1 1 1 1 1]
 [1 0 0 0 0 1 1 1 1 1 1 1 1 1 1 1]
 [1 0 0 0 0 1 1 1 1 1 1 1 1 1 1 1]
 [1 0 0 0 0 1 1 1 1 1 1 1 1 1 1 1]
 [1 1 0 0 0 1 1 1 1 1 1 1 1 1 1 1]]
ffff8000800080008000813fc1ffc1ffc07fc3ffc7ffc7ff87ff87ff87ffc7ff
```

![](img/9de0cb7cd12c1232044968590f57e471_290_0.png)

- 101 图像数据集
    - 美国加州理工学院
    - 包含 101 个对象的图像集
    - http://www.vision.caltech.edu/Image_Datasets/Caltech101/#Download

#### Caltech 101

[描述 ][ 下载 ][ 讨论 [其他数据集]]

![](img/9de0cb7cd12c1232044968590f57e471_290_1.png)

描述
属于 101 个类别的对象图片。每个类别约有 40 到 800 张图像。大多数类别约有 50 张图像。由李飞飞、Marco Andreetto 和 Marc 'Aurelio Ranzato 于 2003 年 9 月收集。每张图像的大小约为 300 x 200 像素。我们已仔细点击了这些图片中每个对象的轮廓，这些内容包含在 'Annotations.tar' 中。还有一个用于查看注释的 matlab 脚本 'show_annotations.m'

#### 平均哈希匹配

- 寻找枪支图像

```python
import cv2
import numpy as np
import glob

img = cv2.imread('../img/pistol.jpg')
cv2.imshow('query', img)
search_dir = '../img/101_ObjectCategories'

def img2hash(img):
    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
    gray = cv2.resize(gray, (16, 16))
    avg = gray.mean()
    bi = 1 * (gray > avg)
    return bi

def hamming_distance(a, b):
    a = a.reshape(1,-1)
    b = b.reshape(1,-1)
    distance = (a !=b).sum()
    return distance

query_hash = img2hash(img)

img_path = glob.glob(search_dir+'/**/*.jpg')
for path in img_path:
    img = cv2.imread(path)
    cv2.imshow('searching...', img)
    cv2.waitKey(5)
    a_hash = img2hash(img)
    dst = hamming_distance(query_hash, a_hash)
    if dst/256 < 0.25:
        print(path, dst/256)
        cv2.imshow(path, img)
cv2.destroyWindow('searching...')
cv2.waitKey(0)
cv2.destroyAllWindows()
```

#### 平均哈希匹配

- 寻找枪支图像 <结果 1>

```
../img/101_ObjectCategories/revolver/image_0033.jpg 0.2421875
../img/101_ObjectCategories/revolver/image_0019.jpg 0.23828125
../img/101_ObjectCategories/revolver/image_0031.jpg 0.21875
../img/101_ObjectCategories/revolver/image_0018.jpg 0.1953125
../img/101_ObjectCategories/revolver/image_0034.jpg 0.23046875
../img/101_ObjectCategories/revolver/image_0021.jpg 0.171875
../img/101_ObjectCategories/revolver/image_0037.jpg 0.2421875
../img/101_ObjectCategories/revolver/image_0023.jpg 0.21875
../img/101_ObjectCategories/revolver/image_0022.jpg 0.21484375
../img/101_ObjectCategories/revolver/image_0081.jpg 0.23046875
../img/101_ObjectCategories/revolver/image_0068.jpg 0.24609375
../img/101_ObjectCategories/revolver/image_0064.jpg 0.18359375
../img/101_ObjectCategories/revolver/image_0072.jpg 0.203125
../img/101_ObjectCategories/revolver/image_0001.jpg 0.2421875
../img/101_ObjectCategories/revolver/image_0015.jpg 0.24609375
../img/101_ObjectCategories/revolver/image_0017.jpg 0.23828125
../img/101_ObjectCategories/binocular/image_0011.jpg 0.23828125
../img/101_ObjectCategories/BACKGROUND_Google/image_0398.jpg 0.234375
../img/101_ObjectCategories/Faces_easy/image_0419.jpg 0.2421875
```

![](img/9de0cb7cd12c1232044968590f57e471_292_0.png)

#### 模板匹配

- 最基本的目标检测方法
- 在所有图像中搜索以查找图像
- cv2.matchTemplate(image, templ, method) : result
  - image : 输入图像
  - templ : 检测对象
  - method : 匹配算法
    - cv2.TM_CCOEFF, cv2.TM_CCOEFF_NORMED
    - cv2.TM_CCORR, cv2.TM_CCORR_NORMED
    - cv2.TM_SQDIFF, cv2.TM_SQDIFF_NORMED
  - 返回值 : 匹配结果数组值，
    - 对于 TM_SQDIFF 和 TM_SQDIFF_NORMED，返回 min_loc
    - 其他情况返回 max_loc
- cv2.minMaxLoc(matched)
  - 返回值 : min_val, max_val, min_loc, max_loc

![](img/9de0cb7cd12c1232044968590f57e471_293_0.png)

#### 匹配方法 ( I : 图像, T : 模板, R : 结果 )

- method=CV_TM_SQDIFF
  - $R(x, y) = \sum_{x', y'} (T(x', y') - I(x + x', y + y'))^2$
- method=CV_TM_SQDIFF_NORMED
  - $R(x, y) = \frac{\sum_{x', y'} (T(x', y') - I(x + x', y + y'))^2}{\sqrt{\sum_{x', y'} T(x', y')^2 \cdot \sum_{x', y'} I(x + x', y + y')^2}}$
- method=CV_TM_CCORR
  - $R(x, y) = \sum_{x', y'} (T(x', y') \cdot I(x + x', y + y'))$
- method=CV_TM_CCORR_NORMED
  - $R(x, y) = \frac{\sum_{x', y'} (T(x', y') \cdot I(x + x', y + y'))}{\sqrt{\sum_{x', y'} T(x', y')^2 \cdot \sum_{x', y'} I(x + x', y + y')^2}}$
- method=CV_TM_CCOEFF
  - $R(x, y) = \sum_{x', y'} (T'(x', y') \cdot I'(x + x', y + y'))$
  - 其中
    - $T'(x', y') = T(x', y') - 1/(w \cdot h) \cdot \sum_{x'', y''} T(x'', y'')$
    - $I'(x + x', y + y') = I(x + x', y + y') - 1/(w \cdot h) \cdot \sum_{x'', y''} I(x + x'', y + y'')$
- method=CV_TM_CCOEFF_NORMED
  - $R(x, y) = \frac{\sum_{x', y'} (T'(x', y') \cdot I'(x + x', y + y'))}{\sqrt{\sum_{x', y'} T'(x', y')^2 \cdot \sum_{x', y'} I'(x + x', y + y')^2}}$

#### ❖ 模板匹配

- 示例

```python
import cv2
import numpy as np

img = cv2.imread('../img/figures.jpg')
template = cv2.imread('../img/taekwonv1.jpg')
th, tw = template.shape[:2]
cv2.imshow('template', template)

methods = ['cv2.TM_CCOEFF_NORMED',
'cv2.TM_CCORR_NORMED', 'cv2.TM_SQDIFF_NORMED']
for i, method_name in enumerate(methods):
    img_draw = img.copy()
    method = eval(method_name)
    res = cv2.matchTemplate(img, template, method)
    min_val, max_val, min_loc, max_loc = cv2.minMaxLoc(res)
    print(method_name, min_val, max_val, min_loc, max_loc)
    if method in [cv2.TM_SQDIFF, cv2.TM_SQDIFF_NORMED]:
        top_left = min_loc
        match_val = min_val
    else:
        top_left = max_loc
        match_val = max_val
    bottom_right = (top_left[0] + tw, top_left[1] + th)
    cv2.rectangle(img_draw, top_left, bottom_right, (0,0,255),2)
    cv2.putText(img_draw, str(match_val), top_left,
cv2.FONT_HERSHEY_PLAIN, 2,(0,255,0), 1, cv2.LINE_AA)
    cv2.imshow(method_name, img_draw)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

```
cv2.TM_CCOEFF_NORMED -0.17781560122966766 0.5126562714576721 (42, 0) (208, 43)
cv2.TM_CCORR_NORMED 0.8271383047103882 0.9236084818840027 (85, 6) (208, 43)
cv2.TM_SQDIFF_NORMED 0.1706070452928543 0.36892083287239075 (208, 43) (86, 7)
```

#### ❖ 模板匹配

- 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_295_0.png)

#### ❖ 多目标匹配

- 当你想在一张图像中查找相同模板时
- cv2.matchTemplate() 只返回一个结果
- cv2.minMaxLoc()：只返回最大或最小值的问题
- 查找最大或最小值中的最小差异。
  - threshold = max * (1-0.01)
  - np.where(res >= threshold)

![](img/9de0cb7cd12c1232044968590f57e471_295_1.png)

#### 多目标匹配

- 示例

```python
img = cv2.imread('../img/mario.png')
imggray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

template = cv2.imread('../img/mario_template.png',0)
w, h = template.shape[::-1]

res = cv2.matchTemplate(imggray,template,cv2.TM_CCOEFF_NORMED)
min_val, max_val, min_loc, max_loc = cv2.minMaxLoc(res)
threshold = max_val * (1-0.01)
print("max:%f, threshold : %f" %(max_val, threshold))

loc = np.where( res >= threshold)
for pt in zip(*loc[::-1]):
    cv2.rectangle(img, pt, (pt[0] + w, pt[1] + h), (0,0,255), 2)

cv2.imshow('Question Box',img)
cv2.waitKey()
cv2.destroyAllWindows()
```

- 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_296_0.png)

### 匹配与跟踪

1. 与相似图像进行匹配
2. **特征与关键点**
3. 描述符提取器
4. 特征匹配
5. 跟踪
6. 实践

#### 角点特征检测器

- 模板匹配的局限性
  - 旋转、缩放、亮度、对比度、色调、无法进行仿射变换
- 图像的角点：
  - 容易从图像中找到特征
  - 除了角点，甚至无法检测到人脸
  - 检测图像特征

![](img/9de0cb7cd12c1232044968590f57e471_298_0.png)

#### ❖ Harris 角点检测

- Chris Harris, Mike Stephens 的论文，1988年
- 寻找角点关键点的代表性方法
- 当移动核时，所有方向都会发生变化
- cv2.cornerHarris(img, blockSize, ksize, k)
  - img : 输入图像，float32
  - blocksize : 邻域像素大小
  - ksize : 在 Sobel 中使用的核大小
  - k : K 参数（经验整数，0.04~0.06）
  - 返回值 : 与输入图像大小相同
    - 变化值

![](img/9de0cb7cd12c1232044968590f57e471_299_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_299_1.png)

![](img/9de0cb7cd12c1232044968590f57e471_299_2.png)

#### ❖ Harris 角点检测

```python
import cv2
import numpy as np
img = cv2.imread('../img/house.jpg')
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
corner = cv2.cornerHarris(gray, 2, 3, 0.04)
coord = np.where(corner > 0.1* corner.max())
coord = np.stack((coord[1], coord[0]), axis=-1)
for x, y in coord:
    cv2.circle(img, (x,y), 5, (0,0,255), 1, cv2.LINE_AA)
corner_norm = cv2.normalize(corner, None, 0, 255, cv2.NORM_MINMAX, cv2.CV_8U)
corner_norm = cv2.cvtColor(corner_norm, cv2.COLOR_GRAY2BGR)
merged = np.hstack((corner_norm, img))
cv2.imshow('Harris Corner', merged)
cv2.waitKey()
cv2.destroyAllWindows()
```

#### ❖ Harris 角点检测

- 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_300_0.png)

#### ❖ Shi-Tomasi

- J.Shi, C. Tomasi 的论文，1994年
- 适用于寻找使用光流法检测的特征
- 仅考虑 Harris 方法中的最小特征值 λ1, λ2
- 在结果值上应用阈值，若大于该阈值，则将其识别为角点
- **R = min(λ1, λ2) > T**
- cv2.goodFeaturesToTrack(img, maxCorner, qualityLev, minDistance)
  - img : 输入图像
  - maxCorner : 希望检测的角点数量，从最强的开始
  - qualityLev: 被视为角点的阈值
  - mask : 检测中要排除的掩码
  - blockSize = 3 : 角点周围区域的大小
    - useHarrisDetector=False: 选择检测角点的方法
    - True = Harris 角点检测。False = Shi-Tomasi 检测
  - K : Harris 角点检测中使用的 k 系数
  - corners : 角点检测坐标结果，数组大小为 N x 1 x 2，其值为实数，因此需要将其转换为整数

#### ❖ Shi-Tomasi

- 示例

```python
import cv2
import numpy as np
img = cv2.imread('../img/house.jpg')
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
corners = cv2.goodFeaturesToTrack(gray, 80, 0.01, 10)
corners = np.int32(corners)
for corner in corners:
    x, y = corner[0]
    cv2.circle(img, (x, y), 5, (0,0,255), 1, cv2.LINE_AA)
cv2.imshow('Corners', img)
cv2.waitKey()
cv2.destroyAllWindows()
```

- 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_301_0.png)

#### 关键点与特征检测器

- 无论特征检测算法如何，都提供通用接口
- 特征检测器：继承 cv2.Feature2D 后操作，共12种类型（截至 3.4.1 版本）
- 特征 : cv2.KeyPoint

![](img/9de0cb7cd12c1232044968590f57e471_302_0.png)

#### cv2.Feature2D

- keypoints = detector.detect(img [, mask]) : 检测关键点的函数
  - img : 输入图像，二值尺度
  - mask : 检测中要排除的掩码
  - keypoints : 特征检测结果，KeyPoint 列表
- KeyPoint : 包含特征信息的对象
  - pt : 关键点坐标 (x, y)，需要转换为整数，其类型为浮点数
  - size : 有意义的关键点邻域半径
  - angle : 特征点方向（顺时针，-1=无意义）
  - response : 特征点响应强度（取决于检测器）
  - octave : 检测图像中的金字塔层数
  - class_id : 关键点所属对象的 ID

#### ❖ 显示关键点

- outImg = cv2.drawKeypoints(img, keypoints, outImg[, color[, flags]])
  - img : 输入图像
  - keypoints : 要显示的关键点列表
  - outImg : 绘制了关键点的结果图像
  - color : 显示颜色，（基本值：随机）
  - flags : 标记显示方法的标志
    - cv2.DRAW_MATCHES_FLAGS_DEFAULT : 仅在坐标中心绘制圆圈
    - cv2.DRAW_MATCHES_FLAGS_DRAW_RICH_KEYPOINTS : 绘制考虑大小和角度的圆圈

#### ❖ GFTTDetector

- cv2.goodFeaturesToTrack() 检测器以函数形式展示
  - detector = cv2.GFTTDetector_create([, maxCorners[, qualityLevel, minDistance, blockSize, useHarrisDetector, k] )
- 所有参数细节与 cv2.goodFeaturesToTrack() 相同
- KeyPoint 对象中除了 PT 特征坐标外没有其他值

```python
import cv2
import numpy as np

img = cv2.imread("../img/house.jpg")
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

gftt = cv2.GFTTDetector_create()

keypoints = gftt.detect(gray, None)

img_draw = cv2.drawKeypoints(img, keypoints, None)

cv2.imshow('GFTTDectector', img_draw)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

#### ❖ GFTTDetector

![](img/9de0cb7cd12c1232044968590f57e471_304_0.png)

#### ❖ FAST (加速段测试特征)

- Edward Rosten, Tom Drummond, 剑桥大学, 2006
- 提高实时速度
  - 检查以点 P 为中心、半径为3的圆上的16个像素
  - 如果比 P 亮超过某个值 (t)，则将其识别为角点
  - n : FAST-9, 10, 11, 12, 13, 14, 15, 16
- 仅检测关键点，不提供描述符

![](img/9de0cb7cd12c1232044968590f57e471_304_1.png)

#### ❖ FAST (加速段测试特征)

- detector = cv2.FastFeatureDetector_create([threshold[, nonmaxSuppression, type]])
  - threshold=10 : 角点检测阈值
  - nonmaxSuppression = True : 控制角点是否为最大值
  - type : 边缘检测模式
    - cv2.FastFeatureDetector_TYPE_9_16: 16个连续值中的9个（基本值）
    - cv2.FastFeatureDetector_TYPE_7_12: 12个连续值中的7个
    - cv2.FastFeatureDetector_TYPE_5_8: 8个连续值中的5个

- 示例

```python
import cv2
import numpy as np
img = cv2.imread('../img/house.jpg')
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
fast = cv2.FastFeatureDetector_create(50)
keypoints = fast.detect(gray, None)
img = cv2.drawKeypoints(img, keypoints, None)
cv2.imshow('FAST', img)
cv2.waitKey()
cv2.destroyAllWindows()
```

- 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_305_0.png)

#### ❖ SimpleBlobDetector

- BLOB (二值大对象)
  - 二值尺度图像中连通的像素组
  - 将小物体识别为噪声
  - 仅对大于某尺寸的物体感兴趣

- detector = cv2.SimpleBlobDetector_create( [parameters] ) : Blob 检测器生成器
  - parameters : Blob 检测过滤器参数的对象

- 示例

```python
import cv2
import numpy as np
img = cv2.imread("../img/house.jpg")
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
detector = cv2.SimpleBlobDetector_create()
keypoints = detector.detect(gray)
img = cv2.drawKeypoints(img, keypoints, None,
(0,0,255),flags=cv2.DRAW_MATCHES_FLAGS_DRAW_RICH_KEYPOINTS)
cv2.imshow("Blob", img)
cv2.waitKey(0)
```

- 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_306_0.png)

#### ❖ SimpleBlobDetector

- cv2.SimpleBlobDetector_Params()
  - minThreshold, maxThreshold, thresholdStep : 生成 Blob 的边界值
    - 从 minThreshold 开始，按 thresholdStep 递增，直到超过 maxThreshold
  - minRepeatability: 参与 Blob 的连续边界值数量
  - minDistBetweenBlobs: 将两个 Blob 识别为一个 Blob 的距离
  - filterByArea: 面积过滤器选项
  - minArea, maxArea: 将 min~max 面积内的检测为 Blob
  - filterByCircularity: 圆形度过滤器选项
  - minCircularity, maxCircularity : 仅将 min ~ max 圆形度内的检测为 Blob
  - filterByColor: 使用亮度的过滤器选项
  - blobColor : 0=检测黑色 Blob, 255= 检测白色 Blob
  - filterByConvexity: 凸度过滤器选项
  - minConvexity, maxConvexity : 将 min~max 凸度内的检测为 Blob
  - filterByInertia : 惯性比过滤器选项
  - minInertiaRatio, maxInertiaRatio: 将 min~max 惯性比内的检测为 Blob

- 示例 <1/2>

```python
import cv2
import numpy as np

img = cv2.imread("../img/house.jpg")
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

params = cv2.SimpleBlobDetector_Params()

params.minThreshold = 10
params.maxThreshold = 240

params.thresholdStep = 5

params.filterByArea = True
params.minArea = 200
```

- 示例 <2/2>

```python
params.filterByColor = False
params.filterByConvexity = False
params.filterByInertia = False
params.filterByCircularity = False

detector = cv2.SimpleBlobDetector_create(params)
keypoints = detector.detect(gray)
img_draw = cv2.drawKeypoints(img, keypoints, None, None,
    cv2.DRAW_MATCHES_FLAGS_DRAW_RICH_KEYPOINTS)

cv2.imshow("Blob with Params", img_draw)
cv2.waitKey(0)
```

- 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_308_0.png)

### 匹配与跟踪

1. 与相似图像匹配
2. 特征与关键点
3. **描述符提取器**
4. 特征匹配
5. 跟踪
6. 实践

#### 特征描述符

- 要求特征描述符不受旋转、大小和方向的影响
- 将关键点周围的像素划分为特定大小的块
- 计算块内像素的梯度直方图
- 通常，它显示8个方向的斜率
  - 每个关键点 4 x 4 x 8 = 由128个值组成

![](img/9de0cb7cd12c1232044968590f57e471_310_0.png)

#### 描述符提取器接口

- cv2.Feature2D 抽象类
- keypoints, descriptors = detector.compute(image, keypoints[, descriptors]) : 在传递关键点进行周转时计算特定描述符
- keypoints, descriptors = detector.detectAndCompute(image, mask[, descriptors, useProvidedKeypoints]): 同时运行关键点检测和特征描述符计算
  - image : 输入图像
  - keypoints : 描述符计算所需的关键点
  - descriptors : 计算出的描述符
  - mask : 关键点检测中使用的掩码
  - useProvidedKeypoints : 当为 True 时，不运行关键点检测（不使用）

#### ❖ SIFT（尺度不变特征变换）

- D.Lowe，不列颠哥伦比亚大学，2004年
- 角点匹配问题
  - 强度：亮度、移动、旋转、仿射强度
  - 缩放：如果尺寸减小，会检测到更多角点
- 使用图像金字塔解决尺度问题
  - 在拉普拉斯值最大化处寻找特征点
- 它拥有专利权；仅允许在学校和研究中使用
- 从OpenCV3.0包开始，它不包含在基础包中
  - 包含在额外（contrib）包中

![](img/9de0cb7cd12c1232044968590f57e471_311_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_311_1.png)

- 生成对象
- detector = cv.xfeatures2d.SIFT_create([, nfeatures[, nOctaveLayers[, contrastThreshold[, edgeThreshold[, sigma]]]]] )
  - nfeatures : 要检测的最大特征数
  - nOctaveLayers : 使用图像金字塔的层数
  - contrastThreshold : 用于过滤弱特征的阈值
  - edgeThreshold : 用于过滤弱边缘的阈值
  - sigma : 在图像金字塔0层使用的高斯滤波器中的Sigma值

#### ❖ SIFT（尺度不变特征变换）

- 示例

```
import cv2
import numpy as np

img = cv2.imread('../img/house.jpg')
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

sift = cv2.xfeatures2d.SIFT_create()
keypoints, descriptor = sift.detectAndCompute(gray, None)
print('keypoint:',len(keypoints), 'descriptor:', descriptor.shape)
print(descriptor)

img_draw = cv2.drawKeypoints(img, keypoints, None,
flags=cv2.DRAW_MATCHES_FLAGS_DRAW_RICH_KEYPOINTS)

cv2.imshow('SIFT', img_draw)
cv2.waitKey()
cv2.destroyAllWindows()
```

- 示例 <结果>

```
keypoint: 413 descriptor: (413, 128)
[[  1.   1.   1. ...   0.   0.   1.]
 [  8.  24.   0. ...   1.   0.   4.]
 [  0.   0.   0. ...   0.   0.   2.]
 ...
 [  1.   8.  71. ...  73. 127.   3.]
 [ 35.   2.   7. ...   0.   0.   9.]
 [ 36.  34.   3. ...   0.   0.   1.]]
```

![](img/9de0cb7cd12c1232044968590f57e471_312_0.png)

#### ❖ SURF（加速稳健特征）

- 改进SIFT性能
  - 改变滤波器尺寸而非图像金字塔
- 它拥有专利权；仅允许在学校和研究中使用
- 从OpenCV3.0包开始，它不包含在基础包中，但包含在额外（contrib）包中
- detector = cv.xfeatures2d.SURF_create([hessianThreshold, nOctaves, nOctaveLayers, extended, upright])
  - hessianThreshold : 检测特征的边界值（100）
  - nOctaves : 图像金字塔层数（3）
  - extended : 描述符生成标志（False），True : 128，False : 64
  - upright : 方向计算标志（False），True : 忽略方向，False: 应用方向
- 示例

```
import cv2
import numpy as np

img = cv2.imread('../img/house.jpg')
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

surf = cv2.xfeatures2d.SURF_create(1000, 3, True, True)

keypoints, desc = surf.detectAndCompute(gray, None)
print(desc.shape, desc)

img_draw = cv2.drawKeypoints(img, keypoints, None,
flags=cv2.DRAW_MATCHES_FLAGS_DRAW_RICH_KEYPOINTS)
cv2.imshow('SURF', img_draw)
cv2.waitKey()
cv2.destroyAllWindows()
```

#### ❖ SURF（加速稳健特征）

- 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_314_0.png)

#### ❖ BRIEF（二进制稳健独立基本特征）

- 将浮点数据转换为二进制字符串
- 处理快速，内存使用减少
- 仅能进行描述符计算
  - 关键点检测依赖于SIFT/SURF/Star
  - 关键点：图像中特征的坐标
  - 描述符：
    - 检测到的局部图像的特征信息
    - 梯度分布直方图信息
    - 主要用于确定两张图像的匹配程度

#### ❖ BRIEF（二进制稳健独立基本特征）

- 示例

```
import cv2
import numpy as np

img = cv2.imread('../img/model.jpg')
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

star = cv2.xfeatures2d.StarDetector_create()
brief = cv2.xfeatures2d.BriefDescriptorExtractor_create()

kp1 = star.detect(img, None)
kp2, desc = brief.compute(img, kp1)

print(desc.shape)
print(desc)
img = cv2.drawKeypoints(img, kp2, img)

cv2.imshow('BRIEF', img)
cv2.waitKey()
cv2.destroyAllWindows()
```

![](img/9de0cb7cd12c1232044968590f57e471_315_0.png)

#### ORB（定向FAST和旋转BRIEF）

- Rublee, Rabaud, Konolige, Bradski, 2011
- 使用FAST检测特征
- 在BRIEF中考虑旋转和方向
  - SIFT/SURF/BRIEF的替代方案
- detector = cv.ORB_create([nfeatures, scaleFactor, nlevels, edgeThreshold, firstLevel, WTA_K, scoreType, patchSize, fastThreshold] )
  - nfeatures=500 : 要检测的最大特征数
  - scaleFactor = 1.2 : 图像金字塔比率
  - nlevels = 8 : 图像金字塔中的层数
  - edgeThreshold = 31 : 排除搜索的边界大小，与patchSize匹配
  - firstLevel = 0 : 初始图像金字塔层级别
  - WTA_K = 2 : 生成的临时坐标数
  - scoreType : 关键点检测中使用的类型
    - cv2.ORB_HARRIS_SCORE : Harris角点检测（基本值）
    - cv2.ORB_FAST_SCORE : FAST角点检测
  - patchSize = 31 : 描述符的块大小
  - fastThreshold = 20 : FAST中使用的阈值
- 示例

```
import cv2
import numpy as np

img = cv2.imread('../img/house.jpg')
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

orb = cv2.ORB_create()
keypoints, descriptor = orb.detectAndCompute(img, None)
img_draw = cv2.drawKeypoints(img, keypoints,
None,  flags=cv2.DRAW_MATCHES_FLAGS_DRAW_RICH_KEYPOINTS)

cv2.imshow('ORB', img_draw)
cv2.waitKey()
cv2.destroyAllWindows()
```

#### ORB（定向FAST和旋转BRIEF）

- 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_317_0.png)

### 匹配与跟踪

1. 与相似图像匹配
2. 特征与关键点
3. 描述符提取器
4. **特征匹配**
5. 跟踪
6. 实践

#### 匹配

- 通过比较不同图像的关键点和描述符来匹配相似性
- 根据匹配点数量来衡量和检测图像的相似性
- 图像搜索、生成全景图像、用于物体识别

#### 通用接口

- cv2.DescriptorMatcher : 继承匹配抽象类
- cv2.DMatch : 保存匹配结果的对象

![](img/9de0cb7cd12c1232044968590f57e471_319_0.png)

#### 通用接口

- matcher = cv2.DescriptorMatcher_create(matcherType) : 匹配生成器
  - matcherType: 要生成的操作类算法，字符串
    - "BruteForce": 使用NORM_L2的BFMatcher
    - "BruteForce-L1": 使用NORM_L1的BFMatcher
    - "BruteForce-Hamming": 使用NORM_HAMMING的BFMatcher
    - "BruteForce-Hamming(2)": 使用NORM_HAMMING2的BFMatcher
    - "FlannBased" : 使用NORM_L2的FlannBasedMatcher
- matches = matcher.match(queryDescriptors, trainDescriptors[, mask]): 1个最佳匹配
- queryDescriptors: 数组特征描述符，作为匹配标准的描述符
- trainDescriptors: 数组特征描述符，作为匹配对象的描述符
- mask: 匹配的掩码
- matches: 匹配结果，DMatch对象列表

- matches = matcher.knnMatch( queryDescriptors, trainDescriptors, k[, mask[, compactResult]] : K个最接近的匹配
  - k : 要匹配的邻居数
  - compactResult=False : True: 如果没有匹配，则在结果中排除

- matches = matcher.radiusMatch(queryDescriptors, trainDescriptors, maxDistance[, mask, compactResult]): 在maxDistance内的距离匹配
  - maxDistance : 匹配的距离

- DMatch
  - queryIdx : queryDescriptor的索引
  - trainIdx : trainDescriptor的索引
  - imgIdx : trainDescriptor的图像索引
  - distance : 相似性距离

- cv2.drawMatches(img1, kp1, img2, kp2, matches, flags): 在img1中标记匹配点，kp1 : queryDescriptor的图像和关键点
- img1, kp1 : trainDescriptor的图像和关键点
- matches : 匹配结果
- flags : 匹配点绘制选项
  - cv2.DRAW_MATCHES_FLAGS_DEFAULT : 新生成结果图像（基本值）
  - cv2.DRAW_MATCHES_FLAGS_DRAW_OVER_OUTIMG : 不新生成结果图像
  - cv2.DRAW_MATCHES_FLAGS_DRAW_RICH_KEYPOINTS: 绘制关键点的大小和方向
  - cv2.DRAW_MATCHES_FLAGS_NOT_DRAW_SINGLE_POINTS : 排除单侧匹配结果绘制

#### BF（暴力）匹配器

- 检查两张图像的所有特征描述符
- matcher = cv.BFMatcher_create([normType[, crossCheck]])
  - normType : 距离度量算法
    - cv2.NORM_L1
    - cv2.NORM_L2 : 基准值
    - cv2.NORM_L2SQR
    - cv2.NORM_HAMMING
    - cv2.NORM_HAMMING2
  - crossCheck=False : 仅应用具有相互匹配的结果
- 根据特征选择 normType
  - SIFT SURF : NORM_L1, NORM_L2
  - ORB : NORM_HAMMING
  - ORB , WTA_K : NORM_HAMMING2
- SIFT-BF 示例

```
import cv2, numpy as np

img1 = cv2.imread('../img/taekwonv1.jpg')
img2 = cv2.imread('../img/figures.jpg')
gray1 = cv2.cvtColor(img1, cv2.COLOR_BGR2GRAY)
gray2 = cv2.cvtColor(img2, cv2.COLOR_BGR2GRAY)

detector = cv2.xfeatures2d.SIFT_create()
kp1, desc1 = detector.detectAndCompute(gray1, None)
kp2, desc2 = detector.detectAndCompute(gray2, None)

matcher = cv2.BFMatcher(cv2.NORM_L1, crossCheck=True)
matches = matcher.match(desc1, desc2)
res = cv2.drawMatches(img1, kp1, img2, kp2, matches, None, \n                      flags=cv2.DRAW_MATCHES_FLAGS_NOT_DRAW_SINGLE_POINTS)

cv2.imshow('BFMatcher + SIFT', res)
cv2.waitKey()
cv2.destroyAllWindows()
```

#### ❖ BF（暴力）匹配

- SIFT-BF 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_322_0.png)

- SURF– BF 示例

```
import cv2
import numpy as np
img1 = cv2.imread('../img/taekwonv1.jpg')
img2 = cv2.imread('../img/figures.jpg')
gray1 = cv2.cvtColor(img1, cv2.COLOR_BGR2GRAY)
gray2 = cv2.cvtColor(img2, cv2.COLOR_BGR2GRAY)
detector = cv2.xfeatures2d.SURF_create()
kp1, desc1 = detector.detectAndCompute(gray1, None)
kp2, desc2 = detector.detectAndCompute(gray2, None)
matcher = cv2.BFMatcher(cv2.NORM_L2, crossCheck=True)
matches = matcher.match(desc1, desc2)
res = cv2.drawMatches(img1, kp1, img2, kp2, matches, None, \n                     flags=cv2.DRAW_MATCHES_FLAGS_NOT_DRAW_SINGLE_POINTS)
cv2.imshow('BF + SURF', res)
cv2.waitKey()
cv2.destroyAllWindows()
```

#### ❖ BF（暴力）匹配

- SURF– BF 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_323_0.png)

- ORB– BF 示例

```
import cv2, numpy as np

img1 = cv2.imread('../img/taekwonv1.jpg')
img2 = cv2.imread('../img/figures.jpg')
gray1 = cv2.cvtColor(img1, cv2.COLOR_BGR2GRAY)
gray2 = cv2.cvtColor(img2, cv2.COLOR_BGR2GRAY)
detector = cv2.ORB_create()
kp1, desc1 = detector.detectAndCompute(gray1, None)
kp2, desc2 = detector.detectAndCompute(gray2, None)
matcher = cv2.BFMatcher(cv2.NORM_HAMMING, crossCheck=True)
matches = matcher.match(desc1, desc2)
res = cv2.drawMatches(img1, kp1, img2, kp2, matches, None, \n                      flags=cv2.DRAW_MATCHES_FLAGS_NOT_DRAW_SINGLE_POINTS)
cv2.imshow('BFMatcher + ORB', res)
cv2.waitKey()
cv2.destroyAllWindows()
```

#### BF（暴力）匹配

- ORB– BF 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_324_0.png)

#### FLANN 匹配

- 用于近似最近邻匹配的快速库
- BF 匹配的缺点
  - 检查所有：如果图像尺寸很大，性能会下降
- 不比较所有描述符，只匹配最接近的邻近值
  - 需要额外的参数来选择算法
- 选择搜索参数和索引参数
  - C++ 类操作在 Python 绑定中被省略
  - 在字典对象中草拟键值对
- matcher = cv2.FlannBasedMatcher([indexParams[, searchParams]])
  - indexParams : 索引参数，字典
    - algorithm : 算法选择键，根据所选键决定附属键
    - FLANN_INDEX_LINEAR = 0: 线性索引，与 BFMatcher 相同
    - FLANN_INDEX_KDTREE = 1 : KD-Tree 索引
    - FLANN_INDEX_KMEANS = 2 : K-均值树索引
    - FLANN_INDEX_COMPOSITE = 3 : KD Tree，K 均值混合索引
    - FLANN_INDEX_LSH = 6 : LSH 索引
    - FLANN_INDEX_AUTOTUNED = 255 : 自动索引
  - searchParams : 搜索参数，字典对象
    - checks=32 : 搜索的候选数量
    - eps =0.0 : 不使用
    - sorted=True : 整理并返回

#### ❖ FLANN 匹配

- 草拟索引参数困难且挑剔
- 建议的索引参数值
  - SIFT, SURF

```
FLANN_INDEX_KDTREE = 1
index_params = dict(algorithm=FLANN_INDEX_KDTREE, trees=5)
```

  - ORB

```
FLANN_INDEX_LSH = 6
index_params= dict(algorithm = FLANN_INDEX_LSH,
                  table_number = 6,
                  key_size = 12,
                  multi_probe_level = 1)
```

#### FLANN 匹配

- SIFT-FLANN 示例

```
import cv2, numpy as np
img1 = cv2.imread('../img/taekwonv1.jpg')
img2 = cv2.imread('../img/figures.jpg')
gray1 = cv2.cvtColor(img1, cv2.COLOR_BGR2GRAY)
gray2 = cv2.cvtColor(img2, cv2.COLOR_BGR2GRAY)
detector = cv2.xfeatures2d.SIFT_create()
kp1, desc1 = detector.detectAndCompute(gray1, None)
kp2, desc2 = detector.detectAndCompute(gray2, None)
FLANN_INDEX_KDTREE = 1
index_params = dict(algorithm=FLANN_INDEX_KDTREE, trees=5)
search_params = dict(checks=50)
matcher = cv2.FlannBasedMatcher(index_params, search_params)
matches = matcher.match(desc1, desc2)
res = cv2.drawMatches(img1, kp1, img2, kp2, matches, None, \n                     flags=cv2.DRAW_MATCHES_FLAGS_NOT_DRAW_SINGLE_POINTS)
cv2.imshow('Flann + SIFT', res)
cv2.waitKey()
cv2.destroyAllWindows()
```

- SIFT-FLANN 示例

![](img/9de0cb7cd12c1232044968590f57e471_326_0.png)

#### FLANN 匹配

- SURF-FLANN 示例

```
import cv2, numpy as np
img1 = cv2.imread('../img/taekwonv1.jpg')
img2 = cv2.imread('../img/figures.jpg')
gray1 = cv2.cvtColor(img1, cv2.COLOR_BGR2GRAY)
gray2 = cv2.cvtColor(img2, cv2.COLOR_BGR2GRAY)
detector = cv2.xfeatures2d.SURF_create()
kp1, desc1 = detector.detectAndCompute(gray1, None)
kp2, desc2 = detector.detectAndCompute(gray2, None)
FLANN_INDEX_KDTREE = 1
index_params = dict(algorithm=FLANN_INDEX_KDTREE, trees=5)
search_params = dict(checks=50)
matcher = cv2.FlannBasedMatcher(index_params, search_params)
matches = matcher.match(desc1, desc2)
res = cv2.drawMatches(img1, kp1, img2, kp2, matches, None, \n                      flags=cv2.DRAW_MATCHES_FLAGS_NOT_DRAW_SINGLE_POINTS)
cv2.imshow('Flann + SURF', res)
cv2.waitKey()
cv2.destroyAllWindows()
```

- SURF-FLANN 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_327_0.png)

#### FLANN 匹配

- ORB-FLANN 示例

```
import cv2, numpy as np
img1 = cv2.imread('../img/taekwonv1.jpg')
img2 = cv2.imread('../img/figures.jpg')
gray1 = cv2.cvtColor(img1, cv2.COLOR_BGR2GRAY)
gray2 = cv2.cvtColor(img2, cv2.COLOR_BGR2GRAY)
detector = cv2.ORB_create()
kp1, desc1 = detector.detectAndCompute(gray1, None)
kp2, desc2 = detector.detectAndCompute(gray2, None)
FLANN_INDEX_LSH = 6
index_params= dict(algorithm = FLANN_INDEX_LSH,
                   table_number = 6,
                   key_size = 12,
                   multi_probe_level = 1)
search_params=dict(checks=32)
matcher = cv2.FlannBasedMatcher(index_params, search_params)
matches = matcher.match(desc1, desc2)
res = cv2.drawMatches(img1, kp1, img2, kp2, matches, None, \n                     flags=cv2.DRAW_MATCHES_FLAGS_NOT_DRAW_SINGLE_POINTS)
cv2.imshow('Flann + ORB', res)
cv2.waitKey()
cv2.destroyAllWindows()
```

- SURF-FLANN 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_328_0.png)

#### 寻找好的匹配点

- match()
  - 为每个描述符返回一个匹配点
  - 使用小距离高排名百分比
- knnMatch()
  - 为每个描述符返回 K 个最近的邻居匹配点
  - 在邻居匹配点中选择最接近的一个
- radiusMatch()
  - 在特定距离内
  - 对于寻找好的匹配点没有意义
- match() 示例

```
import cv2, numpy as np

img1 = cv2.imread('../img/taekwonv1.jpg')
img2 = cv2.imread('../img/figures.jpg')
gray1 = cv2.cvtColor(img1, cv2.COLOR_BGR2GRAY)
gray2 = cv2.cvtColor(img2, cv2.COLOR_BGR2GRAY)

detector = cv2.ORB_create()
kp1, desc1 = detector.detectAndCompute(gray1, None)
kp2, desc2 = detector.detectAndCompute(gray2, None)
matcher = cv2.BFMatcher(cv2.NORM_HAMMING, crossCheck=True)
matches = matcher.match(desc1, desc2)
matches = sorted(matches, key=lambda x:x.distance)
min_dist, max_dist = matches[0].distance, matches[-1].distance
ratio = 0.2
good_thresh = (max_dist - min_dist) * ratio + min_dist
good_matches = [m for m in matches if m.distance < good_thresh]
print('matches:%d/%d, min:%.2f, max:%.2f, thresh:%.2f' \n      %(len(good_matches),len(matches), min_dist, max_dist, good_thresh))
res = cv2.drawMatches(img1, kp1, img2, kp2, good_matches, None, \n                     flags=cv2.DRAW_MATCHES_FLAGS_NOT_DRAW_SINGLE_POINTS)
cv2.imshow('Good Match', res)
cv2.waitKey()
cv2.destroyAllWindows()
```

#### FLANN 匹配

- match() 示例 <result>

![](img/9de0cb7cd12c1232044968590f57e471_330_0.png)

- knnMatch() 示例

```python
import cv2, numpy as np
img1 = cv2.imread('../img/taekwonv1.jpg')
img2 = cv2.imread('../img/figures.jpg')
gray1 = cv2.cvtColor(img1, cv2.COLOR_BGR2GRAY)
gray2 = cv2.cvtColor(img2, cv2.COLOR_BGR2GRAY)
detector = cv2.ORB_create()
kp1, desc1 = detector.detectAndCompute(gray1, None)
kp2, desc2 = detector.detectAndCompute(gray2, None)
matcher = cv2.BFMatcher(cv2.NORM_HAMMING2)
matches = matcher.knnMatch(desc1, desc2, 2)
ratio = 0.75
good_matches = [first for first,second in matches
                if first.distance < second.distance * ratio]
print('matches:%d/%d' %(len(good_matches),len(matches)))
res = cv2.drawMatches(img1, kp1, img2, kp2, good_matches, None,
                     flags=cv2.DRAW_MATCHES_FLAGS_NOT_DRAW_SINGLE_POINTS)
cv2.imshow('Matching', res)
cv2.waitKey()
cv2.destroyAllWindows()
```

#### 寻找良好匹配

- kNNmatch() 示例

![](img/9de0cb7cd12c1232044968590f57e471_331_0.png)

#### 通过单应性匹配寻找物体

- 计算具有良好匹配点的两张图像的距离变换矩阵
- 寻找物体位置
- 消除变换矩阵上方的不良匹配点
- cv2.findHomography()
    - 通过多对匹配点计算近似距离变换矩阵
- cv2.perspectiveTransform()
    - 将原始坐标距离变换以适应变换矩阵
- mtrx, mask = cv.findHomography( srcPoints, dstPoints[, method[, ransacReprojThreshold[, mask[, maxIters[, confidence]]]] )
    - srcPoints：原始坐标数组
    - dstPoints：结果坐标数组
    - method=0：选择近似计算算法
        - 0：计算所有点的最小平方误差
        - cv2.RANSAC
        - cv2.LMEDS
        - cv2.RHO
    - ransacReprojThreshold=3：正常距离阈值，用于 RANSAC、RHO
    - maxIters=2000：近似计算重复次数
    - confidence=0.995：可靠性，0 ~ 1
    - mtrx：结果变换矩阵
    - mask：正常水平判断结果，N x 1 数组（0：正常，1：异常）
- dst = cv.perspectiveTransform(src, m[, dst])
    - src：输入坐标数组
    - m：变换矩阵
    - dst：输出坐标数组
- 近似计算算法
    - RANSAC（随机采样一致性）
        - 选择随机点，为最满意点计算近似点
        - 正常值（内点），异常值（外点）
    - LMEDS（最小中值平方）
        - 平方的最小中值
        - 无需额外参数
        - 仅在正常值超过 50% 时运行
    - RHO
        - 使用 PROSAC（渐进简单一致性）；改进的 RANSAC
        - 当外点较多时更快

#### 通过单应性匹配寻找物体

- 示例

```python
import cv2, numpy as np

img1 = cv2.imread('../img/taekwonv1.jpg')
img2 = cv2.imread('../img/figures.jpg')
gray1 = cv2.cvtColor(img1, cv2.COLOR_BGR2GRAY)
gray2 = cv2.cvtColor(img2, cv2.COLOR_BGR2GRAY)

detector = cv2.ORB_create()
kp1, desc1 = detector.detectAndCompute(gray1, None)
kp2, desc2 = detector.detectAndCompute(gray2, None)
matcher = cv2.BFMatcher(cv2.NORM_HAMMING2)
matches = matcher.knnMatch(desc1, desc2, 2)

ratio = 0.75
good_matches = [first for first,second in matches
                if first.distance < second.distance * ratio]
print('good matches:%d/%d' %(len(good_matches),len(matches)))

src_pts = np.float32([ kp1[m.queryIdx].pt for m in good_matches ])
dst_pts = np.float32([ kp2[m.trainIdx].pt for m in good_matches ])
mtrx, mask = cv2.findHomography(src_pts, dst_pts)
h,w, = img1.shape[:2]
pts = np.float32([ [[0,0]],[[0,h-1]],[[w-1,h-1]],[[w-1,0]] ])
dst = cv2.perspectiveTransform(pts,mtrx)
img2 = cv2.polylines(img2,[np.int32(dst)],True,255,3, cv2.LINE_AA)

res = cv2.drawMatches(img1, kp1, img2, kp2, good_matches, None,
                      flags=cv2.DRAW_MATCHES_FLAGS_NOT_DRAW_SINGLE_POINTS)
cv2.imshow('Matching Homography', res)
cv2.waitKey()
cv2.destroyAllWindows()
```

#### 通过单应性匹配寻找物体

- 示例 <result>

![](img/9de0cb7cd12c1232044968590f57e471_334_0.png)

#### 使用 RANSAC 去除不良匹配

- 示例 <1/2>

```python
import cv2, numpy as np

img1 = cv2.imread('../img/taekwonv1.jpg')
img2 = cv2.imread('../img/figures2.jpg')
gray1 = cv2.cvtColor(img1, cv2.COLOR_BGR2GRAY)
gray2 = cv2.cvtColor(img2, cv2.COLOR_BGR2GRAY)

detector = cv2.ORB_create()
kp1, desc1 = detector.detectAndCompute(gray1, None)
kp2, desc2 = detector.detectAndCompute(gray2, None)
matcher = cv2.BFMatcher(cv2.NORM_HAMMING, crossCheck=True)
matches = matcher.match(desc1, desc2)

matches = sorted(matches, key=lambda x:x.distance)
res1 = cv2.drawMatches(img1, kp1, img2, kp2, matches, None,
                       flags=cv2.DRAW_MATCHES_FLAGS_NOT_DRAW_SINGLE_POINTS)
```

#### 使用 RANSAC 去除不良匹配

- 示例 <2/2>

```python
src_pts = np.float32([ kp1[m.queryIdx].pt for m in matches ])
dst_pts = np.float32([ kp2[m.trainIdx].pt for m in matches ])
mtrx, mask = cv2.findHomography(src_pts, dst_pts, cv2.RANSAC, 5.0)
h,w = img1.shape[:2]
pts = np.float32([ [[0,0]],[[0,h-1]],[[w-1,h-1]],[[w-1,0]] ])
dst = cv2.perspectiveTransform(pts,mtrx)
img2 = cv2.polylines(img2,[np.int32(dst)],True,255,3, cv2.LINE_AA)
matchesMask = mask.ravel().tolist()
res2 = cv2.drawMatches(img1, kp1, img2, kp2, matches, None,
                      matchesMask = matchesMask,
                      flags=cv2.DRAW_MATCHES_FLAGS_NOT_DRAW_SINGLE_POINTS)
accuracy=float(mask.sum()) / mask.size
print("accuracy: %d/%d(%.2f%%)"% (mask.sum(), mask.size, accuracy))
cv2.imshow('Matching-All', res1)
cv2.imshow('Matching-Inlier ', res2)
cv2.waitKey()
cv2.destroyAllWindows()
```

- 示例 <result>

![](img/9de0cb7cd12c1232044968590f57e471_335_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_335_1.png)

#### 使用摄像头寻找物体

- 示例 <1/2>

```python
import cv2, numpy as np
img1 = None
win_name = 'Camera Matching'
MIN_MATCH = 10
detector = cv2.ORB_create(1000)
FLANN_INDEX_LSH = 6
index_params= dict(algorithm = FLANN_INDEX_LSH,
                   table_number = 6,
                   key_size = 12,
                   multi_probe_level = 1)
search_params=dict(checks=32)
matcher = cv2.FlannBasedMatcher(index_params, search_params)
cap = cv2.VideoCapture(0)
cap.set(cv2.CAP_PROP_FRAME_WIDTH, 640)
cap.set(cv2.CAP_PROP_FRAME_HEIGHT, 480)
while cap.isOpened():
    ret, frame = cap.read()
    if img1 is None:  # 没有注册图像，摄像头直通
        res = frame
    else:             # 存在注册图像，开始匹配
        img2 = frame
        gray1 = cv2.cvtColor(img1, cv2.COLOR_BGR2GRAY)
        gray2 = cv2.cvtColor(img2, cv2.COLOR_BGR2GRAY)
        kp1, desc1 = detector.detectAndCompute(gray1, None)
        kp2, desc2 = detector.detectAndCompute(gray2, None)
        matches = matcher.knnMatch(desc1, desc2, 2)
        ratio = 0.75
        good_matches = [m[0] for m in matches
                       if len(m) == 2 and m[0].distance < m[1].distance * ratio]
        matchesMask = np.zeros(len(good_matches)).tolist()
        if len(good_matches) > MIN_MATCH:
            src_pts = np.float32([ kp1[m.queryIdx].pt for m in good_matches ])
            dst_pts = np.float32([ kp2[m.trainIdx].pt for m in good_matches ])
            mtrx, mask = cv2.findHomography(src_pts, dst_pts, cv2.RANSAC, 5.0)
```

#### 使用摄像头寻找物体

- 示例 <2/2>

```python
if mask.sum() > MIN_MATCH: # 最小正常匹配点数超过特定数量
    matchesMask = mask.ravel().tolist()
    h,w, = img1.shape[:2]
    pts = np.float32([ [[0,0]],[[0,h-1]],[[w-1,h-1]],[[w-1,0]] ])
    dst = cv2.perspectiveTransform(pts,mtrx)
    img2 = cv2.polylines(img2,[np.int32(dst)],True,255,3, cv2.LINE_AA)
    res = cv2.drawMatches(img1, kp1, img2, kp2, good_matches, None,
                          matchesMask=matchesMask,
                          flags=cv2.DRAW_MATCHES_FLAGS_NOT_DRAW_SINGLE_POINTS)
    cv2.imshow(win_name, res)
    key = cv2.waitKey(1)
    if key == 27:    # Esc，结束
        break
    elif key == ord(' '): # 使用空格键设置 ROI，设置 img1
        x,y,w,h = cv2.selectROI(win_name, frame, False)
        if w and h:
            img1 = frame[y:y+h, x:x+w]
else:
    print("can't open camera.")
cap.release()
cv2.destroyAllWindows()
```

- 示例 <Result>

![](img/9de0cb7cd12c1232044968590f57e471_337_0.png)

### 匹配与跟踪

1.  使用相似图像进行匹配
2.  特征与关键点
3.  描述符提取器
4.  特征匹配
5.  **跟踪**
6.  实践工作坊

#### 背景减除

-   需要跟踪访客或交通流量
-   使用预备好的背景图像进行计算较为简单
-   但通常没有预备好的背景图像
-   `cv2.BackgroundSubtractor` 抽象类
    -   `fgmask = bgsubtractor.apply( image[, fgmask[, learningRate]] )`
        -   `image`：输入图像
        -   `fgmask`：前景图像掩码
        -   `learningRate=-1`：背景训练速度，范围 0 ~ 1，-1 表示自动
    -   `backgroundImage = bgsubtractor.getBackgroundImage([,backgroundImage])`
        -   `backgroundImage`：训练得到的背景图像
-   消除背景的算法
    -   `BackgroundSubtractorMOG`
    -   `BackgroundSubtractorMOG2`
    -   `BackgroundSubtractorGMG`

#### BackgroundSubtractorMOG

-   基于高斯混合模型的背景/前景分割算法
-   K 个高斯分布（K 从 3 到 5）
-   对每个背景像素使用混合模型
-   权重表示每个像素停留在某处的比例
    -   背景应停留较长时间或保持固定
-   `cv2.bgsegm.createBackgroundSubtractorMOG()`
    -   必须设置历史长度、高斯混合数量、阈值
    -   仅调用 `apply`（帧），基本值已设定
    -   contrib 模块

#### ❖ BackgroundSubtractorMOG

-   示例

```python
import numpy as np, cv2

cap = cv2.VideoCapture('../img/walking.avi')
fps = cap.get(cv2.CAP_PROP_FPS) # 获取帧数
delay = int(1000/fps)

fgbg = cv2.bgsegm.createBackgroundSubtractorMOG()
while cap.isOpened():
    ret, frame = cap.read()
    if not ret:
        break
    fgmask = fgbg.apply(frame)
    cv2.imshow('frame',fgmask)
    cv2.imshow('bg', fgbg.getBackgroundImage())
    if cv2.waitKey(1) & 0xff == 27:
        break
cap.release()
cv2.destroyAllWindows()
```

-   示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_340_0.png)

#### ❖ BackgroundSubtractorMOG2

-   基于两篇论文。
-   Z.Zivkovic, "Improved adaptive Gaussian mixture model for background subtraction" (2004)
-   "Efficient Adaptive Density Estimation per Image Pixel for the Task of Background Subtraction" (2006)。
-   为每个像素选择合适的高斯分布
-   适用于各种场景，尤其对光照变化适应性好
-   `retval = cv.createBackgroundSubtractorMOG2([, history[, varThreshold[, detectShadows]]] )`
    -   `history=500`：历史帧数
    -   `varThreshold=16`：分布阈值
    -   `detectShadows=True`：标记阴影
-   `fgbg.apply(frame)`
    -   `detectShadows` 设为 `True`（基本值）
        -   以灰色标记阴影，处理速度会变慢。
-   示例

```python
import numpy as np, cv2

cap = cv2.VideoCapture('../img/walking.avi')
fps = cap.get(cv2.CAP_PROP_FPS) # 统计帧数
delay = int(1000/fps)
fgbg = cv2.createBackgroundSubtractorMOG2()
while cap.isOpened():
    ret, frame = cap.read()
    if not ret:
        break
    fgmask = fgbg.apply(frame)
    cv2.imshow('frame',frame)
    cv2.imshow('bgsub',fgmask)
    if cv2.waitKey(delay) & 0xff == 27:
        break
cap.release()
cv2.destroyAllWindows()
```

#### ❖ BackgroundSubtractorMOG2

-   示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_342_0.png)

#### ❖ BackgroundSubtractorGMG

-   Andrew B. Godbehere, Akihiro Matsukawa, Ken Goldberg, "Visual Tracking of Human Visitors under Variable-Lighting Conditions for a Responsive Audio Art Installation", 2012。
-   结合统计背景图像估计和像素级贝叶斯分割的算法。
-   用于初始帧（120帧，基本值）的背景建模
    -   前几帧将全黑
-   使用贝叶斯假设识别前景物体
-   对最近的视图差异赋予更高权重以适应光照变化
-   使用形态学滤波器消除噪声（闭运算、开运算）
-   `fgbg = cv2.bgsegm.createBackgroundSubtractorGMG()` # 创建对象
-   `kernel = cv2.getStructuringElement(cv2.MORPH_ELLIPSE,(3,3))`
-   `fgmask = fgbg.apply(frame)`
-   `fgmask = cv2.morphologyEx(fgmask, cv2.MORPH_OPEN, kernel)`

#### ❖ BackgroundSubtractorGMG

-   示例

```python
import numpy as np
import cv2
cap = cv2.VideoCapture('../img/walking.avi')
#fgbg = cv2.createBackgroundSubtractorGMG() # 创建对象
fgbg = cv2.bgsegm.createBackgroundSubtractorGMG() # 创建对象
kernel = cv2.getStructuringElement(cv2.MORPH_ELLIPSE,(3,3))
while(1):
    ret, frame = cap.read()
    fgmask = fgbg.apply(frame)
    fgmask = cv2.morphologyEx(fgmask, cv2.MORPH_OPEN,
kernel) # 滤波以减少噪声
    cv2.imshow('frame',fgmask)
    k = cv2.waitKey(30) & 0xff
    if k == 27:
        break
cap.release()
cv2.destroyAllWindows()
```

-   示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_343_0.png)

#### 光流

-   前一帧与当前帧之间运动方向和距离的分布
-   无需帧内先前信息即可追踪运动的方法
-   稀疏光流（较少使用）
    -   预设部分点进行追踪
    -   Lucas-Kanade：平方方法
-   稠密光流（密集）
    -   计算所有像素的速度
    -   计算量大

![](img/9de0cb7cd12c1232044968590f57e471_344_0.png)

#### Lucas-Kanade

-   LK 算法的 3 个假设
    -   亮度恒定
        -   追踪对象的像素亮度不变
    -   时间持续性
        -   图像中时间流逝快于物体运动
        -   帧间差异不大
    -   空间一致性
        -   相邻点（相邻像素）很可能属于同一物体
        -   并具有相同运动
-   用于小窗口，无法计算大运动
-   使用图像金字塔进行改进

-   OpenCV 函数
    -   `cv2.calcOpticalFlowLK`：不使用图像金字塔
    -   `cv2.calcOpticalFlowPyrLK`：使用图像金字塔
    -   使用 `goodToTrack` 选择的点作为输入

-   `calcOpticalFlowPyrLK(prevImg, nextImg, prevPts, nextPts)`
    -   `prevImg`：前一帧，n*1*2
    -   `nextImg`：下一帧，n*1*2
    -   `prevPts`：可能移动的点，n*1*2
    -   `nextPts`：追踪对象经过的点的位置，None，使用返回值
    -   `winSize`：每个金字塔层级的窗口大小
    -   `maxLevel`：图像金字塔层数，若为 0 则不使用
    -   `criteria`：搜索停止标准

-   返回值：
    -   `nextPts`：追踪对象经过的点的位置
    -   `status`：与 `nextPts` 数量相同
        -   每个索引为 1 或 0，0 表示未找到对应点 (n*1)
    -   `err`：前一帧特征点与当前帧移动距离点之间的距离，用于剔除

-   光流示例 <1/3>

```python
import numpy as np, cv2

cap = cv2.VideoCapture('../img/walking.avi')
fps = cap.get(cv2.CAP_PROP_FPS) # 统计帧数
delay = int(1000/fps)

color = np.random.randint(0,255,(200,3))
lines = None
prevImg = None
##### calcOpticalFlowPyrLK 设置停止条件
termcriteria = (cv2.TERM_CRITERIA_EPS | cv2.TERM_CRITERIA_COUNT, 10, 0.03)
```

#### Lucas-Kanade

-   光流示例 <2/2>

```python
while cap.isOpened():
    ret,frame = cap.read()
    if not ret:
        break
    img_draw = frame.copy()
    if prevImg is None:
        prevImg = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
        lines = np.zeros_like(frame)
        prevPt = cv2.goodFeaturesToTrack(prevImg, 200, 0.01, 10)

    else:
        nextImg = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
        nextPt, status, err = cv2.calcOpticalFlowPyrLK(prevImg, nextImg, prevPt, None, criteria=termcriteria)
        prevMv = prevPt[status==1]
        nextMv = nextPt[status==1]

    for i,(p, n) in enumerate(zip(prevMv, nextMv)):
        px,py = p.ravel()
        nx,ny = n.ravel()
        cv2.line(lines, (px, py), (nx,ny), color[i].tolist(),2)
        cv2.circle(img_draw, (nx,ny), 2, color[i].tolist(), -1)
    img_draw = cv2.add(img_draw, lines)
    prevImg = nextImg
    prevPt = nextMv.reshape(-1,1,2)
    cv2.imshow('OpticalFlow-LK', img_draw)
    key = cv2.waitKey(delay)
    if key == 27 : # Esc: 结束
        break
    elif key == 8: # Backspace: 删除追踪日志
        prevImg = None
cv2.destroyAllWindows()
cap.release()
```

#### Luca—Kaneda

- 流程示例 <Result>

![](img/9de0cb7cd12c1232044968590f57e471_347_0.png)

#### 稠密光流

- cv2.calcOpticalFlowFarneback(prev, next, flow, pyr_scale, levels, winsize, iterations, poly_n, poly_sigma, flags)
  - prev : 前一帧图像
  - next : 下一帧图像
  - flow : 计算结果，与前一帧大小相同，32F类型的结果流，None
  - pyr_scale : 金字塔缩放比例，0.5
  - levels : 金字塔层数
  - winsize : 平均窗口大小，值越大算法越强。
  - iteration : 每层金字塔的重复次数
  - poly_n : 每个像素用于多项式方程的邻近像素数，5或7
  - poly_sigma: 高斯变化中使用的Sigma值，poly_n=5时为1.1，poly_n=7时为1.5
  - flags : 计算模式
    - cv2.OPTFLOW_USE_INITIAL_FLOW: 使用光流值作为初始值
    - cv2.OPTFLOW_FARNEBACK_GAUSSIAN: 使用盒式滤波器代替高斯滤波器
  - 返回值：与输入图像大小相同，每个像素移动的距离值

示例 <1/2>

```python
import cv2, numpy as np

def drawFlow(img,flow,step=16):
    h,w = img.shape[:2]
    idx_y,idx_x = np.mgrid[step/2:h:step,step/2:w:step].astype(np.int)
    indices = np.stack((idx_x,idx_y), axis=-1).reshape(-1,2)

    for x,y in indices:   # Index Tour
        cv2.circle(img, (x,y), 1, (0,255,0), -1)
        dx,dy = flow[y, x].astype(np.int)
        cv2.line(img, (x,y), (x+dx, y+dy), (0,255,0), 2, cv2.LINE_AA)

prev = None # Stored variable of previous frame

cap = cv2.VideoCapture('../img/walking.avi')
fps = cap.get(cv2.CAP_PROP_FPS) # Counting frame number
delay = int(1000/fps)
while cap.isOpened():
    ret,frame = cap.read()
    if not ret: break
    gray = cv2.cvtColor(frame,cv2.COLOR_BGR2GRAY)
    if prev is None:
        prev = gray
    else:
        flow = cv2.calcOpticalFlowFarneback(prev,gray,None,0.5,3,15,3,5,1.1,cv2.OPTFLOW_FARNEBACK_GAUSSIAN)
        drawFlow(frame,flow)
        prev = gray
    cv2.imshow('OpticalFlow-Farneback', frame)
    if cv2.waitKey(delay) == 27:
        break
cap.release()
cv2.destroyAllWindows()
```

示例 <result>

![](img/9de0cb7cd12c1232044968590f57e471_349_0.png)

#### 均值漂移

- 基于密度分布（特征或颜色）追踪感兴趣区域对象
  - 确定初始窗口
  - 计算窗口内数据的质心
  - 将窗口中心移动到质心
  - 重复直到窗口停止移动
- 应用步骤
  - 选择追踪对象，计算HSV颜色的H直方图
  - 反向投影到所有图像的直方图
  - 通过均值漂移从反向投影中追踪移动的对象

![](img/9de0cb7cd12c1232044968590f57e471_350_0.png)

- cv2.meanShift(img, window, criteria)
  - img : 输入图像
  - window : 初始窗口坐标
  - criteria : 停止搜索重复的准则
    - type : cv2.TERM_CRITERIA_*
      - EPS : 当精度小于epsilon时
      - MAX_ITER : 重复max_iter次
      - COUNT : 与MAX_ITER相同
    - max_iter : 最大重复次数
    - epsilon : 最小精度

- 示例

```python
import numpy as np, cv2

roi_hist = None     # Histogram storage variable for chasing objects
win_name = 'MeanShift Tracking'
termination = (cv2.TERM_CRITERIA_EPS | cv2.TERM_CRITERIA_COUNT, 10, 1)

cap = cv2.VideoCapture(0)
while cap.isOpened():
    ret, frame = cap.read()
    img_draw = frame.copy()

    if roi_hist is not None:  # Histogram for chasing object registered
        hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)
        dst = cv2.calcBackProject([hsv], [0], roi_hist, [0,180], 1)
        ret, (x,y,w,h) = cv2.meanShift(dst, (x,y,w,h), termination)
        cv2.rectangle(img_draw, (x,y), (x+w, y+h), (0,255,0), 2)
        result = np.hstack((img_draw, cv2.cvtColor(dst, cv2.COLOR_GRAY2BGR)))
    else:  # Histogram for chasing object not registered
        cv2.putText(img_draw, "Hit the Space to set target to track", (10,30),cv2.FONT_HERSHEY_SIMPLEX, 1, (0,0,255), 1, cv2.LINE_AA)
        result = img_draw
```

- 示例 <Result>

![](img/9de0cb7cd12c1232044968590f57e471_352_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_352_1.png)

#### 连续自适应均值漂移

- 连续自适应均值漂移
- 改进均值漂移的窗口固定问题
- 使用均值漂移计算中心点
- 重置窗口大小和方向

- cv2.CamShift(img, window, criteria)
  - img : 输入图像
  - window : 初始搜索窗口
  - criteria : 停止搜索的准则

- 示例 <1/2>

```python
import numpy as np, cv2

roi_hist = None     # Saved variables for detecting object histogram
win_name = 'MeanShift Tracking'
termination = (cv2.TERM_CRITERIA_EPS | cv2.TERM_CRITERIA_COUNT, 10, 1)

cap = cv2.VideoCapture(0)
cap.set(cv2.CAP_PROP_FRAME_WIDTH, 640)
cap.set(cv2.CAP_PROP_FRAME_HEIGHT, 480)
while cap.isOpened():
    ret, frame = cap.read()
    img_draw = frame.copy()

    if roi_hist is not None:  # detecting object histogram registered
        hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)
        dst = cv2.calcBackProject([hsv], [0], roi_hist, [0,180], 1)
        ret, (x,y,w,h) = cv2.CamShift(dst, (x,y,w,h), termination)
        cv2.rectangle(img_draw, (x,y), (x+w, y+h), (0,255,0), 2)
        result = np.hstack((img_draw, cv2.cvtColor(dst, cv2.COLOR_GRAY2BGR)))
    else:  # detecting object histogram not registered
        cv2.putText(img_draw, "Hit the Space to set target to track",
            (10,30),cv2.FONT_HERSHEY_SIMPLEX, 1, (0,0,255), 1, cv2.LINE_AA)
        result = img_draw
    cv2.imshow(win_name, result)
    key = cv2.waitKey(1) & 0xff
    if key == 27: # Esc
        break
    elif key == ord(' '): # Space bar, Set ROI
        x,y,w,h = cv2.selectROI(win_name, frame, False)
        if w and h:    # ROI is set properly
            roi = frame[y:y+h, x:x+w]
            roi = cv2.cvtColor(roi, cv2.COLOR_BGR2HSV)
            mask = None
            roi_hist = cv2.calcHist([roi], [0], mask, [180], [0,180])
            cv2.normalize(roi_hist, roi_hist, 0, 255, cv2.NORM_MINMAX)
        else:           # ROI is not set properly
            roi_hist = None
else:
    print('no camera!')
cap.release()
cv2.destroyAllWindows()
```

- 示例 <result>

![](img/9de0cb7cd12c1232044968590f57e471_354_0.png)

#### 跟踪API

- 添加到OpenCV3 contrib
- 跟踪API由机器学习算法操作
- 即使用户不了解算法，也可以在界面中选择各种算法来选择跟踪器
- retval = cv.Tracker.init(image, boundingBox) : 初始化跟踪器
  - image : 输入图像
  - boundingBox : 要检测对象的坐标 (x,y,w,h)
- retval, boundingBox = cv.Tracker.update(image) : 在新帧中查找要检测对象的位置
  - image : 新帧图像
  - retval : 检测通过/失败
  - boundingBox : 检测到的对象在新帧中的新位置 (x,y,w,h)

- 跟踪器生成器
  - tracker = cv2.TrackerBoosting_create()
    基于AdaBoot算法
  - tracker = cv2.TrackerMIL_create()
    基于MIL（多实例学习）算法
  - tracker = cv2.TrackerKCF_create()
    基于KCF（核化相关滤波器）算法
  - tracker = cv2.TrackerTLD_create()
    基于TLD（跟踪、学习和检测）算法
  - tracker = cv2.TrackerMedianFlow_create()
    通过检测前后方向来测量对象的不匹配
  - tracker = cv2.TrackerGOTURN_create()
    基于CNN（卷积神经网络），在OpenCV 3.4版本中无法工作；它被识别为错误
  - tracker = cv.TrackerCSRT_create()
    CSRT（通道和空间可靠性）
  - tracker = cv.TrackerMOSSE_create()
    内部使用灰度图

- 示例 <1/3>

```python
import cv2
trackers = [cv2.TrackerBoosting_create,
            cv2.TrackerMIL_create,
            cv2.TrackerKCF_create,
            cv2.TrackerTLD_create,
            cv2.TrackerMedianFlow_create,
            cv2.TrackerGOTURN_create, #Error
            cv2.TrackerCSRT_create,
            cv2.TrackerMOSSE_create]
trackerIdx = 0  # Selection index for tracker generator function
tracker = None
isFirst = True
video_src = "../img/highway.mp4"
cap = cv2.VideoCapture(video_src)
fps = cap.get(cv2.CAP_PROP_FPS) # Get frame number
delay = int(1000/fps)
win_name = 'Tracking APIs'
```

#### 跟踪 API

- 示例 <2/3>

```python
while cap.isOpened():
    ret, frame = cap.read()
    if not ret:
        print('Cannot read video file')
        break
    img_draw = frame.copy()
    if tracker is None: # When tracker is not generated
        cv2.putText(img_draw, "Press the Space to set ROI!!",
            (100,80), cv2.FONT_HERSHEY_SIMPLEX, 0.75,(0,0,255),2,cv2.LINE_AA)
    else:
        ok, bbox = tracker.update(frame)
        (x,y,w,h) = bbox
        if ok:
            cv2.rectangle(img_draw, (int(x), int(y)), (int(x + w), int(y + h)), (0,255,0), 2, 1)
        else :
            cv2.putText(img_draw, "Tracking fail.", (100,80),
                cv2.FONT_HERSHEY_SIMPLEX,
                0.75,(0,0,255),2,cv2.LINE_AA)
            trackerName = tracker.__class__.__name__
            cv2.putText(img_draw, str(trackerIdx) + ":"+trackerName , (100,20), cv2.FONT_HERSHEY_SIMPLEX,
                0.75, (0,255,0),2,cv2.LINE_AA)
    cv2.imshow(win_name, img_draw)
    key = cv2.waitKey(delay) & 0xff
    if key == ord(' ') or (video_src != 0 and isFirst):
        isFirst = False
        roi = cv2.selectROI(win_name, frame, False)
        if roi[2] and roi[3]:
            tracker = trackers[trackerIdx]()
            isInit = tracker.init(frame, roi)
```

- 示例 <3/3>

```python
elif key in range(48, 56):
    trackerIdx = key-48
    if bbox is not None:
        tracker = trackers[trackerIdx]()
        isInit = tracker.init(frame, bbox)
elif key == 27 :
    break
else:
    print( "Could not open video")
cap.release()
cv2.destroyAllWindows()
```

- 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_357_0.png)

### 匹配与跟踪

1. 与相似图像匹配
2. 特征与关键点
3. 描述符提取器
4. 特征匹配
5. 跟踪
6. **实践环节**

#### ❖ 全景图像

- 起草一个程序，用两张连续的照片制作全景图像。
- 结果示例

![](img/9de0cb7cd12c1232044968590f57e471_359_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_359_1.png)

![](img/9de0cb7cd12c1232044968590f57e471_359_2.png)

#### ❖ 全景图像

- 提示
  - 确定左右连接顺序，检测特征和描述符
  - 将左图匹配到右图后，计算距离变换矩阵
  - `cv2.warpPerspective()`：距离变换
  - 生成与两张图像一样大的尺寸后，在每个位置合成图像

#### ❖ 书籍封面搜索器

- 将待搜索的书籍封面放置好，点击空格键打印原始图像
- 结果示例

![](img/9de0cb7cd12c1232044968590f57e471_360_0.png)

- 结果示例

![](img/9de0cb7cd12c1232044968590f57e471_360_1.png)

#### ❖ 书籍封面搜索器

- 提示
  - 标记摄像头的方形区域
  - 在书籍封面上方点击空格键，在方形区域内设置 ROI 后，检测特征和描述符
  - 逐一读取对象目录文件，并检测每个对象的特征和描述符
  - 检测搜索图像并进行匹配和良好匹配
  - 通过执行距离比运算，保存归一化比率
  - 所有匹配完成后，将结果按归一化比率从高到低排序，并打印出最高的结果

### 机器学习

1. **机器学习**
2. K-均值聚类
3. k-近邻
4. SVM 与 HOG
5. 级联分类器
6. 实践环节

#### 人工智能

- 1956年，美国达特茅斯大学的约翰·麦卡锡教授在一次会议上首次使用了“人工智能”一词
- 通用人工智能：像人类一样思考的 AI
- 专用人工智能：仅执行特定功能的 AI，例如图像分类、人脸识别等

### 机器学习

- 使用算法分析数据，学习分析后的数据，并根据学习到的细节进行判断或预测
- 多种算法

#### 深度学习

- 机器学习的一种算法
- 人工神经网络：受人脑运作方式启发的网络；它需要大量的计算，因此需要大量时间和高性能设备。
- 2012年，谷歌和斯坦福大学的吴恩达教授实现了深度神经网络。

### 机器学习

- 通过数据的预处理过程，将数据转换为特征，并通过生成的特征向量构建模型。
- 通过模型分析各种算法、权重、阈值和其他参数的特性，以实现目标。
- 在数据集中，90% 用于训练集，10% 用于测试集

#### 监督学习

- 使用带有标签（答案）的数据进行学习
- 分类：名称到标签
- 回归：数值标签
- 线性回归：学习时间到分数（0~100）
  - `H(x) = Wx + b`，H：假设，W：权重，b：偏置
- 二元分类：学习时间到通过/不通过
- 多标签分类：学习时间到 GPA（A, B, C, D, F）

#### 无监督学习

- 无标签学习
- 聚类：只关心数据被分到哪个组

#### OpenCV 的实现

- 旧版接口：在标准化接口设计之前就已实现
  - 核心模块
    - k-均值，马氏距离
- 标准化接口
  - `ml` 模块，运行 `StatModel` 抽象类继承
    - 正态贝叶斯（正态/纯贝叶斯分类器）
    - 决策树
    - 提升方法
    - 随机树
    - 期望值最大化
    - k-近邻
    - 多层感知机（MLP，多层感知器）
    - 支持向量机（SVM）
- 目标检测器
  - 使用学习方法的高级类
    - `CascadeClassifier`，潜在 SVM（`DPMDetector`），词袋模型

### 机器学习

1. 机器学习
2. **K-均值聚类**
3. k-近邻
4. SVM 与 HOG
5. 级联分类器
6. 实践环节

#### K-均值聚类

- 聚类算法，无监督学习
- 按需聚类混合数据
- 算法
  - 步骤 1.
    - 随机设置两个中心点（C1, C2）。
  - 步骤 2.
    - 计算到两个中心点的距离。
    - 如果测试数据靠近 C1，则标记为 0；靠近 C2，则标记为 1。
    - 如果中心点是 C3, C4，则标记为 2, 3。
  - 步骤 3
    - 将靠近 C1 的标记为 0 的数据点画成红色，标记为 1 的数据点画成蓝色。

![](img/9de0cb7cd12c1232044968590f57e471_366_0.png)

#### K-均值聚类

- 算法
  - 步骤 4.
    - 计算红色和蓝色数据点的平均值，将其设置为新的中心点，并像步骤 2 一样计算距离并标记为 0, 1。
    - 重复步骤 2 到 4，直到每个中心点固定为中心点。
    - 或者，重复直到满足特定条件；设定给定的重复次数或精度。
    - 中心点是数据点距离之和最小的点。

有各种获取初始中心点、重复计算等的算法。

![](img/9de0cb7cd12c1232044968590f57e471_367_0.png)

```
minimize [ J = \sum_{All Red\_Points} distance(C1, Red\_Point) + \sum_{All Blue\_Points} distance(C2, Blue\_Point) ]
```

#### K-均值聚类

- `cv2.kmeans(data, K, bestLabels, criteria, attempts, flags)` : retval, bestLabels, centers
  - data : `np.float32`，应保存为单列
  - k : 期望的聚类数量
  - bestLabels : 结果数据，None
  - criteria : 终止条件，元组 (type, max_iter, epsilon)
  - attempts : 通过不同初始标签运行的次数
  - flags : 选择初始中心点，
    - `cv2.KMEANS_PP_CENTERS`
    - `cv2.KMEANS_RANDOM_CENTERS`
    - `cv2.KMEANS_USE_INITIAL_LABELS`
- 返回值：
  - retval : 中心点与每个数据点距离之和的平方，
  - bestLabels : 标签，0, 1, ...
  - centers : 聚类中心点数组
- 随机数聚类示例

```python
import numpy as np, cv2
import matplotlib.pyplot as plt

x = np.random.randint(0,150,(25,2))
y = np.random.randint(128, 255,(25,2))
z = np.vstack((x,y)).astype(np.float32)

criteria = (cv2.TERM_CRITERIA_EPS + cv2.TERM_CRITERIA_MAX_ITER, 10, 1.0)
ret,label,center=cv2.kmeans(z,2,None,criteria,10,cv2.KMEANS_RANDOM_CENTERS)

a = z[label.ravel()==0]
b = z[label.ravel()==1]
plt.scatter(a[:,0],a[:,1], c='b')
plt.scatter(b[:,0],b[:,1], c='r')
plt.scatter(center[:,0],center[:,1],s = 80,c = 'y', marker = 's')
plt.show()
```

#### ❖ K-Means 聚类

- 随机数聚类示例 <result>

![](img/9de0cb7cd12c1232044968590f57e471_369_0.png)

#### ❖ K-Means 聚类

- 图像颜色聚类示例

```python
import numpy as np
import cv2
img = cv2.imread('../img/taekwonv1.jpg')

z = img.reshape((-1,3))
z = np.float32(z)

criteria=(cv2.TERM_CRITERIA_EPS +cv2.TERM_CRITERIA_MAX_ITER,10,1.0)
K = 8
ret,label,center=cv2.kmeans(z,K,None,criteria,10,cv2.KMEANS_RANDOM_CENTERS)
center = np.uint8(center)
res = center[label.flatten()]
res2 = res.reshape((img.shape))
merged = np.hstack((img, res2))
cv2.imshow('KMeans Color',merged)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

- 16色聚类

![](img/9de0cb7cd12c1232044968590f57e471_370_0.png)

### 机器学习

1. 机器学习
2. K-Means 聚类
3. **k-NN**
4. SVM 与 HOG
5. 级联分类器
6. 实践工作坊

#### k-NN（k-近邻算法）

- 如果将一个新点输入到已分为两组的现有点中，新点将如何被分类？
- 分类到最近的成员
- K 用于设定最近成员的范围
- 你可以为较近的成员和较远的成员赋予不同的权重。
- knn = cv2.ml.KNearest_create()
- knn.train(trainData, cv2.ml.ROW_SAMPLE, responses)
- ret, results, neighbours ,dist = knn.findNearest(newcomer, 3 )

![](img/9de0cb7cd12c1232044968590f57e471_372_0.png)

#### k-NN（k-近邻算法）

- 简单示例

```python
import cv2, numpy as np, matplotlib.pyplot as plt

trainData = np.random.randint(0,100,(25,2)).astype(np.float32)
responses = np.random.randint(0,2,(25,1))#.astype(np.float32)
red = trainData[responses.ravel()==0]
plt.scatter(red[:,0],red[:,1],80,'r','^')
blue = trainData[responses.ravel()==1]
plt.scatter(blue[:,0],blue[:,1],80,'b','s')
newcomer = np.random.randint(0,100,(1,2)).astype(np.float32)
plt.scatter(newcomer[:,0],newcomer[:,1],80,'g','o')
knn = cv2.ml.KNearest_create()
knn.train(trainData, cv2.ml.ROW_SAMPLE, responses)
#ret, res = knn.predict(newcomer)
ret, results, neighbours ,dist = knn.findNearest(newcomer, 3)#K=3
plt.annotate('red' if ret==0.0 else 'blue', xy=newcomer[0],
xytext=(newcomer[0]+1))
print( "result, 0=red, 1=blue:  {}\n".format(results) )
print( "neighbours:  {}\n".format(neighbours) )
print( "distance:  {}\n".format(dist) )
plt.show()
```

- 简单示例 <result>

```
result, 0=red, 1=blue:  [[0.]]
neighbours:  [[0. 0. 1.]]
distance:  [[ 74.  97. 130.]]
```

![](img/9de0cb7cd12c1232044968590f57e471_373_0.png)

#### k-NN（k-近邻算法）

- 移动分类示例

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

trainData = np.random.randint(0,100,(25,2)).astype(np.float32)
responses = (trainData[:, 0] >trainData[:,1]).astype(np.float32)

romantic = trainData[responses==1]
action = trainData[responses==0]

plt.scatter(action[:,0],action[:,1],s=80,marker='^', c="blue",
label='action')
plt.scatter(romantic[:,0],romantic[:,1], s=80, c='m', marker='o',
label="romantic")
newcomer = np.random.randint(0,100,(1,2)).astype(np.float32)
plt.scatter(newcomer[:,0],newcomer[:,1],200,'r','*', label="new")
knn = cv2.ml.KNearest_create()
knn.train(trainData, cv2.ml.ROW_SAMPLE, responses)
ret, results, neighbours ,dist = knn.findNearest(newcomer, 3)#K=3

print( "result(0=action, 1:romantic):  {}\n".format(results) )
print( "neighbours:  {}\n".format(neighbours) )
print( "distance:  {}\n".format(dist) )
label = results == 1 and "romantic" or "action"

anno_x, anno_y = newcomer.ravel()
plt.annotate(label, xy=(anno_x + 1, anno_y+1), xytext=(anno_x+5,
anno_y+10), arrowprops={'color':'green'})
plt.xlabel('kiss count')
plt.ylabel('hit count')
plt.legend(loc="upper right")
plt.show()
```

#### k-NN（k-近邻算法）

- 移动分类示例 <Result>

![](img/9de0cb7cd12c1232044968590f57e471_375_0.png)

- 手写识别
  - 5000 个手写图像字母
  - 每个数字（0 ~ 9）500 个字母
  - 每个字母 20 x 20 = 400 像素
  - 5000 X 400 向量
    - 4500：训练集
    - 500：测试集

![](img/9de0cb7cd12c1232044968590f57e471_375_1.png)

#### k-NN（k-近邻算法）

- 手写识别训练

```python
import numpy as np, cv2

def load():
    image = cv2.imread('../img/digits.png')
    gray = cv2.cvtColor(image,cv2.COLOR_BGR2GRAY)

    cells = [np.hsplit(row,100) for row in np.vsplit(gray,50)]
    x = np.array(cells)

    train = x[:,:90]
    test = x[:,90:100]
    train = train.reshape(-1,400).astype(np.float32) # Size = (4500,400)
    test = test.reshape(-1,400).astype(np.float32) # Size = (500,400)

    k = [0,1,2,3,4,5,6,7,8,9]
    train_labels = np.repeat(k,450).reshape(-1,1)
    test_labels = np.repeat(k,50).reshape(-1,1)
    return (train, train_labels, test, test_labels)

if __name__ == '__main__':
    train, train_labels, test, test_labels = load()
    knn = cv2.ml.KNearest_create()
    knn.train(train, cv2.ml.ROW_SAMPLE, train_labels)

    #ret, result = knn.predict(test, k=3)
    ret, result, neighbors, distance = knn.findNearest(test, k=3)

    correct = np.sum(result == test_labels)
    accuracy = correct * (100.0 / result.size)
    print("correct : %d/%d , Accuracy :%.2f%%" % (correct, result.size, accuracy) )
    #knn.save('digits.yaml') # but knn.load(..) is not implemented
```

#### k-NN（k-近邻算法）

- 手写识别训练

```
correct : 475/500 , Accuracy :95.00%
```

- 手写识别 <1/3>

```python
import numpy as np, cv2
import knn_mnist_train

def flatten_img(src):
    h, w = src.shape[:2]
    square = src
    if h > w:
        pad = (h - w)//2
        square = np.zeros((h, h), dtype=np.uint8)
        square[:, pad:pad+w] = src
    elif w > h :
        pad = (w - h)//2
        square = np.zeros((w, w), dtype=np.uint8)
        square[pad:pad+h, :] = src
    px20 = np.zeros((20,20), np.uint8)
    px20[2:18, 2:18] = cv2.resize(square, (16,16),
interpolation=cv2.INTER_AREA)
    flatten = px20.reshape((1,400)).astype(np.float32)
    return flatten
knn = cv2.ml.KNearest_create()
train, train_labels = knn_mnist_train.load()[:2]
knn.train(train, cv2.ml.ROW_SAMPLE, train_labels)
image = cv2.imread('../img/4027.png')
cv2.imshow("image", image)
cv2.waitKey(0)
```

#### k-NN（k-近邻算法）

- 手写识别 <2/3>

```python
gray = cv2.cvtColor(image,cv2.COLOR_BGR2GRAY)
gray = cv2.GaussianBlur(gray, (5, 5), 0)
_, gray = cv2.threshold(gray, 127, 255, cv2.THRESH_BINARY_INV)
img, contours, _ = cv2.findContours(gray, cv2.RETR_EXTERNAL,
    cv2.CHAIN_APPROX_SIMPLE)
for c in contours:
    (x, y, w, h) = cv2.boundingRect(c)
    if w >= 5 and h >= 25:
        roi = gray[y:y + h, x:x + w]
        cv2.rectangle(image, (x, y), (x + w, y + h), (0, 255, 0), 1)
        flatten = flatten_img(roi)
        ret, result, neighbours, dist = knn.findNearest(flatten, k=1)
        cv2.putText(image, "%d"%ret, (x , y + 155),
            cv2.FONT_HERSHEY_COMPLEX, 2, (255, 0, 0), 2)
cv2.imshow("image", image)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

- 手写识别 <Result>

![](img/9de0cb7cd12c1232044968590f57e471_378_0.png)

### 机器学习

1. 机器学习
2. K-Means 聚类
3. k-NN
4. **SVM 与 HOG**
5. 级联分类器
6. 实践工作坊

#### 支持向量机

- 分类方法之一
- k-NN 需要计算所有数据的距离，这会消耗大量时间和内存
- 区分参数，寻找直线并基于此识别模式
    - SVM、NuSVM、LinearSVM（专注于线性核）
- 能够将数据分为两类的直线称为决策边界，被分类的数据称为线性可分

![](img/9de0cb7cd12c1232044968590f57e471_380_0.png)

- 在所有 SVM 参数中首先寻找直线，因为它对噪声具有鲁棒性
- 无需训练数据；如果数据接近对立类别，就足够了
- 支持向量：蓝色实心圆和红色实心方块
- 支持平面：通过的直线
- 蓝色圆圈：Wx + b > 1
- 红色数据：Wx + b < -1
    - W：权重，决定方向的向量
    - b：偏置，决定位置
- 决策边界：
    - 穿过两个超平面的中心点
    - Wx +b = 0

![](img/9de0cb7cd12c1232044968590f57e471_380_1.png)

#### SVM

- 示例 <1/2>

```python
import cv2
import numpy as np
import matplotlib.pylab as plt

x = np.random.randint(0,158,(25,2))
y = np.random.randint(98, 255,(25,2))
trainData = np.vstack((x,y))
trainData = np.float32(trainData)

responses = np.zeros((50,1), np.int32)
responses[0:25] = 1

red = trainData[responses.ravel()==0]
plt.scatter(red[:,0],red[:,1],80,'r','^')
blue = trainData[responses.ravel()==1]
plt.scatter(blue[:,0],blue[:,1],80,'b','s')

newcomer = np.random.randint(0,255,(1,2)).astype(np.float32)
plt.scatter(newcomer[:,0],newcomer[:,1],80,'g','o')

C=1
model = cv2.ml.SVM_create()
model.trainAuto(trainData, cv2.ml.ROW_SAMPLE, responses)

ret, results = model.predict(newcomer)
plt.annotate('red' if results[0]==0.0 else 'blue', xy=newcomer[0],
xytext=(newcomer[0]+1))
print("result, 0=red, 1=blue:  {}\n".format(results))

plt.show()
```

#### SVM

- 示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_382_0.png)

#### HOG

- 方向梯度直方图特征描述符
- 边缘斜率方向的直方图
- 建议用于识别行人
- 归一化直方图

![](img/9de0cb7cd12c1232044968590f57e471_382_1.png)

![](img/9de0cb7cd12c1232044968590f57e471_382_2.png)

- hogDesc = cv2.HOGDescriptor(winSize,blockSize,blockStride,cellSize,nbins)
  - winSize : HOG 提取区域的大小
  - blockSize : 归一化区域的大小
  - blockStride : 归一化重叠块的大小
  - cellSize : HOG 计算单元的大小
  - nbins : 直方图级别数
  - hogDesc : HOG 专用描述符
    - 向量大小：
      $$nbins \times (\frac{blockSize}{cellSize})^2 \times (\frac{(winSize - blockSize)}{blockStride} + 1)^2$$

#### ❖ HOG SVM

- MNIST 手写数字研究 <1/3>

```python
import cv2 as cv
import numpy as np

winSize = (20,20)
blockSize = (10,10)#blockSize = (8,8)
blockStride = (5,5)#blockStride = (4,4)#half of blockSize
cellSize = (10,10)#cellSize = (8,8)
nbins = 9
hogDesc = cv.HOGDescriptor(winSize,blockSize,blockStride,cellSize,nbins)

affine_flags = cv.WARP_INVERSE_MAP|cv.INTER_LINEAR
def deskew(img):
    m = cv.moments(img)
    if abs(m['mu02']) < 1e-2:
        return img.copy()
    skew = m['mu11']/m['mu02']
    M = np.float32([[1, skew, -0.5*20*skew], [0, 1, 0]])
    img = cv.warpAffine(img,M,(20, 20),flags=affine_flags)
    return img
```

#### HOG SVM

- MNIST 手写数字研究 <2/3>

```python
if __name__ == '__main__':
    img = cv.imread('../img/digits.png',0)
    cells = np.array([np.hsplit(row,100) for row in np.vsplit(img,50)])
    train_cells = cells[:, :90]
    test_cells = cells[:, 90:100]

    deskewed = [list(map(deskew,row)) for row in train_cells]
    hogdata = [list(map(hogDesc.compute,row)) for row in deskewed]
    trainData = np.float32(hogdata)
    print(trainData.shape)
    trainData = trainData.reshape(-1,trainData.shape[2])

    responses = np.repeat(range(10),450).reshape(-1, 1)
    svm = cv.ml.SVM_create()

    '''
    svm.setKernel(cv.ml.SVM_RBF)
    svm.setType(cv.ml.SVM_C_SVC)
    svm.setC(12.5)
    svm.setGamma(0.50625)
    svm.train(trainData, cv.ml.ROW_SAMPLE, responses)
    '''

    svm.trainAuto(trainData, cv.ml.ROW_SAMPLE, responses)
    svm.save('svm_data.yml')

    deskewed = [list(map(deskew,row)) for row in test_cells]
    hogdata = [list(map(hogDesc.compute,row)) for row in deskewed]
    testData = np.float32(hogdata)
    testData = testData.reshape(-1,testData.shape[2])
    test_labels = np.repeat(range(10),50).reshape(-1,1)
    ret, result = svm.predict(testData)
    correct = (result==test_labels).sum()
    print(correct*100.0/result.size)
```

#### HOG SVM

- MNIST 手写数字研究 <结果>

准确率：97.4%

- 识别手写数字 <1/3>

```python
import cv2
import numpy as np
import svm_mnist_train

#svm=ocr.svm
#svm = cv2.ml.SVM_create()
#svm = svm.load('./svm_data.yml')
svm = cv2.ml.SVM_load('./svm_data.yml')

def hog_img(src):
    h, w = src.shape[:2]
    square = src
    if h > w:
        pad = (h - w)//2
        square = np.zeros((h, h), dtype=np.uint8)
        square[:, pad:pad+w] = src
    elif w > h :
        pad = (w - h)//2
        square = np.zeros((w, w), dtype=np.uint8)
        square[pad:pad+h, :] = src
    px20 = np.zeros((20,20), np.uint8)
    px20[2:18, 2:18] = cv2.resize(square, (16,16),
    interpolation=cv2.INTER_AREA)
```

#### HOG SVM

- 识别手写数字 <2/3>

```python
    deskewed = svm_mnist_train.deskew(px20)
    hogdata = svm_mnist_train.hogDesc.compute(deskewed)
    testData = np.float32(hogdata).reshape(-1, hogdata.shape[0])
    return testData

image = cv2.imread('../img/4027.png')
cv2.imshow("image", image)
cv2.waitKey(0)

gray = cv2.cvtColor(image,cv2.COLOR_BGR2GRAY)
gray = cv2.GaussianBlur(gray, (5, 5), 0)
_, gray = cv2.threshold(gray, 127, 255, cv2.THRESH_BINARY_INV)
img, contours, _ = cv2.findContours(gray, cv2.RETR_EXTERNAL,
cv2.CHAIN_APPROX_SIMPLE)
for c in contours:
    (x, y, w, h) = cv2.boundingRect(c)
    if w >= 5 and h >= 25:
        roi = gray[y:y + h, x:x + w]
        cv2.rectangle(image, (x, y), (x + w, y + h), (0, 255, 0),
1)
        testData = hog_img(roi)
        ret, result = svm.predict(testData)
        cv2.putText(image, "%d"%result[0], (x , y + 155),
cv2.FONT_HERSHEY_COMPLEX, 2, (255, 0, 0), 2)
        cv2.imshow("image", image)
        cv2.waitKey(0)
cv2.destroyAllWindows()
```

#### HOG SVM

- 识别手写数字 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_387_0.png)

#### 行人检测

- HOG 训练目标以检测行人
  - cv2.HOGDescriptor_getDefaultPeopleDetector()
  - cv2.HOGDescriptor_getDaimlerPeopleDetector()
  - cv2.HOGDescriptor.setSVMDetector()
  - cv2.HOGDescriptor.detectMultiScale
- 检测行人

```python
import cv2
hogdef = cv2.HOGDescriptor()
hogdef.setSVMDetector(cv2.HOGDescriptor_getDefaultPeopleDetector())
cap = cv2.VideoCapture('../img/walking.avi')
while cap.isOpened():
    ret, img = cap.read()
    if ret is None:
        print('no frame')
        break
    found, _ = hogdef.detectMultiScale(img)
    for (x,y,w,h) in found:
        cv2.rectangle(img, (x,y), (x+w, y+h), (0,255,255))
    cv2.imshow('frame', img)
    if cv2.waitKey(1) == 27:
        break
cap.release()
cv2.destroyAllWindows()
```

#### ❖ 行人检测

- 检测行人

![](img/9de0cb7cd12c1232044968590f57e471_388_0.png)

### 机器学习

1. 机器学习
2. K-Means 聚类
3. k-NN
4. SVM 和 HOG
5. **级联分类器**
6. 实践

#### 级联

- 基于树的对象分割器
- 在识别人脸方面非常有效
- 使用 Haar 特征，通过加减特定的四边形区域
- 提供训练器和检测器
- 检测器：识别预训练的对象文件
  - haarcascades.xml
- 训练器：使用新图像进行训练

![](img/9de0cb7cd12c1232044968590f57e471_390_0.png)

#### 级联

- classifier = cv2.CascadeClassifier('file.xml')
- classifier.detectMultiScale(img, scaleFactor, minNeighbors[, flags, minSize, maxSize])
  - img : 输入图像
  - scaleFactor: 限制图像放大比例，通常为 1.3~1.5
    - 用于尺度金字塔；如果值较大，识别机会增加，但速度会变慢
  - minNeighbors: 需要保留的像素数量
    - 如果数值较大，质量会更好，但检测数量会减少
  - flags : 用于旧版 API；现在不使用
  - minSize, maxSize: 如果检测区域超过给定大小，则忽略检测

#### 级联

- 人脸检测示例

```python
import cv2
img = cv2.imread('../img/children.jpg')
face_cascade = cv2.CascadeClassifier('../data/haarcascade_frontalface_default.xml')
eye_cascade = cv2.CascadeClassifier('../data/haarcascade_eye.xml')
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
faces = face_cascade.detectMultiScale(gray)
for (x,y,w,h) in faces:
    cv2.rectangle(img,(x,y),(x+w,y+h),(255,0,0),2)
    roi_gray = gray[y:y+h, x:x+w]
    roi_color = img[y:y+h, x:x+w]
    eyes = eye_cascade.detectMultiScale(roi_gray)
    for (ex,ey,ew,eh) in eyes:
        cv2.rectangle(roi_color,(ex,ey),(ex+ew,ey+eh),(0,255,0),1)
cv2.imshow('children',img)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

- 人脸检测示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_391_0.png)

#### 级联分类器

- 人脸检测示例

```python
cascade_xml = '../data/haarcascade_frontalface_default.xml'
cascade = cv2.CascadeClassifier(cascade_xml)
eye_cascade = cv2.CascadeClassifier('../data/haarcascade_eye.xml')
cam = cv2.VideoCapture(0)
while True:
    ret, img = cam.read()
    if not ret:
        print('no frame')
        break
    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
    faces = cascade.detectMultiScale(gray, 1.3, 5)
    for (x, y, w, h) in faces:
        cv2.rectangle(img, (x, y), (x+w, y+h), (0, 255, 0), 2)
        roi = gray[y:y+h, x:x+w]
        eyes = eye_cascade.detectMultiScale(roi)
        for (ex, ey, ew, eh) in eyes:
            cv2.rectangle(img[y:y+h, x:x+w], (ex, ey), (ex+ew, ey+eh), (255, 0, 0), 2)
    cv2.imshow('facedetect', img)
    if 0xFF & cv2.waitKey(5) == 27:
        break
cv2.destroyAllWindows()
```

- 人脸检测示例 <结果>

![](img/9de0cb7cd12c1232044968590f57e471_392_0.png)

#### 人脸识别

- 识别你自己的脸
  - 从多个角度拍摄你自己的照片
  - 使用拍摄的照片进行训练
    - `model = cv2.face.LBPHFaceRecognizer_create()`
    - `model.train(train_data, label_data)`
  - 将训练结果保存为xml文件
    - `model.write('file.xml')`
  - 读取保存的xml文件以识别人脸
    - `model = cv2.face.LBPHFaceRecognizer_create()`
    - `model.read('file.xml')`
  - 结果：`model.predict(face)`
    - 结果：`(label, confidence)`
      - `label`：与注册照片最相似的照片标签
      - `confidence`：距离（0：100%，∞：0%）

#### ❖ 识别人脸

- 拍照
  - 输入姓名和ID（数字）
  - 拍摄100张照片
  - 包括正面角度，从多种角度拍摄
  - 各种表情

![](img/9de0cb7cd12c1232044968590f57e471_394_0.png)

![](img/9de0cb7cd12c1232044968590f57e471_394_1.png)

![](img/9de0cb7cd12c1232044968590f57e471_394_2.png)

![](img/9de0cb7cd12c1232044968590f57e471_394_3.png)

![](img/9de0cb7cd12c1232044968590f57e471_394_4.png)

![](img/9de0cb7cd12c1232044968590f57e471_394_5.png)

#### ❖ 识别人脸

- 训练

#### ❖ 识别人脸

- 结果

![](img/9de0cb7cd12c1232044968590f57e471_395_0.png)

### 机器学习

1. 机器学习
2. K-均值聚类
3. k-近邻算法
4. 支持向量机与方向梯度直方图
5. 级联分类器
6. **实践工作坊**

#### 人脸马赛克

- 为人脸照片添加自动马赛克效果。
- 结果示例：

![](img/9de0cb7cd12c1232044968590f57e471_397_0.png)

#### 汉尼拔面具

- 将《沉默的羔羊》中的汉尼拔面具放置在从摄像头拍摄的人脸上
- 结果示例：

![](img/9de0cb7cd12c1232044968590f57e471_397_1.png)

#### ❖ 人脸扭曲

- 制作一个像Snapchat或Snow那样扭曲人脸的摄像头
- 示例：

![](img/9de0cb7cd12c1232044968590f57e471_398_0.png)

## Python与OpenCV3