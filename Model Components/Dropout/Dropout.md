---
title: Dropout
---

## Introduction

Dropout 是一类用于神经网络训练或推理的随机化技术，这类技术已经引起了研究者们的广泛兴趣，并且被广泛地应用于神经网络正则化、模型压缩等任务。虽然  Dropout 最初是为密集的神经网络层量身定制的，但是最近的一些进展使得 Dropout 也适用于卷积和循环神经网络层。

关于 Dropout 的发展史，请阅读[此文](https://www.jiqizhixin.com/articles/2019-05-18-2)

### 标准的 Dropout

- 避免前馈神经网络中出现的过拟合现象提供了一种简单的技术

- 在每轮迭代中，网络中的每个神经元以 p 的概率被丢弃

### 用于训练的 Dropout 方法

- 寻求提高其速度或正则化的有效程度

- 使用了一个 Dropout mask 矩阵，而不是 mask 向量

- Standout

  - 一种试图通过**自适应**地选择待丢弃的神经元（而不是随机丢弃）来改进标准 Dropout的 Dropout 方法
  - 通过在神经网络上叠加一个控制神经网架构的二值信念网络实现的

- Fast Dropout

  - 通过从贝叶斯的角度解释 Dropout 方法，提供了一种更快速地进行类似于 Dropout 的正则化的方式
  - 带有 Dropout 的网络层的输出可以被看做是从一个潜在的分布（可以近似为高斯分布）中进行采样

- 变分 Dropout

### 卷积层

- 朴素 Dropout

  - 在特征图或输入图像中随机地丢弃像素
- 最大池化 Dropout(Max-Pooling Dropout)

  - 一种保留了最大池化层的行为的方法，它同时也以一定概率让其它的特征值可以影响池化层的输出。在执行最大池化操作前，算子对特征值的某个子集进行 mask 运算。 
- Cutout

  - 一种基于Dropout 的用于训练 CNN 的正则化和数据增强方法
- Spatial Dropout

### 循环层

因为在每一个时间步上由 Dropout 引起的噪声让网络难以保留长期的记忆，将标准 Dropout 应用于循环连接的效果很差

- 专门为循环层设计的 Dropout
- RNNDrop@2015
- RNN Dropout 变体
  - 基于一种对 Dropout 方法的贝叶斯化的解释
- 循环 Dropout

### **用于模型压缩的 Dropout 方法** 

标准 Dropout 加大了神经网络权值的稀疏性。这一特性意味着 Dropout 方法可以通过减少有效执行所需的参数数量来压缩神经网络模型。开发用于模型压缩的 Dropout 方法是一个十分活跃的研究领域。

- 变分 Dropout
- Targeted Dropout
  - 神经元被自适应地选择，并以使网络适应神经剪枝的方式被丢弃，在不过多损失准确率的情况下大幅度缩小网络规模。
- Ising-dropout
- Stochastic Depth
- DropBlock
- 蒙特卡罗 Dropout
  - 一种从贝叶斯理论出发的 Dropout 理解方式，并且被广泛接受。
  - 他们将 Dropout 解释为深度高斯过程的贝叶斯近似。
  - **该方法还提供了一种估计神经网络输出置信度的简单方法,蒙特卡罗Dropout 在模型的不确定性估计中得到了广泛的应用。**

## Drop 原理

具体实现和原理，请阅读[此文](https://www.cnblogs.com/zingp/p/11631913.html)

### Dropout 为什么可以防止过拟合？

1. 取平均的作用 
2. 减少神经元之间复杂的共适应关系
3. Dropout 类似于性别在生物进化中的角色

## Links



