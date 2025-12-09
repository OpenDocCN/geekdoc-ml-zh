# IDE 设置

设置你的交互式开发环境（IDE）是实验室序列成功的关键第一步。与基于云的机器学习开发不同，其中基础设施被抽象化，嵌入式系统要求你理解从代码编译到硬件部署的完整工具链。这个动手设置过程介绍了嵌入式开发工作流程的基本概念，同时为实验室练习准备你的工作站。

环境设置通常需要 30-60 分钟，具体取决于平台选择和互联网连接速度。以下程序旨在由没有先前嵌入式系统经验的学生完成，每一步都构建了后续实验室工作所需的技能。

完成硬件选择后，如 硬件套件 章节中所述，这些程序将建立嵌入式机器学习编程所需的发展工具、库和验证方法。

## 平台特定软件安装

每个硬件平台都需要不同的开发方法，这些方法反映了现实世界的嵌入式工程实践。基于 Arduino 的系统侧重于资源效率和实时约束，Raspberry Pi 展示了全面的边缘计算能力，而专门的 AI 硬件突出了专门的加速技术。

选择适合你选择的硬件平台的安装程序。

### Arduino 基于的平台（Nicla Vision，XIAOML 套件）

基于 Arduino 的嵌入式系统提供直接硬件控制，具有最少的抽象层，这使得它们非常适合理解资源限制和优化技术。开发环境强调代码更改和系统行为之间的即时反馈。

**Arduino IDE 安装：**

