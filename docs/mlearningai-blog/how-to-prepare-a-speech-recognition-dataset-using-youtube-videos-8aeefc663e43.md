# 如何使用 YouTube 视频准备语音识别数据集？

> 原文：<https://medium.com/mlearning-ai/how-to-prepare-a-speech-recognition-dataset-using-youtube-videos-8aeefc663e43?source=collection_archive---------1----------------------->

![](img/47815756fe4a34f0676f998a8602a7d4.png)

Photo by [NordWood Themes](https://unsplash.com/@nordwood?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

当使用一种不常见的语言时，训练自己的语音识别模型可能会变得特别忙碌，这正是我最近在一个需要乌尔都语语音识别器的项目中遇到的情况。

经过一些研究，我发现很少有开源的乌尔都语数据集，而且质量非常低。即使我使用了它们，开源数据集也很少能满足您特定项目的需求。这就是为什么在构建人工智能系统时，针对特定领域使用自己的数据集总是一个好主意。

但是你能在哪里找到大量你喜欢的语言的口语数据呢？就在那时，我的一个同事向我推荐了 YouTube。你可以在那里找到各种视频；新闻、课程、脱口秀、博客，应有尽有。但将这些视频转换成训练数据是真正的任务，因为大多数 YouTube 视频都有背景音乐或大量噪音。

# 我们在做什么？

在这篇文章中，我将带您了解准备 YouTube 视频的过程，这些视频将用于训练您的 ASR。为此，我使用了 Python 3 和一些工具和库。我在整个过程中使用了 [Google Colab](https://colab.research.google.com/) ,因为它使设置变得非常快速和容易。正如你已经猜到的，我将为乌尔都语准备一个数据集，但是你也可以使用本教程为任何语言准备一个数据集。你所需要的只是一个 YouTube 链接列表！

# 我们如何做到这一点？

首先，我们需要安装我所说的工具和库。我将在接下来的小节中讨论对这些库的需求。让我们使用 pip 安装它们:

```
!pip install spleeter
!pip install youtube_dl
!pip install pydub
```

您需要为整个程序执行以下导入操作:

```
import spleeter
from __future__ import unicode_literals
import youtube_dl
from pydub import AudioSegment
from pydub.silence import split_on_silence
```

安装并导入依赖项后，我们需要对列表中的每个视频执行以下步骤:

1.  提取并下载音频
2.  将语音从音频中分离出来
3.  调整采样率、样本宽度和通道
4.  分成更小的块
5.  转录音频

所以让我们开始吧。

## 提取并下载音频

我们将使用 [youtube-dl](https://github.com/ytdl-org/youtube-dl) 从给定的 youtube 链接下载音频。它是一个从 YouTube 和其他视频平台下载视频的命令行程序。下面是一个简单的代码，它将音频下载为“audio.wav”。

```
ydl_opts = {
    "format": "bestaudio/best", 
    "audio-format": "wav",
    "outtmpl": "audio.wav",        
}
try:
    with youtube_dl.YoutubeDL(ydl_opts) as ydl:
        ydl.download([link])
except Exception as e:
    print("Video cannot be downloaded. Exception:/n{}".format(e))
```

对象`ydl_opts`用于根据自己的需求定制下载。在这里，我们告诉 youtube_dl 下载 WAV 格式的最佳可用质量音频，并将其保存为“audio.wav”。

由于某些视频在您所在的地区不可下载，或者下载音频可能会因任何其他原因而失败，因此我们添加了 try-except 块。您可以将第 8 行中的“link”替换为您希望在数据中使用的任何 YouTube 视频链接。

## 将语音从音频中分离出来

这是整个过程中最重要的一步，因为它将决定音频数据的质量，从而决定我们使用它训练的 ASR 的质量。有很多工具可以用来分离音频中的人声，但我发现的最好的开源工具是 Deezer 的[Spleeter](https://research.deezer.com/projects/spleeter.html)。

Spleeter 是一个用 Python 编写的源分离库，它提供了不同的预训练模型来分离歌曲中的音轨。您也可以使用库来训练您自己的模型。我们将使用 Spleeter 的 2 项模型来将声音从音频中的所有其他声音中分离出来。这将允许我们从视频中只提取语音数据来满足我们的数据需求。

为此使用了一个单行命令:

```
!spleeter separate -p spleeter:2stems -o output "/content/audio.wav"
```

这将在“/output/audio”文件夹中为您提供两个名为“vocals.wav”和“con 伴奏. wav”的 WAV 文件。很明显，我们将使用 vocals.wav。

## 调整采样率、样本宽度和通道

语音识别数据集中的音频具有以下特征是一个标准:

采样率= 16000 kHz
样本宽度=每个样本 16 位
通道=单声道(1)

我们将使用 [Pydub](https://github.com/jiaaro/pydub) 库来调整这些参数，并用修改后的文件替换原来的音频文件。

```
sound = AudioSegment.from_file("/content/output/audio/vocals.wav")
sound = sound.set_frame_rate(16000)
sound = sound.set_channels(1)
sound = sound.set_sample_width(2)
sound.export("/content/output/audio/vocals.wav", format ="wav")
```

## 分成更小的块

与大多数开源数据集一样，在语音识别数据集中拥有长度为 15 到 30 秒的音频是一个很好的实践。我们可以将音频分成相等的块，但我发现对于 YouTube 视频来说，更好的方法是在静音上分割。这样，我们就不会在音频的开头或结尾出现不完整的单词。

我们将再次使用 Pydub 来分割无声的音频。

```
sound_file = AudioSegment.from_wav("/content/output/audio/vocals.wav")
audio_chunks = split_on_silence(sound_file, min_silence_len=500, silence_thresh=-50)
for chunk_num, chunk in enumerate(audio_chunks):
    out_file = "{chunk_num}.wav".format(i)
    chunk.export(out_file, format="wav")
```

上面的代码读取“vocals.wav ”,并使用函数`pydub.silence.split_on_silence`来分割静音上的音频。for 循环导出所有的块，并将它们命名为`0.wav`、`1.wav`、`2.wav`等等。

我们可以通过调整函数的参数来控制分裂。`min_silence_len`定义无声补丁需要多长时间(以毫秒为单位)才能被考虑拆分，而`silence_thresh`定义无声补丁的最大响度限制(以 dBFS 为单位),超过该限制将不会被考虑拆分。我发现半秒(500 毫秒)的最小静音长度对大多数音频来说都很好，但是你应该根据你的具体音频来调整静音阈值，以获得最佳效果。

## 转录音频

既然我们已经准备好了音频，是时候生成相应的文本来完成我们的数据集了。为了生成高质量的数据，您必须手动完成这项工作。难过，我知道！😢

尽管如此，我还是有一些建议可以让这个过程变得简单一些:

1.  如果你自己做转录，使用语音到文本的应用程序是一个好主意，如果有的话。例如，[谷歌语音打字](https://support.google.com/docs/answer/4492226?hl=en#zippy=%2Clanguages-that-work-with-voice-typing)支持多种语言，可以很容易地在谷歌文档上使用。您可以对着麦克风说出句子，然后将文本粘贴到您需要的地方。它仍然需要一些完善的编辑，但这种技术可以大大减少手动输入文本所需的时间。
2.  但是，如果您没有预算限制，您可以通过使用语音到文本 API 并将文本直接保存为您喜欢的格式，使这个过程自动化，不那么忙乱。同样， [Google Speech](https://cloud.google.com/speech-to-text) 拥有大量受支持的语言、丰富的文档和现收现付的定价计划，可以帮助您完成这项任务。在你使用 API 生成了 TXT 文件或任何你需要的文本格式后，你可以听音频并自己纠正错误。

正如您所看到的，这些技术可以显著减少工作量，但您仍然需要进行编辑来产生高质量的数据集，然后可以使用该数据集来训练高质量的语音识别系统。

# 链接列表的自动化

必须对您在开始时准备的列表中的所有 YouTube 视频执行步骤 1 至 4。你可以在这里找到带有完整代码的 [Colab 笔记本，它可以自动完成上面讨论的过程。](https://github.com/TehreemFarooqi/Preparing-a-speech-recognition-dataset-using-YouTube-videos)

它将一个 TXT 文件作为**输入**，该文件中的链接由一个新行分隔开(意味着每个链接在一个单独的行上)。

笔记本**为`link number 1, link number 2, link number 3…`输出名为`1, 2, 3…`的**文件夹，在这些文件夹中，组块被命名为`{link_number}_{chunk_number}`。所以文件夹`1`会有文件`1_0.wav`、`1_1.wav`、`1_2.wav`等等。

我建议你[安装 Google Drive](https://colab.research.google.com/notebooks/io.ipynb#scrollTo=u22w3BFiOveA) 并将输出直接保存在你的驱动器中，这样可以节省从 Colab 下载文件的时间。如果 Colab 由于某种原因断开连接，它也将帮助您保存您的进度。

这是一个关于如何利用 YouTube 视频为任何 ASR 或相关系统准备语音识别数据集的详细教程。如果这对你有所帮助，请为我鼓掌！

对于任何与数据科学或机器学习相关的问题或疑问，请随时通过我的电子邮件联系我:【teefarooqi@gmail.com】T2 或 [LinkedIn](https://www.linkedin.com/in/tehreemfarooqi/) 。