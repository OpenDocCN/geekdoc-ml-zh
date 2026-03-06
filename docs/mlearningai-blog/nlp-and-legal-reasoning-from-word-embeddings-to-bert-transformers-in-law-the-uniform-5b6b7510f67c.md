# 自然语言处理和法律推理:从单词嵌入到伯特，法律中的变形金刚和统一商业法典——加密货币案例研究

> 原文：<https://medium.com/mlearning-ai/nlp-and-legal-reasoning-from-word-embeddings-to-bert-transformers-in-law-the-uniform-5b6b7510f67c?source=collection_archive---------2----------------------->

> **机器学习、深度学习和数学**

机器或深度学习的核心是应用*复杂*数十年的数学，使用半复杂算法，迫使计算机学习，或者更确切地说是从**数字数据**中*数学提取模式*。任何可以转换为数字格式的数据都可以服从这些严格的分析迭代*公式*，这些公式通常由主要用 **Python** 编程语言 **(** 例如 **Keras、Numpy)** 编写的计算机算法来实现。

可以肯定的是，在互联网**大数据** *大约在* 2000 年到来之前，这些机器或深度学习模型的真正力量很大程度上是*理论上的*，并且在有限的范围内*仅在允许的情况下*实现，或者由于“大”数据的稀缺以及 20 世纪 80 年代和 90 年代速度较慢、不太复杂的非基于 GPU 的处理器的阻碍

*然而，互联网的到来预示着“大”数据的新淘金热，这是一个新的前沿，暴露了这些神经网络理论框架的真正力量。**摩尔定律**确保处理器和处理速度将呈指数级增长，通过*尤其是*引入 GPU 和并行计算的强大功能(例如**多线程**)，进一步允许这一新兴领域极大地改变科学研究和进步的范围。*

*![](img/5c2cb394cf3331a06ca300c0aae1a11b.png)*

*Classical representation of a neural network, which depicts the flow of data (numbers) from one layer to the next. The neurons or ”perceptrons” are nothing more than mathematical formulae which calculate a number based on prior numeric inputs, generating an output number which is fed to the next layer (set of neurons).*

*由此开始了数字淘金热，理论家、开发人员、数学家、物理学家、计算机科学家和工程师都在合作进一步增强深度学习的理论、数学和算法基础。*

*从线性回归，到分类，到感知器，再到深度学习多层神经网络，自世纪之交以来，该领域蓬勃发展，从而导致人工智能(深度学习子集)、科学研究和创新以及工程领域的结构性进步。*

*然而，自然语言处理(“NLP”)从来就不是一个简单的常识性“**大数字数据**”候选对象，因为尽管大量，但文本本质上是非数字的。此外，“模式”(包括从文本模式中提取的“含义”)无法以一种*直观直接的*方式进行数学描述。*

