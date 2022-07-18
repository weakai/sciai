---
title: transformer-xl
date: 2021-11-27
update: 2022-7-18
tags: [transformer, model, transformer-xl]
---

本文介绍 Transformer-XL，为了更好地理解 XLNet。

## 前言

序列模型捕获数据长期依赖的能力在任何 NLP 任务中都是至关重要的，LSTM 通过引进门机制将 RNN 的长期依赖的捕获能力提升到 200 个左右，Transformer 的提出则进一步提升了获长期依赖的能力，但是 Transformer 的捕获长期依赖的能力是无限长的吗？如果有一个需要捕获几千个时间片的能力的模型才能完成的任务，Transformer 能够胜任吗？答案从目前 Transformer 的设计来看，它还是做不到。

这篇文章介绍的 Transformer-XL（extra long）则是为了进一步提升 Transformer 建模长期依赖的能力。它的核心算法包含两部分：片段递归机制（segment-level recurrence）和相对位置编码机制(relative positional  encoding)。Transformer-XL 带来的提升包括：1. 捕获长期依赖的能力；2. 解决了上下文碎片问题（context  segmentation problem）；3. 提升模型的预测速度和准确率。

Transformer 是一个自回归模型（auto-regressive），也就是说在测试时模型依次取时间片为 $L$ 的分段，然后将整个片段提供给模型后预测一个结果，如图 2 所示。在下个时间片时再将这个分段向右移一个单位，这个新的片段也将通过整个网络的计算后得到一个值。Transformer 的这个特性导致其预测阶段的计算量是非常大的，这也限制了其应用领域。注意这是在生物序列的场景下，长度不是问题。

![](https://pic4.zhimg.com/v2-0062f7a1be5afd1b745731fa31833d63_b.webp)

## Transformer-XL 介绍

### 片段递归

和 Transformer 一样，Transformer-XL 在训练的时候也是以固定长度的片段的形式进行输入的，不同的是 Transformer-XL 的上一个片段的状态会被缓存下来然后在计算当前段的时候再重复使用上个时间片的隐层状态。因为上个片段的特征在当前片段进行了重复使用，这也就赋予了 Transformer-XL 建模更长期的依赖的能力。

## References

- [transformer-XL 与 XLNet 笔记](https://carlos9310.github.io/2019/11/11/transformer-xl-and-xlnet/)
