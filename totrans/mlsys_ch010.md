# DNN 架构

*DALL·E 3 提示：一个视觉上引人注目的矩形图像，展示了深度学习算法（如 CNN、RNN 和注意力网络）与机器学习系统的相互作用，相互连接。构图包括神经网络图与处理器、图和数据流等计算系统表示的完美融合。明亮的霓虹色调与黑暗的未来背景形成对比，象征着尖端技术和复杂的系统复杂性。*

![](img/file53.png)

## 目的

*为什么神经网络中的架构选择会影响决定计算可行性、硬件需求和部署约束的系统设计决策？*

神经网络架构代表了直接决定系统性能和部署可行性的工程决策。每个架构选择都会在整个系统堆栈中产生级联效应：内存带宽需求、计算复杂性模式、并行化机会和硬件加速兼容性。理解这些架构影响使工程师能够在模型能力和系统约束之间做出明智的权衡，在问题发生之前预测计算瓶颈，并选择合适的硬件平台。架构决策决定了机器学习系统是否能在可用的计算资源内满足性能要求。这种理解对于构建可扩展的 AI 系统至关重要，这些系统可以在不同的环境中有效部署。

**学习目标**

+   区分四种主要神经网络架构家族（MLPs、CNNs、RNNs、Transformers）的计算特性和归纳偏差

+   分析架构设计选择如何决定计算复杂性、内存需求和并行化机会

+   评估架构模式对硬件利用率、内存带宽和部署约束的系统级影响

+   将架构选择框架应用于匹配数据特征与特定应用中适当的神经网络设计

+   使用复杂性分析评估不同架构方法之间的计算和内存权衡

+   检查基本计算原语（矩阵乘法、卷积、注意力）如何映射到硬件加速机会

+   批判常见的架构选择谬误及其对系统性能和部署成功的影响

+   综合统一的归纳偏差框架，解释架构-数据兼容性模式

## 架构原则与工程权衡

将神经网络计算系统地组织成有效的架构，是当代机器学习系统中最具影响力的进展之一。基于第三章中建立的神经网络计算的数学基础，本章探讨了支配操作（矩阵乘法、非线性激活和基于梯度的优化）如何结构化以解决复杂计算问题的架构原则。这种架构视角架起了数学理论与实际系统实现之间的桥梁，考察了网络层面的设计选择如何决定整个系统的性能特征。

本章聚焦于贯穿机器学习系统设计的工程权衡。虽然数学理论，尤其是通用逼近结果，表明神经网络具有非凡的表示灵活性，但实际部署需要通过明智的架构专门化才能实现的计算效率。这种紧张关系在多个维度上体现出来：理论上的通用性与计算上的可处理性，表示的完备性与内存效率，以及数学的普遍性与领域特定优化。通过架构创新解决这些紧张关系是推动机器学习系统进步的主要驱动力。

当代神经网络架构源于对在结构化数据上部署通用数学框架时遇到的具体计算挑战的系统响应。每个架构范式都体现了独特的归纳偏差（关于数据结构和关系的隐含假设），这使高效学习成为可能，同时以领域适当的方式约束假设空间。这些架构创新代表了将计算原语组织成模式以实现表示能力与计算效率之间最佳平衡的工程解决方案。

本章考察了四个架构家族，它们共同定义了现代神经网络的概念景观。多层感知器作为通用逼近理论的典范实现，展示了密集连接如何实现通用模式识别，并说明了架构通用性的计算成本。卷积神经网络引入了空间架构专业化的范例，利用平移不变性和局部连接来实现显著的效率提升，同时保持对空间数据的表示能力。循环神经网络将架构专业化扩展到时间域，通过引入显式的记忆机制，使序列处理能力成为前馈架构所不具备的。注意力机制和 Transformer 架构代表了当前的进化前沿，用动态的、内容相关的计算取代了固定的结构假设，在并行化操作中保持了计算效率，同时实现了显著的能力。

这些架构模式在系统工程中的重要性不仅限于算法考虑。每个架构选择都会创建独特的计算特征，这些特征会传播到实现堆栈的每一层，从而确定内存访问模式、并行化策略、硬件利用特性，并在资源约束下最终决定系统的可行性。理解这些架构影响对于负责系统设计、资源分配和生产环境性能优化的工程师来说至关重要。

本章采用了一种面向系统的分析框架，阐明了架构抽象与具体实现要求之间的关系。对于每个架构家族，我们系统地检查确定硬件资源需求的计算原语、实现高效算法的组织原则、影响系统可扩展性的内存层次影响，以及架构复杂性与计算开销之间的权衡。

分析方法系统地建立在第三章中建立的神经网络基础之上，通过考察架构专业化如何组织这些操作以利用特定问题的结构，扩展了前向传播、反向传播和基于梯度的优化等核心概念。理解将这些架构范式及其独特的计算特性连接起来的进化关系，从业者发展了进行架构选择、资源规划和复杂部署场景中系统优化的原则性决策所必需的概念工具。

## 多层感知器：密集模式处理

多层感知器（MLP）代表了在第三章（ch009.xhtml#sec-dl-primer）中引入的全连接架构，现在通过架构选择和系统权衡的视角来审视。MLP 体现了一种归纳偏差：**它们假设数据中没有先验结构，允许任何输入与任何输出相关联**。这种架构选择通过将所有输入关系视为同等可能的，从而提供了最大灵活性，使 MLP 与专用替代方案相比具有多功能性但计算密集。它们的计算能力通过通用逼近定理（UAT）1（Cybenko 1989；Hornik, Stinchcombe, and White 1989）从理论上得到证实，我们在第三章（ch009.xhtml#sec-dl-primer）中作为脚注遇到了它。该定理指出，具有非线性激活函数的足够大的 MLP 可以在紧凑域上逼近任何连续函数，前提是给定适当的权重和偏差。

***多层感知器（MLP）***是*全连接神经网络*，其中每个神经元都与相邻层的所有神经元连接，通过*通用逼近*提供*最大灵活性*，但代价是*高参数计数*和*计算强度*。

在实践中，UAT（用户接受测试）解释了为什么 MLP（多层感知器）能够在各种任务中取得成功，同时揭示了理论能力与实际实施之间的差距。定理保证了*某些*MLP 可以逼近任何函数，但并未提供关于所需网络大小或权重确定的指导。这种差距在现实世界的应用中变得至关重要：虽然 MLP 在理论上可以解决任何模式识别问题，但要实现这种能力可能需要不切实际的大型网络或大量的计算。这种理论能力推动了 MLP 在表格数据、推荐系统和输入关系未知的问题中的应用选择，而这些实际限制促使开发了专门架构，这些架构利用数据结构以提高计算效率，如第 4.1 节中详细所述。

当应用于 MNIST 手写数字识别挑战 2 时，MLP 通过将一个<semantics><mrow><mn>28</mn><mo>×</mo><mn>28</mn></mrow><annotation encoding="application/x-tex">28\times 28</annotation></semantics>像素图像转换为数字分类，展示了其计算方法。

### 模式处理需求

深度学习模型经常遇到任何输入特征都可能影响任何输出的问题，这些关系没有内在的约束。金融市场分析是这一挑战的例子：任何经济指标都可能影响任何市场结果。同样，在自然语言处理中，一个词的意义可能取决于句子中的任何其他词。这些场景需要一种能够学习所有输入特征之间任意关系的建筑模式。

密集模式处理通过几个关键能力来解决这些挑战。首先，它实现了无限制的特征交互，其中每个输出可以依赖于任何输入组合。其次，它支持学习特征重要性，使系统能够确定哪些连接是重要的，而不是依赖于预定的关系。最后，它提供自适应表示，使网络能够根据数据重新塑造其内部表示。

MNIST 数字识别任务说明了这种不确定性：虽然人类可能专注于数字的特定部分（例如‘6’中的环或‘8’中的交叉），但用于分类的像素组合仍然是未确定的。带有装饰线的‘7’可能与‘2’共享像素模式，而手写的变体意味着判别特征可能出现在图像的任何位置。关于特征关系的这种不确定性需要密集处理方法，其中每个像素都可能影响分类决策。

这种无限制连接的要求直接导致 MLPs 的数学基础。

### 算法结构

MLPs enable unrestricted feature interactions through a direct algorithmic solution: complete connectivity between all nodes. This connectivity requirement manifests through a series of fully-connected layers, where each neuron connects to every neuron in adjacent layers, the “dense” connectivity pattern introduced in 第三章.

这种建筑原则将密集连接模式转换为矩阵乘法操作 3，建立了使 MLPs 在计算上可处理的数学基础。如图图 4.1 所示，每一层通过在第三章深度学习基础中引入的基本操作转换其输入：

<semantics><mrow><msup><mi>𝐡</mi><mrow><mo stretchy="true" form="prefix">(</mo><mi>l</mi><mo stretchy="true" form="postfix">)</mo></mrow></msup><mo>=</mo><mi>f</mi><mo minsize="1.2" maxsize="1.2" stretchy="false" form="prefix">(</mo><msup><mi>𝐡</mi><mrow><mo stretchy="true" form="prefix">(</mo><mi>l</mi><mo>−</mo><mn>1</mn><mo stretchy="true" form="postfix">)</mo></mrow></msup><msup><mi>𝐖</mi><mrow><mo stretchy="true" form="prefix">(</mo><mi>l</mi><mo stretchy="true" form="postfix">)</mo></mrow></msup><mo>+</mo><msup><mi>𝐛</mi><mrow><mo stretchy="true" form="prefix">(</mo><mi>l</mi><mo stretchy="true" form="postfix">)</mo></mrow></msup><mo minsize="1.2" maxsize="1.2" stretchy="false" form="postfix">)</mo></mrow> <annotation encoding="application/x-tex">\mathbf{h}^{(l)} = f\big(\mathbf{h}^{(l-1)}\mathbf{W}^{(l)} + \mathbf{b}^{(l)}\big)</annotation></semantics>

回想一下，<semantics><msup><mi>𝐡</mi><mrow><mo stretchy="true" form="prefix">(</mo><mi>l</mi><mo stretchy="true" form="postfix">)</mo></mrow></msup><annotation encoding="application/x-tex">\mathbf{h}^{(l)}</annotation></semantics> 代表第 <semantics><mi>l</mi><annotation encoding="application/x-tex">l</annotation></semantics> 层的输出（激活向量），<semantics><msup><mi>𝐡</mi><mrow><mo stretchy="true" form="prefix">(</mo><mi>l</mi><mo>−</mo><mn>1</mn><mo stretchy="true" form="postfix">)</mo></mrow></msup><annotation encoding="application/x-tex">\mathbf{h}^{(l-1)}</annotation></semantics> 代表来自前一层的数据输入，<semantics><msup><mi>𝐖</mi><mrow><mo stretchy="true" form="prefix">(</mo><mi>l</mi><mo stretchy="true" form="postfix">)</mo></mrow></msup><annotation encoding="application/x-tex">\mathbf{W}^{(l)}</annotation></semantics> 表示第 <semantics><mi>l</mi><annotation encoding="application/x-tex">l</annotation></semantics> 层的权重矩阵，<semantics><msup><mi>𝐛</mi><mrow><mo stretchy="true" form="prefix">(</mo><mi>l</mi><mo stretchy="true" form="postfix">)</mo></mrow></msup><annotation encoding="application/x-tex">\mathbf{b}^{(l)}</annotation></semantics> 表示偏置向量，而 <semantics><mrow><mi>f</mi><mrow><mo stretchy="true" form="prefix">(</mo><mi>⋅</mi><mo stretchy="true" form="postfix">)</mo></mrow></mrow><annotation encoding="application/x-tex">f(\cdot)</annotation></semantics> 表示激活函数（例如 ReLU，详见第三章）。这种层级的转换在概念上很简单，但创建的计算模式效率取决于我们如何针对不同的问题结构组织这些操作。

![](img/file54.svg)

图 4.1：**层状变换**：多层感知器（MLPs）通过序列矩阵乘法和非线性激活实现密集连接，支持复杂特征交互和输入数据的层次表示。每一层将前一层输入向量转换，生成一个新的向量作为下一层的输入，如文本中的方程所示。来源：(Reagen 等人 2017)。

这些操作的维度揭示了密集模式处理的计算规模：

+   输入向量：<semantics><mrow><msup><mi>𝐡</mi><mrow><mo stretchy="true" form="prefix">(</mo><mn>0</mn><mo stretchy="true" form="postfix">)</mo></mrow></msup><mo>∈</mo><msup><mi>ℝ</mi><msub><mi>d</mi><mtext mathvariant="normal">in</mtext></msub></msup></mrow><annotation encoding="application/x-tex">\mathbf{h}^{(0)} \in \mathbb{R}^{d_{\text{in}}}</annotation></semantics>（在本公式中作为行向量处理）代表所有潜在输入特征

+   权重矩阵：<semantics><mrow><msup><mi>𝐖</mi><mrow><mo stretchy="true" form="prefix">(</mo><mi>l</mi><mo stretchy="true" form="postfix">)</mo></mrow></msup><mo>∈</mo><msup><mi>ℝ</mi><mrow><msub><mi>d</mi><mtext mathvariant="normal">in</mtext></msub><mo>×</mo><msub><mi>d</mi><mtext mathvariant="normal">out</mtext></msub></mrow></msup></mrow><annotation encoding="application/x-tex">\mathbf{W}^{(l)} \in \mathbb{R}^{d_{\text{in}} \times d_{\text{out}}}</annotation></semantics>捕捉所有可能的输入输出关系

+   输出向量：<semantics><mrow><msup><mi>𝐡</mi><mrow><mo stretchy="true" form="prefix">(</mo><mi>l</mi><mo stretchy="true" form="postfix">)</mo></mrow></msup><mo>∈</mo><msup><mi>ℝ</mi><msub><mi>d</mi><mtext mathvariant="normal">out</mtext></msub></msup></mrow><annotation encoding="application/x-tex">\mathbf{h}^{(l)} \in \mathbb{R}^{d_{\text{out}}}</annotation></semantics>产生转换后的表示

考虑一个由 3 个神经元隐藏层处理的简化 4 像素图像：

**输入**：<semantics><mrow><msup><mi>𝐡</mi><mrow><mo stretchy="true" form="prefix">(</mo><mn>0</mn><mo stretchy="true" form="postfix">)</mo></mrow></msup><mo>=</mo><mrow><mo stretchy="true" form="prefix">[</mo><mn>0.8</mn><mo>,</mo><mn>0.2</mn><mo>,</mo><mn>0.9</mn><mo>,</mo><mn>0.1</mn><mo stretchy="true" form="postfix">]</mo></mrow></mrow><annotation encoding="application/x-tex">\mathbf{h}^{(0)} = [0.8, 0.2, 0.9, 0.1]</annotation></semantics>（4 个像素强度）

**权重矩阵**: <semantics><mrow><msup><mi>𝐖</mi><mrow><mo stretchy="true" form="prefix">(</mo><mn>1</mn><mo stretchy="true" form="postfix">)</mo></mrow></msup><mo>=</mo><mrow><mo stretchy="true" form="prefix">[</mo><mtable><mtr><mtd columnalign="center" style="text-align: center"><mn>0.5</mn></mtd><mtd columnalign="center" style="text-align: center"><mn>0.1</mn></mtd><mtd columnalign="center" style="text-align: center"><mi>−</mi><mn>0.2</mn></mtd></mtr><mtr><mtd columnalign="center" style="text-align: center"><mi>−</mi><mn>0.3</mn></mtd><mtd columnalign="center" style="text-align: center"><mn>0.8</mn></mtd><mtd columnalign="center" style="text-align: center"><mn>0.4</mn></mtd></mtr><mtr><mtd columnalign="center" style="text-align: center"><mn>0.2</mn></mtd><mtd columnalign="center" style="text-align: center"><mi>−</mi><mn>0.4</mn></mtd><mtd columnalign="center" style="text-align: center"><mn>0.6</mn></mtd></mtr><mtr><mtd columnalign="center" style="text-align: center"><mn>0.7</mn></mtd><mtd columnalign="center" style="text-align: center"><mn>0.3</mn></mtd><mtd columnalign="center" style="text-align: center"><mi>−</mi><mn>0.1</mn></mtd></mtr></mtable><mo stretchy="true" form="postfix">]</mo></mrow></mrow><annotation encoding="application/x-tex">\mathbf{W}^{(1)} = \begin{bmatrix} 0.5 & 0.1 & -0.2 \\ -0.3 & 0.8 & 0.4 \\ 0.2 & -0.4 & 0.6 \\ 0.7 & 0.3 & -0.1 \end{bmatrix}</annotation></semantics> (4×3 矩阵)

**计算**: <semantics><mtable><mtr><mtd columnalign="center" style="text-align: center"><msup><mi>𝐳</mi><mrow><mo stretchy="true" form="prefix">(</mo><mn>1</mn><mo stretchy="true" form="postfix">)</mo></mrow></msup><mo>=</mo><msup><mi>𝐡</mi><mrow><mo stretchy="true" form="prefix">(</mo><mn>0</mn><mo stretchy="true" form="postfix">)</mo></mrow></msup><msup><mi>𝐖</mi><mrow><mo stretchy="true" form="prefix">(</mo><mn>1</mn><mo stretchy="true" form="postfix">)</mo></mrow></msup><mo>=</mo><mrow><mo stretchy="true" form="prefix">[</mo><mtable><mtr><mtd columnalign="center" style="text-align: center"><mn>0.5</mn><mo>×</mo><mn>0.8</mn><mo>+</mo><mrow><mo stretchy="true" form="prefix">(</mo><mi>−</mi><mn>0.3</mn><mo stretchy="true" form="postfix">)</mo></mrow><mo>×</mo><mn>0.2</mn><mo>+</mo><mn>0.2</mn><mo>×</mo><mn>0.9</mn><mo>+</mo><mn>0.7</mn><mo>×</mo><mn>0.1</mn></mtd></mtr><mtr><mtd columnalign="center" style="text-align: center"><mn>0.1</mn><mo>×</mo><mn>0.8</mn><mo>+</mo><mn>0.8</mn><mo>×</mo><mn>0.2</mn><mo>+</mo><mrow><mo stretchy="true" form="prefix">(</mo><mi>−</mi><mn>0.4</mn><mo stretchy="true" form="postfix">)</mo></mrow><mo>×</mo><mn>0.9</mn><mo>+</mo><mn>0.3</mn><mo>×</mo><mn>0.1</mn></mtd></mtr><mtr><mtd columnalign="center" style="text-align: center"><mrow><mo stretchy="true" form="prefix">(</mo><mi>−</mi><mn>0.2</mn><mo stretchy="true" form="postfix">)</mo></mrow><mo>×</mo><mn>0.8</mn><mo>+</mo><mn>0.4</mn><mo>×</mo><mn>0.2</mn><mo>+</mo><mn>0.6</mn><mo>×</mo><mn>0.9</mn><mo>+</mo><mrow><mo stretchy="true" form="prefix">(</mo><mi>−</mi><mn>0.1</mn><mo stretchy="true" form="postfix">)</mo></mrow><mo>×</mo><mn>0.1</mn></mtd></mtr></mtable><mo stretchy="true" form="postfix">]</mo></mrow></mtd></mtr><mtr><mtd columnalign="center" style="text-align: center"><mo>=</mo><mrow><mo stretchy="true" form="prefix">[</mo><mtable><mtr><mtd columnalign="center" style="text-align: center"><mn>0.65</mn></mtd></mtr><mtr><mtd columnalign="center" style="text-align: center"><mi>−</mi><mn>0.17</mn></mtd></mtr><mtr><mtd columnalign="center" style="text-align: center"><mn>0.47</mn></mtd></mtr></mtable><mo stretchy="true" form="postfix">]</mo></mrow></mtd></mtr></mtable><annotation encoding="application/x-tex">\begin{gather*} \mathbf{z}^{(1)} = \mathbf{h}^{(0)}\mathbf{W}^{(1)} = \begin{bmatrix} 0.5×0.8 + (-0.3)×0.2 + 0.2×0.9 + 0.7×0.1 \\ 0.1×0.8 + 0.8×0.2 + (-0.4)×0.9 + 0.3×0.1 \\ (-0.2)×0.8 + 0.4×0.2 + 0.6×0.9 + (-0.1)×0.1 \end{bmatrix} \\ = \begin{bmatrix} 0.65 \\ -0.17 \\ 0.47 \end{bmatrix} \end{gather*}</annotation></semantics> **ReLU 后**: <semantics><mrow><msup><mi>𝐡</mi><mrow><mo stretchy="true" form="prefix">(</mo><mn>1</mn><mo stretchy="true" form="postfix">)</mo></mrow></msup><mo>=</mo><mrow><mo stretchy="true" form="prefix">[</mo><mn>0.65</mn><mo>,</mo><mn>0</mn><mo>,</mo><mn>0.47</mn><mo stretchy="true" form="postfix">]</mo></mrow></mrow><annotation encoding="application/x-tex">\mathbf{h}^{(1)} = [0.65, 0, 0.47]</annotation></semantics> (negative values zeroed)

