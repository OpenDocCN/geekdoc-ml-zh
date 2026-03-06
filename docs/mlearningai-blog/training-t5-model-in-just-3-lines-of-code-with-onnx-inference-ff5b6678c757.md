# 使用 ONNX 推理，仅用 3 行代码训练 T5 模型

> 原文：<https://medium.com/mlearning-ai/training-t5-model-in-just-3-lines-of-code-with-onnx-inference-ff5b6678c757?source=collection_archive---------3----------------------->

## 使用“simplet 5”python 包推理和微调 T5 模型，然后使用 ONNX 进行快速推理

![](img/47dc18dec624b1f70f1f85bd63734201.png)

Image from [Source](https://github.com/Shivanandroy/simpleT5/blob/main/data/simplet5.png)

# 背景

[simpleT5](https://github.com/Shivanandroy/simpleT5) 是一个基于 [PyTorch-lightning](https://www.pytorchlightning.ai/) 和[拥抱脸变形金刚](https://huggingface.co/transformers/)的 python 包，可以让你快速*(仅 3 行代码)* …