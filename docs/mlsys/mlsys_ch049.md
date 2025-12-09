# 设置

![图片](img/file777.jpg)

*DALL·E 提示 - 一个受 20 世纪 50 年代启发的电子实验室环境，采用卡通风格。实验室应配备复古设备、大型示波器、老式收音机和大型、方形的计算机。Raspberry Pi 5 板显著展示，准确展示了其实际尺寸，类似于信用卡，在工作台上。Pi 板周围环绕着经典的实验室工具，如烙铁、电阻和电线。整体场景应充满活力，具有卡通特有的夸张色彩和趣味细节。不应包含任何标志或文本。*

本章将指导您设置 Raspberry Pi Zero 2 W（*Raspi-Zero*）和 Raspberry Pi 5（*Raspi-5*）型号。我们将涵盖硬件设置、操作系统安装、初始配置和测试。

> *Raspi-5*的一般说明也适用于较老的 Raspberry Pi 版本，如 Raspi-3 和 Raspi-4。

## 概述

Raspberry Pi 是一款功能强大且多功能的单板计算机，已成为各个学科工程师的必备工具。由[Raspberry Pi 基金会](https://www.raspberrypi.org/)开发，这些紧凑型设备提供了独特的性价比、计算能力和广泛的 GPIO（通用输入/输出）功能，使其成为原型设计、嵌入式系统开发和高级工程项目的理想选择。

### 关键特性

1.  **计算能力**：尽管体积小巧，Raspberry Pi 提供了显著的计算能力，最新型号配备了多核 ARM 处理器，最高可达 8 GB RAM。

1.  **GPIO 接口**：40 引脚 GPIO 接口允许直接与传感器、执行器和其他电子组件交互，促进硬件和软件集成项目。

1.  **广泛的连接性**：内置 Wi-Fi、蓝牙、以太网和多个 USB 端口，可实现多样化的通信和网络项目。

1.  **低级硬件访问**：Raspberry Pi 提供了访问 I2C、SPI 和 UART 等接口的能力，允许对外部设备进行详细控制和通信。

1.  **实时能力**：通过适当的配置，Raspberry Pi 可用于软实时应用，使其适用于控制系统和信号处理任务。

1.  **功率效率**：低功耗使得电池供电和节能设计成为可能，尤其是在像 Pi Zero 这样的型号中。

### Raspberry Pi 型号（本书涵盖）

1.  **Raspberry Pi Zero 2 W** (*Raspi-Zero*):

    +   适用范围：紧凑型嵌入式系统

    +   关键规格：1 GHz 单核 CPU（ARM Cortex-A53），512 MB RAM，功耗低

1.  **Raspberry Pi 5** (*Raspi-5*):

    +   适用范围：更复杂的应用，如边缘计算、计算机视觉和边缘 AI 应用，包括 LLMs。

    +   关键规格：2.4 GHz 四核 CPU（ARM Cortex A-76），最高 8 GB RAM，支持 PCIe 接口的扩展

### 工程应用

1.  **嵌入式系统设计**：开发和原型化嵌入式系统，用于实际应用。

1.  **物联网和网络设备**：创建互联设备，并探索 MQTT、CoAP 和 HTTP/HTTPS 等协议。

1.  **控制系统**：实现反馈控制回路、PID 控制器，并与执行器接口。

1.  **计算机视觉和 AI**：利用 OpenCV 和 TensorFlow Lite 等库进行边缘图像处理和机器学习。

1.  **数据采集和分析**：收集传感器数据，进行实时分析，并创建数据记录系统。

1.  **机器人技术**：构建机器人控制器，实现运动规划算法，并与电机驱动器接口。

1.  **信号处理**：执行实时信号分析、滤波和 DSP 应用。

1.  **网络安全**：设置 VPN、防火墙，并探索网络渗透测试。

本教程将指导您设置最常用的 Raspberry Pi 型号，使您能够快速开始机器学习项目。我们将涵盖硬件设置、操作系统安装和初始配置，重点关注为 Pi 准备机器学习应用。

## 硬件概述

### Raspberry Pi Zero 2W

![图片](img/file778.jpg)

+   **处理器**：1 GHz 四核 64 位 Arm Cortex-A53 CPU

+   **RAM**：512 MB SDRAM

+   **无线**：2.4 GHz 802.11 b/g/n 无线局域网，蓝牙 4.2，BLE

+   **端口**：迷你 HDMI，微型 USB OTG，CSI-2 摄像头连接器

+   **电源**：通过微型 USB 端口供电 5 V

### Raspberry Pi 5

![图片](img/file779.jpg)

+   **处理器**:

    +   Pi 5：四核 64 位 Arm Cortex-A76 CPU @ 2.4 GHz

    +   Pi 4：四核 Cortex-A72（ARM v8）64 位 SoC @ 1.5 GHz

+   **RAM**：2 GB、4 GB 或 8 GB 选项（AI 任务推荐 8 GB）

+   **无线**：双频段 802.11ac 无线，蓝牙 5.0

+   **端口**：2 个微型 HDMI 端口，2 个 USB 3.0 端口，2 个 USB 2.0 端口，CSI 摄像头端口，DSI 显示端口

+   **电源**：通过 USB-C 连接器供电 5 V DC（3A）

> 在实验室中，我们将使用不同的名称来称呼 Raspberry：`Raspi`、`Raspi-5`、`Raspi-Zero`等。通常，`Raspi`用于指令或注释适用于每个型号的情况。

## 安装操作系统

### 操作系统（OS）

操作系统（OS）是一种基本软件，它管理计算机的硬件和软件资源，为计算机程序提供标准服务。它是运行在计算机上的核心软件，作为硬件和应用软件之间的中介。操作系统管理计算机的内存、进程、设备驱动程序、文件和安全协议。

1.  **主要功能**：

    +   进程管理：为不同的程序分配 CPU 时间

    +   内存管理：根据需要分配和释放内存

    +   文件系统管理：组织和跟踪文件和目录

    +   设备管理：与连接的硬件设备通信

    +   用户界面：为用户提供与计算机交互的方式

1.  **组件**：

    +   内核：管理硬件资源的操作系统核心

    +   Shell：与操作系统交互的用户界面

    +   文件系统：组织和管理工作存储

    +   设备驱动程序：允许操作系统与硬件通信的软件

Raspberry Pi 运行的是专为嵌入式系统设计的 Linux 专用版本。这个操作系统通常是 Debian 的一个变种，称为 Raspberry Pi OS（以前称为 Raspbian），针对 Pi 的 ARM 架构和有限资源进行了优化。

> Raspberry Pi OS 的最新版本基于[Debian Bookworm](https://www.raspberrypi.com/news/bookworm-the-new-version-of-raspberry-pi-os/)。

**关键特性**：

1.  轻量级：针对 Pi 的硬件高效运行。

1.  多功能：支持广泛的应⽤程序和编程语言。

1.  开源：允许自定义和社区驱动的改进。

1.  GPIO 支持：通过 Pi 的引脚与传感器和其他硬件进行交互。

1.  定期更新：持续改进性能和安全。

Raspberry Pi 上的嵌入式 Linux 提供了一个功能齐全的操作系统，体积紧凑，非常适合从简单的物联网设备到更复杂的边缘机器学习应用的各种项目。它与标准 Linux 工具和库的兼容性使其成为开发和实验的强大平台。

### 安装

要使用 Raspberry Pi，我们需要一个操作系统。默认情况下，Raspberry Pi 会在任何插入插槽的 SD 卡上检查操作系统，因此我们应该使用[Raspberry Pi Imager](https://www.raspberrypi.com/software/)安装操作系统。

*Raspberry Pi Imager*是用于在*macOS*、*Windows*和*Linux*上下载和写入镜像的工具。它包括许多适用于 Raspberry Pi 的流行操作系统镜像。我们还将使用 Imager 来预配置凭证和远程访问设置。

按照以下步骤在您的 Raspi 上安装操作系统。

1.  [下载](https://www.raspberrypi.com/software/)并在您的计算机上安装 Raspberry Pi Imager。

1.  将一张 microSD 卡插入您的计算机（推荐使用 32GB SD 卡）。

1.  打开 Raspberry Pi Imager 并选择您的 Raspberry Pi 型号。

1.  选择适当的操作系统：

    +   **对于 Raspi-Zero**：例如，您可以选择：`Raspberry Pi OS Lite (64-bit)`。

![img](img/file780.png)

> 由于其减少的 SDRAM（512 MB），Raspi-Zero 推荐的操作系统是 32 位版本。但是，为了运行一些机器学习模型，例如 Ultralytics 的 YOLOv8，我们应该使用 64 位版本。尽管 Raspi-Zero 可以运行*桌面*，但我们将选择 LITE 版本（无桌面）以减少常规操作所需的 RAM。

+   对于**Raspi-5**：我们可以选择包含桌面的完整 64 位版本：`Raspberry Pi OS (64-bit)`

![](img/file781.png)

1.  选择您的 microSD 卡作为存储设备。

1.  点击`下一步`然后点击`齿轮`图标以访问高级选项。

1.  设置*主机名*，Raspi *用户名和密码*，配置*WiFi*并*启用 SSH*（非常重要！）

![](img/file782.jpg)

1.  将镜像写入 microSD 卡。

> 在以下示例中，我们将根据所使用的设备使用不同的主机名：raspi、raspi-5、raspi-Zero 等。如果您使用的是这些中的一个，请将其替换。

### 初始配置

1.  将 microSD 卡插入您的 Raspberry Pi。

1.  连接电源以启动 Raspberry Pi。

1.  请等待初始引导过程完成（可能需要几分钟）。

> 您可以在[这里](https://www.jwdittrich.people.ysu.edu/raspberrypi/UsefulRaspberryPiCommands.pdf)或[这里](https://www.codecademy.com/learn/learn-raspberry-pi/modules/raspberry-pi-command-line-module/cheatsheet)找到与 Raspi 一起使用的最常见 Linux 命令。

## 远程访问

### SSH 访问

与 Raspi-Zero 交互的最简单方法是 SSH（“无头”）。您可以使用终端（MAC/Linux）、[PuTTy (](https://www.putty.org/)Windows）或任何其他。

1.  查找您的 Raspberry Pi 的 IP 地址（例如，检查您的路由器）。

1.  在您的计算机上打开一个终端并通过 SSH 连接：

    ```py
    ssh username@[raspberry_pi_ip_address]
    ```

    或者，如果您没有 IP 地址，您可以尝试以下操作：`bash ssh username@hostname.local` 例如，`ssh mjrovai@rpi-5.local`，`ssh mjrovai@raspi.local` 等。

    ![](img/file783.png)

    img

    当您看到提示：

    ```py
    mjrovai@rpi-5:~ $
    ```

    这意味着您正在远程与您的 Raspi 交互。定期更新/升级系统是一个好习惯。为此，您应该运行：

    ```py
    sudo apt-get update
    sudo apt upgrade
    ```

    您应该确认 Raspi 的 IP 地址。在终端中，您可以使用：

    ```py
    hostname -I
    ```

![](img/file784.png)

### 通过终端关闭 Raspi：

当您想要关闭 Raspberry Pi 时，除了拔掉电源线之外，还有更好的方法。这是因为 Raspi 可能仍在向 SD 卡写入数据，在这种情况下，仅仅关闭电源可能会导致数据丢失，甚至更糟的是，SD 卡损坏。

为了安全关机，请使用命令行：

```py
sudo shutdown -h now
```

> 为了避免可能的数据丢失和 SD 卡损坏，在断电之前，您应该在关闭后等待几秒钟，直到 Raspberry Pi 的 LED 停止闪烁并变暗。一旦 LED 熄灭，就可以安全地关闭电源。

### 在 Raspi 和计算机之间传输文件

使用 U 盘、直接在终端（使用 scp）或通过网络上的 FTP 程序在 Raspi 和我们的主计算机之间传输文件。

#### 使用安全复制协议（`scp`）：

##### 将文件复制到您的 Raspberry Pi

让我们在计算机上创建一个文本文件，例如，`test.txt`。

![](img/file785.png)

> 您可以使用任何文本编辑器。在同一个终端中，一个选项是`nano`。

要将名为`test.txt`的文件从您的个人计算机复制到 Raspberry Pi 上的用户主目录，请从包含`test.txt`的目录运行以下命令，将`<username>`占位符替换为您用于登录 Raspberry Pi 的用户名，将`<pi_ip_address>`占位符替换为您的 Raspberry Pi 的 IP 地址：

```py
$ scp test.txt <username>@<pi_ip_address>:~/
```

> 注意，`~/`表示我们将文件移动到 Raspi 的根目录。您可以选择 Raspi 中的任何文件夹。但在运行`scp`之前，您应该创建文件夹，因为`scp`不会自动创建文件夹。

例如，让我们将文件`test.txt`传输到我的 Raspi-zero 的根目录，其 IP 地址为`192.168.4.210`：

```py
scp test.txt mjrovai@192.168.4.210:~/
```

![](img/file786.png)

我使用不同的配置文件来区分终端。上述操作发生在**您的计算机**上。现在，让我们转到我们的 Raspi（使用 SSH）并检查文件是否在那里：

![](img/file787.png)

##### 从您的 Raspberry Pi 复制文件

要将名为`test.txt`的文件从 Raspberry Pi 上的用户主目录复制到另一台计算机的当前目录，请在宿主计算机上运行以下命令**：

```py
$ scp <username>@<pi_ip_address>:myfile.txt .
```

例如：

在 Raspi 上，让我们创建一个带有另一个名称的文件副本：

```py
cp test.txt test_2.txt
```

并且在宿主计算机（以我的 Mac 为例）

```py
scp mjrovai@192.168.4.210:test_2.txt .
```

![](img/file788.png)

#### 使用 FTP 传输文件

使用 FTP 传输文件，例如[FileZilla FTP 客户端](https://filezilla-project.org/download.php?type=client)，也是可能的。按照说明，为您的桌面操作系统安装程序，并使用 Raspi IP 地址作为`主机`。例如：

```py
sftp://192.168.4.210
```

并输入您的 Raspi `用户名和密码`。按下`快速连接`将打开两个窗口，一个用于您的宿主计算机桌面（右），另一个用于 Raspi（左）。

![](img/file789.png)

## 增加 SWAP 内存

使用跨平台的交互式进程查看器`htop`，您可以轻松监控 Raspi 上运行的资源，例如进程列表、运行的 CPU 和实时使用的内存。要启动`htop`，请在终端输入以下命令：

```py
htop
```

![](img/file790.png)

关于内存，在 Raspberry Pi 系列设备中，Raspi-Zero 的 SRAM（500 MB）最少，相比之下，Raspi 4 或 5 的选择为 2 GB 到 8 GB。对于任何 Raspi，都可以通过“SWAP”来增加系统可用的内存。SWAP 内存，也称为交换空间，是计算机操作系统在物理 RAM 完全使用时，将数据从 RAM（随机存取存储器）临时存储到 SD 卡的技术。这允许操作系统（OS）在 RAM 满时继续运行，从而可以防止系统崩溃或减慢。

SWAP 内存对 RAM 有限的设备，如 Raspi-Zero，有益。增加 SWAP 可以帮助运行更复杂的应用或进程，但与频繁磁盘访问的潜在性能影响保持平衡是至关重要的。

默认情况下，Rapi-Zero 的 SWAP（Swp）内存只有 100 MB，这对于运行一些更复杂和需求更高的机器学习应用（例如，YOLO）来说非常小。让我们将其增加到 2 MB：

首先，关闭 swap 文件：

```py
sudo dphys-swapfile swapoff
```

接下来，您应该打开并更改文件`/etc/dphys-swapfile`。为此，我们将使用 nano：

```py
sudo nano /etc/dphys-swapfile
```

搜索**CONF_SWAPSIZE**变量（默认为 200）并将其更新为**2000**：

```py
CONF_SWAPSIZE=2000
```

并保存文件。

接下来，再次打开 swap 文件并重新启动 Raspi-zero：

```py
sudo dphys-swapfile setup
sudo dphys-swapfile swapon
sudo reboot
```

当您的设备重新启动（您应该再次使用 SSH 进入），您会意识到顶部显示的最大交换内存值现在接近 2 GB（在我的情况下，是 1.95 GB）。

> 要保持*htop*运行，您应该打开另一个终端窗口以持续与您的 Raspi 交互。

## 安装摄像头

Raspi 是计算机视觉应用的优秀设备；需要摄像头。我们可以使用 USB OTG 适配器（Raspi-Zero 和 Raspi-5）或连接到 Raspi CSI（摄像头串行接口）端口的摄像头模块在 micro-USB 端口上安装标准 USB 摄像头。

> USB 网络摄像头通常比连接到 CSI 端口的摄像头模块质量差。它们也不能使用终端中的`raspistill`和`raspivid`命令或 Python 中的`picamera`录制包进行控制。尽管如此，您可能出于某些原因想要将 USB 摄像头连接到 Raspberry Pi，例如，因为它更容易使用单个 Raspberry Pi 设置多个摄像头、长电缆，或者简单地因为您手头上有这样的摄像头。

### 安装 USB 网络摄像头

1.  关闭 Raspi：

```py
sudo shutdown -h no
```

1.  将 USB 网络摄像头（USB 摄像头模块 30 fps，<semantics><mrow><mn>1280</mn><mo>×</mo><mn>720</mn></mrow><annotation encoding="application/x-tex">1280\times 720</annotation></semantics>）连接到您的 Raspi（在这个例子中，我使用的是 Raspi-Zero，但说明适用于所有 Raspi）。

![图片](img/file791.jpg)

1.  再次开机并运行 SSH

1.  要检查您的 USB 摄像头是否被识别，运行：

```py
lsusb
```

您应该在输出中看到您的摄像头被列出。

![图片](img/file792.png)

1.  要使用 USB 摄像头拍摄测试照片，请使用：

```py
fswebcam test_image.jpg
```

这将在您的当前目录下保存一个名为“test_image.jpg”的图片。

![图片](img/file793.png)

1.  由于我们使用 SSH 连接到我们的 Rapsi，我们必须将图像传输到我们的主计算机，以便我们可以查看它。我们可以使用 FileZilla 或 SCP 来完成此操作：

在您的宿主机上打开一个终端并运行：

```py
scp mjrovai@raspi-zero.local:~/test_image.jpg .
```

> 将“mjrovai”替换为您的用户名，将“raspi-zero”替换为 Pi 的主机名。

![图片](img/file794.png)

1.  如果图像质量不令人满意，您可以调整各种设置；例如，定义一个适合 YOLO 的分辨率（<semantics><mrow><mo stretchy="true" form="prefix">(</mo><mn>640</mn><mi>x</mi><mn>640</mn><mo stretchy="true" form="postfix">)</mo></mrow><annotation encoding="application/x-tex">(640x640)</annotation></semantics>）：

```py
fswebcam -r 640x640 --no-banner test_image_yolo.jpg
```

这将捕获一个更高分辨率的图像，而不显示默认横幅。

![图片](img/file795.png)

普通的 USB 网络摄像头也可以使用：

![图片](img/file796.jpg)

使用`lsusb`进行验证

![图片](img/file797.png)

#### 视频流传输

对于流视频（这需要更多的资源），我们可以安装并使用 mjpg-streamer：

首先，安装 Git：

```py
sudo apt install git
```

现在，我们应该安装 mjpg-streamer 所需的依赖项，克隆仓库，并继续安装：

```py
sudo apt install cmake libjpeg62-turbo-dev
git clone https://github.com/jacksonliam/mjpg-streamer.git
cd mjpg-streamer/mjpg-streamer-experimental
make
sudo make install
```

然后使用以下命令启动流：

```py
mjpg_streamer -i "input_uvc.so" -o "output_http.so -w ./www"
```

我们可以通过打开网页浏览器并导航到以下位置来访问流：

`http://<你的树莓派 IP 地址>:8080`。在我的情况下：http://192.168.4.210:8080

我们应该看到一个带有查看流选项的网页。点击“Stream”链接或尝试访问：

```py
http://<raspberry_pi_ip_address>:8080/?action=stream
```

![图片](img/file798.png)

### 在 CSI 端口安装相机模块

现在有几种树莓派相机模块。原始的 500 万像素型号于 2013 年[发布](https://www.raspberrypi.com/news/camera-board-available-for-sale/)，随后是 2016 年发布的[800 万像素相机模块 2](https://www.raspberrypi.com/products/camera-module-v2/)。最新的相机型号是 2023 年发布的[1200 万像素相机模块 3](https://www.raspberrypi.com/documentation/accessories/camera.html)。

原始的 500 万像素相机（**Arducam OV5647**）已从树莓派处停售，但可以从几个替代供应商处找到。以下是在 Raspi-Zero 上此类相机的示例。

![图片](img/file799.jpg)

这是另一个 v2 相机模块的示例，它具有**索尼 IMX219** 800 万像素传感器：

![图片](img/file800.png)

任何相机模块都可以在树莓派上使用，但为了这样做，必须更新`configuration.txt`文件：

```py
sudo nano /boot/firmware/config.txt
```

在文件底部，例如，要使用 500 万像素的 Arducam OV5647 相机，添加以下行：

```py
dtoverlay=ov5647,cam0
```

或者对于具有 800 万像素索尼 IMX219 相机的 v2 模块：

```py
dtoverlay=imx219,cam0
```

保存文件（CTRL+O [ENTER] CRTL+X）并重启树莓派：

```py
Sudo reboot
```

启动后，你可以查看相机是否被列出：

```py
libcamera-hello --list-cameras
```

![图片](img/file801.jpg)

![图片](img/file802.png)

> [libcamera](https://www.raspberrypi.com/documentation/computers/camera_software.html#libcamera) 是一个开源软件库，它支持从基于 Arm 处理器的 Linux 操作系统直接支持相机系统。它最小化了在 Broadcom GPU 上运行的专有代码。

让我们捕获一个分辨率为<semantics><mrow><mn>640</mn><mo>×</mo><mn>480</mn></mrow><annotation encoding="application/x-tex">640\times 480</annotation></semantics>的 jpeg 图像进行测试，并将其保存到名为`test_cli_camera.jpg`的文件中

```py
rpicam-jpeg --output test_cli_camera.jpg --width 640 --height 480
```

如果我们想查看保存的文件，应该使用`ls -f`，它会以长格式列出当前目录的所有内容。像之前一样，我们可以使用 scp 查看图像：

![图片](img/file803.png)

## 远程运行 Raspi 桌面

尽管我们主要通过 SSH 使用终端命令与 Raspberry Pi 交互，但如果已安装完整的 Raspberry Pi OS（例如，`Raspberry Pi OS (64-bit)`），我们还可以远程访问整个图形桌面环境。这对于受益于图形界面的任务特别有用。要启用此功能，必须在 Raspberry Pi 上设置 VNC（虚拟网络计算）服务器。以下是操作步骤：

1.  启用 VNC 服务器：

    +   通过 SSH 连接到您的 Raspberry Pi。

    +   通过输入以下内容运行 Raspberry Pi 配置工具：

        ```py
        sudo raspi-config
        ```

    +   使用箭头键导航到 `接口选项`。

![](img/file804.png)

+   选择 `VNC` 并选择 `Yes` 以启用 VNC 服务器。

![](img/file805.png)

+   当提示时，退出配置工具，并保存更改。

![](img/file806.png)

1.  在您的计算机上安装 VNC 观看器：

    +   在您的主计算机上下载并安装一个 VNC 观看器应用程序。流行的选项包括 RealVNC Viewer、TightVNC 或 RealVNC 的 VNC Viewer。我们将安装 RealVNC 的 [VNC Viewer](https://www.realvnc.com/en/connect/download/viewer)。

1.  安装完成后，您应确认 Raspi IP 地址。例如，在终端中，您可以使用：

    ```py
    hostname -I
    ```

![](img/file807.png)

1.  连接到您的 Raspberry Pi：

    +   打开您的 VNC 观看器应用程序。

![](img/file808.png)

+   输入您的 Raspberry Pi 的 IP 地址和主机名。

+   当提示时，输入您的 Raspberry Pi 的用户名和密码。

![](img/file809.png)

1.  Raspberry Pi 5 桌面应出现在您的计算机监视器上。

![](img/file810.png)

1.  调整显示设置（如有必要）：

    +   连接后，调整显示分辨率以获得最佳观看效果。这可以通过 Raspberry Pi 的桌面设置或修改 config.txt 文件来完成。

    +   让我们使用桌面设置来完成此操作。到达菜单（左上角的小型 Raspberry 图标）并选择适合您监视器的最佳屏幕定义：

![](img/file811.png)

## 更新和安装软件

1.  更新您的系统：

    ```py
    sudo apt update && sudo apt upgrade -y
    ```

1.  安装必需的软件：

    ```py
    sudo apt install python3-pip -y
    ```

1.  为 Python 项目启用 pip：

    ```py
    sudo rm /usr/lib/python3.11/EXTERNALLY-MANAGED
    ```

## 模型特定注意事项

### Raspberry Pi Zero (Raspi-Zero)

+   处理能力有限，最适合轻量级项目

+   使用无头设置（SSH）来节省资源会更好。

+   考虑为内存密集型任务增加交换空间。

+   它可用于图像分类和目标检测实验室，但不能用于 LLM (SLM)。

### Raspberry Pi 4 或 5 (Raspi-4 或 Raspi-5)

+   适用于更复杂的项目，包括人工智能和机器学习。

+   它可以流畅地运行整个桌面环境。

+   Raspi-4 可用于图像分类和目标检测实验室，但与 LLMs (SLM) 的兼容性不佳。

+   对于 Raspi-5，在 LLMs (SLMs) 实验室进行密集型任务时，考虑使用主动冷却器进行温度管理。

记住根据你使用的具体树莓派型号调整你的项目需求。Raspi-Zero 非常适合低功耗、空间受限的项目，而 Raspi-4 或 5 型号则更适合计算密集型任务。
