---
title: Transformer-xl
date: 2022-7-20
tags: []
---

## Motivation

- Transformer 在预训练阶段，设置了固定序列长度 max_len 的上下文，finetuning 阶段，模型不能获取大于 max_len 的上下文依赖；
- Transformer 在长文本编码过程中，可采用按照句子边界拆分和按照 max_len 截断的方式，进行片段的编码，论文指出第二种不考虑句子边界的编码方式效果更好，但仍然存在上下文碎片的问题 context fragmentation

## How to solve

### Segment-level recurrence mechanism - 使模型获取更长上下文

在单向编码长文本时，由于 transformer 设置了 max_len，从而在训练时，第 i 个 token 只能 attention 到前 i 个 token，且当 transformer 以 max_len 为窗口滑动时，每前进一步，绝对位置编码需要跟着前进一步，使得每一步都需要重新计算 max_len 中每个 token 的隐层表示，如下图。

Transformer-XL 通过设置 memory-span 使得当前 max_len 窗口中的每个 toke n都能 attention 到前 max_len 个 token，因此 Transformer-XL 在每前进一步时，只用计算当前位置的 token 的隐层表示，同时在更新梯度时，只更新当前窗口内的梯度(防止梯度 bp 的距离太远)，从而实现了输出隐层表示的更长上下文关联，和高效的编码速度。

### Relative Positional Encodings - 使模型在前向过程中更快

Segment-level recurrence mechanism 机制中提到的，max_len 窗口中的每个 token 都能 attention 前 max_len 个 token，其中可能会有一些 token 在上一个 seg，这样就存在位置编码不连续，或者相同 token 在当前 seg 和前一个 seg 具有相同的 attention 值的问题。因此为了实现 transformer-XL 训练和长文本编码运用之间的等效表示，将绝对位置编码替换为以当前 token 为基准的相对位置编码 Relative positional encodings。

绝对位置编码在输入 transformer 之前就和 token emb 求和，相对位置编码需要在计算 attention score 加入和计算。

### Relative positional emb 代码解析

在解析代码前，先用图示展示 relative pos emb 的实现过程(无 memory 版本) `rel_shift(*)`



## Reference

- [【核心代码解读】Transformer-XL](https://zhuanlan.zhihu.com/p/74485142)