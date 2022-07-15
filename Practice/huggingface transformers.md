---
title: huggingface transformers
---

- [Huggingface Transformer教程(一)](http://fancyerii.github.io/2021/05/11/huggingface-transformers-1/)

```python
from transformers import AutoTokenizer, AutoModelForSequenceClassification

model_name = "nlptown/bert-base-multilingual-uncased-sentiment"
model_name = "distilbert-base-uncased-finetuned-sst-2-english"

model = AutoModelForSequenceClassification.from_pretrained(model_name)
tokenizer = AutoTokenizer.from_pretrained(model_name)
classifier = pipeline('sentiment-analysis', model=model, tokenizer=tokenizer)

inputs = tokenizer("We are very happy to show you the 🤗 Transformers library.")

pt_batch = tokenizer(
    ["We are very happy to show you the 🤗 Transformers library.", "We hope you don't hate it."],
    padding=True,
    truncation=True,
    max_length=512,
    return_tensors="pt"
)

# 查看标记处理后的批数据
for key, value in pt_batch.items():
    print(f"{key}: {value.numpy().tolist()}")

# 使用，送入模型
pt_outputs = model(**pt_batch)
print(pt_outputs)
# SequenceClassifierOutput(loss=None, logits=tensor([[-4.0833,  4.3364],
#     [ 0.0818, -0.0418]], grad_fn=<AddmmBackward>), hidden_states=None, attentions=None)
# all outputs are objects that contain the model’s final activations along with other metadata

# 激活函数 softmax
# 将 logits 转化为预测数值
from torch import nn
pt_predictions = nn.functional.softmax(pt_outputs.logits, dim=-1)
print(pt_predictions) # Output: tensor
```

如果你输入了 label 到模型中，模型的输出对象也会包含 loss 属性

```python
import torch
pt_outputs = pt_model(**pt_batch, labels = torch.tensor([1, 0]))
print(pt_outputs) # Outputs: SequenceClassifierOutput
```

保存以及加载预训练数据

```python
tokenizer.save_pretrained(save_directory)
model.save_pretrained(save_directory)

from transformers import AutoModel
tokenizer = AutoTokenizer.from_pretrained(save_directory)
model = AutoModel.from_pretrained(save_directory, from_tf=True)
```

从模型中提取 隐藏状态 和权重

```python
pt_outputs = pt_model(**pt_batch, output_hidden_states=True, output_attentions=True)
all_hidden_states  = pt_outputs.hidden_states
all_attentions = pt_outputs.attentions
```

从源码而不是用 AutoModel 加载数据和模型

```python
from transformers import DistilBertTokenizer, DistilBertForSequenceClassification
model_name = "distilbert-base-uncased-finetuned-sst-2-english"
model = DistilBertForSequenceClassification.from_pretrained(model_name)
tokenizer = DistilBertTokenizer.from_pretrained(model_name)
```

定制模型

```python
from transformers import DistilBertConfig, DistilBertTokenizer, DistilBertForSequenceClassification
# 生成配置
config = DistilBertConfig(n_heads=8, dim=512, hidden_dim=4*512)
model = DistilBertForSequenceClassification(config)

tokenizer = DistilBertTokenizer.from_pretrained('distilbert-base-uncased')
```

你如果只改变 head 的 label 数，你是不需要改变 model 的 pretrained body 的。这种方式的模型定制会比较简单

```python
from transformers import DistilBertConfig, DistilBertTokenizer, DistilBertForSequenceClassification
model_name = "distilbert-base-uncased"
model = DistilBertForSequenceClassification.from_pretrained(model_name, num_labels=10)

tokenizer = DistilBertTokenizer.from_pretrained(model_name)

```

## 哲学

# Glossary

- CLM: causal language modeling, a pretraining task where the model reads the texts in order and has to predict the next word. It’s usually done by reading the whole sentence but using a mask inside the model to hide the future tokens at a certain timestep.

- MLM: masked language modeling, a pretraining task where the model sees a corrupted version of the texts, usually done by masking some tokens randomly, and has to predict the original text.

- multimodal: a task that combines texts with another kind of inputs (for instance images).

- 

- NLG: natural language generation, all tasks related to generating text (for instance talk with transformers, translation).

- NLU: natural language understanding, all tasks related to understanding what is in a text (for instance classifying the whole text, individual words).

## Input IDs

模型输入的唯一必须参数

```python
from transformers import BertTokenizer
tokenizer = BertTokenizer.from_pretrained("bert-base-cased")
sequence = "A Titan RTX has 24GB of VRAM"

# 转化为可读形式
tokenized_sequence = tokenizer.tokenize(sequence)
print(tokenized_sequence)

# 转化为 Input IDs
inputs = tokenizer(sequence)
```



## Good words

wait

These objects are described in greater detail here.

activations 激活（函数）

 browse the source code

 the auto magic

Transformers is an opinionated library built for 专为谁设计

hands-on practitioners 亲身实践者

Be as easy and fast to use as possible

Every model is different yet bears similarities with the others. 

which are detailed here alongside usage examples

## 参考

https://huggingface.co/transformers/quicktour.html