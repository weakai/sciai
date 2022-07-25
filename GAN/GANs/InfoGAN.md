---
title: InfoGAN
date: 2022-7-22
tags: [GAN, InfoGAN]
---

NIPS 2016

它应该是 CGAN 的升级版

原始 GAN 训练太自由，只能生成真实的数据，不能生成我们想要的某一种类型的数据。加入条件约束 ![[公式]](https://www.zhihu.com/equation?tex=y) 引导模型训练， ![[公式]](https://www.zhihu.com/equation?tex=y) 可以是任何种类的辅助信息。

InfoGAN 使用无监督的方法，学习可解释的特征，产生不同属性的数据。

InfoGAN 中，生成器 $G$ 的输入有两部分，噪声 $z$ 和隐含变量 $c$，其中 ![[公式]](https://www.zhihu.com/equation?tex=c%3D%5Cleft+%5C%7B+c_%7B1%7D%2C+c_%7B2%7D...c_%7BL%7D%5Cright+%5C%7D) ，并最大化 $c$ 和 $G(z,c)$ 的互信息。

**c 就是增加了一个约束，对系统的动力学行为进行了约束，减小了解空间。**

#### Why InfoGAN

GAN 作为一个非常成功的深度学习模型被大量学者研究，而原始的 GAN 由于其生成网络 G 输入的信息为服从某分布的随机数，导致其生成的数据有一个巨大痛点：没错 GAN 的确可以生成以假乱真的数据，然而如何控制其输出具有我们想要特征的数据又该怎么办呢？

于是便有了 InfoGAN 。

#### InfoGAN 做了哪些改进？

既然原始 GAN 中的生成网络 G 输入的难以捉摸的随机数向量（当然，其实它们是服从某一分布的），那么如果我们把输入 G 的向量分成“固定”的和“随机”的：

1. 固定的部分 C：用于表示生成数据的某种特征，可以是离散的特征（比如在 mnist 数据集中图片代表的数字），也可以是连续的特征（比如图片中数字笔画的粗细、角度）。
2. 随机数 Z：和原始 GAN 中相同，代表噪音。

升级了损失函数，且加入两个可调的参数。

#### motivation

在标准的 GAN 中，生成数据的来源一般是一段连续单一的噪声 **z**，这样带来的一个问题是，Generator 往往会将 **z** 高度耦合处理，我们无法通过控制 **z** 的某些维度来控制生成数据的语义特征，也就是说，**z** 是不可解释的。比如，假设我们打算生成像 MNIST 那样的手写数字图像，每个手写数字可以分解成多个维度特征：代表的数字、倾斜度、粗细度等等，在标准 GAN 的框架下，我们无法在上述维度上具体指定 Generator 生成什么样的手写数字。

为了解决这一问题，文章对 GAN 的目标函数进行了一些小小的改进，成功让网络学习到了可解释的特征表示（即论文题目中的 interpretable representation）。

#### latent code

这个 trick 的关键是 latent code 的应用。

既然原始的噪声是杂乱无章的，那就人为地加上一些限制，于是作者把原来的噪声输入分解成两部分：一是原来的 **z**；二是由若干个 latent variables 拼接而成的 latent code c，这些 latent variables 会有一个先验的概率分布，且可以是离散的或连续的，用于代表生成数据的不同特征维度，比如 MNIST 实验的 latent variables 就可以由一个取值范围为 0-9 的离散随机变量（用于表示数字）和两个连续的随机变量（分别用于表示倾斜度和粗细度）构成。

但仅有这个设定还不够，因为 GAN 中 Generator 的学习具有很高的自由度，它很容易找到一个解，使得

![img](https://pic2.zhimg.com/80/v2-35d137422e3d423f6ff18ca45ee4075d_720w.jpg)

，导致 c 完全不起作用。

#### mutual information

作者从信息论中得到启发，提出了基于互信息（**mutual information**）的正则化项。c 的作用是对生成数据的分布施加影响，于是需要对这两者的关系建模，在信息论中，互信息 $I(X;Y)$ 用来衡量“已知 $Y$ 的情况下可获取多少有关 $X$ 的信息”，计算公式为：



![img](https://pic2.zhimg.com/80/v2-0cc246021ab057524e159a2f991aa9b5_720w.jpg)



其中*H*表示计算entropy，*H(X|Y)*衡量的是“给定*Y*的情况下，*X*的不确定性”。从公式可以得知，当*X*和*Y*毫无关联时，*H(X|Y)* = *H(X)*，*I(X;Y)* = 0，取得最小值；当*X*和*Y*有明确的关联时，已知*Y*时，*X*没有不确定性，因而*H(X|Y)* = 0，此时*I(X;Y)*取得最大值。

所以，现在优化目标就很明确了：要让 c 和 Generator 分布 G(z, c) 的互信息达到最大值。现在目标函数可改写为：

![img](https://pic1.zhimg.com/80/v2-b13912949afabde5f7b0af9e03bcc7c0_720w.jpg)

#### Variational Mutual Information Maximization

上述的推导看似已经很充足了，然而上面互信息的计算涉及后验概率分布 $P(c|x)$，而后者在实际中是很难获取的，所以需要定义一个辅助性的概率分布 $Q(c|x)$，采用一种叫作 Variational Information Maximization 的技巧对互信息进行下界拟合

## 网络模型概览

![](https://pic3.zhimg.com/80/v2-7835437fabca5e68e4ba1e27be16db1a_720w.jpg)

网络是基于 DCGAN（Deep Convolutional GAN）的，G 和 D 都由 CNN 构 成。在此基础上，Q 和 D 共享卷积网络，然后分别通过各自的全连接层输出不同的内容：Q 输出对应于 fake data 的 c 的概率分布，D 则仍然判别真伪。

## References

- [PaperWeekly@2018 | 经典论文复现 | InfoGAN：一种无监督生成方法](https://zhuanlan.zhihu.com/p/47850287)
- [InfoGAN ：从混沌中分离有序](https://zhuanlan.zhihu.com/p/73324607)