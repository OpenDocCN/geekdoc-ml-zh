# 用 Conda 在一台机器上安装多个 Tensorflow 版本

> 原文：<https://medium.com/mlearning-ai/install-multiple-tensorflow-versions-in-one-machine-with-conda-ee4bee19844c?source=collection_archive---------3----------------------->

## 更加专业地管理您的项目。

当然，所有的程序员都是从非常基本的代码行和简单的项目开始学习编程的，我相信我们大多数人过去都忽略或不理解虚拟环境的重要性。由于我们应该每天提高我们的编程技能，我们的项目确实在扩展。然后，我们可以感受到使用虚拟环境进行管理的必要性。您在自己的机器上使用 Tensorflow 1.x 顺利完成项目，但您的老板只是希望您在同一台愚蠢的机器上实施另一个需要 Tensorflow 2.x 支持的项目，这种情况解释了为什么我们需要使用虚拟环境管理我们的项目。为此，Conda 作为开源环境管理系统推出，可以帮助您安装多个版本的软件包及其依赖项，并且可以轻松地在它们之间进行切换。在这篇文章中，我将帮助你使用 Conda 安装多个运行不同版本 CUDA 和 cuDNN 的环境。

![](img/f92c54656689d5abe801c251fdc5e4d9.png)

Photo by [Soubhagya Ranjan Maharana](https://unsplash.com/@soubhagya23?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 安装康达

## 第一步:

我假设您的机器上没有下载 conda bash 脚本，那么登录到您的服务器并导航到/tmp 目录，然后执行:

```
cd /tmp
curl -O [https://repo.anaconda.com/archive/Anaconda3-2019.03-Linux-x86_64.sh](https://repo.anaconda.com/archive/Anaconda3-2019.03-Linux-x86_64.sh)
```

## 第二步:

使用 SHA-256 校验和验证安装程序:

```
$ sha256sum Anaconda3-2019.03-Linux-x86_64.sh
```

输出应该类似于:

```
45c851b7497cc14d5ca060064394569f724b67d9b5f98a926ed49b834a6bb73a  Anaconda3-2019.03-Linux-x86_64.sh
```

## 第三步:

运行 conda bash 脚本:

```
$ bash Anaconda3–2019.03-Linux-x86_64.sh
```

当您收到(1)许可协议条款，(2)默认安装位置的建议(如果需要，您可以指定另一个位置)，以及(3)使用 *conda* 命令的建议时，请按回车键。

我希望您会收到一条消息，通知安装至此已完成。

## 第四步:

激活安装:

```
$ source ~/.bashrc
```

## 第五步:

测试安装:

```
$ conda list
```

# 创建环境和安装包

假设您想要安装 Tensorflow-GPU 1.14、CUDA 10.0 和 cuDNN 7.6.5 的组合(请参考此[兼容性表](https://www.tensorflow.org/install/source#gpu)以了解正确的安装，注意我们不必完全遵循该表，您可以有不同的组合，只要您知道它可以在没有任何暂停的情况下工作)。

## 第一步:

用一个特定的 python 版本创建一个新环境*iamsamed*(在这个例子中假设是 *python3.6* )，然后激活它:

```
$ conda create -n iamhandsome python=3.6
$ conda activate iamhandsome
```

## 第二步:

安装 Tensorflow-GPU 1.14、CUDA 10.0 和 cuDNN 7.6.5:

```
(iamhandsome)$ conda install cudatoolkit=10.0
(iamhandsome)$ conda install cudnn=7.6.5
(iamhandsome)$ pip install tensorflow-gpu==1.14
```

现在让你的项目完成！如果你顺利到达这里，你可能会明白为什么我把这个环境命名为“我帅”。

# 别的东西

1.  获取康达版本

```
$ conda -V
```

2.列出您已经安装的所有环境

```
$ conda info --envs
```

3.移除一个环境*一个*

```
$ conda env remove --name A
```

# 结论

在这篇文章中，我已经向大家介绍了使用 Conda 安装不同版本 CUDA 和 cuDNN 的虚拟环境的步骤。

欢迎读者访问我的脸书粉丝页，这是一个分享关于机器学习的东西的页面:[深入机器学习](https://www.facebook.com/diveintomachinelearning)。

感谢您抽出时间！

## 参考

[1][https://www . digital ocean . com/community/tutorials/how-to-install-anaconda-on-Ubuntu-18-04-quick start](https://www.digitalocean.com/community/tutorials/how-to-install-anaconda-on-ubuntu-18-04-quickstart)

[2][https://stack overflow . com/questions/64811841/tensor flow-1-15-cuda-cud nn-installation-using-conda](https://stackoverflow.com/questions/64811841/tensorflow-1-15-cuda-cudnn-installation-using-conda)