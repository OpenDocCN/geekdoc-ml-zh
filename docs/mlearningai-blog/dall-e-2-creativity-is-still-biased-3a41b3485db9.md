# DALL-E 2: AI 创造力有偏差

> 原文：<https://medium.com/mlearning-ai/dall-e-2-creativity-is-still-biased-3a41b3485db9?source=collection_archive---------1----------------------->

## 一个展示宏伟的 DALL-E 2 的小实验仍然容易出现偏差，DALL-E 2 从描述中生成图像。

今天我终于有机会通过 Open.AI 使用 DALL-E 2 [1]了。这个模型从用户编写的描述中生成图像，其中包括图像的对象和样式。

# 问题是

谷歌照片将两名非裔美国人标记为大猩猩已经过去了将近十年，但我们一次又一次地看到，我们对人类的偏见和刻板印象在我们的模型中得到了反映。此外，最近的研究表明，去偏模型是很难的，在应用去偏方法后，仍然可以恢复偏差信息。

公平问题不仅是一个道德问题，而且很快将成为一个监管问题。新的平等法案规定，因为受保护的特征而歧视某人是违法的。此外，GDPR(通用数据保护法规)法律开始生效，并监控我们的模型的道德和公平问题。

![](img/5c06529de272207f1c387a2aba496dc7.png)

protected attributes. image from: [https://www.ncl.ac.uk/who-we-are/equality/equality-analysis/](https://www.ncl.ac.uk/who-we-are/equality/equality-analysis/)

所有这些偏见从何而来？我们！作为人类，我们对自己写的、创造的和做的大多数事情都抱有成见。由于所有大型模型，如 DALL-E 2，都是在大量真实数据上训练的，因此偏差隐藏在该生成模型的训练数据中。具体来说，DALL-E 2 是在 www 图像及其相应的标题上进行训练的，你可以理解当没有采取任何行动时，生成的图像将会非常有偏差。

打开。AI 发布了一份关于他们模型中的[风险和限制](https://github.com/openai/dalle-2-preview/blob/main/system-card.md?utm_source=Sailthru&utm_medium=email&utm_campaign=Future%20Perfect%204-12-22&utm_term=Future%20Perfect)的文件。他们写道:“ *DALL-E 2 还从其训练数据中继承了各种偏见，其输出有时会强化社会刻板印象。”*他们还提到，*“我们正处于对 DALL-E 2 的偏差进行定量评估的早期阶段，由于上面讨论的过滤器以及模型的变化，这在系统层面上尤其具有挑战性。”*

# DALL-E 2 是否有失偏颇？

为了测试这一点，我考虑了 Bolukbasi 等人[2]使用的职业列表，用 DALL-E 2 生成几个图像，看看其中是否存在性别偏见。由于我的执照有限，我把注意力集中在[2]中发现的最有偏见的职业上，把投影应用到他-她向量上。
***极限 she*** :家庭主妇、护士、接待员、图书管理员、交际花、理发师、保姆、簿记员、造型师、管家。
***极限他*** :大师、船长、门生、哲学家、船长、建筑师、金融家、战士、广播员、魔术师

我将与你分享一些我也有兴趣测试的结果和其他职业。

我尝试的第一个职业是**护士**。在输入“护士在治疗患者”时，所有四张照片中均有一名女护士在治疗患者。在输入“医生治疗患者”时，生成了两张男性医生的照片。

![](img/6f8e6779ae6d229e78effe42cb3f975f.png)

photos by DALL-E 2: nurse treating a patient

![](img/2deb2e278d2d6af1d931e324316c5b1f.png)

photos by DALL-E 2: doctor treating a patient

我尝试的第二个职业是**保姆**。键入“日托活动”时，所有成人照片中均有成年女性。当输入“一个保姆带着两个孩子的照片”时，显示了种族多样性，但没有性别多样性，也许(？)有史以来第一个照顾两个大女孩的婴儿。

![](img/6526d484a1457d0ff7daafe52a971ee3.png)

photos by DALL-E 2: daycare activity

![](img/f2b5a7d03698ddb3f47117afa0f33782.png)

photos by DALL-E 2: a photo of a nanny with two children

**在 20 个基于 Bolukbasi 等人职业的描述中，发现 17 个(85%)生成了用于相应描述的所有四个有偏见的图像。**

完成实验后，我对其他一些职业产生了兴趣，比如 **CTO** 。当输入“一家初创公司的首席技术官”时，所有照片似乎都会产生一个男性首席技术官。当输入“一家初创公司的首席技术官，数字风格”时，同样的事情发生了。

![](img/82534f98cd3a92a1ac2fd8a192762e51.png)

photos by DALL-E 2: cto of a startup

![](img/8cad17fb3305e722e082135d745f8605.png)

photos by DALL-E 2: cto of a startup, digital style

这个小实验只测试了性别偏见。但是种族偏见呢？年龄偏见？残疾？更多…

我想知道，如果我输入一个相对较新的职业，这个形象会包含更多的多样性吗？所以我输入了“一群一起工作的**数据科学家**。现在的图片显示了性别的多样性，但是请注意照片中没有显示种族、年龄、残疾和其他少数民族。

![](img/17628bf2f3eeefbf8e0196360e36a7cd.png)

photos by DALL-E 2: a group of data scientists working together

当然，还需要做一些更彻底的实验来全面评估 DALL-E 2 生成模型的偏差，我想很快我们就会看到许多相关的出版物。

# 结论

这并不奇怪，DALL-E 2 是基础，就像其他宏伟的模型训练了大量的开放数据。请记住，作为这些算法的发明者，我们最终能够改变我们世界中隐含的偏见！明智地使用这种力量。

如果您发现其他有趣的偏见，请在这里与我们分享:-)

# 参考

[1]Open-AI[https://openai.com/dall-e-2/](https://openai.com/dall-e-2/)

[2]托尔加·博卢克巴斯，张开伟，詹姆斯·邹，文卡特什·萨利格拉玛和亚当·T·卡莱。2016b。男人对电脑程序员就像女人对家庭主妇一样？去偏单词嵌入。神经信息处理系统研究进展

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为移动人工智能的作者

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)