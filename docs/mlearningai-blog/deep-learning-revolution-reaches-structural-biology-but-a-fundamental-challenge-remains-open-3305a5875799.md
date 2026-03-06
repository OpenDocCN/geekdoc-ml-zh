# Deep learning revolution reaches structural biology, but a fundamental challenge remains open.

> 原文：<https://medium.com/mlearning-ai/deep-learning-revolution-reaches-structural-biology-but-a-fundamental-challenge-remains-open-3305a5875799?source=collection_archive---------8----------------------->

![](img/68ba7b87bd9a6ba8b452b40d9b67172d.png)

credit: AlphaFold DB, entry Q04207

Life as we know it, is an ensemble of tightly interconnected and regulated chemical reactions. These reactions, typically involve giant molecules composed by thousands of atoms arranged in a certain order. Structural biology is the study of how the spatial structure of these macromolecules affect their biological function. However, deciphering the atomic arrangement of biological macromolecules is not an easy task, particularly for proteins.

In 1962, Max Perutz and John Kendrew were awarded the Nobel Prize in Chemistry for their pioneering work on elucidating the atomic structure of proteins by X-ray crystallography. This monumental achievement opened the doors to a remarkable progress in structural biology. Several thousand structures of proteins had been described ever since. Other techniques like nuclear magnetic resonance (NMR), and cryo-electron microscopy greatly expanded the toolbox of structural biologists. Despite the notable advances in structure determination, many proteins still pose a significant barrier, to reveal its three-dimensional (3D) structure by experimental means. To fill the gap in experimental tools, computational methods had been developed for the prediction of protein structure. Computational algorithms could generate theoretical models of the 3D structure that a protein will fold into, based largely on its primary structure, that is, its amino acid sequence.

Despite notable progress on bioinformatics, achieving atomic accuracy in the predicted models had proven difficult. Even for small proteins, the total number of unique conformations is astronomical. Additionally, the physic laws governing protein folding are computationally expensive to calculate. However, in 2021 two groundbreaking reports foretold the advent of revolutionary approaches in structural bioinformatics. For the first time, computational algorithms achieved predictions with close-to-experimental atomic accuracies. Such jump in performance, resulted from a novel approach that employed state of the art machine learning methods.

**The deep learning revolution**

用于预测生物分子结构的新型深度学习算法彻底背离了二十多年来定期部署的传统方法。经典的结构生物信息学工具使用能量函数和力场来描述生物大分子中的原子相互作用。另一方面，深度学习算法训练神经网络从实验确定的蛋白质结构的大数据集的输入一级序列中生成精确的 3D 结构。这些新方法通过对训练集的数百万个参数进行多次迭代的进化学习来完善分子几何结构。与经典力场模拟不同，新方法中的连续几何细化通过深度学习进行优化——这导致轨迹总是朝着最优方向移动，并收敛于非常精确的结构。