每个隐藏神经元将所有输入像素以不同的权重组合起来，展示了无限制的特征交互。

MNIST 示例展示了这些操作的实用规模：

+   每个由 784 维输入（<semantics><mrow><mn>28</mn><mo>×</mo><mn>28</mn></mrow><annotation encoding="application/x-tex">28\times 28</annotation></semantics>像素）连接到第一隐藏层中的每个神经元

+   一个包含 100 个神经元的隐藏层需要一个<semantics><mrow><mn>784</mn><mo>×</mo><mn>100</mn></mrow><annotation encoding="application/x-tex">784\times 100</annotation></semantics>权重矩阵

+   这个矩阵中的每个权重代表一个可学习的输入像素和隐藏特征之间的关系

这种算法结构解决了任意特征关系的需求，同时创建了计算机系统必须适应的特定计算模式。

#### 架构特性

这种密集连接方法既带来了优势，也带来了权衡。密集连接提供了之前建立的通用逼近能力，但引入了计算冗余。虽然这种理论上的能力使得 MLP 在足够宽的情况下能够模拟任何连续函数，但这种灵活性需要大量参数来学习相对简单的模式。密集连接确保每个输入特征影响每个输出，以最大计算成本换取最大表达能力。

这些权衡促使了复杂的优化技术，在保持模型能力的同时减少计算需求。结构化剪枝可以消除 80-90%的连接，同时最小化精度损失，而量化将精度要求从 32 位降低到 8 位或更低。虽然第十章详细介绍了这些压缩策略，但这里建立的架构基础决定了哪种优化方法对于密集连接模式最为有效，而第十一章探讨了利用规则矩阵操作结构的特定硬件实现。

### 计算映射

密集矩阵乘法的数学表示映射到系统必须处理的特定计算模式。这种映射从数学抽象到计算现实，正如在第一个实现列表 4.1 中所示。

函数 mlp_layer_matrix 直接反映了数学方程，使用高级矩阵操作（`matmul`）在单行中表达计算，同时抽象了底层复杂性。这种实现风格是深度学习框架的特征，其中优化的库管理实际的计算。

列表 4.1：这个实现展示了神经网络通过矩阵运算在层之间执行加权求和和激活函数。代码强调了多层感知器中的核心计算模式。

```py
def mlp_layer_matrix(X, W, b):
    # X: input matrix (batch_size × num_inputs)
    # W: weight matrix (num_inputs × num_outputs)
    # b: bias vector (num_outputs)
    H = activation(matmul(X, W) + b)
    # One clean line of math
    return H
```

要理解这种架构的系统影响，我们必须查看高级框架调用的“内部结构”。从硬件的角度来看，优雅的单行矩阵乘法`output = matmul(X, W)`实际上是一系列嵌套循环，这些循环揭示了系统真正的计算需求。这种从逻辑模型到物理执行的转换揭示了决定内存访问、并行化策略和硬件利用的关键模式。

第二种实现方式，`mlp_layer_compute`（如列表 4.2 所示），通过嵌套循环揭示了实际的计算模式。这个版本揭示了当我们计算层的输出时真正发生的事情：我们处理批处理中的每个样本，通过累积所有输入的加权贡献来计算每个输出神经元。

列表 4.2：这个实现通过累积批处理中所有输入的加权贡献来计算每个输出神经元。详细的逐步过程揭示了神经网络中单个层如何处理数据，强调了偏差和加权总和在产生输出中的作用。

```py
def mlp_layer_compute(X, W, b):
    # Process each sample in the batch
    for batch in range(batch_size):
        # Compute each output neuron
        for out in range(num_outputs):
            # Initialize with bias
            Z[batch, out] = b[out]
            # Accumulate weighted inputs
            for in_ in range(num_inputs):
                Z[batch, out] += X[batch, in_] * W[in_, out]

    H = activation(Z)
    return H
```

这种从数学抽象到具体计算的转换揭示了密集矩阵乘法如何分解为更简单操作的嵌套循环。外层循环处理批处理中的每个样本，中间循环计算每个输出神经元的值。在内层循环中，系统执行重复的乘加操作 4，将每个输入与其对应的权重组合。

在 MNIST 示例中，每个输出神经元需要 784 次乘加操作和至少 1,568 次内存访问（784 次用于输入，784 次用于权重）。虽然实际实现使用 BLAS5 或 cuBLAS 等库进行优化，但这些模式驱动着关键的系统设计决策。加速这些矩阵操作的硬件架构，包括 GPU 张量核心 6 和专门的 AI 加速器，在第十一章中有所介绍。

### 系统影响

神经网络架构表现出独特的系统级特征，这些特征在系统分析中具有三个核心维度：内存需求、计算需求和数据移动。这个框架使得对算法模式如何影响系统设计决策进行一致分析成为可能，揭示了共性和架构特定的优化。我们在分析每个架构家族时都应用了这个框架。这些系统级考虑因素直接建立在第三章中讨论的神经网络计算模式、内存系统和系统扩展的基础概念之上。

#### 内存需求

对于密集模式处理，内存需求源于存储和访问权重、输入和中间结果。在我们的 MNIST 示例中，将 784 维输入层连接到 100 个神经元的隐藏层需要 78,400 个权重参数。每次正向传递都必须访问所有这些权重，以及输入数据和中间结果。全全连接模式意味着这些访问没有固有的局部性；每个输出都需要每个输入及其对应的权重。

这些内存访问模式通过精心组织数据和使用优化。现代处理器通过专门的方法处理这些密集访问模式：CPU 利用其缓存层次结构进行数据重用，而 GPU 采用专为高带宽访问大参数矩阵设计的内存架构。框架通过高性能矩阵操作（如我们早期分析中所述）抽象这些优化。

#### 计算需求

核心计算围绕嵌套循环中的乘加操作进行。每个输出值需要的乘加次数与输入数量相同。对于 MNIST，每个输出神经元需要 784 次乘加。在隐藏层有 100 个神经元的情况下，单个输入图像需要进行 78,400 次乘加。虽然这些操作很简单，但它们的数量和排列对处理资源提出了特定的要求。

这种计算结构使得现代硬件能够实现特定的优化策略。密集矩阵乘法模式可以在多个处理单元之间并行化，每个单元处理不同的神经元子集。现代硬件加速器通过专门的矩阵乘法单元利用这一点，而软件框架自动将这些操作转换为优化的 BLAS（基本线性代数子程序）调用。CPU 和 GPU 都可以通过仔细地分块计算来利用缓存局部性，以最大化数据重用，尽管它们的具体方法基于其架构优势而有所不同。

#### 数据移动

MLPs 中的全全连接模式产生了显著的数据移动需求。每个乘加操作需要三份数据：一个输入值、一个权重值和运行总和。对于我们的 MNIST 示例层，计算单个输出值需要将 784 个输入和 784 个权重移动到计算发生的任何地方。这种移动模式对每个 100 个输出神经元都重复一次，在内存和计算单元之间产生了大量的数据传输需求。

可预测的数据移动模式使得战略数据调度和传输优化成为可能。不同的架构通过各种机制来应对这一挑战；CPU 使用预取和多级缓存，而 GPU 则采用高带宽内存系统和通过大量线程隐藏延迟。软件框架通过内存管理系统来编排这些数据移动，减少冗余传输并增加数据重用。

对 MLP 计算需求的这种分析揭示了一个关键见解：虽然密集连接提供了通用逼近能力，但当数据表现出固有结构时，它会产生显著的不效率。这种架构假设与数据特征之间的不匹配促使开发出专门的方法，这些方法可以利用结构模式来实现计算上的收益。

## CNNs：空间模式处理

MLPs 的计算强度和参数需求在应用于结构化数据时揭示了不匹配。在第 4.1 节中概述的计算复杂性考虑的基础上，这种低效促使开发出利用固有数据结构的架构模式。

卷积神经网络（CNNs）作为解决这一挑战的方案出现（Lecun 等人 1998；Krizhevsky, Sutskever, 和 Hinton 2017a），体现了一种特定的归纳偏差：它们假设空间局部性和平移不变性，即附近的像素相关，模式可以出现在任何地方。这种架构假设使得两个关键创新得以实现，从而提高了空间结构数据的效率。参数共享允许相同的特征检测器应用于不同的空间位置，将参数从数百万减少到数千，同时提高泛化能力。局部连接性将连接限制在空间相邻区域，反映了空间邻近与特征相关性的见解。

***卷积神经网络（CNNs）*** 是一种利用*局部连接*和*参数共享*通过*可学习滤波器*构建具有比全连接网络少得多的参数的*层次表示*的神经网络架构。

这些建筑创新代表了深度学习设计中的权衡：当数据表现出已知结构时，为了实现实际效率的提升而牺牲了 MLPs 的理论通用性。虽然 MLPs 独立处理每个输入元素，但 CNNs 通过利用空间关系来实现计算节省和提升在视觉任务上的性能。

### 模式处理需求

空间模式处理解决数据点之间的关系依赖于它们的相对位置或邻近性的场景。考虑处理自然图像：像素与其邻居的关系对于检测边缘、纹理和形状很重要。这些局部模式随后以分层的方式组合，形成更复杂的功能：边缘形成形状，形状形成对象，对象形成场景。

这种分层空间模式处理在许多领域都有出现。在计算机视觉中，局部像素模式形成边缘和纹理，这些纹理组合成可识别的对象。语音处理依赖于邻近时间段的模式来识别音素和单词。传感器网络分析物理邻近传感器之间的相关性，以理解环境模式。医学成像依赖于识别组织模式，这些模式指示生物结构。

专注于图像处理来阐述这些原则，如果我们想在图像中检测一只猫，必须识别某些空间模式：耳朵的三角形形状、脸部圆润的轮廓、毛发的纹理。重要的是，这些模式无论出现在图像的哪个位置都保持其意义。猫无论出现在左上角还是右下角，仍然是猫。这表明空间模式处理有两个关键要求：检测局部模式的能力以及识别这些模式而不考虑它们位置的能力 7。图 4.2 展示了卷积神经网络通过分层特征提取实现这一点，简单模式在连续层中组合成越来越复杂的表示。

![图片](img/file55.svg)

图 4.2：**空间特征提取**：卷积神经网络通过在整个输入上应用可学习的过滤器来识别图像中独立于其位置的图案，从而实现鲁棒的对象识别。这些过滤器检测局部特征，并在图像上重复应用这些特征创建平移不变性，即无论图案的位置如何都能识别图案的能力。

这引导我们到卷积神经网络架构（CNN），由 Yann LeCun8 和 Y. LeCun 等人 1989 开创。CNN 通过几个关键创新实现这一点：参数共享 9、局部连接和平移不变性 10。

### 算法结构

CNN 中的核心操作可以用数学表达式表示为：

这个方程描述了 CNN 如何处理空间数据。 <semantics><msubsup><mi>𝐇</mi><mrow><mi>i</mi><mo>,</mo><mi>j</mi><mo>,</mo><mi>k</mi></mrow><mrow><mo stretchy="true" form="prefix">(</mo><mi>l</mi><mo stretchy="true" form="postfix">)</mo></mrow></msubsup><annotation encoding="application/x-tex">\mathbf{H}^{(l)}_{i,j,k}</annotation></semantics> 是在层 <semantics><mi>l</mi><annotation encoding="application/x-tex">l</annotation></semantics> 的通道 <semantics><mi>k</mi><annotation encoding="application/x-tex">k</annotation></semantics> 中空间位置 <semantics><mrow><mo stretchy="true" form="prefix">(</mo><mi>i</mi><mo>,</mo><mi>j</mi><mo stretchy="true" form="postfix">)</mo></mrow><annotation encoding="application/x-tex">(i,j)</annotation></semantics> 的输出。三重求和遍历滤波器维度：<semantics><mrow><mo stretchy="true" form="prefix">(</mo><mi>d</mi><mi>i</mi><mo>,</mo><mi>d</mi><mi>j</mi><mo stretchy="true" form="postfix">)</mo></mrow><annotation encoding="application/x-tex">(di,dj)</annotation></semantics> 扫描空间滤波器大小，而 <semantics><mi>c</mi><annotation encoding="application/x-tex">c</annotation></semantics> 覆盖输入通道。 <semantics><msubsup><mi>𝐖</mi><mrow><mi>d</mi><mi>i</mi><mo>,</mo><mi>d</mi><mi>j</mi><mo>,</mo><mi>c</mi><mo>,</mo><mi>k</mi></mrow><mrow><mo stretchy="true" form="prefix">(</mo><mi>l</mi><mo stretchy="true" form="postfix">)</mo></mrow></msubsup><annotation encoding="application/x-tex">\mathbf{W}^{(l)}_{di,dj,c,k}</annotation></semantics> 表示滤波器权重，捕捉局部空间模式。与将所有输入连接到输出的 MLPs 不同，CNN 只连接局部空间邻域。

进一步分解符号，<semantics><mrow><mo stretchy="true" form="prefix">(</mo><mi>i</mi><mo>,</mo><mi>j</mi><mo stretchy="true" form="postfix">)</mo></mrow><annotation encoding="application/x-tex">(i,j)</annotation></semantics> 对应空间位置，<semantics><mi>k</mi><annotation encoding="application/x-tex">k</annotation></semantics> 指的是输出通道，<semantics><mi>c</mi><annotation encoding="application/x-tex">c</annotation></semantics> 指的是输入通道，而 <semantics><mrow><mo stretchy="true" form="prefix">(</mo><mi>d</mi><mi>i</mi><mo>,</mo><mi>d</mi><mi>j</mi><mo stretchy="true" form="postfix">)</mo></mrow><annotation encoding="application/x-tex">(di,dj)</annotation></semantics> 覆盖了局部感受野 11。与 MLPs 的密集矩阵乘法不同，这个操作：

+   处理局部邻域（通常是 <semantics><mrow><mn>3</mn><mo>×</mo><mn>3</mn></mrow><annotation encoding="application/x-tex">3\times 3</annotation></semantics> 或 <semantics><mrow><mn>5</mn><mo>×</mo><mn>5</mn></mrow><annotation encoding="application/x-tex">5\times 5</annotation></semantics>）

+   在每个空间位置重复使用相同的权重

+   在其输出中保持空间结构

为了具体说明这一过程，考虑 MNIST 数字分类任务，该任务使用<semantics><mrow><mn>28</mn><mo>×</mo><mn>28</mn></mrow><annotation encoding="application/x-tex">28\times 28</annotation></semantics>灰度图像。每个卷积层应用一组过滤器（例如，<semantics><mrow><mn>3</mn><mo>×</mo><mn>3</mn></mrow><annotation encoding="application/x-tex">3\times 3</annotation></semantics>），这些过滤器在图像上滑动，计算局部加权求和。如果我们使用 32 个过滤器并使用填充来保持维度，该层将产生一个<semantics><mrow><mn>28</mn><mo>×</mo><mn>28</mn><mo>×</mo><mn>32</mn></mrow><annotation encoding="application/x-tex">28\times 28\times 32</annotation></semantics>输出，其中每个空间位置包含其局部邻域的 32 个不同特征测量。这与多层感知器（MLP）方法形成鲜明对比，在处理之前，整个图像被展平成一个 784 维向量。

该算法结构直接实现了空间模式处理的需求，创建了影响系统设计的独特计算模式。与 MLPs 不同，卷积网络保留了空间局部性，利用上述建立的分层特征提取原则。这些特性推动了人工智能加速器中的架构优化，其中数据重用、瓦片化和并行滤波计算等操作对于性能至关重要。

**数学背景**

群论为理解数据中的对称性和变换提供了数学框架。平移等变性意味着输入的平移会产生相应平移的输出——这是 CNN 能够识别位置无关模式的关键属性。

群论为理解卷积神经网络（CNN）的有效性 12 提供了框架，这为理解数据中的对称性提供了数学框架。平移不变性出现是因为卷积与平移群等变——如果我们移动输入图像，输出特征图也会以相同的量移动。从数学上讲，如果<semantics><msub><mi>T</mi><mi>v</mi></msub><annotation encoding="application/x-tex">T_v</annotation></semantics>表示由向量<semantics><mi>v</mi><annotation encoding="application/x-tex">v</annotation></semantics>进行的平移，那么一个卷积层<semantics><mi>f</mi><annotation encoding="application/x-tex">f</annotation></semantics>满足：<semantics><mrow><mi>f</mi><mrow><mo stretchy="true" form="prefix">(</mo><msub><mi>T</mi><mi>v</mi></msub><mi>x</mi><mo stretchy="true" form="postfix">)</mo></mrow><mo>=</mo><msub><mi>T</mi><mi>v</mi></msub><mi>f</mi><mrow><mo stretchy="true" form="prefix">(</mo><mi>x</mi><mo stretchy="true" form="postfix">)</mo></mrow></mrow><annotation encoding="application/x-tex">f(T_v x) = T_v f(x)</annotation></semantics>。这种等变性属性使得 CNN 能够学习跨空间位置的泛化特征。

卷积的选择反映了关于神经网络设计中归纳偏置 13 的更深层原则。通过限制连接到局部邻域并在空间位置间共享参数，CNN 编码了关于视觉数据结构的先验知识：重要的特征是局部的和平移不变的。这种架构约束减少了网络必须搜索的假设空间 14，与全连接网络相比，这使得从有限数据中学习更加高效。

CNN 通过其分层结构自然地实现了层次化表示学习。早期层检测具有小感受野的低级特征，如边缘和纹理，而深层层将这些特征组合成具有更大感受野的越来越复杂的模式。这种层次化组织反映了视觉皮层的结构，使得 CNN 能够构建组合表示：复杂对象被表示为更简单部分的组合。这种数学基础的来源是，堆叠卷积层创建了一个树状依赖结构，其中每个深层神经元依赖于一个指数级大的输入像素集，从而能够有效地表示层次化模式。

#### 建筑特性

与 MLP 相比，参数共享通过在空间位置间重用相同的过滤器大大减少了复杂性。这种共享体现了这样的假设：有用的特征（如边缘或纹理）可以出现在图像的任何地方，使得相同的特征检测器在所有空间位置都有价值。

CNNs 的架构效率使得可以通过专用技术进行进一步优化。深度可分离卷积将标准卷积分解为深度和点操作，对于典型的移动部署，计算量减少了 8-9 倍。通道剪枝根据重要性指标消除整个特征图，在小于 1%的精度损失下实现了 40-50%的 FLOPs 减少。这些优化策略建立在空间局部性原理之上，第十章探讨了特定硬件的实现，第十一章详细说明了现代处理器如何利用卷积固有的数据重用模式。

