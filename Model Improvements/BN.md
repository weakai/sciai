---
title: Normalization
subtitle: 批正则化
tags: AI, pytorch
---

#### [BN 究竟起了什么作用？一个闭门造车的分析](https://kexue.fm/archives/6992)

BN，也就是Batch Normalization，是当前深度学习模型（尤其是视觉相关模型）的一个相当重要的技巧，它能加速训练，甚至有一定的抗过拟合作用，还允许我们用更大的学习率，总的来说颇多好处（前提是你跑得起较大的batch size）。

那 BN 究竟是怎么起作用呢？早期的解释主要是基于概率分布的，大概意思是将每一层的输入分布都归一化到 N(0,1) 上，减少了所谓的 Internal Covariate Shift，从而稳定乃至加速了训练。
这种解释看上去没什么毛病，但细思之下其实有问题的：不管哪一层的输入都不可能严格满足正态分布，从而单纯地将均值方差标准化无法实现标准分布 N(0,1)；
其次，就算能做到 N(0,1)，这种诠释也无法进一步解释其他归一化手段（如 Instance Normalization、Layer Normalization）起作用的原因。

在去年的论文《How Does Batch Normalization Help Optimization?》里边，作者明确地提出了上述质疑，否定了原来的一些观点，并提出了自己关于 BN 的新理解：他们认为 BN 主要作用是使得整个损失函数的 landscape 更为平滑，从而使得我们可以更平稳地进行训练。



与迁移学习
预训练得到的各特征，如果分布差别大，那么需要在 fine-tuning 的时候重新 grid-search。不方便。因此可以用 Normalization

## Batch Normalization

BN 用的好很强，用不好就有莫名的 Bug。
如果用 Transform 的 LayerNorm, 就不用理会 BN 了。

Shuffling BN

出自 Moco
BN 会信息泄露： 通过 running mean, running variance 值就可以推断整个模型的数据分布，从而走捷径。 因为 BN 一般在 GPU 上运算，因此可以在送去 GPU 之前打乱数据，这个操作不会改变 Loss 值

在 CNN 中
1. N 作用在非线性映射前，当神经网络收敛速度缓慢时候，或者梯度爆炸无法训练时候可以考虑用 BN
2. 一般情况也可以用 BN 来尝试加快训练速度，提高模型的精度。

BN比较适用的场景是：每个 mini-batch 比较大，数据分布比较接近。在进行训练之前，要做好充分的 shuffle，否则效果会差很多。

由于 BN 需要在运行过程中统计每个 mini-batch 的一阶统计量和二阶统计量，因此不适用于动态的网络结构和 RNN 网络。不适用于动态的网络结构和 RNN 网络

## Sync BN

适用多 GPUs 的 BN

## Reference

[Batch normalization in 3 levels of understanding](https://towardsdatascience.com/batch-normalization-in-3-levels-of-understanding-14c2da90a338)


