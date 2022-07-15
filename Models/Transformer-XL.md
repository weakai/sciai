---
title: transformer-xl
post:
modified:
tags: [transformer, model, transformer-xl]
---

Transformer 改进，根据历史的词预测下一个词。

Transformer 要求输入是定长的词序列(不像 RNN 可以处理变成的输入序列)，太长的截断，不足的 padding，这样我们把一个语料库的字符串序列切分成固定长度的 segments。它有下面一些问题：

- 由于定长的要求，我们不可能让输入太长。因此虽然Self-Attention机制虽然不太受长度的约束，但是 Transformer 的语言模型实际能够考虑的上下文就是输入的长度。
- 因为我们在序列语言模型的时候通常很难准确的分句(或者有时候一个句子比最大长度还长)，所以一个 Segment 很可能不是一个完整的句子(甚至它是从某个句子的中间部分开始的)，这样前面的几个词就很难预测(给人一个没头没脑的句子也很难预测)，因为语言模型是自回归的，一步错步步错。这就是所谓的 context fragmentation 的问题。
- 性能问题

