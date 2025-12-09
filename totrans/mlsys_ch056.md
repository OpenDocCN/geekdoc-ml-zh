# DSP 频谱特征

![图片](img/file972.jpg)

*DALL·E 3 Prompt：1950 年代风格的拉丁男性和女性科学家在振动研究室的卡通插图。男性正在使用计算尺检查古老的电路。女性坐在电脑前，有复杂的振动图。木质桌子上铺有传感器板，其中最显眼的是加速度计。一台经典的、圆背的电脑显示了 Arduino IDE 中的代码，用于 LED 引脚分配和运动检测的机器学习算法。串行监视器显示 FFT、分类、小波和 DSP。复古的灯具、工具和带有 FFT 和小波图的图表完善了场景*。

## 概述

与运动（或振动）相关的 TinyML 项目涉及 IMU（通常是**加速度计**和**陀螺仪**）的数据。这些时间序列类型的数据集在输入到机器学习模型训练之前应该进行预处理，这对于嵌入式机器学习来说是一个具有挑战性的领域。然而，Edge Impulse 通过其数字信号处理（DSP）预处理步骤以及更具体的[频谱特征块](https://docs.edgeimpulse.com/docs/edge-impulse-studio/processing-blocks/spectral-features)来帮助克服这种复杂性。

但它内部是如何工作的呢？让我们深入探究。

## 特征提取回顾

从使用惯性传感器（如加速度计）捕获的数据集中提取特征涉及处理和分析原始数据。加速度计测量物体沿一个或多个轴的加速度（通常是三个，标记为 X、Y 和 Z）。这些测量可以用来理解物体的运动的各种方面，如运动模式和振动。以下是该过程的概述：

**数据收集**：首先，我们需要从加速度计收集数据。根据应用的不同，数据可能以不同的采样率收集。确保采样率足够高，以捕捉所研究运动的相关动态（采样率应至少是信号中存在的最大相关频率的两倍）是至关重要的。

**数据预处理**：原始加速度计数据可能包含噪声、错误或不相关信息。预处理步骤，如滤波和归一化，可以帮助清理和标准化数据，使其更适合特征提取。

> 工作室不执行归一化或标准化，因此有时在处理传感器融合时，在将数据上传到工作室之前执行此步骤可能是必要的。这在传感器融合项目中尤为重要，如本教程所示，[使用 Spresense 和 CommonSense 进行传感器数据融合](https://docs.edgeimpulse.com/experts/air-quality-and-environmental-projects/environmental-sensor-fusion-commonsense)。

**分割**：根据数据的性质和应用，将数据分割成更小的段或**窗口**可能是必要的。这有助于关注数据集中的特定事件或活动，使特征提取更易于管理和有意义。**窗口大小**和重叠（**窗口跨度**）的选择取决于应用和感兴趣事件的频率。作为一个经验法则，我们应该尝试捕捉几个“数据周期”。

**特征提取**：一旦数据预处理和分割完成，就可以提取描述运动特性的特征。从加速度计数据中提取的一些典型特征包括：

+   **时域**特征描述了每个段内数据的[统计属性](https://www.mdpi.com/1424-8220/22/5/2012)，如均值、中位数、标准差、偏度、峰度和零交叉率。

+   **频域**特征是通过使用如[快速傅里叶变换（FFT）](https://en.wikipedia.org/wiki/Fast_Fourier_transform)等技术将数据转换到频域获得的。一些典型的频域特征包括功率谱、频谱能量、主导频率（幅度和频率）和频谱熵。

+   **时频**域特征结合了时间和频率域信息，如[短时傅里叶变换（STFT）](https://en.wikipedia.org/wiki/Short-time_Fourier_transform)或[离散小波变换（DWT）](https://en.wikipedia.org/wiki/Discrete_wavelet_transform)。它们可以提供对信号频率内容随时间变化的更详细理解。

在许多情况下，提取的特征数量可能很大，这可能会导致过拟合或增加计算复杂度。特征选择技术，如互信息、基于相关性的方法或主成分分析（PCA），可以帮助识别给定应用中最相关的特征，并降低数据集的维度。工作室可以帮助进行此类与特征相关的计算。

让我们更详细地探讨本系列动手操作中涵盖的典型 TinyML 运动分类项目。

## 一个 TinyML 运动分类项目

![](img/file973.jpg)

在动手项目中，“运动分类和异常检测”，我们模拟了运输中的机械应力，我们的问题是分类四种运动类型：

+   **海上**（船上的托盘）

+   **地面**（卡车或火车上的托盘）

+   **提升**（叉车操作的托盘）

+   **空闲**（仓库中的托盘）

加速度计提供了有关托盘（或容器）的数据。

![](img/file974.png)

下面是 10 秒的样本（原始数据），以 50 Hz 的采样频率捕获：

![](img/file975.png)

> 当使用相同原理的另一个数据集进行此分析时，结果相似，使用不同的采样频率，62.5 Hz 而不是 50 Hz。

## 数据预处理

加速度计（一种“时间序列”数据）捕获的原始数据应使用上一节中描述的典型特征提取方法之一转换为“表格数据”。

我们应该使用滑动窗口对样本数据进行数据分段，以进行特征提取。项目每 10 秒捕获一次加速度计数据，采样率为 62.5 Hz。2 秒窗口捕获 375 个数据点（3 轴 <semantics><mi>×</mi><annotation encoding="application/x-tex">\times</annotation></semantics> 2 秒 <semantics><mi>×</mi><annotation encoding="application/x-tex">\times</annotation></semantics> 62.5 个样本）。窗口每 80 毫秒滑动一次，创建一个更大的数据集，其中每个实例都有 375 个“原始特征”。

![](img/file976.png)

在 Studio 上，之前版本（V1）的**频谱分析块**仅提取了时间域特征 RMS，对于频域，使用 FFT 提取峰值和频率以及信号随时间的功率特性（PSD），从而得到一个固定表格数据集，包含 33 个特征（每个轴 11 个），

![](img/file977.png)

这 33 个特征是神经网络分类器的输入张量。

在 2022 年，Edge Impulse 发布了频谱分析块的第二个版本，我们将在这里探讨。

### Edge Impulse - 频谱分析块 V.2 内部机制

在版本 2 中，每个轴/通道的时间域统计特征如下：

+   均方根

+   偏度

+   偏度

并且每个轴/通道的频域频谱特征如下：

+   频谱功率

+   偏度（在下一个版本中）

+   偏度（在下一个版本中）

在此[链接](https://docs.edgeimpulse.com/docs/edge-impulse-studio/processing-blocks/spectral-features)中，我们可以了解更多关于特征提取的细节。

> 克隆[公共项目](https://studio.edgeimpulse.com/public/198358/latest)。您还可以按照说明，使用我的 Google CoLab 笔记本操作代码：[Edge Impulse 频谱分析块笔记本](https://colab.research.google.com/github/Mjrovai/TinyML4D/blob/main/SciTinyM-2023/Edge_Impulse-Spectral_Analysis_Block/Edge_Impulse_Spectral_Analysis_Block_V3.ipynb)。

开始导入库：

```py
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import math
from scipy.stats import skew, kurtosis
from scipy import signal
from scipy.signal import welch
from scipy.stats import entropy
from sklearn import preprocessing
import pywt

plt.rcParams['figure.figsize'] = (12, 6)
plt.rcParams['lines.linewidth'] = 3
```

从研究的项目中，让我们选择以下加速度计的数据样本：

+   窗口大小为 2 秒：`[2,000]` ms

+   采样频率：`[62.5]` Hz

+   我们将选择`[None]`过滤器（为了简单起见）和一个

+   FFT 长度：`[16]`。

```py
f =  62.5 # Hertz
wind_sec = 2 # seconds
FFT_Length = 16
axis = ['accX', 'accY', 'accZ']
n_sensors = len(axis)
```

![](img/file978.png)

在 Studio 频谱分析标签上选择*原始特征*，我们可以将特定 2 秒窗口的所有 375 个数据点复制到剪贴板。

![](img/file979.png)

将数据点粘贴到新的变量 *data*：

```py
data = [
    -5.6330,  0.2376,  9.8701,
    -5.9442,  0.4830,  9.8701,
    -5.4217, ...
]
No_raw_features = len(data)
N = int(No_raw_features/n_sensors)
```

总原始特征为 375，但我们将逐个轴单独处理，其中 <semantics><mrow><mi>N</mi><mo>=</mo><mn>125</mn></mrow><annotation encoding="application/x-tex">N= 125</annotation></semantics>（每个轴的样本数）。

我们旨在了解 Edge Impulse 如何获取处理后的特征。

![](img/file980.png)

因此，您还应该将处理后的特征传递给一个变量（以便比较 Python 中计算的特征与 Studio 提供的特点）：

```py
features = [
    2.7322, -0.0978, -0.3813,
    2.3980, 3.8924, 24.6841,
    9.6303, ...
]
N_feat = len(features)
N_feat_axis = int(N_feat/n_sensors)
```

处理后的特征总数为 39，这意味着 13 个特征/轴。

仔细观察这 13 个特征，我们将找到 3 个时域特征（均方根、偏度和峰度）：

+   `[均方根] [偏度] [峰度]`

以及 10 个频域（我们稍后会回到这一点）。

+   `[光谱偏度][光谱峰度][光谱功率 1] ... [光谱功率 8]`

**按传感器分割原始数据**

数据包含所有轴的样本；让我们分别拆分并绘制它们：

```py
def plot_data(sensors, axis, title):
    [plt.plot(x, label=y) for x,y in zip(sensors, axis)]
    plt.legend(loc='lower right')
    plt.title(title)
    plt.xlabel('#Sample')
    plt.ylabel('Value')
    plt.box(False)
    plt.grid()
    plt.show()

accX = data[0::3]
accY = data[1::3]
accZ = data[2::3]
sensors = [accX, accY, accZ]
plot_data(sensors, axis, 'Raw Features')
```

![图片](img/file981.png)

**减去平均值**

接下来，我们应该从*数据*中减去平均值。从数据集中减去平均值是统计学和机器学习中常见的数据预处理步骤。从数据中减去平均值的目的是将数据围绕零居中。这很重要，因为它可以揭示如果数据未居中可能隐藏的模式和关系。

这里有一些具体的原因说明为什么减去平均值可能会有所帮助：

+   它简化了分析：通过居中数据，平均值变为零，这使得一些计算更简单且易于解释。

+   它消除了偏差：如果数据有偏差，减去平均值可以消除它，从而允许更准确的分析。

+   它可以揭示模式：将数据居中可以帮助揭示如果数据未居中可能隐藏的模式。例如，如果分析时间序列数据集，居中数据可以帮助您识别时间趋势。

+   它可以提高性能：在某些机器学习算法中，居中数据可以通过减少异常值的影响并使数据更容易比较来提高性能。总的来说，减去平均值是一种简单但强大的技术，可用于改进数据分析和解释。

```py
dtmean = [
    (sum(x) / len(x))
    for x in sensors
]

[
    print('mean_' + x + ' =', round(y, 4))
    for x, y in zip(axis, dtmean)
][0]

accX = [(x - dtmean[0]) for x in accX]
accY = [(x - dtmean[1]) for x in accY]
accZ = [(x - dtmean[2]) for x in accZ]
sensors = [accX, accY, accZ]

plot_data(sensors, axis, 'Raw Features - Subtract the Mean')
```

![图片](img/file982.png)

## 时域统计特征

**均方根计算**

一组值（或连续时间波形）的均方根是该值平方的算术平均值的平方根，或定义连续波形的函数的平方。在物理学中，电流的均方根被定义为“在电阻中消耗相同功率的直流电值。”

在一组<semantics><mi>n</mi><annotation encoding="application/x-tex">n</annotation></semantics>值<semantics><mrow><msub><mi>𝑥</mi><mn>1</mn></msub><mo>,</mo><msub><mi>𝑥</mi><mn>2</mn></msub><mo>,</mo><mi>…</mi><mo>,</mo><msub><mi>𝑥</mi><mi>𝑛</mi></msub></mrow><annotation encoding="application/x-tex">{𝑥_1, 𝑥_2, \ldots, 𝑥_𝑛}</annotation></semantics>的情况下，均方根（RMS）为：<semantics><mrow><msub><mi>x</mi><mrow><mi mathvariant="normal">R</mi><mi mathvariant="normal">M</mi><mi mathvariant="normal">S</mi></mrow></msub><mo>=</mo><msqrt><mrow><mfrac><mn>1</mn><mi>n</mi></mfrac><mrow><mo stretchy="true" form="prefix">(</mo><msubsup><mi>x</mi><mn>1</mn><mn>2</mn></msubsup><mo>+</mo><msubsup><mi>x</mi><mn>2</mn><mn>2</mn></msubsup><mo>+</mo><mi>⋯</mi><mo>+</mo><msubsup><mi>x</mi><mi>n</mi><mn>2</mn></msubsup><mo stretchy="true" form="postfix">)</mo></mrow></mrow></msqrt></mrow> <annotation encoding="application/x-tex">x_{\mathrm{RMS}} = \sqrt{\frac{1}{n} \left( x_1² + x_2² + \cdots + x_n² \right)}</annotation></semantics>

> 注意，原始原始数据与减去平均值后的 RMS 值不同

```py
# Using numpy and standardized data (subtracting mean)
rms = [np.sqrt(np.mean(np.square(x))) for x in sensors]
```

我们可以将这里计算出的 RMS 值与 Edge Impulse 提供的值进行比较：

```py
[print('rms_'+x+'= ', round(y, 4)) for x,y in zip(axis, rms)][0]
print("\nCompare with Edge Impulse result features")
print(features[0:N_feat:N_feat_axis])
```

`rms_accX= 2.7322`

`rms_accY= 0.7833`

`rms_accZ= 0.1383`

与 Edge Impulse 的结果特征相比：

`[2.7322, 0.7833, 0.1383]`

**偏度和峰度计算**

在统计学中，偏度和峰度是衡量分布**形状**的两种方式。

这里，我们可以看到传感器值的分布：

```py
fig, axes = plt.subplots(nrows=1, ncols=3, figsize=(13, 4))
sns.kdeplot(accX, fill=True, ax=axes[0])
sns.kdeplot(accY, fill=True, ax=axes[1])
sns.kdeplot(accZ, fill=True, ax=axes[2])
axes[0].set_title('accX')
axes[1].set_title('accY')
axes[2].set_title('accZ')
plt.suptitle('IMU Sensors distribution', fontsize=16, y=1.02)
plt.show()
```

![图片](img/file983.png)

[**偏度**](https://en.wikipedia.org/wiki/Skewness)是衡量分布不对称性的度量。此值可以是正数或负数。

![图片](img/file984.png)

+   负偏度表示分布的尾巴在左侧，延伸到更负的值。

+   正偏度表示分布的尾巴在右侧，延伸到更正的值。

+   零值表示分布中完全没有偏度，意味着分布是完全对称的。

```py
skew = [skew(x, bias=False) for x in sensors]
[print('skew_'+x+'= ', round(y, 4))
  for x,y in zip(axis, skew)][0]
print("\nCompare with Edge Impulse result features")
features[1:N_feat:N_feat_axis]
```

`skew_accX= -0.099`

`skew_accY= 0.1756`

`skew_accZ= 6.9463`

与 Edge Impulse 结果特征相比：

`[-0.0978, 0.1735, 6.8629]`

[**峰度**](https://en.wikipedia.org/wiki/Kurtosis)是衡量分布相对于正态分布是重尾还是轻尾的度量。

![图片](img/file985.png)

+   正态分布的峰度为零。

+   如果一个给定的分布具有负的峰度，则称为 playkurtic，这意味着它比正态分布产生更少和更极端的异常值。

+   如果一个给定的分布具有正的峰度，则称为 leptokurtic，这意味着它比正态分布产生更多的异常值。

```py
kurt = [kurtosis(x, bias=False) for x in sensors]
[print('kurt_'+x+'= ', round(y, 4))
  for x,y in zip(axis, kurt)][0]
print("\nCompare with Edge Impulse result features")
features[2:N_feat:N_feat_axis]
```

`kurt_accX= -0.3475`

`kurt_accY= 1.2673`

`kurt_accZ= 68.1123`

与 Edge Impulse 结果特征相比：

`[-0.3813, 1.1696, 65.3726]`

## 频谱特征

经过滤波的信号被传递到频谱功率部分，该部分计算**FFT**以生成频谱特征。

由于采样窗口通常大于 FFT 大小，窗口将被分成帧（或“子窗口”），FFT 将在每个帧上计算。

**FFT 长度** - FFT 大小。这决定了 FFT 分箱的数量和可以分离的频率峰值的分辨率。数字低意味着更多信号将在同一个 FFT 分箱中平均，但它也减少了特征数量和模型大小。数字高将更多信号分离到单独的分箱中，生成更大的模型。

+   总的频谱功率特征数量将根据您设置的滤波器和 FFT 参数而变化。没有滤波时，特征数量是 FFT 长度的 1/2。

**频谱功率 - 威尔奇方法**

我们应该使用[威尔奇方法](https://docs.scipy.org/doc/scipy-0.14.0/reference/generated/scipy.signal.welch.html)在频域的 bins 中分割信号，并对每个 bin 计算功率谱。这种方法将信号分割成重叠的段，对每个段应用窗口函数，使用 DFT 计算每个段的周期图，并将它们平均以获得功率谱的更平滑的估计。

```py
# Function used by Edge Impulse instead of scipy.signal.welch().
def welch_max_hold(fx, sampling_freq, nfft, n_overlap):
    n_overlap = int(n_overlap)
    spec_powers = [0 for _ in range(nfft//2+1)]
    ix = 0
    while ix <= len(fx):
        # Slicing truncates if end_idx > len,
        # and rfft will auto-zero pad
        fft_out = np.abs(np.fft.rfft(fx[ix:ix+nfft], nfft))
        spec_powers = np.maximum(spec_powers, fft_out**2/nfft)
        ix = ix + (nfft-n_overlap)
    return np.fft.rfftfreq(nfft, 1/sampling_freq), spec_powers
```

将上述函数应用于 3 个信号：

```py
fax,Pax = welch_max_hold(accX, fs, FFT_Length, 0)
fay,Pay = welch_max_hold(accY, fs, FFT_Length, 0)
faz,Paz = welch_max_hold(accZ, fs, FFT_Length, 0)
specs = [Pax, Pay, Paz ]
```

我们可以绘制功率谱 P(f)：

```py
plt.plot(fax,Pax, label='accX')
plt.plot(fay,Pay, label='accY')
plt.plot(faz,Paz, label='accZ')
plt.legend(loc='upper right')
plt.xlabel('Frequency (Hz)')
#plt.ylabel('PSD [V**2/Hz]')
plt.ylabel('Power')
plt.title('Power spectrum P(f) using Welch's method')
plt.grid()
plt.box(False)
plt.show()
```

![](img/file986.png)

除了功率谱，我们还可以将特征的偏度和峰度包含在频域中（应在新版本中可用）：

```py
spec_skew = [skew(x, bias=False) for x in specs]
spec_kurtosis = [kurtosis(x, bias=False) for x in specs]
```

现在我们列出每个轴的所有频谱特征，并与 EI 进行比较：

```py
print("EI Processed Spectral features (accX): ")
print(features[3:N_feat_axis][0:])
print("\nCalculated features:")
print (round(spec_skew[0],4))
print (round(spec_kurtosis[0],4))
[print(round(x, 4)) for x in Pax[1:]][0]
```

EI 处理后的频谱特征（accX）：

2.398, 3.8924, 24.6841, 9.6303, 8.4867, 7.7793, 2.9963, 5.6242, 3.4198, 4.2735

计算特征：

2.9069 8.5569 24.6844 9.6304 8.4865 7.7794 2.9964 5.6242 3.4198 4.2736

```py
print("EI Processed Spectral features (accY): ")
print(features[16:26][0:]) # 13: 3+N_feat_axis;
                           # 26 = 2x N_feat_axis
print("\nCalculated features:")
print (round(spec_skew[1],4))
print (round(spec_kurtosis[1],4))
[print(round(x, 4)) for x in Pay[1:]][0]
```

EI 处理后的频谱特征（accY）：

0.9426, -0.8039, 5.429, 0.999, 1.0315, 0.9459, 1.8117, 0.9088, 1.3302, 3.112

计算特征：

1.1426 -0.3886 5.4289 0.999 1.0315 0.9458 1.8116 0.9088 1.3301 3.1121

```py
print("EI Processed Spectral features (accZ): ")
print(features[29:][0:]) #29: 3+(2*N_feat_axis);
print("\nCalculated features:")
print (round(spec_skew[2],4))
print (round(spec_kurtosis[2],4))
[print(round(x, 4)) for x in Paz[1:]][0]
```

EI 处理后的频谱特征（accZ）：

0.3117, -1.3812, 0.0606, 0.057, 0.0567, 0.0976, 0.194, 0.2574, 0.2083, 0.166

计算特征：

0.3781 -1.4874 0.0606 0.057 0.0567 0.0976 0.194 0.2574 0.2083 0.166

## 时频域

### 小波

[小波](https://en.wikipedia.org/wiki/Wavelet)是一种强大的技术，用于分析具有瞬态特征或突然变化的信号，如尖峰或边缘，这些特征难以用传统的基于傅里叶的方法解释。

小波变换通过将信号分解成不同的频率分量并单独分析它们来实现。这种转换是通过将信号与**小波函数**卷积来实现的，这是一个以特定时间和频率为中心的小波形。这个过程有效地将信号分解成不同的频率带，每个频率带都可以单独分析。

小波变换的一个关键优点是它们允许进行时间-频率分析，这意味着它们可以揭示信号随时间变化的频率内容。这使得它们特别适用于分析非平稳信号，这些信号随时间变化。

小波有许多实际应用，包括信号和图像压缩、去噪、特征提取和图像处理。

让我们在同一项目中光谱特征块中选择小波：

+   类型：小波

+   小波分解级别：1

+   小波：bior1.3

![](img/file987.png)

**小波函数**

```py
wavelet_name='bior1.3'
num_layer = 1

wavelet = pywt.Wavelet(wavelet_name)
[phi_d,psi_d,phi_r,psi_r,x] = wavelet.wavefun(level=5)
plt.plot(x, psi_d, color='red')
plt.title('Wavelet Function')
plt.ylabel('Value')
plt.xlabel('Time')
plt.grid()
plt.box(False)
plt.show()
```

![](img/file988.png)

如我们之前所做的那样，让我们复制并粘贴处理过的特征：

![](img/file989.png)

```py
features = [
    3.6251, 0.0615, 0.0615,
    -7.3517, -2.7641, 2.8462,
    5.0924, ...
]
N_feat = len(features)
N_feat_axis = int(N_feat/n_sensors)
```

Edge Impulse 对每个选定的波分解级别计算 [离散小波变换 (DWT)](https://pywavelets.readthedocs.io/en/latest/ref/dwt-discrete-wavelet-transform.html)。之后，将提取特征。

在 **小波** 的情况下，提取的特征是 *基本统计值*、*交叉值* 和 *熵*。总共每个层有 14 个特征，如下所示：

+   [11] 统计特征：**n5, n25, n75, n95, mean, median**，标准差 **(std**)，方差 **(var**)，均方根 **(rms**)，峰度 **(kurtosis**) 和偏度 **(skew)**。

+   [2] 交叉特征：零交叉率 **(zcross**) 和平均交叉率 **(mcross**) 分别是信号通过基线 <semantics><mrow><mo stretchy="true" form="prefix">(</mo><mi>y</mi><mo>=</mo><mn>0</mn><mo stretchy="true" form="postfix">)</mo></mrow><annotation encoding="application/x-tex">(y = 0)</annotation></semantics> 和平均水平（y = u）的单位时间次数

+   [1] 复杂度特征：**熵**是信号复杂性的特征度量

所有的上述 14 个值都是针对每个层（包括 L0，原始信号）计算的

+   特征的总数取决于你如何设置滤波器和层数。例如，使用 [None] 滤波器和级别[1]，每个轴的特征数将是 <semantics><mrow><mn>14</mn><mo>×</mo><mn>2</mn></mrow><annotation encoding="application/x-tex">14\times 2</annotation></semantics>（L0 和 L1）= 28。对于三个轴，我们将有总共 84 个特征。

### 小波分析

小波分析通过一组滤波器将信号（**accX, accY**, **和 accZ**）分解成不同的频率成分，这些滤波器将这些成分分离成低频（信号缓慢变化的部分，包含长期模式），例如 **accX_l1, accY_l1, accZ_l1** 和高频（信号快速变化的部分，包含短期模式）成分，例如 **accX_d1, accY_d1, accZ_d1**，允许提取特征以进行进一步分析或分类。

只会使用低频分量（近似系数，或 cA）。在这个例子中，我们假设只有一个级别（单级离散小波变换），函数将返回一个元组。在多级分解中，“多级 1D 离散小波变换”，结果将是一个列表（详情请见：[离散小波变换 (DWT)](https://pywavelets.readthedocs.io/en/latest/ref/dwt-discrete-wavelet-transform.html)）

```py
(accX_l1, accX_d1) = pywt.dwt(accX, wavelet_name)
(accY_l1, accY_d1) = pywt.dwt(accY, wavelet_name)
(accZ_l1, accZ_d1) = pywt.dwt(accZ, wavelet_name)
sensors_l1 = [accX_l1, accY_l1, accZ_l1]

# Plot power spectrum versus frequency
plt.plot(accX_l1, label='accX')
plt.plot(accY_l1, label='accY')
plt.plot(accZ_l1, label='accZ')
plt.legend(loc='lower right')
plt.xlabel('Time')
plt.ylabel('Value')
plt.title('Wavelet Approximation')
plt.grid()
plt.box(False)
plt.show()
```

![](img/file990.png)

### 特征提取

让我们从基本的统计特征开始。请注意，我们对原始信号和 DWT 的结果 cA 都应用了该函数：

```py
def calculate_statistics(signal):
    n5 = np.percentile(signal, 5)
    n25 = np.percentile(signal, 25)
    n75 = np.percentile(signal, 75)
    n95 = np.percentile(signal, 95)
    median = np.percentile(signal, 50)
    mean = np.mean(signal)
    std = np.std(signal)
    var = np.var(signal)
    rms = np.sqrt(np.mean(np.square(signal)))
    return [n5, n25, n75, n95, median, mean, std, var, rms]

stat_feat_l0 = [calculate_statistics(x) for x in sensors]
stat_feat_l1 = [calculate_statistics(x) for x in sensors_l1]
```

Skelness 和 Kurtosis：

```py
skew_l0 = [skew(x, bias=False) for x in sensors]
skew_l1 = [skew(x, bias=False) for x in sensors_l1]
kurtosis_l0 = [kurtosis(x, bias=False) for x in sensors]
kurtosis_l1 = [kurtosis(x, bias=False) for x in sensors_l1]
```

**零穿越（zcross**）是小波系数穿越零轴的次数。它可以用来测量信号的频率内容，因为高频信号往往比低频信号有更多的零穿越。

另一方面，**均值穿越（mcross**）是小波系数穿越信号均值次数。它可以用来测量振幅，因为高振幅信号往往比低振幅信号有更多的均值穿越。

```py
def getZeroCrossingRate(arr):
    my_array = np.array(arr)
    zcross = float(
        "{:.2f}".format(
          (((my_array[:-1] * my_array[1:]) < 0).sum()) / len(arr)
        )
    )
    return zcross

def getMeanCrossingRate(arr):
    mcross = getZeroCrossingRate(np.array(arr) - np.mean(arr))
    return mcross

def calculate_crossings(list):
    zcross=[]
    mcross=[]
    for i in range(len(list)):
        zcross_i = getZeroCrossingRate(list[i])
        zcross.append(zcross_i)
        mcross_i = getMeanCrossingRate(list[i])
        mcross.append(mcross_i)
    return zcross, mcross

cross_l0 = calculate_crossings(sensors)
cross_l1 = calculate_crossings(sensors_l1)
```

在小波分析中，**熵**指的是小波系数分布的无序度或随机度。在这里，我们使用了香农熵，它衡量信号的确定性或随机性。它是通过将信号不同可能结果的概率乘以它们的 2 为底的对数，然后取负和来计算的。在小波分析中，香农熵可以用来衡量信号的复杂性，值越高表示复杂性越大。

```py
def calculate_entropy(signal, base=None):
    value, counts = np.unique(signal, return_counts=True)
    return entropy(counts, base=base)

entropy_l0 = [calculate_entropy(x) for x in sensors]
entropy_l1 = [calculate_entropy(x) for x in sensors_l1]
```

现在让我们列出所有的小波特征，并按层创建一个列表。

```py
L1_features_names = [
    "L1-n5", "L1-n25", "L1-n75", "L1-n95", "L1-median",
    "L1-mean", "L1-std", "L1-var", "L1-rms", "L1-skew",
    "L1-Kurtosis", "L1-zcross", "L1-mcross", "L1-entropy"
]

L0_features_names = [
    "L0-n5", "L0-n25", "L0-n75", "L0-n95", "L0-median",
    "L0-mean", "L0-std", "L0-var", "L0-rms", "L0-skew",
    "L0-Kurtosis", "L0-zcross", "L0-mcross", "L0-entropy"
]

all_feat_l0 = []
for i in range(len(axis)):
    feat_l0 = (
        stat_feat_l0[i]
        + [skew_l0[i]]
        + [kurtosis_l0[i]]
        + [cross_l0[0][i]]
        + [cross_l0[1][i]]
        + [entropy_l0[i]]
    )
    [print(axis[i] + ' +x+= ', round(y, 4))
       for x, y in zip(LO_features_names, feat_l0)][0]
    all_feat_l0.append(feat_l0)

all_feat_l0 = [
    item
    for sublist in all_feat_l0
    for item in sublist
]
print(f"\nAll L0 Features = {len(all_feat_l0)}")

all_feat_l1 = []
for i in range(len(axis)):
    feat_l1 = (
        stat_feat_l1[i]
        + [skew_l1[i]]
        + [kurtosis_l1[i]]
        + [cross_l1[0][i]]
        + [cross_l1[1][i]]
        + [entropy_l1[i]]
    )
    [print(axis[i]+' '+x+'= ', round(y, 4))
       for x,y in zip(L1_features_names, feat_l1)][0]
    all_feat_l1.append(feat_l1)

all_feat_l1 = [
    item
    for sublist in all_feat_l1
    for item in sublist
]
print(f"\nAll L1 Features = {len(all_feat_l1)}")
```

![](img/file991.png)

## 概述

Edge Impulse Studio 是一个强大的在线平台，可以为我们处理预处理任务。然而，从我们的工程角度来看，我们想要了解底层发生了什么。这些知识将帮助我们找到最佳选项和超参数来调整我们的项目。

Daniel Situnayake 在他的 [博客](https://situnayake.com/) 中写道：“原始传感器数据高度多维且噪声大。数字信号处理算法帮助我们从噪声中提取信号。DSP 是嵌入式工程的重要组成部分，许多边缘处理器都内置了 DSP 加速。作为一名机器学习工程师，学习基本的 DSP 可以让你在模型中处理高频时间序列数据时拥有超能力。” 我建议您完整地阅读 Dan 的优秀文章：[nn to cpp：将深度学习模型移植到边缘需要了解的内容](https://situnayake.com/2023/03/21/nn-to-cpp.html)。
