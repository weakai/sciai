# Multi-Task

多任务学习（multi task learning）简称为 MTL

## 单任务学习 VS 多任务学习

![](https://pic2.zhimg.com/80/v2-fe9026815104589c22322fe33ae2a601_720w.jpg)

单任务学习（single task learning）：一个loss，一个任务，例如NLP里的情感分类、NER任务一般都是可以叫单任务学习。大部分的机器学习任务都属于单任务学习。

多任务学习：简单来说有多个目标函数 loss 同时学习的就算多任务学习。

多任务学习（Multitask Learning）是一种推导迁移学习方法，主任务（main tasks）使用相关任务（related tasks）的训练信号（training signal）所拥有的领域相关信息（domain-specific information），做为一直推导偏差（inductive bias）来提升主任务（main tasks）泛化效果（generalization performance）的一种机器学习方法。

多任务学习（multitask learning）产生的原因？

现在大多数机器学习任务都是单任务学习。对于复杂的问题，也可以分解为简单且相互独立的子问题来单独解决，然后再合并结果，得到最初复杂问题的结果。这样做看似合理，其实是不正确的，因为现实世界中很多问题不能分解为一个一个独立的子问题，即使可以分解，各个子问题之间也是相互关联的，通过一些共享因素或共享表示（share representation）联系在一起。把现实问题当做一个个独立的单任务处理，忽略了问题之间所富含的丰富的关联信息。多任务学习就是为了解决这个问题而诞生的。把多个相关（related）的任务（task）放在一起学习。这样做真的有效吗？答案是肯定的。多个任务之间共享一些因素，它们可以在学习过程中，共享它们所学到的信息，这是单任务学习所具备的。相关联的多任务学习比单任务学习能去的更好的泛化（generalization）效果。

## 共享表示

共享表示的目的是为了提高泛化（improving generalization），图2中给出了多任务学习最简单的共享方式，多个任务在浅层共享参数。MTL中共享表示有两种方式：

基于参数的共享（Parameter based）：比如基于神经网络的MTL，高斯处理过程。

基于约束的共享（regularization based）：比如均值，联合特征（Joint feature）学习（创建一个常见的特征集合）。

## 多任务学习与其他学习算法之间的关系

多任务学习（Multitask learning）是迁移学习算法的一种，迁移学习之前介绍过。定义一个一个源领域source domain和一个目标领域（target domain），在source domain学习，并把学习到的知识迁移到target domain，提升target domain的学习效果（performance）。

多标签学习（Multilabel learning）是多任务学习中的一种，建模多个label之间的相关性，同时对多个label进行建模，多个类别之间共享相同的数据/特征。

![](https://pic2.zhimg.com/80/v2-ac2579934ee805c8a7fbac8ff5cb3c31_720w.png)

多类别学习（Multiclass learning）是多标签学习任务中的一种，对多个相互独立的类别（classes）进行建模。这几个学习之间的关系如图5所示：

##  多任务学习的基本模型框架

通常将多任务学习方法分为：hard parameter sharing和soft parameter sharing。

![](https://pic2.zhimg.com/80/v2-cedb49f4bbf82f3f6c88a5f3840a6a91_720w.jpg)

**一个老当益壮的方法：hard parameter sharing**

即便是2021年，hard parameter sharing依旧是很好用的baseline系统。

**现代研究重点倾向的方法：soft parameter sharing**

## 多任务学习应用概述

基于神经网络的多任务学习，尤其是基于深度神经网络的多任务学习（DL based Multitask Learning），适用于解决很多 NLP 领域的问题，比如把词性标注、句子句法成分划分、命名实体识别、语义角色标注等任务，都可以采用 MTL 任务来解决。

## Links

- [收藏|浅谈多任务学习（Multi-task Learning）](https://zhuanlan.zhihu.com/p/348873723)