*因此，与计算机视觉或强化学习相比，NLP 需要更高的创造力，甚至可能需要更高的“大数据”直觉，因为数字图像非常丰富，很容易进行数学描述。此外，计算机视觉的一些工具，例如[卷积](https://en.wikipedia.org/wiki/Convolutional_neural_network)，并不是深度学习特有的新概念。它们是为其他目的而创建的，但只是最近才被用作深度学习的工具(许多其他的*深度学习数学构造*，例如 [**Relu**](https://en.wikipedia.org/wiki/Rectifier_(neural_networks)) 、 [**Sigmoid**](https://en.wikipedia.org/wiki/Sigmoid_function) 、逻辑回归、梯度下降、奇异值分解、特征向量、 [**Softmax**](https://en.wikipedia.org/wiki/Softmax_function) 等)。**另一方面，文本不能被直观地视为数学结构**。“数据科学家”、工程师、语言学家、计算机科学家必须合作建立一个可行的框架，以数学方式处理和表示文本*。**

> ****机器学习中文本的数值表示****

**与它们的过程化或面向对象的非科学对应物不同，机器学习程序依赖于**大型数据集**——这与变量和对象或数据库不同，后者是大多数商业或非科学编程范例的基础。相反，机器或深度学习范式(**数学+代码+大数据**)依赖于内部化或处理为大型表格、数组或矩阵的数据，这些数据可以由机器学习工程师以无数种方式操纵。因此，例如，程序员/工程师可以使用**线性代数**，矩阵的转置，向量或矩阵的乘法，和/或通过获得矩阵的“SVD”或**奇异值分解**，以及许多其他基于矩阵的操作来处理大型数据集(由于这个原因，**线性代数**在机器学习中是最重要的，其次是**统计，然后是微分**)。**

**![](img/86663e3c27d7988a249c3216440ef7b2.png)**

**Relu and Sigmoid “Activation Functions” used in Neural Networks. These are simply mathematical equations which take a numeric input, x, and produce a numeric output, y, which is then fed to another layer in the network. Through “gradient descent” and “backpropagation”, these activation functions help the computer *numerically* identify patterns in data.**

**机器学习(ML)程序处理的数据不同于它们的非 ML 对应物处理的数据。因此，虽然其他程序通常处理几乎每一种可以想到的数据类型(例如字符串、布尔型)，而数据库可以存储包括字符串(文本)、图像、文档等在内的异构数据。机器学习矩阵通常必须*映射*或转换成适用于复杂的线性代数和微积分的数字格式，这是识别数据中的模式所必需的。这对于“有监督的”机器学习模型以及它们的“无监督的”对应物都是如此，这可以更好地理解，因为大多数神经网络依赖于“T2”激活函数，例如 **Relu** 或 **Sigmoid** ，它们需要*数字*输入，以便计算*数字*输出。**

**就图像而言，大多数图像都是以一种矩阵或向量的形式存储的，可以很容易地按原样输入神经网络进行处理。关于如何在神经网络架构中描绘这样的图像，没有太多的猜测。此外，几乎没有预处理(除非图像取自视频)，并且如果需要加速处理或出于任何其他原因，机器学习程序本身通常执行必要的转换(例如，将彩色图像转换为灰度图像)。**

**![](img/19ebb111cf5cdd1e22cbc12df1e77304.png)**

**A convolutional neural network essentially takes an image matrix (basically a table of numbers), applies various mathematical operations, and “learns” to identify patterns depicting a happy or angry face. Image Source: [Three convolutional neural network models for facial expression recognition in the wild (techxplore.com)](https://techxplore.com/news/2019-05-convolutional-neural-network-facial-recognition.html)**

**另一方面， **T** ext 通常由单个字符表示(每个字符在内部以二进制格式用数字表示)，这些字符被*组合*以形成*单词、句子和整个文档或语料库*。字符、单词和文档的可能排列或组合的数量是一个天文数字，可能是不可理解的。而且，虽然很少有人争论图像代表它所代表的东西，并且可以不声不响地按原样输入到神经网络中，但更大的问题比比皆是——*什么应该输入到神经网络中以进行* ***自然语言处理*** ？个人角色？单词本身？一段话？包含主题词的整个文档(或“语料库”)。字典呢？什么字典？维基百科？韦氏词典？而语境呢？什么背景？文档整体本身就是“上下文”吗？如果文档引用了另一个文档，那不是上下文吗？还有，如果文件的作者想要误导读者，出于潜在的非法目的，为什么这不是上下文的一部分呢？*无限期*。**

**![](img/2558187b238528314521a0a2ad2fffbf.png)**

**Simple Bag of Words illustration. The numeric mapping does not show the relationship of the words to each other and depict no “meaning”.**

**诚然，这些基于文本的自然语言处理问题的解决方案仍处于起步阶段。因此，文本的表示通常首先从“标记化”开始，本质上是将整个文档、段落或句子转换成一系列*不相关的*单词，或**标记**，通常被认为是一个**单词包**。这些记号有时又通过引用**词频**用数字*表示*，例如段落、句子或文档*语料库*中特定单词的字数。从这个过程中产生的一个流行的度量是 **TF-IDF** 度量，“术语频率，逆文档频率，这是一种在数学上稍微复杂一些的计算单词数量的方法，其目标是破译文本的含义或其他特征(例如**情感分析** —句子、段落或文档通常是生气、悲伤还是高兴——这可以通过计算单词“高兴”在语料库中的出现次数来实现)。**

**![](img/fcaa591fe5b02dbc551562383f6fde7a.png)**

**N-Dimensional Word Embeddings Viewed in 3D using “PCA”. The words thus grouped show “meaning” in the sense that words close to each other in the 3D space are closer in “meaning” to each other (e.g. man is close to woman). Words that are far apart, conversely, are not similar (hence further providing a clue as to “meaning”, e.g., a car is not the same as a vegetable, and would appear far apart in the vector space.)**

**经过大量的研究，这些通用的词袋模型演变成了稍微更有意义的表示，或**词嵌入**，用最简单的术语来说，它指定了一个向量(每个向量由 2 到 N 个条目组成)，该向量描述了该词与被检查文档生成的词典中的*其他*词的关系。这种映射可以在三维空间中完成(每个向量和/或单词有三个数字)，并且表示一个单词的每个向量相对于同一字典中的其他单词具有由其数字向量定义的相对“含义”。因此，例如，在经常引用的国王的例子中，代表国王的三维向量应该在代表王后的三维向量附近*。***

**由此产生的一个流行模型是 [**word2vec**](https://en.wikipedia.org/wiki/Word2vec) 模型，随后是基于 [**transformer**](https://en.wikipedia.org/wiki/Transformer_(machine_learning_model)) 的模型，如谷歌的 **BERT 或 GPT-3** 模型。**

**![](img/7aaeaccbae14de6367e7288cdd16ec7e.png)**

**Depiction of words in 3D vector space mapping of words. Notice that “man” is near “woman”, and “queen” is near “king”, depicting a relationship (e.g. partial meaning) between the two words which can be described or otherwise handled numerically.**

**这些模型，包括 word2vec 和 BERT，允许神经网络“学习”如何解读意义、完成句子、回答问题和其他与 NLP 相关的任务，尽管这些模型的性能与非基于 NLP 的模型相比相形见绌(例如，用于游戏的**强化学习**，它在自动化游戏性能方面取得了显著进步)。**

**![](img/b75e549297211fdf682e32c16fd49b34.png)**

**Example Word Embedding, whose numeric values are calculated relative to other similar words in the vector space, such as to imply a partial “meaning” of the words.**

> ****法律程序中的自然语言处理****

**一个可行的法律 NLP 神经网络模型必须应对难以克服的障碍，有些是法律实践中特有的，有些只是在处理任何文本或基于文本的文档时遇到的一般问题。**

**法律中一个突出的、无处不在的问题通常是**歧义**，例如，一个有歧义的单词或短语的正确含义。这在法院解释成文法或先前的判例法时经常遇到，在私法中，在解释合同条款时也经常遇到，仅举几个例子。**

**![](img/1217b5446a9fec59c5f0e19a9fef440c.png)**

**Law School “Case Book” On Statutory Interpretation, Which References the “Canons” of Statutory Interpretation**

**用最简单的术语来说，例如，法官可能试图破译计算机犯罪法规中“计算机”一词的含义。该法规可能会禁止第三人对“计算机”的黑客攻击，但没有明确规定游戏控制台是否也构成“计算机”。同样，合同纠纷的私人当事方可能声称租赁协议中的“计算机”一词包括相关硬件，如键盘和鼠标，因此可能声称保险赔偿条款也涵盖计算机外围设备的失窃或损坏。在解决这种模糊性时，法官可能会参考其他法令、先前的地方判例法(已公布的判决)、先前的国家或联邦判例法、字典或经常被引用的法令解释条例。在解决合同纠纷的过程中，私人当事人或者法官(如果有后续诉讼的话)也可能会查看过去的商业交易、字典或合同本身，以得出“计算机”的含义。私人当事人和法官也可以利用法定解释准则，即“**expresso unius est exclusio alter ius**”——*提及一个项目排除其他未提及的项目*——来确定“计算机”是否包括或排除其外围设备。**

**这种“法律推理”，在某些方面并不完全不同于日常的“推理”，必须在任何用于法律的基于自然语言处理的模型中加以考虑。**

# ****法律中无处不在的大数据****

**法律的一个固有优势是，如果你不考虑偶尔遇到的借口和其他形式的欺诈，那么“法律”推理的来源，例如，含义，通常都可以在网上获得，供任何人审查。事实上，数十亿页的法律文本唾手可得。**

**因此，法律中“意义”的潜在来源，例如“法律”推理，是丰富的。每个州的法规都可以在网上找到，判例法(公布的判决或“先例”)也是如此，它列出了用于解决歧义或以其他方式定义法律术语的准则。**

**确定某一特定事实模式是否“符合”法规也是“法律”推理的一个功能，它基本上涵盖了法官或陪审团确定的事实是否是特定刑事或民事(或行政)法规规定的有罪或责任的事实类型的问题。已公布的案例法，例如先例，充满了这样的分析，是确定哪些事实模式会导致可起诉的侵权行为或犯罪的良好来源。法官在决定事实模式是否“适合”法规时，也可能被赋予使用上述工具和方法解决不明确的法规术语的任务。**

> ****迈向破译法律推理和训练深度学习 NLP 模型的范式——统一商业代码和** [密码货币](https://en.wikipedia.org/wiki/Cryptocurrency)**

**[统一商法典](https://www.law.cornell.edu/ucc)是一套全面的“统一”商业法规，已经被美国所有 50 个州以某种*的形式单独*。UCC 的法律规定涵盖销售、租赁、流通票据、银行存款和其他此类商业相关话题。任何关于 UCC 术语含义的当地争议都由当地州法院解决，而不是*国家*联邦法院。****

*因此*每个单独的*地方(州)法院有能力确定术语“银行”、“货币”或“交易”的含义，以及[这些术语是否包含例如**加密货币**](https://media2.mofo.com/documents/190424-security-interests-bitcoins-cryptocurrency.pdf) 。因此，纽约州法院可以认定 UCC *确实*监管加密货币“交易”，而加州法院可以自由决定其*不监管*。在这样做的时候，法院将依靠法定解释的准则、他们自己关于该主题或类似主题的判例法、UCC 法令的“立法历史”、当地立法机构在通过 UCC 时是否就该问题发表过意见、关于该主题的学术论文，最后，他们还可能依靠其他州(其他司法管辖区)关于 UCC 交易的判例法。这些“来源”中的一些在一定程度上具有约束力，而另一些仅仅是提示性的或“有说服力的”(例如，来自另一个司法管辖区或美国州的关于该主题的判例法或先例)。法院还发现，先前的商业交易和订立私人合同的其他当事人对法规的主观解释，可能为 UCC 中某一特定术语的含义提供进一步的线索(尽管不具有约束力)。*

*在这种背景下，很明显，即使是最好的基于" [transformer](https://en.wikipedia.org/wiki/Transformer_(machine_learning_model)) "的模型，即目前最先进的自然语言处理范式的选择，也很难*正确地*(通过回归或分类)裁决一场 UCC 争端，因为这将由(公正的)法官客观地裁决。因此，除了最简单的案例(如*)之外，任何只根据 UCC 法令训练的自然语言处理模型(不考虑案例法或无数其他来源的“含义”)对大多数人来说都是非常有限和不利的。，NLP 模型可能“知道”使用美元或****Paypal****的交易受 UCC 监管，但它将难以决定* ***加密货币*** *交易是否也受其规定*。*

*加密货币在 UCC“意义”的不确定(“模糊”)性质由[本条](https://media2.mofo.com/documents/190424-security-interests-bitcoins-cryptocurrency.pdf)进一步描述，它可以被解读为“意味着”目前的 UCC 不监管加密货币，即“[*I*]*n 2017 年 7 月，统一法律委员会完成了统一示范州法，* ***《虚拟货币商业统一监管法案》(法案)****……..截至本文发表之日，尚未有任何州将该法案或其补编纳入法律。比特币基金会强烈反对通过该法案和补充条款，认为在一个不稳定和不断变化的领域制定这种类型的示范法还为时过早。**

*当然，这并不是说，美国 50 个州中任何一个州的地方法院都不能认定目前的 UCC 确实监管比特币。*

> ***结论***

*深度神经网络建模在计算机视觉(CV)和自然语言处理(NLP)领域取得了巨大进步。在 CV 中，流行的[甘](https://en.wikipedia.org/wiki/Generative_adversarial_network#Fashion,_art_and_advertising)和[强化学习](https://en.wikipedia.org/wiki/Reinforcement_learning)模型正在彻底改变过多的行业，并且只会继续发展。在 NLP 中，众所周知的 [GPT](https://en.wikipedia.org/wiki/GPT-3) 和[伯特](https://en.wikipedia.org/wiki/BERT_(language_model))模型只是触及了它们真正潜力的表面，随着新模型在大数据集上得到训练，它们将会随着时间的推移而改进。尽管法律实践是由不一致的半统一的异质法规和法律组成的复杂拼凑物，但它是 NLP 的一个有前途的候选对象，因为存在数十亿潜在的“语料库”文档来源——但必须仔细选择它们以提供某种程度的效用。*

*本文只是简要地介绍了法律中的自然语言处理这一主题，其他许多文章肯定会接踵而至。*