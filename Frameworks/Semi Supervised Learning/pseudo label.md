# 伪标签技术

## 什么是伪标签技术

用于深度神经网络的简单高效的半监督学习方法。

伪标签的定义来自于半监督学习，半监督学习的核心思想是通过借助无标签的数据来提升有监督过程中的模型性能。

举个简单的半监督学习例子，我想去训练一个通过胸片图像来诊断是否患有乳腺癌的模型，但是专家标注一张胸片图像要收费，于是我掏空自己的钱包让专家帮我标注了 10 张胸片，可是我这 10 张图片又要划分训练集测试集，咋训练看着都要过拟合哇，这可咋办？

聪明的我问了问专家，说不标注的胸片要钱吗？专家一愣，不要钱，随便拿（此处忽略病人隐私的问题，单纯举例子）。于是我掏出 1 张标注的胸片，换了 10 张没标注的胸片，在专家还没缓过劲之前先溜了。

粗略来讲，伪标签技术就是利用在已标注数据所训练的模型在未标注的数据上进行预测，根据预测结果对样本进行筛选，再次输入模型中进行训练的一个过程。

伪标签先说第一个问题，假设我们现在有一个文本分类模型（先不用管分类模型是怎么来的以及怎么训练的），以及大量的无标注数据。我们现在使用文本分类模型对无标注数据进行预测，挑选softmax之后概率最大的那个类别为当前无标注数据对应的标签。因为是无标注数据而且我们模型准确不可能是百分之百，从而导致预测的这个标签我们并不清楚是不是精准，所以我们称之为"伪标签"。

## 怎么使用伪标签

“伪标签”可以帮助模型学习到无标注数据中隐藏的信息。

我们先来看模型的损失函数是如何定义的：

![](https://pic4.zhimg.com/v2-5742d01b99d62b1b6676d2cd61a7d3d3_b.jpg)

在 t steps 之前，只是在训练数据上进行训练。
随着模型的训练，无标注数据的损失函数权重在慢慢的增加。
简单来说，就是模型现在标注数据上进行训练，到一定 steps 之后，开始使用无标签数据的损失函数。

### 入门版

![](https://pic1.zhimg.com/80/v2-d8cbdbe575e45f2adf47b61c3eed3e9c_720w.jpg)

### 进阶版

![](https://pic1.zhimg.com/80/v2-eff00934a0659e6467bcf12d50eadb9c_720w.jpg)

### 创新版

![](https://pic1.zhimg.com/80/v2-ab5838db93e9dca013c95905017721d0_720w.jpg)

## 伪标签为啥有用

> 《Pseudo-Label : The Simple and Efficient Semi-Supervised Learning Method for Deep Neural Networks》

- 根据聚类假设（cluster assumption），这些概率较高的点，通常在相同类别的可能性较大，所以其 pseudo-label 是可信度非常高的。**（合理性）**
- 熵正则化是在最大后验估计框架内从未标记数据中获取信息的一种方法，通过最小化未标记数据的类概率的条件熵，促进了类之间的低密度分离，而无需对密度进行任何建模，通过熵正则化与伪标签具有相同的作用效果，都是希望利用未标签数据的分布的重叠程度的信息。**（有效性）**

![](https://pic1.zhimg.com/80/v2-ef7e5be86d84f6b2a855716fe08ae43c_720w.png)

在理论外，伪标签技术给人的第一感觉就是利用置信度高的样本来提升模型的拟合能力。在**聚类假设**及**熵正则化**的角度上，这是符合我们的感受的，这也使得使用这项技术变得自然而然。

**值得注意的是**：当场景不满足聚类假设 、熵正则化失效（**样本空间覆盖密集**）情况下，伪标签技术很有可能失效。在用之前判断适用条件，对症下药，才能将伪标签这把匕首的作用发挥出来。

#### 伪标签何时无效

错误的伪标签引入过多噪声，误导模型的学习。
过于自信的伪标签，没有带来新的信息，模型一直学习已知的知识，导致过拟合。
