# Python 自动格式化程序:Autopep8 与 Black(以及一些实用技巧)

> 原文：<https://medium.com/mlearning-ai/python-auto-formatter-autopep8-vs-black-and-some-practical-tips-e71adb24aee1?source=collection_archive---------0----------------------->

## 以正确的方式格式化您的 Python 代码！

![](img/95ddbddb99d8c21b0be2927270af7e92.png)

Photography by Author

> 注意:本文是我的系列文章“[为数据科学编写生产就绪代码](/@josephlyu.sj/list/write-productionready-code-for-data-science-6ce4f3a12a3e)”的一部分，该系列文章关注代码质量和产品化。如果您已经尝试了这两种工具，并且想要选择一种来合并到您的工作流中，这将是最有帮助的。本文根据:`*autopep8 2.0.0*` `*black 22.12.0*` `*isort 5.10.1*`。

*以下是结论:相比 autopep8，我更喜欢 black，但在使用 black 时，有一些实用的提示(最后)你应该记住，它与其他一些工具(如 isort)并行工作时效果最好。*

# 介绍

[Autopep8](https://github.com/hhatto/autopep8) 和 [Black](https://github.com/psf/black) 都是很棒的工具，可以自动格式化你的 Python 代码，以符合 [PEP 8](https://www.python.org/dev/peps/pep-0008/) 风格指南。Black 在 GitHub 上有 30.4k 星，可能是同类工具中最受欢迎的，而 autopep8 有 4.2k 星。

一个主要的区别是，black 是一个固执己见的格式化程序，这意味着它总是将整个代码库转换为自己的风格，而 autopep8 在某种程度上保留了输入风格，只修复了必要的部分。

我在工作中使用了这两种工具，我想与您分享为什么我更喜欢黑色而不是 autopep8。

# autopep8 的问题

不要误解我，autopep8 是一个伟大的工具，我喜欢它，autopep8 最大的优点是它允许大量的用户配置，而 black 几乎不允许任何用户配置。然而，我遇到了一些 autopep8 的问题，这些问题足以让我停止使用它。

## 1.它积极地对进口商品进行分类

有些人可能认为这是一件好事，然而，在我看来，一个好的工具应该专注于一件事，而且只专注于这件事。作为一个格式化工具，它不应该试图改变代码的顺序，事实上，它有时会引起问题。

让我们考虑下面的例子。虽然`sys.path.append`一般不是好的做法，但是姑且说我们确实想在导入其余模块之前先做点什么。

Autopep8 会将脚本重写为以下内容:

这是有问题的。我能想到的另一个例子是，如果我们想为 Linux 终端启用 matplotlib 的后端，我们需要在 pyplot 导入`from matplotlib import pyplot as plt`之前设置后端`matplotlib.use("agg")` **。**

相比之下，黑色不会改变上面的示例代码。只有黑色代码的格式和语义保持不变。换句话说，black 不会对您的导入进行排序，也不会改变代码的顺序。*(而我们可以把整理导入的任务留给另一个伟大的工具* ***或者*** *，请继续阅读！)*

你仍然可以通过在一些代码片段中添加`# nopep8`来解决这个问题，明确告诉 autopep8 不要碰它:

在这种情况下它是有效的，但是我发现`# nopep8`排除规则并不适用于评论，例如 autopep8 仍然会将下面的`### some comment # nopep8`转换成`# some comment # nopep8`。

另一方面，black 不编辑注释和文档字符串，您可以通过在代码块前后添加两个注释行`# fmt: off`和`# fmt: on`来将一些代码排除在格式化之外。

不管怎样，这不是很 Pythonic 化，对吧？

## 2.它不能正确地强制缩进

Autopep8 的默认缩进设置为与编辑器的制表符大小相同，您可以使用`--indent-size`参数指定该值。一切都好，但是，autopep8 只使用这个输入值来设置每个语句开头的缩进，但是括号内的缩进总是设置为默认的制表符大小。

让我们看看这个例子，其中代码缩进是 1，我的编辑器的默认制表符大小是 4。

用 autopep8 `autopep8 --indent-size 3 -i format_02_raw.py` *(-i 表示到位)*将缩进设置为 3 后，我们得到如下:

我们可以看到，第一个函数中的缩进被正确地设置为 3，但是括号中的所有内容(在本例中，print 语句和矩阵)都被设置为默认的制表符大小 4。

我发现这一点是因为在我的工作环境中默认的标签大小是 2，而我更喜欢缩进 4。这听起来可能微不足道，但是为什么要冒不必要的不一致的风险呢？Black 不允许您配置缩进大小，因为它总是在所有地方将缩进设置为 4，这一点我一点也不介意。

# 正确使用黑色

黑色实现了自己的风格，有人喜欢，有人不喜欢。我很喜欢它，因为它通常非常干净和可读，但有两件事要记住。

## 1.使用结尾逗号

下面的例子可能是一些人不喜欢黑色的最常见的原因之一。让我们看一个格式化前后的例子:

before formatting (without trailing commas)

after formatting (without trailing commas)

这很烦人，但是在你生气之前，让我们对代码做一些小的修改，看看 black 如何改变它的行为:

before formatting (with trailing commas)

after formatting (with trailing commas)

是的，black 使用尾随逗号来决定项目是将被打包在一起还是留在新的行中。如果一个类似数组的项结尾没有逗号，无论是列表、矩阵还是字典，black 总是试图将其扭曲成一行；如果超出了行的长度，black 会将其内容放在新的行中，然后**会为您添加一个尾随逗号**。如果项目中有尾随逗号，black **确保其内容被分隔成新行**。

简而言之，一个类似数组的项目的内容在新行中的任何地方都会有一个尾随逗号。我认为这是有意义的，一般来说尾随逗号是很好的做法，因为它们使代码更容易维护，并且当你进行更改时产生干净的`git diff`。所以如果你不想让 black 把你的代码绕成一行，就在末尾加一个逗号！

*额外提示:当我写 SQL 查询时，我会写*

```
SELECT column_a
      ,column_b
      ,column_c
FROM some_table
```

*而不是*

```
SELECT column_a,
       column_b,
       column_c
FROM some_table
```

*同样的原因。它使得添加或删除项目变得更加容易，并且使得代码更容易维护。*

## 2.如果需要，指定线路长度

到目前为止，我不喜欢 black code 风格的唯一一点是关于长 if 语句，如下例所示:

before formatting

after formatting

它加强了一致性，但牺牲了可读性。Black 的默认行长度是 88，但有时我确实有稍微长一点的语句，我不希望它们被格式化成那样。

我的解决方案是允许稍微长一点的线长度。我们可以使用`--line-length`或`-l`参数指定行长度，如果我们将行长度设置为 100，`black -l 100 format_05_raw.py`，上面的例子将不会被重新格式化。根据我的经验，100 行左右的长度适合大多数长语句，同时仍然保持良好的代码可读性(尽管如果语句长度超过 100 个字符，您真的应该考虑重写您的语句)，但是当然，这取决于每个团队。

Autopep8 也有这样一个选项`--max-line-length`，但是，由于 auto pep 8 倾向于保留原始代码样式，格式化结果对指定行长度的敏感度**远不如黑色。**

## 3.使用黑色搭配 isort

正如我前面提到的，black 不会对你的导入进行排序，但是我们可以使用[isort](https://github.com/PyCQA/isort)(GitHub 上的 5.4k stars)来做到这一点。让我们看一个简单的例子，看看 isort 能做什么:

before using isort

after using isort

这很简洁，而且不会打乱我们的`sys.path.append`示例。

注意，isort 和 black 组织导入的方式略有不同，我们可以将 isort 的`--profile`选项设置为`black`，对符合 black code 风格的导入进行排序，`isort --profile black format_06_raw.py`；我们也可以用`-l`参数指定行的长度。

我们现在有了一个非常好的工作流程:首先使用 isort 对导入进行排序，然后使用 black 对代码进行格式化。我们可以在 Makefile 中将这两个步骤结合在一起:

```
format:
 isort --profile black -l 100 src/
 black -l 100 src/
```

现在我们可以简单地使用`make format`来实现上面讨论的一切(在一个假想的`src/`文件夹上)。

# 结论

在本文中，我们比较了 Python 中两种流行的自动格式化工具——autopep 8 和 black。我解释了为什么我更喜欢黑色和我喜欢如何使用它，我希望这是有帮助的。

这也是我第一次写关于 Medium 的文章，如果你喜欢我的作品，请考虑为它鼓掌，并在下面留下你的评论，我们下次再见！

感谢您的阅读，非常欢迎您的反馈。您也可以在我创建标签[*# 100 Articles fords*](https://www.linkedin.com/feed/hashtag/?keywords=100articlesfords)*(面向数据科学家的 100 篇文章)的* [*LinkedIn*](https://www.linkedin.com/in/shangjielyu/) 、*上关注或联系我，分享我认为有见地和有帮助的数据科学文章，以及我的评论、想法和其他实用技巧。*

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)