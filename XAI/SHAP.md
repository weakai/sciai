---
title: SHAP
date: 2022-4-17
---

SHAP，它是一个框架，可以更好地理解为什么机器学习模型会那样运行。

事实证明，Shapley 值已经存在了一段时间，它们最早起源于 1953 年的博弈论领域，目的是解决以下情况：

> 一群拥有不同技能的参与者为了集体奖励而相互合作。那么，如何在小组中公平分配奖励？

SHAP 是 Python 开发的一个"模型解释"包，可以解释任何机器学习模型的输出。
其名称来源于 SHapley Additive exPlanation，在合作博弈论的启发下SHAP构建一个加性的解释模型，所有的特征都视为“贡献者”。
对于每个预测样本，模型都产生一个预测值，SHAP value 就是该样本中每个特征所分配到的数值。

## Basic

### 排列重要性

- [Permutaion Importance —— 排列重要性 | 翻译自 kaggle course | by 冰焰虫子](https://iceflameworm.github.io/2019/08/17/permutaion-importance/)

### 部分依赖图 PDP Figure

跟排列重要性一样，部分依赖图也是要在拟合出模型之后才能进行计算。
模型是在真实的未加修改的真实数据上进行拟合的。

2D 部分依赖图: 如果你对特征之间的相互作用感兴趣的话，2D部分依赖图就能排得上用场了。

- [Partial Dependence Plots —— 部分依赖图](https://iceflameworm.github.io/2019/08/28/partial-plots/)

## Usage

Note:

1. shap 强调的是单样本解释，每个样本各个特征的贡献度，和多少数据哪个数据没有关系。模型训练好以后 shap 的参数就固化了
2. shap 是 toy model friendly?

## Links

### Resources

- [Shape GitHub](https://github.com/slundberg/shap)

### 入门

- [能不能形象的介绍一下 shapley 值法？](https://www.zhihu.com/question/23180647/answer/152293880)
- [SHAP 模型解释三部曲之相识篇](https://zhuanlan.zhihu.com/p/454243793)

### 实战

- [SHAP: Python的可解释机器学习库](https://zhuanlan.zhihu.com/p/83412330)
- [特征重要度和 SHAP 值](https://zhuanlan.zhihu.com/p/163764368)