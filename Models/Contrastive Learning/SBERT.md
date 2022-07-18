---
title: SBERT
date: 2022-6-15
website: https://www.sbert.net/
github: https://github.com/UKPLab/sentence-transformers
stars: 8k
---

## Why Sentence-BERT

使用 BERT 或 RoBERT 本身就可以进行文本相似计算(BERT 下游任务中句子对作为输入进行)。但是这种方法需要使用大量的计算资源，例如在 10000 条文本中找出最相似的两条文本，由于每对文本都需要输入 BERT 中进行计算，所以共计需要约 500 万次计算，耗时约 65 小时，因此 BERT 是不适合进行语义搜索匹配和无监督聚类这些任务。那么可不可以直接将每个句子转化为一个定长的 Embedding 再来比较呢？当然可以，Sentence-BERT（SBERT）就可以解决这个问题。

SBERT 基于 BERT，该网络结构利用孪生网络和三胞胎网络结构生成具有语义意义的句子 embedding 向量，语义相近的句子其 embedding 向量距离就比较近，从而可以用来进行相似度计算(余弦相似度、曼哈顿距离、欧式距离)。该网络结构在查找最相似的句子对，从上述的 65 小时大幅降低到 5 秒(计算余弦相似度大概 0.01 s)，精度能够依然保持不变。这样 SBERT 可以完成某些新的特定任务，例如相似度对比、聚类、基于语义的信息检索。

另外：

- 将 RoBERTa 代替 BERT 没有显著提升。

## 原理

### 孪生网络

挛生网络 Siamese network（后简称SBERT），其中 Siamese 意为“连体人”，即两人共用部分器官。SBERT 模型的子网络都使用 BERT 模型，且两个 BERT 模型共享参数。当对比 A,B 两个句子相似度时，它们分别输入 BERT 网络，输出是两组表征句子的向量，然后计算二者的相似度；利用该原理还可以使用向量聚类，实现无监督学习任务。

![](https://upload-images.jianshu.io/upload_images/5357893-b2a0a42ef78d44fa.png?imageMogr2/auto-orient/strip|imageView2/2/w/268/format/webp)

挛生网络有很多应用，比如使用图片搜索时，输入照片将其转换成一组向量，和库中的其它图片对比，找到相似度最高（距离最近）的图片；在问答场景中，找到与用户输入文字最相近的标准问题，然后给出相应解答；对各种文本标准化等等。

衡量语义相似度是自然语言处理中的一个重要应用，BERT源码中并未给出相应例程（run_glue.py只是在其示例框架内的简单示例），真实场景使用时需要做大量修改；而SBERT提供了现成的方法解决了相似度问题，并在速度上更有优势，直接使用更方便。

SBERT 对 PyTorch 进行了封装，简单使用该工具时，不仅不需要了解太多 BERT API 的细节， Pytorch 相关方法也不多。

## Sentence-BERT Model

### pooling 策略

由于输入的 Sentence 长度不一，而我们希望得到统一长度的 Embeding，所以当 Sentence 从 BERT 输出后我们需要执行 pooling 操作，常用的 Pooling 操作有：

1. CLS-pooling: 直接取 [CLS] 的 Embedding
2. mean-pooling: 取每个 Token 的平均 Embedding
3. max-pooling: 对得到的每个 Embedding 取 max

### 模型结构

同时针对不同的任务，可以设计出不同的结构和损失函数，这里给出了 3 个举例:

1. Classification Objective Function
2. Regression Objective Function
3. Triplet Objective Function

### 模型训练

文中训练结合了[SNLI](https://nlp.stanford.edu/projects/snli/)(Stanford Natural Language Inference)和[Multi-Genre NLI](https://cims.nyu.edu/~sbowman/multinli/)两种数据集。

## 测评

### 语义文本相似度(Semantic Textual Similarity-STS)

在评测的时候，这里采用余弦相似度来比较两个句子向量的相似度。

- 有监督 STS
- 无监督 STS

### SentEval

SentEval 是一个当前流行的用来评测句子 embedding 质量的工具，这里句子 embedding 可以作为逻辑回归模型的特征，从而构建一个分类器，并在 test 集上计算其精度。

## Links

- [论文翻译 | Sentence-BERT: 一种能快速计算句子相似度的孪生网络](https://www.cnblogs.com/gczr/p/12874409.html)

- [文本相似 Sentence-BERT 原理与实践](https://zhuanlan.zhihu.com/p/373253456)