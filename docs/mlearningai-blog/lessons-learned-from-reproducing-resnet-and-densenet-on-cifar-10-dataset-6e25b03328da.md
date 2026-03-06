# 在 CIFAR-10 数据集上复制 ResNet 和 DenseNet 的经验教训

> 原文：<https://medium.com/mlearning-ai/lessons-learned-from-reproducing-resnet-and-densenet-on-cifar-10-dataset-6e25b03328da?source=collection_archive---------2----------------------->

# 介绍

深度卷积神经网络(DCNNs)在图像分类中显示了它们的成功前景。最近提出的两个 DCNNs 很快获得了很大的声誉:2015 年提出的残差网络(ResNet)在分类精度方面有着出色的表现，它仍然是 CIFAR-10 数据集上最好的分类器之一，在 56 层的情况下达到了 6.97%的非常小的错误率；此外，密集连接网络(DenseNet)…