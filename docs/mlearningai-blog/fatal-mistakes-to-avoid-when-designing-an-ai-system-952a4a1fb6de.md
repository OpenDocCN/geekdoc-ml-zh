# 订购人工智能系统时要避免的 8 个致命错误

> 原文：<https://medium.com/mlearning-ai/fatal-mistakes-to-avoid-when-designing-an-ai-system-952a4a1fb6de?source=collection_archive---------6----------------------->

## 一个非技术客户和经理订购人工智能项目的指南，来自一个既是技术开发人员又是经理的人，带着爱。

![](img/5e55660074cb3a5465a23f78033d62ed.png)

Photo by [Sammie Chaffin](https://unsplash.com/@sammiechaffin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

人工智能(AI)是一个相对较新且快速变化的行业。人们普遍认为，人工智能有潜力改变我们的生活和工作方式，但它如何做到这一点的细节却难以捉摸。围绕人工智能和机器学习(ML)的广泛宣传增加了人们对人工智能项目的兴趣和困惑。大多数经理感到压力，想了解先进技术却不问问题，这没有什么帮助。

高期望和糟糕的理解相结合，在人工智能项目中提供了反常的激励，以优先考虑快速构建具有华而不实结果的模型，并将最少的资源分配给枯燥但关键的工作，以确保系统在初始合同结束后继续良好运行。因此，如果一开始更快或更方便，开发人员(dev)可能会面临削弱他们自己的系统的压力。像使用糟糕的训练数据这样的捷径会导致糟糕的模型性能——这是开发人员、他们的经理和他们的客户都不希望看到的。然而，在订购系统的人不理解他们所要求的含义的情况下，这样的结果是难以避免的。

好消息:为了设计和实现一个伟大的人工智能项目，订购人工智能系统的人(无论是内部经理还是外部客户)不需要成为机器学习专家。毕竟，理解 AI/ML 并不是他们的工作，就像理解阑尾切除手术的实际细节也不是他们的工作一样——不同的专业人员专攻不同的领域，这种专业化让我们每个人都可以深化特定领域的知识。相反，为了设计和管理一个伟大的人工智能项目，管理者需要理解人工智能发挥作用所需的基本输入，以及在给定这些输入的情况下，他们实际上可以期望什么样的输出。

不幸的是，考虑到大多数行业采用人工智能的相对[早期阶段](https://www.softwaretestingnews.co.uk/firms-lack-the-knowledge-to-deal-with-ai-reveals-survey/)，以及一般公众对人工智能的[理解](https://www.forbes.com/sites/steveandriole/2017/05/15/the-lack-of-intelligence-about-artificial-intelligence/?sh=27e85a6e5ebd)不足，经理们在人工智能和 ML 项目的设计和执行中仍然容易犯一些错误。以下错误对项目的成功绝对是致命的，但只要留心它们就可以很容易地避免——不需要机器学习课程。

1.  **未能明确定义问题**
    你想用 AI 实现什么？我经常看到经理们专注于使用人工智能的*想法*，而没有考虑结果将如何在实践中使用。在你试图解决问题之前，明确定义问题——记住，更花哨的解决方案并不总是最好的！而不是说，“我听说过这个很酷的技术，它能为我做什么？”通过识别一个现存的问题并问“什么技术可以帮助我解决这个问题？”可以实现更多的目标你不需要确切地确定问题应该如何解决，但是你需要能够清楚地说明问题是什么。一个好的人工智能架构师或开发人员应该能够带你了解潜在的人工智能解决方案将如何解决你的问题，并帮助你选择最合适的一个。
2.  **专注于任务而非成果**
    作为管理者或客户，专注于特定的任务会拖累系统功能，引起开发人员的困惑，延迟成功的开发。任务是可互换的，只需要被将要执行它们的开发人员理解；在项目的整个生命周期中，结果要具体得多，并且应该得到所有相关利益相关者的同意。一个通过完成特定任务来衡量进展的经理，在发现实现相同结果的更好方法时，将很难适应。通过实现结果来衡量进展的经理只需要确认结果已经实现，让开发人员自由地优化他们认为合适的工作细节。
3.  **不提问是因为你不想看起来很蠢**
    这是一种正常但非常适得其反的人类本能，尤其是在一些以信誉为主要货币的行业。如果你担心听起来很愚蠢，你可以试着在提问前谷歌一下，但是请不要犹豫向你的开发者和架构师询问你不熟悉的技术问题。他们工作的一部分是确保你明白他们卖给你的是什么，但是如果你为了面子而假装懂，他们就做不到这一点。如果你不明白你同意的是什么，各种讨厌的问题可能会出现。不懂技术并不可耻——帮助你的开发者帮助你做出正确的决定！
4.  **未能对训练数据进行优先排序**
    你可能听说过训练数据可以用来训练机器学习模型。但是你可能没有意识到，没有训练数据，几乎不可能产生一个 ML 模型。机器*学习*的[定义因素](https://mitsloan.mit.edu/ideas-made-to-matter/machine-learning-explained)是模型需要“学习”数据来“学习”完成任务。此外，坏的训练数据意味着模型将学习错误的东西并产生错误的结果。这意味着您不仅需要训练数据，而且训练数据集需要健壮、准确和一致。训练数据应该包含许多以您希望的方式解决问题的记录-如果您正在尝试自动化任务，这将是正确完成任务的示例，或者如果您正在尝试识别特定条件，则是已经识别的那些条件的示例。[【2】](#_ftn2)
    因为充足的训练数据对于 ML 模型来说是如此重要的一个要求——但通常很难获得——所以已经开发出了像[迁移学习](https://www.analyticsvidhya.com/blog/2021/10/understanding-transfer-learning-for-deep-learning/)这样的快捷方式来减轻获取训练数据的负担。这些减轻了负担，但并没有消除它。没有一个机器学习模型是真正不需要训练数据的。
    (在人工智能中，这个规则有一个例外:基于规则的模型，它不是 ML，但仍然是人工智能的一种类型，不需要训练数据。对于基于规则的模型，您并没有完全摆脱困境-这些模型的开发仍然需要与主题专家进行清晰的讨论，以定义模型将使用的规则，但如果无法获得训练数据，这可能是一种有用的技术。并不是所有的问题都可以用基于规则的模型来解决，所以这不是解决缺乏训练数据的灵丹妙药。关于基于规则与 ML 分类模型的更多信息，请参见[这篇文章](https://bdtechtalks.com/2020/06/05/rule-based-ai-vs-machine-learning/)，或者关于混合模型的信息，请参见[这篇演讲](https://www.youtube.com/watch?v=YHAeGBM9z3I&feature=youtu.be)
5.  **利用腐败或误导性数据来获得足够的训练数据**
    可悲的是，光是观测数据的数量还不足以确保良好的训练集。有时，通过收集许多“自然发生”的容易获得的记录，即在系统中已经存在的记录，可以快速收集训练数据，而无需彻底审查。这种数据集——即使它包含数百万行——也会导致许多问题。你可能听说过人工智能在[招聘算法](https://www.reuters.com/article/us-amazon-com-jobs-automation-insight/amazon-scraps-secret-ai-recruiting-tool-that-showed-bias-against-women-idUSKCN1MK08G)中偏向男性，或者面部识别技术[识别浅肤色面孔](https://www.nytimes.com/2018/02/09/technology/facial-recognition-race-artificial-intelligence.html)最好。最容易获得的观察结果可能不是训练你的模型的最佳依据，并且可能包括你(希望)不想延续的现有社会偏见。训练集中某类观察的相对流行程度对模型分配给不同特征的权重有很大影响。这对你的人工智能系统有道德和商业上的影响。要开始探索人工智能中复杂的统计偏差问题的冰山一角，请查看本文。
6.  **为生命系统制造死亡模型**
    想象一下，你正在根据 6 个月来收集的用户偏好训练一个购物推荐算法。但是这个算法应该已经使用了很多年了。潮流在变，用户在成熟，很快一个年轻的专业人士就会被推荐过时的童装，而不是适合他当前生活方式的时髦的办公室服装。这种算法显然永远不会在快速时尚的现实世界中生存——真正的广告算法是[通过不断吸收新数据来不断调整](https://www.wired.co.uk/article/ai-personalised-shopping)以适应用户更新的偏好。[反馈循环](https://www.oreilly.com/library/view/building-machine-learning/9781492053187/ch13.html)即使用户和环境发生变化，也能保持模型的相关性。[【3】](#_ftn3)欲了解更多关于响应性和适应性人工智能系统的信息，请查看[这些](https://www.clarifai.com/blog/closing-the-loop-how-feedback-loops-help-to-maintain-quality-long-term-ai-results) [博客](https://databistro.tech/moving-towards-scalable-responsive-and-sustainable-machine-learning/)或[本文](https://cloud.google.com/architecture/mlops-continuous-delivery-and-automation-pipelines-in-machine-learning)。
7.  **制作依赖模型**
    没有什么是永恒的。这包括您的员工、供应合同和硬盘。依赖人或系统的模型最终会消失，这些模型将变得无法使用，在设计你的人工智能系统时考虑到这一点是个好主意。在编码社区中，[可再生代码](https://www.software.ac.uk/blog/2016-10-03-why-reproducibility-important) [和](https://levelup.gitconnected.com/why-documentation-is-more-important-than-your-code-f5607639a8fe) [文档](https://filtered.com/blog/post/project-management/the-importance-of-documentation-in-software-development#:~:text=For%20a%20programmer%20reliable%20documentation%20is%20always%20a%20must.&text=Successful%20documentation%20will%20make%20information,and%20help%20cut%20support%20costs.)的重要性是众所周知的，但是它们可能很耗时，并且许多客户没有兴趣预先支付额外的工作来确保代码在几年后可以有效地运行。抵制这种短视的本能！如果您的代码库存储在一个不可持续的位置(例如，单个开发人员的计算机)，它甚至会消失。您的技术开发人员应该非常熟悉像 [GitHub](https://github.com/) 和 [BitBucket](https://bitbucket.org/product) 这样的工具，这些工具使他们能够在工作时跟踪变更，与队友协作，管理部署，以及保存和更新文档。为了确保项目的持久性，花费在使系统可持续上的额外时间和精力是非常值得的。
8.  **忘记用户体验**
    如果一个系统很难使用或者使用起来很混乱，如果访问太困难，它永远不会被用户广泛采用。除了清晰直观的界面，你还应该确保你设计的系统易于访问，并且容易得到结果。这不应该是事后的想法。在一个组织的整体运营中嵌入一个模型或人工智能系统需要时间、规划和资源。有一个完整的专业致力于 UX/UI ( [用户体验和用户界面](https://www.springboard.com/library/ui-ux-design/job-description/))，你的 AI 和 ML 开发者可能有也可能没有第二专业。值得投资于用户体验——并理解这是与人工智能开发重叠但独立的职业——以确保你的人工智能系统能够为你的组织带来承诺的好处。

人工智能和人工智能已经席卷了许多行业，就像任何其他技术一样，它们可能是有益的，浪费资源的，也可能是有害的，这取决于它们如何被使用。当订购人工智能系统时(无论是从你自己的员工还是承包商那里),你真的不需要知道人工智能如何工作的具体细节，但你需要理解一些关于人工智能的总体概念和危险。释放人工智能给你的组织带来的好处的关键在于很好地使用它——并避免这些致命的错误。

[【1】](#_ftnref1)首先澄清一点:机器学习是人工智能的一种。所以所有的 ML 都是 AI，但不是所有的 AI 都是 ML。在这里和这里看到有用的博客[。](https://blogs.nvidia.com/blog/2016/07/29/whats-difference-artificial-intelligence-machine-learning-deep-learning-ai/)

这一段可能会让许多读者感到惊讶，这一事实是许多人工智能和人工智能项目中困惑和沮丧的来源，并经常驱使我考虑写一篇题为“机器学习不是魔法”的博客或文章。不幸的是，不，机器学习不能凭空变出结果。

[【3】](#_ftnref3)注意，无监督的反馈循环可能会盲目地接受有问题的数据(参见致命错误#5)。更多详情，请参见[这篇文章](/unpackai/feedback-loops-friend-or-foe-81a552203228)或这篇著名的关于社交媒体的[用例](https://theconversation.com/feedback-loops-and-echo-chambers-how-algorithms-amplify-viewpoints-107935)。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)