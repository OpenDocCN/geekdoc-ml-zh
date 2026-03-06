# 使用 Transformers trainer 和 pipeline 进行 NLP 的迁移学习

> 原文：<https://medium.com/mlearning-ai/transfer-learning-with-transformers-trainer-and-pipeline-for-nlp-8b1d2c1a8c3d?source=collection_archive---------2----------------------->

深度学习如今有很多有趣的应用，[智能冰箱](https://www.businessinsider.com/samsung-lg-unveil-artificial-intelligence-equipped-smart-fridges-2020-1)，[智能电梯](https://digital.hbs.edu/platform-digit/submission/thyssenkrup-steelmaker-transforms-elevators-using-ai/)，[深酿](https://towardsdatascience.com/deep-brew-transforming-starbucks-into-an-ai-data-driven-company-8eb2e370af7b)，[自动驾驶](https://en.wikipedia.org/wiki/Self-driving_car)，仅举几例。

[](https://machinelearningmastery.com/applications-of-deep-learning-for-natural-language-processing/) [## 7 深度学习在自然语言处理中的应用-机器学习掌握

### 自然语言处理领域正从统计方法转向神经网络方法。有…

machinelearningmastery.com](https://machinelearningmastery.com/applications-of-deep-learning-for-natural-language-processing/) 

然而，通常你需要大量数据来训练 [SOTA](https://pub.towardsai.net/state-of-the-art-models-in-every-machine-learning-field-2021-c7cf074da8b2) (最先进的)深度学习模型。也许你需要计算能力，但是到目前为止，你需要大量高质量的数据。如果没有那么多数据，你能做什么？进入[迁移学习](https://towardsdatascience.com/a-comprehensive-hands-on-guide-to-transfer-learning-with-real-world-applications-in-deep-learning-212bf3b2f27a)的世界。

在高层次上，迁移学习是重用通过解决其他问题获得的“知识”(这些知识是在大量数据上训练的，并达到 SOTA 水平)，以有效地解决一个不同但相关的问题，而不需要大量的标记数据。

[](https://machinelearningmastery.com/transfer-learning-for-deep-learning/) [## 深度学习迁移学习的温和介绍——机器学习掌握

### 迁移学习是一种机器学习方法，在这种方法中，为一项任务开发的模型被重新用作一项任务的起点

machinelearningmastery.com](https://machinelearningmastery.com/transfer-learning-for-deep-learning/) [](https://www.seldon.io/transfer-learning/) [## 机器学习的迁移学习

### 机器学习的迁移学习是指预训练模型的元素在新的机器学习中被重用…

www.seldon.io](https://www.seldon.io/transfer-learning/) [](https://research.aimultiple.com/transfer-learning/) [## 2022 年的迁移学习:它是什么&如何工作

### 训练机器学习模型可能是一项具有挑战性的数据科学任务。训练算法可能不会像…

research.aimultiple.com](https://research.aimultiple.com/transfer-learning/) 

在一系列文章中，我想为开发人员提供在自然语言处理、计算机视觉中使用迁移学习的快速入门。

对于自然语言处理， [transformers 架构](https://towardsdatascience.com/transformers-89034557de14)是解决不同问题的首选模型，例如文本分类、机器翻译、语言建模、文本生成、问题回答等。多亏了[拥抱脸](https://huggingface.co)和它的生态系统，[用变形金刚](https://towardsdatascience.com/how-to-use-transformer-based-nlp-models-a42adbc292e5)转移学习已经变得非常容易开始了。

让我们来关注一下变形金刚的迁移学习，主要是如何从变形金刚库中微调一个预训练的模型。变形金刚库提供了[训练器](https://huggingface.co/docs/transformers/training)和[流水线](https://huggingface.co/docs/transformers/quicktour)，让训练和预测变得真正容易。

# 文本分类

## 加载数据集

```
from datasets import load_datasetraw_datasets = load_dataset("imdb")
```

## 加载标记器并标记数据

目的是将文本标记为模型稍后可读的格式。重要的是要加载与训练模型时相同的分词器，以确保正确地对单词进行分词。

```
from transformers import AutoTokenizertokenizer = AutoTokenizer.from_pretrained("bert-base-cased")def tokenize_function(examples):
    return tokenizer(examples["text"], padding="max_length", truncation=True)

tokenized_datasets = raw_datasets.map(tokenize_function, batched=True)
```

理解数据的形状对我们来说也很重要。如果你看看 tokenized_datasets 内部，它是

```
DatasetDict({
    train: Dataset({
        features: ['text', 'label', 'input_ids', 'token_type_ids', 'attention_mask'],
        num_rows: 25000
    })
```

文本和标签在原始原始数据集中。要做分类，你需要“标签”来告诉地面真相。“input _ ids”是记号索引，记号的数字表示构成将被模型用作输入的序列。“attention_mask”指示令牌索引的位置，使得模型不关注它们(通常 1 指示应该关注的值)。“token_type_ids”被一些模型用来识别序列的类型(更多信息请参考 [Token Type IDs](https://huggingface.co/docs/transformers/glossary#token-type-ids) )。了解更多关于[预处理](https://huggingface.co/docs/transformers/preprocessing)的信息。

## 分割训练，测试数据集

raw_datasets 对象是一个字典，有三个键:“train”、“test”和“unsupervised”(对应于数据集的三个拆分)。我们将使用“训练”分割进行训练，使用“测试”分割进行验证。

```
small_train_dataset = tokenized_datasets["train"].shuffle(seed=42).select(range(1000))
small_eval_dataset = tokenized_datasets["test"].shuffle(seed=42).select(range(1000))
full_train_dataset = tokenized_datasets["train"]
full_eval_dataset = tokenized_datasets["test"]
```

## 定义预训练模式

请注意，模型名称与 tokenizer 相同

```
from transformers import AutoModelForSequenceClassificationmodel = AutoModelForSequenceClassification.from_pretrained("bert-base-cased")
```

## 运动鞋

现在最精彩的部分来了。定义您的训练参数，使用您的模型、训练参数、训练数据集和评估数据集创建训练器

```
from transformers import TrainingArgumentstraining_args = TrainingArguments("test_trainer")from transformers import Trainertrainer = Trainer(model=model, args=training_args, train_dataset=small_train_dataset, eval_dataset=small_eval_dataset)
```

## 开始训练

```
trainer.train()
```

这个 API 非常简单(当然你可以做很多定制，比如训练时间，学习速度)。否则，在本机 Pytorch 中将会出现如下内容:

```
from transformers import AdamWoptimizer = AdamW(model.parameters(), lr=5e-5)from transformers import get_schedulernum_epochs = 3
num_training_steps = num_epochs * len(train_dataloader)
lr_scheduler = get_scheduler("linear", optimizer=optimizer, num_warmup_steps=0, num_training_steps=num_training_steps)import torchdevice = torch.device("cuda") if torch.cuda.is_available() else torch.device("cpu")
model.to(device)from tqdm.auto import tqdmprogress_bar = tqdm(range(num_training_steps))model.train()
for epoch in range(num_epochs):
    for batch in train_dataloader:
        batch = {k: v.to(device) for k, v in batch.items()}
        outputs = model(**batch)
        loss = outputs.loss
        loss.backward()optimizer.step()
        lr_scheduler.step()
        optimizer.zero_grad()
        progress_bar.update(1)
```

将这些样板代码放入一个更干净的界面是很好的。

## 管道

现在我们用微调后的模型来做预测。首先，保存模型

```
trainer.save_model()
```

并预测。变形金刚库提供了基于任务的管道来简化我们的任务。

```
from transformers import pipeline# load from previously saved model
pipe = pipeline("text-classification", model="test_trainer", tokenizer="bert-base-cased")
pipe("This restaurant is awesome")
```

输出

```
[{'label': 'LABEL_1', 'score': 0.9312610626220703}]
```

# 问答
加载数据集

```
from datasets import load_datasetsquad = load_dataset("squad")
```

## 加载标记器并标记数据

```
from transformers import AutoTokenizertokenizer = AutoTokenizer.from_pretrained("distilbert-base-uncased")def preprocess_function(examples):
    questions = [q.strip() for q in examples["question"]]
    inputs = tokenizer(
        questions,
        examples["context"],
        max_length=384,
        truncation="only_second",
        return_offsets_mapping=True,
        padding="max_length",
    )# offset_mapping gives corresponding start and end character in the original text for each token in our input IDsoffset_mapping = inputs.pop("offset_mapping")
    answers = examples["answers"]
    start_positions = []
    end_positions = []for i, offset in enumerate(offset_mapping):
        answer = answers[i]
        start_char = answer["answer_start"][0]
        end_char = answer["answer_start"][0] + len(answer["text"][0])# sequence_ids helps distinguish which parts of the offsets correspond to the question and which part correspond to the context
        sequence_ids = inputs.sequence_ids(i)# Find the start and end of the context
        idx = 0
        while sequence_ids[idx] != 1:
            idx += 1
        context_start = idx
        while sequence_ids[idx] == 1:
            idx += 1
        context_end = idx - 1# If the answer is not fully inside the context, label it (0, 0)
        if offset[context_start][0] > end_char or offset[context_end][1] < start_char:
            start_positions.append(0)
            end_positions.append(0)
        else:
            # Otherwise it's the start and end token positions
            idx = context_start
            while idx <= context_end and offset[idx][0] <= start_char:
                idx += 1
            start_positions.append(idx - 1)idx = context_end
            while idx >= context_start and offset[idx][1] >= end_char:
                idx -= 1
            end_positions.append(idx + 1)inputs["start_positions"] = start_positions
    inputs["end_positions"] = end_positions
    return inputstokenized_squad = squad.map(preprocess_function, batched=True, remove_columns=squad["train"].column_names)
```

记号化 _ 小队的形状

```
DatasetDict({
    train: Dataset({
        features: ['input_ids', 'attention_mask', 'start_positions', 'end_positions'],
        num_rows: 87599
    })
```

原始的小队数据集有上下文、问题、答案，但是我们需要在标记中找到这些答案的确切开始和结束位置。这就是“起始位置”、“结束位置”告诉我们的。

## 负载模型

```
from transformers import AutoModelForQuestionAnswering, TrainingArguments, Trainermodel = AutoModelForQuestionAnswering.from_pretrained("distilbert-base-uncased")
```

## 相同的培训师模式

```
training_args = TrainingArguments(
    output_dir="./results",
    evaluation_strategy="epoch",
    learning_rate=2e-5,
    per_device_train_batch_size=16,
    per_device_eval_batch_size=16,
    num_train_epochs=3,
    weight_decay=0.01
)trainer = Trainer(
    model=model,
    args=training_args,
    train_dataset=tokenized_squad["train"],
    eval_dataset=tokenized_squad["validation"],
    data_collator=data_collator,
    tokenizer=tokenizer
)
```

## 火车

```
trainer.train()
```

## 管道预测

首先保存模型

```
trainer.save_model()
```

现在我们使用“问答”管道来完成问答任务

```
context  = r"""
The Mars Orbiter Mission (MOM), also called Mangalyaan ("Mars-craft", from Mangala, "Mars" and yāna, "craft, vehicle")
is a space probe orbiting Mars since 24 September 2014? It was launched on 5 November 2013 by the Indian Space Research Organisation (ISRO). It is India's first interplanetary mission
and it made India the fourth country to achieve Mars orbit, after Roscosmos, NASA, and the European Space Company. and it made India the first country to achieve this in the first attempt.
The Mars Orbiter took off from the First Launch Pad at Satish Dhawan Space Centre (Sriharikota Range SHAR), Andhra Pradesh, using a Polar Satellite Launch Vehicle (PSLV) rocket C25 at 09:08 UTC on 5 November 2013.
The launch window was approximately 20 days long and started on 28 October 2013.
The MOM probe spent about 36 days in  Earth orbit, where it made a series of seven apogee-raising orbital maneuvers before trans-Mars injection
on 30 November 2013 (UTC).[23] After a 298-day long journey to the Mars Orbit, it was put into Mars orbit on 24 September 2014."""
nlp = pipeline("question-answering")
result = nlp(question="When did Mars Mission Launched?", context=context)
print(result['answer'])
```

# 摘要

## 加载数据集

```
from datasets import load_datasetraw_datasets = load_dataset("xsum")
```

## 加载标记器并标记数据

```
model_checkpoint = "t5-small"
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained(model_checkpoint)if model_checkpoint in ["t5-small", "t5-base", "t5-larg", "t5-3b", "t5-11b"]:
    prefix = "summarize: "
else:
    prefix = ""max_input_length = 1024
max_target_length = 128def preprocess_function(examples):
    inputs = [prefix + doc for doc in examples["document"]]
    model_inputs = tokenizer(inputs, max_length=max_input_length, truncation=True)# Setup the tokenizer for targets
    with tokenizer.as_target_tokenizer():
        labels = tokenizer(examples["summary"], max_length=max_target_length, truncation=True)model_inputs["labels"] = labels["input_ids"]
    return model_inputstokenized_datasets = raw_datasets.map(preprocess_function, batched=True)
```

符号化数据集的形状

```
DatasetDict({
    train: Dataset({
        features: ['document', 'summary', 'id', 'input_ids', 'attention_mask', 'labels'],
        num_rows: 204045
    })
```

“文档”、“摘要”、“id”来自原始数据集，表示原始文档、文档摘要、文档索引号。“标签”是标记化摘要的“输入标识”。

## 负载模型

```
from transformers import AutoModelForSeq2SeqLM, DataCollatorForSeq2Seq, Seq2SeqTrainingArguments, Seq2SeqTrainermodel = AutoModelForSeq2SeqLM.from_pretrained(model_checkpoint)
```

## 创建培训师

```
batch_size = 16
model_name = model_checkpoint.split("/")[-1]
args = Seq2SeqTrainingArguments(
    f"{model_name}-finetuned-xsum",
    evaluation_strategy = "epoch",
    learning_rate=2e-5,
    per_device_train_batch_size=batch_size,
    per_device_eval_batch_size=batch_size,
    weight_decay=0.01,
    save_total_limit=3,
    num_train_epochs=1,
    predict_with_generate=True,
    fp16=True
)data_collator = DataCollatorForSeq2Seq(tokenizer, model=model)trainer = Seq2SeqTrainer(
    model,
    args,
    train_dataset=tokenized_datasets["train"],
    eval_dataset=tokenized_datasets["validation"],
    data_collator=data_collator,
    tokenizer=tokenizer
)
```

## 火车

```
trainer.train()
```

## 管道预测

首先保存模型

```
trainer.save_model()
```

## 使用管道预测

```
# use bart in pytorch
from transformers import pipeline
summarizer = pipeline("summarization", model=f"{model_name}-finetuned-xsum",tokenizer=model_checkpoint)
summarizer("Designing a proper application architecture is the difference between a project that flourishes and one that gets pigeonholed into a set of inextensible features. When it comes to an open source project like Ostia, it becomes especially important, as the ‘activation energy’ for a developer to get up to speed on a project is a huge barrier.", min_length=5, max_length=20)
#[{'summary_text': 'Ostia is an open source project that flourishes and gets pigeonhole'}]
```

# 工作流模式

对于这些任务，模式是

1.  加载原始数据集
2.  加载标记器并标记数据
3.  可选:分类任务时拆分数据
4.  定义培训参数和培训师
5.  开始训练/微调
6.  保存模型，然后加载带有管道和特定任务的模型
7.  使用管道预测

# 附录

## 迁移学习

[](https://towardsdatascience.com/transfer-learning-from-pre-trained-models-f2393f124751) [## 从预先训练的模型中转移学习

### 如何快速简单地解决任何图像分类问题

towardsdatascience.com](https://towardsdatascience.com/transfer-learning-from-pre-trained-models-f2393f124751) 

## 管道

[](https://www.analyticsvidhya.com/blog/2021/12/all-nlp-tasks-using-transformers-package/) [## 使用 Transformers Pipeline-Analytics vid hya 的所有 NLP 任务

### 1.理解变压器包 2。文本分类 3。问题回答 4。文本摘要 5。语言…

www.analyticsvidhya.com](https://www.analyticsvidhya.com/blog/2021/12/all-nlp-tasks-using-transformers-package/) [](https://www.analyticsvidhya.com/blog/2022/01/hugging-face-transformers-pipeline-functions-advanced-nlp/) [## 拥抱脸变压器管道功能|高级自然语言处理

### 这篇文章作为数据科学博客的一部分发表。目的这篇博客文章将学习如何使用…

www.analyticsvidhya.com](https://www.analyticsvidhya.com/blog/2022/01/hugging-face-transformers-pipeline-functions-advanced-nlp/) [](/geekculture/pipeline-object-in-transformers-using-hugging-face-6577f57a4c18) [## 变形金刚中的管道对象🤗

### 上周我完成了一个免费的拥抱脸课程，在那里我学到了很多新东西，比如变形金刚…

medium.com](/geekculture/pipeline-object-in-transformers-using-hugging-face-6577f57a4c18) [](https://huggingface.co/docs/transformers/glossary) [## 词汇表

### 自动编码模型:见 MLM 自回归模型:见 CLM CLM:因果语言建模，一个预训练任务，其中…

huggingface.co](https://huggingface.co/docs/transformers/glossary) [](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)