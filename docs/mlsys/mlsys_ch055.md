# KWS 特征工程

![](img/file964.jpg)

*DALL·E 3 Prompt: 1950s style cartoon scene set in an audio research room. Two scientists, one holding a magnifying glass and the other taking notes, examine large charts pinned to the wall. These charts depict FFT graphs and time curves related to audio data analysis. The room has a retro ambiance, with wooden tables, vintage lamps, and classic audio analysis tools.*

## 概述

在这个动手教程中，重点是特征工程在优化应用于音频分类任务（如语音识别）的机器学习模型性能中的关键作用。重要的是要意识到任何机器学习模型的性能在很大程度上依赖于所使用的特征质量，我们将处理特征提取的“内部机制”，主要关注梅尔频率倒谱系数（MFCCs），这是音频信号处理领域的基石。

机器学习模型，尤其是传统算法，不理解音频波形。它们理解以某种有意义方式排列的数字，即特征。这些特征封装了音频信号的特征，使得模型更容易区分不同的声音。

> 本教程将处理为音频分类生成特定特征。这对于将机器学习应用于各种音频数据特别有趣，无论是用于语音识别、音乐分类、基于翅膀拍打声的昆虫分类，还是其他声音分析任务。

## KWS

最常见的 TinyML 应用是关键词识别（KWS），它是更广泛的语音识别领域的一个子集。虽然通用语音识别将所有 spoken words 转录成文本，但关键词识别专注于检测连续音频流中的特定“关键词”或“唤醒词”。系统被训练来识别这些关键词作为预定义的短语或单词，如 *yes* 或 *no*。简而言之，KWS 是一种具有自己一套挑战和要求的专门化的语音识别形式。

这里是一个典型的使用梅尔频率倒谱系数（MFCC）特征转换器的 KWS 流程：

![](img/file965.jpg)

### KWS 的应用

+   **语音助手**: 在亚马逊的 Alexa 或 Google Home 等设备中，KWS 用于检测唤醒词（“Alexa”或“Hey Google”）以激活设备。

+   **语音激活控制**: 在汽车或工业环境中，KWS 可以用来启动特定的命令，如“启动引擎”或“关灯”。

+   **安全系统**: 语音激活的安全系统可能使用 KWS 根据语音密码短语验证用户。

+   **电信服务**: 客户服务线路可能使用关键词识别（KWS）来根据语音关键词路由电话。

### 与通用语音识别的不同之处

+   **计算效率**: KWS 通常设计为比完整的语音识别计算量更小，因为它只需要识别一小组短语。

+   **实时处理**：KWS 通常在实时模式下运行，并针对低延迟的关键词检测进行了优化。

+   **资源限制**：KWS 模型通常被设计成轻量级，以便在计算资源有限的设备上运行，例如微控制器或手机。

+   **专注任务**：虽然通用语音识别模型被训练以处理广泛的词汇和口音，但 KWS 模型经过微调以准确识别特定的关键词，通常在嘈杂环境中也能准确识别。

## 音频信号概述

理解音频信号的基本特性对于有效的特征提取以及最终在音频分类任务中成功应用机器学习算法至关重要。音频信号是复杂的波形，它捕捉了随时间变化的空气压力波动。这些信号可以通过几个基本属性来描述：采样率、频率和幅度。

