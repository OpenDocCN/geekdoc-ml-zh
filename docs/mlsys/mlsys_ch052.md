# 小型语言模型（SLM）

![图片](img/file908.jpg)

*DALL·E 提示 - 一幅 1950 年代风格的卡通插图，展示一个 Raspberry Pi 在边缘运行一个小型语言模型。Raspberry Pi 以复古未来主义的方式设计，边缘圆润，铬色装饰，连接到有趣的卡通式传感器和设备。气泡在周围飘浮，代表语言处理，背景是一个奇妙的互联设备景观，有电线和小型小玩意，所有这些都以复古卡通风格绘制。色彩调色板使用柔和的粉彩颜色和典型的 1950 年代卡通的粗轮廓，给场景增添了乐趣和怀旧感。*

## 概述

在人工智能快速增长的领域，边缘计算为将传统上保留给强大、集中式服务器的功能去中心化提供了机会。本实验探讨了将传统大型语言模型（LLMs）的小型版本集成到 Raspberry Pi 5 中的实际应用，将这个边缘设备转变为一个能够实时、现场处理数据的 AI 中心。

随着大型语言模型在规模和复杂性上的增长，小型语言模型（SLMs）为边缘设备提供了一个引人注目的替代方案，在性能和资源效率之间取得了平衡。通过直接在 Raspberry Pi 上运行这些模型，我们可以创建响应式、保护隐私的应用程序，即使在有限或没有互联网连接的环境中也能运行。

