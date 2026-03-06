# 现实世界中的人工智能-3。用你自己的语法

> 原文：<https://medium.com/mlearning-ai/ai-in-the-real-world-3-make-your-own-grammarly-36348d3f1c62?source=collection_archive---------3----------------------->

![](img/eff7c603a9d7c7c9bb09df7793544b98.png)

Grammarly 是 AI 在这十年取得的重大突破之一。我假设我们大多数人都知道什么是语法，以及我们如何从这样的工具中受益匪浅。如果我告诉你，你可以使用[变形金刚](https://huggingface.co/docs/transformers/index)很容易地制作你自己的最小拼写纠正工具，而不需要专门的编码知识。好吧，让我们不要等待，一头扎进帖子里。

根据我的理解，Grammarly 的补充版本完成了这三项主要任务:

*   寻找**拼写错误**
*   **意译**句子
*   为我们提供了句子的情感

因此，我们将无法实现与语法相似的性能，但我们可以通过为拼写错误建立简单的序列到序列模型以及为分类任务建立零命中率学习器来轻松达到基本准确性。

# 先决条件

*   至少支持 2GB GPU 的系统
*   变形金刚(电影名)
*   火炬
*   大约 50，000 个随机句子的数据

# 系统模型化

为了建立任何模型，我们都需要数据，如果你得不到足够的数据，不用担心，使用任何文本生成模型，如 GPT，甚至任何文本到文本生成模型，可以生成大约 5 万到 10 万个句子。然后使用这段简单的代码随机估算句子中的拼写错误，这样现在您就可以将源作为损坏的句子，将目标作为语法正确的句子。

上面的代码片段将估算句子中随机选择的单词的错误。参数 ***n_words*** 是我们如何控制一个句子中我们需要估算错误的单词数。

我选择了 T5 small 并使用了大约 50k 个句子作为建立模型的数据集。选择 T5 模型的主要原因是因为它们是 seq-seq 建模任务中最好的模型之一，并且由于变压器中的注意力机制，它们在效率和性能方面远远优于 LSTMs 或 rnn。为了实现建模部分，你可以看看[变形金刚](https://github.com/huggingface/notebooks/blob/master/examples/summarization.ipynb)的官方教程。上面提到的笔记本解释了如何微调 t5-small 模型的摘要任务，但是通过更改每个句子的前缀，您可以根据自己的意愿针对不同的任务进行微调。如果您想跳过所有这些，并且需要一个带有可配置参数的简单包装器，请随意尝试[这个](https://www.kaggle.com/vishnunkumar/neuspell)。这基本上是一个简单的包装器，我写的微调 NLP 模型目前，它只支持这三个:

*   Bert-base-不考虑分类任务
*   t5-小型，用于序列-序列任务。

一旦你对你的模型进行了微调，你就可以尝试你的模型，或者使用下面的代码片段来利用我已经微调过的模型。

上面的代码实例化了两个 transformers 模型，分别用于给定句子中的拼写纠正和情感分类。对于后者，请仔细阅读变压器给出的关于[零炮](https://huggingface.co/docs/transformers/v4.14.1/en/main_classes/pipelines#transformers.ZeroShotClassificationPipeline)分类的文件。请在下面找到上述代码如何工作的简单例子

```
input_text = input()
gram = Grammarly(input_text)
gram.grammarly()
```

**输出:**

```
input_text = I am happu to inform that w ehave won the competetio{'emotion': 'happy',
 'emotion_score': 0.5591591596603394,
 'modified_text': 'I am happy to inform that i have won the competition'} input_text = That is sad to har the passing away of hs grandfather{'emotion': 'sad',
 'emotion_score': 0.5666968822479248,
 'modified_text': 'That is sad to have the passing away of his grandfather'}
```

好吧，我们不会对上面看到的完全满意，但至少有一个重要的部分，拼写纠正器能够正确地找到不正确的单词，并用语义上接近句子上下文的单词替换它。通过使用更大的模型，即(来自 BigScience 的 t5-large 甚至 T0，它在许多任务中击败了来自 OpenAI 的 GPT-3)或增加训练数据，调整参数，我们可以获得更好的结果。我用最大序列长度 32 对模型进行了微调，这里使用的模型对较长的句子不太适用。也就是说，对于更小更简单的句子，这种模式将一如既往地有效

好吧，让我们总结一下，因为我想拖 X)，谢谢大家的宝贵时间和支持。再见，下次再见。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)