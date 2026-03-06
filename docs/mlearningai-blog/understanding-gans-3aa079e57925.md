# 了解 GANs

> 原文：<https://medium.com/mlearning-ai/understanding-gans-3aa079e57925?source=collection_archive---------4----------------------->

萨提亚·克里希南·苏雷什

随着围绕 DALL-E、稳定扩散和深度伪造的所有讨论，探索或了解由 Ian Goodfellow 早在 2014 年在他的论文*中开发的生成对抗网络*是很重要的。甘斯奠定了发展图像的基础，这些图像是创造性的，美丽的，不同于我们以前看到或想到的任何东西。GANs 背后的想法很简单——两个深度学习模型相互竞争——但这个想法变得如此强大，以至于今天我们正在研究可以从一些随机文本中产生不真实图像的高级模型。在这种情况下，让我们试着了解一下 GANs 的基本知识。

**生成敌对网络:**

如前所述，GANs 背后的想法是让两个模型相互竞争，这样一个模型使另一个更好，反之亦然。GAN 模型通常由发电机模型和鉴别器模型组成。

*生成器模型—* 该模型用于从随机噪声或高斯分布中生成预定义大小的图像。生成器模型的架构类似于自动编码器模型中解码器的架构。

![](img/517673cf2079a09b1c2fd7896cee7681.png)

*鉴别器模型—* 该模型用于对真实图像和生成器模型生成的图像进行分类，基本上是一个分类模型。

![](img/8661eb1136044d7e6fc8e649f20c764f.png)

现在我们知道了这两个模型是什么，让我们试着理解这两个模型如何一起工作来产生真实的图像。让我们假设我们正在处理时尚 mnist 数据集。

最初，一些随机噪声(shape — [batch_size，coding_size])被馈送到生成器模型。生成器模型穿过噪声并产生形状阵列[28，28](时尚 mnist)。然后，由生成器模型生成的图像与来自 mnist 数据集的一批真实图像连接，并被馈送到鉴别器模型。鉴别器模型试图将图像分为真的和假的——假的是生成器模型生成的图像。如果鉴别器成功地对大多数图像进行了分类，则意味着生成器无法生成逼真的图像来迷惑鉴别器。因此，生成器和鉴别器被反复训练，直到生成器能够欺骗鉴别器，使其相信它生成的图像是真实的。这是 GANs 背后的基本思想。现在让我们看看如何使用 tensorflow 实现一个简单的 GAN。

发生器和鉴别器模型是简单的顺序模型。发电机模型接受随机噪声，这可以从架构的第一层看出。然后，我们有一个密集层，它试图将噪声矩阵转换成具有图像某些特征的矩阵。倒数第二个密集层有 784 个单元，这里使用的激活函数是“sigmoid ”,因为该模型试图预测每个像素点亮多少的概率。

鉴别器模型接收形状为[batch_size，28，28]的图像，并将其展平为形状为[batch_size，784]的数据集。然后，该模型试图从输入的图像中捕捉各种特征，使用两个密集层找出真实和伪造的图像。模型的最后一层只有一个单元，它输出图像是真实的(概率> =0.5)还是伪造的(概率<0.5).

![](img/185a2ce60206aa1a7a50e2550f0e3cd7.png)

Now comes the most important part. When the GAN model is compiled, the discriminator model should be made untrainable. Since the work of the GAN model is to produce realistic images, only the generator model has to be trained while the discriminator model will just be predicting 1 or 0\. The gradients obtained from the predictions will be used by the GAN model to train the generator. Again, a simple concept but powerful. The code given below shows how to train a GAN.

![](img/f84fa41a842aaaefb47aac52526a6cf1.png)

After training the model for 50 epochs, the generator model is able to generate the following images and most of the images look realistic.

![](img/da7ed1ebbbee4931d084fa24052d7972.png)

This is as far as you can go with a simple sequential model because of training difficulties of GANs which we will looking at in my next article. But for now, to produce more realistic fashion mnist images let’s try to implement a DCGAN (Deep Convolutional GAN).

**DCGAN:**

顾名思义，DCGAN 在生成器和鉴别器模型中都使用卷积层，而不是简单的密集层。在生成器模型中，Conv2DTranspose 层用于生成图像，而在鉴别器中，Conv2D 层用于执行分类。代码如下。训练一只 DCGAN 比训练一只普通的 GAN 要困难得多。但是亚历克·拉德福德所做的研究提供了一些在尝试实现 DCGAN 时非常有效的指导原则。一些准则是:使用步长卷积层代替最大池层，在生成器模型中的每个卷积层之后使用批量归一化层，在生成器模型的最后一层使用 tanh 激活函数，等等。

![](img/3e748291c5b95b5d99284f19f33df5a4.png)

在 50 个时期之后，由 DCGAN 使用与上述相同的训练函数产生的图像如下所示。如你所见，图像比普通 GAN 产生的图像好得多。

![](img/26cc8f641ef73bca26dbcc81871fddd9.png)

**结论:** 在这篇文章中，我已经介绍了 GAN 的基础知识，但最重要的概念是当你训练 GAN 时所面临的困难，这将在下一篇文章中讨论。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)