# 巴克 65 的词汇，量化

> 原文：<https://medium.com/mlearning-ai/the-vocabulary-of-buck-65-quantified-62c188f2d669?source=collection_archive---------2----------------------->

# 介绍

自从我在高中第一次听到他的歌曲[邪恶和怪异](https://www.youtube.com/watch?v=YjxfF7T97z0)和[以来，65 岁的巴克一直是我最喜欢的说唱歌手，也是我最喜欢的任何流派的艺术家之一。这就是为什么当我在大学里第一次看到](https://www.youtube.com/watch?v=XkQSvtcUhWI) [Matt Daniels 的说唱歌手词汇排行榜](https://pudding.cool/projects/vocabulary/index.html)时，我做的第一件事就是去找他。他不在名单上并不奇怪——巴克并不完全是一个家喻户晓的名字——但这确实让我想知道他在排行榜上的位置。

![](img/500b60885cc825b9b69d897771f7eb15.png)

Buck 65 Live — Daniel Jordahl from Östersund, Sweden, [CC BY-SA 2.0](http://Daniel Jordahl from Östersund, Sweden, CC BY-SA 2.0 <https://creativecommons.org/licenses/by-sa/2.0>, via Wikimedia Commons), via Wikimedia Commons

从那以后的几年里，我偶尔会看到图表的更新版本弹出来。我总是检查它，看看巴克是否在名单上，但我每次都很失望。这似乎也是一种真正的耻辱:我有一种感觉，他会在排行榜上名列前茅，歌词是这样的[“我觉得自己像一只水母，没有头脑，不文明/不具体，不专业/水流带着我，我自己的忍耐力埋葬了我/威慑使我疲惫，所以我戴上这个戒指以寻求安慰”](https://www.youtube.com/watch?v=cdaE1YDWNuM)。我想，如果有人能推翻伊索摇滚(说唱中最大的词汇)，那可能就是他。

最近，我发现自己又听了一堆 Buck 65，这让我想起了他在 Daniels 图表中的位置。当我第一次看到这张图表时，我不知道如何找到这个问题的答案。然而，在这中间的几年里，我学到了很多用代码解决这类问题的方法，所以我开始努力解决它。

# 获取数据

![](img/4515d06d9803253f52711a7d7b7dd7bd.png)

Not pictured: how I got the lyric data - Photo by [CDMA](https://unsplash.com/@barebonesfotoes?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我的第一步是获得足够的歌词来分析。坚持丹尼尔斯的原始方法将需要至少 35，000 个单词。粗略地看了一下他在 Genius 上的条目，似乎就能扫清这个门槛，几个谷歌搜索就让我找到了一篇很棒的文章和一个非常有用的 Python 包，这让我很容易地收集到了我需要的数据。如果你想按照我以前做的代码去做，你可以在这里找到库，还有我的 Jupyter 笔记本的 HTML 版本[这里](https://www.logan-cooper.com/Buck-65-Lyrics/)。

我最终需要一些清洁和修整，但这相对容易。我去掉了歌词中所有的换行符和特殊字符(谢谢正则表达式)，从‘album’列中输入的杂乱的 JSON 字符串中提取出专辑名。

我也删除了一些歌词。你看，丹尼尔斯试图只使用 E.P.s 和录音室专辑，这让我有很多歌曲要过滤掉。其中大部分属于混音带和未发行的材料，但也有一些特例。我不得不从数据集中删除两张录音室专辑:*怪人磁铁*和*这是 Buck 65。*前者只有一首关于天才的歌，后来的一张专辑里重新录制了这首歌。后者是一个最佳专辑，这意味着除了一两首歌曲外，所有的歌曲都已被收录。我看不出转述歌曲有多大价值，就把它们拿出来了。

# 处理数据

![](img/cb02844c4461909c8cf79490128eb6f1.png)

Kind of pictured: me crunching the lyrics - Photo by [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

清理和整理数据的过程给我留下了总共 43，291 首歌词。有了所有可用的歌曲，我将所有的歌曲连接成一个巨大的字符串，用自然语言工具包将其标记为单词，然后将结果列表转换为一个集合以过滤掉重复的内容——留给我一个独特单词的列表。在所有这些歌词中，我发现了 7521 个独特的词。然而，为了与丹尼尔斯的图表进行比较，我需要将样本缩减到巴克的前 35，000 首歌词。对于 65 岁的书呆子们来说，这可以翻译成专辑*语言艺术*、*顶点*、*落水人员*、*联觉*、*广场*、*谈及‘红蓝调’*、*对抗世界的秘密房子*以及大多数*情况。*

缩小样本范围后，我得到了 6557 个唯一单词的结果。这使得巴克在名单上排在第三位，领先于 6424 个单词的《绝地心灵技巧》,但仍远远落后于《巴士司机》和《伊索摇滚》,两者都有 7000 个单词。事实上，伊索洛克在他的前 35，000 个单词中使用的单词比巴克 65 在他的整个作品中使用的单词还多。

这个由来已久的问题有了答案，我决定用这个例子来玩一玩。使用他最后的 35，000 个单词，我们得到 6，537 个独特的单词，这意味着随着时间的推移，词汇量略有减少。使用一系列 35，000 个单词的随机抽样，我们得到的结果趋向于平均在 6，600 以上，但达到 6，590 以下和 6，700 以下。

就在那时，我决定添加更多的专辑。你看，65 岁的巴克发行了两张名为 [Bike for Three](https://bikeforthree.bandcamp.com/) 的专辑，作为他与比利时 DJ [Greetings from Tuskan](https://soundcloud.com/greetingsfromtuskan) 合作的一部分。因为所有的歌词都是巴克的，所以我决定把这些专辑放到我的数据集中。我期待它们以独特的歌词出现，因为在我看来，这些专辑是他最有诗意的一些。

独特歌词的总数上升到 8197 个(从增加的 51739 个样本，所以这并不奇怪)。每个样本的变化大多是轻微的，但仍然令人惊讶。前 35，000 个样本变化不大:从 6，557 个增加到 6，554 个。三张专辑的 *Bike 都是后来的专辑，所以这个样本只稍有改动是有道理的。最后的 35，000 个样本从 6，537 个一直下降到 6，436 个，这很令人惊讶。就像我说的，我预计会增加，但正如一位粉丝指出的那样:这些专辑严重依赖重复作为一种修辞手段，这将降低独特歌词占总歌词的比例。随机样本倾向于停留在同一个地方:最高 6600。*

# 最后的想法

![](img/32e90f9e6273b6bacdb95d51de1f2297.png)

Pictured: What was going on in the background while I was doing this — Photo by [Adrian Korte](https://unsplash.com/@adkorte?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我有一种感觉，巴克 65 将在排名上，所以第三名并不奇怪。我有一部分认为他会更接近伊索摇滚和巴士司机(甚至超过他们)，但这主要是一厢情愿的想法。尽管巴克使用了许多独特的词汇，他的歌词却不像《伊索寓言》那样快或密。如果你想要一个明确的例子，我会指引你到 [B .多兰的*越狱*](https://www.youtube.com/watch?v=f71ToeWClQU) ，这是我知道的唯一一首既有伊索摇滚又有巴克 65 的歌。

如果能找到并包含更多的*怪人磁铁*就好了，因为这是我在任何类型的专辑中唯一一张听到过[、【腔棘鱼】](https://en.wikipedia.org/wiki/Coelacanth)这个词的专辑，但我看不出它有什么不同。要超过 Busdriver 的 767 个单词，除了“腔棘鱼”，还需要有很多独特的单词，我不认为有那么多。

这个问题终于有了答案，我不知道还有什么可说的。对于一个来自新斯科舍农村的人来说，第三名已经很不错了。现在，去听一些巴克 65，他相当不错。