# 从零开始建立神经网络动物园:长短期记忆网络

> 原文：<https://medium.com/mlearning-ai/building-a-neural-network-zoo-from-scratch-the-long-short-term-memory-network-1cec5cf31b7?source=collection_archive---------3----------------------->

![](img/cdf6855050fdbe4cdcd8b4a6e9217ded.png)

Visualization of the Long Short-Term Memory Network from [Asimov Institute](https://www.asimovinstitute.org/neural-network-zoo/).

长短期记忆(LSTM)网络是最著名的递归神经网络类型之一。最初由 Jürgen Schmidhuber 和 Sepp Hochreiter 提出，目的是通过解决[消失梯度问题](https://www.analyticsvidhya.com/blog/2021/06/the-challenge-of-vanishing-exploding-gradients-in-deep-neural-networks/)来学习数据的长期依赖性，LSTMs 此后一直是许多重要系统的开发工具，其中最重要的是苹果的 Siri。

## Vanilla RNNs v.s. LSTMs

普通 RNNs 和 LSTMs 之间有两个主要区别:门的数量和存储单元的数量。

![](img/0b9799d5220aada961acd882d384e0dd.png)

Vanilla RNN Diagram v.s. LSTM Diagram

正如我在[的上一篇文章](/mlearning-ai/building-a-neural-network-zoo-from-scratch-the-recurrent-neural-network-9357b43e113c)中解释的，普通的 rnn 有一个存储单元，称为隐藏状态(在上图中表示为 HS)。隐藏状态用于保留过去的信息，这也是 RNN 区别于任何其他神经网络的原因。由于 LSTMs 也是一种递归神经网络，它们也有一个隐藏状态，但它们有另一个称为单元状态的存储单元(在上图中表示为 CS)。这种新型记忆的目的是学习长期模式，而隐藏状态通常保留更多的短期信息。为了更好地理解这是为什么，首先理解 LSTM 引入的新门是很重要的。

## 遗忘之门

遗忘之门的目的正如其名:遗忘。就像我们自己的记忆一样，LSTM 网络只能记住这么多。为了决定哪些信息要记住，哪些要忘记，遗忘门使用 sigmoid 函数。

![](img/767dfcb9db054c21a360265f3d1e58c0.png)

Forget Gate Definition

遗忘门的定义如上。它是权重矩阵的 sigmoid(*W _ f*)乘以前一个隐藏状态和当前输入的级联(Z *_t* )，加上 bias ( *b_f* )。

![](img/e3c8b7bc2c5e91a415f7ef9a23d79fbd.png)

如果你还记得的话，sigmoid 函数挤压 0 到 1 之间的所有数字。这就是为什么当遗忘门乘以细胞状态时，它会“选择”要记忆的信息。在上图中，单元状态中任何被认为不值得保留的数据将被乘以一个最接近零的数字，而任何被认为重要的内存将被乘以一个最接近 1 的数字。

对于那些了解线性代数的人来说，你可能会注意到这不是一个标准的矩阵乘法，你可能是对的:这是一个[哈达玛乘积](https://en.wikipedia.org/wiki/Hadamard_product_(matrices))。这对网络来说并不特别重要，但值得注意的是，单元状态中的每个单独的“内存”都有自己的重要性级别。

## 候选门

候选门是应用于显示给网络的新信息的激活层。

![](img/862ae55a7e634b99775ece7b90c44221.png)

Candidate Gate Definition

正如你在上面看到的，候选门的定义类似于遗忘门，除了它有自己的权重和偏差( *W_c* 和 *b_c* )，并且应用了双曲正切函数而不是 sigmoid。

## 输入门

像遗忘门一样，输入门决定在网络的时间步长之间保留多少信息。区别在于，输入门决定要记住哪些进入网络的信息，而不是要记住多少来自存储器的信息。

![](img/05a5b36d69f8b40f71f3268b0d3347f8.png)

Input Gate Definition

如上所示，输入门的定义与遗忘门相同，当然，除了它有自己的权重和偏置( *W_i* 和 *b_i* )。因此，当输入门乘以候选门时，它与当遗忘门乘以单元状态时具有相同的效果:它通过将重要的事情乘以最接近 1 的数字，将不重要的事情乘以最接近 0 的数字来“选择”要记住的信息。

## 输出门

与其名称相反，输出门是网络输出之前的最后一个门*而不是*。事实上，它是用于确定隐藏状态之间记住什么信息的门。

![](img/d910132fef592c55d0e691132ef6c30f.png)

Output Gate Definition

输出门如上定义。它是偏差之和(b_o)与权重和级联输入(分别为 W_o 和 Z_t)之积的 sigmoid。

## 最后的大门

与输出门不同，末级门顾名思义:末级门。在网络返回其最终答案之前，它会通过最终的门，其定义如下:

![](img/fb7a451f1afb5b27f35316cfac4b793b.png)

The final gate definition.

由于在 LSTM 网络中有许多激活功能，所以没有必要在最后一个门上使用激活功能。然而，应用激活函数不会损害网络，只要你记得将激活函数添加到反向传播计算中。

## 简要回顾

细胞状态是网络的长期记忆，而隐藏状态是短期的。遗忘门、输入门和输出门都用于选择哪些信息重要到需要记住，这是使用 sigmoid 函数完成的。候选门保存来自输入和先前隐藏状态的信息，该信息被传递给网络的未来版本，最终门被应用于网络的输出。

## 数学

如果你从他开始就一直关注这个系列，你就知道我要说什么:理解 LSTMs 背后的数学最简单的方法是使用计算图(CGs)。然而，LSTM 比我们之前看到的网络要大得多，这意味着必须有所改变。

![](img/fe6dda25839f4713f245d191e2d912d1.png)

Diagram showing the construction of a computational graph.

如果你对 CGs 不熟悉，我建议你看一下我以前的文章，那里有更深入的解释。现在，我给你一个快速复习。图中的每个节点(或圆)代表一个操作，每个边(或线)代表一个公式。前向传播可以从左到右在每个边缘上方以绿色读取。反向传播可以从右到左在每条边下面以红色读取，计算如下:第一个误差由一个 *e* 表示，其余的是右边的误差乘以右边的函数相对于上面的函数的导数。

如果我们要构建一个完整的 LSTM CG，它会非常大，以至于实际的计算难以辨认。出于这个原因，有一个简单的方法来缩小 CG:分而治之。

在网络的每个时间步长 *t* ，依次进行以下计算。

![](img/8d2b4d95e41ac8ab94c127596e074cb9.png)

Forget Gate Definition

![](img/a9b74ac899d3be0ce37ad9e62a3969ef.png)

Input Gate Definition

![](img/9930204ad170d67624ad6e3fdaea5ff4.png)

Candidate Gate Definition

![](img/fb88146deddcba679133b78a4bd91497.png)

Output Gate Definition

![](img/2261ef0aa7de62d0eac7df4b09eaa2ee.png)

Cell State Definition

![](img/e368a1b829d455458859d68b4fa7c4ab.png)

Hidden State Definition

![](img/3121e8e81f00cf2c9d7f193c103cfe1e.png)

Final Gate Definition

和反向传播一样，我们从后面开始，一路向前。

![](img/c9ac640e924e2fe37a5bf88642ceedfc.png)

Final Gate CG.

因此，我们发现最终门的权重和偏差的误差如下。

![](img/b1115d23f8e22dcc1f65aa5c7f1e58c7.png)

Error of the Final Gate Weights.

![](img/0fe7ffb6aa560de390026ca6c94a5a0e.png)

Error of the Final Gate Biases.

接下来，我们可以建立一个隐藏状态定义的 CG。

![](img/887031d4bfef80c6b736b87c474c567a.png)

Hidden State CG.

所以我们发现了隐藏状态的错误…但是什么是 *e* ？对于最后一个门， *e* 将是我们计算的输入和标签之间的误差，但是对于隐藏状态来说就不一样了，不是吗？这个 *e* 会不一样，但是我们其实已经计算过了。我们发现了最后一扇门 CG 中隐藏状态的错误。

![](img/fd610f3826a01e359d0b0f9e8c874664.png)

Error of the Hidden State.

这意味着我们可以在隐藏状态内部的错误中替换 *e* 。

![](img/e21db6bec5a97a156ece94ed5e2c4525.png)

Error of the Output Gate.

![](img/b82ea675b9bd7d6f4f802fad53a1d74d.png)

Error of the Cell State.

在计算了最终门内的误差后，您现在应该能够计算整个 LSTM 的误差。为了使这篇文章尽可能简洁，我将不解释每一个单独的门，因为它们都是以同样的方式完成的。如果您不确定您的计算是否正确，请继续阅读本文的编码部分，并对照代码检查您的计算。

## 最后，代码

作为这个系列的标准，这个网络背后唯一用于数学的包是 NumPy。TQDM 也被导入到培训阶段的进度条中。如果你更喜欢保持这个只有 NumPy 的项目，你可以很容易地删除 TQDM。

LSTMs 现在在工作中很常见，用于许多不同的事情。由于它们是递归神经网络，它们意味着时间序列预测，因此对于本文，我们将预测莎士比亚文本；更确切地说，是哈姆雷特。

`char_to_idx`和`idx_to_char`将在后面用于实现一个热编码。当我们处理文本时，一个热门的编码会给我们一种方法，把我们的莎士比亚式英语翻译成网络上的数字。`train_X`和`train_y`将是训练数据和标签。对于`train_X`中的每个角色，在`train_y`中正好有一个角色。这意味着网络将试图只根据前一个字符来预测句子中的下一个字符。听起来不可能？这就是递归神经网络的神奇之处。

接下来，我们定义了几个函数，它们会让我们以后的生活更加轻松，第一个是`oneHotEncode`函数。如果你不熟悉这个过程，我推荐[这篇](https://www.educative.io/blog/one-hot-encoding)文章。我们将要定义的另一个函数是 [Xavier 初始化](https://365datascience.com/tutorials/machine-learning-tutorials/what-is-xavier-initialization/)。递归神经网络对使用的初始化非常敏感，因此选择正确的一个非常重要。大概知道 Xavier 初始化是个不错的选择就够了；不过，你可以在这里自己看一下数学[。](https://cs230.stanford.edu/section/4/)

LSTMs 中使用了三个激活函数:sigmoid、tanh 和 softmax。如果你熟悉这些，你可能会注意到，导数的定义是错误的，但这是有原因的。

![](img/8b26f378e4287336d53d7b89504f737b.png)

Tanh derivative definition.

![](img/e8488ace3abd6bca49779111b237f61a.png)

Sigmoid derivative definition.

当稍后在代码中调用导数函数时，将对已经应用了双曲正切函数的变量调用它们。为此，定义了以下函数。

![](img/0242d58f2113b02b2afdbb9fc10b47d8.png)

New tanh derivative definition.

![](img/38ede72c647626dfc30dee9e711bd279.png)

New sigmoid derivative definition.

`LSTM`类接受 5 个输入:`input_size`、`hidden_size`、`output_size`、`num_epochs`和`learning_rate`。`num_epochs`将决定网络经历的时期(或训练阶段)的数量，而`learning_rate`将决定网络适应新信息的速度。每个网络权重和偏差被初始化为变量，分别以“w”或“b”开始，以对应于“忘记”、“输入”、“候选”、“输出”和“最终”的“y”的字母结束。

为了防止网络使用它不应该拥有的信息，在每次向前传播之前，必须清除网络的内存。这类似于在提问前从学生那里拿走抽认卡:你拿走了书面答案，但没有拿走他们先前存在的知识。同样的，清除网络记忆可以防止它作弊，但不能防止它学习。该记忆清除在`reset`功能中完成。同时，我们也用空白条目初始化`hidden_states`和`cell_states`字典。当网络第一次启动时，它仍然需要隐藏和小区状态，即使没有从过去回忆的信息。因此，我们用零初始化这两个存储单元。

下一个要定义的函数是`forward`函数。这将重置网络的存储器，并执行之前描述的每个计算以获得输出。每个门都存储在各自的字典中以备后用。

现在对于反向传播功能，`backward`。如前一篇文章所解释的，递归神经网络使用时间的[反向传播。这意味着每个权重和偏差的误差是其随时间变化的误差之和。](https://en.wikipedia.org/wiki/Backpropagation_through_time)

将每个误差初始化为零后，我们向后迭代每个时间步，将误差添加到变量中。如果您在上一节中正确地计算了，这应该都是有意义的。可能突出的一点是第 26 行。记住，我们的`tanh`导数函数假设输入已经应用了`tanh`。这不是`cell_states[q]`的情况，所以在找到导数之前`tanh`必须应用于它。

计算出每个误差后，我们用`np.clip`对它们进行限幅。这意味着任何大于 1 的值都被设置为等于 1，任何小于-1 的值都被设置为等于-1。LSTMs 通常不会受到消失梯度问题的影响，但它们仍然很容易受到爆炸梯度问题的影响，这就是我们这样做的原因。

最后，我们可以使用发现的错误和定义的学习率来更新网络的权重和偏差。

`train`和`test`功能是标准功能。`train` one hot 对每个输入进行编码，使用前向传播获得每个输出正确的概率。计算误差，使得预测加上误差等于期望的输出或标签。`test`函数向前传播数据，以获得每个字符正确的网络概率。然后`np.random.choice`被用来根据网络给出的概率随机选择一个角色。然后，它会打印出基本事实和预测文本，以及百分比准确度。

最后，我们可以初始化我们的网络。注意`input_size`等于`char_size + hidden_size`。这是因为我们的`input_size`是我们连接输入和隐藏状态后网络的大小。因此，它是两个尺寸的总和。

需要注意的是，我们正在用相同的数据进行训练和测试。LSTMs 不擅长概括，这意味着很难训练一个人来预测它从未见过的文本。当我们从零开始建造这一个的时候，有一些限制，其中最大的是时间。NumPy 不是为机器学习而设计的，这意味着这个网络效率非常低。正是因为这个原因，像 Tensorflow 和 PyTorch 这样的 ML 包才存在，所以如果你打算从头开始编写神经网络进行部署……我不推荐。然而，如果你是从零开始建立网络来学习，我认为这是最好的方法——也许是唯一的方法。

我的“从零开始构建神经网络”系列的第四篇文章到此结束。如果你从一开始就一直在关注，我希望你喜欢这些内容。如果你是新手，看看我以前的一些文章，我解释了[感知器](/mlearning-ai/building-a-neural-network-zoo-from-scratch-the-perceptron-335759f48089?source=user_profile---------2----------------------------)、[多层感知器](/mlearning-ai/building-a-neural-network-zoo-from-scratch-feed-forward-neural-networks-f754cc88eca2?source=user_profile---------1----------------------------)和[普通 RNNs](/mlearning-ai/building-a-neural-network-zoo-from-scratch-the-recurrent-neural-network-9357b43e113c) 。如果你喜欢这篇文章，我会很感激你把它分享给你的朋友和同事，一如既往地，[我的 GitHub 上有完整的代码](https://github.com/CallMeTwitch/Neural-Network-Zoo/blob/main/LongShortTermMemoryNetwork.py)。

再次感谢艾米丽·赫尔的精彩剪辑。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)