深度学习革命的到来具体化在**alpha fold**(*Nature*2021，DOI:[10.1038/s 41586–021–03819–2](https://www.nature.com/articles/s41586-021-03819-2))和**Rosetta fold**(*Science*2021，DOI:[10.1126/Science . abj 8754](https://www.science.org/doi/10.1126/science.abj8754))。AlphaFold 算法是由 Alphabet Inc .位于伦敦的子公司 DeepMind 于 2020 年开发的，在两年一度的第 14 届 CASP(结构预测的关键评估)比赛中，AlphaFold 在达到接近实验的原子精度方面表现出了一致性，并大大超过了其他参赛者。AlphaFold 前所未有的准确性和速度让 DeepMind 与欧洲分子生物学实验室的欧洲生物信息学研究所(EMBL-EBI)联手，建立了一个庞大的结构预测数据库 AlphaFold DB，其中包含超过 800，000 个结构，跨越 21 个模式生物蛋白质组( *NAR* 2021，DOI:[10.1093/NAR/gkab 1061](https://academic.oup.com/nar/article/50/D1/D439/6430488?login=false))。

DeepMind 在 AlphaFold 上的成功激发了 David Baker ( [华盛顿大学](https://sites.uw.edu/biochemistry/faculty/david-baker/))及其同事开发 RoseTTAFold，这是一种三轨神经网络，能够实现结构预测，精确度与 AlphaFold 相当。RoseTTAFold 和 AlphaFold 的组合进一步增强了结构生物学领域中的机器学习方法的能力，特别是在蛋白质-蛋白质相互作用的研究中。例如，Baker 和他的同事利用 RoseTTAFold 和 AlphaFold 的组合实现了酵母中蛋白质相互作用的蛋白质组范围的预测。作者证明了该组合与单独使用任一种方法相比的优越性。预测的结构，产生了基本真核生物蛋白质-蛋白质关联的模型，揭示了关于生物功能的有趣线索(*科学* 2021，DOI:[10.1126/科学. abm4805](https://doi.org/10.1126/science.abm4805) )。

随着 AlphaFold 和 RoseTTAFold 的出现，结构生物学领域将会发生翻天覆地的变化。科学家现在可以准确预测几乎任何蛋白质的三维结构。此外，模型的高质量增加了对可靠生物学见解的提取以及假设检验实验的设计的信心。

尽管深度学习提供了这些强大的方法，但仍然存在重大挑战。许多蛋白质区域没有明确的三维结构，但却编码蛋白质的功能关键部分。此外，通过当前的 AlphaFold 算法( *Nature* 2021，DOI:[10.1038/s 41586–021–03828–1](https://www.nature.com/articles/s41586-021-03828-1))，大约 42%的人类蛋白质组不能以高置信度进行预测。这些蛋白质中的许多通常形成短命的结构，并对我们理解蛋白质折叠提出了根本性的挑战。

**预测蛋白质折叠的问题**

![](img/8f5a1e58490157eb3f8965b61365abd9.png)

The structure of the transactivation domain of RelA. (A) Secondary structure of RelA resolved experimentally by 2D-NMR (entry: 2LWW, PDB). (B) Secondary structure of RelA predicted by AlphaFold (entry: Q04207, AlphaFold DB)

没有明确形状的蛋白质被称为固有无序蛋白质(IDP)。这种蛋白质序列不能自发地形成持久的 3D 结构。它们在动力学上是无序的，并迅速从伸展的线圈转变成塌陷的小球。IDP 的特征在于它们的灵活性，这是由于高比例的带电和亲水性氨基酸以及因此低含量的疏水性残基( *Nature* 2015，DOI: [10.1038/nrm3920](https://www.nature.com/articles/nrm3920) )。

尽管没有确定的 3D 形状，IDP 经常充当生物网络中的主要枢纽，由于其物理特性，在代谢级联中实现了精致的控制水平。IDP 的灵活性允许与不同的靶标混杂相互作用，并实现有效的翻译后修饰(PTM)。内在紊乱允许通过 PTMs 微调的多个基序进行分子相互作用，以这种方式，一个氨基酸序列可以调节不同的信号通路并引起不同的生物反应。

对蛋白质折叠问题的充分理解，必须考虑大分子几何结构上的动态和瞬时事件。诸如 AlphaFold 和 RoseTTAFold 的方法提供了蛋白质 3D 结构的静态快照，其不能准确地代表 IDPs 的关键构象。这方面的一个例子可以在属于转录因子 Nf-κB 家族的一个突出的蛋白质中看到。RelA 的转录激活结构域的结构分别在结合和自由状态的螺旋和延伸构象之间波动。然而，参与结合配偶体的分子识别的 RelA 上的瞬时螺旋构象没有被 AlphaFold 预测(条目: [Q04207](https://alphafold.ebi.ac.uk/entry/Q04207) ，AlphaFold DB)。幸运的是，使用二维核磁共振(2D-核磁共振)光谱的时间分辨结构研究可以揭示束缚态的短期二级结构(条目: [2LWW](https://www.rcsb.org/structure/2LWW) ，PDB)，为 RelA 的生物学功能提供了重要的见解。

AlphaFold 在处理 IDP 方面的缺点的类似证据来自最近的一份报告，该报告强调了神经网络对蛋白质结构上的错义突变的影响不敏感。错义突变导致蛋白质变得内在无序，结构完整性丧失。尽管有灾难性结构影响的实验证据，AlphaFold 预测这些突变不会丧失结构完整性( *Nature* 2022，DOI:[10.1038/s 41594–021–00714–2](https://www.nature.com/articles/s41594-021-00714-2))。

当前深度学习方法的局限性始于有限的训练数据。例如，随着更多实验数据变得可用，可以在破坏结构的突变数据库上训练更通用、更强大的深度学习网络。毫无疑问，蛋白质折叠问题的未来解决方案将需要解决大分子复合物的瞬时和动态性质。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)