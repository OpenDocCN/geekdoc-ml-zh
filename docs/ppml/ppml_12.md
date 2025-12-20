# 第六章 编写机器学习代码

> 原文：[`ppml.dev/writing-code.html`](https://ppml.dev/writing-code.html)

编程在很多方面是和计算机的对话，但也是和其他开发者的对话（Fowler 2018）。尽管听起来很模糊，我们应努力编写易于阅读且含义明显的代码（Ousterhout 2018）。代码的阅读次数远多于编写次数：软件的大部分成本在于维护，这通常由最初编写代码的人之外的人来完成。

实现清晰度需要在多个方面付出努力。在清晰度、一致性、开发速度以及有用库的存在之间进行不同的权衡，可能会促使人们为不同的模块选择特定的编程语言（见第 6.1 节）。事物应该被适当地命名（见第 6.2 节），代码应该格式化和布局一致（见第 6.3 节），函数和模块应该在文件和目录中整洁地组织（见第 6.4 节）。

最后，让多个人审查代码并对其进行*审查*（见第 6.6 节）有助于确定如何改进它。然后我们可以通过*重构*它（见第 6.7 节）来逐渐改变它，这是确保我们不引入任何新错误的最安全方式。这两个活动都需要有效地使用源*版本控制*（见第 6.5 节），这对于部署（见第七章）、文档化（见第八章）和测试（见第九章）我们的机器学习流程也将至关重要。作为一个例子，我们将重构用于学术教学中的一段代码样本（见第 6.8 节）。

## 6.1 选择语言和库

选择用于编写机器学习软件的编程语言主要取决于它们的性能、可观察性、可用库的功能以及编程的易用性。

程序语言的性能主要取决于它是否是**编译**的（例如 C 和 Rust）还是**解释**的（例如 R 和 Python）。编译过程会将程序转换成存储在二进制可执行文件或库中的机器指令，这些指令可以反复运行。编译后的代码通常性能较高，因为它在运行时不需要进一步处理：在运行前就已经完成了寻找最有效机器指令序列的所有工作。这包括决定在程序运行的系统上，哪些指令适合利用 CPU、GPU 和 TPU。相比之下，解释型语言通过在运行时将程序翻译成机器指令来执行程序。因此，解释型代码不一定表现出高性能，但通常是高级的（从硬件具体细节抽象出来，例如管理内存）且更容易编程，因为我们可以在 REPLs 中与之交互。¹⁴ 实际上，用于机器学习的编程语言存在于这两种极端之间的光谱上。尽管 R 和 Python 是解释型语言，但它们都有包，这些包只是围绕用编译代码编写的 BLAS、LAPACK、TensorFlow 或 Torch 等高性能库的薄包装。根据我们在机器学习代码中使用的包，我们可能实现与编译代码相当的性能，而不会牺牲代码中非计算密集部分编程的易用性。另一方面，Julia 使用**即时编译**来在运行时调用每个模块或函数之前编译和优化代码。因此，启动执行 Julia 代码所需的时间相当慢，但一旦运行，开销就很小。

编译型和解释型语言在可观察性方面也有很大差异。我们可以通过分析（在固定时间间隔记录相关指标）或通过跟踪（在记录特定事件时记录程序和计算系统状态）来轻松观察编译代码的行为，因为每次执行时它都运行相同的指令序列。另一方面，解释型代码在软件运行时由解释器动态地映射到机器指令。除非解释器可以在运行程序的同时将其内部状态暴露给分析器，否则将性能映射到特定的代码块更困难。因此，解释型代码通常通过简单地添加打印语句和时间戳来研究。一种更严格的方法是对代码本身进行*仪器化*，即要求解释器在预定的时间间隔或事件记录其状态。然而，大多数类型的仪器化会显著增加执行时间，并且即使用于调试也难以使用。例如，R 的`Rprof()`和`Rprofmem()`就是这种情况。

在编程的易用性方面，所有常用的编译型语言都是*低级语言*：我们用它们编写的代码没有从将要运行的计算系统抽象出来。手动内存管理、依赖关系管理、对数据结构实现细节的重视（第三章）、为利用特定硬件能力而结构化代码（第二章）是使用 C 或.等语言时日常关注的焦点。相比之下，常用的解释型语言是*高级语言*。它们允许我们编写在许多方面类似于伪代码的代码，并且可以更多地关注我们正在实现的模型和算法。因此，它们使得跟踪整体设计和机器学习管道的结构变得更加容易。R、Python 和 Julia 等高级语言还附带软件包存储库和依赖关系管理（CRAN Team 2022；Python Software Foundation 2022a；JuliaLang 2022）。再次强调，这表明最佳权衡是在机器学习管道中性能关键的部分使用低级、编译型语言，而在其他所有方面使用高级语言。前者将包括模型训练和推理；后者可能包括数据清理、可视化和性能监控。根据其规模和复杂性，管道的不同部分可能或可能不是性能关键。

最后，我们能够构建的库的可用性同样重要。理想情况下，我们希望将精力集中在实现、优化和运行我们的机器学习系统和管道上，而不是重新实现其他地方已经存在的功能。即使我们愿意重新发明轮子，我们也很难达到大多数流行软件库的设计质量和性能优化水平。在多种语言中可用的机器学习模型之间存在显著的重叠，但在某些特定情况下，某些实现可能比其他的好。Python 可能是神经网络、概率编程以及计算机视觉和自然语言处理应用的最佳选择。R 拥有从经典到现代统计学最广泛的选择，包括流行模型的参考实现，如混合效应模型和弹性网络惩罚回归。在幕后，这两种语言（以及 Julia）使用相同的标准数值库，因此它们通常具有相似的性能水平。

最后但同样重要的是，再次考虑第 5.3 节中关于机器学习软件模块化性质的讨论。当我们的软件模块具有明确定义的接口，指定它们的输入和输出时，并且输入和输出都使用标准格式进行序列化，我们就可以用不同的语言实现它们。模型训练和推理模块（第 5.3.4 节和 5.3.5 节）计算量更大，因此应该用编译语言如 C 或实现。不需要太多资源如用户界面、仪表板（第 5.3.6 节）以及通常的数据摄取（第 5.3.3 节）的模块可能可以用解释性语言如 R 或 Python 实现。编排、模型部署和服务（第 5.3.5 节）、日志记录和监控（第 5.3.6 节）通常由第三方软件提供；任何补充它们的粘合代码可能位于完全无关的系统或脚本语言中。模块之间的隔离，以及模块与底层计算系统之间的隔离，使得每个模块内部使用的编程语言的选择对其他模块无关紧要。然而，编程语言和模块结构的某种程度的一致性是可取的，以便不同的人更容易共同工作（第 5.2.4 节）。

## 6.2 命名事物

仔细命名变量、函数、模型和模块对于将它们的含义传达给阅读代码的其他人至关重要（Ousterhout 2018；Thomas 和 Hunt 2019）。但在机器学习软件的情况下，这些人是谁呢？他们将是*最终用户*、*开发者*、*机器学习专家*和*领域专家*的组合。每个群体都会对对他们有意义的名称有不同的看法。同样，我们将在第八章中论证，我们应该用来自不同视角编写的文档来补充软件，以确保所有参与其中的人都能很好地理解它。

对用户和领域专家最有用的名称描述了一个函数应该做什么，一个变量包含什么或应该如何使用，一个模块实现了哪种模型，等等。他们可以通过利用软件所使用的领域的命名约定来实现这一点。这些名称并不描述函数内部的工作方式，变量的类型或其他实现细节：大多数用户和领域专家可能不是开发者，因此这类信息对他们来说可能没有用。他们主要会对使用函数、模块等为自己目的而感兴趣，而不必理解他们调用的每一块代码的实现。这样做会增加与任何复杂软件一起工作的认知负荷，超出了合理的范围。出于同样的原因，我们建议在伪代码（第 4.1 节）中使用来自领域的名称。

描述它们所指向的实现细节的名称对正在同一模块上工作的其他开发者可能很有用。同样，直接映射到科学文献中使用的数学符号的简短名称对机器学习专家来说将最有用。这两种类型的名称都假设读者熟悉相关模型和算法的数学和实现细节，并假设阅读代码的人会参考文献来理解代码的功能以及为什么以这种方式实现。这类名称通常非常简短，使得代码更加简洁。用户和领域专家可能不熟悉这种符号，他们可能会发现没有大量的努力和大量注释的帮助，这样的代码难以理解（见第 8.1 节）。另一方面，如果命名约定与文献中的一致，那么熟悉数学符号的人可以更快地理解代码。这在编写仅将在研究相似主题的协作者之间共享的研究代码时是有利的。然而，使用数学符号也可能导致误解，因为相同的概念用不同的符号表达，反之亦然，相同的符号在不同的机器学习子领域中用来表示非常不同的概念。

因此，在实践中，在整个机器学习流程中建立单一合适的命名约定是不可能的：其中的代码种类繁多，与之交互的人员也各不相同。（这一点在下一节中我们将看到，对于任何类型的编码约定来说都是如此。）然而，Kernigham 和 Pike（Kernigham and Pike 1999）的一般性指南即使在命名约定方面也适用。*为全局变量使用描述性名称，为局部变量使用简短名称*：在实现机器学习模型和算法的模块中遵循数学符号可能是可以的，因为只有开发人员和机器学习专家可能会接触这样的代码。模块的作用域及其包含的注释将缩小上下文（第 8.1 节）并使简短名称与较长名称一样易于理解（但阅读速度更快）。另一方面，可以从模块外部访问的变量和函数，最好根据它们的领域意义来命名，因为它们很可能会被最终用户和领域专家使用。公共接口文档（第 8.2 节）有助于阐述它们与模型和数据之间的关系，并扩展它们的意义。*保持一致性*：机器学习流程中所有模块中的相同类型的代码应遵循相同的命名约定，实践在注释、接口和架构文档（第 8.3 节）中使用的相同通用语言，或者在技术文档（第 8.4 节）中建立的相同数学符号。*保持准确性*：避免使用模糊的名称和可能被不同背景的人误解为不同含义的名称。¹⁵

## 6.3 编码风格和编码规范

代码清晰度也是其可读性的一个函数。在低层次上，我们可以通过采用*代码风格*来提高可读性，这些风格标准化了代码的格式（缩进、括号的使用、命名大小写、行长度等），并在整个机器学习软件中赋予其统一的外观。其理念是，持续使用相同的风格使得代码更容易被编写者和他人阅读和理解。因此，遵守代码风格可以降低出错的风险，并使得开发团队内部以及跨团队之间的协作更加容易。在机器学习软件中普遍使用的所有编程语言，包括 Python（van Rossum、Warsaw 和 Coghlan 2001；Google 2022d）、R（Wickham 2022b）和 Julia（Bezanson 等人 2022）都有行业标准的代码风格，这些风格在此背景下适用得很好。然而，机器学习管道将包含用不同编程语言编写的代码（第 6.1 节）：我们可能需要考虑对这些风格进行一些小的调整，使它们更加相似，并在同时使用多种语言时减少摩擦。

在更高层次上，我们可能希望采用*代码标准*，这些标准限制了哪些编程结构被认为是安全的，并规定了在局部级别（例如，函数内的块或模块内的函数）结构代码的最佳实践。这些标准是语言无关的，并且是补充而不是取代代码风格：例如，它们可能描述如何处理异常，如何在函数和模块级别组织输入和输出，如何跟踪软件依赖项，如何为日志记录和可观察性对代码进行配置，以及出于性能原因应避免哪些代码模式。Lopes（Lopes 2020）展示了这些选择在实践中可以产生多大的差异。在更高的层次上，代码标准也可能解决软件安全问题。与代码风格不同，没有普遍适用的代码标准：它们的广泛性使得它们必然是应用或领域特定的。结合模块化管道设计（第 5.3 节），我们可以对代码的行为做出假设，这反过来使得代码更容易阅读、部署、维护，并与其他代码集成，通过减少重构的需要（第 6.7 节）和使代码更容易测试（第 9.4 节）来实现。它们可以通过自动化工具系统地采用，这些工具用于检查合规性，并在代码审查（第 6.6 节）期间强制执行。

在撰写本文时，采用代码风格和标准是全面提高机器学习软件质量的一个低垂的果实。Jupyter 笔记本（Project Jupyter 2022）作为开发平台的使用普及，鼓励编写一次性的代码，这些代码不需要遵循任何特定的约定，因为它们不与其他软件交互，并且与用户的交互非常有限。因此，Jupyter 笔记本中的代码没有很好地组织成函数（与普通软件相比，它们的耦合度高出 1.5 倍，尽管它们各自更简单），其依赖关系没有得到良好的管理（未声明的、间接的或未使用的导入数量是两倍），总的来说，代码的质量问题更多（1.3 倍）(Grotov 等人 2022)。即使不考虑 Jupyter 笔记本，对所有开源机器学习代码的系统分析都发现了重大且普遍存在的问题。在控制年龄和流行度之后，机器学习软件的复杂性和开放工单与其他类型的软件相似。然而，单个项目似乎有更少的贡献者和更多的分支，这表明代码可能没有得到彻底的审查（Simmons 等人 2020)。可重复性和可维护性存在问题，因为软件依赖项通常没有得到适当的跟踪（van Oort 等人 2021)：要么它们没有被列出，要么它们是供应商的（第 5.2.4 节），它们的版本没有被固定，或者因为它们被自动检测出来而从未经过审查，所以无法解决。Pylint 无法可靠地检查本地导入和具有 C/后端（即所有基础包，包括 TensorFlow、NumPy 和 PyTorch）的包中的导入，这使得 Python 项目的问题更加严重。此外，用户通常对自己的机器学习软件中记录的问题和陷阱一无所知（张、克鲁斯和 van Deursen 2022)，部分原因是因为如果它们是库特定的，这些问题和陷阱只会在独立的博客文章中报告。

