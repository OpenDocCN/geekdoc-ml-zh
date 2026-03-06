# Python 中的迭代器和生成器

> 原文：<https://medium.com/mlearning-ai/iterators-and-generators-in-python-8d392382f158?source=collection_archive---------7----------------------->

> python 中的生成器和迭代器是什么，如何高效地使用它们。

![](img/90d0f234b2a8696559df528964f904b3.png)

Photo by [Luca Bravo](https://unsplash.com/@lucabravo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 让我们先从迭代器开始。

你有没有想过如何在列表中循环“列表中的元素”？迭代器是这一功能的基础。要理解迭代器和生成器，首先要理解什么是迭代器和可迭代的。

**迭代器**是一个通过一系列值(iterable)管理迭代的对象。对于迭代器对象' I '，每次调用内置函数 next(i)都会从底层序列中产生一个后续元素。如果一个序列中没有其他元素，则会引发 StopIteration 异常，指示已到达该序列的末尾。我们可以通过 *iter(obj)创建一个迭代器。*

**iterable** 是迭代器迭代的对象，比如 list 和 tuple。现在让我们看看语法。

```
data = [1,2,3]
i = iter(data)
print(next(i))
print(next(i))
print(next(i))
```

python 中的 for-loop 语法“for i in data”只是在幕后实现了上述过程。它为给定的 iterable 创建一个 iterator 对象，然后不断调用 next(iterator ),直到捕捉到 StopIteration 异常。

有些人可能会想，如果我在所有后续调用执行之前在列表中添加一个元素，会发生什么？它会简单地将该元素添加到列表中，该列表可以通过最后一次调用来检索。

```
data = [1,2,3,4,5]
i = iter(data)
print(next(i))
print(next(i))data.append('hi')print(next(i))
print(next(i))
print(next(i))
print(next(i))#output
1
2
3
4
5
hi
```

现在让我们在一个类中实现迭代器。

您可能想知道为什么 __next__ 和 __iter__ 方法前后有双下划线。这是因为它们都是 python 中 **Dunder** 函数的一部分，也称为神奇函数。这些 Dunder 函数是内置 Python 类的预定义方法，但是可以被覆盖以实现良好的多态形式。我将会写另一篇关于这个主题的文章，但是现在，让我们只关注迭代器。在上面的代码 **__iter__** 中，方法返回 self，这个 self 就是我们调用 __iter_ 的同一个类对象。我们通过 __iter__ 返回 self，因为我们覆盖了 iter dunder 函数，这样我们就可以为我们的类创建一个迭代器，并指定它的行为方式。每次调用 __next__ 方法都会返回后面的偶数。for 循环“for num in evennum(10):”的底层机制现在被覆盖，因为我们已经覆盖了我们的类的 __iter__ 和 __next__ 方法，所以 for 循环的每次迭代都将返回下一个偶数，并在捕获到 __next__ 方法引发的 StopIteration 异常时立即停止。

## 现在让我们看看发电机。

**生成器**被认为是在 python 中创建迭代器最方便的技术。生成器的语法类似于传统函数的语法，但不是使用 **return** 语句，而是使用 **yield** 语句来指示序列的每个元素。考虑下面的例子。

```
def factors_return(n):
  results = []
  for k in range(1,n+1):
      if n%k == 0:
          results.append(k)
  return results def factors_yield(n):
  for k in range(1,n+1):
    if n%k == 0:
      yield k
```

在上面的代码中，可以看到关键字 yield 的使用。Python 用这个来区分普通函数和生成器。函数 factors_return(n)将返回包含所有因子的列表，而 factors_yield(n)将生成一系列值，yield 关键字用于迭代这些值。如果我们为 factors(100)中的 **因子编写**:**，就会创建一个生成器实例，并且对于循环的每次迭代，python 都会执行 factors_yield()中的过程，直到 yield 语句指示下一个值。此时，该过程被暂时挂起，只有在请求另一个值时才会恢复。当整个控制流到达过程的自然结尾时，会自动引发 StopIteration 异常。换句话说，yield 语句中断过程的执行，并将一个值发送回调用者，但保持足够的状态，使过程能够从停止的地方恢复，并且当 for 循环请求下一个值时，过程从上一次 yield 运行后立即恢复其功能。与此相反，factors_return(n)将返回一个包含所有因子的列表，而不是生成一个序列，因此，如果传递一个很大的数，如 100000000000000000，就会占用很大的内存。**

**为什么要使用迭代器和生成器？**

使用迭代器和生成器的好处是它们使用传统函数不使用的**惰性求值、**。只有在请求时才计算结果，整个系列不需要立即驻留在内存中，从而避免了与内存相关的问题。生成器可以有效地生成无限系列的值。考虑下面的一个例子。

```
# fibonacci using generator
def fibonacci_generator(n):
  a = 0
  b = 1
  for i in range(n):
    yield a
    future = a+b
    a = b
    b = futurefor i in fibonacci_generator(10000):
    print(i) # fibonacci using list
def fibonacci_list(n):
  a=0
  b=1
  lis = []
  count = 0
  while count<n:
     lis.append(a)
     future = a+b
     a=b
     b=future
     count+=1
  return lisprint(fibonacci_list(10000))
```

在上面的代码中，函数 fibonacci_list 将创建一个列表对象，并将所有 fibonacci 元素存储在该列表中。随着新元素被推入列表，这个列表将不断增加，导致它占据越来越多的空间。因此，在某一点之后会导致内存问题。与此相反，函数 fibonacci_generator 将产生一系列 fibonacci 数列，Fibonacci 元素不会存储在列表中，而是在 for 循环的每次迭代后返回每个 Fibonacci 元素，因此不会占用内存。这样，我们就可以使用生成器生成无限的斐波那契数列。

在为机器学习和深度学习问题加载数据时，生成器也被广泛使用。这是因为，例如，包含数千幅图像的整个数据集无法加载到内存中。因此，发电机被用来救援。考虑这个来自使用深度学习项目的图像字幕的小代码片段。

```
def data_generator(descriptions, features, tokenizer, max_length):
   while 1:
    for key, description_list in descriptions.items():
        feature = features[key][0]
        input_image, input_sequence, output_word
        create_sequences(tokenizer, max_length, description_list,   feature)
        yield ([input_image, input_sequence], output_word)
```

## 摘要

在本文中，我们介绍了迭代器和生成器的基础知识，以及如何实现它们。我们还讨论了为什么我们应该使用这些迭代器和生成器。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)