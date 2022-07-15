---
title: 匹配网络
paper: https://arxiv.org/abs/1606.04080
github: https://github.com/gitabcworld/MatchingNetworks
stars: 0.3k
year: 2016
---

MAML 和 Peptile 的基本思想是一致的。主要通过一个个的 N-ways K-shots 的任务，学习适合模型**快速学习的初始化参数**，和直接将 pretrained 的参数进行迁移不同，该模型学习出来的初始化参数往往**收敛速度更快**。

本小节我们要介绍一个 Metric-Based 的元学习方法：Matching Network。该网络借鉴了基于深度学习**Metric Learning** 的部分思想，并使用**外部记忆**（LSTM 做一个简单的信息传递）来增强网络，提高了网络的学习能力。

本文的创新点主要有两点，

1. 在模型方面，作者借鉴了最近（2017年）在一些**注意力和外部记忆**方面的经验来搭建一个叫做 matching net 的网络；

2. 在训练方式上，和其他 meta-learning 一样训练的数据是一个个的任务（N-ways K-shots），而不是类似于 metric-learning 一样输入是**固定类别**的若干图片。

至于作者为什么选择外部记忆来搭建模型，个人理解可能是使用外部记忆来模拟人类在认知过程中的“经验积累”过程（也就是记忆）。

## 网络建模和解析

![](https://pic1.zhimg.com/80/v2-7cf6e85d0e84c1805a546ed65870b414_720w.jpg)

Matching Network 架构有**两个输入**：

1. 输入任务 S 是一个 N-way 1 shot 的任务（图上是4分类问题，每个类别 1 个图片，又叫做 4-way 1-shot task，其中 ![[公式]](https://www.zhihu.com/equation?tex=S%3D%7B%28x_%7Bi%7D%2Cy_%7Bi%7D%29%7D_%7Bi%3D1%7D%5E%7Bk%7D) （为图中红色框显示内容)
2. 需要预测类别的图片 ![[公式]](https://www.zhihu.com/equation?tex=%5Chat%7Bx%7D)

Matching Network 架构的**输出**为：

图片 ![[公式]](https://www.zhihu.com/equation?tex=%5Chat%7Bx%7D)的**预测类别** ![[公式]](https://www.zhihu.com/equation?tex=%5Chat%7By%7D) （对应输入任务S中的哪个 ![[公式]](https://www.zhihu.com/equation?tex=y_%7Bi%7D) )

那么 Matching Network 架构可以被建模为

![[公式]](https://www.zhihu.com/equation?tex=P%EF%BC%88%5Chat%7By%7D%7C%5Chat%7Bx%7D%2CS%EF%BC%89) 

其中 ![[公式]](https://www.zhihu.com/equation?tex=P%EF%BC%88.%EF%BC%89) 为Matching Network架构的**参数映射**，也就是上面提到的**注意力和外部记忆**。

## Links

- [[Meta-Learning]Matching Network 详解](https://zhuanlan.zhihu.com/p/260187183)

  