这些一般性问题由于几个特定于机器学习代码的“坏味道”而变得更加严重，这些“坏味道”源于此类代码的开发方式。我们引用的许多来源（Sculley 等人 2015；Simmons 等人 2020；van Oort 等人 2021；张、Cruz 和 van Deursen 2022；Tang 等人 2021）指出模块接口和函数具有过多参数的问题（因为它们与底层模型的数学符号映射过于紧密）；重复代码（由于剪切粘贴的实验和未剪枝的无效代码）；函数过长，变量和分支过多（因为它们执行多个任务，从未重构为更小的函数）；以及缺乏配置管理（例如，我们在第五章中提出的实验跟踪和基础设施即代码方法）。其中一些问题可能因为机器学习代码的固有特性而可以容忍：我们之前（第 6.2 节）认为，即使名称不具有描述性，将局部变量命名为数学符号也是可以的。然而，大多数问题不应该如此。为了公平起见，我们承认许多这些问题不能仅仅从技术层面解决，因为它们源于错误的激励。在学术界，代码被视为一次性丢弃的物品（Nature 2016；Tatman、VanderPlas 和 Dane 2018），因为工作表现是通过发表的论文数量来衡量的（“发表或灭亡！”），而不是代码本身的质量。因此产生的软件通常既没有得到维护，也没有部署到生产系统中。在工业界，许多从事机器学习管道工作的专业人士在软件工程方面几乎没有或没有背景（Sculley 等人 2015），并且公司已经接受从头开始重新实现机器学习代码以用于生产的做法是不可避免的。为了采用最佳实践，如代码风格和代码标准（以及模块化管道设计）成为常态，需要文化上的变革。

