# 机器学习的实用程序员

> 原文：[`ppml.dev/index.html`](https://ppml.dev/index.html)

## *工程分析与数据科学解决方案*

*Marco Scutari, Mauro Malvestio*

*2023-04-22*

# 前言

通过引用诸如“数据科学家：21 世纪最性感的工作”（哈佛商业评论 2012）或“数据是新石油”（经济学人 2017）这样的名言来提出新想法，已经成为一种陈词滥调，以至于任何听众（无论是商业界还是学术界）都会集体翻白眼表示无奈。而且有充分的理由。同样，我们也不认为放射科医生或卡车司机在可预见的未来会被人工智能取代而失业，我们并不是唯一意识到机器学习局限性的人（经济学人 2020）。

尽管如此，机器学习对我们生活的许多方面产生的影响是难以低估的。它将使用数据和数据分析（在“数据挖掘”、“大数据”等类似术语的旗帜下）来指导商业决策和推动科学发现的现有趋势普及化。机器学习结合了信息理论和统计学数学上的严谨性、计算机科学的计算方面以及优化理论目标驱动的灵活性，重新定义了我们如何处理数据。

尝试提炼众多不同学科的部分内容所带来的另一面，是它们各自文化的冲突，这被 Leo Breiman 在《统计建模：两种文化》一文中总结得很好（Breiman 2001b）。除此之外，工业界和学术界在机器学习实践上存在紧张关系：后者高度重视产生新颖的模型和理论成果，而前者则受制于产生具有商业价值实际结果的需求。在如此多的不同观点中，一个粗略的关于机器学习是什么的共识实际上已经形成！（就个人而言，我们的底线是将深度学习与机器学习混为一谈。深度神经网络之外还有其他生命！）

在这个思想的熔炉中，我们感觉软件工程相对于其他学科所扮演的角色非常小。毕竟，机器学习是一种“允许计算机系统通过经验和数据改进的技术”（Goodfellow, Bengio, and Courville 2016）。因此，人们普遍认为将与计算机系统进行交互，而这反过来又需要通过设计一个与计算机系统通信的软件来实现。这种工程的品质在学术界和工业界都至关重要。在学术界，软件质量问题是“可重复性危机”的潜在原因之一（Nature 2016；Tatman, VanderPlas, and Dane 2018）。在工业界，糟糕的工程会导致实际和计算性能降低（Kang et al. 2021)，技术债务迅速积累（Sculley et al. 2015)，有时甚至导致数百万美元的灾难性失败（.Seven 2014；The Register 2020；VPNOverview 2022；Sherman 2022）。当然，在基础书籍如 *《实用程序员》*（Thomas and Hunt 2019）和 *《软件设计哲学》*（Ousterhout 2018）中，积累了大量关于如何架构和编写软件的智慧。然而，这些书籍是以商业软件为背景编写的，我们发现它们并没有充分捕捉或仅仅触及到成功实施和部署机器学习模型的关键实践。算法分析；将数据与适当的硬件匹配；将数据视为软件的一部分；测试和记录算法及其实现；模块化和构建管道；最后但同样重要的是，命名变量。从我们在学术界和工业界的经验来看，无论是工程软件还是向学生和新人教授软件工程，这些主题往往没有得到应有的重视。我们希望说服本书的读者，任何分析数据的软件的可行性，无论你称之为机器学习、数据科学还是商业分析，都取决于对这些工程实践的深思熟虑。我们并不旨在给出规定性的指导：我们讨论的个别实践在不同环境中可能会有或多或少的关联，并且可以使用各种软件工具来实现。相反，我们希望我们的读者在自己的经验背景下思考我们所写的内容，并找出哪些部分适用，哪些不适用！

本书以对机器学习和软件工程的简要介绍开始，阐述我们如何看待它们以及我们认为它们在实际应用中应该如何互动。其余部分分为四个部分，从基础到实践：

1.  **科学计算基础**：涵盖机器学习软件**规划**、**分析**和**设计**的基础关键主题，例如：使用不同硬件配置的权衡；不同数据类型和合适的数据结构的特征；以及算法分析以确定其计算复杂度。

1.  **机器学习和数据科学最佳实践**：从机器学习工程师的角度回顾软件工程中的最佳实践，从**编写**、**调试**和**部署代码**到生产（即，提供模型）以及**编写技术文档**。

1.  **工具和技术**：讨论**广泛的工具类别**，这些工具塑造了我们思考如何使用机器学习流程可行性的方式，包括从最先进的技术和它们所做出的权衡的例子。

