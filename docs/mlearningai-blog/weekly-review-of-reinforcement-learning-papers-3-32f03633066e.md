# 强化学习论文#3 的每周回顾

> 原文：<https://medium.com/mlearning-ai/weekly-review-of-reinforcement-learning-papers-3-32f03633066e?source=collection_archive---------5----------------------->

## 每周一，我都会发表我研究领域的 4 篇论文。大家来讨论一下吧！

![](img/098ccf672027ab44fa1cf7c02019ade6.png)

[ [←上一次回顾](/mlearning-ai/weekly-review-of-reinforcement-learning-papers-2-649e96b00c66?source=friends_link&sk=6b89f7a8194780375e880891082cce51) ][ [下一次回顾→](https://qgallouedec.medium.com/weekly-review-of-reinforcement-learning-papers-4-d735531f629c?source=friends_link&sk=53ee1d401bfcb4fe3d7ee642bd1decc9)

# 论文 1:长期信用转让的综合回报

Raposo，d .，Ritter，s .，桑托罗，a .，Wayne，g .，Weber，t .，Botvinick，m .，van Hasselt H .和 Song，F. (2021)。[长期债权转让综合收益](https://arxiv.org/abs/2102.12425)。 *arXiv 预印本 arXiv:2102.12425* 。

良好的行为会产生高回报。此外，因果律告诉我们，原因总是先于结果。综上所述:一个好的行动与未来的高回报联系在一起。一个逻辑学家会回答:倒数是真的吗？高回报是否意味着所有之前的行为都是好的？不。所有的 RL 算法都是基于这个错误的断言。举一个基于策略的 RL 的例子:代理增加了之前动作的概率(不是原因！)高奖励。

本文中介绍的方法与您通常看到的截然不同:*状态关联*学习(SA)。代理人必须了解状态和任意遥远的未来奖励之间的联系。**目标是模拟过去状态对当前奖励的贡献**。进入一点算法细节，代理增加了一个神经网络，它被训练来预测每个时间步骤的回报。这个神经网络将事件开始以来的访问状态集作为输入。神经网络的输出称为“*综合回报*”。直观地说，这种神经网络允许我们测量可归因于代理在当前状态下的存在的奖励，而不管需要多长时间才能达到奖励。在强化学习过程中，代理将最大化这个数量。

有用吗？是的。这里是 Deep-RL 解决的最后一个雅达利游戏之一，因为它的延迟奖励结构:雅达利滑雪。

![](img/50e1fab72cdfc4b3c473c0a6aae118ad.png)

Atari Skiing. [Image source](http://www.8-bitcentral.com/reviews/2600skiing.html).

正是在这种环境下，他们的算法给出了最好的结果。他们能够用比文献中最好的代理 57 少 25 倍的交互来解决它。

![](img/b904ac75aabada158cf6504815733305.png)

Figure from the paper: Agent (IMPALA+SR) performance on *Atari Skiing*. The agent is IMPALA-based and augmented with the syntetic reward (SR).

我很想看看在其他环境中使用这个代理的工作。正是这种范式的转变带来了巨大的进步。

# 论文 2:医学影像中的深度强化学习:文献综述

周树国、乐海宁、吕国光、阮海伟、吴亚奇(2021)。[医学成像中的深度强化学习:文献综述](https://arxiv.org/abs/2103.05115)。 *arXiv 预印本 arXiv:2103.05115* 。

如果我们抬起眼睛，我们可以看到我们领域的最新进展在医学上惠及了我们的邻居。这仍然是边缘性的，但是将医学和 Deep-RL 联系起来的论文正在成倍增加。最近这份出版物的作者提议对他们进行调查。

Deep-RL 的主要应用(奇怪的是)是医学成像。区分三类:
(一)医学图像的参数分析
(二)优化任务的解析
(三)其他。让我们为第一类给出一个简短的有代表性的例子。

Maicas 等人(2017)提出了基于 DRL 的方法，用于通过对比增强动态磁共振成像(DCE-MRI)进行乳腺病变检测。这包括训练一个必须连续选择如何进化其边界框的大小和位置的代理。从概念上讲，一个动作可以是“*向右移动 20 个像素，并将边界框的尺寸减小 7 个像素*”。观察空间是边界框的内容和奖励:如果病变在框内。

![](img/dece8ea4b1ae3e2b7a142b186cfc7048.png)

使用基于 ResNet 的 DQN，结果显示与最先进的方法相似的准确性，但检测时间显著减少。

本文中还有许多其他有趣的例子。我很高兴看到这种跨学科方法的结果。

# 论文 3:通过存储嵌入提高视觉强化学习的计算效率

Chen，l .，Lee，k .，Srinivas，a .，，Abbeel，P. (2021)。[通过存储嵌入提高视觉强化学习的计算效率](https://arxiv.org/abs/2103.02886)。 *arXiv 预印本 arXiv:2103.02886* 。

我们经常从简单的例子开始学习强化学习:在这些简单的例子中，一个观察=状态；观测向量中的所有坐标都是有用的，所有有用的东西都在观测向量中。但是生活中的事情并没有那么简单。当我们进行基于图像的学习时，并不是所有的像素都有用，也不是所有有用的东西都在像素中。这就是高维观察空间的挑战。

非策略强化学习算法通常使用内存来回放情节。这是一个在实践中非常有效的技巧。然而，当观察是图像时重放情节是非常存储和计算昂贵的。

在这篇论文中，作者提出了一个非常有吸引力的想法:他们不是将原始图像存储在重放记忆中，而是存储潜在的表征。存储数据的维度要低得多。因此减少了计算量。然而，一个问题仍然存在:如何产生这种潜在的表现？解决方案是通过一起学习策略和表示来开始学习。一旦表征被正确学习，我们就可以冻结它，**只在潜在表征**上继续学习。

![](img/efd76503fdcb45c7c060cf57242f8811.png)

Figure from the paper: (a) All forward and backward passes are active through the network. Replay buffer stores images. (b) Encoder is frozen, replay buffer stores latent vectors. (Published with the author’s permission.)

SEER 代表有效强化学习的存储嵌入。这是结果。

![](img/5d515d2811dd9f965421749cc8c342ee.png)

Figure from the article: Learning curves for Rainbow with and without SEER, where the x-axis shows estimated cumulative FLOPs. (Published with the author’s permission.)

您会注意到，x 轴给出了估计的失败次数。这很不寻常，但是请记住，这里的目标是减少算法的计算需求。从冻结开始，模型学习效率更高(就操作次数而言)。赌赢了！

我不能在这里展开致力于学习过程的转移和 CNN 下层的概括的部分。我邀请你完整地阅读这篇论文。

# 论文 4:基于音频应用的深度强化学习综述

Latif，s .，Cuayáhuitl，h .，Pervez，f .，Shamshad，f .，Ali，H. S .，和 Cambria，E. (2021 年)。[针对音频应用的深度强化学习调查](https://arxiv.org/abs/2101.00240)。 *arXiv 预印本 arXiv:2101.00240* 。

在其众多应用之一中，Deep-RL 用于音频系统:从声音、语音、音乐……任何包含信息的声音信号中学习。但是音频是一种非常特殊的输入类型。当我们研究信号的演变、时间和频谱维度时，它就有了意义。我们习惯于 CNN 捕捉图像的空间连续性，音频应用仍然比较边缘化。

本文综述了针对音频应用的全系列 Deep-RL 论文。评论很详细。有几个大的表格，允许有一个非常综合的看法，所有的方法，目前在文学和他们的特殊性。比较应用领域、使用的算法、结果、观察空间、奖励函数的形式是非常容易的

![](img/93cb80942bcaa450fafda87e584720ff.png)

Figure from the paper: summary of audio-based DRL connecting the application areas and algorithms (red: value-based, green: policy-based, blue: model-based)

作者确定了 5 种类型的应用:自动语音识别(ASR)、语音情感识别(SER)、口语对话系统(SDSs)、音频增强、音频驱动的机器人控制和音乐生成。对于这些类别中的每一个，都有越来越多的研究，并且每次都展示了 Deep-RL 的威力。然而，作者发现了一个陷阱:这些结果通常是在高度受控或模拟的环境中获得的。这并没有反映现实世界的复杂性，现实世界非常嘈杂，不稳定，而且肯定充满了其他偏见，使学习变得不那么容易。未来的研究肯定会解决这个在机器人领域已经众所周知的问题:模拟和现实之间的差距。

毫无疑问，我们将继续观察 Deep-RL 应用于音频的惊人应用。

# 奖励论文:强化学习:导论

萨顿和巴尔托(2018 年)。 [*强化学习:简介*。](https://books.google.com/books?hl=fr&lr=&id=uWV0DwAAQBAJ&oi=fnd&pg=PR7&dq=reinforcement+learning&ots=mioLp5XZg3&sig=BHjwIZgRw_nAOWYKJvxSNW_-K7c)麻省理工学院出版社。

本周有个小小的例外，因为我展示的不是一篇文章，而是一本书。这是一本每个对 Deep-RL 感兴趣的人都应该拥有的书。

![](img/4df820daeb2143eca5fc523b96eceaa5.png)

这本书由理查德·萨顿和安德鲁·g·巴尔托于 2018 年出版，一步一步地解释了强化学习的基础。这本书被广泛引用，因为它在时间上冻结了 2018 年已知的关于强化学习的一切。这是一本高质量的书，研究充分，插图精美，涵盖了所有的难度，对初学者和专家都一样。

![](img/f13f45504e2a6c0cfa78290c2ac4423d.png)

这本书以致力于表格学习的一章开始(行动和观察空间是离散的和有限的)。然后下一章重点讲基于深度神经网络的近似解(我们去 Deep-RL)。它以致力于学科的前沿的一些部分结束:他们讨论可以发现与神经科学和心理学的相似性。

我决定买纸质版的，但是在[网站](http://incompleteideas.net/book/the-book.html)之后有免费版。

我很高兴向你们展示我本周的阅读材料。请随时向我发送您的反馈。