+   **频率和幅度**：[频率](https://en.wikipedia.org/wiki/Audio_frequency)是指波形在单位时间内经历的振荡次数，也以 Hz 为单位测量。在音频信号的上下文中，不同的频率对应不同的音调。[幅度](https://en.wikipedia.org/wiki/Amplitude)另一方面，测量振荡的幅度，与声音的响度相关。频率和幅度都是捕捉音频信号音调和节奏特性的基本特征。

+   **采样率**：[采样率](https://en.wikipedia.org/wiki/Sampling_(signal_processing))，通常以赫兹（Hz）表示，定义了在数字化模拟信号时每秒采集的样本数。较高的采样率可以更准确地表示信号，但也需要更多的计算资源来处理。典型的采样率包括 CD 音质的 44.1 kHz，以及用于语音识别任务的 16 kHz 或 8 kHz。理解选择适当采样率的权衡对于平衡准确性和计算效率至关重要。在 TinyML 项目中，我们通常使用 16 kHz。尽管音乐音调可以达到高达 20 kHz 的频率，但人声的最高频率为 8 kHz。传统的电话系统使用 8 kHz 的采样频率。

> 为了准确表示信号，采样率必须至少是信号中最高频率的两倍。

+   **时域与频域**：音频信号可以在时域和频域中进行分析。在时域中，信号以波形的形式表示，振幅随时间变化。这种表示有助于观察时间特征，如起始和持续时间，但信号的音调特征并不明显。相反，频域表示提供了信号组成频率及其相应振幅的视图，通常通过傅里叶变换获得。这对于需要理解信号频谱内容的任务非常有价值，例如识别音符或语音音素（我们的案例）。

下面的图像显示了`YES`和`NO`这两个词在时域（原始音频）和频域中的典型表示：

![图片](img/file966.jpg)

### 为什么不是原始音频？

直接使用原始音频数据用于机器学习任务可能看起来很有吸引力，但这种方法存在一些挑战，使其不太适合构建稳健和高效的模型。

例如，在 TinyML 设备上使用原始音频数据进行关键词检测（KWS）时，由于其高维性（使用 16 kHz 采样率）、捕获时间特征的计算复杂性、对噪声的敏感性以及缺乏语义上有意义的特征，使得 MFCCs 等特征提取技术在资源受限的应用中更为实用。

这里是关于使用原始音频的一些关键问题的附加细节：

+   **高维性**：音频信号，尤其是以高采样率采样的信号，会产生大量数据。例如，以 16 kHz 采样率采样的 1 秒音频剪辑将包含 16,000 个独立的数据点。高维数据增加了计算复杂性，导致训练时间更长，计算成本更高，这使得在资源受限的环境中不切实际。此外，音频信号的广泛动态范围需要每个样本大量的比特数，而传达的信息却很少。

+   **时间依赖性**：原始音频信号具有时间结构，简单的机器学习模型可能难以捕捉。虽然循环神经网络（如 LSTMs）可以建模这种依赖关系，但它们在小型设备上计算密集且难以训练。

+   **噪声和可变性**：原始音频信号通常包含背景噪声和其他非必要元素，这些都会影响模型性能。此外，同一声音可以根据麦克风距离、声音源方向和环境声学特性等因素具有不同的特征，这增加了数据的复杂性。

+   **缺乏语义意义**：原始音频本身不包含用于分类任务的语义上有意义的特征。像音高、节奏和频谱特征这样的特征，对于语音识别可能至关重要，但它们不能直接从原始波形数据中获取。

+   **信号冗余**：音频信号通常包含冗余信息，信号中的一些部分对当前任务的价值很小或没有价值。这种冗余可能会使学习效率低下，并可能导致过拟合。

由于这些原因，特征提取技术，如梅尔频率倒谱系数（MFCCs）、梅尔频率能量（MFEs）和简单的频谱图，通常被用来将原始音频数据转换为更易于管理和信息丰富的格式。这些特征捕捉了音频信号的基本特征，同时降低了维度和噪声，促进了更有效的机器学习。

## MFCCs 概述

### 什么是 MFCCs？

[梅尔频率倒谱系数（MFCCs）](https://en.wikipedia.org/wiki/Mel-frequency_cepstrum)是一组从音频信号的频谱内容中派生出来的特征。它们基于人类的听觉感知，通常用于捕捉音频信号的语音特征。MFCCs 通过一个多步骤的过程计算得出，包括预加重、分帧、加窗、应用快速傅里叶变换（FFT）将信号转换为频域，最后应用离散余弦变换（DCT）。结果是原始音频信号频谱特征的紧凑表示。

下面的图像显示了`YES`和`NO`单词的 MFCC 表示：

![](img/file967.jpg)

> 这[视频](https://youtu.be/SJo7vPgRlBQ?si=KSgzmDg8DtSVqzXp)解释了梅尔频率倒谱系数（MFCC）及其计算方法。

### 为什么 MFCCs 很重要？

MFCCs 之所以至关重要，有多个原因，尤其是在关键词检测（KWS）和 TinyML 的背景下：

+   **维度降低**：MFCCs 在捕捉音频信号的基本频谱特征的同时，显著降低了数据的维度，使其非常适合资源受限的 TinyML 应用。

+   **鲁棒性**：MFCCs 对噪声和音高、振幅的变化不太敏感，为音频分类任务提供了更稳定、更鲁棒的特征集。

+   **人类听觉系统建模**：MFCCs 中的梅尔尺度近似了人耳对不同频率的响应，使得它们在需要类似人类感知的语音识别中变得实用。

+   **计算效率**：计算 MFCCs 的过程计算效率高，非常适合在计算资源有限的硬件上实时应用。

总结来说，MFCCs 在信息丰富性和计算效率之间提供了平衡，因此在音频分类任务中很受欢迎，尤其是在 TinyML 等受限环境中。

### 计算 MFCCs

梅尔频率倒谱系数 (MFCCs) 的计算涉及几个关键步骤。让我们逐一了解这些步骤，这对于在 TinyML 设备上的关键词检测 (KWS) 任务尤为重要。

+   **预加重**: 第一步是预加重，这是为了强调音频信号的高频成分并平衡频谱。这是通过应用一个放大连续样本之间差异的滤波器来实现的。预加重的公式是：<semantics><mrow><mi>y</mi><mrow><mo stretchy="true" form="prefix">(</mo><mi>t</mi><mo stretchy="true" form="postfix">)</mo></mrow><mo>=</mo><mi>x</mi><mrow><mo stretchy="true" form="prefix">(</mo><mi>t</mi><mo stretchy="true" form="postfix">)</mo></mrow><mo>−</mo><mi>α</mi><mi>x</mi><mrow><mo stretchy="true" form="prefix">(</mo><mi>t</mi><mo>−</mo><mn>1</mn><mo stretchy="true" form="postfix">)</mo></mrow></mrow><annotation encoding="application/x-tex">y(t)=x(t)-\alpha x(t-1)</annotation></semantics>，其中 <semantics><mi>α</mi><annotation encoding="application/x-tex">\alpha</annotation></semantics> 是预加重因子，通常约为 0.97。

+   **分帧**: 音频信号被分成短帧（*帧长度*），通常为 20 到 40 毫秒。这是基于信号中的频率在短时间内是平稳的假设。分帧有助于在如此小的时隙内分析信号。*帧步长*（或步进）将移动一个帧及其相邻帧。这些步骤可以是顺序的或重叠的。

+   **窗口化**: 每个帧随后被窗口化以最小化帧边界处的间断性。常用的窗口函数是汉明窗口。窗口化通过最小化边缘效应为傅里叶变换准备信号。下面的图像显示了三个帧（10、20 和 30）以及窗口化后的时间样本（注意帧长度和帧步长为 20 ms）：

![](img/file968.jpg)

+   **快速傅里叶变换 (FFT)** 快速傅里叶变换 (FFT) 被应用于每个窗口化的帧，将其从时域转换为频域。FFT 给出的是一个复数值表示，包括幅度和相位信息。然而，对于梅尔频率倒谱系数 (MFCCs)，仅使用幅度来计算功率谱。功率谱是幅度谱的平方，并测量每个频率成分的能量。

> 信号的功率谱 <semantics><mrow><mi>P</mi><mrow><mo stretchy="true" form="prefix">(</mo><mi>f</mi><mo stretchy="true" form="postfix">)</mo></mrow></mrow><annotation encoding="application/x-tex">P(f)</annotation></semantics> 定义为 <semantics><mrow><mi>P</mi><mrow><mo stretchy="true" form="prefix">(</mo><mi>f</mi><mo stretchy="true" form="postfix">)</mo></mrow><mo>=</mo><msup><mrow><mo stretchy="true" form="prefix">|</mo><mi>X</mi><mrow><mo stretchy="true" form="prefix">(</mo><mi>f</mi><mo stretchy="true" form="postfix">)</mo></mrow><mo stretchy="true" form="postfix">|</mo></mrow><mn>2</mn></msup></mrow><annotation encoding="application/x-tex">P(f)=|X(f)|²</annotation></semantics>，其中 <semantics><mrow><mi>X</mi><mrow><mo stretchy="true" form="prefix">(</mo><mi>f</mi><mo stretchy="true" form="postfix">)</mo></mrow></mrow><annotation encoding="application/x-tex">X(f)</annotation></semantics> 是信号 <semantics><mrow><mi>x</mi><mrow><mo stretchy="true" form="prefix">(</mo><mi>t</mi><mo stretchy="true" form="postfix">)</mo></mrow></mrow><annotation encoding="application/x-tex">x(t)</annotation></semantics> 的傅里叶变换。通过对傅里叶变换的幅度进行平方，我们强调了 *更强的* 频率相对于 *较弱的* 频率，从而捕捉到音频信号的更多相关频谱特性。这在音频分类、语音识别和关键词检测（KWS）等应用中非常重要，这些应用的重点是识别不同音频类别或语音音素的不同频率模式。

![](img/file969.jpg)

+   **梅尔滤波器组**：然后将频域映射到 [梅尔尺度](https://en.wikipedia.org/wiki/Mel_scale)，该尺度近似人类耳朵对不同频率的响应。想法是在低频提取更多特征（更多滤波器组），而在高频提取较少。因此，它在人类耳朵区分的声音上表现良好。通常，20 到 40 个三角滤波器提取梅尔频率能量。然后对这些能量进行对数变换，将乘法因子转换为加法因子，使其更适合进一步处理。

![](img/file970.jpg)

+   **离散余弦变换 (DCT)**：最后一步是将对数梅尔能量应用离散余弦变换 (DCT)。DCT 有助于去相关能量，有效地压缩数据并仅保留最具判别性的特征。通常，保留前 12-13 个 DCT 系数，形成最终的 MFCC 特征向量。

![](img/file971.jpg)

## 使用 Python 进行实践

让我们在实际音频样本上工作时应用我们讨论的内容。在 Google CoLab 上打开笔记本，并在您的音频样本上提取 MLCC 特征：[[在 Colab 中打开]](https://colab.research.google.com/github/Mjrovai/Arduino_Nicla_Vision/blob/main/KWS/Audio_Data_Analysis.ipynb)

## 摘要

*我们应该使用哪种特征提取技术？*

梅尔频率倒谱系数（MFCCs）、梅尔频率能量（MFEs）或频谱图是表示音频数据的技术，它们在不同的环境中通常很有帮助。

通常，MFCCs 更专注于捕捉功率谱的包络，这使得它们对细粒度频谱细节不太敏感，但对噪声更鲁棒。这对于语音相关任务通常是希望的。另一方面，频谱图或 MFEs 保留了更多详细的频率信息，这对于需要基于细粒度频谱内容进行区分的任务可能是有利的。

### MFCCs 在以下方面特别强大

1.  **语音识别**：MFCCs 在识别语音信号中的语音内容方面非常出色。

1.  **说话人识别**：它们可以根据声音特征区分不同的说话人。

1.  **情感识别**：MFCCs 可以捕捉到表明情绪状态的细微变化。

1.  **关键词检测**：特别是在 TinyML 中，低计算复杂度和小特征尺寸至关重要。

### 频谱图或 MFEs 通常更适合

1.  **音乐分析**：频谱图可以捕捉音乐中的和声和音色结构，这对于流派分类、乐器识别或音乐转录等任务至关重要。

1.  **环境声音分类**：在识别非语音环境声音（例如，雨、风、交通）时，完整的频谱图可以提供更多区分性特征。

1.  **鸟鸣识别**：鸟鸣的复杂细节通常更适合使用频谱图来捕捉。

1.  **生物声信号处理**：在像海豚或蝙蝠叫声分析这样的应用中，频谱图中的细粒度频率信息可能是至关重要的。

1.  **音频质量保证**：频谱图通常在专业音频分析中用于识别不需要的噪音、点击或其他伪迹。

## 资源

+   [音频数据分析 Colab 笔记本](https://colab.research.google.com/github/Mjrovai/Arduino_Nicla_Vision/blob/main/KWS/Audio_Data_Analysis.ipynb)
