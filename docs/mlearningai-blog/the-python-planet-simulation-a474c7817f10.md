# Python 星球模拟

> 原文：<https://medium.com/mlearning-ai/the-python-planet-simulation-a474c7817f10?source=collection_archive---------1----------------------->

计算天体物理学

![](img/b2c051ee4eb1103ca066af02e535ec85.png)

让我们从软件包开始

![](img/913a76a6db6df75c09e0d3eb8c02d6c8.png)

# 包装

1.  **pygame** 是一个创建 2d 游戏的包。
2.  **数学**软件包包含了我们模拟行星物理所需的所有数学函数

*注自* [*安德雷·迪亚斯*](https://medium.com/u/f12d59af6cd6?source=post_page-----a474c7817f10--------------------------------)

*为了更精确的计算，你可以考虑使用小数，而不是浮点数*

```
from decimal import Decimal
```

# 安装

![](img/8e53cf1b668ed4b579fb64f09539faab.png)

我们将设置查看区域

我们从用 **init()初始化 **pygame** 开始。**

**宽度**和**高度**可以根据您的意愿，但建议坚持**方形**形状，即**高度**和**宽度**相同

为您的窗口设置标题。这是你的标题，在窗口*的上方，如下图*所示。

![](img/cc68b65f29e63dfe6d2296c62a747947.png)

## 基本视觉元素

![](img/e5a6060ff4146f6082b03a9d33601686.png)

*   与 RGB 值关联的颜色值
*   字体将用于显示轨道值

## 行星

![](img/b261c5310275c9689acbae4023c1d423.png)

在班级星球上

*   **AU** 是天文单位。它被乘以 1000 以转换成米
*   G 表示重力
*   **缩放**是模拟视图的缩放
*   **时间步长**显示 1 天的时间范围

让我们添加功能

## 变量的初始化

![](img/feb3d478944ef9e5cfb81d48a2faf2b8.png)

1.  **x** 是 x 坐标
2.  **y** 是 y 坐标
3.  **半径**是行星的**大小**
4.  **颜色**是行星的**色调**
5.  **质量**是行星质量对应的**值**
6.  这里我们也把太阳算作一颗行星，所以 **self.sun** 为 **False** 默认基本上就是说其他行星都不是太阳。
7.  **x_vel** 和 **y_vel** 是**速度**值

## 模拟

![](img/164b0f6428aaee116994560763ccbc8c.png)

这个函数将帮助我们在视图中放置行星。

```
x = self.x * self.SCALE + WIDTH / 2
y = self.y * self.SCALE + HEIGHT / 2
```

上面的线将帮助我们在视图中居中可视化。

## 轨道

```
pygame.draw.lines(win, self.color, False, updated_points, 2)
```

## 行星

```
pygame.draw.circle(win,self.color,(x,y), self.radius)
```

# 物理学

![](img/5b055e9908166f3f6e5d54fde3343813.png)

借助于三角学，我们可以估计行星在不同时间的位置。综合这些位置，我们将被赋予行星所遵循的轨道。这个公式是模拟中物理学的核心部分。

![](img/e5437f70c3206f17ac8ef574d1b7f21c.png)![](img/23155fa39c06cd7bcf3163895ea7b431.png)

我们需要不断意识到来自其他星球的力量以及太阳的力量。

## 速度

![](img/be26fd000b6cd7b14f4ee0be99dbefb7.png)

由于速度将在多个实例中计算，我们将把信息附加到轨道上来追踪行星的路径。

轨道将是椭圆的，因为角度和距离将会改变，使得力处于负的和正的状态。

## 让我们添加行星

## 太阳

![](img/fd3e12cfba3bf558ab429e314a6299d5.png)

## 汞

![](img/19a2111bd42ef97d33029416d2dfeae7.png)

## 维纳斯

![](img/f5e8a7441268468959dd10446475d799.png)

## **地球**

![](img/6c0e15554be12324ebea8ce645ba8947.png)

## 火星

![](img/777b79484b421ddb54bd2613db2b13a0.png)

## 木星

![](img/4dfa5c81dd67cb7cf87de4c67de51510.png)

## 土星

![](img/eb7eafd5151c8a237e6f885aaf031659.png)

## 天王星

![](img/a1f5da9d6a17ece5c1bddee4aaa17d2a.png)

## 海王星

![](img/ba909bfd826ce2c538b13f1985fba92c.png)

## 普路托

*让尼尔·德格拉斯·泰森疯狂！*

![](img/5d5ef7192053a793f0b90aa4f5f57670.png)

# 最终模拟

![](img/84dae1d07241f70310db8f8a1d707e85.png)

行星信息可以在 NASA 网站上找到

[](https://nssdc.gsfc.nasa.gov/planetary/factsheet/) [## 行星实况报道

### -参见概况介绍注释。美国单位的行星概况表行星概况表-与地球指数相比的数值…

nssdc.gsfc.nasa.gov](https://nssdc.gsfc.nasa.gov/planetary/factsheet/) 

我的 github 上有完整的代码

[](https://github.com/enosjeba/Planet-Simulation) [## GitHub-Enos jeba/Planet-Simulation:使用 Python 的计算天体物理学

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/enosjeba/Planet-Simulation) 

这是基于蒂姆的[视频](https://www.youtube.com/watch?v=WTLPmUHTPqo)

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)