## 6.4 文件系统结构

将代码组织到文件和目录中，有助于提高清晰度，因为它使得查找任何特定的代码片段变得更加容易。这对于机器学习管道和其他类型的软件都适用：执行相关任务的函数应存储在一起，执行正交任务的函数应存储在文件系统的不同部分。（这是对文件层次结构的单一责任原则（Thomas 和 Hunt 2019）的应用。）每个模块应存储在单独的目录中，将功能合理地分割到文件中。从模块导出的方法和变量应存储在与内部代码分开的文件集中，以便用户更容易检查它们，并将它们与接口文档（第 8.2 节）链接起来。模块的单元测试（第 9.4.4 节）应放置在单独的子目录中，但与它们测试的代码版本一致。

对于机器学习管道中的模块，最佳文件系统结构是什么？没有单一、通用的标准：既有语言无关（Kriasoft 2016）的提议，也有针对 Python（Greenfeld 2022; Alam 等 2022）、R（Blagotic 等 2021）和 Go（Quest 2022）的语言特定提议，它们已在现实世界的软件中使用。它们在很大程度上重叠，广泛同意以下子目录和文件集：

+   用于模块源代码的`src`目录，可能进一步细分为子目录。

+   用于存储构建过程中创建的工件（如目标文件、机器学习模型和用于测试、部署和 CI/CD 的文件）的`build`或`dist`目录。

+   用于 CI/CD 中使用的任何容器的规范文件目录，例如，用于 Dockerfile 的`docker`（Docker 2022a）。控制容器部署和管理配置的进一步配置文件，如 Kubernetes（The Kubernetes Authors 2022a）YAML 配置，可以为了方便而放置在同一目录中。

+   包含构建和开发模块所需的配置文件`config`目录，包括版本化的软件依赖项的完整列表（例如，Python 模块的`requirements.txt`）和 IDE 设置。

+   用于单元测试及其参考输出的`test`目录。

+   包含模块文档的`docs`目录，文档可以是源码形式或最终形式。接口文档可以与所引用的代码存储在一起，如第 8.2 节所述，作为替代方案。

+   用于存储第三方代码和构建模块所需的软件工具的`vendor`目录。

+   用于从`src`构建的可执行文件的`tools`目录。

+   一个`examples`目录，用于存储示例使用模式和描述算法和领域知识的其他文档，例如在第 8.4 和 8.5 节中讨论的那些。通常以 Jupyter 笔记本的形式存在。

+   一个`.secrets`目录，用于存储凭证、证书、身份验证令牌和其他应加密存储（例如，使用`git-crypt`（Ayer 2022））的特权信息。

+   生成工件（存储在`build`目录中）并运行测试（在`test`目录中）的构建系统的配置文件。例如，一个`.Makefile`文件。

+   一个包含模块简短描述的`README`文件。

+   一个包含版权声明和许可文本的`LICENSE`文件，如果模块可以作为独立的软件组件分发。

考虑这些目录和文件应该如何存储在源版本控制系统（第 6.5 节）中也是很有趣的。一方面，我们可以遵循谷歌的“单仓库”（monorepo）方法（Potvin 和 Levenberg 2016），将所有这些（整个管道的代码）存储在一个单一的仓库中。这种选择提供了统一的版本控制，具有单一的真实来源，简化了依赖管理，促进了代码重用和跨多个模块的大规模重构，并通过使不同开发团队之间的协作更容易，增加了代码的可见性。集成、系统测试和验收测试（第 9.4.4 节）的实现和运行也变得更加直接。然而，由于仓库的大小，单仓库需要更多的硬件资源和高品质的工具来导航代码、修改它并保持其组织。

另一方面，我们可以将每个模块存储在单独的仓库中。跨模块的代码和配置存储在单独的“父”仓库中，这些仓库通过使用如 git-repo（谷歌 2022e）或 meta git（沃尔特斯和李·斯科特 2021）等工具来实现编排和部署“子”仓库，这些“子”仓库用于模块。换句话说，这些“父”仓库会克隆、设置和管理“子”仓库（例如，使用`docker-compose`），以产生使用单仓库工作的错觉。单个“子”仓库将会更小，需要的硬件资源更少，并且对单个模块的工作不需要任何特定的工具。然而，跟踪模块之间的依赖关系以及在整个管道中保持第三方软件依赖的一致性，并不能像在单仓库中那样容易自动化：这是技术债务（第 5.2 节）的一个重要来源，我们应该在“父”仓库中手动解决。导航整个管道的代码库需要额外的工具来隐藏仓库之间的边界，并给出统一仓库的外观。任何跨越多个模块的任务不再是原子的：在模块之间移动代码、拆分或合并模块，或者更改模块的接口以及在其他模块中使用该接口的所有地方，不能再作为一个单一提交在单个仓库中执行。同样，我们现在需要创建和维护“父”仓库来设置运行集成和系统测试的环境。与许多其他设计选择一样，没有最佳解决方案，只有不同权衡的选择：哪个最适合特定的管道将取决于它的大小、包含的模块数量以及模型的训练和部署方式。

## 6.5 有效的版本控制

将代码存储在版本控制系统（简称“版本控制”）已成为软件工程中的标准做法（Duvall, Matyas, 和 Glover 2007; Fowler 2018)，这对机器学习管道的好处与传统软件一样。我们可以跟踪代码随时间的变化，导航其历史并回滚到功能状态，如果它出现故障。我们还可以跟踪数据、模型和管道配置，正如第 5.2.3 节中讨论的那样。多个开发者可以同时工作在代码上，合并他们的更改，借助专用工具解决可能出现的任何冲突，并发布带有语义版本控制方案的标签（Preston-Werner 2022）。版本控制还确保所有对代码的更改都被跟踪（以确保代码完整性和开发者责任）并通过将它们附加到只读的提交账本中来应用（以获得不可变版本和快照）。因此，版本控制为我们提供了代码的“单一真相来源”，这使 MLOps（第 5.3 节）的自动化工作流程、持续部署（第七章）、软件测试（第 9.4 节）和重构（第 6.7 节）成为可能。

