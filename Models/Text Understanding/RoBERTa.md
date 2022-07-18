---
title: RoBERTa
tags: [RoBERTa, model]
---

站在 BERT 的肩膀上。

RoBERTa 的全称叫做 Robustly optimized BERT approach。RoBERTa 之于 BERT 的改动很简单，主要是用了更多的数据，训练上，采用动态【MASK】、去掉下一句预测的 NSP 任务、更大的 batch_size、文本编码。

- 更大的模型参数量（从 RoBERTa 论文提供的训练时间来看，模型使用 1024 块 V 100 GPU 训练了 1 天的时间）
- 更多的训练数据（包括：CC-NEWS 等在内的 160GB 纯文本）

训练方法上的改进

- 动态 MASK
  - BERT 依赖随机掩码和预测 token。原版的 BERT 实现在数据预处理期间执行一次掩码，得到一个静态掩码。而 RoBERTa 使用了动态掩码：每次向模型输入一个序列时都会生成新的掩码模式。这样，在大量数据不断输入的过程中，模型会逐渐适应不同的掩码策略，学习不同的语言表征。
  - 预训练的每一个 step，是重新挑选 15% token进行【MASK】的，而 BERT 是固定的，就是对于同一个输入样本，在不同的 epoch，输入是一样的，实验结果见下图，有很微弱的提升吧。
- 更大 batch_size
  - 研究人员尝试过从 256 到 8000 不等的批数量。
- 文本编码
  - Byte-Pair Encoding（BPE）是字符级和词级别表征的混合，支持处理自然语言语料库中的众多常见词汇。原版的 BERT 实现使用字符级别的 BPE 词汇，大小为 30K，是在利用启发式分词规则对输入进行预处理之后学得的。Facebook 研究者没有采用这种方式，而是考虑用更大的 byte 级别 BPE 词汇表来训练 BERT，这一词汇表包含 50K 的 subword 单元，且没有对输入作任何额外的预处理或分词。
- 去 NSP 任务

## 参考

[RoBERTa中文预训练模型，你离中文任务的「SOTA」只差个它](https://www.jiqizhixin.com/articles/2019-09-05-6)

# RoBERTa
