---
title: RankGAN
date: 2022-07-25
tags: [RankGAN]
---

NIPS 2018

以产生高质量的语言描述

RankGAN 在一个对抗性的框架下，从机器书写和人类书写句子之间的相对排名信息中学习该模型。在所提出的 RankGAN 中，将 discriminator 的训练放宽为一个 learning-to-rank 优化问题。具体地说，新的对抗网络由两个神经网络模型组成，一个生成器和一个 ranker。与执行二元分类任务不同，对 ranker 进行训练，将机器写的句子排列得比人写的句子低。因此，训练 generator 来合成混淆语言的句子，使机器书写的句子在参考方面比人类书写的句子更高。在 Learning 过程中能够采用策略梯度技术来克服不可微问题。因此，通过共同查看一组数据样本并通过相对排名评估其质量，discriminator 能够更好地评估样本的质量，进而帮助 generator更好地学习。

![](https://img-blog.csdnimg.cn/2020091316231154.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzMzkwODA5,size_16,color_FFFFFF,t_70#pic_center)



## Reference

- []()