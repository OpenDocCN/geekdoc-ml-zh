# 为什么加载先前保存的 Keras 模型会产生不同的结果:经验教训

> 原文：<https://medium.com/mlearning-ai/why-loading-a-previously-saved-keras-model-gives-different-results-lessons-learned-aeea1014e0ba?source=collection_archive---------1----------------------->

![](img/209ce0eba239096e57084ed4b61440f1.png)

Photo by [Markus Winkler](https://unsplash.com/@markuswinkler?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

现在，机器学习模型在生产中的使用比以往任何时候都多。一个这样的用于创建强大的机器学习和深度学习模型的流行库是 Keras。然而，这些模型的训练过程通常在计算上非常昂贵和冗长，这取决于手头的数据和模型架构。有些模特需要几周到几个月的时间来训练。这使得能够在本地存储我们的模型并在我们需要进行预测时再次检索它们变得非常重要。但是，如果出于某种原因，保存的模型没有正确加载，我们该怎么办呢？我会试着根据我的经历给出答案。

我不会详细介绍如何使用和保存 Keras 模型，我只是假设读者熟悉这个过程，并直接跳到如何处理加载时的意外模型行为。也就是说，在训练了一个存储在`model` 变量中的 Keras 模型后，我们喜欢按原样保存它，以便下次加载时我们可以跳过训练，只进行预测。我的首选方法是保存模型的权重，这些权重在模型创建之初是随机的，并随着模型的训练而更新。于是我打了`model.save_weights(“model.h5”)`。创建了一个“model.h5”文件，其中包含了我们的模型学习到的权重。接下来，在另一个会话中，我用和以前一样的架构重新创建了一个模型，并用`new_model.load_weights(“model.h5”)`加载我保存的训练权重。一切似乎都很好。除了，当我点击`new_model.predict(test_data)`时，我得到的精度为零。却不知道为什么。

事实证明，你的模型不能做出正确预测的原因有很多。在这里，我将尝试总结最常见的错误，并给你提示如何解决它们。

## 1.首先，仔细检查你的数据。

我知道这似乎是显而易见的，但是当从磁盘重新加载你的模型时，微小的疏忽会导致糟糕的性能。例如，如果您正在构建语言模型，请确保在每个新会话中:

*   重新检查你的班级标签的顺序。如果您将它们映射到数字，请再次检查每个会话中每个类标签是否都获得了相同的数字。如果你用一个`list(set())` 函数来检索它们，每次都会以不同的顺序返回你的标签，这可能会发生。这最终可能会打乱你的标签预测。
*   检查你的数据集。如果您在另一个文件中没有您的测试数据，请检查您的训练测试分割是否是随机的，这样每次您进行预测时，您都会预测不同的数据，因此您的预测准确性最终会不一致。

当然，根据您工作的领域，您可能会遇到其他与数据相关的问题。但是，请始终检查数据表示的一致性。

## 2.指标问题

错误或不一致结果的另一个原因是准确性度量的选择。通常，当建立一个模型并保存它的权重时，我们会做这样的事情:

```
def build_model(max_len, n_tags): 
   input_layer = Input(shape=(max_len, ))
    output_layer = Dense(n_tags, activation = 'softmax')(input_layer)
    model = Model(input_layer, output_layer)

    return modelmodel = build_model()
model.compile(optimizer="adam", loss="sparse_categorical_crossentropy", metrics=["accuracy"])model.fit(..)
model.save_weights("model.h5")
```

如果我们需要在新的会话/脚本中打开它，我们将执行以下操作:

```
def build_model(max_len, n_tags): 
   input_layer = Input(shape=(max_len, ))
    output_layer = Dense(n_tags, activation = 'softmax')(input_layer)
    model = Model(input_layer, output_layer)

    return modelmodel = build_model()
model.compile(optimizer="adam", loss="sparse_categorical_crossentropy", metrics=["accuracy"])model.load_weights("model.h5")
model.evaluate()
```

根据您使用的特定 Keras/Tensorflow 版本，这可能会引发错误。当编译模型并选择“准确性”作为度量标准时，问题就出现了。Keras 对准确性有不同的定义:“稀疏分类准确性”、“分类准确性”等等，取决于您使用的数据，不同的定义是最佳解决方案。这是因为，如果我们将指标设置为“准确性”，Keras 将根据它认为最适合数据分布的类型，尝试分配一个特定的准确性类型。它可以在不同的运行中推断出不同的准确性度量。这里最好的解决方法是总是显式地设置精度指标，而不是让 Keras 自己选择。例如，替换

```
model.compile(optimizer="adam", loss="sparse_categorical_crossentropy", metrics=["accuracy"])
```

随着

```
model.compile(optimizer="adam", loss="sparse_categorical_crossentropy", metrics=["sparse_categorical_accuracy"])
```

## 3.随机性

当像以前一样对相同的数据重新训练一个 Keras 神经网络时，你很少会得到两次相同的结果。这是由于 Keras 中的神经网络在初始化它们的权重时使用随机性，所以每次运行时权重被不同地初始化，因此在学习过程中这些权重将被不同地更新，所以在进行预测时不可能得到相同的准确性结果。

如果出于某种原因，您需要在训练之前使权重相等，您可以在代码之前设置随机数生成器:

```
from numpy.random import seed
seed(42)
from tensorflow import set_random_seed
set_random_seed(42)
```

对于 Keras，numpy 随机种子是 sat，而对于 Tensorflow 后端，我们需要将它自己的随机数生成器设置为相等的种子。这段代码将确保您每次运行代码时，您的神经网络权重将被相等地初始化。

## 4.注意自定义图层的使用

Keras 提供了各种各样的[层](https://keras.io/api/layers/)(密集层、LSTM 层、漏失层、批处理规格化层等等)，但是有时我们想要对模型中的数据应用一些特定的操作，并且没有为它定义的特定层。一般来说，Keras 提供了两种类型的层: [Lambda](https://keras.io/api/layers/core_layers/lambda/) 和基础层类。对这两者要非常小心，尤其是当你将你的模型架构保存为 json 格式的时候。Lambda 层的棘手之处在于它的序列化限制。由于它是通过 Python 字节码的序列化保存的，所以只能在保存它的环境中加载，也就是说，它是不可移植的。当面对这个问题时，通常建议覆盖一个`[keras.layers.Layer](https://keras.io/api/layers/base_layer/#trainable_weights-property)`层，或者保存整个模型，只保存其权重并从头开始重建模型。

## 5.自定义对象

通常，您会希望使用应用于数据的自定义函数，或者计算损失/准确性等的函数。Keras 允许我们在保存/加载模型时指定额外的参数。假设我们想用自己创建的特殊损失函数加载之前保存的模型:

```
model = load_model("model.h5", custom_objects={"custom_loss":custom_loss})
```

如果我们在一个新的环境中加载这个模型，我们必须小心地在那里定义我们的`custom_loss`函数，因为默认情况下，当保存模型时，这些函数不会被记住。即使我们保存我们模型的整个架构，它也会保存我们自定义函数的名称，但是函数体是我们必须另外提供的东西。

## 6.全局变量初始化器

如果您使用 Tensorflow 1.x 作为后端，这一点尤其重要，因为许多应用程序可能仍然需要它。运行 tf 1.x 会话时，需要运行`tf.global_variables_initializer()`，它会随机初始化所有变量。这样做的副作用是，当您尝试保存模型时，它可能会重新初始化所有权重。您可以通过运行以下命令来手动停止此行为:

```
from keras.backend import manual_variable_initialization manual_variable_initialization(True)
```

# 摘要

本文简要总结了(至少在我的经验中)最常导致 Keras 模型无法在新环境中正确加载的因素。有时这些问题会导致不可预测的结果，而在其他情况下，它们只是抛出一个错误。它们发生的时间和方式也在很大程度上取决于您正在使用的 Python 版本，以及您的 Tensorflow 和 Keras 版本，因为其中一些版本不兼容，并且以一种导致意外行为的方式进行交互。我希望这个简短的概述能让你知道，如果面临这样的问题，应该从哪里着手。干杯！

# 来源

1.  [https://keras . io/getting _ started/FAQ/# how-can-I-obtain-reproducible-results-using-keras-in-development](https://keras.io/getting_started/faq/#how-can-i-obtain-reproducible-results-using-keras-during-development)
2.  https://keras.io/api/layers/core_layers/lambda/
3.  【https://www.tensorflow.org/guide/keras/save_and_serialize 
4.  [https://keras . io/guides/making _ new _ layers _ and _ models _ via _ subclass ing/](https://keras.io/guides/making_new_layers_and_models_via_subclassing/)
5.  [https://machine learning mastery . com/reproducible-results-neural-networks-keras/](https://machinelearningmastery.com/reproducible-results-neural-networks-keras/)

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)