1.  **案例研究**：通过**讨论和原型设计**一个自然语言理解机器学习流程来实施前几章中的建议，该流程基于 Lipizzi 等人（Lipizzi et al. 2022）的工作。

本书的所有材料，包括本书本身，都可以在以下网址在线获取：

> [`ppml.dev`](https://ppml.dev)

并将根据我们了解到的错误和代码问题进行更新。

最后，我们想感谢所有支持我们并使这本书成为可能的人们。首先，是我们的家人，他们忍受了我们长时间的工作。为我们早期书稿提供反馈的同事：文森佐·曼佐尼、法比奥·斯特拉和罗恩·肯内特。还有，最后但同样重要的是，我们的编辑兰迪·科恩，她在本书在新冠疫情期间多次延误期间一直与我们同在。

### 参考文献

Breiman, L. 2001b. “统计建模：两种文化。” *统计科学* 16 (3): 199–231。

Goodfellow, I., Y. Bengio, and A. Courville. 2016\. *深度学习*。麻省理工学院出版社。

Harvard Business Review. 2012\. *数据科学家：21 世纪最性感的工作*。[`hbr.org/2012/10/data-scientist-the-sexiest-job-of-the-21st-century`](https://hbr.org/2012/10/data-scientist-the-sexiest-job-of-the-21st-century)。

Kang, S., R. Jin, X. Deng, and R. S. Kenett. 2021\. “网络制造建模和分析的挑战：从机器学习和计算视角的综述。” *智能制造杂志* 在线首版。

Lipizzi, C., H. Behrooz, M. Dressman, A. G. Vishwakumar, and K. Batra. 2022\. “获取研究：创造信息变化的协同效应。” 在 *第 19 届年度获取研究研讨会论文集* 中，第 242–55 页。

Nature. 2016\. “关于可重复性的现实检查。” *自然* 533 (437)。

Ousterhout, J. 2018\. *《软件设计的哲学》*. Yaknyam 出版社.

Sculley, D., G. Holt, D. Golovin, E. Davydov, T. Phillips, D. Ebner, V. Chaudhary, M. Young, J.-F. Crespo, and D. Dennison. 2015\. “机器学习系统中的隐藏技术债务。” 在 *第 28 届国际神经网络信息处理系统会议（NIPS）论文集*，第 2 卷，第 2503-2511 页。

.Seven, D. 2014\. *《噩梦：DevOps 的警示故事》*。[`dougseven.com/2014/04/17/knightmare-a-devops-cautionary-tale/`](https://dougseven.com/2014/04/17/knightmare-a-devops-cautionary-tale/).

Sherman, E. 2022\. *Zillow 的失败算法对数据科学未来的影响*。[`fortune.com/education/business/articles/2022/02/01/what-zillows-failed-algorithm-means-for-the-future-of-data-science/`](https://fortune.com/education/business/articles/2022/02/01/what-zillows-failed-algorithm-means-for-the-future-of-data-science/).

Tatman, R., J. VanderPlas, and S. Dane. 2018\. “机器学习研究可重复性的实用分类。” 在 *2018 年 ICML 机器学习可重复性研讨会论文集*。

The Economist. 2017\. *世界最有价值的资源不再是石油，而是数据*。[`www.economist.com/leaders/2017/05/06/the-worlds-most-valuable-resource-is-no-longer-oil-but-data`](https://www.economist.com/leaders/2017/05/06/the-worlds-most-valuable-resource-is-no-longer-oil-but-data).

The Economist. 2020\. *对人工智能局限性的理解开始深入人心*。[`www.economist.com/technology-quarterly/2020/06/11/an-understanding-of-ais-limitations-is-starting-to-sink-in`](https://www.economist.com/technology-quarterly/2020/06/11/an-understanding-of-ais-limitations-is-starting-to-sink-in).

The Register. 2020\. *Twilio：有人潜入我们的未受保护 AWS S3 存储库，向我们的客户 JavaScript SDK 添加了可疑代码*。[`www.theregister.com/2020/07/21/twilio_javascript_sdk_code_injection`](https://www.theregister.com/2020/07/21/twilio_javascript_sdk_code_injection).

Thomas, D., and A. Hunt. 2019\. *《实用程序员：你的精通之旅》*. 周年纪念版. Addison-Wesley.

VPNOverview. 2022\. *金融科技应用切换泄露用户交易和个人身份信息*。[`vpnoverview.com/news/fintech-app-switch-leaks-users-transactions-personal-ids`](https://vpnoverview.com/news/fintech-app-switch-leaks-users-transactions-personal-ids).