在处理机器学习管道时，我们如何才能最大限度地利用版本控制？现代软件工程中的两种实践特别相关。首先，*尽可能缩小开发和生产代码之间的差距*（通常称为“dev-prod parity”（Wiggins 2017）），以最大限度地利用 CI/CD 开发工作流程（第 5.3 节）。通过引入*小而自包含的提交集*中的更改，使它们易于审查（第 6.6 节），易于进行持续集成测试（因为只有一小部分测试是相关的），并且使得它们能够非常频繁地合并到主线分支（例如，每天）。结果，代码的更改可以立即对所有开发者可见，使他们能够有效地协作。将代码划分为存储在单独目录中的模块，并将实现不同功能的函数存储在单独的文件中（第 6.4 节），可以大大降低冲突的可能性：任何两个在不同功能上工作的开发者不太可能修改相同的文件。然而，它不能完全防止由于管道各部分行为变化而可能出现的更高级别问题，如修正级联（第 5.2.2 和 9.1.2 节）。减少冲突并及早发现此类问题的最佳方法是*仅使用短期分支*，这些分支立即合并到从中提取生产发布的主线分支。不完整的更改应隐藏在*功能标志*后面，这些标志阻止新代码默认运行，并且可以使用环境变量轻松切换。换句话说：

1.  将我们想要更改的现有代码放在控制其是否运行的功能标志后面，开启标志以保持代码运行。

1.  在相同的标志下引入新代码，配置它当标志关闭时运行。

1.  使用标志关闭的情况下，用现有的单元、集成和系统测试来测试机器学习软件，检查是否存在回归以及新代码是否比现有代码有所改进。

1.  如果新代码适合，则移除现有代码和功能标志。当标志变得过时，有工具可以自动完成这项工作（Uber 2022）。

这种实践被称为“主干分支开发”（Hammant 2020)（“主干”是主线分支的传统名称，与“master”类似）。在机器学习软件的情况下，我们应该将这种方法扩展到数据和模型。将数据和模型与代码一起进行版本控制对于通过允许实验跟踪和可重复的模型训练来减少技术债务（第 5.1 节）至关重要。它还使我们能够在非平凡设置中构建基于属性的测试，通过允许我们匹配模型、它们的输入和输出（第 9.4.2 节）。同样，处理管道问题并将它回滚到已知的良好版本（第 7.6 节）在更新失败时也成为可能。

其次，编写信息丰富且遵循既定惯例的提交信息非常重要：Linux 内核（Linux 内核组织 2022）和 Git（Git 开发团队 2022）是这方面做得很好的例子。提交信息应提供足够的信息，以便理解所描述的更改，包括*更改了什么*，*为什么进行这些更改*以及*为什么以这种方式（而不是如何）进行更改*（Tian 等人 2022）。非平凡代码更改通常跨越多个文件，通常没有单一的地方可以放置解释其理由的注释。在所有修改的地方重复该注释会增加陈旧注释的可能性（第 8.1 节）。因为我们必须记住每次重新访问我们更改的代码时，必须一次性更新所有这些注释的副本。这种信息的自然位置是在提交信息中，因为提交引用了所有更改的文件（Ousterhout 2018）。在任何长期运行的代码库中，提交信息可能是未来开发者理解原始开发者离开后代码更改的唯一信息来源。如果我们实践基于主干的开发，我们可以将我们短期开发分支中的提交合并在一起，并且只有在将代码合并到主线分支时才编写有意义的提交信息。此外，我们应该写一个简短的标题来总结更改（例如，50-60 个字符），然后是更详细的描述。导航代码的历史将变得更加容易，因为我们现在可以浏览提交标题，并且只为与我们相关的提交阅读详细的提交信息。如果我们使用现代代码审查实践（第 6.6 节），我们还可能能够阅读审查该提交的开发者的评论：当代码合并时，所有当前版本控制系统都会通过链接或包含在提交信息中。最后，我们可能还想包括结构化信息：执行代码审查的开发者的签名行，标识提交为系列一部分的标签，票据编号及其状态。所有这些信息都可以由 CI/CD 工具处理，以自动化提交中的代码合并和部署。作为参考，Tian 等人（Tian 等人 2022）详细讨论了“良好”提交信息的特征及其对不同类型提交的内容。

## 6.6 代码审查

代码质量对于机器学习管道的有效性至关重要：编码风格和标准（第 6.3 节）、版本控制（第 6.5 节）、重构（第 6.7 节）、测试（第 9.4 节）、MLOps（第 5.3 节）和持续部署（第七章）都是为了最大限度地减少缺陷数量。由于数据、模型和代码之间的相互作用以及它们的可变性质（第 9.1 节和第 9.2 节），技术债务的风险增加（第 5.2 节），这使得代码质量变得更加重要。

然而，本书中描述的实践和自动化工作流程本身并不足够：虽然它们可以显著减少缺陷的数量，但有些问题只能由开发者自己发现和解决。这就是*代码审查*（Rigby 和 Bird 2013）的原因。除了编写特定代码的开发者之外的其他开发者应该检查它，并共同努力确保：

+   它实现了所需的功能。

+   它既高效又伴随着软件测试。

+   它遵循编码风格、编码标准和命名约定的精神和文字。

+   它组织良好，文档齐全。

利益很多：

+   我们确保每个开发者编写的代码其他开发者都能理解。

+   交换建设性的批评是教授初级和未来开发者的一种有价值的方式。

+   在机器学习管道上工作的更多人将对其设计有实际的理解，这使得找到改进它的方法的可能性更大。

+   我们鼓励对代码有一种集体所有权的感受。

显然，每个模块都将有一个主要的“所有者”，他最终负责它并控制哪些更改被合并到主线分支。这位开发者将是该模块更改的理想审查者，因为他将是了解其代码和设计最好的人。然而，其他人应该感到舒适地为其做出贡献、修复它以及提供关于代码质量和设计的反馈。同时，没有人应该在未经监督的情况下提交代码，这正是代码审查所提供的。

代码审查通常以两种互补的方式进行：

+   利用*代码审查工具*（Toro 2020; Sadowski 等人 2018）：提出代码更改的开发者准备一个提交，并将其提交给某个软件工具进行测试，然后将其分配给一个或多个审查员。审查本身是异步的，性质上是非正式的，开发者和审查员通过该工具交换评论并完善代码，直到他们对提交的质量满意为止。然后，该工具将提交合并到主线分支，将提交信息中的评论链接起来。

+   在开发软件时实践*结对*（*团队*）*编程*（Popescu 2019; Swoboda 2021）：两位（或更多）开发者一起编写、调试或探索代码。其中一位开发者（“驾驶员”）负责实现，专注于编写高质量且无错误的代码。其他开发者（“导航员”）则关注问题的更广泛范围，并确保过程按计划进行。在实际操作中，导航员在代码编写过程中充当“现场”审查员。在相当短的时间间隔（例如，30 分钟）后，当前的“驾驶员”提交其正在工作的代码，并将角色传递给另一位开发者，该开发者将拉取代码并成为下一个“导航员”。

这两种方法都鼓励编写小的增量更改并频繁提交，就像在基于仓库的开发（第 6.5 节）中一样：很难找到对机器学习管道较大部分有深入了解的资深审查员，而且审查员找到时间审查大量代码更困难。理想情况下，要审查的代码应解决单个问题并完全解决，只涉及一到两位审查员。这样，如果出现问题，更容易确定错误引入的位置，并且只需回滚有问题的更改。

