# 序列标记——自然语言处理的基础

> 原文：<https://medium.com/mlearning-ai/sequence-labeling-the-basis-of-nlp-a52c8fffa567?source=collection_archive---------0----------------------->

在这篇博文中，我们将介绍**自然语言处理****【NLP】**领域的序列标注。我们将从谈论这种技术在现实世界应用中的使用开始，然后通过列出我们可以找到的主要任务来更深入地讨论: ***命名实体识别*** 、 ***词性标注*** 和 ***分块*** 。

![](img/b8b6d886a17136141275749844c73064.png)

From [Unsplash](https://unsplash.com/fr/photos/wWpir9iTsaA)

**序列标注**是自然语言处理中的一项基本技术，用于*识别*和*标注*序列的组成部分，如句子中的单词或短语。它经常被用作其他 NLP 任务的预处理步骤，如*词性标注*或*命名实体识别*，并且是许多 NLP 应用的关键组件。

序列标记有许多实际应用。它可以在**文本分类**中用于对文档进行分类，在**情感分析**中用于确定文本的情感。在**信息检索**中，它有助于澄清查询的上下文和含义。此外，在**机器翻译**中使用了序列标记来识别句子的语法结构并简化翻译过程。

也就是说，我们现在可以借助一些代码示例来研究它是如何工作的。

# 序列标记的主要任务

序列标注用于广泛的自然语言处理任务，例如**词性标注**、**命名实体识别**、**组块**和**语义角色标注**，让我们来看看它们中的每一个…

## **词性标注**

词性(POS)标注是对句子中的词性(如*名词*、*动词*、*形容词*)进行标注的过程。

在信息检索中，通过区分不同的词性，可以帮助搜索引擎提供更相关、更准确的结果。

这里有一个例子:

```
import nltk

def tag_sentence(sentence):
    tokens = nltk.word_tokenize(sentence)
    tagged_tokens = []
    for token in tokens:
        if token.endswith('ed'):
            tagged_tokens.append((token, 'VERB'))
        elif token.istitle():
            tagged_tokens.append((token, 'PROPER NOUN'))
        else:
            tagged_tokens.append((token, 'NOUN'))
    return tagged_tokens

sentence = "The cat chased the mouse."
tagged_tokens = tag_sentence(sentence)
print(tagged_tokens)

# Output: [('The', 'PROPER NOUN'), ('cat', 'NOUN'), ('chased', 'VERB'), 
# ('the', 'PROPER NOUN'), ('mouse', 'NOUN'), ('.', 'NOUN')]
```

## **命名实体识别**

命名实体识别(NER)是对文本中的命名实体(如*人*、*组织*、*地点*)进行识别和分类的任务。

作为词性标注，它可以用于从大量文本中提取信息，并帮助更快地识别所需内容。

下面是一个使用 Spacy 的预训练 *en_core_web_sm* 模型的示例:

```
import spacy

nlp = spacy.load('en_core_web_sm')

def extract_entities(text):
    doc = nlp(text)
    entities = []
    for ent in doc.ents:
        entities.append((ent.label_, ent.text))
    return entities

text = "Barack Obama was born in Hawaii."
print(extract_entities(text))

# Output : [('PERSON', 'Barack Obama'), ('GPE', 'Hawaii')]
```

该模型已经过预训练，可以识别各种命名实体，并且可以通过对附加注释数据进行训练，针对特定任务或语言进行微调。

## **分块**

组块是序列标记的任务，包括将一个单词序列分成**组块**或**非重叠子序列**。这些组块通常用标签来标记，该标签指示它们在序列中的类型或角色。

它经常被用作其他自然语言处理任务的预处理步骤，如**命名实体识别**或**词性标注**。

假设我们有下面这句话:“*敏捷的棕色狐狸跳过了懒惰的狗。*

使用组块法，我们可以将句子分成以下几个组块:

*   ***敏捷的棕色狐狸*** (名词短语)
*   ***跳跃*** (动词)
*   ***战胜了*** (名词短语)

这是通过识别句子中的名词短语来进行组块的一种方法。组块和标签的结果**序列将是:**

> (NP，敏捷的棕色狐狸)(V，跳跃)(NP，越过懒狗)

## **语义角色标注**

**语义角色标注** (SR)不仅仅是将单词的语法角色确定为词性标注，而是专注于确定它们的意义以及它们之间的关系。

它通常包括以下步骤:

1.  识别句子中的谓语
2.  识别**谓词**的**自变量**
3.  用它们的**对应角色**标记自变量

下面是一个使用**斯坦福 CoreNLP** 模型/解析器的例子:

```
import os
from nltk.parse.corenlp import CoreNLPParser

# Set the path to the Stanford CoreNLP jar file and the Stanford CoreNLP models
os.environ['CORENLP_HOME'] = '/path/to/stanford-corenlp'

# Create the parser
parser = CoreNLPParser(url='http://localhost:9000')

# Parse a sentence and extract the SRL information
sentence = "The boy kicked the ball."
parse_tree = next(parser.raw_parse(sentence))
srl_triples = list(parse_tree.srl_triples())

# Print the SRL triples
for triple in srl_triples:
   print(triple)

# Output :
# (('kicked', 'VBD'), 'nsubj', ('boy', 'NN'))
# (('kicked', 'VBD'), 'dobj', ('ball', 'NN'))
```

每个三元组由一个**谓词**(三元组的第一个元素)、一个角色标签(三元组的第二个元素)和一个**参数**(三元组的第三个元素)组成。

在这个例子中，谓语是“*踢*”，角色标签“ *nsubj* ”表示“ *boy* ”是谓语的主语，角色标签“ *dobj* ”表示“ *ball* ”是谓语的直接宾语。

既然我们已经介绍了最常见的序列标记任务，让我们回顾一下使用的主要方法…

# 序列标记任务的方法

执行这些任务有多种方式，所选择的方法会显著影响性能和结果。

*   **基于规则的方法**:这些方法依赖于一组手动定义的规则，从预定义的规则到标记句子中的每个单词或识别文本中的命名实体。这些工具可以完成简单的任务，但是容易出错并且耗时。
*   **基于机器学习的方法**:这些方法使用机器学习技术从带注释的训练数据中学习给定任务的模式。
    它们的范围从**随机方法**到**基于深度学习的方法**，比如[变形金刚](/mlearning-ai/transformers-the-nlp-revolution-5c3b6123cfb4)。
*   **混合方法**:这些方法结合了基于规则和统计方法的优势，使用手写规则和机器学习技术的组合来识别参数和角色。

在结束之前，我想给你一些有趣的软件包，可以帮助你开始学习 NLP。

## 你需要知道的 Python 库

除了一般的机器学习库如 *Scikit Learn* 、 *Keras* 、 *Tensorflow* 和 *Pytorch* 之外，以下库专门针对自然语言处理:

*   [**NLTK**](https://www.nltk.org/)**:自然语言工具包(NLTK)是 NLP 的一个综合库，包括用于序列标注的工具，比如词性标注器和组块器。**
*   **[**spaCy**](https://spacy.io/) : spaCy 是一个 NLP 库，提供了各种用于序列标注的工具，包括词性标注器、命名实体识别、依赖解析器等。**
*   **[**GenSim**](https://radimrehurek.com/gensim/)**:GenSim 是一个用于 NLP 的库，包括用于单词嵌入和语言建模的工具，以及用于词性标注和命名实体识别的工具。****
*   ****[**Polyglot**](https://polyglot.readthedocs.io/en/latest/)**:Polyglot 是一个库，它提供了对广泛的自然语言处理任务的支持，包括词性标注、命名实体识别和语言检测。******
*   ******[**text blob**](https://textblob.readthedocs.io/en/dev/)**:text blob 是 NLP 的一个库，包括词性标注、名词短语分块、情感分析等工具。********

******感谢您花时间阅读这篇文章，我希望您在学习序列标签的过程中度过了愉快的时光。请随意与你的朋友分享这篇文章，如果你对数据科学或机器学习感兴趣，请查看我的其他文章[这里](https://www.npogeant.com/#articles)。******

******[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)******