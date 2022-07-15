---
title: Active Learning Sampling Strategy
---

## 主动学习的基本策略

1. 随机采样策略(Random Sampling，RS)

## 不确定性策略(Uncertainty Strategy，US)

- US 假设最靠近分类超平面的样本相对分类器具有较丰富的信息量，根据当前模型对样本的预测值筛选出最不确定的样本。

### 最小置信度：（Least confidence)

将预测概率的最大值的相反数作为样本的不确定性分数

最小置信度 = 1（100％置信度）和每个项目的最自信的标签之间的差异。

$$
S_{LC} = \underset{x}{\mathrm{argmin}}(1-P(\hat y | x)) \\
\hat y = \underset{y}{\mathrm{argmin}}(P(y|x))
$$

最小置信度是最简单，最常用的方法，它提供预测顺序的排名，这样可以以最低的置信度对其预测标签进行采样。

### 边缘采样(Margin Sampling，MS)

MS 认为距离分类超平面越近的样本具有越高的不确定性，**常与 SVM 结合并用于解决二分类任务，但在多分类任务上的表现不佳。**

- 多类别不确定采样(Multi-Class Level Uncertainty，MCLU)是 MS 在多分类问题上的扩展，MCLU  选择离分类界面最远的两个样本，并将它们的距离差值作为评判标准。MCLU  能够在混合类别区域中筛选出最不确信度的样本，如式(2.3)所示。其中，xj 表示被选中的样本，C 表示样本 xi 所属的类别集合，c+  表示最大预测概率对应的类别，f (xi, c) 表示样本 xi 到分类超平面的距离。
- 熵值最大化(Maximize Entropy，ME)优先筛选具有更大熵值的样本，熵值可以通过计算![img](https://image.jiqizhixin.com/uploads/editor/04279ed7-1272-4288-94c8-5b861d54482c/640.svg)得到，其中 pi 表示第 i 个类别的预测值。
- 样本最优次优类别(Best vs Second Best, BvSB)[79]主要是针对多分类问题的一种衡量指标，并且能够缓解  ME 在多分类问题上效果不佳的情况。BvSB 只考虑样本预测值最大的两个类别，忽略了其他预测类别的影响，从而在多分类问题上的效果更佳。

- 