1.  从 [arduino.cc/software](https://www.arduino.cc/en/software) 下载 Arduino IDE 2.0

1.  按照平台特定的设置向导进行安装

1.  启动 Arduino IDE 并导航到 文件 → 预设

1.  添加板支持 URL：

    +   对于 XIAOML 套件：`https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json`

    +   对于 Nicla Vision：Arduino IDE 板管理器中提供的 URL

**板包安装：**

1.  打开 工具 → 板 → 板管理器

1.  搜索你的平台：

    +   XIAOML 套件：搜索“ESP32”并安装“esp32 by Espressif Systems”

    +   Nicla Vision：搜索“Arduino Mbed OS Nicla Boards”并安装

1.  从 工具 → 板菜单中选择你的板

1.  通过库管理器安装所需的库

**必需的库：**

+   TensorFlow Lite Micro

+   平台特定的摄像头驱动程序

+   传感器接口库（I2C、SPI）

### Grove Vision AI V2 平台

该平台通过专用神经网络单元引入硬件加速 AI，展示了专用硅如何实现通用处理器无法实现的性能提升。可视化编程界面展示了快速原型设计能力，而传统开发环境提供了更广泛的定制选项。

**SenseCraft AI 设置：**

1.  在[sensecraft.seeed.cc](https://sensecraft.seeed.cc)创建账户

1.  通过 USB 连接 Grove Vision AI V2

1.  通过 SenseCraft AI 网络界面访问设备

1.  视觉编程工作流程无需本地软件安装

**Arduino IDE 设置（用于自定义开发）：**

按照上述基于 Arduino 的平台说明进行操作，使用板管理器中的 Seeed Studio 板包 URL。

### Raspberry Pi 平台

Raspberry Pi 环境将嵌入式约束与完整计算能力桥接，使学生能够体验资源优化和高级机器学习框架。这种双重视角说明了计算资源如何影响算法选择和系统架构决策。

**操作系统安装：**

1.  下载[Raspberry Pi Imager](https://www.raspberrypi.com/software/)

1.  将 Raspberry Pi OS（推荐 64 位）闪存到 microSD 卡（最小 32GB）

1.  在镜像过程中配置 SSH 访问和 WiFi 凭证

1.  插入已闪存的 SD 卡并启动 Raspberry Pi

**软件环境设置：**

以下命令建立了一个基于 Python 的 ML 开发环境，具有适当的依赖关系管理：

```py
# Update system packages
sudo apt update && sudo apt upgrade -y

# Install Python development tools
# python3-pip: Python package installer
# python3-venv: Virtual environment creation
# python3-dev: Python development headers
sudo apt install python3-pip \
                 python3-venv \
                 python3-dev -y

# Install ML framework dependencies
# libatlas-base-dev: Linear algebra library (BLAS/LAPACK)
# libhdf5-dev: HDF5 data format library
# libhdf5-serial-dev: HDF5 serial version
sudo apt install libatlas-base-dev \
                 libhdf5-dev \
                 libhdf5-serial-dev -y

# Install computer vision dependencies
# libcamera-dev: Camera interface library
# python3-libcamera: Python bindings for libcamera
# python3-kms++: Kernel mode setting library
sudo apt install libcamera-dev \
                 python3-libcamera \
                 python3-kms++ -y

# Create virtual environment for projects
python3 -m venv ~/ml_projects
source ~/ml_projects/bin/activate

# Install core ML packages
# tensorflow: Main ML framework
# tensorflow-lite: Optimized for edge/mobile devices
# opencv-python: Computer vision library
# numpy: Numerical computing foundation
pip install tensorflow \
            tensorflow-lite \
            opencv-python \
            numpy
```

## 开发工具配置

正确的工具配置确保开发工作站与嵌入式硬件之间可靠的通信。这些设置构成了在整个实验室练习中代码部署、调试和性能监控的基础。

### 串行通信设置

串行通信为嵌入式系统中的调试和数据监控提供了主要接口，提供了理解性能约束和优化机会的系统行为洞察。

**Windows：**

+   安装适当的 USB 到串行驱动程序（CH340、FTDI 或特定平台）

+   配置设备管理器以识别开发板

**macOS/Linux:**

+   大多数 USB 到串行适配器无需额外驱动程序

+   验证设备检测：`ls /dev/tty*`（macOS/Linux）

+   将用户添加到 dialout 组：`sudo usermod -a -G dialout $USER`（Linux）

### IDE 配置

开发环境设置直接影响以代码-测试-部署周期为特征的嵌入式开发的效率。正确的配置减少了调试时间，并提供了关于系统性能的清晰反馈。

**Arduino IDE 设置：**

+   在工具→端口下配置适当的 COM 端口

+   设置正确的板和处理器选择

+   验证上传速度（通常是 115200 波特率）

+   在编译期间启用详细输出以进行调试

**Raspberry Pi 开发：**

+   配置 SSH 密钥以进行远程开发

+   安装带有 Python 和远程 SSH 扩展的 VS Code

+   设置 Jupyter 笔记本以进行交互式开发

## 环境验证

验证程序确认您的开发环境可以成功与硬件通信并执行基本操作。这些测试在进入更复杂的实验室练习之前建立基本功能。

### 硬件检测测试

以下验证程序测试实验室练习所需的核心功能，确保硬件通信和软件库操作正确。

**Arduino 平台：**

```py
void setup() {
  Serial.begin(115200);
  Serial.println("Development environment test");
  Serial.print("Board: ");
  Serial.println(ARDUINO_BOARD);
}

void loop() {
  Serial.println("Environment operational");
  delay(1000);
}
```

**树莓派：**

```py
# Test camera interface
libcamera-hello --timeout 5000

# Test Python ML environment
python3 -c \
  "import tensorflow as tf; print('TensorFlow:', tf.__version__)"
python3 -c \
  "import cv2; print('OpenCV:', cv2.__version__)"
```

**Grove Vision AI V2：**

+   在 SenseCraft AI 网页界面中验证设备检测

+   通过可视化编程界面测试基本模型部署

## 常见设置问题和解决方案

设置挑战很常见，提供了关于嵌入式系统限制和调试技术的宝贵学习机会。以下解决方案解决了在环境配置过程中遇到的最常见问题。

**设备连接问题：**

+   验证 USB 线缆支持数据传输（不仅是供电）

+   如果设备未识别，安装平台特定的 USB 驱动程序

+   如果连接不稳定，尝试不同的 USB 端口或 USB 集线器

**编译错误：**

+   在 IDE 中确认正确的板和处理器选择

+   确认所有必需的库都已安装并兼容

+   检查编译过程是否有足够的磁盘空间

**运行时问题：**

+   确保充足的电源供应（尤其是摄像头操作）

+   验证 SD 卡兼容性和格式化（树莓派）

+   在平台限制内检查 ML 模型的内存分配

**网络连接（支持 WiFi 的平台）：**

+   确认网络凭据和安全协议

+   检查防火墙设置以允许开发工具访问

+   验证网络是否允许设备与开发机器通信

## 故障排除和支持

**常见硬件问题：**

+   **设备未识别：** 确保 USB 线缆支持数据传输，尝试不同的端口

+   **上传失败：** 在 IDE 中检查板选择和端口配置

+   **电源问题：** 验证充足的电源供应，尤其是摄像头操作

+   **内存错误：** 确认模型大小适合平台限制

**软件设置问题：**

+   **库冲突：** 使用设置指南中指定的兼容版本

+   **编译错误：** 验证所有依赖项是否正确安装

+   **网络连接：** 检查防火墙设置和网络权限

**平台特定资源：**

+   **XIAOML 套件：** [Seeed Studio 文档](https://www.seeedstudio.com/The-XIAOML-Kit.html)

    +   [XIAO ESP32S3 系列文档](https://wiki.seeedstudio.com/xiao_esp32s3_getting_started/)

+   **Arduino Nicla Vision：** [Arduino 文档](https://docs.arduino.cc/hardware/nicla-vision)

+   **Grove Vision AI V2：** [SenseCraft AI 平台](https://sensecraft.seeed.cc)

+   **Raspberry Pi：** [官方文档](https://www.raspberrypi.com/documentation/)

**社区支持：**

+   **GitHub Issues：** 通过项目仓库报告错误和请求功能

+   **讨论论坛：** Arduino、Raspberry Pi 和 Seeed Studio 网站上的平台特定社区

+   **Stack Overflow：** 使用适当的平台标签标记问题以获得社区协助

## 准备进行实验室练习

在配置和验证开发环境后，您已经建立了嵌入式机器学习编程所需的基础工具。在环境设置期间发展的技能——理解工具链、管理依赖项和验证系统功能——适用于所有后续的实验室工作。

您配置的环境现在支持整个开发工作流程，从算法实现到硬件部署和性能优化。实验室概述提供了按复杂性和学习目标组织的练习类别，旨在系统地构建这些基础能力。

**推荐起始序列：**

1.  从基本的传感器练习开始，以验证硬件功能

1.  进阶到单模态机器学习应用（图像或音频）

1.  进阶到多模态和优化练习

每个实验室练习都包括详细的实现步骤、预期的性能基准和针对项目要求的具体故障排除指南。您已建立的开发环境为探索嵌入式机器学习应用的完整范围和优化技术提供了基础。
