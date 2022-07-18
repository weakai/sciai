---
title: XLNet
date: 2022-7-18
---

- 18 年底谷歌推出了 bert，该模型一经问世就占据了 nlp 界的统治地位，如今 CMU 和 google brain 联手推出了 bert 的改进版 xlnet。
在这之前也有很多公司对 bert 进行了优化，包括百度、清华的知识图谱融合，微软在预训练阶段的多任务学习等等，但是这些优化并没有把 bert 致命缺点进行改进。
xlnet 作为 bert 的升级模型，主要在以下三个方面进行了优化
  - 采用 AR 模型替代 AE 模型，解决 mask 带来的负面影响
  - 双流注意力机制
  - 引入 transformer-xl

- XLNet = Bert + GPT 2.0 + Transformer XL
  - XLNet 融合自回归语言模型（AR LM）和自编码语言模型（DAE LM）的优点
  - 首先，它通过 PLM 预训练目标，吸收了 Bert 的双向语言模型
    - Bert 这种自编码语言模型的好处是: 能够同时利用上文和下文，所以信息利用充分
  - 然后，GPT2.0 的核心其实是更多更高质量的预训练数据，这个明显也被 XLNet 吸收进来了
  - 再然后，Transformer XL 的主要思想也被吸收进来，它的主要目标是解决 Transformer 对于长文档 NLP 应用不够友好的问题。

## 训练

- XLNet 仍然遵循两阶段的过程
  - 第一个阶段是语言模型预训练阶段
  - 第二阶段是任务数据 Fine-tuning 阶段

## AR 与 AE 语言模型

目前主流的 nlp 预训练模型包括两类 AR 和 AE。
AR 模型的缺点，没错该模型是单向的，我们更希望的是根据上下文来预测目标，而不单是上文或者下文，之前 open AI 提出的 GPT 就是采用的 AR 模式，
包括 GPT2.0 也是该模式，那么为什么 open ai 头要这么铁坚持采用单向模型呢，看完下文你就知道了。
AE 模型采用的就是以上下文的方式，最典型的成功案例就是 bert。
bert 的最大问题也是处在这个 MASK 的点，因为在微调阶段，没有 MASK 这就导致预训练和微调数据的不统一，从而引入了一些人为误差，我觉得这应该就是为什么 GPT 坚持采用AR模型的原因。

## 排列语言模型

该模型不再对传统的 AR 模型的序列的值按顺序进行建模，而是最大化所有可能的序列的排列组合顺序的期望对数似然。
Permutation Language Model，因为排列语言模型开启了自回归语言模型如何引入下文的一个思路。

## 基于目标感知表征的双流自注意力

- 和 Bert 比，只是实现方式不同而已。
- 虽然排列语言模型能满足目前的目标，但是对于普通的 transformer 结构来说是存在一定的问题的
- 通过 Attention 掩码，这里简单说下“双流自注意力机制”，一个是内容流自注意力，其实就是标准的 Transforme r的计算过程；
主要是引入了 Query 流自注意力，这个是干嘛的呢？其实就是用来代替 Bert 的那个 `[Mask]` 标记的，因为 XLNet 希望抛掉 `[Mask]` 标记符号，
但是比如知道上文单词 x1,x2，要预测单词 x3，此时在 x3 对应位置的 Transformer 最高层去预测这个单词，但是输入侧不能看到要预测的单词 x3，
Bert 其实是直接引入 `[Mask]` 标记来覆盖掉单词 x3 的内容的，等于说 `[Mask]` 是个通用的占位符号。
而 XLNet 因为要抛掉 [`Mask]` 标记，但是又不能看到 x3 的输入，于是 Query 流，就直接忽略掉 x3 输入了，只保留这个位置信息，
用参数 w 来代表位置的 embedding 编码。其实 XLNet 只是扔了表面的 `[Mask]` 占位符号，内部还是引入 Query 流来忽略掉被 Mask 的这个单词。

## 集成 Transformer-XL

片段循环机制，解决超长序列的依赖问题
相对位置编码，搭配片段循环机制
预训练，去除了 Next Sentence Prediction，因为该任务对结果的提升并没有太大的影响

## 参考

- [XLNet 详解](https://blog.nowcoder.net/n/bfbb3794a38d47aa9dfce3de29efea01?from=nowcoder_improve)
- [transformer-XL 与 XLNet 笔记](https://carlos9310.github.io/2019/11/11/transformer-xl-and-xlnet/)
- [最通俗易懂的 XLNET 详解](https://blog.csdn.net/u012526436/article/details/93196139)
- [XLNet: 运行机制及和 Bert 的异同比较](https://zhuanlan.zhihu.com/p/70257427)

