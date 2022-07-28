---
title: ELECTRA
date: 2022-07-25
tags: [ELECTRA]
---

#### 创新点

- 提出了新的模型预训练的框架，采用 generator 和 discriminator 的结合方式，但又不同于 GAN
- 将 Masked Language Model 的方式改为了 replaced token detection
- 因为 masked language model 能有效地学习到 context 的信息，所以能很好地学习 embedding，所以使用了 weight sharing 的方式将 generator 的 embedding 的信息共享给 discriminator
- discriminator 预测了 generator 输出的每个 token 是不是 original 的，从而高效地更新 transformer 的各个参数，使得模型的熟练速度加快
- 该模型采用了小的 generator 以及 discriminator 的方式共同训练，并且采用了两者 loss 相加，使得 discriminator 的学习难度逐渐地提升，学习到更难的 token（plausible tokens）
- 模型在 fine-tuning 的时候，丢弃 generator，只使用 discriminator

#### 关键思想

Electra 采用的预训练方式主要是 GAN 思想主导的预训练，笔者之前也介绍过 GAN 到底是怎么回事，不会的小伙伴可以去翻阅一下生成对抗网络 | 原理及训练过程。

我们知道 BERT 是直接采用 15% 的“[MASK] ”来掩盖某些字符（token ），让模型在预训练过程中预测被“[MASK] ”掉的字符（token ）。

而 electra 则采用将这个思想用在Gan （generator ）的生成器中，先随机“[MASK] ”掉一些字符（token ），然后用一个生成器（generator ）对被“[MASK] ”的字符生成相应的“伪字符（fake token ）”，而discriminator 辨别器（也就是electra ）用来判断哪些字符（token ）被更换过，论文作者将这个预训练任务称之为RTD（replaced token detection） 。

这篇文章主要的贡献是提出了一种最新的 BERT 类模型的预训练方式 ：RTD（replaced token detection） 。

关键思想是训练文本编码器，以区分输入令牌与由小型生成器 generator 网络产生的高质量负样本。与 MLM （ masked language modeling, 也就是 BERT 的预训练方式）相比，它的预训练目标具有更高的计算效率，并且可以在下游任务上实现更好的性能。即使使用相对较少的计算量，它也能很好地工作。

至于具体的效果好不好，笔者这边还没有完全测过。不过笔者的师弟用 electra 跑某个比赛数据，线上成绩倒是上了 6 个百分点，然而这个比赛的数据集比较小，也不具备太多权威性。

最后值得一说的是，现在的预训练模型自 BERT 横空出世之后，便如雨后春笋般层出不穷，不过我们只要掌握 BERT 的原理与应用，大致就可以快速读懂一个新的预训练模型的原理，它们大多都是基于 BERT 原有的缺陷进行改进的。

#### 主要作用

ELECTRA 的论文指出，自己的模型效果能够达到 state-of-the-art，但是在真正的 GELU 榜单上，还是敌不过 Roberta 等模型。但是在小模型的表现上，我们可以发现 ELECTRA 的效果确实更加地好，所以 ELECTRA 的目前的作用主要：

- 可以利用这个框架，自己训练一个预训练模型，单个 GPU 就可以训练得到一个小的语言模型，然后在特定的领域可以得到更优的结果，然后再在这个领域下进行特定任务的 fine-tuning。
- 使用小的 ELECTRA 模型，在不能使用 GPU 的场景，或者性能要求高的场景，可以得到好的结果
- ELECTRA 的效果在多分类上不如 Roberta，可能与预训练时采用的是二分类任务有关。

#### architecture



## Reference

- [ELECTRA 详解](https://zhuanlan.zhihu.com/p/118135466)