在基于工具的代码审查环境中，编写代码的开发者应首先进行个人代码审查，以免浪费审查员的时间。在将代码发送进行审查之前，让代码自动通过 linters、静态代码分析器和我们的软件测试套件进行测试，也将加快代码审查的迭代速度：审查员将看到他们的输出，以帮助检查提交。同样，出于同样的原因，开发者应在代码中添加注释（第 8.1 节）并编写描述性的提交信息（第 6.5 节），说明提出更改的原因、可能的影响以及任何相关的设计决策。

在结对和集体编程中，反复轮换“驾驶员”和“领航员”的角色有效地确保了代码的审查，并有助于让更多开发者参与到代码中来。领域专家也可以参与其中：即使他们对编程只有边缘的了解，当他们作为“驾驶员”时，也可以由开发者进行指导；当他们作为“领航员”时，他们可以将其知识贡献给编写代码的开发者。然而，只有当开发环境可以快速设置，并且拉取和推送代码毫不费力时，这种方法才能顺利运行：频繁而顺畅的角色转换对于保持每个人的参与和相互讨论至关重要，这正是这种方法的主要目的。特别是对于特别困难的编码任务，有更多的眼睛查看问题并协作代码的底层和高层设计，将带来最大的好处。

两种代码审查方法都需要付出努力和初始投资来建立为标准实践，但它们将通过提高开发者的生产力来回报自己。而且，也许与其他实践不同，绝大多数程序员都喜欢它们（Sadowski 等人 2018；Williams、Kessler 和 Cunningham 2000）！基于工具的审查流程需要适当的工具来维护和扩展。结对和集体编程要求开发者进行协调，并花费时间一起在相同的代码片段上工作。但这并不意味着参与其中的人会降低生产力。

在基于工具的代码审查情况下，一个或最多两个开发者就足够审查一个提交，如果提交只涉及一个或两个文件，审查者可以很容易地在几小时或最多一天内提供反馈（Sadowski 等人 2018；Rigby 和 Bird 2013）。随着时间的推移，开发者将产生越来越好的代码，从而加快审查速度并减少每个提交的注释数量。错误和架构问题将很快被发现，因此它们将更容易和更快地修复（Tornhill 和 Borg 2022）。因此，我们将减少对大规模重构和直接代码重写的需求，从而有更多时间编写更好的代码、测试和文档。（根据定义，这意味着随着时间的推移，生产力将提高，因为我们将更快地取得进展而不是原地打转。）此外，高级开发者将在审查不同模块的代码时拓宽对机器学习管道架构的理解。此外，审查补丁对于审查者来说不必耗时：在谷歌，开发者每周审查大约 4 个提交，平均花费 2.6 小时（中位数），每个提交大约花费 40 分钟（Sadowski 等人 2018）；在微软，开发者平均每天花费 20 分钟（每周 1.6 小时）进行代码审查（Jacek 等人 2018）。

我们可以对结对编程和集体编程做出类似的考虑：在过去 30 年中进行的几项研究（Williams、Kessler 和 Cunningham 2000；de Lima Salge 和 Berente 2016；Shiraishi 等人 2019），包括一些关于机器学习软件和数据科学应用的研究（Saltz 和 Shamshurin 2017），发现它们可以提高生产力和代码质量。为了使它们最有效，我们需要足够复杂以吸引多个人注意的任务（琐碎的任务几乎没有错误空间）以及足够的经验，以便在结对（无论是高级和初级开发者，还是两个“中级”开发者）或集体（Arisholm 等人 2007；Popescu 2019）中有效地处理它们。

## 6.7 代码重构

正式来说，*重构*是指在不改变代码外部行为的前提下，改变代码的方式，从而改善其内部结构，澄清其意图和假设的过程（Fowler 2018）。遵循第 6.5 节，我们通过一系列小增量更改来实现这一目标，每个更改都通过运行我们的测试套件（第 9.4 章节所述）和持续集成工具进行单独验证。在过程结束时，我们可以将所有提交合并在一起，并提交给审查（第 6.6 章节所述），就像我们对其他代码更改所做的那样。我们在添加新功能、改变现有代码的设计以适应它时进行重构。我们在攻击错误时进行重构，既是为了修复它们，也是为了适应测试它们（并确保它们保持修复状态）。我们重构以提高对命名约定（第 6.2 节）、编码风格和编码标准的遵守程度。重构可以让我们有信心从正确的代码开始每个提交，这使得跟踪我们可能引入的任何错误变得容易，并且代码不会花费太多时间（如果有的话）处于损坏状态。

佛勒（Fowler 2018）提供了一套详尽的重构方法目录。根据编程语言的不同，其中一些可以自动化：例如，PyCharm（JetBrains 2022b）和 Visual Studio Code（Microsoft 2022i）都为 Python 代码提供了一个“重构”按钮。（这是在选择编程语言时，除了我们在第 6.1 节中讨论的因素外，我们可能还需要考虑的另一个因素。）只有少数几种被广泛用于机器学习代码，而且还有一些专门针对机器学习代码的重构方法：唐等人（Tang et al. 2021）通过对机器学习软件的大规模调查构建了这两种方法的分类。机器学习代码只是典型流程中的一小部分，因此掌握佛勒（Fowler 2018）提出的方法对于解决我们在第 6.3 节中讨论的代码异味仍然很有价值。另一方面，专门针对机器学习代码的重构方法可以控制我们在第 5.2.4 节中涵盖的各种技术债务。唐等人（Tang et al. 2021）特别指出三点：使用继承来减少重复的配置和模型代码；改变变量类型和数据结构以允许性能优化（见 3.3 和 3.4 节）；以及隐藏原始模型参数和超参数，并暴露具有领域意义的自定义类型，以实现训练和推理在一侧与通用元启发式和领域规则在另一侧之间的更好分离。

然而，还有一个额外的观点使得实现机器学习模型的代码在重构方面本质上与其他代码不同：我们无法像处理其他代码那样轻松地在重构过程中对其进行切割和分解。一些模型执行单一任务（例如，平滑或预测）并且与其他代码很好地组合，但其他模型是黑盒，它们以使它们无法分割的方式集成多个任务（例如，特征提取和预测）。深度神经网络是这一点的典型例子。即使我们可以将模型及其相关代码重构为良好的子模型，也不能保证我们可以像我们希望的那样更改它们。每个子模型的概率属性是从我们开始的模型继承下来的：我们应该确保我们引入的任何新子模型的概率属性与其他模型兼容。未能做到这一点将产生具有难以诊断和无法纠正的偏差的输出，因为它们缺乏我们通常视为理所当然的数学属性。（对于在现有管道中交换整个模型也是如此。）一个可能明显的例子：我们应该将使用二次损失函数的模型（如大多数线性回归）与在方差和线性相关性上工作的特征选择和提取以及使用相同的二次损失函数在验证集上评估模型的模型选择策略相匹配。如果我们以不保留线性依赖性的方式提取特征，我们可能会丢失模型可以从数据中捕获的信息。如果我们使用与优化时不同的损失函数评估模型，我们最终可能会得到一个脆弱的模型，它在新的数据上容易出错。换句话说，*重构机器学习模型意味着同时重构实现它的代码及其数学公式*。我们希望保留代码的外部行为以及模型的输入和输出的概率行为。基于属性的测试可以帮助我们处理后者，正如我们将在第 9.4.2 节中讨论的那样。

