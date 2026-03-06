# 如何让我的 for 循环更快？Python 中的多重处理和多线程

> 原文：<https://medium.com/mlearning-ai/how-do-i-make-my-for-loop-faster-multiprocessing-multithreading-in-python-8f7c3de36801?source=collection_archive---------0----------------------->

我们多久必须对一系列对象运行一次计算密集型操作？或者从像 S3 这样的存储空间中读取文件列表？

上述两个问题的共同点是什么？漫长的等待时间！

有什么不同？一个是 CPU 绑定的进程，另一个是 I/O(输入输出)绑定的进程。

是啊，我知道！那是沉重的。让我们来一点一点地探讨这个问题。

什么是 CPU？

CPU 是中央处理器。就是这样！

开个玩笑，让我解释一下。CPU 的生产率是根据周期来衡量的(但它也取决于许多其他因素，包括架构)——执行一个简单的处理器操作所需的时间。所以，举例来说，如果你检查你系统上的任务管理器，有多个进程在同一个 CPU 上工作，例如，Google Chrome，Slack，VSCode 等。他们通过分享每个周期的运行时间来实现这一目标。一个软件可能运行了 10 个周期，而另一个运行了接下来的 5 个周期，第三个运行了接下来的 8 个周期，并且它重复。所有这些看起来都像是在等待，对不运行的软件什么都不做，但是你猜怎么着？人眼根本看不到它。

什么是多处理？

利用多个处理器来运行一个任务。这通常是 CPU 受限操作的完美用例。在我们的例子中，我们将创建进程，并从列表中给每个进程一个我们想要处理的对象。这将导致并行处理和更快的响应，但同样数量的 CPU 时间。简单地说，如果我们在列表上有 2 个项目，并且它提前了 20 秒，由于多处理，它将花费 10 秒(大约)，但是相同数量的 CPU 周期在两个进程之间分配。

注意:处理器不共享内存，所以在两个进程之间共享一个全局变量比较困难，但是这个问题的解决方案超出了本文的范围。

什么是多线程？

python 环境中的多线程更类似于将工作分配给同一处理器中的不同工作人员，但他们不能同时工作。那么，优势是什么呢？如果这是一个 I/O 绑定的功能，当第一个工作线程等待从存储中获取文件时，我们将尝试多线程化，第二个工作线程可以开始其工作，并在第一个工作线程完成等待后交还控制权。

# 让我们来测试一下这些概念！

我们将使用 Python 模块— **concurrent.futures**

它有**threadpoolexcutor**和 **ProcessPoolExecutor** 类，它们有相同的接口，是 *Executor* 类的子类。

代码示例在我的本地系统上运行，配置如下。

![](img/c80e687d5462e5815cd958bd7be4cd67.png)

System config: The highlighted text shows the system has 6 cores that can handle two threads each, hence 12 logical processors. Simply it means that each core (working unit) can run two threads but it does not happen simultaneously. The threads are just scheduled to share the cycles of the core.

下面给出了两个代码示例，它们具有相同的计算密集型功能，由应用于图像的多个变换组成。是的，我想不出任何其他的用例，所以决定在没有明显原因的情况下对图像做一些事情。无论如何，正常执行的代码样本运行时间是 456.551 秒，而多重处理的代码样本运行时间是 236.995 秒。在这里，ProcessPoolExecutor 用于 61 个并行工作者的多处理。还有许多其他参数可用于定制流程，文档可在[此处](https://docs.python.org/3/library/concurrent.futures.html)找到。

The compute intensive function is run on a list of images with normal execution

The compute intensive function is run on a list of images using multiprocessing

多线程以类似的方式实现，但是需要它的原因完全不同——I/O 绑定过程，如前所述。下面给出了一个多线程实现的例子，其中我们试图从本地系统的一个文件夹中读取图像。使用多线程的任务的总时间是 0.3212 秒，而没有多线程的任务的总时间是 0.3732 秒，这对于一个非常小的列表来说是一个显著的差异。这里，我们还使用了部分函数，这在我们想要传递一个预先填充了多个参数的函数时非常有用。

*concurrent.futures* 的另一个有用特性是 *Executor.submit()* 创建的未来对象。未来对象非常类似于 javascript 中的承诺。该模块中的一个函数是 *future.as_completed* ，用于获取 **completed 结果**，因此收到的响应可能与提交的输入顺序不同。

这只是一个非常简单的概述，介绍了引擎下正在发生的事情，以及我们如何轻松地让我们的代码运行得更快。虽然这个主题还有很多内容，但我希望这能帮助你开始。这里有一篇非常好的文章，介绍了 [ThreadPoolExecutor](https://superfastpython.com/threadpoolexecutor-in-python/) 的来龙去脉。

*您好，非常感谢您花时间阅读本文。我是一名机器学习工程师，目前正在探索 MLOps 世界以及其他行业。我用媒介记下我对最近感兴趣的话题的想法。你可以在*[*LinkedIn*](https://www.linkedin.com/in/apurva-misra/)*上和我联系，我随时准备和你聊一聊:)*

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)