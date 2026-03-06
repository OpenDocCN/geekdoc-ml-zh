# 如何使用 Wasserstein GAN 改善图像生成？

> 原文：<https://medium.com/mlearning-ai/how-to-improve-image-generation-using-wasserstein-gan-1297f449ca75?source=collection_archive---------1----------------------->

## 通过使用 PyTorch 实现 Wasserstein GAN(WGAN ),并使用权重裁剪和梯度惩罚来改进 MNIST 图像生成。

你会学到什么？

*   DCGAN 面临的挑战
*   Wasserstein GAN 如何用 DCGAN 解决挑战？
*   推土机的距离是多少
*   经由权重裁剪和梯度惩罚的 1-Lipshitz 约束