## 6.8 重构学术代码：一个示例

考虑以下代码片段，它被用于在 QS 排名前 10 的顶尖大学（QS Quacquarelli Symonds 2022）教授研究生机器学习时使用。它相当典型，反映了我们在许多 GitHub 仓库和 Stack Overflow 的许多答案中可以找到的内容，这些内容最终被导入或剪切粘贴到机器学习代码库中。

```py
f<-function(x,mu1,mu2,S1i,S2i,p1=0.5) {
 #mixture of normals, density up to constant factor
 c1<-exp(-t(x-mu1)%*%S1i%*%(x-mu1))
 c2<-exp(-t(x-mu2)%*%S2i%*%(x-mu2))
 return(p1*c1+(1-p1)*c2)
}

a=3
n=2000
mu1=c(1,1)
mu2=c(4,4)
S=diag(2)
S1i=S2i=solve(S)
X=matrix(NA,2,n)
X[,1]=x=mu1
for (t in 1:(n-1)) {
y<-x+(2*runif(2)-1)*a
MHR<-f(y,mu1,mu2,S1i,S2i)/f(x,mu1,mu2,S1i,S2i)
if (runif(1)<MHR)
x<-y
X[,t+1]<-x
}
```

猜测这段代码打算实现什么比应该要难，因为函数和变量有描述性不强的名字，这些名字反映了某些数学符号。这本身并没有帮助，因为代码中没有注释提供文献参考，我们可以用它来查找这些符号的含义。我们唯一有的线索是一个提到正态混合的注释和一个名为`MHR`的变量。

参加这个代码被展示的讲座会告诉我们，这个代码实现了从正态混合分布中采样的 Metropolis-Hasting 算法。了解这一点后，我们可以给函数和变量起更具有描述性的名字：将一些变量命名为它们的*实际标准*表示法（例如，来自 Marin 和 Robert 2014），在简洁性和清晰性之间是一种可接受的权衡。现在我们可以猜测`MHR`是用于接受或拒绝从混合分布中抽取的新随机样本的 Metropolis-Hastings 比率。同时，我们可以添加间距和缩进，使代码更容易阅读。

```py
dmix2norm =  function(x, mu, Sigma, pi, log = FALSE) {

 Omega1 =  MASS::ginv(Sigma[1:2, 1:2])
 Omega2 =  MASS::ginv(Sigma[3:4, 3:4])

 elem1 =  exp(-t(x -  mu[1]) %*%  Omega1 %*%  (x -  mu[1]))
 elem2 =  exp(-t(x -  mu[2]) %*%  Omega2 %*%  (x -  mu[2]))

 return(pi[1] *  elem1 +  pi[2] *  elem2)

}#DMIX2NORM

metropolis.hastings =  function(mu, Sigma, pi, iter) {

 X =  matrix(NA, 2, iter)
 X[, 1] =  old =  mu[1:2]
 for (t in seq(iter -  1)) {

 new =  old +  (2 *  runif(2) -  1) *  a
 acceptance.probability =
 dmix2norm(new, mu = mu, Sigma = Sigma, pi = pi) /
 dmix2norm(old, mu = mu, Sigma = Sigma, pi = pi)

 if (runif(1) <  acceptance.probability)
 old =  new
 else
 old =  old

 X[, t +  1] =  old

 }#FOR

 return(X)

}#METROPOLIS.HASTINGS

mu =  c(c(1, 1), c(4, 4))
Sigma =  diag(rep(1, 4))
pi =  c(0.5, 0.5)
metropolis.hastings(mu = mu, Sigma = Sigma, pi = pi, iter = 2000)
```

我们通过创建一个临时（局部）提交并对其进行测试来完成这个重构步骤。虽然代码组织得更好，更容易阅读，但它有两个方面的不足：混合分布中的组件数量是硬编码为两个，而且密度本身是硬编码为正态分布。现在我们已经将代码组织成函数，我们可以继续到下一个重构步骤：向`metropolis.hastings()`添加两个参数，使用户能够控制混合的定义。我们可以称它们为`density`，即调用混合每个组件的密度函数，以及`density.args`，该函数的附加参数列表。为了保持代码的现有行为，我们将`dmix2norm()`更新为可以处理超过两个组件，同时确保当混合只有两个组件时，其返回值保持不变。此外，我们对生成新随机样本的提议函数也做了同样的处理，向`metropolis.hastings()`添加了两个额外的参数`proposal`和`proposal.args`。

这些更改使代码更加灵活和易读。我们采用的函数式编程方法允许我们将`metropolis.hastings()`重写，使其几乎看起来像伪代码（第 4.1 节）。因此，除了引用一本我们可以找到 Metropolis-Hastings 伪代码和深入解释其如何以及为什么工作的教科书之外，对代码正在做什么的注释需求减少。当然，对代码结构为什么是这样的注释可能仍然有用，因为它们将包含特定于这个特定实现的信息，这些信息在其他地方找不到。

```py
dmix2norm =  function(x, mu, Sigma, pi, log = FALSE) {

 nmix =  length(mu)
 mixture.component.density =  function(x, mu, Sigma)
 exp(-t(x -  mu[1]) %*%  MASS::ginv(Sigma) %*%  (x -  mu[1]))

 comp =  sapply(seq(nmix), function(i)
 mixture.component.density(x, mu[[i]], Sigma[[i]]))

 return(sum(pi *  comp))

}#DMIX2NORM

proposal.update =  function(dim = 2, a) {

 return((2 *  runif(dim) -  1) *  a)

}#PROPOSAL.UPDATE

metropolis.hastings =  function(density, density.args, proposal,
 proposal.args, pi, start, iter) {

 X =  matrix(NA, length(start), iter)
 X[, 1] =  old =  start
 for (t in seq(iter -  1)) {

 new =  old +
 do.call(proposal, c(list(dim = nrow(X)), proposal.args))

 update.threshold =
 do.call(density, c(list(x = new, pi = pi), density.args)) /
 do.call(density, c(list(x = old, pi = pi), density.args))

 if (runif(1) <  update.threshold)
 old =  new
 else
 old =  old

 X[, t +  1] =  old

 }#FOR

 return(X)

}#METROPOLIS.HASTINGS

mu =  list(c(1, 1), c(4, 4))
Sigma =  list(diag(2), diag(2))
metropolis.hastings(density = dmix2norm,
 density.args = list(mu = mu, Sigma = Sigma),
 proposal = proposal.update, proposal.args = list(a = 3),
 pi = c(0.5, 0.5), start = c(2, 2), iter = 2000)
```

我们再创建一个临时的提交，以测试代码是否仍然正常工作。最后，我们希望使代码更具可重用性。为了实现这一点，我们将运行`metropolis.hastings()`时创建的 Metropolis-Hastings 模拟实例存储到一个包含我们生成的随机样本以及通过`density`和`proposal`参数传递的函数的数据结构中，同时还包括相应的参数集`density.args`和`proposal.args`。为了方便起见，我们给这个数据结构赋予类名`"metropolis-hastings"`，以便以后为其编写方法。

