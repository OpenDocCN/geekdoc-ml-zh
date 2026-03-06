# 转述一篇论文:《3D 建筑重建的深度学习:综述》

> 原文：<https://medium.com/mlearning-ai/re-tell-a-paper-deep-learning-for-3d-building-reconstruction-a-review-daa690b962ac?source=collection_archive---------2----------------------->

## 笔记，城市监控，3D 重建，深度学习。

## 我读过的论文的注释

![](img/140192f1dbb3ca1a6f67f0b019eeab21.png)

Photo by [Benjamin Davies](https://unsplash.com/@bendavisual?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

大家好，

在处理了一段时间的实际事情后，我有一些时间再次阅读论文并做一些笔记。在这个时候，我想分享我在 [Buyukdemircioglu 等人(2022)](https://www.researchgate.net/publication/360958629_DEEP_LEARNING_FOR_3D_BUILDING_RECONSTRUCTION_A_REVIEW) 写的一篇论文上做的笔记，这篇论文是关于 3D 重建方法的综述以及它是如何采用深度学习方法的。这篇论文对我们来说很有帮助，因为我们想了解 3D 建筑重建主题的背景知识。

# 介绍

总的来说，本文将三维建筑重建发展分为三个主要部分:传统方法、机器学习方法和深度学习方法。

我的一点小小的贡献是，我在论文中提供了已评审方法的列表。它们被写在块中。其中一些是我转述的，而另一些是直接从论文中引用的。我根据论文的正文引用列出了它，因此完整的参考请直接查看论文。

# 1.传统的

这里，常规方法是使用非机器学习的方法。它可以分为模型驱动和数据驱动方法。模型驱动方法尝试将屋顶的几何图形与库中的屋顶类型相匹配。而数据驱动方法从原始数据源生成模型，而不关注任何特定的参数。

模型驱动方法的一些优点和缺点是生成的模型可以是拓扑正确的，但它必须得到库中候选模型的支持。而在数据驱动的方法中，我们不需要指定任何候选，但生成的模型可能容易出现拓扑或几何错误。

在下面的方框中，列出了该论文综述的方法。在常规方法中，已经实现了各种方法来重建 3D 建筑模型，例如 RANSAC、半空间、ETL 等。

```
a. Musialski et al.(2013) and kada(2010)
   - Show comprehensive review on conventional urban reconstruction 
     methods.b. Kada and Wichmann(2012)
   - Apply subsurface growing metodhs for 3D building reconstructionc. Nan and Wonka(2017)
   - Data-driven method, polyfit
   - Lightwight polygonal surface from point cloudsd. Fischler and Bolles(1981)
   - RANSAC with contextual knowledge.e. Malihi et al.(2018)
   - Develpoed a novel two-level segmentation scheme.
   - Generating 3D building models from point clouds derived 
     from UAV photogrammetry.f. Buyukdemircioglu and Kocaman(2018)
   - LoD1 building derived by combining 2D building footprint 
     and DSMs.g. Buyukdemircioglu et al.(2018)
   - Use model driven approach for semi-automatics reconstruction of
     3D city models in LoD2 using stereo aerial imagery.h. Bizjak et al.(2021)
   - 3d building based on half-spaces in LoD2.i.Drešček et al.(2021)
  - ETL solution for 3D building reconstruction using unmanned   
    aerial vehicle,
  - Point cloud based.j. Murtiyoso et al.(2020)
  - Data-driven and algorithmic solution to the automatice
    reconstruction of 3D buildings at LoD2 from UAV point clouds.k. Li and Wu(2020)
  - Using incomplete points cloud with RANSAC and topological 
    relation constraints to generate 3D models of complex buildings.l. Li and shan(2022)
  - A RANSAC-based multi primitive reconstruction (MPR) to segment a
    compound boundary into predefined primitives and determine their 
    parameter values from the point clouds.m. Buyukdemircioglu and Kocaman(2018)
  - Generate LoD3 model manually and combined with automatically
    generated 3D city models in different LoDs.n. Peters et al. (2021)
  - Automatic workflow for reconstructing 3D building model in Lod1 
    and Lod2 based on 2D building footprints and LiDAR.
```

# 2.机器学习

```
a. Dehbi et al.(2016)
   - Developed weighted attribute context-free grammar rules for 
     3D building reconstruction.
   - Using SVM to infer the weighted context-free grammar.
   - Using input-output pairs as structured data.
   - Using MLNs to reconstruct the 3d building as a 
     statistical relation learning method.b. Biljecki et al.(2017)
   - Building models without elevation data by using the random 
     forest method.
   - Predict height based on footprints and building attributes and 
     then extrudes the footprint to generate 3D modelsc. Biljecki and Dehbi(2019)
   - They shows that it is possible to predict roof type from lower 
     Lod (Lod0 and Lod1) dataset to generate LoD2 models without 
     roof measurements.d. Park and Guldman(2019)
   - Reconstruct LoD1+ building models with ML-based point cloud 
     classification methodology,e. Farella et al.(2021)
   - Reconstruct building in 4D representation using ML algorithm
     and historical information.
```

# 3.深度学习

采用深度学习三维建筑重建的思想是为了克服传统方法的瓶颈。例如，传统方法很难自动学习三维形状的语义特征。然后，他们也高度依赖于图像的质量和内容。在本文中，研究了使用深度学习模型进行 3D 建筑物重建的三种方法:CNN、GANs 以及基于 DL 的方法和传统方法的组合。

```
a. Liu et al(2021)
   - Two main problems with conventional 3D reconstruction method.
   - Firstly, they involve multiple manual designs that can lead to 
     the accumulation of errors but can hardly learn semantic 
     features of 3D shapes automatically.
   - Secondly, they are highly dependent on the quality and content   
     of the images as well as precisely calibrated camera.
```

## 3.1.美国有线新闻网；卷积神经网络

```
a. Wang and Frahm(2017)
   - Proposed a DL model for performing a single-view parametric 
     reconstruction of buildings based on satellite imagery by  
     parameterizing buildings as 3D cuboids.b. Zeng et al.(2018)
   - Using Neural Procedural Reconstruction 3D points were mapped 
     into CAD model with procedural structure inferred by sequences 
     of shape grammar rules.c. Nishida et al.(2018)
   - User can generate a grammar automatically from a single image 
     of building with the help of CNNs for procedural modelling of 
     buildings.d. Alidoost et al.(2019)
   - Developed DL architecture for detecting  building from a single 
     aerial imagery.e. Agoub et al.(2019)
   - A pipeline based on multiple CNN with encoder and decoder 
     architecture.f. Alidoost et al.(2020)
   - Automatic detecting, localizing, and estimating building 
     heights simultaneously from a single aerial image for Lod2 
     building reconstruction based on Y-shape CNNg. Mahmud et al.(2020)
   - A multi-task, multi-feature learning framework for modelling a 
     building in 3D from a single overhead image.h. Knyaz et al.(2020)
   - CNN is used for automatic semantic segmentation of wire 
     structure and overcoming the limitation of photogrammetric 
     processing applied to the 3D reconstruction of complex grid 
     structuresi. Muftah et al.(2021)
   - CNN is used for classifying and segmenting roofs based on 
     aerial imagery for 3D building reconstruction in LoD2.j. Qian et al(2022)
   - Proposed deep roof definer network and uses satellite imagery 
     to generate roof structure lines using a detail-oriented DL 
     network.
```

## 3.2 甘斯

```
a. Bittner et al.(2018a)
   - Presented an automatic processing methods for better-quality 
     LiDAR-like DSMs with refined 3D building shape extraction from  
     noisy DSMs.b. Bittner et al.(2018b)
   - Use stereo half-meter resolution satellite imagery to create 
     three-dimensional surface models and refine building shapes in 
     LoD2.c. Bittner et al.(2019)
   - Produce good quality DSMs that show a full, accurate level of 
     detail that is similar to LoD2-like building form.d. Kelly et al. (2018)
   - FrankenGAN proposed a network for modelling realistic geometric 
     and texture information on large mass models of coarse 
     buildings with examples of  guidese. Qian et al.(2021)
   - Presented Roof-GAN, a network that generates structured 
     geometry of residential roof structures as a combination of 
     roof primitives.
```

## 3.3 基于 DL 的方法和传统方法的结合

```
a. Zhang and Zhang (2018)
   - Using 3D CNN, a deep Q-network and RNN, 
   - Develop a deep reinforcement learning framework for parsing the   
     semantic large-scale 3D point clouds and reconstruction 3D  
     building models.b. Leotta et al.(2019)
   - End to end system to for reconstructing urban 3D buildings from 
     worldview-3 multiview satellite imagery using DL by segementing 
     buildings and bridges and reconstructing low polygon 3D 
     textural mesh models.c. Xu-et al.(2020)
   - Use satellite imagery-derived point clouds to build an 
     automated DL guided 3D reconstruction framework that 
     distinguishes the of building roods in complex and noisy 
     scenes.d. Yu et al.(2021)
   - Introduce a new fully automatic 3D building reconstruction 
     pipeline based on DL,
   - It can automatically construct building models at LoD1 from 
     Multiview aerial images without any assistance form the other 
     data source.e. Gui and Qin.(2021)
   - DL based approach for reconstruction the LoD2 models using data 
     derived from very-high-resolution multi-view satellite stereo 
     images.f. Kapoor et al.(2019)
   - Proposed four step approach for generating 3D city model from 
     historical images using DL.g. Partovi et al (2019)
   - Propose automatic 3D building model reconstruction workflow.h. Teo (2019)
   - Proposed FCN to detect initial building regions from LiDAR data 
     and automatically reconstruct 3D prismatic building model form 
     3D LiDAR data.i. Kippers et al.(2021)
   - Proposed a new automatic DL based method in LoD1: DSM 
     generation, Deep learning based 2D building footprint 
     generation, and 3D building reconstruction proposed by Yu et 
     al(2020).j. Li et al (2021)
   - Demonstrated a novel method for reconstructing 3D building 
     models with accurate roofs, facades, footprints ,and height 
     from monocular remote sensing images.k. Zhao et al.(2021)
   - Developed a novel 3D reconstruction framework based on an off-
     nadir satellite image. 
   - The approach: Scale-ONet for model reconstruction, Optim-Net  
     for model scale optimization, and model-image for restoring 
     reconstructed scenes.l. Zhang et al.(2021)
   - The holistic primitive fitting method was used along the
     PointNet++ (Qi et al .2017) for 3D building reconstruction from 
     point clouds.m. Chen et al (2021)
   - Using deep implicit fields and point cloud to build three-step 
     3D building reconstruction approach.n. Pepe et al.(2021)
   - Combining DL methods with GIS for reconstruction 3D city models 
     from high resolution satellite imagery.
```

通过实现 DL 方法，可以在不同的数据集上执行不同的任务，并且完全自动化。目前，许多城市已经在开放数据门户中提供了他们的 3D 城市模型。而且计算机硬件和 3D DL 库的进步会导致这个话题变得更加热门。因此，这个研究课题在不久的将来会变得更加有趣。

今天到此为止。我希望你喜欢它，并能够节省一些你的宝贵时间。感谢阅读。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)