本实验将指导您在 Raspberry Pi 上设置、优化和利用 SLMs。我们将探讨[Ollama](https://ollama.com/)的安装和使用。这个开源框架允许我们在自己的机器上（我们的台式机或边缘设备，如 Raspberry Pi 或 NVidia Jetsons）本地运行 LLMs。Ollama 被设计成高效、可扩展且易于使用，使其成为部署 AI 模型（如 Microsoft Phi、Google Gemma、Meta Llama 和 LLaVa（多模态））的好选择。我们将使用 Python 生态系统将这些模型中的某些集成到项目中，探索它们在现实世界场景中的潜力（或者至少指向这个方向）。

![图片](img/file909.jpg)

## 设置

我们可以使用之前实验中的任何 Raspi 型号，但在这里，选择必须是 Raspberry Pi 5（Raspi-5）。这是一个坚固的平台，实质性地升级了上一版本的 4，配备了 Broadcom BCM2712，一个 2.4 GHz 四核 64 位 Arm Cortex-A76 CPU，具有加密扩展和增强的缓存功能。它拥有 VideoCore VII GPU，双 4Kp60 HDMI®输出带 HDR，以及 4Kp60 HEVC 解码器。内存选项包括 4 GB 和 8 GB 的高速 LPDDR4X SDRAM，我们选择 8 GB 来运行 SLMs。它还通过 microSD 卡槽和 PCIe 2.0 接口提供可扩展的存储，用于快速外围设备，如 M.2 SSDs（固态硬盘）。

> 对于真正的 SSL 应用，SSD 比 SD 卡是更好的选择。

顺便提一下，正如[阿拉斯代尔·艾伦](https://www.hackster.io/aallan)所讨论的，直接在 Raspberry Pi 5 CPU 上进行推理——没有 GPU 加速——现在的性能已经与 Coral TPU 相当。

![图片](img/file910.png)

更多信息，请参阅完整文章：[在 Raspberry Pi 5 上基准测试 TensorFlow 和 TensorFlow Lite](https://www.hackster.io/news/benchmarking-tensorflow-and-tensorflow-lite-on-raspberry-pi-5-b9156d58a6a2?mc_cid=0cab3d08f4&mc_eid=e96256ccba)。

### Raspberry Pi 活性散热器

我们建议为这个实验室安装一个活性散热器，这是为 Raspberry Pi 5（Raspi-5）设计的专用夹具式冷却解决方案。它结合了铝制散热器和温度控制的吹风风扇，以保持 Raspi-5 在重负载下（如运行 SLMs）舒适运行。

![图片](img/file911.png)

活性散热器预先涂有热垫以进行热传递，并使用弹簧加载的推钉直接安装到 Raspberry Pi 5 板上。Raspberry Pi 固件会主动管理它：在 60°C 时，风扇将开启；在 67.5°C 时，风扇速度将提高；最后，在 75°C 时，风扇将全速运转。当温度降至这些限制以下时，风扇将自动减速。

![图片](img/file912.png)

> 为了防止过热，所有 Raspberry Pi 板在温度达到 80°C 时开始降低处理器速度，当温度达到最大温度 85°C 时进一步降低速度（更多详情[这里](https://www.raspberrypi.com/news/heating-and-cooling-raspberry-pi-5/))。

## 生成式 AI（GenAI）

生成式 AI 是一种能够在各种媒介如**文本、图像、音频和视频**中创建新、原创内容的人工智能系统。这些系统从现有数据中学习模式，并利用这些知识生成之前不存在的新颖输出。**大型语言模型（LLMs）**、**小型语言模型（SLMs）**和**多模态模型**都可以被认为是用于生成任务的生成式 AI 的类型。

生成式 AI 为 AI 驱动的内容创作提供了概念框架，其中 LLMs 作为强大的通用文本生成器。SLMs 将这项技术应用于边缘计算，而多模态模型扩展了生成式 AI 在不同数据类型上的能力。它们共同代表了一组生成式 AI 技术，每种技术都有其优势和用途，共同推动 AI 驱动的创作和理解。

### 大型语言模型（LLMs）

大型语言模型（LLMs）是高级人工智能系统，能够理解、处理和生成类似人类的文本。这些模型的特点是它们在训练数据量和参数数量上的巨大规模。LLMs 的关键方面包括：

1.  **规模**：LLMs 通常包含数十亿个参数。例如，GPT-3 有 1750 亿个参数，而一些较新的模型超过了万亿个参数。

1.  **训练数据**：它们在大量的文本数据上训练，通常包括书籍、网站和其他多样化的来源，总计达到数百甚至数千吉字节（GB）或甚至太字节（TB）的文本。

1.  **架构**：大多数 LLMs 使用基于[transformer 架构](https://en.wikipedia.org/wiki/Transformer_(deep_learning_architecture))，这使得它们能够通过同时关注输入的不同部分来处理和生成文本。

1.  **功能**：LLMs 可以在不进行特定微调的情况下执行广泛的语言任务，包括：

    +   文本生成

    +   翻译

    +   摘要

    +   问答

    +   代码生成

    +   逻辑推理

1.  **少样本学习**：它们通常可以通过最少的示例或指令来理解和执行新任务。

1.  **资源密集型**：由于它们的规模，LLMs 通常需要大量的计算资源来运行，通常需要强大的 GPU 或 TPU。

1.  **持续发展**：LLMs 领域正在迅速发展，新的模型和技术不断涌现。

1.  **伦理考量**：LLMs 的使用引发了关于偏见、错误信息和训练如此大型模型的环境影响的重大问题。

1.  **应用领域**：大型语言模型（LLMs）被应用于各个领域，包括内容创作、客户服务、研究辅助和软件开发。

1.  **局限性**：尽管功能强大，LLMs 仍然可能产生错误或带有偏见的信息，并且缺乏真正的理解或推理能力。

我们必须注意，我们使用的是超越文本的大型模型，我们称它们为**多模态模型**。这些模型能够同时整合和处理来自多种类型输入的信息。它们被设计成能够理解和生成跨各种数据形式的内容，例如文本、图像、音频和视频。

### 封闭模型与开放模型：

**封闭模型**，也称为专有模型，是指其内部工作原理、代码和训练数据不公开的 AI 模型。例如：GPT-4（由 OpenAI 开发）、Claude（由 Anthropic 开发）、Gemini（由 Google 开发）。

**开放模型**，也称为开源模型，是指其底层代码、架构以及通常的训练数据都是公开可用和可访问的 AI 模型。例如：Gemma（由 Google 开发）、LLaMA（由 Meta 开发）和 Phi（由 Microsoft 开发）。

开放模型特别适用于在边缘设备（如 Raspberry Pi）上运行模型，因为它们更容易适应、优化和部署在资源受限的环境中。然而，验证它们的许可证至关重要。开放模型附带各种开源许可证，可能会影响它们在商业应用中的使用，而封闭模型则具有明确、尽管有些限制的服务条款。

![](img/file913.png)

修改自 arXiv

### 小型语言模型（SLMs）

在边缘计算设备如树莓派（Raspberry Pi）的背景下，全规模的大型语言模型（LLMs）通常太大且资源密集，无法直接运行。这种限制推动了更小、更高效的模型的发展，例如小型语言模型（SLMs）。

SLMs 是 LLMs 的紧凑版本，旨在在资源受限的设备上（如智能手机、物联网设备和树莓派等单板计算机）高效运行。这些模型在尺寸和计算需求方面比其大型对应物小得多，同时仍然保留了令人印象深刻的语言理解和生成能力。

SLMs 的关键特性包括：

1.  **参数数量减少**：通常从几亿到几十亿参数不等，相比之下，大型模型有两位数的十亿参数。

1.  **更低的内存占用**：最多只需要几 GB 的内存，而不是数十或数百 GB。

1.  **更快的推理时间**：在边缘设备上可以以毫秒到秒的速度生成响应。

1.  **能源效率**：消耗更少的电力，使其适合电池供电的设备。

1.  **隐私保护**：允许在设备上处理数据，而无需发送到云端服务器。

1.  **离线功能**：无需互联网连接即可运行。

SLMs 通过知识蒸馏、模型剪枝和量化等各种技术实现其紧凑的尺寸。虽然它们可能无法与更大模型的广泛能力相匹配，但 SLMs 在特定任务和领域方面表现出色，使它们非常适合边缘设备上的针对性应用。

> 我们通常将 SLMs 视为参数量少于 50 亿且量化为 4 位的语言模型。

SLMs 的例子包括 Meta Llama、Microsoft PHI 和 Google Gemma 等模型的压缩版本。这些模型使得在边缘设备上直接执行广泛的自然语言处理任务成为可能，从文本分类和情感分析到问答和有限的文本生成。

关于 SLMs 的更多信息，论文《LLM Pruning and Distillation in Practice: The Minitron Approach》（[`arxiv.org/pdf/2408.11796`](https://arxiv.org/pdf/2408.11796)）提供了一种将剪枝和蒸馏应用于从 LLMs 获得 SLMs 的方法。而《SMALL LANGUAGE MODELS: SURVEY, MEASUREMENTS, AND INSIGHTS》（[`arxiv.org/pdf/2409.15790`](https://arxiv.org/pdf/2409.15790)）则对小型语言模型（SLMs）进行了全面的调查和分析，这些模型具有 1 亿到 50 亿参数，旨在为资源受限的设备设计。

## Ollama

![图片](img/file914.png)

ollama logo

[Ollama](https://ollama.com/) 是一个开源框架，允许我们在自己的机器上本地运行语言模型（LMs），无论是大型还是小型。以下是关于 Ollama 的一些关键点：

1.  **本地模型执行**：Ollama 使得在个人电脑或边缘设备（如 Raspi-5）上运行 LMs 成为可能，从而消除了对基于云的 API 调用的需求。

1.  **易于使用**：它提供了一个简单的命令行界面，用于下载、运行和管理不同的语言模型。

1.  **模型多样性**：Ollama 支持各种 LLM，包括 Phi、Gemma、Llama、Mistral 和其他开源模型。

1.  **定制**：用户可以创建和分享定制模型，以满足特定需求或领域。

1.  **轻量级**：设计为高效且能在消费级硬件上运行。

1.  **API 集成**：提供 API，允许与其他应用程序和服务集成。

1.  **隐私优先**：通过本地运行模型，它解决了与向外部服务器发送数据相关的隐私问题。

1.  **跨平台**：适用于 macOS、Windows 和 Linux 系统（我们的案例，在这里）。

1.  **活跃开发**：定期更新新功能和模型支持。

1.  **社区驱动**：受益于社区贡献和模型共享。

要了解更多关于 Ollama 是什么以及它的工作原理，你应该看看来自[Matt Williams](https://www.youtube.com/@technovangelist)，Ollama 创始人之一的这个简短视频：

[`www.youtube.com/embed/90ozfdsQOKo`](https://www.youtube.com/embed/90ozfdsQOKo)

> Matt 有一个关于 Ollama 的完全免费的课程，我们推荐：[`youtu.be/9KEUFe4KQAI?si=D_-q3CMbHiT-twuy`](https://youtu.be/9KEUFe4KQAI?si=D_-q3CMbHiT-twuy)

### 安装 Ollama

让我们设置并激活一个用于与 Ollama 一起工作的虚拟环境：

```py
python3 -m venv ~/ollama
source ~/ollama/bin/activate
```

然后运行以下命令来安装 Ollama：

```py
curl -fsSL https://ollama.com/install.sh | sh
```

因此，API 将在后台运行在`127.0.0.1:11434`。从现在起，我们可以通过终端运行 Ollama。首先，让我们验证 Ollama 的版本，这也会告诉我们它是否正确安装：

```py
ollama -v
```

![](img/file915.png)

在[Ollama 库页面](https://ollama.com/library)，我们可以找到 Ollama 支持的模型。例如，通过按`最受欢迎`筛选，我们可以看到 Meta Llama、Google Gemma、Microsoft Phi、LLaVa 等。

### Meta Llama 3.2 1B/3B

![](img/file916.png)

让我们安装并运行我们的第一个小型语言模型[Llama 3.2](https://ai.meta.com/blog/llama-3-2-connect-2024-vision-edge-mobile-devices/) 1B（和 3B）。Meta Llama 3.2 系列包括一系列可在 1 亿和 30 亿参数大小中使用的多语言生成语言模型。这些模型旨在处理文本输入并生成文本输出。此系列中的指令调整变体专门针对多语言对话应用进行了优化，包括涉及信息检索和摘要的代理方法。与许多现有的开源和专有聊天模型相比，Llama 3.2 指令调整模型在广泛使用的行业基准测试中表现出卓越的性能。

1B 和 3B 模型是从 Llama 8B 中剪枝得到的，然后使用 8B 和 70B 模型的 logits 作为标记级目标（标记级蒸馏）。使用知识蒸馏来恢复性能（它们是用 9 万亿个标记训练的）。1B 模型有 1,24B 个参数，量化为整数（Q8_0），3B 模型有 3.12B 个参数，使用 Q4_0 量化，最终大小分别为 1.3 GB 和 2 GB。其上下文窗口为 131,072 个标记。

![](img/file917.jpg)

**安装并运行** **模型**

```py
ollama run llama3.2:1b
```

使用之前的命令运行模型，我们应该有 Ollama 提示可用，以便我们输入问题并开始与 LLM 模型聊天；例如，

`>>> 法国首都是什么？`

几乎立即，我们得到了正确答案：

`The capital of France is Paris.`

在调用模型时使用 `--verbose` 选项将生成关于其性能的几个统计数据（模型将在我们第一次运行命令时进行轮询）。

![](img/file918.png)

每个指标都能揭示模型处理输入和生成输出的方式。以下是每个指标的含义分解：

+   **总持续时间（2.620170326 秒）**：这是从命令开始到响应完成所花费的完整时间。它包括加载模型、处理输入提示和生成响应。

+   **加载持续时间（39.947908 毫秒）**：这个持续时间表示将模型或必要组件加载到内存中的时间。如果这个值很小，可以表明模型是预先加载的，或者只需要最小的设置。

+   **提示评估计数（32 个标记）**：输入提示中的标记数量。在 NLP 中，标记通常是单词或子词，因此这个计数包括模型评估以理解和响应查询的所有标记。

+   **提示评估持续时间（1.644773 秒）**：这衡量模型评估或处理输入提示的时间。它占用了总持续时间的大部分，这意味着理解查询和准备响应是整个过程中最耗时的部分。

+   **提示评估速率（19.46 个标记/秒）**：这个速率表明模型处理输入提示中的标记有多快。它反映了模型在自然语言理解方面的速度。

+   **评估计数（8 个标记）**：这是模型响应中的标记数量，在这种情况下是，“法国的首都是巴黎。”

+   **评估持续时间（889.941 毫秒）**：这是根据评估输入生成输出所需的时间。它比提示评估要短得多，这表明生成响应比理解提示更简单或计算密集度更低。

+   **评估速率（8.99 个标记/秒）**：类似于提示评估速率，这表明模型生成输出标记的速度。这是理解模型在输出生成效率方面的一个关键指标。

这详细的分解可以帮助我们了解在边缘设备（如 Raspberry Pi 5）上运行 SLMs（如 Llama）的计算需求和性能特征。这表明，虽然提示评估耗时更长，但实际生成响应相对较快。这种分析对于优化性能和诊断实时应用中的潜在瓶颈至关重要。

加载并运行 3B 模型，我们可以看到相同提示下的性能差异；

![](img/file919.png)

评估率较低，5.3 个 token/s 与较小模型相比为 9 个 token/s。

当提出问题时

`>>> 巴黎和智利圣地亚哥之间的距离是多少？`

1B 模型回答了 `9,841 公里（6,093 英里）`，这是不准确的，而 3B 模型回答了 `7,300 英里（11,700 公里）`，接近正确答案（11,642 公里）。

让我们询问巴黎的坐标：

`>>> 巴黎的经纬度是多少？`

```py
The latitude and longitude of Paris are 48.8567° N (48°55'
42" N) and 2.3510° E (2°22' 8" E), respectively.
```

![](img/file920.png)

1B 和 3B 模型都给出了正确答案。

### Google Gemma 2 2B

让我们安装 [Gemma 2](https://ollama.com/library/gemma2:2b)，这是一个高性能且高效的模型，有三种尺寸：2B、9B 和 27B。我们将安装 [**Gemma 2 2B**](https://huggingface.co/collections/google/gemma-2-2b-release-66a20f3796a2ff2a7c76f98f)，这是一个经过 2 万亿 token 训练的轻量级模型，通过蒸馏从更大模型中学习，产生了超出预期的结果。该模型有 26 亿个参数和一个 Q4_0 量化，最终大小为 1.6 GB。其上下文窗口为 8,192 个 token。

![](img/file921.png)

**安装和运行** **模型**

```py
ollama run gemma2:2b --verbose
```

使用之前的命令运行模型，我们应该有 Ollama 提示可用，以便我们输入问题并开始与 LLM 模型聊天；例如，

`>>> 法国的首都是什么？`

几乎立即，我们得到了正确答案：

`法国的首都是**巴黎**。 🗼`

以及它的统计数据。

![](img/file922.png)

我们可以看到 Gemma 2:2B 的性能与 Llama 3.2:3B 大致相同，但参数更少。

**其他示例**:

```py
>>> What is the distance between Paris and Santiago, Chile?

The distance between Paris, France and Santiago, Chile is
approximately **7,000 miles (11,267 kilometers)**.

Keep in mind that this is a straight-line distance, and actual
travel distance can vary depending on the chosen routes and any
stops along the way. ✈️`
```

虽然回答很好，但不如 Llama3.2:3B 准确。

```py
>>> what is the latitude and longitude of Paris?

You got it! Here are the latitudes and longitudes of Paris,
France:

* **Latitude**: 48.8566° N (north)
* **Longitude**: 2.3522° E (east)

Let me know if you'd like to explore more about Paris or its
location! 🗼🇫🇷
```

一个好且准确的答案（比 Llama 的答案稍微冗长一些）。

### 微软 Phi3.5 3.8B

让我们拉取一个更大的（但仍然很小）模型，[PHI3.5](https://ollama.com/library/phi3.5)，这是微软的一个 3.8B 轻量级最先进开源模型。该模型属于 Phi-3 模型系列，支持 `128K token` 的上下文长度，以及以下语言：阿拉伯语、中文、捷克语、丹麦语、荷兰语、英语、芬兰语、法语、德语、希伯来语、匈牙利语、意大利语、日语、韩语、挪威语、波兰语、葡萄牙语、俄语、西班牙语、瑞典语、泰语、土耳其语和乌克兰语。

模型大小（以字节为单位）将取决于所使用的特定量化格式。大小可以从 2 位量化（`q2_k`）的 1.4 GB（高性能/低质量）到 16 位量化（fp-16）的 7.6 GB（低性能/高质量）不等。

让我们运行 4 位量化（`Q4_0`），这将需要 2.2GB 的 RAM，在输出质量和性能之间进行中间权衡。

```py
ollama run phi3.5:3.8b --verbose
```

> 您可以使用`run`或`pull`来下载模型。发生的情况是 Ollama 会记录拉取的模型，一旦 PHI3 不存在，在运行之前，Ollama 会拉取它。

让我们使用之前使用的相同提示进入：

```py
>>> What is the capital of France?

The capital of France is Paris. It' extradites significant
historical, cultural, and political importance to the country as
well as being a major European city known for its art, fashion,
gastronomy, and culture. Its influence extends beyond national
borders, with millions of tourists visiting each year from around
the globe. The Seine River flows through Paris before it reaches
the broader English Channel at Le Havre. Moreover, France is one
of Europe's leading economies with its capital playing a key role

...
```

答案非常“冗长”，让我们指定一个更好的提示：

![](img/file923.png)

在这种情况下，答案仍然比我们预期的要长，评估速率为 2.25 个 token/秒，是 Gemma 和 Llama 的两倍多。

> 选择最合适的提示是与 LLM 一起使用的重要技能之一，无论其大小如何。

当我们询问关于距离和经纬度的相同问题时，我们没有得到关于`13,507 公里（8,429 英里）`距离的良好答案，但对于坐标来说还可以。再次强调，它可能更加简洁（每个答案超过 200 个 token）。

由于它们的速度相对适中，我们可以使用任何模型作为助手，但截至 2023 年 9 月 24 日，Llama2:3B 是一个更好的选择。您应该根据您的需求尝试其他模型。[🤗 Open LLM Leaderboard](https://huggingface.co/spaces/open-llm-leaderboard/open_llm_leaderboard)可以给您一个关于最佳模型在大小、基准、许可等方面的想法。

> 最好的模型是适合您特定需求的模型。同时，考虑到这个领域每天都在随着新模型的发展而演变。

### 多模态模型

多模态模型是能够处理和理解来自多个来源的信息的人工智能（AI）系统，如图像、文本、音频和视频。在我们的上下文中，多模态 LLM 可以处理各种输入，包括文本、图像和音频，并将这些提示转换为各种输出，而不仅仅是源类型。

我们将在这里使用[LLaVA-Phi-3](https://ollama.com/library/llava-phi3:3.8b)，这是一个来自 Phi 3 Mini 4k 的微调过的 LLaVA 模型。它具有与原始[LLaVA](https://llava-vl.github.io/)（大型语言和视觉助手）模型相当的性能基准。

LLaVA-Phi-3 是一个端到端训练的大型多模态模型，旨在根据视觉输入（图像）和文本指令理解和生成内容。它结合了视觉编码器和语言模型的能力，以处理和响应用户的多模态输入。

让我们安装模型：

```py
ollama run llava-phi3:3.8b --verbose
```

让我们从文本输入开始：

```py
>>> You are a helpful AI assistant.
    What is the capital of France?

As an AI language model, I can tell you that the capital of
France is Paris. It's not only the largest city in the country
but also serves as its political and administrative center.
Paris is known for its iconic landmarks such as the Eiffel
Tower, Notre-Dame Cathedral, and the Louvre Museum. The city
has a rich history, beautiful architecture, and is widely
considered to be one of the most romantic cities in the world.
```

响应大约需要 30 秒，评估速率为 3.93 个 token/秒！还不错！

但让我们知道以图像作为输入进入。为此，让我们创建一个工作目录：

```py
cd Documents/
mkdir OLLAMA
cd OLLAMA
```

让我们从互联网下载一张 <semantics><mrow><mn>640</mn><mo>×</mo><mn>320</mn></mrow><annotation encoding="application/x-tex">640\times 320</annotation></semantics> 图片，例如（维基百科：[法国巴黎](https://upload.wikimedia.org/wikipedia/commons/thumb/4/4b/La_Tour_Eiffel_vue_de_la_Tour_Saint-Jacques%2C_Paris_ao%C3%BBt_2014_%282%29.jpg/640px-La_Tour_Eiffel_vue_de_la_Tour_Saint-Jacques%2C_Paris_ao%C3%BBt_2014_%282%29.jpg)）：

![图片](img/file924.jpg)

例如，使用 FileZilla 将图片上传到 Raspi-5 的 OLLAMA 文件夹，并命名为 `image_test_1.jpg`。我们应该有完整的图片路径（我们可以使用 `pwd` 来获取它）。

`/home/mjrovai/Documents/OLLAMA/image_test_1.jpg`

如果你使用的是桌面，可以通过右键点击图片来复制图片路径。

![图片](img/file925.png)

使用以下提示进入：

```py
>>> Describe the image /home/mjrovai/Documents/OLLAMA/\
                        image_test_1.jpg
```

结果很棒，但整体延迟很大；推理几乎需要 4 分钟。

![图片](img/file926.png)

### 检查本地资源

使用 htop，我们可以监控设备上运行的资源。

```py
htop
```

在模型运行期间，我们可以检查资源：

![图片](img/file927.png)

所有四个 CPU 都几乎以 100% 的容量运行，加载模型时使用的内存为 `3.24 GB`。退出 Ollama 后，内存下降到大约 `377 MB`（没有桌面）。

监控温度也非常重要。当使用桌面运行 Raspberry 时，你可以在任务栏上看到温度：

![图片](img/file928.png)

如果你处于“无头”状态，可以使用以下命令监控温度：

```py
vcgencmd measure_temp
```

如果你什么也不做，CPU 在 1% 运行时，温度大约为 `50°C`。在推理过程中，当 CPU 达到 100% 时，温度可以上升到几乎 `70°C`。这是正常的，意味着活动散热器正在工作，将温度保持在 80°C / 85°C（其限制）以下。

## Ollama Python 库

到目前为止，我们已经使用终端上的命令探索了 SLMs 的聊天功能。然而，我们希望将这些模型集成到我们的项目中，因此 Python 似乎是一条正确的路径。好消息是 Ollama 有这样的库。

[Ollama Python 库](https://github.com/ollama/ollama-python) 简化了与高级 LLM 模型的交互，除了提供将 Python 3.8+ 项目与 [Ollama](https://github.com/ollama/ollama) 集成的最简单方式外，还使更复杂的响应和功能成为可能。

为了更好地理解如何使用 Python 和 Ollama 创建应用程序，我们可以参考 [Matt Williams 的视频](https://www.youtube.com/@technovangelist)，如下所示：

[`www.youtube.com/embed/_4K20tOsXK8`](https://www.youtube.com/embed/_4K20tOsXK8)

**安装**：

在终端中运行以下命令：

```py
pip install ollama
```

我们需要一个文本编辑器或 IDE 来创建 Python 脚本。如果你在桌面上运行 Raspberry OS，Thonny 和 Geany 等几个选项已经默认安装（通过 `[Menu][Programming]` 访问）。你可以从 `[Menu][Recommended Software]` 下载其他 IDE，例如 Visual Studio Code。当窗口弹出时，转到 `[Programming]`，选择你的选项，然后按 `[Apply]`。

![](img/file929.png)

如果你更喜欢使用 Jupyter Notebook 进行开发：

```py
pip install jupyter
jupyter notebook --generate-config
```

要运行 Jupyter Notebook，运行以下命令（更改 IP 地址为你的）：

```py
jupyter notebook --ip=192.168.4.209 --no-browser
```

在终端上，你可以看到打开 notebook 的本地 URL 地址：

![](img/file930.png)

我们可以通过在网页浏览器中输入树莓派的 IP 地址和提供的令牌来从另一台计算机访问它（我们应该从终端复制它）。

在 Raspi 的工作目录中，我们将创建一个新的 Python 3 笔记本。

让我们用一个非常简单的脚本来验证已安装的模型：

```py
import ollama

ollama.list()
```

所有模型都将打印为字典，例如：

```py
  {'name': 'gemma2:2b',
   'model': 'gemma2:2b',
   'modified_at': '2024-09-24T19:30:40.053898094+01:00',
   'size': 1629518495,
   'digest': (
     '8ccf136fdd5298f3ffe2d69862750ea7fb56555fa4d5b18c0'
     '4e3fa4d82ee09d7'
    ),

   'details': {'parent_model': '',
    'format': 'gguf',
    'family': 'gemma2',
    'families': ['gemma2'],
    'parameter_size': '2.6B',
    'quantization_level': 'Q4_0'}}]}
```

让我们重复之前做的一个问题，但现在使用 Ollama Python 库中的`ollama.generate()`。这个 API 将为给定的提示生成响应，并使用提供的模型。这是一个流式端点，所以会有一系列的响应。最终的响应对象将包括请求的统计信息和附加数据。

```py
MODEL = "gemma2:2b"
PROMPT = "What is the capital of France?"

res = ollama.generate(model=MODEL, prompt=PROMPT)
print(res)
```

如果你将代码作为 Python 脚本运行，你应该保存它，例如，test_ollama.py。你可以使用 IDE 运行它或在终端上直接运行。此外，请记住，在运行独立脚本时，你应该始终调用模型并定义它。

```py
python test_ollama.py
```

因此，我们将以 JSON 格式获得模型响应：

```py
{
  'model': 'gemma2:2b',
  'created_at': '2024-09-25T14:43:31.869633807Z',
  'response': 'The capital of France is **Paris**.\n',
  'done': True,
  'done_reason': 'stop',
  'context': [
      106, 1645, 108, 1841, 603, 573, 6037, 576, 6081, 235336,
      107, 108, 106, 2516, 108, 651, 6037, 576, 6081, 603, 5231,
      29437, 168428, 235248, 244304, 241035, 235248, 108
  ],
  'total_duration': 24259469458,
  'load_duration': 19830013859,
  'prompt_eval_count': 16,
  'prompt_eval_duration': 1908757000,
  'eval_count': 14,
  'eval_duration': 2475410000
}
```

如我们所见，生成了几条信息，例如：

+   **response**：模型针对我们的提示生成的文本输出。

    +   `法国的首都是**巴黎**。🇫🇷`

+   **context**：表示模型使用的输入和上下文的标记 ID。标记是用于语言模型处理的文本的数值表示。

    +   `[106, 1645, 108, 1841, 603, 573, 6037, 576, 6081, 235336, 107, 108, 106, 2516, 108, 651, 6037, 576, 6081, 603, 5231, 29437, 168428, 235248, 244304, 241035, 235248, 108]`

性能指标：

+   **total_duration**：操作所需的总时间，以纳秒为单位。在这种情况下，大约 24.26 秒。

+   **load_duration**：加载模型或组件所需的时间，以纳秒为单位。大约 19.83 秒。

+   **prompt_eval_duration**：评估提示所需的时间，以纳秒为单位。大约 16 纳秒。

+   **eval_count**：生成过程中评估的标记数量。这里，14 个标记。

+   **eval_duration**：模型生成响应所需的时间，以纳秒为单位。大约 2.5 秒。

但是，我们想要的是简单的‘响应’以及，也许为了分析，推理的总时长，所以让我们修改代码从字典中提取它：

```py
print(f"\n{res['response']}")
print(
    f"\n [INFO] Total Duration: "
    f"{res['total_duration']/1e9:.2f} seconds"
)
```

现在，我们得到了：

```py
The capital of France is **Paris**. 🇫🇷

 [INFO] Total Duration: 24.26 seconds
```

**使用 Ollama.chat()**

获取我们响应的另一种方式是使用`ollama.chat()`，它生成与提供的模型进行的聊天中的下一个消息。这是一个流式端点，因此将发生一系列响应。可以通过将`"stream": false`禁用来关闭流。最终响应对象还将包括请求的统计信息和附加数据。

```py
PROMPT_1 = "What is the capital of France?"

response = ollama.chat(
    model=MODEL,
    messages=[
        {
            "role": "user",
            "content": PROMPT_1,
        },
    ],
)
resp_1 = response["message"]["content"]
print(f"\n{resp_1}")
print(
    f"\n [INFO] Total Duration: "
    f"{(res['total_duration']/1e9):.2f} seconds"
)
```

答案与之前相同。

一个重要的考虑因素是，使用`ollama.generate()`后，响应从模型的“记忆”中“清除”（仅使用一次），但如果我们想保持对话，我们必须使用`ollama.chat()`。让我们看看它是如何工作的：

```py
PROMPT_1 = "What is the capital of France?"
response = ollama.chat(
    model=MODEL,
    messages=[
        {
            "role": "user",
            "content": PROMPT_1,
        },
    ],
)
resp_1 = response["message"]["content"]
print(f"\n{resp_1}")
print(
    f"\n [INFO] Total Duration: "
    f"{(response['total_duration']/1e9):.2f} seconds"
)

PROMPT_2 = "and of Italy?"
response = ollama.chat(
    model=MODEL,
    messages=[
        {
            "role": "user",
            "content": PROMPT_1,
        },
        {
            "role": "assistant",
            "content": resp_1,
        },
        {
            "role": "user",
            "content": PROMPT_2,
        },
    ],
)
resp_2 = response["message"]["content"]
print(f"\n{resp_2}")
print(
    f"\n [INFO] Total Duration: "
    f"{(response_2['total_duration']/1e9):.2f} seconds"
)
```

在上述代码中，我们运行了两个查询，第二个提示考虑了第一个查询的结果。

这是模型的响应：

```py
The capital of France is **Paris**. 🇫🇷

 [INFO] Total Duration: 2.82 seconds

The capital of Italy is **Rome**. 🇮🇹

 [INFO] Total Duration: 4.46 seconds
```

**获取图像描述**：

就像我们使用`LlaVa-PHI-3`模型通过命令行分析图像一样，这里也可以用 Python 做同样的事情。让我们使用同样的巴黎图像，但现在使用`ollama.generate()`：

```py
MODEL = "llava-phi3:3.8b"
PROMPT = "Describe this picture"

with open("image_test_1.jpg", "rb") as image_file:
    img = image_file.read()

response = ollama.generate(model=MODEL, prompt=PROMPT, images=[img])
print(f"\n{response['response']}")
print(
    f"\n [INFO] Total Duration: "
    f"{(res['total_duration']/1e9):.2f} seconds"
)
```

这里是结果：

```py
This image captures the iconic cityscape of Paris, France. The
vantage point is high, providing a panoramic view of the Seine
River that meanders through the heart of the city. Several
bridges arch gracefully over the river, connecting different
parts of the city. The Eiffel Tower, an iron lattice structure
with a pointed top and two antennas on its summit, stands
tall in the background, piercing the sky. It is painted in a
light gray color, contrasting against the blue sky speckled
with white clouds.

The buildings that line the river are predominantly white or
beige, their uniform color palette broken occasionally by red
roofs peeking through. The Seine River itself appears calm
and wide, reflecting the city's architectural beauty in its
surface. On either side of the river, trees add a touch of
green to the urban landscape.

The image is taken from an elevated perspective, looking down
on the city. This viewpoint allows for a comprehensive view of
Paris's beautiful architecture and layout. The relative
positions of the buildings, bridges, and other structures
create a harmonious composition that showcases the city's charm.

In summary, this image presents a serene day in Paris, with its
architectural marvels - from the Eiffel Tower to the river-side
buildings - all bathed in soft colors under a clear sky.

 [INFO] Total Duration: 256.45 seconds
```

模型大约用了 4 分钟（256.45 秒）返回详细的图像描述。

> 在[10-Ollama_Python_Library](https://github.com/Mjrovai/EdgeML-with-Raspberry-Pi/blob/main/OLLAMA_SLMs/10-Ollama_Python_Library.ipynb)笔记本中，可以找到使用 Ollama Python 库的实验。

### 函数调用

到目前为止，我们可以观察到，通过将模型的响应放入一个变量中，我们可以有效地将其融入现实世界的项目中。然而，当模型对相同的输入提供不同的响应时，就会出现一个主要问题。例如，假设我们只需要在之前的例子中模型响应的国家首都名称及其坐标，没有任何附加信息，即使使用像微软 Phi 这样的冗长模型。为了确保响应的一致性，我们可以使用“Ollama 函数调用”，它与 OpenAI API 完全兼容。

#### 但“函数调用”到底是什么呢？

在现代人工智能中，使用大型语言模型（LLMs）进行函数调用允许这些模型执行生成文本之外的操作。通过集成外部函数或 API，LLMs 可以访问实时数据，自动化任务，并与各种系统交互。

例如，与其仅仅响应关于天气的查询，一个 LLM 可以调用天气 API 来获取当前条件并提供准确、最新的信息。这种能力增强了模型响应的相关性和准确性，使其成为驱动工作流程和自动化流程的强大工具，使其成为现实世界应用中的积极参与者。

关于函数调用的更多详细信息，请参阅由[Marvin Prison](https://www.youtube.com/@MervinPraison)制作的此视频：

[`www.youtube.com/embed/eHfMCtlsb1o`](https://www.youtube.com/embed/eHfMCtlsb1o)

#### 让我们创建一个项目。

我们希望创建一个*应用程序*，用户输入一个国家的名称，然后得到输出，即该国首都与该应用程序位置（为了简单起见，我们将使用智利的圣地亚哥作为应用程序位置）之间的距离（公里）。

![图片](img/file931.png)

一旦用户输入国家名称，模型将返回该国家的首都名称（作为字符串）以及该城市的纬度和经度（以浮点数表示）。使用这些坐标，我们可以使用一个简单的 Python 库([haversine](https://pypi.org/project/haversine/))来计算这两个点之间的距离。

这个项目的想法是展示语言模型交互、使用 Pydantic 进行结构化数据处理以及使用 Haversine 公式（传统计算）进行地理空间计算的组合。

首先，让我们安装一些库。除了*Haversine*，主要的一个是[OpenAI Python 库](https://github.com/openai/openai-python)，它为任何 Python 3.7+应用程序提供了方便的访问 OpenAI REST API 的方式。另一个是[Pydantic](https://docs.pydantic.dev/latest/)（和 instructor），这是一个由 Python 构建的强大数据验证和设置管理库，旨在增强我们代码库的健壮性和可靠性。简而言之，*Pydantic*将帮助确保我们的模型响应始终一致。

```py
pip install haversine
pip install openai
pip install pydantic
pip install instructor
```

现在，我们应该创建一个 Python 脚本，用于与我们的模型（LLM）交互，以确定一个国家首都的坐标，并计算从圣地亚哥到该首都的距离。

让我们来看看代码：

### 1. 导入库

```py
import sys
from haversine import haversine
from openai import OpenAI
from pydantic import BaseModel, Field
import instructor
```

+   **sys**：提供对系统特定参数和函数的访问。它用于获取命令行参数。

+   **haversine**：一个来自 haversine 库的函数，它使用 Haversine 公式计算两个地理点之间的距离。

+   **openAI**：一个用于与 OpenAI API 交互的模块（尽管它与本地设置（Ollama）一起使用，但这里一切都是离线的）。

+   **pydantic**：使用 Python 类型注解提供数据验证和设置管理。它用于定义预期响应数据的结构。

+   **instructor**：一个模块，用于修补 OpenAI 客户端以在特定模式下工作（可能与其结构化数据处理有关）。

### 2. 定义输入和模型

```py
country = sys.argv[1]  # Get the country from
# command-line arguments
MODEL = "phi3.5:3.8b"  # The name of the model to be used
mylat = -33.33  # Latitude of Santiago de Chile
mylon = -70.51  # Longitude of Santiago de Chile
```

+   **country**：在 Python 脚本中，可以从命令行参数中获取国家名称。在 Jupyter 笔记本中，我们可以输入其名称，例如，

    +   `country = "France"`

+   **MODEL**：指定正在使用的模型，在这个例子中是 phi3.5。

+   **mylat** **和** **mylon**：圣地亚哥的坐标，用作距离计算的起点。

### 3. 定义响应数据结构

```py
class CityCoord(BaseModel):
    city: str = Field(..., description="Name of the city")
    lat: float = Field(
        ..., description="Decimal Latitude of the city"
    )
    lon: float = Field(
        ..., description="Decimal Longitude of the city"
    )
```

+   **CityCoord**：一个 Pydantic 模型，用于定义 LLM 响应的预期结构。它期望有三个字段：city（城市名称）、lat（纬度）和 lon（经度）。

### 4. 设置 OpenAI 客户端

```py
client = instructor.patch(
    OpenAI(
        base_url="http://localhost:11434/v1",  # Local API base
        # URL (Ollama)
        api_key="ollama",  # API key
        # (not used)
    ),
    mode=instructor.Mode.JSON,  # Mode for
    # structured
    # JSON output
)
```

+   **OpenAI**: 此设置初始化了一个带有本地基本 URL 和 API 密钥（ollama）的 OpenAI 客户端。它使用本地服务器。

+   **instructor.patch**: 修补 OpenAI 客户端以在 JSON 模式下工作，启用与 Pydantic 模型匹配的结构化输出。

### 5. 生成响应

```py
resp = client.chat.completions.create(
    model=MODEL,
    messages=[
        {
            "role": "user",
            "content": f"return the decimal latitude and \
 decimal longitude of the capital of the {country}.",
        }
    ],
    response_model=CityCoord,
    max_retries=10,
)
```

+   **client.chat.completions.create**: 调用 LLM 生成响应。

+   **model**: 指定要使用的模型（llava-phi3）。

+   **messages**: 包含对 LLM 的提示，要求提供指定国家首都的纬度和经度。

+   **response_model**: 指示响应应符合 CityCoord 模型。

+   **max_retries**: 如果请求失败，最大重试次数。

### 6. 计算距离

```py
distance = haversine((mylat, mylon), (resp.lat, resp.lon), unit="km")

print(
    f"Santiago de Chile is about {int(round(distance, -1))} "
    f"kilometers away from {resp.city}."
)
```

+   **haversine**: 使用各自的坐标计算 LLM 返回的圣地亚哥·德·智利的首都之间的距离。

+   **(mylat, mylon)**: 圣地亚哥·德·智利的坐标。

+   **resp.city**: 国家的首都名称

+   **(resp.lat, resp.lon)**: 首都的坐标由 LLM 响应提供。

+   **unit = ‘km’**: 指定距离应以公里计算。

+   **print**: 输出距离，四舍五入到最接近的 10 公里，并使用千位分隔符以提高可读性。

**运行代码**

如果我们输入不同的国家，例如，法国、哥伦比亚和美国，我们可以注意到我们总是收到相同结构化的信息：

```py
Santiago de Chile is about 8,060 kilometers away from
    Washington, D.C..
Santiago de Chile is about 4,250 kilometers away from Bogotá.
Santiago de Chile is about 11,630 kilometers away from Paris.
```

如果您将代码作为脚本运行，结果将在终端上打印：

![图片](img/file932.png)

计算结果相当准确！

![图片](img/file933.png)

> 在 [20-Ollama_Function_Calling](https://github.com/Mjrovai/EdgeML-with-Raspberry-Pi/blob/main/OLLAMA_SLMs/20-Ollama_Function_Calling.ipynb) 笔记本中，可以找到所有已安装模型的实验。

### 添加图片

现在是时候总结到目前为止的内容了！让我们修改脚本，以便用户输入的不是国家名称（作为文本），而是输入一个图像，并且基于 SLM 的应用程序返回图像中的城市及其地理位置。有了这些数据，我们可以像以前一样计算距离。

![图片](img/file934.png)

为了简单起见，我们将分两步实现此新代码。首先，LLM 将分析图像并创建描述（文本）。此文本将被传递到另一个实例，其中模型将提取所需信息以传递。

我们将开始导入库

```py
import sys
import time
from haversine import haversine
import ollama
from openai import OpenAI
from pydantic import BaseModel, Field
import instructor
```

如果您在 Jupyter Notebook 上运行代码，我们可以看到这张图片。为此，我们还需要导入：

```py
import matplotlib.pyplot as plt
from PIL import Image
```

> 如果我们以脚本方式运行代码，则不需要这些库。

现在，我们定义模型和本地坐标：

```py
MODEL = "llava-phi3:3.8b"
mylat = -33.33
mylon = -70.51
```

我们可以下载一张新图片，例如，来自维基百科的马丘比丘。在笔记本中我们可以看到它：

```py
# Load the image
img_path = "image_test_3.jpg"
img = Image.open(img_path)

# Display the image
plt.figure(figsize=(8, 8))
plt.imshow(img)
plt.axis("off")
# plt.title("Image")
plt.show()
```

![图片](img/file935.jpg)

现在，让我们定义一个函数，该函数将接收图像并将`返回图像中城市的十进制度量和十进制度，以及该城市的名称和所在国家`

```py
def image_description(img_path):
    with open(img_path, "rb") as file:
        response = ollama.chat(
            model=MODEL,
            messages=[
                {
                    "role": "user",
                    "content": """return the decimal latitude and \
 decimal longitude of the city in the image, \
 its name, and what country it is located""",
                    "images": [file.read()],
                },
            ],
            options={
                "temperature": 0,
            },
        )
    # print(response['message']['content'])
    return response["message"]["content"]
```

> 我们可以打印整个响应以供调试目的。

为该函数生成的图像描述将再次作为提示传递给模型。

```py
start_time = time.perf_counter()  # Start timing


class CityCoord(BaseModel):
    city: str = Field(
        ..., description="Name of the city in the image"
    )
    country: str = Field(
        ...,
        description=(
            "Name of the country where "
            "the city in the image is located"
        ),
    )
    lat: float = Field(
        ...,
        description=("Decimal latitude of the city in " "the image"),
    )
    lon: float = Field(
        ...,
        description=("Decimal longitude of the city in " "the image"),
    )


# enables `response_model` in create call
client = instructor.patch(
    OpenAI(base_url="http://localhost:11434/v1", api_key="ollama"),
    mode=instructor.Mode.JSON,
)

image_description = image_description(img_path)
# Send this description to the model
resp = client.chat.completions.create(
    model=MODEL,
    messages=[
        {
            "role": "user",
            "content": image_description,
        }
    ],
    response_model=CityCoord,
    max_retries=10,
    temperature=0,
)
```

如果我们打印图像描述，我们将得到：

```py
The image shows the ancient city of Machu Picchu, located in
Peru. The city is perched on a steep hillside and consists of
various structures made of stone. It is surrounded by lush
greenery and towering mountains. The sky above is blue with
scattered clouds.

Machu Picchu's latitude is approximately 13.5086° S, and its
longitude is around 72.5494° W.
```

模型的第二个响应（`resp`）将是：

```py
CityCoord(city='Machu Picchu', country='Peru', lat=-13.5086,
                lon=-72.5494)
```

现在，我们可以进行“后处理”，计算距离并准备最终答案：

```py
distance = haversine((mylat, mylon), (resp.lat, resp.lon), unit="km")

print(
    (
        f"\nThe image shows {resp.city}, with lat: "
        f"{round(resp.lat, 2)} and long: "
        f"{round(resp.lon, 2)}, located in "
        f"{resp.country} and about "
        f"{int(round(distance, -1)):,} kilometers "
        f"away from Santiago, Chile.\n"
    )
)

end_time = time.perf_counter()  # End timing
elapsed_time = end_time - start_time  # Calculate elapsed time
print(
    f"[INFO] ==> The code (running {MODEL}), "
    f"took {elapsed_time:.1f} seconds to execute.\n"
)
```

我们将得到：

```py
 The image shows Machu Picchu, with lat:-13.16 and long:
 -72.54, located in Peru  and about 2,250 kilometers away
 from Santiago, Chile.

print(
    f"[INFO] ==> The code (running {MODEL}), "
    f"took {elapsed_time:.1f} seconds "
    f"to execute.\n"
)
```

在[30-Function_Calling_with_images](https://github.com/Mjrovai/EdgeML-with-Raspberry-Pi/blob/main/OLLAMA_SLMs/30-Function_Calling_with_images.ipynb)笔记本中，可以找到使用多张图像的实验。

现在，让我们从 GitHub 下载脚本`calc_distance_image.py`并在终端中使用以下命令运行它：

```py
python calc_distance_image.py \
  /home/mjrovai/Documents/OLLAMA/image_test_3.jpg
```

使用马丘比丘图像完整补丁作为参数。我们将得到相同的前一个结果。

![图片](img/file936.png)

那么，巴黎怎么样？

![图片](img/file937.png)

当然，有许多方法可以优化这里使用的代码。但想法是探索在边缘使用 SLMs 进行**函数调用**的巨大潜力，使这些模型能够与外部函数或 API 集成。超越文本生成，SLMs 可以访问实时数据，自动化任务，并与各种系统交互。

## SLMs：优化技术

大型语言模型（LLMs）已经彻底改变了自然语言处理，但它们的部署和优化带来了独特的挑战。一个重大问题是 LLMs（以及更多的 SLMs）倾向于生成听起来合理但实际上错误的信息，这种现象被称为**幻觉**。这发生在模型产生看似连贯但缺乏真实或现实世界事实的内容时。

其他挑战包括训练和运行这些模型所需的巨大计算资源、在模型中维护最新知识的难度，以及需要特定领域的适应性。在训练或推理过程中处理敏感数据时，也会出现隐私问题。此外，确保跨不同任务的一致性能以及保持这些强大工具的道德使用也带来了持续挑战。解决这些问题对于在现实世界应用中有效且负责任地部署大型语言模型（LLMs）至关重要。

提高 LLM（和 SLM）性能和效率的基本技术包括微调、提示工程和检索增强生成（RAG）。

+   **微调**，虽然资源消耗更大，但为将 LLMs 特定化到特定领域或任务提供了一种方法。这个过程涉及在精心挑选的数据集上进一步训练模型，使其能够将广泛的一般知识适应到特定应用中。微调可以带来性能的显著提升，尤其是在特定领域或独特用例中。

+   **提示工程**是 LLM 优化的前沿。通过精心设计输入提示，我们可以引导模型生成更准确和相关的输出。这种技术涉及构建利用模型预先训练的知识和能力的问题结构，通常包括示例或具体指令来塑造期望的响应。

+   **检索增强生成（RAG）**代表了一种提高 LLM 性能的强大方法。这种方法结合了预训练模型中嵌入的广泛知识以及访问和整合外部最新信息的能力。通过检索相关数据以补充模型的决策过程，RAG 可以显著提高准确性并减少生成过时或错误信息的可能性。

对于边缘应用，更有益的是关注像 RAG 这样的技术，这些技术可以增强模型性能，而无需在设备上进行微调。让我们来探讨它。

## RAG 实现

在用户与语言模型的基本交互中，用户提出一个问题，这个问题作为提示发送给模型。模型根据其预先训练的知识生成一个响应。在 RAG 过程中，用户的问题和模型的响应之间有一个额外的步骤。用户的问题触发从知识库中检索的过程。

![](img/file938.png)

### 一个简单的 RAG 项目

这里是实现基本检索增强生成（RAG）的步骤：

+   **确定你将使用的文档类型**：最佳类型是我们可以从中获得干净且未被遮挡的文本的文档。PDF 可能存在问题，因为它们是为打印而设计的，而不是为了提取有意义的文本。为了处理 PDF，我们应该获取源文档或使用工具来处理它。

+   **将文本分块**：由于上下文大小限制和混淆的可能性，我们不能将文本存储为一个长流。分块涉及将文本分成更小的部分。分块文本有多种方式，例如字符计数、标记、单词、段落或部分。也可以重叠分块。

+   **创建嵌入**：嵌入是捕获语义意义的文本的数值表示。我们通过将每个文本块通过特定的嵌入模型来创建嵌入。模型输出一个向量，其长度取决于所使用的嵌入模型。我们应该从 Ollama 拉取一个（或多个）[嵌入模型](https://ollama.com/blog/embedding-models)来执行此任务。以下是 Ollama 可用的嵌入模型的一些示例。

    | 模型 | 参数大小 | 嵌入大小 |
    | --- | --- | --- |
    | mxbai-embed-large | 334M | 1024 |
    | nomic-embed-text | 137M | 768 |
    | all-minilm | 23M | 384 |

    > 通常，更大的嵌入大小可以捕获关于输入的更细微的信息。然而，它们也需要更多的计算资源来处理，并且参数数量的增加应该会增加延迟（但也会提高响应的质量）。

+   **将片段和嵌入存储在向量数据库中**：我们需要一种方法来高效地找到给定提示中最相关的文本片段，这就是向量数据库的作用。我们将使用[Chromadb](https://www.trychroma.com/)，这是一个 AI 原生开源向量数据库，通过创建知识、事实和技能插件简化了 RAG 的构建。每个片段的嵌入和源文本都存储在其中。

+   **构建提示**：当我们有一个问题时，我们创建一个嵌入并查询向量数据库以找到最相似的片段。然后，我们选择前几个结果并将它们的文本包含在提示中。

RAG 的目标是为模型提供我们文档中最相关的信息，使其能够生成更准确和更有信息量的响应。因此，让我们实现一个简单示例，将关于蜜蜂的特定事实集（“蜜蜂事实”）纳入 SLM 中。

在`ollama`环境中，在终端中输入 Chromadb 安装命令：

```py
pip install ollama chromadb
```

让我们拉取一个中间嵌入模型，`nomic-embed-text`

```py
ollama pull nomic-embed-text
```

并创建一个工作目录：

```py
cd Documents/OLLAMA/
mkdir RAG-simple-bee
cd RAG-simple-bee/
```

让我们创建一个新的 Jupyter 笔记本，[40-RAG-simple-bee](https://github.com/Mjrovai/EdgeML-with-Raspberry-Pi/blob/main/OLLAMA_SLMs/40-RAG-simple-bee.ipynb)来进行一些探索：

导入所需的库：

```py
import ollama
import chromadb
import time
```

定义 aor 模型：

```py
EMB_MODEL = "nomic-embed-text"
MODEL = "llama3.2:3B"
```

首先，应该创建一个关于蜜蜂事实的知识库。这涉及到收集相关文档并将它们转换为向量嵌入。然后，这些嵌入存储在向量数据库中，以便以后进行高效的相似性搜索。以“文档”的形式输入“蜜蜂事实”的基础列表：

```py
documents = [
    "Bee-keeping, also known as apiculture, involves the \
 maintenance of bee colonies, typically in hives, by humans.",
    "The most commonly kept species of bees is the European \
 honey bee (Apis  mellifera).",

    ...

    "There are another 20,000 different bee species in \
 the world.",
    "Brazil alone has more than 300 different bee species, and \
 the vast majority, unlike western honey bees, don’t sting.",
    "Reports written in 1577 by Hans Staden, mention three \
 native bees used by indigenous people in Brazil.", \
    "The indigenous people in Brazil used bees for medicine \
 and food purposes",
    "From Hans Staden report: probable species: mandaçaia \
 (Melipona quadrifasciata), mandaguari (Scaptotrigona \
 postica) and jataí-amarela (Tetragonisca angustula)."
]
```

> 我们在这里不需要“分块”文档，因为我们将会使用列表中的每个元素和一个片段。

现在，我们将创建我们的向量嵌入数据库`bee_facts`并将文档存储在其中：

```py
client = chromadb.Client()
collection = client.create_collection(name="bee_facts")

# store each document in a vector embedding database
for i, d in enumerate(documents):
    response = ollama.embeddings(model=EMB_MODEL, prompt=d)
    embedding = response["embedding"]
    collection.add(
        ids=[str(i)], embeddings=[embedding], documents=[d]
    )
```

现在我们已经创建了“知识库”，我们可以开始进行查询，从其中检索数据：

![](img/file939.png)

**用户查询**：这个过程从用户提出问题开始，例如：“一个蜂群中有多少蜜蜂？谁产卵，有多少？常见的害虫和疾病又是怎样的？”

```py
prompt = "How many bees are in a colony? Who lays eggs and \
 how much? How about common pests and diseases?"
```

**查询嵌入**：用户的提问被转换为向量嵌入，使用与知识库相同的嵌入模型。

```py
response = ollama.embeddings(prompt=prompt, model=EMB_MODEL)
```

**相关文档检索**：系统使用查询嵌入在知识库中进行搜索，以找到最相关的文档（在这种情况下，5 个最可能的文档）。这是通过相似性搜索完成的，它将查询嵌入与数据库中的文档嵌入进行比较。

```py
results = collection.query(
    query_embeddings=[response["embedding"]], n_results=5
)
data = results["documents"]
```

**提示增强**：检索到的相关信息与原始用户查询相结合，以创建增强提示。现在，这个提示包含了用户的问题和知识库中的相关事实。

```py
prompt = (
    f"Using this data: {data}. " f"Respond to this prompt: {prompt}"
)
```

**答案生成**：增强后的提示随后被输入到语言模型中，在这种情况下，是`llama3.2:3b`模型。该模型使用这个丰富的上下文来生成全面的答案。参数如温度、top_k 和 top_p 被设置为控制生成响应的随机性和质量。

```py
output = ollama.generate(
  model=MODEL,
  prompt = (
      f"Using this data: {data}. "
      f"Respond to this prompt: {prompt}"
  )

  options={
    "temperature": 0.0,
    "top_k":10,
    "top_p":0.5                          }
)
```

**响应交付**：最后，系统将生成的答案返回给用户。

```py
print(output["response"])
```

```py
Based on the provided data, here are the answers to your \
questions:

1. How many bees are in a colony?
A typical bee colony can contain between 20,000 and 80,000 bees.

2. Who lays eggs and how much?
The queen bee lays up to 2,000 eggs per day during peak seasons.

3. What about common pests and diseases?
Common pests and diseases that affect bees include varroa \
mites, hive beetles, and foulbrood.
```

让我们创建一个函数来帮助回答新问题：

```py
def rag_bees(prompt, n_results=5, temp=0.0, top_k=10, top_p=0.5):
    start_time = time.perf_counter()  # Start timing

    # generate an embedding for the prompt and retrieve the data
    response = ollama.embeddings(
      prompt=prompt,
      model=EMB_MODEL
    )

    results = collection.query(
      query_embeddings=[response["embedding"]],
      n_results=n_results
    )
    data = results['documents']

    # generate a response combining the prompt and data retrieved
    output = ollama.generate(
      model=MODEL,
      prompt = (
         f"Using this data: {data}. "
         f"Respond to this prompt: {prompt}"
      )

      options={
        "temperature": temp,
        "top_k": top_k,
        "top_p": top_p                          }
    )

    print(output['response'])

    end_time = time.perf_counter()  # End timing
    elapsed_time = round(
       (end_time - start_time), 1
    )  # Calculate elapsed time

print(
    f"\n[INFO] ==> The code for model: {MODEL}, "
    f"took {elapsed_time}s to generate the answer.\n"
)

    print(
       f"\n[INFO] ==> The code for model: {MODEL}, "
       f"took {elapsed_time}s to generate the answer.\n"
    )
```

我们现在可以创建查询并调用函数：

```py
prompt = "Are bees in Brazil?"
rag_bees(prompt)
```

```py
Yes, bees are found in Brazil. According to the data, Brazil \
has more than 300 different bee species, and indigenous people \
in Brazil used bees for medicine and food purposes. \
Additionally, reports from 1577 mention three native bees \
used by indigenous people in Brazil.

 [INFO] ==> The code for model: llama3.2:3b, took 22.7s to \
 generate the answer.
```

顺便说一句，如果使用的模型支持多种语言，我们可以使用它（例如，葡萄牙语），即使数据集是用英语创建的：

```py
prompt = "Existem abelhas no Brazil?"
rag_bees(prompt)
```

```py
Sim, existem abelhas no Brasil! De acordo com o relato de Hans \
Staden, há três espécies de abelhas nativas do Brasil que \
foram mencionadas: mandaçaia (Melipona quadrifasciata), \
mandaguari (Scaptotrigona postica) e jataí-amarela \
(Tetragonisca angustula). Além disso, o Brasil é conhecido \
por ter mais de 300 espécies diferentes de abelhas, a \
maioria das quais não é agressiva e não põe veneno.

 [INFO] ==> The code for model: llama3.2:3b, took 54.6s to \
            generate the answer.
```

### 进一步探索

测试的小型 LLM 模型在边缘上运行良好，无论是文本还是图像，但当然，在最后一个方面有很高的延迟。结合特定和专用模型可以带来更好的结果；例如，在实际案例中，一个目标检测模型（如 YOLO）可以对图像中的对象进行一般描述和计数，一旦传递给 LLM，就可以帮助提取关键见解和行动。

根据 Hailo 的首席技术官 Avi Baum，

> 在人工智能（AI）的广阔领域中，最引人入胜的旅程之一就是边缘 AI 的演变。这一旅程带我们从经典的机器视觉领域走向了判别性 AI、增强性 AI，现在，是突破性的生成 AI 前沿。每一步都让我们更接近一个未来，在那里智能系统无缝地融入我们的日常生活，提供不仅在感知上而且在手掌中也能进行创造的沉浸式体验。

![](img/file940.jpg)

## 摘要

本实验室展示了如何将 Raspberry Pi 5 转变为一个强大的 AI 中心，能够运行大型语言模型（LLMs），利用 Ollama 和 Python 进行实时、现场数据分析和洞察。Raspberry Pi 的多功能和强大性能，加上轻量级 LLMs 如 Llama 3.2 和 LLaVa-Phi-3-mini 的能力，使其成为边缘计算应用的优秀平台。

在边缘运行 LLMs 的潜力远远超出了简单的数据处理，正如本实验室的示例所示。以下是一些创新性的使用这个项目的建议：

**1. 智能家居自动化**:

+   集成 SLMs 以解释语音命令或分析传感器数据以实现智能家居自动化。这可能包括对家庭设备、安全系统和能源管理的实时监控和控制，所有这些处理都在本地进行，不依赖云服务。

**2. 场地数据收集和分析**：

+   在远程或移动设置中在 Raspberry Pi 上部署 SLMs 进行实时数据收集和分析。这可以用于农业监测作物健康，在环境研究中进行野生动物追踪，或在灾害响应中进行态势感知和资源管理。

**3. 教育工具**:

+   创建利用 SLMs 提供即时反馈、语言翻译和辅导的交互式教育工具。这在技术先进性和互联网连接有限的发达地区尤其有用。

**4. 医疗保健应用**:

+   使用 SLMs 进行医疗诊断和患者监测。它们可以提供症状的实时分析并建议潜在的治疗方法。这可以集成到远程医疗平台或便携式健康设备中。

**5. 本地商业智能**:

+   在零售或小型商业环境中实施 SLMs 以分析客户行为、管理库存和优化运营。本地处理数据的能力确保了隐私并减少了对外部服务的依赖。

**6. 工业物联网**:

+   将 SLMs 集成到工业物联网系统中，用于预测性维护、质量控制和工作流程优化。树莓派可以作为本地数据处理单元，减少延迟并提高自动化系统的可靠性。

**7. 自主车辆**:

+   使用 SLMs 处理来自自主车辆的感官数据，实现实时决策和导航。这可以应用于无人机、机器人和自动驾驶汽车，以增强自主性和安全性。

**8. 文化遗产和旅游**:

+   实施 SLMs 以提供互动和富有信息量的文化遗产遗址和博物馆导游。游客可以使用这些系统获取实时信息和见解，增强他们的体验，无需互联网连接。

**9. 艺术和创意项目**:

+   使用 SLMs 分析和生成创意内容，如音乐、艺术和文学。这可以促进创意产业中的创新项目，并在展览和表演中提供独特的互动体验。

**10. 定制辅助技术**:

+   为残疾人士开发辅助技术，通过实时文本到语音、语言翻译和其他可访问工具提供个性化自适应支持。

## 资源

+   [10-Ollama_Python_Library 笔记本](https://github.com/Mjrovai/EdgeML-with-Raspberry-Pi/blob/main/OLLAMA_SLMs/10-Ollama_Python_Library.ipynb)

+   [20-Ollama_Function_Calling 笔记本](https://github.com/Mjrovai/EdgeML-with-Raspberry-Pi/blob/main/OLLAMA_SLMs/20-Ollama_Function_Calling.ipynb)

+   [30-Function_Calling_with_images 笔记本](https://github.com/Mjrovai/EdgeML-with-Raspberry-Pi/blob/main/OLLAMA_SLMs/30-Function_Calling_with_images.ipynb)

+   [40-RAG-simple-bee 笔记本](https://github.com/Mjrovai/EdgeML-with-Raspberry-Pi/blob/main/OLLAMA_SLMs/40-RAG-simple-bee.ipynb)

+   [calc_distance_image python 脚本](https://github.com/Mjrovai/EdgeML-with-Raspberry-Pi/blob/main/OLLAMA_SLMs/calc_distance_image.py)