```py
metropolis.hastings =  function(density, density.args, proposal,
 proposal.args, pi, start, iter) {

 [...]

 return(structure(list(values = X, call = match.call(),
 density = density, density.args = density.args,
 proposal = proposal, proposal.args = proposal.args,
 start = start), class = "metropolis.hastings"))

}#METROPOLIS.HASTINGS
```

如果我们对代码现在的样子（或者我们有其他事情要做）感到满意，我们可以创建最后一个临时的提交，并将其与之前的两个提交合并。新提交的合适提交信息可以是：

```py
Refactoring Metropolis-Hastings mixture of Gaussians.

* Clarify function and variable names, following Bayesian Essentials
    with R (Marin and Robert, 2014).
* Switch to a functional implementation that takes arbitrary density
    functions as arguments, each with separate optional arguments.
* Store the simulation in an S3 object, to allow for methods.
```

在提交此提交进行代码审查之前，我们应该编写一些单元测试来测试`metropolis.hastings()`的新功能接口。我们将在第九章详细讨论这个话题：目前，让我们说我们想要确保`metropolis.hastings()`只接受所有参数的有效值。为此，我们添加了清理它们的代码，并生成类似以下的信息性错误消息

```py
 if (missing(density))
 stop("missing a 'density' a function, with no default.")
 if (!is.function(density))
 stop("the 'density' argument must be a density function.")
```

然后我们添加测试来检查有效值是否被接受，无效值是否被拒绝。

```py
error =  try(metropolis.hastings(density = dmix2norm, [...])
stopifnot(!is(error, "try-error"))
error = try(metropolis.hastings(density = "not.a.function", [...])
stopifnot(is(error, "try-error"))
```

我们应该对通过`proposal`参数传递的函数做同样的事情。此外，我们应该使用相应的可选参数列表`density.args`和`proposal.args`调用这两个函数，以确保它们能够成功执行：单个参数值可能单独看起来很好，但将它们一起传递给`metropolis.hastings()`可能会导致失败。例如，清理`proposal.args`的代码可能看起来像这样

```py
 if (missing(proposal.args))
 proposal.args =  list()
 if (!is.list(proposal.args))
 stop("the 'proposal.args' argument must be a list.")
```

其中我们将`proposal.args`设置为空列表作为后备，默认选择，如果用户没有提供它。然后，清理`proposal`和`proposal.args`的代码可以检查`proposal`函数是否运行，以及其输出是否具有正确的类型和维度。

```py
 try.proposal =  try(do.call(proposal, proposal.args))
 if (is(try.proposal, "try-error"))
 stop("the 'proposal' function fails to run with ",
 "the arguments in 'proposal.args'.")
 if (!is.numeric(try.proposal) ||
 (length(try.proposal) !=  length(start)))
 stop("the 'proposal' function returns invalid samples.")
```

测试此代码的测试应该使用有效和无效的提案函数，以及带有有效和无效可选参数集的提案函数调用`metropolis.hastings()`。

作为另一个例子，我们应该检查`iter`参数中的迭代次数，再次选择一个合理的默认值。

```py
 if (missing(iter))
 iter =  10
 if (!is.numeric(iter) ||  ((x %/%  1) ==  x))
 stop("the 'iter' argument must be a non-negative integer.")
```

相应的软件测试可以尝试边界值（`0`）、有效值（`10`）、无效值（`Inf`）和特殊值（`NaN`），以确认清理代码按预期工作。

```py
error =  try(metropolis.hastings([...], iter = 0)
stopifnot(!is(error, "try-error"))
error = try(metropolis.hastings([...], iter = 10)
stopifnot(!is(error, "try-error"))
error = try(metropolis.hastings([...], iter = Inf)
stopifnot(is(error, "try-error"))
error = try(metropolis.hastings([...], iter = NaN)
stopifnot(is(error, "try-error"))
```

清理代码应该包含在一个提交中，而测试代码则放在另一个提交中：它们将位于不同的文件中，具有不同的目的，因此将它们一起提交是不合适的。完成这些后，我们新的 Metropolis-Hastings 实现就准备好提交进行代码审查了。

### 参考文献

