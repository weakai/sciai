---
title: WGAN
date: 2022-7-22
tags: [WGAN, GAN]
---

推荐阅读 [生成对抗网络——FGAN和WGAN](https://alberthg.github.io/2018/05/13/wgan/)

Wasserstein GAN（下面简称 WGAN）成功地做到了以下爆炸性的几点：

- 彻底解决 GAN 训练不稳定的问题，不再需要小心平衡生成器和判别器的训练程度
- 基本解决了 collapse mode 的问题，确保了生成样本的多样性 
- 训练过程中终于有一个像交叉熵、准确率这样的数值来指示训练的进程，这个数值越小代表 GAN 训练得越好，代表生成器产生的图像质量越高（如题图所示）
- 以上一切好处不需要精心设计的网络架构，最简单的多层全连接网络就可以做到

那以上好处来自哪里？这就是令人拍案叫绝的部分了——实际上作者整整花了两篇论文，在第一篇《[Towards Principled Methods for Training Generative Adversarial Networks](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1701.04862)》里面推了一堆公式定理，从理论上分析了原始 GAN 的问题所在，从而针对性地给出了改进要点；在这第二篇《[Wasserstein GAN](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1701.07875)》里面，又再从这个改进点出发推了一堆公式定理，最终给出了改进的算法实现流程，而改进后相比原始 GAN 的算法实现流程却只改了四点：

- 判别器最后一层去掉 sigmoid
- 生成器和判别器的 loss 不取 log
- 每次更新判别器的参数之后把它们的绝对值截断到不超过一个固定常数 c
- 不要用基于动量的优化算法（包括 momentum 和 Adam），推荐 RMSProp，SGD 也行

改动是如此简单，效果却惊人地好，以至于 Reddit 上不少人在感叹：就这样？没有别的了？太简单了吧！这些反应让我想起了一个颇有年头的鸡汤段子，说是一个工程师在电机外壳上用粉笔划了一条线排除了故障，要价一万美元——画一条线，1 美元；知道在哪画线，9999美元。上面这四点改进就是作者 Martin Arjovsky 划的简简单单四条线，对于工程实现便已足够，但是知道在哪划线，背后却是精巧的数学分析，而这也是本文想要整理的内容。

Wasserstein GAN 对 GAN 的改进也是从替换 KL Divergence 这个角度对 GAN 进行改进。如下图：

本文内容分为五个部分：

- 原始 GAN 究竟出了什么问题？（此部分较长）
- WGAN 之前的一个过渡解决方案 
- Wasserstein 距离的优越性质
  - “推土机-Divergence”，将两个分布看作两堆土，Divergence 计算的就是为了将两个土堆推成一样的形状所需要泥土搬运总距离。
- 从 Wasserstein 距离到 WGAN
- 总结

*理解原文的很多公式定理需要对测度论、 拓扑学等数学知识有所掌握，本文会从直观的角度对每一个重要公式进行解读，有时通过一些低维的例子帮助读者理解数学背后的思想，所以不免会失于严谨，如有引喻不当之处，欢迎在评论中指出。* 

*以下简称《[Wassertein GAN](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1701.07875)》为 “WGAN本作”，简称《[Towards Principled Methods for Training Generative Adversarial Networks](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1701.04862)》为“WGAN前作”。*

*WGAN源码实现：[martinarjovsky/WassersteinGAN](https://link.zhihu.com/?target=https%3A//github.com/martinarjovsky/WassersteinGAN)*

## 第一部分：原始 GAN 究竟出了什么问题？

## 第二部分：WGAN 之前的一个过渡解决方案 

## 第三部分：Wasserstein 距离的优越性质

## 第四部分：从 Wasserstein 距离到 WGAN

## 第五部分：总结

WGAN 前作分析了 Ian  Goodfellow 提出的原始 GAN 两种形式各自的问题，第一种形式等价在最优判别器下等价于最小化生成分布与真实分布之间的 JS 散度，由于随机生成分布很难与真实分布有不可忽略的重叠以及 JS 散度的突变特性，使得生成器面临梯度消失的问题；第二种形式在最优判别器下等价于既要最小化生成分布与真实分布直接的 KL 散度，又要最大化其 JS 散度，相互矛盾，导致梯度不稳定，而且 KL 散度的不对称性使得生成器宁可丧失多样性也不愿丧失准确性，导致 collapse mode 现象。

WGAN 前作针对分布重叠问题提出了一个过渡解决方案，通过对生成样本和真实样本加噪声使得两个分布产生重叠，理论上可以解决训练不稳定的问题，可以放心训练判别器到接近最优，但是未能提供一个指示训练进程的可靠指标，也未做实验验证。

WGAN 本作引入了 Wasserstein 距离，由于它相对 KL 散度与 JS 散度具有优越的平滑特性，理论上可以解决梯度消失问题。接着通过数学变换将Wasserstein 距离写成可求解的形式，利用一个参数数值范围受限的判别器神经网络来最大化这个形式，就可以近似 Wasserstein 距离。在此近似最优判别器下优化生成器使得 Wasserstein 距离缩小，就能有效拉近生成分布与真实分布。WGAN 既解决了训练不稳定的问题，也提供了一个可靠的训练进程指标，而且该指标确实与生成样本的质量高度相关。作者对 WGAN 进行了实验验证。

## Reference

- [高赞 | 令人拍案叫绝的 Wasserstein GAN](https://zhuanlan.zhihu.com/p/25071913)