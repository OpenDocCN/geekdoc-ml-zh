# 使用机器学习在 python 中制作不可破解的记忆密码。

> 原文：<https://medium.com/mlearning-ai/making-uncrackable-memorable-passwords-in-python-using-machine-learning-5512ef95bc4b?source=collection_archive---------8----------------------->

![](img/c94445401903b8698467aef64aa88c79.png)

Photo by [Dan Nelson](https://unsplash.com/@danny144?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我知道没有密码是真正无法破解的，但考虑到破解这些密码所需的资源和时间范围，这将是相当困难的。

这段代码是约翰·戈迪诺尝试制作一个随机生成器的密码生成器代码的延续和扩展。虽然这肯定是一个有用的程序，但我试图通过使该项目更加用户友好，使任何人都可以记住给定的密码来进一步扩展该项目。你可以在这里找到一个随机密码生成器[的原始视频链接。](https://www.youtube.com/watch?v=qgwEs36D_Xc)

这个程序的核心功能是从你喜欢的东西中建立密码。它将要求用户输入 3 个信息。首先，输入他们喜欢/去过的地方，然后输入他们喜欢的单词。

首先，您需要导入相关的库。

对于这个项目，将使用两个主要的库

python 内置的 Random 和 nltk，后者是处理自然语言时最好的机器学习库之一。#

如果您的机器上没有内置的 nltk，那么您需要安装它。如果是，运行

Pip 安装 nltk

这将安装库。因为 nltk 并没有将您需要的所有数据都预先构建好，所以您可能需要运行

Nltk.download('omw-1.4 ')

首先，我们应该定义一些我们将要使用的局部变量:

最重要的一个是本地变量 password，这是我们用来存储组成密码的字符串的地方。

其次，我们需要设置一个包含所有特殊字符的变量特殊字符，我们将使用这些特殊字符来拆分组成密码的不同单词。从 UX 的角度来看，使用特殊字符的原因是为了使单词更容易记忆，就像我们根据字母拆分单词一样，这看起来像是混乱的单词，可能会让用户感到困惑。在最糟糕的情况下，如果两个单词组合在一起，可能会产生一个已经存在的单词，从而使用户更加迷惑。

好了，设置了两个局部变量，我们要开始一些功能了。首先，我使用了一个 lambda 表达式，用 random.choice 从特殊字符中选取一个随机字符。然后，添加相同字符的副本，并将它们都添加到密码中。我将它们都定义为函数，以便以后能够重用它们。

接下来是程序最简单的部分，要求用户输入他们最喜欢去的度假地或他们去过的地方，并将其附加到密码变量中。因为已经定义了特殊的字符选择器和加倍器，所以也将它添加到密码的位置之后。

然后转移到可以说是该计划最困难的方面，即使用机器学习。我们将定义一个新的嵌套函数；在这个函数中，我们想要创建一个列表。然后，您希望对他们输入的最喜欢的单词调用 Synset for 循环。Synset 是机器学习的 NLP 分支的一部分，但它为输入的单词查找同义词，有时它只会有一个精确的匹配。其他时候，这个词可能有许多同义词。然后，我运行了一个嵌套的 for 循环来对同义词进行词汇化。

什么是引理

一个引理是一个词的缩写，以回到它的基本形式。一个很好的例子是单词 running，而不是输入到列表中的 running，因为 run 是 running 的基本形式。

一旦你这样做了，这是直截了当的，你必须做的就是将选择的同义词附加到密码上。然后重复附加用户输入单词的另一个同义词的过程，并且对于最后的部分，在末尾附加一个 2 位数。

结果是:

我认为测量这些密码的有效性是很好的，所以我已经通过密码强度检查器运行了它们，该检查器可以在[https://bitwarden.com/password-strength/](https://bitwarden.com/password-strength/)找到。所有这些密码都是几个世纪前的，我认为这足以说明这些密码目前已经足够好了。

这个测试我运行了三次

第一次我的输入是:

斯德哥尔摩

雪

火

产生的密码是:

”“stockholm^^lead_by_the_nose%%terminate9

我第二次使用的输入是:

伦敦

足球

扑克

产生的密码是:

^^london[[football_game@@poker56

我的第三次:

柏林

食物

聚会

第三个结果是:

”“Berlin^^food>>company79

如您所见，这些密码通常很容易记住，但也比随机生成的密码更容易记住。仍然有一些升级会改善这一点，如删除精确匹配。但如果你想接受挑战，我会让你自己决定。

就 NLP 而言，这是机器学习的一个非常简单的应用。你并不是真的在分析单词的用法，但它让你很好地理解了如何使用一个库来实现有趣的功能。

如果你想要一个挑战或者改进项目，这里有一些方法。

3 种改进项目的方法

1.交换单词和位置，使位置不总是在第一位，而是附加在随机位置。

2.通过允许用户添加他们认为与自己生活相关的数字，例如出生年份、毕业年份，而不是一些随机生成的数字，来切换它，使它变得与用户更相关

3.使输入的单词的精确匹配不可能被插入到密码中。

你可以在这里找到 GitHub [的链接。](https://github.com/TyanBr/random_password)

完整的代码是:

```
from random import randomimport nltkfrom nltk.corpus import wordnetimport random#you will need to download this data set if you do not already have it loaded as it does not come preinstalled with NLTK#nltk.download('omw-1.4')def password_generator():password = ''special_Characters  = '!"£$%^&*()[]:;@#~?<>'divider = lambda x: random.choice(x)doubler = lambda x: x + xpassword = doubler(divider(special_Characters))city = input('What is a city you like that you have visited?')password = password + city + doubler(divider(special_Characters))word1 = input('What is a word that you like (do not type a name) ')def synonym(x):synonym_list =[]for syn in wordnet.synsets(x):for lemm in syn.lemmas():synonym_list.append(lemm.name())return random.choice(synonym_list)password = password + synonym(word1) + doubler(divider(special_Characters))word2 = input('What is a second word that you like (do not type a name) ')password = password + synonym(word2) + str(random.randint(0,99))return print(password)password_generator()
```

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)