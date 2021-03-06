---
title: GAN
date: 2022-7-22
tags: [GAN]
---

## 简介

首先，GAN 最厉害的地方是它的学习性质是无监督的。
GAN 也不需要标记数据，这使 GAN 功能强大，因为数据标记的工作非常枯燥。
其次，GAN 的潜在用例使它成为交谈的中心。
它可以生成高质量的图像，图片增强，从文本生成图像，将图像从一个域转换为另一个域，随年龄增长改变脸部外观等等。
第三，围绕 GAN 不断的研究是如此令人着迷，以至于它吸引了其他所有行业的注意力。我们将在本文后面部分讨论重大技术突破。

#### GAN 的宏观背景

GAN 是属于机器学习中 generative 中的 implicit model 的一种。
Generative 体现在：GAN 并不能计算数据真实分布的公式，也就是不能计算概率，但它能根据学习到的数据真实分布来生成一个样本。
implicit 体现在：它的模型是通过网络层实现的，并不是一个确定的数学公式，好比高斯分布等。
VAE，GAN 这些生成模型终极目标是模拟数据的真实分布，模拟的好坏自然得有个测距公式来计算：
VAE 里面是用 KL divegence 来计算两个分布的距离。
GAN 里面可以理解成是用 Jessen-Shannon divegence 来计算两个分布的距离。
我们常说 GAN 是一个 min-max 训练过程，所谓的 max 其实是对应着鉴别网络，目的是为了训练鉴别网络让其等同于最优 JS divence 的作用，然后在这个最优的测距网络下，min 生成网络。
现在要说到 WS-GAN 了，它的最大贡献是（个人观点）指出了 KL,JS 等这些测距工具都有一个缺点，那就是不连续性，意思就是两个分布的差距是跳跃的，不是连续的，这就导致训练鉴别网络时很不稳定，然后作者提出了 WS divegence 这个测距工具，WS 算出来的两个分布的差距是连续的，  用它来代替鉴别网络（撤换掉 sigmoid 等），因为是连续，所以训练的时候你可以很清晰的看到鉴别网络的 loss 是逐步的减小，整个训练过程稳定下来了。

只是根据自己的记忆大概的写一下，所以难免很多地方不准确，多谅解，只是希望大家能够有个更高的视角去看待 GAN，有很多文章都在 divegence 这块做贡献，谢谢。

### 历史

![](https://static001.geekbang.org/wechat/images/f0/f0b6aa5e59f9f9f503512d65b4d8adb2.png)

## Reference

- [生成对抗网络(GAN)的发展史](https://zhuanlan.zhihu.com/p/63428113)