如图 4.3 所示，卷积操作涉及在输入图像上滑动一个小滤波器以生成特征图 15。此过程捕捉局部结构同时保持平移不变性。对于卷积网络的交互式视觉探索，[CNN Explainer](https://poloclub.github.io/cnn-explainer/)项目提供了一个有见地的演示，展示了这些网络是如何构建的。

![图片](img/file56.svg)

图 4.3：卷积操作通过使用在图像上滑动的滤波器进行局部特征提取来处理输入数据，以识别位置无关的模式。

### 计算映射

卷积操作创建的计算模式与 MLP 密集矩阵乘法不同。这种从数学运算到实现细节的转换揭示了独特的计算特性。

第一种实现，`conv_layer_spatial`（如列表 4.3 所示），使用高级卷积操作简洁地表达计算。这在深度学习框架中很典型，其中优化的库处理底层复杂性。

列表 4.3：这种分层方法通过使用结合核和偏置的卷积操作进行特征提取来处理输入数据，然后在应用激活函数之前。

```py
def conv_layer_spatial(input, kernel, bias):
    output = convolution(input, kernel) + bias
    return activation(output)
```

逻辑模型与物理执行之间的桥梁对于理解 CNN 系统需求至关重要。虽然高级卷积操作看起来像是一个简单的滑动窗口计算，但硬件必须编排复杂的数据移动模式并利用空间局部性以提高效率。

第二种实现，conv_layer_compute（见列表 4.4），揭示了实际的计算模式：嵌套循环处理每个空间位置，将相同的滤波器权重应用于输入的局部区域。这七个嵌套循环揭示了卷积计算结构的真正本质以及它所创造的优化机会。

列表 4.4：**嵌套循环**：卷积层通过多个嵌套循环处理批量图像、空间维度、输出通道、核窗口和输入特征，揭示了卷积操作的详细计算结构。

```py
def conv_layer_compute(input, kernel, bias):
    # Loop 1: Process each image in batch
    for image in range(batch_size):

        # Loop 2&3: Move across image spatially
        for y in range(height):
            for x in range(width):

                # Loop 4: Compute each output feature
                for out_channel in range(num_output_channels):
                    result = bias[out_channel]

                    # Loop 5&6: Move across kernel window
                    for ky in range(kernel_height):
                        for kx in range(kernel_width):

                            # Loop 7: Process each input feature
                            for in_channel in range(
                                num_input_channels
                            ):
                                # Get input value from correct window position
                                in_y = y + ky
                                in_x = x + kx
                                # Perform multiply-accumulate operation
                                result += (
                                    input[
                                        image, in_y, in_x, in_channel
                                    ]
                                    * kernel[
                                        ky,
                                        kx,
                                        in_channel,
                                        out_channel,
                                    ]
                                )

                    # Store result for this output position
                    output[image, y, x, out_channel] = result
```

七层嵌套循环揭示了计算的各个方面：

+   外层循环（1-3）管理位置：哪个图像以及图像中的位置

+   中间循环（4）处理输出特征：计算不同的学习模式

+   内层循环（5-7）执行实际的卷积操作：滑动核窗口

详细考察此过程，外两层循环（`for y`和`for x`）遍历输出特征图中的每个空间位置（对于 MNIST 示例，这遍历了所有<semantics><mrow><mn>28</mn><mo>×</mo><mn>28</mn></mrow><annotation encoding="application/x-tex">28\times 28</annotation></semantics>位置）。在每个位置，计算每个输出通道的值（`for k`循环），代表不同的学习特征或模式——32 个不同的特征检测器。

内层三个循环在每个位置实现实际的卷积操作。对于每个输出值，我们处理输入的局部<semantics><mrow><mn>3</mn><mo>×</mo><mn>3</mn></mrow><annotation encoding="application/x-tex">3\times 3</annotation></semantics>区域（`dy`和`dx`循环）以及所有输入通道（`for c`循环）。这产生了一个滑动窗口效果，其中相同的<semantics><mrow><mn>3</mn><mo>×</mo><mn>3</mn></mrow><annotation encoding="application/x-tex">3\times 3</annotation></semantics>滤波器在图像上移动，执行滤波器权重和局部输入值之间的乘加操作。与 MLP 的全局连接性不同，这种局部处理模式意味着每个输出值只依赖于输入的小邻域。

对于我们的 MNIST 示例，使用<semantics><mrow><mn>3</mn><mo>×</mo><mn>3</mn></mrow><annotation encoding="application/x-tex">3\times 3</annotation></semantics>滤波器和 32 个输出通道，每个输出位置只需要每个输入通道 9 次乘加操作，而我们的 MLP 层需要 784 次操作。这个操作必须对每个空间位置<semantics><mrow><mo stretchy="true" form="prefix">(</mo><mn>28</mn><mo>×</mo><mn>28</mn><mo stretchy="true" form="postfix">)</mo></mrow><annotation encoding="application/x-tex">(28\times 28)</annotation></semantics>和每个输出通道（32）重复进行。

尽管每个输出使用的操作较少，但空间结构创建了不同的内存访问和计算模式，系统必须处理。这些模式影响系统设计，既带来了优化挑战，也带来了优化机会。

### 系统影响

CNN 在三个分析维度上显示出与 MLP 密集连接显著不同的系统级模式。

#### 内存需求

对于卷积层，内存需求主要集中在两个关键组件上：滤波器权重和特征图。与需要存储完整连接矩阵的 MLP 不同，CNN 使用小型、可重用的滤波器。对于一个典型的 CNN 处理 224×224 的 ImageNet 图像，一个包含 64 个<semantics><mrow><mn>3</mn><mo>×</mo><mn>3</mn></mrow><annotation encoding="application/x-tex">3\times 3</annotation></semantics>大小滤波器的卷积层只需要存储 576 个权重参数<semantics><mrow><mo stretchy="true" form="prefix">(</mo><mn>3</mn><mo>×</mo><mn>3</mn><mo>×</mo><mn>64</mn><mo stretchy="true" form="postfix">)</mo></mrow><annotation encoding="application/x-tex">(3\times 3\times 64)</annotation></semantics>，这比等效的全连接处理所需的数百万个权重少得多。系统必须存储所有空间位置的特征图，从而产生不同的内存需求。一个 224×224 的输入和 64 个输出通道需要存储 320 万个激活值（224×224×64）。

这些内存访问模式表明，通过权重重用和仔细的特征图管理可以实现优化的机会。处理器通过缓存用于跨位置重用的滤波器权重来优化这些空间模式，同时流式传输特征图数据。框架通过专门的内存布局实现空间优化，这些布局能够实现滤波器重用和特征图访问的空间局部性。CPU 和 GPU 采用不同的方法来处理这个问题。CPU 使用其缓存层次结构来保持常用滤波器的驻留，而 GPU 则采用专为图像处理的空间访问模式设计的专用内存架构。这些专用处理器的详细架构设计原则在第十一章中有所介绍。

#### 计算需求

CNNs 的核心计算涉及在空间位置上反复应用小型滤波器。每个输出值都需要在滤波器区域内进行局部乘累加操作。对于使用<semantics><mrow><mn>3</mn><mo>×</mo><mn>3</mn></mrow><annotation encoding="application/x-tex">3\times 3</annotation></semantics>滤波器和 64 个输出通道的 ImageNet 处理，计算一个空间位置涉及 576 次乘累加<semantics><mrow><mo stretchy="true" form="prefix">(</mo><mn>3</mn><mo>×</mo><mn>3</mn><mo>×</mo><mn>64</mn><mo stretchy="true" form="postfix">)</mo></mrow><annotation encoding="application/x-tex">(3\times 3\times 64)</annotation></semantics>，并且这必须在所有 50,176 个空间位置（224×224）上重复进行。虽然每个单独的计算涉及的运算比 MLP 层少，但由于空间重复，总的计算负载仍然很大。

这种计算模式比 MLPs 提供了不同的优化机会。卷积操作的规则、重复性质通过结构化并行化实现了高效的硬件利用。现代处理器以各种方式利用这种模式。CPU 利用 SIMD 指令 16 同时处理多个过滤器位置，而 GPU 在空间位置和通道上并行化计算。包括专门的卷积优化和稀疏模式在内的进一步降低这些计算需求的模型优化技术，在第十章中详细说明。

#### 数据移动

卷积的滑动窗口模式创建了一个独特的数据移动配置文件。与 MLPs 中每个权重在每个前向传递中只使用一次不同，CNN 的过滤器权重在过滤器在空间位置上滑动时被多次重用。对于 ImageNet 处理，每个<semantics><mrow><mn>3</mn><mo>×</mo><mn>3</mn></mrow><annotation encoding="application/x-tex">3\times 3</annotation></semantics>过滤器权重被重用 50,176 次（在 224×224 特征图中的每个位置都使用一次）。这带来了不同的挑战：系统必须在保持过滤器权重稳定的同时，将输入特征流经计算单元。

可预测的空间访问模式使得数据移动优化成为可能。不同的架构通过专门的机制来处理这种移动模式。CPU 在流经输入特征的同时，将频繁使用的过滤器权重保持在缓存中。GPU 采用针对空间局部性优化的内存架构，并为高效的滑动窗口操作提供硬件支持。深度学习框架通过组织计算来最大化过滤器权重的重用并最小化冗余的特征图访问，从而协调这些移动。

## RNNs：序列模式处理

卷积神经网络通过利用空间局部性实现了效率提升，但当模式依赖于时间顺序而不是空间邻近性时，其架构假设就会失败。虽然 CNN 在通过共享特征检测器识别数据中的“什么”方面表现出色，但它们无法捕捉“何时”事件发生或它们随时间如何相关。这种限制在诸如自然语言处理等领域表现出来，其中词义取决于句子上下文，以及在时间序列分析中，未来的值依赖于历史模式。

序列数据提出了与空间处理不同的挑战：模式可以跨越任意的时间距离，使得固定大小的核无效。虽然空间卷积利用了邻近像素通常相关的原理，但时间关系运作方式不同。重要的连接可能跨越数百或数千个时间步，与邻近性没有相关性。传统的前馈架构，包括 CNN，独立处理每个输入，无法维持这些长距离依赖所需的时序上下文。

循环神经网络通过体现时间归纳偏差来解决这个问题（Elman 1990；Hochreiter 和 Schmidhuber 1997）：它们假设序列依赖性，即信息的顺序很重要，过去影响现在。这个架构假设指导了将记忆作为计算模型组件的引入。RNNs 不是独立处理输入，而是维持一个内部状态，将信息从先前的时间步传播出去，使网络能够根据历史上下文条件化其当前输出。这种架构体现了一种另一种权衡：虽然 CNN 牺牲了理论上的通用性以换取空间效率，但 RNN 引入了计算依赖性，这挑战了并行执行，以换取时序处理能力。

***循环神经网络 (RNNs)*** 是一种序列神经网络架构，通过**循环连接**在时间步长中维持*内部记忆状态*，以*序列计算*为代价实现*可变长度序列处理*。

**覆盖说明**

本节涵盖了 RNNs，强调了它们对序列处理的核心贡献以及影响现代注意力机制的架构原则。虽然 RNNs 引入了关键概念——记忆状态、时序依赖和序列计算——但当代实践越来越倾向于基于注意力的架构进行序列建模。我们专注于基础原则而不是广泛的实现变体，对注意力机制和 Transformer（第 4.5 节）进行了深入探讨，这些机制在很大程度上已经取代了 RNNs 在生产系统中的应用，同时直接基于循环架构的见解。

### 模式处理需求

序列模式处理解决当前输入解释依赖于先前信息的情况。在自然语言处理中，词义往往严重依赖于句子中的先前单词。上下文决定了解释，正如根据周围术语的不同，单词的多种含义所证明的那样。同样，在语音识别中，音素解释依赖于周围的声音，而金融预测需要理解历史数据模式。

序列处理中的挑战在于随着时间的推移维护和更新相关上下文。人类的文本理解并不是从每个单词开始重新启动；相反，随着新信息的处理，一个持续的理解在不断发展。同样，时间序列数据处理会遇到跨越不同时间尺度的模式，从即时依赖关系到长期趋势。这需要一种能够随着时间的推移保持状态并根据新输入更新状态的架构。

这些需求转化为特定的架构要求：系统必须维护内部状态以捕捉时间上下文，根据新输入更新此状态，并学习哪些历史信息对当前预测是相关的。与处理固定大小输入的 MLPs 和 CNNs 不同，序列处理必须适应可变长度的序列，同时保持计算效率。这些需求最终导致了循环神经网络（RNN）架构的出现。

### 算法结构

RNNs 通过循环连接处理序列处理，使其与 MLPs 和 CNNs 区分开来。RNNs 不仅仅是将输入映射到输出，而是在每个时间步更新内部状态，创建一种记忆机制，将信息向前传播。这种时间依赖性建模能力最初由 Elman (1990) 探索，他展示了 RNN 识别时间依赖数据结构的能力。基本的 RNNs 受到梯度消失问题 17 的困扰，限制了它们学习长期依赖的能力。

基本 RNN 的核心操作可以用数学公式表示为：<semantics><mrow><msub><mi>𝐡</mi><mi>t</mi></msub><mo>=</mo><mi>f</mi><mrow><mo stretchy="true" form="prefix">(</mo><msub><mi>𝐖</mi><mrow><mi>h</mi><mi>h</mi></mrow></msub><msub><mi>𝐡</mi><mrow><mi>t</mi><mo>−</mo><mn>1</mn></mrow></msub><mo>+</mo><msub><mi>𝐖</mi><mrow><mi>x</mi><mi>h</mi></mrow></msub><msub><mi>𝐱</mi><mi>t</mi></msub><mo>+</mo><msub><mi>𝐛</mi><mi>h</mi></msub><mo stretchy="true" form="postfix">)</mo></mrow></mrow> <annotation encoding="application/x-tex">\mathbf{h}_t = f(\mathbf{W}_{hh}\mathbf{h}_{t-1} + \mathbf{W}_{xh}\mathbf{x}_t + \mathbf{b}_h)</annotation></semantics> 其中 <semantics><msub><mi>𝐡</mi><mi>t</mi></msub><annotation encoding="application/x-tex">\mathbf{h}_t</annotation></semantics> 表示时间 <semantics><mi>t</mi><annotation encoding="application/x-tex">t</annotation></semantics> 的隐藏状态，<semantics><msub><mi>𝐱</mi><mi>t</mi></msub><annotation encoding="application/x-tex">\mathbf{x}_t</annotation></semantics> 表示时间 <semantics><mi>t</mi><annotation encoding="application/x-tex">t</annotation></semantics> 的输入，<semantics><msub><mi>𝐖</mi><mrow><mi>h</mi><mi>h</mi></mrow></msub><annotation encoding="application/x-tex">\mathbf{W}_{hh}</annotation></semantics> 包含循环权重，而 <semantics><msub><mi>𝐖</mi><mrow><mi>x</mi><mi>h</mi></mrow></msub><annotation encoding="application/x-tex">\mathbf{W}_{xh}</annotation></semantics> 包含输入权重，如图图 4.4 中的展开网络结构所示。

在词序列处理中，每个词可以表示为一个 100 维向量（<semantics><msub><mi>𝐱</mi><mi>t</mi></msub><annotation encoding="application/x-tex">\mathbf{x}_t</annotation></semantics>），具有 128 维的隐藏状态（<semantics><msub><mi>𝐡</mi><mi>t</mi></msub><annotation encoding="application/x-tex">\mathbf{h}_t</annotation></semantics>）。在每一个时间步，网络将当前输入与其先前状态结合以更新其序列理解，建立一个能够捕捉时间步之间模式的记忆机制。

这种循环结构通过保持内部状态并向前传播信息来满足序列处理需求。RNNs 不是独立处理所有输入，而是通过迭代更新一个基于当前输入和先前隐藏状态的隐藏状态来处理序列数据，如图图 4.4 所示。这种架构适用于包括语言建模、语音识别和时间序列预测在内的任务。

RNN 实现了一种递归算法，其中每个时间步的函数调用依赖于前一个调用的结果。类似于通过调用栈维护状态的递归函数，RNN 通过其隐藏向量来维护状态。数学公式<semantics><mrow><msub><mi>𝐡</mi><mi>t</mi></msub><mo>=</mo><mi>f</mi><mrow><mo stretchy="true" form="prefix">(</mo><msub><mi>𝐡</mi><mrow><mi>t</mi><mo>−</mo><mn>1</mn></mrow></msub><mo>,</mo><msub><mi>𝐱</mi><mi>t</mi></msub><mo stretchy="true" form="postfix">)</mo></mrow></mrow><annotation encoding="application/x-tex">\mathbf{h}_t = f(\mathbf{h}_{t-1}, \mathbf{x}_t)</annotation></semantics>直接对应于递归函数定义，其中`f(n) = g(f(n-1), input(n))`。这种对应关系解释了 RNN 处理可变长度序列的能力：正如递归算法通过递归应用相同的函数来处理任意长度的列表一样，RNN 通过应用相同的循环计算来处理任意长度的序列。

#### 效率和优化

顺序处理会创建计算瓶颈，但为内存使用提供了独特的效率特性。RNN 在隐藏状态存储方面实现恒定的内存开销，无论序列长度如何，这使得它们在处理长序列时极为内存高效。而 Transformers 需要 O(n²)的内存来处理长度为 n 的序列，RNN 则保持固定的内存使用，使得在适度硬件上处理数千步长的序列成为可能。

隐藏到隐藏连接的结构化剪枝可以实现 10 倍的速度提升，同时保持序列建模能力。循环权重矩阵<semantics><msub><mi>W</mi><mrow><mi>h</mi><mi>h</mi></mrow></msub><annotation encoding="application/x-tex">W_{hh}</annotation></semantics>通常占大隐藏状态参数数量的主导地位，但基于幅度的剪枝表明，其中 70-80%的连接对时间依赖性的贡献很小。块结构化剪枝在保持计算效率的同时，实现了显著的模型压缩。

顺序操作会累积量化误差，需要仔细放置量化点和进行梯度缩放以实现稳定的低精度训练。与量化误差保持局部化的前馈网络不同，RNN 误差会随时间传播，使得 INT8 量化更具挑战性。需要采用每时间步量化方案和仔细处理隐藏状态精度，以在量化 RNN 部署中保持准确性。

![图片](img/file57.svg)

图 4.4：**循环神经网络展开**：RNN 通过维护一个包含先前时间步信息的状态来处理顺序数据，如图所示。展开的结构明确表示了由循环权重建模的时间依赖性，使网络能够学习可变长度序列中的模式。

### 计算映射

RNN 顺序处理创建的计算模式与 MLPs 和 CNNs 都不同，扩展了第 4.1 节中讨论的架构多样性。这种实现方法表明时间依赖性转化为特定的计算需求。

如列表 4.5 所示，`rnn_layer_step`函数展示了在深度学习框架中找到的高级矩阵运算的使用。它处理单个时间步，接受当前输入`x_t`和前一个隐藏状态`h_prev`，以及两个权重矩阵：`W_hh`用于隐藏到隐藏连接，`W_xh`用于输入到隐藏连接。通过矩阵乘法运算(`matmul`)，它将先前状态和当前输入合并以生成下一个隐藏状态。

列表 4.5：**RNN 层步骤**：神经网络通过整合当前输入和过去状态的转换来处理顺序数据。

```py
def rnn_layer_step(x_t, h_prev, W_hh, W_xh, b):
    # x_t: input at time t (batch_size × input_dim)
    # h_prev: previous hidden state (batch_size × hidden_dim)
    # W_hh: recurrent weights (hidden_dim × hidden_dim)
    # W_xh: input weights (input_dim × hidden_dim)
    h_t = activation(matmul(h_prev, W_hh) + matmul(x_t, W_xh) + b)
    return h_t
```

理解 RNN 系统的影响需要检查优雅的数学抽象如何转化为硬件执行模式。简单的递归关系`h_t = tanh(W_hh h_{t-1} + W_xh x_t + b)`隐藏了一个创建独特挑战的计算结构：防止并行化的顺序依赖性、与前馈网络不同的内存访问模式，以及影响系统设计的状态管理需求。

详细实现(列表 4.6)揭示了数学抽象背后的计算现实。嵌套循环结构揭示了顺序处理如何在系统优化中创造限制和机会。

列表 4.6：**循环层计算**：通过涉及先前状态和当前输入的顺序转换来计算每个时间步的隐藏状态。

```py
def rnn_layer_compute(x_t, h_prev, W_hh, W_xh, b):
    # Initialize next hidden state
    h_t = np.zeros_like(h_prev)

    # Loop 1: Process each sequence in the batch
    for batch in range(batch_size):
        # Loop 2: Compute recurrent contribution
        # (h_prev × W_hh)
        for i in range(hidden_dim):
            for j in range(hidden_dim):
                h_t[batch, i] += h_prev[batch, j] * W_hh[j, i]

        # Loop 3: Compute input contribution (x_t × W_xh)
        for i in range(hidden_dim):
            for j in range(input_dim):
                h_t[batch, i] += x_t[batch, j] * W_xh[j, i]

        # Loop 4: Add bias and apply activation
        for i in range(hidden_dim):
            h_t[batch, i] = activation(h_t[batch, i] + b[i])

    return h_t
```

`rnn_layer_compute`中的嵌套循环揭示了 RNNs 的核心计算模式（参见列表 4.6）。循环 1 独立处理批次中的每个序列，从而允许批处理级别的并行性。在每个批次项中，循环 2 通过循环权重`W_hh`计算前一个隐藏状态如何影响下一个状态。然后，循环 3 通过输入权重`W_xh`将当前输入的新信息纳入。最后，循环 4 添加偏差并应用激活函数以产生新的隐藏状态。

对于输入维度为 100 和隐藏状态维度为 128 的序列处理任务，每个时间步需要两次矩阵乘法：一次是用于循环连接的<semantics><mrow><mn>128</mn><mo>×</mo><mn>128</mn></mrow><annotation encoding="application/x-tex">128\times 128</annotation></semantics>，另一次是用于输入投影的<semantics><mrow><mn>100</mn><mo>×</mo><mn>128</mn></mrow><annotation encoding="application/x-tex">100\times 128</annotation></semantics>。虽然单个时间步可以在批处理元素之间并行处理，但时间步本身必须按顺序处理。这创建了一种独特的计算模式，系统必须处理。

### 系统影响

在为 MLPs 建立的解析框架之后，RNN 在内存需求、计算需求和数据处理方面表现出独特的模式，与密集和空间处理架构有显著差异。

#### 内存需求

RNN 需要存储两组权重（输入到隐藏和隐藏到隐藏）以及隐藏状态。对于输入维度为 100 和隐藏状态维度为 128 的例子，这需要存储 12,800 个用于输入投影的权重<semantics><mrow><mo stretchy="true" form="prefix">(</mo><mn>100</mn><mo>×</mo><mn>128</mn><mo stretchy="true" form="postfix">)</mo></mrow><annotation encoding="application/x-tex">(100\times 128)</annotation></semantics>和 16,384 个用于循环连接的权重<semantics><mrow><mo stretchy="true" form="prefix">(</mo><mn>128</mn><mo>×</mo><mn>128</mn><mo stretchy="true" form="postfix">)</mo></mrow><annotation encoding="application/x-tex">(128\times 128)</annotation></semantics>。与 CNN 中权重在空间位置间重用不同，RNN 的权重在时间步间重用。系统必须维护隐藏状态，这是内存使用和访问模式的关键因素。

这些内存访问模式与 MLPs 和 CNNs 不同。处理器通过在缓存中保持权重矩阵并流过时间元素来优化顺序模式。框架通过批处理序列和管理时间步之间的隐藏状态存储来优化时间处理。CPU 和 GPU 通过不同的策略来处理这个问题；CPU 利用其缓存层次结构来实现权重重用；同时，GPU 使用专为在顺序操作中维护状态而设计的专用内存架构。关于顺序处理的专用硬件优化，包括内存银行和流水线架构，在第十一章中详细说明。

#### 计算需求

RNN 的核心计算涉及在时间步内反复应用权重矩阵。对于每个时间步，我们执行两次矩阵乘法：一次是与输入权重相乘，另一次是与循环权重相乘。在我们的例子中，处理单个时间步需要 12,800 次乘加操作用于输入投影<semantics><mrow><mo stretchy="true" form="prefix">(</mo><mn>100</mn><mo>×</mo><mn>128</mn><mo stretchy="true" form="postfix">)</mo></mrow><annotation encoding="application/x-tex">(100\times 128)</annotation></semantics>和 16,384 次乘加操作用于循环连接<semantics><mrow><mo stretchy="true" form="prefix">(</mo><mn>128</mn><mo>×</mo><mn>128</mn><mo stretchy="true" form="postfix">)</mo></mrow><annotation encoding="application/x-tex">(128\times 128)</annotation></semantics>。

这种计算模式在关键方面与 MLP 和 CNN 不同：虽然我们可以在批处理元素之间并行化，但由于顺序依赖性，我们不能在时间步之间并行化。每个时间步必须在开始计算之前等待前一个步骤的隐藏状态。这就在算法的固有顺序性质和现代硬件中并行执行的需求之间产生了紧张关系。

处理器通过专门的方法解决顺序约束问题。CPU 在时间步内对操作进行流水线处理，同时保持时间顺序。GPU 将多个序列批量处理在一起，以维持高吞吐量，尽管存在顺序依赖性。软件框架通过序列打包和跨多个时间步展开计算等技术进一步优化，在可能的情况下，使并行处理资源的利用更加高效，同时尊重循环架构中固有的顺序约束。

#### 数据移动

RNN 中的顺序处理创建了一种独特的数据移动模式，这种模式与 MLP 和 CNN 都不同。虽然 MLP 在每次前向传递中只需要每个权重一次，而 CNN 在空间位置上重复使用权重，但 RNN 在时间步内重复使用其权重，同时需要仔细管理隐藏状态数据流。

对于我们的例子，具有 128 维隐藏状态，每个时间步必须：加载前一个隐藏状态（128 个值），访问两个权重矩阵（来自输入和循环连接的总权重为 29,184），并存储新的隐藏状态（128 个值）。这种模式在序列的每个元素上重复。与 CNN 不同，我们无法根据空间模式预测和预取数据，RNN 的数据移动是由时间依赖性驱动的。

不同的架构通过专门的机制处理这种顺序数据移动。CPU 在通过序列元素并管理隐藏状态更新时，在缓存中维护权重矩阵。GPU 采用优化了的状态信息维护的内存架构，在并行处理多个序列的同时，处理顺序操作。深度学习框架通过管理时间步之间的数据传输和优化批量操作来协调这些移动。

虽然 RNN 为顺序处理建立了概念，但它们的架构约束创造了瓶颈：顺序依赖性阻止了时间步之间的并行化，固定容量的隐藏状态为长序列创造了信息瓶颈，并且当重要关系跨越遥远位置时，时间邻近性假设就会崩溃。这些限制促使注意力机制的发展，通过动态、内容依赖的连接消除顺序处理约束。下一节将探讨注意力机制如何解决这些 RNN 限制，同时引入新的计算挑战。这种广泛的处理反映了注意力机制在现代机器学习系统中的主导地位以及它们对顺序模式处理的根本性重新构想。

## 注意力机制：动态模式处理

循环神经网络成功地引入了记忆来处理顺序依赖性，但它们的固定顺序处理创造了限制。RNN 按时间顺序处理信息，这使得难以捕捉远距离元素之间的关系，并且无法在序列位置之间并行化计算。更重要的是，RNN 假设时间邻近性与重要性相关——即附近的单词或时间步比远处的更相关。这种假设在许多实际场景中都会崩溃。

考虑以下句子：“The cat, which was sitting by the window overlooking the garden, was sleeping.” 在这里，“cat”和“sleeping”被多个间隔词分开，但它们构成了核心的主谓关系。RNN 架构会按顺序处理所有间隔元素，可能会在它们的固定容量隐藏状态中丢失这个关键联系。这种限制揭示了需要能够根据内容而不是位置识别和加权关系的架构。

注意力机制通过引入根据输入内容自适应的动态连接模式，成为解决这种架构约束的解决方案（Bahdanau, Cho, and Bengio 2014）。与按预定顺序处理具有固定关系的元素不同，注意力机制计算所有元素对之间的相关性，并相应地加权它们的交互。这代表了一种从结构约束到学习依赖的数据处理模式的转变。

**注意力机制**是计算序列元素之间**内容相关关系**的神经网络组件，通过**查询-键-值操作**，实现**选择性关注**相关信息和**长距离依赖**，而不受位置约束。

虽然注意力机制最初被用作循环架构中的组件，但 Transformer 架构(Vaswani 等人 2017)证明了仅注意力本身就可以完全取代顺序处理，创造了一种新的架构范式。

**Transformer**是基于**注意力机制**的神经网络架构，使用**多头自注意力**和**位置编码**来并行处理序列，而不是顺序处理，从而实现大规模的效率和推理。

### 模式处理需求

动态模式处理解决的是元素之间的关系不是由架构固定，而是从内容中产生的场景。语言翻译就是这种挑战的例证：在翻译“河边的银行”时，理解“银行”需要关注“河”，但在“银行批准了贷款”中，重要的关系是与“批准”和“贷款”。与按顺序处理信息的 RNN 或使用固定空间模式的 CNN 不同，需要一个能够动态确定哪些关系重要的架构。

超越语言，这种对动态处理的需求出现在许多领域。在蛋白质结构预测中，氨基酸之间的相互作用取决于它们的化学性质和空间排列。在图分析中，节点关系根据图结构和节点特征而变化。在文档分析中，不同部分之间的连接取决于语义内容，而不仅仅是邻近性。

综合这些需求，动态处理要求我们的处理架构具备特定的能力。系统必须计算所有元素对之间的关系，根据内容权衡这些关系，并使用这些权重来选择性地组合信息。与具有固定连接模式的前一代架构不同，动态处理需要根据输入本身修改其计算图的能力。这些能力自然引导我们到注意力机制，它是以下章节中详细探讨的 Transformer 架构的基础。图 4.5 显示了注意力如何实现这种动态信息流。

![图片](img/file58.svg)

图 4.5：**注意力权重**：Transformer 的注意力机制动态评估子词之间的关系，为序列中更相关的连接分配更高的权重，使模型能够关注关键信息。这些学习到的权重，以连接强度可视化，揭示了模型在处理语言时如何关注输入的不同部分。

### 基本注意力机制

注意力机制代表了从固定架构连接到序列元素之间动态、基于内容的交互的转变。本节探讨了注意力的数学基础，探讨了查询-键-值操作如何实现灵活的模式处理。我们分析了计算需求、内存访问模式和系统影响，这些使得注意力既强大又计算密集。

#### 算法结构

注意力机制通过根据其内容计算元素之间的加权连接，形成了动态模式处理的基础（Bahdanau, Cho, 和 Bengio 2014）。这种方法处理的关系不是由架构固定的，而是从数据本身中产生的。注意力机制的核心是一个可以用数学表达式表示的操作：

<semantics><mrow><mtext mathvariant="normal">注意力</mtext><mrow><mo stretchy="true" form="prefix">(</mo><mi>𝐐</mi><mo>,</mo><mi>𝐊</mi><mo>,</mo><mi>𝐕</mi><mo stretchy="true" form="postfix">)</mo></mrow><mo>=</mo><mtext mathvariant="normal">softmax</mtext><mrow><mo stretchy="true" form="prefix">(</mo><mfrac><mrow><mi>𝐐</mi><msup><mi>𝐊</mi><mi>T</mi></msup></mrow><msqrt><msub><mi>d</mi><mi>k</mi></msub></msqrt></mfrac><mo stretchy="true" form="postfix">)</mo></mrow><mi>𝐕</mi></mrow> <annotation encoding="application/x-tex">\text{Attention}(\mathbf{Q}, \mathbf{K}, \mathbf{V}) = \text{softmax} \left(\frac{\mathbf{Q}\mathbf{K}^T}{\sqrt{d_k}}\right)\mathbf{V}</annotation></semantics>

这个方程展示了缩放点积注意力。 <semantics><mi>𝐐</mi><annotation encoding="application/x-tex">\mathbf{Q}</annotation></semantics>（查询）和 <semantics><mi>𝐊</mi><annotation encoding="application/x-tex">\mathbf{K}</annotation></semantics>（键）通过矩阵乘法计算相似度分数，除以 <semantics><msqrt><msub><mi>d</mi><mi>k</mi></msub></msqrt><annotation encoding="application/x-tex">\sqrt{d_k}</annotation></semantics>（键维度）以实现数值稳定性，然后通过 softmax18 进行归一化以获得注意力权重。这些权重应用于 <semantics><mi>𝐕</mi><annotation encoding="application/x-tex">\mathbf{V}</annotation></semantics>（值）以产生输出。结果是加权组合，其中每个位置根据内容相似性从所有相关位置接收信息。

在这个方程中，<semantics><mi>𝐐</mi><annotation encoding="application/x-tex">\mathbf{Q}</annotation></semantics>（查询）、<semantics><mi>𝐊</mi><annotation encoding="application/x-tex">\mathbf{K}</annotation></semantics>（键）和<semantics><mi>𝐕</mi><annotation encoding="application/x-tex">\mathbf{V}</annotation></semantics>（值）19 代表输入的学习投影。对于一个长度为<semantics><mi>N</mi><annotation encoding="application/x-tex">N</annotation></semantics>且维度为<semantics><mi>d</mi><annotation encoding="application/x-tex">d</annotation></semantics>的序列，这个操作创建了一个<semantics><mrow><mi>N</mi><mo>×</mo><mi>N</mi></mrow><annotation encoding="application/x-tex">N\times N</annotation></semantics>注意力矩阵，确定每个位置应该如何关注所有其他位置。

注意力操作涉及几个关键步骤。首先，它为序列中的每个位置计算查询、键和值投影。接下来，它通过查询-键交互生成一个<semantics><mrow><mi>N</mi><mo>×</mo><mi>N</mi></mrow><annotation encoding="application/x-tex">N\times N</annotation></semantics>注意力矩阵。这些步骤在图 4.6 中得到了说明。最后，它使用这些注意力权重来组合值向量，生成输出。

![图片](img/file59.svg)

图 4.6：**查询-键-值交互**：Transformer 注意力机制通过计算查询、键和值之间的关系，动态地权衡输入序列元素，使模型能够关注相关信息。这些投影有助于创建一个注意力矩阵，该矩阵决定了每个值向量对最终输出的贡献，有效地捕捉序列中的上下文依赖关系。来源：[Transformer 解释器](https://poloclub.GitHub.io/transformer-explainer/).

关键在于，与之前架构中发现的固定权重矩阵不同，如图 4.7 所示，这些注意力权重是针对每个输入动态计算的。这使得模型能够根据动态内容调整其处理方式。

![图片](img/file60.svg)

图 4.7：**动态注意力权重**：Transformer 模型根据查询、键和值向量之间的关系动态计算注意力权重，使模型能够在每个处理步骤中关注输入序列的相关部分。这与固定权重架构形成对比，并允许自适应模式处理以处理变长输入和复杂依赖关系。来源：[Transformer 解释器](https://poloclub.GitHub.io/transformer-explainer/).

#### 计算映射

注意力机制创建的计算模式与之前的架构有显著差异。在 列表 4.7 中展示的实现方法表明，动态连接性转化为特定的计算需求。

列表 4.7：**注意力机制**：Transformer 模型通过查询-键-值交互来计算注意力，从而在输入序列上实现动态关注，以改善语言理解。

```py
def attention_layer_matrix(Q, K, V):
    # Q, K, V: (batch_size × seq_len × d_model)
    scores = matmul(Q, K.transpose(-2, -1)) / sqrt(
        d_k
    )  # Compute attention scores
    weights = softmax(scores)  # Normalize scores
    output = matmul(weights, V)  # Combine values
    return output


# Core computational pattern
def attention_layer_compute(Q, K, V):
    # Initialize outputs
    scores = np.zeros((batch_size, seq_len, seq_len))
    outputs = np.zeros_like(V)

    # Loop 1: Process each sequence in batch
    for b in range(batch_size):
        # Loop 2: Compute attention for each query position
        for i in range(seq_len):
            # Loop 3: Compare with each key position
            for j in range(seq_len):
                # Compute attention score
                for d in range(d_model):
                    scores[b, i, j] += Q[b, i, d] * K[b, j, d]
                scores[b, i, j] /= sqrt(d_k)

        # Apply softmax to scores
        for i in range(seq_len):
            scores[b, i] = softmax(scores[b, i])

        # Loop 4: Combine values using attention weights
        for i in range(seq_len):
            for j in range(seq_len):
                for d in range(d_model):
                    outputs[b, i, d] += scores[b, i, j] * V[b, j, d]

    return outputs
```

将注意力数学上的优雅性转换为硬件执行，揭示了动态连接性的计算成本。虽然注意力方程 `Attention(Q,K,V) = softmax(QK^T/√d_k)V` 看起来是一个简单的矩阵运算，但物理实现需要协调成千上万的成对计算，这比之前的架构产生了不同的系统需求。

`attention_layer_compute` 中的嵌套循环揭示了注意力的真实计算特征（参见 列表 4.7）。第一个循环独立处理批处理中的每个序列。第二个和第三个循环计算所有位置对之间的注意力分数，形成了使注意力既强大又计算密集的二次计算模式。第四个循环使用这些注意力权重来组合所有位置的价值，完成定义注意力机制的动态连接模式。

#### 系统影响

注意力机制通过其动态连接性需求展现出与之前架构不同的系统级模式。

##### 内存需求

在内存需求方面，注意力机制需要存储注意力权重、键-查询-值投影和中间特征表示。对于一个长度小于 `<semantics><mi>N</mi><annotation encoding="application/x-tex">N</annotation></semantics>` 的序列和维度 d，每个注意力层必须为批处理中的每个序列存储一个 `<semantics><mrow><mi>N</mi><mo>×</mo><mi>N</mi></mrow><annotation encoding="application/x-tex">N\times N</annotation></semantics>` 的注意力权重矩阵，三组投影矩阵用于查询、键和值（每个大小为 `<semantics><mrow><mi>d</mi><mo>×</mo><mi>d</mi></mrow><annotation encoding="application/x-tex">d\times d</annotation></semantics>`），以及大小为 `<semantics><mrow><mi>N</mi><mo>×</mo><mi>d</mi></mrow><annotation encoding="application/x-tex">N\times d</annotation></semantics>` 的输入和输出特征图。为每个输入动态生成注意力权重创建了一个内存访问模式，其中中间注意力权重成为内存使用的一个重要因素。

##### 计算需求

注意力机制中的计算需求集中在两个主要阶段：生成注意力权重并将它们应用于值。对于每个注意力层，系统在多个计算阶段执行许多乘加操作。仅查询-键交互就需要<semantics><mrow><mi>N</mi><mo>×</mo><mi>N</mi><mo>×</mo><mi>d</mi></mrow><annotation encoding="application/x-tex">N\times N\times d</annotation></semantics>次乘加操作，应用注意力权重到值也需要相同数量的操作。还需要额外的计算来处理投影矩阵和 softmax 操作。这种计算模式与之前的架构不同，因为它与序列长度呈二次关系，并且需要对每个输入进行新鲜的计算。

##### 数据移动

注意力机制中的数据移动提出了独特的挑战。每个注意力操作都涉及对每个位置的查询、键和值向量进行投影和移动，存储和访问完整的注意力权重矩阵，并在加权组合阶段协调值向量的移动。这创造了一种数据移动模式，其中中间注意力权重成为系统带宽需求的主要因素。与 CNN 的更可预测的访问模式或 RNN 的顺序访问不同，注意力操作需要频繁地在内存层次结构中移动动态计算的权重。

注意力机制在内存、计算和数据移动方面的独特特性对系统设计和优化具有重大影响，为更高级架构如传输器的开发奠定了基础。

### 传输器：仅注意力架构

当注意力机制引入了动态模式处理的概念时，它们最初被作为现有架构的补充应用，尤其是在序列到序列任务中用于循环神经网络（RNNs）。这种混合方法仍然受到循环架构的基本限制：序列处理约束阻止了有效的并行化，以及处理非常长序列的困难。突破性的洞察是认识到，仅注意力机制本身就可以完全取代卷积和循环处理。

Vaswani 等人在里程碑式的论文 "Attention is All You Need"20 中介绍了 Transformers，该论文发表于 2017。该论文体现了一种革命性的归纳偏差：**它们假设没有先验结构，但允许模型根据内容动态地学习所有成对关系**。这种架构假设代表了在 第 4.1 节 中详细阐述的架构演变的最终成果，通过消除所有结构约束，以纯内容依赖处理为代价。Transformers 不是向 RNNs 添加注意力，而是围绕注意力机制构建了整个架构，引入自注意力作为主要的计算模式。这一架构决策以 CNNs 的参数效率和 RNNs 的序列一致性为代价，换取了最大的灵活性和并行性。

这代表了我们架构旅程的最终一步：从连接一切到一切的 MLPs，到连接局部的 CNNs，再到按顺序连接的 RNNs，最后到基于学习到的内容关系动态连接的 Transformers。每一次演变都牺牲了约束以换取能力，Transformers 在 第 4.1 节 中确立的计算成本下实现了最大的表达能力。

#### 算法结构

Transformers 的关键创新在于它们使用了自注意力层。在 Transformers 所使用的自注意力机制中，查询（Query）、键（Key）和值（Value）向量都来自相同的输入序列。这与早期注意力机制的关键区别在于，查询可能来自解码器，而键和值来自编码器。通过使所有组件都自参照，自注意力允许模型在编码每个位置时，根据内容动态地权衡同一序列中不同位置的重要性。例如，在处理句子“动物没有过街，因为它太宽了”时，自注意力允许模型将“它”与“街道”联系起来，捕捉到传统序列模型难以处理的长期依赖关系。

自注意力机制可以用类似于基本注意力机制的形式进行数学表达：<semantics><mrow><mtext mathvariant="normal">SelfAttention</mtext><mrow><mo stretchy="true" form="prefix">(</mo><mi>𝐗</mi><mo stretchy="true" form="postfix">)</mo></mrow><mo>=</mo><mtext mathvariant="normal">softmax</mtext><mrow><mo stretchy="true" form="prefix">(</mo><mfrac><mrow><mrow><mi>𝐗</mi><msub><mi>𝐖</mi><mi>𝐐</mi></msub></mrow><msup><mrow><mo stretchy="true" form="prefix">(</mo><mrow><mi>𝐗</mi><msub><mi>𝐖</mi><mi>𝐊</mi></msub></mrow><mo stretchy="true" form="postfix">)</mo></mrow><mi>T</mi></msup></mrow><msqrt><msub><mi>d</mi><mi>k</mi></msub></msqrt></mfrac><mo stretchy="true" form="postfix">)</mo></mrow><mrow><mi>𝐗</mi><msub><mi>𝐖</mi><mi>𝐕</mi></msub></mrow></mrow> <annotation encoding="application/x-tex">\text{SelfAttention}(\mathbf{X}) = \text{softmax} \left(\frac{\mathbf{XW_Q}(\mathbf{XW_K})^T}{\sqrt{d_k}}\right)\mathbf{XW_V}</annotation></semantics>

在这里，<semantics><mi>𝐗</mi><annotation encoding="application/x-tex">\mathbf{X}</annotation></semantics> 是输入序列，而 <semantics><msub><mi>𝐖</mi><mi>𝐐</mi></msub><annotation encoding="application/x-tex">\mathbf{W_Q}</annotation></semantics>、<semantics><msub><mi>𝐖</mi><mi>𝐊</mi></msub><annotation encoding="application/x-tex">\mathbf{W_K}</annotation></semantics> 和 <semantics><msub><mi>𝐖</mi><mi>𝐕</mi></msub><annotation encoding="application/x-tex">\mathbf{W_V}</annotation></semantics> 分别是用于查询、键和值的学到的权重矩阵。这种公式突出了自注意力如何从相同的输入中推导出所有组件，从而创建一个动态的、内容相关的处理模式。

建立在这样一个基础上，Transformers 使用多头注意力，通过并行运行多个注意力函数来扩展自注意力机制。每个“头”都涉及一组独立的查询/键/值投影，可以关注输入的不同方面，使模型能够同时关注来自不同表示子空间的信息。这种多头结构为模型提供了更丰富的表示能力，使其能够同时捕捉数据中的各种类型的关系。

多头注意力的数学公式为：<semantics><mrow><mtext mathvariant="normal">MultiHead</mtext><mrow><mo stretchy="true" form="prefix">(</mo><mi>𝐐</mi><mo>,</mo><mi>𝐊</mi><mo>,</mo><mi>𝐕</mi><mo stretchy="true" form="postfix">)</mo></mrow><mo>=</mo><mtext mathvariant="normal">Concat</mtext><mrow><mo stretchy="true" form="prefix">(</mo><msub><mtext mathvariant="normal">head</mtext><mn>1</mn></msub><mo>,</mo><mi>…</mi><mo>,</mo><msub><mtext mathvariant="normal">head</mtext><mi>h</mi></msub><mo stretchy="true" form="postfix">)</mo></mrow><msup><mi>𝐖</mi><mi>O</mi></msup></mrow> <annotation encoding="application/x-tex">\text{MultiHead}(\mathbf{Q}, \mathbf{K}, \mathbf{V}) = \text{Concat}(\text{head}_1, \ldots, \text{head}_h)\mathbf{W}^O</annotation></semantics> 其中每个注意力头是按照以下方式计算的：<semantics><mrow><msub><mtext mathvariant="normal">head</mtext><mi>i</mi></msub><mo>=</mo><mtext mathvariant="normal">Attention</mtext><mrow><mo stretchy="true" form="prefix">(</mo><mi>𝐐</mi><msubsup><mi>𝐖</mi><mi>i</mi><mi>Q</mi></msubsup><mo>,</mo><mi>𝐊</mi><msubsup><mi>𝐖</mi><mi>i</mi><mi>K</mi></msubsup><mo>,</mo><mi>𝐕</mi><msubsup><mi>𝐖</mi><mi>i</mi><mi>V</mi></msubsup><mo stretchy="true" form="postfix">)</mo></mrow></mrow> <annotation encoding="application/x-tex">\text{head}_i = \text{Attention}(\mathbf{Q}\mathbf{W}_i^Q, \mathbf{K}\mathbf{W}_i^K, \mathbf{V}\mathbf{W}_i^V)</annotation></semantics>

在自注意力和多头注意力中，一个关键组件是缩放因子 <semantics><msqrt><msub><mi>d</mi><mi>k</mi></msub></msqrt><annotation encoding="application/x-tex">\sqrt{d_k}</annotation></semantics>，它具有重要的数学作用。这个因子防止点积变得过大，这会将 softmax 函数推向梯度极小的区域。对于维度为 <semantics><msub><mi>d</mi><mi>k</mi></msub><annotation encoding="application/x-tex">d_k</annotation></semantics> 的查询和键，它们的点积具有方差 <semantics><msub><mi>d</mi><mi>k</mi></msub><annotation encoding="application/x-tex">d_k</annotation></semantics>，因此除以 <semantics><msqrt><msub><mi>d</mi><mi>k</mi></msub></msqrt><annotation encoding="application/x-tex">\sqrt{d_k}</annotation></semantics> 将方差归一化到 1，保持稳定的梯度并实现有效的学习。21

除了数学机制之外，注意力机制可以从概念上理解为实现了内容可寻址内存系统的一种形式。类似于基于键匹配检索值的哈希表，注意力计算查询与所有可用键之间的相似度，然后检索相应值的加权组合。点积相似度`Q·K`就像一个哈希函数，它衡量每个键与查询匹配的程度。softmax 归一化确保权重之和为 1，实现了概率检索机制。这种联系解释了为什么注意力对于需要灵活信息检索的任务是有效的——它提供了对数据库查找操作的微分近似。 

从信息论的角度来看，注意力机制在不确定性下实现了最优的信息聚合。注意力权重代表了关于哪些输入部分包含当前处理步骤的相关信息的不确定性。softmax 操作实现了最大熵原理：在所有可能的将注意力分配到输入位置的方式中，softmax 选择具有最大熵的分布，同时满足相似度分数决定相对重要性的约束（Cover 和 Thomas 2001）。

#### 效率和优化

注意力机制高度冗余，许多头部学习相似的模式。通过头部剪枝和低秩注意力因子分解，可以在谨慎实施的情况下将计算量减少 50-80%。对大型 Transformer 模型的分析表明，大多数注意力头部都落入几种常见的模式（位置、句法、语义），这表明显式的架构专业化可以替代学习到的冗余。

注意力操作由于 softmax 操作和注意力分数的二次数量，对量化特别敏感。需要为 Q、K、V 投影制定单独的量化方案，并仔细处理 softmax 操作，以确保稳定的量化。训练后 INT8 量化通常会导致 2-3%的精度损失，而 INT4 量化则需要更复杂的量化感知训练方法。

与序列长度的二次缩放创造了效率限制。稀疏注意力模式（如局部窗口、步进模式或学习到的稀疏性）可以将复杂度从 O(n²)降低到 O(n log n)或 O(n)，同时保持大多数建模能力。线性注意力近似以牺牲一些表达能力为代价，实现了线性缩放，使得在有限的硬件上能够处理更长的序列。

这种信息论解释揭示了为什么注意力对于选择性处理如此有效。该机制自动平衡两个相互竞争的目标：关注最相关的信息（最小化熵）同时保持足够的广度以避免遗漏重要细节（最大化熵）。注意力模式作为这些目标之间的最佳权衡出现，解释了为什么变换器可以有效地处理长序列和复杂依赖关系。

自注意力学习输入序列中的动态激活模式。与使用固定滤波器的 CNN 或使用固定递归模式的 RNN 不同，注意力根据其内容学习哪些元素应该一起激活。这创造了一种自适应连接形式，其中每个输入的有效网络拓扑都会发生变化。最近的研究表明，训练模型中的注意力头通常专门用于检测特定的语言或语义模式（Clark 等人 2019），这表明该机制自然地发现了数据中的可解释结构规律。

变换器架构利用这种自注意力机制，在更广泛的结构中，通常包括前馈层、层归一化和残差连接（参见图 4.8）。这种组合允许变换器并行处理输入序列，捕捉复杂依赖关系，而无需进行顺序计算。因此，变换器在从自然语言处理到计算机视觉的广泛任务中展示了显著的有效性，跨越了不同领域的深度学习架构。

![图片](img/file61.svg)

图 4.8：**注意力头**：神经网络通过查询-键-值交互计算注意力，使子词能够动态聚焦，从而提高句子理解能力。来源：Attention Is All You Need.

#### 计算映射

虽然变换器自注意力建立在基本注意力机制之上，但它引入了独特的计算模式，使其与众不同。为了理解这些模式，我们必须检查变换器中自注意力的典型实现（参见列表 4.8）：

列表 4.8：**自注意力机制**：变换器模型通过查询-键-值交互计算注意力，使输入序列能够动态聚焦，从而提高语言理解能力。

```py
def self_attention_layer(X, W_Q, W_K, W_V, d_k):
    # X: input tensor (batch_size × seq_len × d_model)
    # W_Q, W_K, W_V: weight matrices (d_model × d_k)

    Q = matmul(X, W_Q)
    K = matmul(X, W_K)
    V = matmul(X, W_V)

    scores = matmul(Q, K.transpose(-2, -1)) / sqrt(d_k)
    attention_weights = softmax(scores, dim=-1)
    output = matmul(attention_weights, V)

    return output


def multi_head_attention(X, W_Q, W_K, W_V, W_O, num_heads, d_k):
    outputs = []
    for i in range(num_heads):
        head_output = self_attention_layer(
            X, W_Q[i], W_K[i], W_V[i], d_k
        )
        outputs.append(head_output)

    concat_output = torch.cat(outputs, dim=-1)
    final_output = matmul(concat_output, W_O)

    return final_output
```

#### 系统影响

这种实现揭示了适用于基本注意力机制的关键计算特性，其中 Transformer 自注意力代表了一个具体案例。首先，自注意力使得在整个序列的所有位置上实现并行处理。这在同时计算所有位置的`Q`、`K`和`V`的矩阵乘法中表现得非常明显。与按顺序处理输入的循环架构不同，这种并行性质允许更有效的计算，尤其是在为并行操作设计的现代硬件上。

第二，注意力分数计算结果是一个大小为 `(seq_len × seq_len)` 的矩阵，导致序列长度上的二次复杂度。当处理长序列时，这种二次关系成为一个显著的计算瓶颈，这也促使人们研究更有效的注意力机制。

第三，多头注意力机制有效地并行运行多个自注意力操作，每个操作都有其自己的学习投影集。虽然这会随着头数的增加而线性增加计算负载，但它允许模型捕捉到相同输入中的不同类型的关系，从而增强了模型的表示能力。

第四，自注意力中的核心计算主要由大矩阵乘法组成。对于一个长度为 <semantics><mi>N</mi><annotation encoding="application/x-tex">N</annotation></semantics> 的序列和嵌入维度 <semantics><mi>d</mi><annotation encoding="application/x-tex">d</annotation></semantics>，主要操作涉及大小为 <semantics><mrow><mo stretchy="true" form="prefix">(</mo><mi>N</mi><mo>×</mo><mi>d</mi><mo stretchy="true" form="postfix">)</mo></mrow><annotation encoding="application/x-tex">(N\times d)</annotation></semantics>、<semantics><mrow><mo stretchy="true" form="prefix">(</mo><mi>d</mi><mo>×</mo><mi>d</mi><mo stretchy="true" form="postfix">)</mo></mrow><annotation encoding="application/x-tex">(d\times d)</annotation></semantics> 和 <semantics><mrow><mo stretchy="true" form="prefix">(</mo><mi>N</mi><mo>×</mo><mi>N</mi><mo stretchy="true" form="postfix">)</mo></mrow><annotation encoding="application/x-tex">(N\times N)</annotation></semantics> 的矩阵。这些密集的矩阵运算非常适合在专门的硬件（如 GPU）上加速，但它们也显著增加了模型的整体计算成本。

最后，自注意力生成了内存密集的中间结果。注意力权重矩阵<semantics><mrow><mo stretchy="true" form="prefix">(</mo><mi>N</mi><mo>×</mo><mi>N</mi><mo stretchy="true" form="postfix">)</mo></mrow><annotation encoding="application/x-tex">(N\times N)</annotation></semantics>以及每个注意力头的中间结果创建了大量的内存需求，尤其是对于长序列。这可能在内存受限的设备上部署时提出挑战，并需要在实现中仔细管理内存。

这些计算模式为 Transformer 的自注意力机制创造了一个独特的轮廓，与之前的架构不同。计算的并行性使得 Transformer 非常适合现代并行处理硬件，但序列长度的二次复杂性对处理长序列提出了挑战。因此，许多研究都集中在开发优化技术，如稀疏注意力模式或低秩近似，以解决这些挑战。每种优化都在计算效率和模型表达能力之间提供了自己的权衡，这在实际应用中必须仔细考虑。

对四种不同架构家族的考察揭示了它们的个体特征和集体演变。与其将这些架构孤立看待，不如当我们考虑它们之间的关系以及它们如何建立在共同基础上时，会获得更深入的理解。

## 建筑模块

在考察了四个主要架构家族——MLP、CNN、RNN 和 Transformer——每个都具有独特的计算特性和系统影响后，一个统一的视角出现了。虽然在前面的章节中，深度学习架构被作为不同的方法提出，但它们更好地被理解为随着时间的推移而演变的模块组合。就像由基本砖块构建的复杂乐高结构一样，现代神经网络结合并迭代了数十年来研究产生的核心计算模式(杨·勒克文、本吉奥和辛顿 2015)。每一次架构创新都引入了新的模块，同时发现了现有模块的新应用。

这些模块及其演变过程照亮了现代建筑设计。简单的感知器(罗森布拉特 1958)演变为多层网络(鲁梅尔哈特、辛顿和威廉姆斯 1986)，随后又产生了专门用于空间和序列处理的模式。每一次进步都保留了前辈的有用元素，同时引入了新的计算原语。当代架构，如 Transformer，代表了这些模块精心设计的组合。

这一进展揭示了神经网络的发展以及核心计算模式的发现和改进，这些模式仍然相关。在第 4.1 节中概述的架构进展的基础上，每个新的架构都引入了独特的计算需求和系统级挑战。

表 4.1 总结了这一演变，突出了每个深度学习发展阶段的关键原语和系统关注点。这张表捕捉了深度学习架构设计的主要转变以及系统级考虑的相应变化。这一进展涵盖了从针对 CPU 优化的早期密集矩阵操作，通过利用 GPU 加速的卷积和需要复杂内存层次结构的顺序操作，到当前需要灵活加速器和高速内存的注意力机制时代。

表 4.1：**深度学习演变**：神经网络架构已从简单的全连接层发展到利用专用硬件和解决顺序数据依赖的复杂模型。这张表将架构时代映射到关键计算原语和相应的系统级优化，揭示了一个历史趋势，即向更高的并行性和内存带宽需求发展。

| **时代** | **主导架构** | **关键原语** | **系统关注点** |
| --- | --- | --- | --- |
| **早期神经网络** | MLP | 稠密矩阵操作 | CPU 优化 |
| **CNN 革命** | CNN | 卷积 | GPU 加速 |
| **序列建模** | RNN | 序列操作 | 内存层次结构 |
| **注意力时代** | Transformer | 注意力，动态计算 | 灵活的加速器，高速内存 |

对这些构建块的考察显示，原语在演变和组合中创造越来越强大的神经网络架构。

### 从感知器到多层网络的演变

虽然我们在第 4.2 节中考察了 MLP 作为密集模式处理机制的机制，但在这里我们关注的是它们如何建立了贯穿深度学习的构建块。从感知器到 MLP 的演变引入了几个关键概念：层堆叠的力量、非线性转换的重要性以及基本的正向计算模式。

在输入和输出之间引入隐藏层，为几乎每个现代架构中出现的特征转换创建了一个模板。即使在像 Transformer 这样的复杂网络中，我们也能找到执行特征处理的 MLP 风格的前馈层。通过连续的非线性层转换数据的概念已经成为超越特定架构类型的范式。

最重要的是，MLPs 的发展确立了反向传播算法 22，这一算法至今仍然是神经网络优化的基石。这一关键贡献使得深度架构的发展成为可能，并影响了后续架构的设计，以保持梯度流。

这些构建块，包括分层特征转换、非线性激活和基于梯度的学习，为更专业的架构奠定了基础。后续的创新通常集中在以新的方式构建这些基本组件，而不是完全取代它们。

### 从密集处理到空间处理的演变

CNNs 的发展标志着架构创新，特别是我们能够专门化 MLPs 的密集连接以处理空间模式的认识。在保留层状处理的核心概念的同时，CNNs 引入了几个将影响所有未来架构的构建块。

第一个关键创新是参数共享的概念。与每个连接都有自己的权重不同，CNNs 展示了相同的参数可以在输入的不同部分重复使用。这不仅使网络更高效，还引入了强大的思想，即架构结构可以编码关于数据的有用先验信息 (Lecun et al. 1998)。

可能更具影响力的是通过 ResNets23 (K. He et al. 2015)引入的跳跃连接。最初它们被设计用来帮助训练非常深的 CNNs，跳跃连接已经成为几乎每个现代架构中出现的构建块。它们展示了如何通过网络中的直接路径帮助梯度流和信息传播，这一概念现在已成为 Transformer 设计中的核心。

CNNs 还引入了批量归一化，这是一种通过归一化中间特征来稳定神经网络优化的技术 (Ioffe and Szegedy 2015a)。这种特征归一化的概念，虽然起源于 CNNs，但演变成了层归一化，现在已成为现代架构的关键组成部分。

这些创新，如参数共享、跳跃连接和归一化，超越了它们在空间处理中的起源，成为深度学习工具箱中的基本构建块。

### 序列处理的发展

当 CNNs 专门用于空间模式的多层感知器（MLPs）时，序列模型适应了神经网络以处理时间依赖性。循环神经网络（RNNs）引入了维持和更新状态的概念，这是影响网络如何处理序列信息的一个构建块，(Elman 1990)。

LSTM24 和 GRU25 的发展将复杂的门控机制引入了神经网络（Hochreiter 和 Schmidhuber 1997；Cho 等人 2014）。这些门控机制，本身是小型 MLP，展示了如何通过简单的前馈计算来组合以控制信息流。这种使用神经网络调节其他神经网络的概念成为架构设计中的常见模式。

也许最显著的是，序列模型展示了自适应计算路径的力量。与 MLPs 和 CNNs 的固定模式不同，RNNs 展示了网络如何通过在时间上重复使用权重来处理可变长度的输入。这一洞察，即架构模式可以适应输入结构，为更灵活的架构奠定了基础。

序列模型通过编码器-解码器架构（Bahdanau, Cho, 和 Bengio 2014）也普及了注意力概念。最初作为机器翻译的改进而引入，注意力机制展示了网络如何学习动态关注相关信息。这个构建块后来成为 Transformer 架构的基础。

### 现代架构：综合与统一

现代架构，尤其是 Transformer，代表了这些基本构建块的高级综合。它们并非引入全新的模式，而是通过战略性地组合和改进现有组件进行创新。Transformer 架构就是这一方法的例证：在其核心，MLP 风格的前馈网络在注意力层之间处理特征。注意力机制本身建立在序列模型概念之上，同时消除了循环连接，转而采用受 CNN 直觉启发的位置嵌入。该架构广泛利用跳跃连接（见图 4.9），这些连接是从 ResNets 继承而来的，而层归一化（从 CNN 批归一化演变而来）则稳定了优化过程（Ba, Kiros, 和 Hinton 2016）。

![图片](img/file62.svg)

图 4.9：**残差连接**：跳跃连接将层的输入添加到其输出中，使得梯度可以直接通过网络流动，缓解了深层架构中的梯度消失问题。这允许训练深度更大的网络，正如在 resnets 中看到的那样，并被现代 Transformer 架构采用以改善优化和性能。

这种构建块的组合创造了超越单个组件总和的涌现能力。自注意力机制在先前注意力概念的基础上，实现了新型动态模式处理。这些组件的排列——注意力机制后跟前馈层，带有跳过连接和归一化——已被证明足够有效，成为新架构的模板。

视觉和语言模型中的最近创新遵循这种重新组合构建块的模式。视觉 Transformer26 将 Transformer 架构应用于图像，同时保持其基本组件(Dosovitskiy 等人 2021)。大型语言模型扩展了这些模式，同时引入了分组查询注意力或滑动窗口注意力等改进，但仍然依赖于通过这种架构演变建立的核心理念(T. Brown 等人 2020)。这些现代架构创新展示了第九章中涵盖的高效扩展原则，而其实际实施挑战和优化将在第十章中探讨。

下面对不同神经网络架构中原始利用率的比较显示了现代架构在先前方法的基础上综合和创新：

表 4.2：**原始利用率**：神经网络架构在核心计算和内存访问模式上有所不同，这影响了硬件需求和效率。Transformer 独特地将矩阵乘法与注意力机制相结合，导致随机的内存访问和数据移动模式，与顺序的 RNN 或步进 CNN 不同。

| **原始类型** | **MLP** | **CNN** | **RNN** | **Transformer** |
| --- | --- | --- | --- | --- |
| **计算** | 矩阵乘法 | 卷积（矩阵乘法） | 矩阵乘法 + 状态更新 | 矩阵乘法 + 注意力 |
| **内存访问** | 顺序 | 步进 | 顺序 + 随机 | 随机（注意力） |
| **数据移动** | 广播 | 滑动窗口 | 顺序 | 广播 + 收集 |

如表 4.2 所示，Transformer 结合了先前架构的元素，同时引入了新的模式。它们保留了所有架构共有的核心矩阵乘法操作，但通过其注意力机制引入了更复杂的内存访问模式。它们的数据移动模式融合了 MLP 的广播操作和类似更动态架构的收集操作。

这种在 Transformers 中对原语的合成表明，现代架构通过重新组合和改进在第 4.1 节中确立的现有构建块，而不是发明全新的计算范式，来进行创新。这一进化过程指导了未来架构的发展，并有助于设计支持它们的效率系统。

## 系统级构建块

对不同深度学习架构的考察使得可以将它们的系统需求提炼为支撑硬件和软件实现的原语。这些原语代表了一些操作，在保持其基本特征的同时无法进一步分解。正如复杂分子是由基本原子构建而成，复杂的神经网络也是由这些操作构建而成。

### 核心计算原语

三种操作是所有深度学习计算的基础构建块：矩阵乘法、滑动窗口操作和动态计算。这些操作是原语，因为如果不失去其基本的计算属性和效率特性，就无法进一步分解。

矩阵乘法代表了特征集转换的基本形式。当我们用一个输入矩阵乘以一个权重矩阵时，我们正在计算加权组合，这是神经网络的核心操作。例如，在我们的 MNIST 网络中，每个 784 维输入向量与一个<semantics><mrow><mn>784</mn><mo>×</mo><mn>100</mn></mrow><annotation encoding="application/x-tex">784\times 100</annotation></semantics>权重矩阵相乘。这种模式无处不在：MLPs 直接用它进行层计算，CNNs 将卷积重塑为矩阵乘法（将一个<semantics><mrow><mn>3</mn><mo>×</mo><mn>3</mn></mrow><annotation encoding="application/x-tex">3\times 3</annotation></semantics>卷积转换为矩阵操作，如图图 4.10 所示），并且 Transformers 在其注意力机制中广泛使用它。

#### 计算构建块

现代神经网络通过三种计算模式在所有架构中运行。这些模式解释了不同架构如何实现其计算目标，以及为什么某些硬件优化是有效的。

对稀疏计算模式的详细分析，包括结构化和非结构化稀疏性、硬件感知优化策略和算法-硬件协同设计原则，在第十章和第十一章中讨论。

![图片](img/file63.svg)

图 4.10：**卷积作为矩阵乘法**：使用`im2col`技术将卷积层重塑为矩阵乘法，可以有效地使用优化的 BLAS 库进行计算，并允许在标准硬件上进行并行处理。这种转换对于加速卷积神经网络至关重要，并构成了在多种平台上实现卷积的基础。

im2col27（图像到列）技术，由英特尔在 20 世纪 90 年代开发，通过展开重叠的图像块到矩阵的列中来实现矩阵重塑，如图图 4.10 所示。卷积中的每个滑动窗口位置在转换矩阵中成为一列，而滤波器核则按行排列。这使得卷积操作可以表示为一个标准的 GEMM（通用矩阵乘法）操作。这种转换以内存消耗——在窗口重叠处复制数据——为代价，换取计算效率，使得卷积神经网络可以利用数十年的 BLAS 优化，并在 CPU 上实现 5-10 倍的加速。在现代系统中，这些矩阵乘法映射到特定的硬件和软件实现。硬件加速器提供专门的张量核心，可以并行执行数千次乘加操作；NVIDIA 的 A100 张量核心可以针对混合精度（TF32）工作负载达到高达 312 TFLOPS，或通过这些操作的巨大并行化实现 156 TFLOPS 的 FP32。像 PyTorch 和 TensorFlow 这样的软件框架自动将这些高级操作映射到优化的矩阵库（NVIDIA [cuBLAS](https://developer.nvidia.com/cublas)，Intel [MKL](https://www.intel.com/content/www/us/en/developer/tools/oneapi/onemkl.html#gs.kxb9ve)），这些库利用了这些硬件能力。

滑动窗口操作通过将相同的操作应用于数据块来计算局部关系。在卷积神经网络处理 MNIST 图像时，一个<semantics><mrow><mn>3</mn><mo>×</mo><mn>3</mn></mrow><annotation encoding="application/x-tex">3\times 3</annotation></semantics>卷积滤波器在<semantics><mrow><mn>28</mn><mo>×</mo><mn>28</mn></mrow><annotation encoding="application/x-tex">28\times 28</annotation></semantics>输入上滑动，需要<semantics><mrow><mn>26</mn><mo>×</mo><mn>26</mn></mrow><annotation encoding="application/x-tex">26\times 26</annotation></semantics>个计算窗口，假设步长大小为 1。现代硬件加速器通过专门的内存访问模式和数据缓冲方案来实现这一点，这些方案优化了数据重用。例如，谷歌的 TPU 使用一个<semantics><mrow><mn>128</mn><mo>×</mo><mn>128</mn></mrow><annotation encoding="application/x-tex">128\times 128</annotation></semantics>的收缩阵列 28，其中数据有系统地通过处理单元流动，允许每个输入值在多个计算中重用而不访问内存。

动态计算，其中操作本身取决于输入数据，随着关注机制的出现而变得突出，但代表了自适应处理所需的能力。在 Transformer 关注中，每个查询动态确定其与所有键的交互权重；对于长度为 512 的序列，必须在运行时动态计算 512 种不同的权重模式。与预先知道计算图的固定模式不同，动态计算需要运行时决策。这创造了特定的实现挑战：硬件必须提供灵活的数据路由（现代 GPU 采用动态调度）并支持可变的计算模式，而软件框架需要高效的机制来处理数据相关的执行路径（PyTorch 的动态计算图，TensorFlow 的动态控制流）。

这些原语在现代架构中以复杂的方式组合。一个处理 512 个标记序列的 Transformer 层清楚地展示了这一点：它使用矩阵乘法进行特征投影（<semantics><mrow><mn>512</mn><mo>×</mo><mn>512</mn></mrow><annotation encoding="application/x-tex">512\times 512</annotation></semantics>操作，通过张量核心实现），可能使用滑动窗口对长序列进行高效的关注（使用针对局部区域的专用内存访问模式），并且需要动态计算关注权重（在运行时计算<semantics><mrow><mn>512</mn><mo>×</mo><mn>512</mn></mrow><annotation encoding="application/x-tex">512\times 512</annotation></semantics>关注模式）。这些原语之间的交互方式对系统设计提出了具体要求，从内存层次结构组织到计算调度。

我们所讨论的构建块有助于解释为什么某些硬件特性存在（如用于矩阵乘法的张量核心）以及为什么软件框架以特定方式组织计算（如将类似的操作一起批处理）。当我们从计算原语转向考虑内存访问和数据移动模式时，认识到这些操作如何塑造对内存系统和数据传输机制的特定要求变得至关重要。计算原语的实施和组合方式对数据在系统内部存储、访问和移动的需求有直接影响。

### 内存访问原语

深度学习模型的效率在很大程度上取决于内存访问和管理。内存访问通常是现代机器学习系统中的主要瓶颈；即使矩阵乘法单元可能能够在每个周期执行数千次操作，如果没有在所需时间提供数据，它将保持空闲。例如，从 DRAM 访问数据通常需要数百个周期，而片上计算只需要几个周期。

在深度学习架构中，三种内存访问模式占主导地位：顺序访问、步长访问和随机访问。每种模式都对内存系统提出了不同的要求，并提供了不同的优化机会。

顺序访问是最简单且最有效的模式。考虑一个执行 MNIST 图像批处理矩阵乘法的 MLP：它需要顺序访问 784×100 的权重矩阵和输入向量。这种模式很好地映射到现代内存系统；DRAM 可以在突发模式下进行顺序读取（在现代 GPU 中达到高达 400 GB/s），硬件预取器可以有效地预测和获取即将到来的数据。软件框架通过确保数据在内存中连续排列并对齐到缓存行边界来优化这一点。

步长访问在 CNN 中尤为突出，其中每个输出位置需要以固定间隔访问输入值的一个窗口。对于一个处理 MNIST 图像的 CNN，使用 3×3 的过滤器，每个输出位置需要以与输入宽度匹配的步长访问 9 个输入值。虽然不如顺序访问高效，但硬件通过模式感知的缓存策略和专门的内存控制器来支持这一点。软件框架通常通过数据布局重组将这些步长模式转换为顺序访问，其中深度学习框架中的 im2col 转换将卷积的步长访问转换为高效的矩阵乘法。

随机访问对系统效率提出了最大的挑战。在一个处理 512 个标记序列的 Transformer 中，每个注意力操作可能需要访问序列中的任何位置，从而创建不可预测的内存访问模式。随机访问可能会通过缓存未命中（可能每次访问造成 100+个周期延迟）和不可预测的内存延迟严重影响性能。系统通过大型缓存层次结构（现代 GPU 具有几个 MB 的 L2 缓存）和复杂的预取策略来解决这个问题，而软件框架则采用诸如注意力模式剪枝等技术来减少随机访问需求。

这些不同的内存访问模式对每个架构的整体内存需求做出了贡献。为了说明这一点，表 4.3 比较了 MLPs、CNNs、RNNs 和 Transformers 的内存复杂度。

表 4.3：**内存访问复杂度**：不同的神经网络架构表现出不同的内存访问模式和存储需求，这会影响系统性能和可扩展性。参数存储随着输入依赖和模型大小而扩展，而激活存储代表了显著的运行时成本，尤其是在序列模型中，当序列长度超过隐藏状态大小时（<semantics><mrow><mi>n</mi><mo>></mo><mi>h</mi></mrow><annotation encoding="application/x-tex">n > h</annotation></semantics>），循环神经网络（RNN）在参数效率上具有优势。

| **架构** | **输入依赖** | **参数存储** | **激活存储** | **缩放行为** |
| --- | --- | --- | --- | --- |
| **MLP** | 线性 | <semantics><mrow><mi>O</mi><mrow><mo stretchy="true" form="prefix">(</mo><mi>N</mi><mo>×</mo><mi>W</mi><mo stretchy="true" form="postfix">)</mo></mrow></mrow><annotation encoding="application/x-tex">O(N \times W)</annotation></semantics> | <semantics><mrow><mi>O</mi><mrow><mo stretchy="true" form="prefix">(</mo><mi>B</mi><mo>×</mo><mi>W</mi><mo stretchy="true" form="postfix">)</mo></mrow></mrow><annotation encoding="application/x-tex">O(B \times W)</annotation></semantics> | 可预测的 |
| **CNN** | 常数 | <semantics><mrow><mi>O</mi><mrow><mo stretchy="true" form="prefix">(</mo><mi>K</mi><mo>×</mo><mi>C</mi><mo stretchy="true" form="postfix">)</mo></mrow></mrow><annotation encoding="application/x-tex">O(K \times C)</annotation></semantics> | <semantics><mrow><mi>O</mi><mo stretchy="false" form="prefix">(</mo><mi>B</mi><mo>×</mo><msub><mi>H</mi><mtext mathvariant="normal">img</mtext></msub></mrow><annotation encoding="application/x-tex">O(B\times H_{\text{img}}</annotation></semantics> <semantics><mrow><mi>×</mi><msub><mi>W</mi><mtext mathvariant="normal">img</mtext></msub><mo stretchy="false" form="postfix">)</mo></mrow><annotation encoding="application/x-tex">\times W_{\text{img}})</annotation></semantics> | 高效的 |
| **RNN** | 线性 | <semantics><mrow><mi>O</mi><mrow><mo stretchy="true" form="prefix">(</mo><msup><mi>h</mi><mn>2</mn></msup><mo stretchy="true" form="postfix">)</mo></mrow></mrow><annotation encoding="application/x-tex">O(h²)</annotation></semantics> | <semantics><mrow><mi>O</mi><mrow><mo stretchy="true" form="prefix">(</mo><mi>B</mi><mo>×</mo><mi>T</mi><mo>×</mo><mi>h</mi><mo stretchy="true" form="postfix">)</mo></mrow></mrow><annotation encoding="application/x-tex">O(B \times T \times h)</annotation></semantics> | 具挑战性 |
| **Transformer** | 二次 | <semantics><mrow><mi>O</mi><mrow><mo stretchy="true" form="prefix">(</mo><mi>N</mi><mo>×</mo><mi>d</mi><mo stretchy="true" form="postfix">)</mo></mrow></mrow><annotation encoding="application/x-tex">O(N \times d)</annotation></semantics> | <semantics><mrow><mi>O</mi><mrow><mo stretchy="true" form="prefix">(</mo><mi>B</mi><mo>×</mo><msup><mi>N</mi><mn>2</mn></msup><mo stretchy="true" form="postfix">)</mo></mrow></mrow><annotation encoding="application/x-tex">O(B \times N²)</annotation></semantics> | 存在问题 |

其中：

+   <semantics><mi>N</mi><annotation encoding="application/x-tex">N</annotation></semantics>: 输入或序列大小

+   <semantics><mi>W</mi><annotation encoding="application/x-tex">W</annotation></semantics>: 层宽度

+   <semantics><mi>B</mi><annotation encoding="application/x-tex">B</annotation></semantics>: 批处理大小

+   <semantics><mi>K</mi><annotation encoding="application/x-tex">K</annotation></semantics>: 核大小

+   <semantics><mi>C</mi><annotation encoding="application/x-tex">C</annotation></semantics>: 通道数

+   <semantics><msub><mi>H</mi><mtext mathvariant="normal">img</mtext></msub><annotation encoding="application/x-tex">H_{\text{img}}</annotation></semantics>: 输入特征图的高度（CNN）

+   <semantics><msub><mi>W</mi><mtext mathvariant="normal">img</mtext></msub><annotation encoding="application/x-tex">W_{\text{img}}</annotation></semantics>: 输入特征图的宽度（CNN）

+   <semantics><mi>h</mi><annotation encoding="application/x-tex">h</annotation></semantics>: 隐藏状态大小（RNN）

+   <semantics><mi>T</mi><annotation encoding="application/x-tex">T</annotation></semantics>: 序列长度

+   <semantics><mi>d</mi><annotation encoding="application/x-tex">d</annotation></semantics>: 模型维度

表 4.3 展示了内存需求如何随着不同的架构选择而变化。例如，在 Transformer 中的激活存储的二次扩展突出了为基于 Transformer 的工作负载设计的系统中需要大内存容量和高效内存管理的必要性。相比之下，由于参数共享和局部处理，CNN 显示出更有利的内存扩展。这些内存访问模式补充了随后在 表 4.6 中考察的计算扩展行为，完成了每个架构资源需求的图景。这些内存复杂度考虑因素为系统级设计决策提供了信息，例如选择内存层次结构配置和开发内存优化策略。

当我们考虑数据重用机会时，这些模式的影响变得明显。在 CNN 中，每个输入像素参与多个卷积窗口（通常为<semantics><mrow><mn>3</mn><mo>×</mo><mn>3</mn></mrow><annotation encoding="application/x-tex">3\times 3</annotation></semantics>滤波器），这使得有效的数据重用对于性能至关重要。现代 GPU 提供多级缓存层次结构（L1、L2、共享内存）来捕获这种重用，而软件技术如循环分块确保数据一旦加载就保持在缓存中。

工作集大小，即同时用于计算所需的数据量，在架构之间差异很大。处理 MNIST 图像的 MLP 层可能只需要几百 KB（权重加激活），而处理长序列的 Transformer 可能只需要几个 MB 来存储注意力模式。这些差异直接影响硬件设计选择，如计算单元和片上内存之间的平衡，以及软件优化，如激活检查点或注意力近似技术。

随着架构的发展，理解这些内存访问模式至关重要。例如，从 CNN 到 Transformer 的转变推动了具有更大片上内存和更复杂缓存策略的硬件的发展，以处理更大的工作集和更动态的访问模式。未来的架构可能会继续由它们的内存访问特性以及它们的计算需求来塑造。

### 数据移动原语

当计算和内存访问模式定义了操作发生在何处时，数据移动原语描述了信息如何通过系统流动。这些模式至关重要，因为数据移动通常比计算本身消耗更多的时间和能量，将数据从片外内存移动到芯片通常需要比执行浮点运算多 100-1000 倍（使用 LaTeX 语法表示）的能量。

在深度学习架构中，四种数据移动模式很常见：广播、散射、收集和归约。图 4.11 展示了这些模式及其关系。广播操作同时将相同的数据发送到多个目的地。在批量大小为 32 的矩阵乘法中，每个权重都必须进行广播以并行处理不同的输入。现代硬件通过专用互连支持这一点，NVIDIA GPU 提供硬件多播功能，实现高达 600 GB/s 的广播带宽，而 TPU 使用专用的广播总线。软件框架通过重构计算（如矩阵分块）来优化广播，以最大化数据重用。

![图片](img/file64.svg)

图 4.11：**集体通信模式**：深度学习训练和推理通常需要在处理单元之间进行数据交换；此图概述了四个核心模式（广播、散列、聚合和减少），这些模式定义了数据如何在分布式系统中移动以及如何影响整体性能。理解这些模式能够优化数据移动，这在现代机器学习工作负载中至关重要，因为通信成本通常占主导地位。

散列操作将不同的元素分配到不同的目的地。当将<semantics><mrow><mn>512</mn><mo>×</mo><mn>512</mn></mrow><annotation encoding="application/x-tex">512\times 512</annotation></semantics>矩阵乘法并行化到 GPU 核心时，每个核心都会接收到计算的一部分。这种并行化对于性能至关重要，但具有挑战性，因为内存冲突和负载不平衡可能会降低效率 50%或更多。硬件提供了灵活的互连（如 NVIDIA 的 NVLink 提供 600 GB/s 的双向带宽），而软件框架采用复杂的工作分配算法以保持高利用率。

聚合操作从多个来源收集数据。在序列长度为 512 的 Transformer 注意力中，每个查询必须从 512 个不同的键值对中收集信息。这些不规则的访问模式具有挑战性，随机收集可能比顺序访问慢<semantics><mrow><mn>10</mn><mo>×</mo></mrow><annotation encoding="application/x-tex">10\times</annotation></semantics>。硬件通过高带宽互连和大型缓存来支持这一点，而软件框架采用诸如注意力模式剪枝等技术来减少收集开销。

减少操作通过求和等操作将多个值合并成一个单一的结果。在计算 Transformer 中的注意力分数或 MLP 中的层输出时，高效的减少操作至关重要。硬件实现了树状结构的减少网络（将延迟从<semantics><mrow><mi>O</mi><mrow><mo stretchy="true" form="prefix">(</mo><mi>n</mi><mo stretchy="true" form="postfix">)</mo></mrow></mrow><annotation encoding="application/x-tex">O(n)</annotation></semantics>减少到<semantics><mrow><mi>O</mi><mrow><mo stretchy="true" form="prefix">(</mo><mo>log</mo><mi>n</mi><mo stretchy="true" form="postfix">)</mo></mrow></mrow><annotation encoding="application/x-tex">O(\log n)</annotation></semantics>），而软件框架使用优化的并行减少算法，可以实现接近理论峰值性能。

这些模式以复杂的方式结合在一起。序列长度为 512 且批大小为 32 的 Transformer 注意力操作涉及：

+   广播查询向量（<semantics><mrow><mn>512</mn><mo>×</mo><mn>64</mn></mrow><annotation encoding="application/x-tex">512\times 64</annotation></semantics>个元素）

+   收集相关键和值（<semantics><mrow><mn>512</mn><mo>×</mo><mn>512</mn><mo>×</mo><mn>64</mn></mrow><annotation encoding="application/x-tex">512\times 512\times 64</annotation></semantics>元素）

+   降低注意力分数（<semantics><mrow><mn>512</mn><mo>×</mo><mn>512</mn></mrow><annotation encoding="application/x-tex">512\times 512</annotation></semantics>元素每个序列）

从 CNN 到 Transformer 的演变增加了对收集和归约操作的需求，推动了更灵活互连和更大片上内存等硬件创新的产生。随着模型的增长（一些现在超过 1000 亿参数 29），高效数据移动变得越来越关键，导致了近内存处理和复杂数据流优化等创新。

### 系统设计影响

我们所探讨的计算、内存访问和数据移动原语构成了深度学习系统设计的根本要求。这些原语如何影响硬件设计、创造共同瓶颈并推动权衡，对于开发高效有效的机器学习系统至关重要。

这些原语对系统设计最显著的影响之一是推动专用硬件的发展。深度学习中矩阵乘法和卷积的普遍存在导致了张量处理单元（TPUs）30 和 GPU 中的张量核心的发展，这些单元专门设计用于高效执行这些操作。

存储系统也受到了深度学习原语需求的深远影响。为了高效地支持顺序和随机访问模式，推动了复杂内存层次结构的发展。高带宽内存（HBM）31 已在人工智能加速器中变得常见，以支持大量数据移动需求，特别是对于 Transformer 中的注意力机制等操作。片上内存层次结构变得更加复杂，具有多级缓存和快速存储器 32，以支持不同神经网络层的不同工作集大小。

数据移动原语特别影响了互连和片上网络的设计。支持高效广播、收集和归约的需求导致了更灵活和更高带宽的互连的发展。一些人工智能芯片现在具有专门设计的片上网络，旨在加速神经网络中的常见数据移动模式。

表 4.4 总结了这些原语对系统的含义：

表 4.4：**原语-硬件协同设计**：高效的机器学习系统需要算法原语与底层硬件之间的紧密集成；此表将常见的原语映射到特定的硬件加速和软件优化，突出了其实现中的关键挑战。专用硬件，如张量核心和数据路径，解决了矩阵乘法和滑动窗口等原语的计算需求，而批处理和动态图执行等软件技术进一步提升了性能。

| **原语** | **硬件影响** | **软件优化** | **关键挑战** |
| --- | --- | --- | --- |
| **矩阵乘法** | 张量核心 | 批处理，GEMM 库 | 并行化，精度 |
| **滑动窗口** | 专用数据路径 | 数据布局优化 | 步长处理 |
| **动态计算** | 灵活路由 | 动态图执行 | 负载均衡 |
| **顺序访问** | 爆发模式 DRAM | 连续分配 | 访问延迟 |
| **随机访问** | 大容量缓存 | 内存感知调度 | 缓存未命中 |
| **广播** | 专用互连 | 操作融合 | 带宽 |
| **收集/分散** | 高带宽内存 | 工作分配 | 负载均衡 |

尽管取得了这些进步，但深度学习模型中仍存在几个瓶颈。内存带宽通常仍然是关键限制，特别是对于具有大工作集或需要频繁随机访问的模型。数据移动的能量成本，尤其是在片外内存和处理单元之间，仍然是一个重大问题。对于大规模模型，分布式训练中的通信开销可能成为瓶颈，限制扩展效率。

#### 架构间的能耗分析

神经网络架构的能耗模式差异很大，对数据中心部署和边缘计算场景都有影响。每种架构模式都表现出独特的能耗特征，这些特征可以指导部署决策和优化策略。

在 MLP 中的密集矩阵操作实现了出色的算术强度 33（每数据移动的计算量），但消耗了显著的绝对能量。每次乘加操作消耗大约 4.6pJ，而数据从 DRAM 移动到 32 位值需要 640pJ。对于典型的 MLP 推理，70-80% 的能量用于数据移动而不是计算，这使得内存带宽优化对于能效至关重要。

卷积操作通过数据重用减少能耗，但效率因实现方式而异。基于 Im2col 的卷积实现以简单性换取内存，通常加倍内存需求和能耗。直接卷积实现通过消除冗余数据移动，尤其是在较大的内核尺寸下，实现了 3-5 倍更好的能效。

RNNs 中的顺序处理通过时间数据的重复使用创造了能源效率的机会。RNN 隐藏状态的恒定内存足迹使得可以采用积极的缓存策略，对于长序列，可以减少 80-90%的 DRAM 访问能源。顺序依赖性限制了并行化机会，通常导致硬件利用率低下和每操作更高的能耗。

由于二次扩展和复杂的数据移动模式，Transformers 中的注意力机制在每操作中表现出最高的能耗。由于不规则的内存访问模式和需要存储注意力矩阵，自注意力操作每 FLOP 比标准矩阵乘法消耗 2-3 倍的能源。这种能源成本与序列长度成二次方关系，使得没有架构修改的长序列处理成为能源禁止的。

系统设计者必须在不同原语的支持之间进行权衡，每个原语都有其独特的特性，这些特性会影响系统设计和性能。例如，为 MLP 和 CNN 中常见的密集矩阵运算进行优化可能会以牺牲注意力机制中更动态计算所需的灵活性为代价。支持 Transformers 的大工作集可能需要牺牲能源效率。

平衡这些权衡需要考虑目标工作负载和部署场景。理解每个原语的本质指导了机器学习系统中硬件和软件优化的开发，使得设计者能够就系统架构和资源分配做出明智的决定。

对架构模式、计算原语和系统影响的分析为解决一个实际挑战奠定了基础：工程师如何系统地选择适合他们特定问题的正确架构？神经网络架构的多样性，每个架构都针对不同的数据模式和计算约束进行了优化，需要一种结构化的架构选择方法。这个选择过程不仅要考虑算法性能，还要考虑第二章中涵盖的部署约束和第十三章中详细说明的操作效率要求。

## 架构选择框架

从密集 MLP 到动态 Transformers 的神经网络架构的探索展示了每个设计如何体现对数据结构和计算模式的具体假设。MLP 假设任意特征关系，CNN 利用空间局部性，RNN 捕获时间依赖性，而 Transformers 模拟复杂的关联模式。对于面临现实世界问题的从业者来说，一个问题出现了：如何系统地选择适用于特定用例的适当架构？

可用架构的多样性让从业者感到不知所措，因为每种架构都声称适用于不同的场景。成功的架构选择需要理解原则而不是跟随趋势：将数据特性与架构优势相匹配，评估计算约束与系统能力，以及平衡精度要求与部署现实。

这种架构选择的系统性方法借鉴了前述分析中探讨的计算模式和系统影响。通过理解不同架构如何处理信息和相应的资源需求，工程师可以做出符合问题需求和实际约束的明智决策。该框架将高效 AI 设计第九章的原则与 ML 操作第十三章中讨论的实际部署考虑因素相结合。

### 数据到架构映射

系统性架构选择的第一个步骤涉及理解不同数据类型如何与架构优势相匹配。每个神经网络架构都是为了解决数据中的特定模式而演化的：MLPs 处理表格数据中的任意关系，CNNs 利用图像中的空间局部性，RNNs 捕捉序列中的时间依赖性，而 Transformers 则模拟任何元素可能影响任何其他元素的复杂关系模式。

这种匹配并非偶然。它反映了计算权衡。与数据特性匹配的架构可以利用自然结构提高效率，而与设计假设不匹配的架构必须与之对抗，导致性能不佳或资源消耗过多。

表 4.5 提供了一个系统框架，用于匹配数据特性到适当的架构：

表 4.5：**架构选择框架**：基于计算需求和模式类型，系统地匹配数据特性与神经网络架构。

| **架构** | **数据类型** | **关键特性** | **示例应用** |
| --- | --- | --- | --- |
| **MLPs** | 表格/结构化 | • 无空间/时间 • 任意关系 • 密集连接 | • 金融建模 • 医学测量 • 推荐系统 |
| **卷积神经网络（CNNs**） | 空间/网格状 | • 局部模式 • 平移等变性 • 参数共享 | • 图像识别 • 2D 传感器数据 • 信号处理 |
| **循环神经网络（RNNs**） | 序列/时间 | • 时间依赖性 • 变长 • 时间上的记忆 | • 时间序列预测 • 简单语言任务 • 语音识别 |
| **Transformers** | 复杂关系 | • 长距离依赖性 • 注意力机制 • 动态关系 | • 语言理解 • 机器翻译 • 复杂推理任务 |

虽然数据特性指导了初始架构选择，但计算约束通常决定了最终的可行性。理解每种架构的扩展行为可以促进合理的资源规划和部署决策。

### 计算复杂性考虑

架构选择必须考虑到计算和内存权衡，这些权衡决定了部署的可行性。每种架构都表现出独特的扩展行为，随着问题规模的增加，会形成不同的瓶颈。理解这些模式可以促进合理的资源规划，并在部署期间避免昂贵的架构不匹配。

每种架构的计算特征反映了其背后的设计理念。密集架构如 MLP 通过全连接优先考虑表示能力，而结构化架构如 CNN 通过参数共享和局部性假设实现效率。顺序架构如 RNN 以内存效率为代价换取并行化，而基于注意力的架构如 Transformers 则以内存换取计算灵活性。为了全面性，我们从计算扩展和内存访问两个角度（参见表 4.3）考察了这些相同的架构，因为每个视角都揭示了不同的优化机会和系统设计考虑。

表 4.6 总结了每种架构的关键计算特性：

表 4.6：**计算复杂性比较**：主要神经网络架构的扩展行为和资源需求。变量：<semantics><mi>d</mi><annotation encoding="application/x-tex">d</annotation></semantics> = 维度，<semantics><mi>h</mi><annotation encoding="application/x-tex">h</annotation></semantics> = 隐藏层大小，<semantics><mi>k</mi><annotation encoding="application/x-tex">k</annotation></semantics> = 核大小，<semantics><mi>c</mi><annotation encoding="application/x-tex">c</annotation></semantics> = 通道数，<semantics><mrow><mi>H</mi><mo>,</mo><mi>W</mi></mrow><annotation encoding="application/x-tex">H,W</annotation></semantics> = 空间维度，<semantics><mi>T</mi><annotation encoding="application/x-tex">T</annotation></semantics> = 时间步长，<semantics><mi>n</mi><annotation encoding="application/x-tex">n</annotation></semantics> = 序列长度，<semantics><mi>b</mi><annotation encoding="application/x-tex">b</annotation></semantics> = 批处理大小。

| **架构** | **参数** | **正向传播** | **内存** | **并行化** |
| --- | --- | --- | --- | --- |
| **MLPs** | <semantics><mrow><mi>O</mi><mo stretchy="false" form="prefix">(</mo><msub><mi>d</mi><mtext mathvariant="normal">in</mtext></msub><mo>×</mo></mrow><annotation encoding="application/x-tex">O(d_{\text{in}}\times</annotation></semantics> <semantics><mrow><msub><mi>d</mi><mtext mathvariant="normal">out</mtext></msub><mo stretchy="false" form="postfix">)</mo></mrow><annotation encoding="application/x-tex">d_{\text{out}})</annotation></semantics> 每层 | <semantics><mrow><mi>O</mi><mo stretchy="false" form="prefix">(</mo><msub><mi>d</mi><mtext mathvariant="normal">in</mtext></msub><mo>×</mo></mrow><annotation encoding="application/x-tex">O(d_{\text{in}}\times</annotation></semantics> <semantics><mrow><msub><mi>d</mi><mtext mathvariant="normal">out</mtext></msub><mo stretchy="false" form="postfix">)</mo></mrow><annotation encoding="application/x-tex">d_{\text{out}})</annotation></semantics> 每层 | <semantics><mrow><mi>O</mi><mrow><mo stretchy="true" form="prefix">(</mo><msup><mi>d</mi><mn>2</mn></msup><mo stretchy="true" form="postfix">)</mo></mrow></mrow><annotation encoding="application/x-tex">O(d²)</annotation></semantics> 权重 <semantics><mrow><mi>O</mi><mrow><mo stretchy="true" form="prefix">(</mo><mi>d</mi><mo>×</mo><mi>b</mi><mo stretchy="true" form="postfix">)</mo></mrow></mrow><annotation encoding="application/x-tex">O(d\times b)</annotation></semantics> 激活 | 优秀的矩阵操作并行 |
| **CNNs** | <semantics><mrow><mi>O</mi><mo stretchy="false" form="prefix">(</mo><msup><mi>k</mi><mn>2</mn></msup><mo>×</mo></mrow><annotation encoding="application/x-tex">O(k²\times</annotation></semantics> <semantics><mrow><msub><mi>c</mi><mtext mathvariant="normal">in</mtext></msub><mo>×</mo></mrow><annotation encoding="application/x-tex">c_{\text{in}}\times</annotation></semantics> <semantics><mrow><msub><mi>c</mi><mtext mathvariant="normal">out</mtext></msub><mo stretchy="false" form="postfix">)</mo></mrow><annotation encoding="application/x-tex">c_{\text{out}})</annotation></semantics> 每层 | <semantics><mrow><mi>O</mi><mrow><mo stretchy="false" form="prefix">(</mo><mi>H</mi><mo>×</mo><mi>W</mi><mo>×</mo></mrow><annotation encoding="application/x-tex">O(H\times W\times</annotation></semantics> <semantics><mrow><msup><mi>k</mi><mn>2</mn></msup><mo>×</mo></mrow><annotation encoding="application/x-tex">k²\times</annotation></semantics> <semantics><mrow><msub><mi>c</mi><mtext mathvariant="normal">in</mtext></msub><mo>×</mo></mrow><annotation encoding="application/x-tex">c_{\text{in}}\times</annotation></semantics> <semantics><mrow><msub><mi>c</mi><mtext mathvariant="normal">out</mtext></msub><mo stretchy="false" form="postfix">)</mo></mrow><annotation encoding="application/x-tex">c_{\text{out}})</annotation></semantics> | <semantics><mrow><mi>O</mi><mrow><mo stretchy="true" form="prefix">(</mo><mi>H</mi><mo>×</mo><mi>W</mi><mo>×</mo><mi>c</mi><mo stretchy="true" form="postfix">)</mo></mrow></mrow><annotation encoding="application/x-tex">O(H\times W\times c)</annotation></semantics> 特征 | <semantics><mrow><mi>O</mi><mrow><mo stretchy="true" form="prefix">(</mo><msup><mi>k</mi><mn>2</mn></msup><mo>×</mo><msup><mi>c</mi><mn>2</mn></msup><mo stretchy="true" form="postfix">)</mo></mrow></mrow><annotation encoding="application/x-tex">O(k²\times c²)</annotation></semantics> 权重 | 良好的空间独立性 |
| **RNNs** | <semantics><mrow><mi>O</mi><mrow><mo stretchy="true" form="prefix">(</mo><msup><mi>h</mi><mn>2</mn></msup><mo>+</mo><mi>h</mi><mo>×</mo><mi>d</mi><mo stretchy="true" form="postfix">)</mo></mrow></mrow><annotation encoding="application/x-tex">O(h²+h\times d)</annotation></semantics> 总计 | <semantics><mrow><mi>O</mi><mrow><mo stretchy="true" form="prefix">(</mo><mi>T</mi><mo>×</mo><msup><mi>h</mi><mn>2</mn></msup><mo stretchy="true" form="postfix">)</mo></mrow></mrow><annotation encoding="application/x-tex">O(T\times h²)</annotation></semantics> 对于 <semantics><mi>T</mi><annotation encoding="application/x-tex">T</annotation></semantics> 时间步 | <semantics><mrow><mi>O</mi><mrow><mo stretchy="true" form="prefix">(</mo><mi>h</mi><mo stretchy="true" form="postfix">)</mo></mrow></mrow><annotation encoding="application/x-tex">O(h)</annotation></semantics> 隐藏状态（常数） | 较差的序列依赖性 |
| **Transformers** | <semantics><mrow><mi>O</mi><mrow><mo stretchy="true" form="prefix">(</mo><msup><mi>d</mi><mn>2</mn></msup><mo stretchy="true" form="postfix">)</mo></mrow></mrow><annotation encoding="application/x-tex">O(d²)</annotation></semantics> 投影 <semantics><mrow><mi>O</mi><mrow><mo stretchy="true" form="prefix">(</mo><msup><mi>d</mi><mn>2</mn></msup><mo>×</mo><mi>h</mi><mo stretchy="true" form="postfix">)</mo></mrow></mrow><annotation encoding="application/x-tex">O(d²\times h)</annotation></semantics> 多头 | <semantics><mrow><mi>O</mi><mo stretchy="false" form="prefix">(</mo><msup><mi>n</mi><mn>2</mn></msup><mo>×</mo><mi>d</mi><mo>+</mo><mi>n</mi></mrow><annotation encoding="application/x-tex">O(n²\times d+n</annotation></semantics> <semantics><mrow><mi>×</mi><mi>d</mi><mi>²</mi><mo stretchy="false" form="postfix">)</mo></mrow><annotation encoding="application/x-tex">\times d²)</annotation></semantics> 每层 | <semantics><mrow><mi>O</mi><mrow><mo stretchy="true" form="prefix">(</mo><msup><mi>n</mi><mn>2</mn></msup><mo stretchy="true" form="postfix">)</mo></mrow></mrow><annotation encoding="application/x-tex">O(n²)</annotation></semantics> 注意力 <semantics><mrow><mi>O</mi><mrow><mo stretchy="true" form="prefix">(</mo><mi>n</mi><mo>×</mo><mi>d</mi><mo stretchy="true" form="postfix">)</mo></mrow></mrow><annotation encoding="application/x-tex">O(n\times d)</annotation></semantics> 序列 | 优秀（位置）受限于内存 |

#### 可扩展性和生产考虑因素

生产部署引入了超出算法性能的约束，包括延迟要求、内存限制、能源预算和容错需求。每种架构都表现出独特的生产特性，这些特性决定了实际应用的可行性。

MLPs 和 CNNs 通过数据并行性在多个设备上具有良好的可扩展性，通过适当的批量大小缩放实现接近线性的加速。RNNs 由于序列依赖性而面临并行化挑战，需要管道并行或其他专用技术。Transformers 在序列位置上实现了出色的并行化，但受到二次内存缩放的限制，这限制了批量大小和有效利用率。

MLPs 提供与层大小成比例的可预测延迟，这使得它们适合具有严格 SLA 要求的实时应用。CNNs 的延迟根据实现策略和硬件能力而变化，优化实现可以达到亚毫秒级的推理。RNNs 会根据序列长度创建延迟依赖，这使得它们在交互式应用中具有挑战性。Transformers 为批量处理提供了出色的吞吐量，但由于注意力开销，在单次推理延迟方面存在困难。

在生产环境中，不同架构的内存需求差异很大。MLPs 需要与模型大小成比例的固定内存，这使得容量规划变得简单。CNNs 需要可变内存来存储特征图，其大小与输入分辨率成比例，对于可变大小的输入需要动态内存管理。RNNs 维护隐藏状态的常量内存，但对于非常长的序列可能需要无界的内存。Transformers 面临二次内存增长，这在生产中为序列长度设置了硬性限制。

容错和恢复特性在架构之间差异显著。MLPs 和 CNNs 表现出无状态计算，这使得简单的检查点和恢复成为可能。RNNs 维护时间状态，这使分布式训练和故障恢复过程复杂化。Transformers 结合了无状态计算和巨大的内存需求，使得检查点大小成为大型模型的一个实际关注点。

硬件映射效率在架构模式之间差异很大。现代 MLPs 在专门的张量单元上实现了 80-90%的峰值硬件性能。CNNs 的效率取决于层配置和内存层次结构设计，通常在 60-75%之间。RNNs 由于顺序约束和不规则的内存访问模式，通常只能达到峰值性能的 30-50%。Transformers 对于大批次大小可以达到 70-85%的效率，但对于小批次由于注意力开销而显著下降。

#### 硬件映射和优化策略

不同的架构模式需要不同的优化策略来实现高效的硬件映射。理解这些模式能够实现系统性的性能调整和硬件选择决策。

MLPs 中的密集矩阵运算自然映射到张量处理单元和 GPU 张量核心。这些操作受益于几个关键优化：矩阵分块以适应缓存层次结构，混合精度计算以加倍吞吐量，以及操作融合以减少内存流量。最优的分块大小取决于缓存层次结构，通常是 64x64 用于 L1 缓存和 256x256 用于 L2，而张量核心通过特定的维度倍数达到峰值效率，例如 Volta 架构中的 16x16 块。

CNNs 得益于专门的卷积算法和数据布局优化，这些优化与密集矩阵运算有显著差异。Im2col 变换将卷积转换为矩阵乘法，但会加倍内存使用。Winograd 算法通过降低 3x3 卷积的算术复杂度 2.25 倍来提高效率，但以数值稳定性为代价。使用自定义内核的直接卷积实现了最优的内存效率，但需要针对特定架构进行调整。

RNN 由于其时间依赖性，需要不同的优化方法。循环展开减少了控制开销，但增加了内存使用。状态向量化使得可以在多个序列上执行 SIMD 操作。波前并行化利用时间步之间的独立性进行双向处理。

Transformer 由于其二次方复杂度，需要专门的注意力优化。FlashAttention 算法通过在线 softmax 计算和梯度重计算，将内存使用从 O(n²) 降低到 O(n)。包括局部、步进和随机方法在内的稀疏注意力模式在降低复杂度的同时保持建模能力。多查询注意力在头部之间共享键和值投影，将内存带宽降低了 30-50%。

多层感知器代表了最直接的计算模式，其成本主要由矩阵乘法决定。使 MLP 能够建模任意关系的密集连接以二次方参数增长为代价。每个神经元都与前一层中的每个神经元相连，创建了大量的参数，这些参数随着网络宽度的增加而二次方增长。计算主要由矩阵-向量乘法主导，这在现代硬件上得到了高度优化。矩阵操作本质上是并行的，并且可以有效地映射到 GPU 架构，每个输出神经元都是独立计算的。降低这些参数数量的优化技术，包括修剪和低秩近似，特别是针对密集层的，在第十章中进行了介绍。

卷积神经网络通过参数共享和空间局部性实现计算效率，但其成本随着空间维度和通道深度的增加而增加。卷积操作的计算强度很大程度上取决于内核大小和特征图分辨率。与等效的 MLP 相比，跨空间位置的参数共享大大减少了内存使用，而计算成本随着图像分辨率线性增长，随着内核大小二次方增长。特征图内存的使用占主导地位，对于高分辨率输入变得难以承受。空间独立性使得可以在不同的空间位置和通道之间进行并行处理，尽管内存带宽经常成为限制因素。

循环神经网络在牺牲并行化的代价下优化内存效率。它们的顺序特性创造了计算瓶颈，但允许以恒定的内存开销处理可变长度的序列。对于大隐状态，隐藏到隐藏的连接（<semantics><msup><mi>h</mi><mn>2</mn></msup><annotation encoding="application/x-tex">h²</annotation></semantics>项）占参数数量的主导地位。顺序依赖性阻止了时间上的并行处理，使得循环神经网络在本质上比前馈替代方案慢。它们对隐藏状态存储的恒定内存使用使得循环神经网络在长序列中内存效率高，但这种效率是以计算速度为代价的。

通过注意力机制，Transformer 实现了最大的灵活性，但付出了高昂的内存使用代价。它们与序列长度的二次关系限制了它们可以处理的序列长度。参数数量与模型维度成比例，但与序列长度无关。对于长序列，注意力计算中的<semantics><msup><mi>n</mi><mn>2</mn></msup><annotation encoding="application/x-tex">n²</annotation></semantics>项占主导地位，而来自前馈层的<semantics><mrow><mi>n</mi><mo>×</mo><msup><mi>d</mi><mn>2</mn></msup></mrow><annotation encoding="application/x-tex">n \times d²</annotation></semantics>项在短序列中占主导地位。注意力矩阵创建了主要的内存瓶颈，因为每个注意力头必须存储所有序列位置之间的成对相似性，导致长序列的内存使用不可承受。虽然序列位置和注意力头之间的并行化非常出色，但二次内存需求通常迫使批量大小更小，限制了有效的并行化。

这些复杂性模式定义了每个架构的最佳领域。当参数效率不是关键因素时，MLPs 表现卓越；对于中等分辨率的空間数据，CNNs 占主导地位；RNNs 在内存受限的非常长序列中仍然可行；而 Transformer 在复杂关系任务中表现卓越，其计算成本通过优越的性能得到证明。

### 建筑比较总结

对每个架构家族的系统分析揭示了不同的计算特征，这些特征决定了它们在不同部署场景中的适用性。表 4.7 提供了关键系统指标的定量比较，使工程师能够在模型能力和计算约束之间做出明智的权衡。

表 4.7：**定量架构比较**：对四个主要神经网络架构的计算复杂性分析。参数随网络维度缩放（N=神经元，M=输入，K=核大小，C=通道，D=深度，H=隐藏层大小，T=时间步长，d=模型维度）。内存需求反映训练期间的峰值激活存储。并行性表示适合并行计算。关键瓶颈代表典型部署中的主要性能限制因素。

| **指标** | **MLP** | **CNN** | **RNN** | **Transformer** |
| --- | --- | --- | --- | --- |
| **参数** | O(N×M) | O(K²×C×D) | O(H²) | O(N×d²) |
| **（每样本 FLOPs）** | O(N×M) | O(K²×H×W×C) | O(T×H²) | O(N²×d) |
| **内存** | O(B×M) | O(B×H×W×C) | O(B×T×H) | O(B×N²) |
| **（激活）** |  |  |  |  |
| **并行性** | 高 | 高 | 低（顺序） | 高 |
| **关键瓶颈** | 内存带宽 | 内存带宽 | 顺序依赖 | 内存（N²） |

这个定量框架通过明确揭示决定计算可行性的缩放行为，使系统性的架构选择成为可能。MLP 和 CNN 实现高并行性，但随着模型大小的增长面临内存带宽限制。RNN 保持恒定的内存使用，但为了顺序处理牺牲了并行性。Transformer 实现最大表达能力，但面临二次内存缩放，限制了序列长度。理解这些权衡对于将架构选择与部署限制相匹配至关重要。

### 决策框架

有效的架构选择需要平衡多个相互竞争的因素：数据特征、计算资源、性能要求和部署限制。虽然数据模式提供初始指导，复杂性分析建立可行性界限，但最终的架构选择通常涉及细微的权衡，需要系统性的评估。

![](img/file65.svg)

图 4.12：**架构选择决策框架**：基于数据特征和部署限制的系统流程图，用于选择神经网络架构。该过程从数据类型识别（文本/序列/图像/表格）开始，以选择初始架构候选者（Transformer/RNN/CNN/MLPs），然后迭代评估内存预算、计算成本、推理速度、准确度目标和硬件兼容性。

图 4.12 提供了一种结构化的架构选择决策方法，确保考虑所有相关因素，同时避免基于新颖性或感知复杂性的常见陷阱。决策流程图通过首先将数据特征与架构优势相匹配，然后验证实际限制，来指导系统性的架构选择。该过程本质上是迭代的——资源限制或性能差距通常需要重新考虑早期选择。

此框架通过四个关键步骤应用。首先，数据分析：数据中的模式类型提供了最强的初始信号。空间数据自然与卷积神经网络（CNNs）相匹配，序列数据与循环神经网络（RNNs）相匹配。其次，渐进式约束验证：每个约束检查（内存、计算预算、推理速度）都充当一个过滤器。任何约束失败都需要降低当前架构的规模或考虑一个根本不同的方法。

第三，当准确性目标未达成时，进行迭代权衡处理。可能需要额外的模型容量，这需要回到约束检查。如果部署硬件无法支持所选架构，可能需要重新考虑整个架构方法。第四，预期多次迭代，因为真实项目通常在实现数据拟合、计算可行性和部署要求之间的最佳平衡之前，会多次循环通过此框架。

这种系统方法防止仅基于新颖性或感知到的复杂性选择架构，确保选择与问题要求和系统能力相一致。

## 统一框架：归纳偏差

探索的架构多样性——从多层感知器（MLPs）到转换器（Transformers）——共享一个统一的理论框架：每个架构体现特定的归纳偏差，这些偏差限制了假设空间并指导学习，以适应不同的数据类型和问题结构。

不同的架构形成了一个递减的归纳偏差层次。卷积神经网络（CNNs）通过局部连接、参数共享和平移等变性表现出最强的归纳偏差。这些约束大大减少了参数空间，同时限制了局部结构的空间数据的灵活性。循环神经网络（RNNs）通过序列处理和共享时间权重表现出适度的归纳偏差。隐藏状态机制假定过去的信息会影响当前的处理，这使得它们适用于时间序列。

多层感知器（MLPs）在层内处理之外保持最小的架构偏差。密集连接允许建模任意关系，但需要更多数据来学习其他架构明确编码的结构。转换器通过学习到的注意力模式表示自适应归纳偏差。该架构可以根据数据动态调整其归纳偏差，结合灵活性以及发现相关结构规律的能力。

所有成功的架构都实现了某种形式的分层表示学习，但通过不同的机制。CNN 通过逐步扩展感受野构建空间层次结构，应用了第 4.3 节中详细描述的空间模式处理框架。RNN 通过隐藏状态演变构建时间层次结构，扩展了第 4.4 节中的顺序处理方法。转换器通过多头注意力构建内容相关的层次结构，应用了第 4.5 节中描述的动态模式处理机制。

这种分层组织反映了一个原则：复杂的模式可以通过简单组件的组合有效地表示。深度学习成功的原因在于发现，当提供适当的架构归纳偏差时，基于梯度的优化可以有效地学习这些组合结构。

关于表示学习的理论洞察对系统工程有直接影响。分层表示需要能够高效地将较低级别的特征组合成高级抽象的计算模式。这推动了系统设计决策：

+   存储层次结构必须与表示层次结构对齐以最小化数据移动成本

+   并行化策略必须尊重分层计算的依赖结构

+   硬件加速器必须高效地支持实现特征组合的矩阵运算

+   软件框架必须提供抽象，以实现跨不同架构的高效分层计算

将架构视为体现不同的归纳偏差有助于解释它们的优点和系统需求，为架构选择和系统优化决策提供原则性的基础。

## 谬误与陷阱

神经网络架构代表为不同数据类型和问题域设计的专用计算结构，这导致了关于它们选择和部署的常见误解。从密集网络到转换器等丰富的架构模式往往导致工程师基于新颖性或感知的复杂性而非特定任务的要求和计算约束做出选择。

**谬误**：*更复杂的架构总是比简单的架构表现更好*。

这种误解促使团队立即采用基于 transformer 的模型或复杂的架构，而不了解其需求。虽然像 transformer 这样的复杂架构在处理需要长距离依赖的复杂任务方面表现出色，但它们需要显著更多的计算资源和内存。对于许多问题，尤其是那些数据有限或具有明显结构模式的问题，简单的架构（如 MLPs 或 CNNs）在显著减少计算开销的情况下也能达到相当的准确度。架构选择应与问题复杂性相对应，而不是默认选择最先进的选择。

**陷阱：** *在模型选择过程中忽视了架构选择的计算影响。*

许多从业者仅根据学术论文中的准确度指标选择架构，而没有考虑计算需求。CNN 的空间局部性假设可能为图像任务提供出色的准确度，但需要特定的内存访问模式。同样，RNN 的序列依赖性创建了序列瓶颈，限制了并行化机会。这种疏忽导致模型在生产环境中无法满足延迟要求或超出内存限制时部署失败。

**谬误：** *架构性能与硬件特性无关。*

这种信念假设所有架构在不同硬件平台上表现都一样好。实际上，不同的架构利用不同的硬件特性：CNN 受益于专门的张量核心，MLPs 利用高带宽内存，而 RNN 需要高效的序列处理能力。在 GPU 上实现最佳性能的模型可能在移动设备或嵌入式处理器上表现不佳。理解硬件-架构对齐对于有效的部署策略至关重要。

**陷阱：** *在不了解其交互效应的情况下混合架构模式。*

结合不同的架构组件（例如向 CNN 添加注意力层或在 RNN 中使用跳过连接）可能会产生意外的计算瓶颈。每种架构模式都表现出独特的内存访问模式和计算特性。简单的组合可能会消除单个组件的性能优势或产生内存带宽冲突。成功的混合架构需要仔细分析不同模式在系统层面的交互。

**陷阱：** *在设计架构时没有考虑部署管道中全硬件-软件协同设计的全部影响。*

许多架构决策优化高端 GPU 性能，而没有考虑从开发到部署的整个系统生命周期。为大规模计算集群设计的架构可能由于内存限制、缺乏专用计算单元或有限的并行化能力，不适合边缘部署。同样，针对推理延迟优化的架构可能会牺牲开发效率，导致更长的开发周期和更高的计算成本。有效的架构选择需要分析整个系统堆栈，包括计算基础设施、模型编译和优化工具、目标部署硬件和操作约束。在 CNN 深度和宽度、Transformer 头配置或激活函数之间的选择会对内存带宽利用率、缓存效率和数值精度要求产生级联效应，这些都需要整体考虑，而不是孤立地考虑。

## 摘要

神经网络架构形成了专门的计算结构，针对处理不同类型的数据和解决不同类别的问题。多层感知器通过密集连接处理表格数据，卷积网络利用图像中的空间局部性，循环网络处理序列信息。每个架构都体现了对数据结构和计算模式的具体假设。现代 Transformer 架构通过注意力机制统一了许多这些概念，该机制根据相关性而不是固定的连接模式动态路由信息。

尽管这些架构在表面上看起来多样化，但它们共享基本的计算原语，这些原语在不同设计中反复出现。矩阵乘法操作构成了计算核心，无论是在密集层、卷积还是注意力机制中。内存访问模式在架构之间差异很大，一些架构需要滑动窗口操作进行局部处理，而另一些架构则要求全局信息聚合。注意力机制中的动态计算模式创建了数据依赖的执行流程，这对传统的优化方法构成了挑战。

**关键要点**

+   不同的架构对数据结构有特定的假设：MLPs 用于表格数据，CNNs 用于空间关系，RNNs 用于序列，Transformers 用于灵活的注意力

+   包括矩阵运算、滑动窗口和动态路由在内的共享计算原语构成了跨不同架构的基础

+   内存访问模式和数据处理需求在架构之间差异很大，这直接影响系统性能和优化策略

+   理解算法意图与系统实现之间的映射关系，能够实现有效的性能优化和硬件选择

本章中建立的建筑学基础——计算模式、内存访问特性和数据移动原语——直接指导了后续章节中探讨的专用硬件和优化策略的设计。理解 CNN 表现出空间局部性使得可以开发针对卷积操作的谐振阵列优化（第十一章）。认识到 Transformer 需要二次方内存扩展促使了如 FlashAttention 和稀疏注意力模式等注意力特定优化（第十章）。从架构理解到硬件设计再到算法优化的发展过程代表了机器学习系统工程的系统方法。

随着架构变得更加动态和复杂，算法创新与系统优化之间的关系对于在现实世界的部署中实现实际性能提升变得越来越关键。在第十三章中讨论了在生产环境中部署和维护这些复杂架构的操作挑战，而在第十八章中探讨了可持续人工智能发展的更广泛影响，包括由架构选择引起的能源效率考虑。

* * *