Alam, S., L. Bălan, N. L. Chan, G. Comym, Y. Dada, I. Danov, L. Hoang, et al. 2022\. *Kedro*. [`github.com/kedro-org/kedro`](https://github.com/kedro-org/kedro).

Arisholm, E., H. Gallis, T. Dybå, and D. I. K. Sjøberg. 2007\. “Evaluating Pair Programming with Respect to System Complexity and Programmer Expertise.” *IEEE Transactions on Software Engineering* 33 (2): 5–86.

Ayer, A. 2022\. *git-crypt: Transparent File Encryption in Git*. [`github.com/AGWA/git-crypt`](https://github.com/AGWA/git-crypt).

Bezanson, J., S. Karpinski, V. B. Shah, and et al. 2022\. *Style Guide: The Julia Language*. [`docs.julialang.org/en/v1/manual/style-guide/index.html`](https://docs.julialang.org/en/v1/manual/style-guide/index.html).

Blagotic, A., D. Valle-Jones, J. Breen, J. Lundborg, J. M. White, J. Bode, K. White, et al. 2021\. *ProjectTemplate: Automates the Creation of New Statistical Analysis Projects*. [`cran.r-project.org/web/packages/ProjectTemplate/`](https://cran.r-project.org/web/packages/ProjectTemplate/).

CRAN Team. 2022\. *The Comprehensive R Archive Network*. [`cran.r-project.org/`](https://cran.r-project.org/).

de Lima Salge, C. A., and N. Berente. 2016\. “Pair Programming vs. Solo Programming: What Do We Know After 15 Years of Research?” In *Proceedings of the Annual Hawaii International Conference on System Sciences*, 5398–5406.

Docker. 2022a. *Docker*. [`www.docker.com/`](https://www.docker.com/).

Duvall, P. M., S. Matyas, and A. Glover. 2007\. *Continuous Integration: Improving Software Quality and Reducing Risk*. Addison-Wesley.

Fowler, M. 2018\. *Refactoring: Improving the Design of Existing Code*. 2nd ed. Addison-Wesley.

Google. 2022d. *Google Python Style Guide*. [`google.github.io/styleguide/pyguide.html`](https://google.github.io/styleguide/pyguide.html).

Google. 2022e. *repo: The Multiple Git Repository Tool*. [`github.com/GerritCodeReview/git-repo`](https://github.com/GerritCodeReview/git-repo).

Greenfeld, A. R. 2022\. *Cookiecutter Data Science*. [`drivendata.github.io/cookiecutter-data-science/`](https://drivendata.github.io/cookiecutter-data-science/).

Grotov, K., S. Titov, V. Sotnikov, Y. Golubev, and T. Bryksin. 2022\. “A Large-Scale Comparison of Python Code in Jupyter Notebooks and Scripts.” In *Proceedings of the 19th Working Conference on Mining Software Repositories*, 1–12.

Hammant, P. 2020\. *Trunk Based Development*. [`trunkbaseddevelopment.com/`](https://trunkbaseddevelopment.com/).

Jacek, C., M. Greiler, C. Bird, L. Panjer, and T. Coatta. 2018\. “CodeFlow: Improving the Code Review Process at Microsoft.” *ACM Queue* 6 (5): 1–20.

JetBrains. 2022b. *PyCharm*. [`www.jetbrains.com/pycharm/`](https://www.jetbrains.com/pycharm/).

JuliaLang. 2022\. *Pkg: Package Manager for the Julia Programming Language*. [`pkgdocs.julialang.org/v1/`](https://pkgdocs.julialang.org/v1/).

Kernigham, B. W., and R. Pike. 1999\. *The Practice of Programming*. Addison-Wesley.

Kriasoft。 2016\. *文件夹结构约定*。[`github.com/kriasoft/Folder-Structure-Conventions`](https://github.com/kriasoft/Folder-Structure-Conventions)。

Linux 内核组织。 2022\. *Linux 内核存档*。[`kernel.org/`](https://kernel.org/).

Lopes, C. V. 2020\. *编程风格练习*。 CRC 出版社。

Marin, J.-M.，和 C. P. Robert。 2014\. *使用 R 的贝叶斯基础*。 第 2 版。 Springer。

微软。 2022i. *Visual Studio Code：代码编辑，重新定义*。[`code.visualstudio.com/`](https://code.visualstudio.com/).

自然。 2016\. “关于可重复性的现实检验。” *自然* 533 (437)。

Ousterhout, J. 2018\. *软件设计哲学*。 Yaknyam 出版社。

Popescu, M. 2019\. *结对编程解释*。[`shopify.engineering/pair-programming-explained`](https://shopify.engineering/pair-programming-explained)。

Potvin, R., and J. Levenberg. 2016\. “为什么谷歌将数十亿行代码存储在单个仓库中。” *ACM 通讯* 59 (7): 78–87。

Preston-Werner, T. 2022\. *语义版本控制*。[`semver.org/`](https://semver.org/).

Project Jupyter。 2022\. *Jupyter*。[`jupyter.org/`](https://jupyter.org/).

Python 软件基金会。 2022a. *PyPI：Python 包索引*。[`pypi.org/`](https://pypi.org/).

QS Quacquarelli Symonds。 2022\. *QS 世界大学排名*。[`www.topuniversities.com/qs-world-university-rankings`](https://www.topuniversities.com/qs-world-university-rankings)。

Quest, K. 2022\. *标准 Go 项目布局*。[`github.com/golang-standards/project-layout`](https://github.com/golang-standards/project-layout)。

Rigby, P., and C. Bird. 2013\. “趋同的当代软件同行评审实践。” 在 *第 9 届欧洲软件工程会议与 ACM SIGSOFT 软件工程基础研讨会联合会议论文集中*，202–12。

Sadowski, C., E. Söderberg, L. Church, M. Sipko, and A. Bacchelli. 2018\. “现代代码审查：谷歌的案例研究。” 在 *第 40 届国际软件工程会议：软件工程实践* 会议论文集中，181–90 页。

Saltz, J. S., and I. Shamshurin. 2017\. “在数据科学背景下结对编程是否有效？初步案例研究。” 在 *IEEE 国际大数据会议论文集中*，2348–54。

Sculley, D., G. Holt, D. Golovin, E. Davydov, T. Phillips, D. Ebner, V. Chaudhary, M. Young, J.-F. Crespo, and D. Dennison. 2015\. “机器学习系统中的隐藏技术债务。” 在 *第 28 届国际神经网络信息处理系统（NIPS）会议论文集中*，2:2503–11。

Shiraishi, M., H. Washizaki, Y. Fukazawa, and J. Yoder. 2019\. “团队编程：系统文献综述。” 在 *IEEE 第 43 届年度计算机软件与应用会议论文集中*，616–21。

Simmons, A. J., S. Barnett, J. Rivera-Villicana, A. Bajaj, and R. Vasa. 2020\. “开源数据科学项目中编码标准符合性的大规模比较分析.” 在 *第 14 届 ACM / IEEE 经验软件工程和度量国际研讨会论文集*，1–11.

Swoboda, S. 2021\. *与团队编程连接*. [`shopify.engineering/mob-programming`](https://shopify.engineering/mob-programming).

Tang, Y., R. Khatchadouriant, M. Bagherzadeh, R. Singh, A. Stewart, and A. Raja. 2021\. “机器学习系统中重构和技术债务的实证研究.” 在 *2021 年 IEEE/ACM 第 43 届国际软件工程会议论文集*，238–50.

Tatman, R., J. VanderPlas, and S. Dane. 2018\. “机器学习研究可重复性的实用分类.” 在 *2018 年 ICML 第二次机器学习可重复性研讨会论文集*.

Git 开发团队. 2022\. *Git 源代码镜像*. [`github.com/git/git`](https://github.com/git/git).

Kubernetes 作者. 2022a. *Kubernetes*. [`kubernetes.io/`](https://kubernetes.io/).

Thomas, D., and A. Hunt. 2019\. *实用程序员：您的精通之旅*. 周年纪念版. Addison-Wesley.

Tian, Y., Y. Zhang, K.-J. Stol, L. Jiang, and H. Liu. 2022\. “什么使一个好的提交信息？” 在 *第 44 届国际软件工程会议论文集*，1–13.

Tornhill, A., and M. Borg. 2022\. “代码红色：代码质量对业务的影响：对 39 个专有生产代码库的定量研究.” 在 *国际技术债务会议论文集*，1–10.

Toro, A. L. 2020\. *优秀的代码审查–团队需要的超级力量*. [`shopify.engineering/great-code-reviews`](https://shopify.engineering/great-code-reviews).

Uber. 2022\. *Piranha：用于重构与功能标志 API 相关的代码的工具*. [`github.com/uber/piranha`](https://github.com/uber/piranha).

van Oort, B., L. Cruz, M. Aniche, and A. van Deursen. 2021\. “机器学习项目中代码恶臭的普遍性.” 在 *2021 年 IEEE/ACM 第一次人工智能工程研讨会：人工智能的软件工程*，35–42.

van Rossum, G., B. Warsaw, and N. Coghlan. 2001\. *PEP 8: Python 代码风格指南*. [`peps.python.org/pep-0008/`](https://peps.python.org/pep-0008/).

Walters, M., and P. Lee Scott. 2021\. *meta-git: 管理您的元仓库和子 Git 仓库*. [`www.npmjs.com/package/meta-git`](https://www.npmjs.com/package/meta-git).

Wickham, H. 2022b. *tidyverse 风格指南*. [`style.tidyverse.org/`](https://style.tidyverse.org/).

Wiggins, A. 2017\. *十二要素应用*. [`12factor.net`](https://12factor.net).

Williams, L., R. R. Kessler, and W. Cunningham. 2000\. “加强结对编程的案例.” *IEEE 软件* 17 (4): 19–25.

张，H.，克鲁兹，L.，范德尔斯，A. van Deursen. 2022. “面向机器学习应用的代码异味.” 见 *《第 1 届国际人工智能工程会议：人工智能的软件工程》论文集*，1–12 页。

* * *

1.  REPL（“读取-评估-打印循环”）是一个交互式编程环境，用户可以编写代码语句，这些语句会被立即评估，并且输出结果会返回给用户。它们对于分步骤运行软件和理解其组件的行为非常有价值。↩︎

1.  在软件工程中，许多技术术语具有完全不同的含义：例如，“测试”（统计测试与单元测试）、“回归”（统计模型与对现有软件功能产生不利影响）或“特性”（数据集中的变量与软件的一个区分特征）。类似的问题也可能出现在其他领域的术语中。↩︎
