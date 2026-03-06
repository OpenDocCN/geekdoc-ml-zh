# 让我们来谈谈古代帝国及其历史

> 原文：<https://medium.com/mlearning-ai/lets-talk-about-ancient-empires-and-their-history-d4538fb98598?source=collection_archive---------3----------------------->

对一些有趣数据的分析

![](img/3a2bfb61fcb4e4d333463c12cd12a2a0.png)

Photo by [Mustafa ezz](https://www.pexels.com/@ezz7?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/sphynx-egypt-1738536/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

> 世界上 90%的数据都是在过去两年中产生的。互联网公司充斥着可以分组和利用的数据。—正如 Petter Bae Brandtzæ在 2013 年的一篇文章中所述

从文明的黎明到冷战的结束，我们拥有的数据比过去 30 年要少得多。在 17 世纪中叶现代统计学出现之前，我们能够检验的数据量是极其微小的。如果我们回到 1086 年《末日审判书》之前，情况会变得更糟。

幸运的是，没有什么能阻止现代历史学家和学者产生新的历史数据。

## 轴向年龄数据集

为了进行分析，我使用了一个相对非常小的数据集，[轴向年龄数据集](https://www.kaggle.com/usharengaraju/axial-age-dataset)，由 [Seshat:全球历史数据库](http://seshatdatabank.info/datasets/axial-age-dataset/)提供，“一个由来自世界各地的进化科学家、历史学家、人类学家、考古学家、经济学家和其他社会科学家组成的大型多学科团队。”

```
This research employed data from the Seshat Databank (seshatdatabank.info) under Creative Commons Attribution Non-Commercial (CC By-NC SA) licensing.Turchin, P., R. Brennan, T. E. Currie, K. Feeney, P. François, […] H. Whitehouse. 2015\. “Seshat: The Global History Databank.” *Cliodynamics* 6(1): 77–107\. [https://doi.org/10.21237/C7clio6127917.](https://doi.org/10.21237/C7clio6127917)Mullins, D., D. Hoyer, […] P. Turchin. Preprint. “Mullins, D., D. Hoyer, […] P. Turchin. 2018\. “A Systematic Assessment of ‘Axial Age’ Proposals Using Global Comparative Historical Evidence.” *American Sociological Review* 83(3): 596–626\. [https://doi.org/10.1177/0003122418772567.](https://doi.org/10.1177/0003122418772567)
```

轴向年龄数据集追踪各种社会政治规范及其在非洲-欧亚大陆关键地区的发展。在 10 个 NGAs(自然地理区域)内，每个日期(以 100 年为增量，在公元前 5300 年和公元 1800 年之间变化的时间跨度)的每个社会政治规范的具体得分由一组专家商定，并编入数据集。

## 什么是轴性年龄？

轴心时代(也称为轴心时代)是伟大的知识、哲学和宗教体系出现的时代，这些体系形成了后来的人类文明和文化，大部分有人居住的世界几乎在同一时间出现。大多数主要宗教和精神传统出现的时期。

## 探索性数据分析

[链接到我的 GitHub 项目和代码库。](https://github.com/bharti-pd/EDA-History-Of-Ancient-Empires)

[链接到我的 Kaggle 笔记本，了解同一个项目。](https://www.kaggle.com/bhartiprasad17/history-of-ancient-empires)

![](img/5faf97a5e2dba9f98189ba918abb5bc4.png)

**Matrix to visualize the pattern of missingness in our data**

![](img/d453b4d6449d313bbb93966dd3cd8e9a.png)

**The bar chart gives us an idea about how many missing values are there in each column**

![](img/41f20dea35a30cff71a1dbf541ee6a85.png)

**Heatmap shows the correlation of missingness**

![](img/09960c0300bd0c432fe1803b1b5bfb2a.png)

**The dendrogram allows us to more fully correlate variable completion, revealing trends deeper**

正如我们从上面的可视化中看到的，这个数据集有很多缺失值。让我们直接进入数据可视化，因为我已经清理了数据。

![](img/b68b6cfe3bbc68914361b065d7b86693.png)

Autocorrelation plot

![](img/621ef7d7597fa2a30166830808bf6105.png)![](img/1b427739e49c08b8de4261456fb201e1.png)

数据集中每个 NGA 覆盖的时间段是不同的，从公元前 5300 年到公元 200 年不等。我们可以注意到，从公元前 5300 年到公元前 4000 年，没有积极的文化特征观察。其他地方的总体累积趋势是积极的。

![](img/876719f3305466511688dea00bd80d32.png)

随着时间的推移，大多数功能继续呈现总体上升趋势。然而，由于特定 nga 的特定偏移，图案不太平滑。

> 随着时间的推移，地中海和亚洲地区的各种社会发展出了什么样的社会政治规范？

![](img/44cf19e7844655190297412d24916079.png)

道德规范，在较小程度上，亲社会的促进，统治者不是神的信仰，正式的法典，法律的普遍适用性，对行政部门的约束，以及全职官僚是除关西以外所有 NGA 的共同特征，这可能是由于 NGA 的空值的高价值。

![](img/45181f2d87275d99fb43bbb14ef4faf1.png)

加利利、卡奇平原、科尼亚平原、苏西亚纳、上埃及和克里特岛(程度较轻)在将统治者和平民等同于精英方面表现明显更好。

![](img/5bcf19ee10dbf021c2bfedb1540e8261.png)

黄河流域的弹劾案确实比上埃及地区略多。

![](img/825401ac4da624e3772ceb1f43c41b42.png)

除了关西，每个 NGA 都有比拉丁姆多得多的全职官员。

![](img/92af7d2d1f2b4ad4ac2fc02fa3f858d6.png)

道德惩罚和对无所不知的超自然生物的信仰在整个古典世界都很普遍，除了柬埔寨和卡奇平原。

![](img/bfef48015ccd11466e14e6cfcba9c27c.png)

克里特岛有一套共同的详细和正式的规则，但比柬埔寨更强调个人福利。虽然在某些情况下统治者可能会被替换，但统治者和平民在感知的重要性方面存在显著差异。有时也有对无所不知的上帝的信仰。

![](img/99da6edcf5dd904e9c50376cc6957206.png)

加利利有一个高度结构化的法典，它通常被强制执行和官僚化，但更侧重于社区利益和道德惩罚。平等被赋予了更高的优先地位，行政人员受到进一步的限制。由于对无所不知的超自然存在的信仰盛行，统治者变得不太可能被称为神。然后，一旦掌权，统治者很少被官僚手段取代。

![](img/9d3cf45716ad4eb859d4011806dd2caf.png)

随着时间的推移，柬埔寨盆地被一种优先考虑社区而非个人的文化，以及一个支持先进官僚制度的正式、广泛适用的法律体系所定义。尽管对权力关系的限制很少，但不平等似乎一直存在。

![](img/2be21f8c313963d4406f5c5fe6802cd4.png)

在卡奇平原上，尽管有遵守社会道德规范的巨大压力，但社会不太可能因为宗教而不是法律原因惩罚你。尽管法律有点可替代性，但在社会眼中，统治者、精英和平民拥有惊人的平等的内在价值。

![](img/c9ffc7a6bb880c8e9bf6bcd018aa5ab5.png)

在关西，融入社会的社会压力很大，再加上正式且广泛实施的法律法规和强大的官僚机构，这可能意味着人们普遍理解社会对他们的要求。尽管精英和平民相对平等，但统治者是上帝。

![](img/a0f851b7c2538dff0e697d18e67efe1a.png)

科尼亚平原的概况与加利利相似，有一些法治，对公共利益的关注，适度的平等，受限制的行政，以及对统治者不是神的信仰，以及对全知的超自然存在的信仰。另一方面，道德惩罚没有加利利那么普遍。

![](img/46fe5be24bc4f39130f6bb44438d2788.png)

像柬埔寨和关西一样，黄河中游地区是亲社会规范、坚实的法律体系和官僚机构相结合的典范。然而，统治者并不总是被视为神圣的，有时会受到制度上的约束。由于黄河流域的精英与平民不太等同，社会分层可能比关西更高。

![](img/8edf9f1cbb901767170d6a0540e1a685.png)

拉蒂姆是法治的典范，标准化的法典比道德惩罚、行政限制，甚至是强大的弹劾历史更受青睐。统治者不是神，人们普遍相信全能的超自然存在。然而，不平等现象普遍存在，普通人通常不像精英或统治者那样被社会所重视。

![](img/2a47dea242bbb3b69937042f0848356a.png)

尽管有明确的亲社会和道德标准、结构严谨的法典和一大批官僚，但法律似乎并没有在苏珊娜得到广泛实施。高管面临适度的约束，社会分层最小。

![](img/1ccb8f4e90307bfcbecdcdaf7a038154.png)

在埃及，道德规范非常重要，尽管存在一个正式的一般法律框架。有一个强大的官僚机构，精英和平民之间的平等程度相对较高。尽管统治者有时被认为是神圣的，但他们可能受到高度的制度约束。

> 这些规范中的哪些最有可能在同一时间同一地点得到遵守？

![](img/930e46ccb8f1bbd24c2734ef84307852.png)

正式的法律规范、亲社会促进和法律的普遍适用性最有可能在数量上被一起观察到，在结果中有明显的可能的相关性。弹劾是最不可能与精英和平民等同出现的特征，所有这些都有轻微的负面联系。

![](img/70efa0d20acce2fd49acc8ac27c02885.png)

弹劾也与全职官员的参与有关，尽管这种联系要弱得多。

弹劾和无所不知的超自然存在，亲社会的促进和道德惩罚是最不相关的特征，这意味着后三者与前者的存在或不存在没有任何实质性的联系。

轴心时代是早期人类历史上的一个关键时期，人类开始第一次思考个体的存在，以及生与死的意义。

## 轴心时代会再次发生吗？

有人说，我们现在可能正处于一个新时代的边缘。毫无疑问，技术已经改变了人们的生活方式、与社会的联系方式、沟通方式以及对周围世界的看法，无论是个人还是集体。第一个轴心时代标志着超越的发现。让我们看看未来会怎样。

在 LinkedIn 上与我联系[这里](https://www.linkedin.com/in/bharti-prasad/)。