# 8.2.1 自然语言处理

> 原文：[`huyenchip.com/ml-interviews-book/contents/8.2.1-natural-language-processing.html`](https://huyenchip.com/ml-interviews-book/contents/8.2.1-natural-language-processing.html)

1.  RNNs

    1.  [E] RNN 的动机是什么？

    1.  [E] LSTM 的动机是什么？

    1.  [M] 你会如何在 RNN 中进行 dropout？

1.  [E] 什么是密度估计？为什么我们说语言模型是一个密度估计器？

1.  [M] 语言模型通常被称为无监督学习，但有人说其机制与监督学习并没有太大的区别。你的看法是什么？

1.  词嵌入。

    1.  [M] 为什么我们需要词嵌入？

    1.  [M] 基于计数和预测的词嵌入有什么区别？

    1.  [H] 大多数词嵌入算法基于这样的假设：在相似上下文中出现的词具有相似的意义。基于上下文的词嵌入有哪些问题？

1.  给定 5 个文档：

    ```py
     D1: The duck loves to eat the worm
     D2: The worm doesn’t like the early bird
     D3: The bird loves to get up early to get the worm
     D4: The bird gets the worm from the early duck
     D5: The duck and the birds are so different from each other but one thing they have in common is that they both get the worm 
    ```

    1.  [M] 给定一个查询 Q：“The early bird gets the worm”，使用余弦相似度度量以及术语集{bird, duck, worm, early, get, love}，根据 TF/IDF 排名找到两个排名最高的文档。这些排名最高的文档与查询相关吗？

    1.  [M] 假设文档 D5 继续讲述关于鸭子和鸟的内容，并且提到“bird”三次，而不是一次。D5 的排名会发生什么变化？这种对 D5 排名的变化是 TF/IDF 的理想属性吗？为什么？

1.  [E] 你的客户希望你在他们的数据集上训练一个语言模型，但他们的数据集非常小，只有大约 10,000 个标记。你会使用 n-gram 模型还是神经语言模型？

1.  [E] 对于 n-gram 语言模型，增加上下文长度（n）是否会提高模型的表现？为什么或为什么不？

1.  [M] 当我们将 softmax 作为词级语言模型的最后一层时，可能会遇到哪些问题？我们如何解决这个问题？

1.  [E] 两个单词“doctor”和“bottle”的 Levenshtein 距离是多少？

1.  [M] BLEU 是机器翻译中常用的一个指标。BLEU 的优点和缺点是什么？

1.  [H] 在相同的测试集上，LM 模型 A 具有 2 的字符级熵，而 LM 模型 A 具有 6 的词级熵。你会选择哪个模型进行部署？

1.  [M] 假设你需要在文本语料库 A 上训练一个 NER 模型。你会使 A 区分大小写还是不区分大小写？

1.  [M] 为什么去除停用词有时会损害情感分析模型？

1.  [M] 许多模型使用相对位置嵌入而不是绝对位置嵌入。为什么？

1.  [H] 一些 NLP 模型在嵌入层和 softmax 层之前使用相同的权重。这种做法的目的是什么？
