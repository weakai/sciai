---
title: kNN 图
subtitle: k-Nearest Neighbour Graph
---

实际上是在经典的 kNN(k-Nearest Neighbor) 算法上增加了一步构图过程。

## kNN 图的用途

在计算流形上的测地线距离时，可以构造基于欧式距离的 kNN 图得到一个近似。原因很简单，我们可以把一个流形在很小的局部邻域上近似看成欧式的，也就是局部线性的。

这一点很好理解，比如我们所处的地球表面就是一个流形，在范围较小的日常生活中依然可以使用欧式几何。但是在航海、航空等范围较大的实际问题中，再使用欧式几何就不合适了，使用黎曼几何更加精确。

kNN 图还可用于异常点检测。在大量高维数据点中，一般正常的数据点会聚集为一个个簇，而异常数据点与正常数据点簇的距离较远。通过构建 kNN 图，可以快速找出这样的异常点。

## 如何构建 kNN 图

常见的方法一般有三类：
间分割树(space-partitioning trees)算法
局部敏感哈希(locality sensitive hashing)算法
邻居搜索(neighbor exploring techniques)算法

其中有名的是 k-d 树和随机投影树，均属于第一类算法

### k-d 树

kd 树（k-dimensional tree）是一个包含空间信息的二项树数据结构，它是用来计算 kNN 的一个非常常用的工具。如果特征的维度是 $D$，样本的数量是 $N$，那么一般来讲 kd 树算法的复杂度是 O(DlogN)O(DlogN)O(Dlog⁡N)，相比于穷算的 O(DN)O(DN)O(DN) 省去了非常多的计算量。

## LINE

LINE，即Large-scale Information Network Embedding，是唐建大神2015年的一项工作(_www’15_)。内容依旧很好很强大，而且代码是开源的。

一句话概括，LINE是“Embed Everything”思想在网络表示中的发扬光大。自从Mikolov开源word2vec以来，词向量(word embedding)的概念在NLP界可谓是火的一塌糊涂，embedding的概念更是快速渗透到其他各研究领域。entity embedding、relation embedding…等如雨后春笋般涌现，更是有人在Twitter上犀利的吐槽：


## 一只兔子帮你理解 kNN

**导语**：商业哲学家 Jim Rohn 说过一句话，“你，就是你最常接触的五个人的平均。”那么，在分析一个人时，我们不妨观察和他最亲密的几个人。同理的，在判定一个未知事物时，可以观察离它最近的几个样本，这就是 kNN（k最近邻）的方法。

### 简介

kNN（k-Nearest Neighbours）是机器学习中最简单易懂的算法，它的适用面很广，并且在样本量足够大的情况下准确度很高，多年来得到了很多的关注和研究。kNN 可以用来进行分类或者回归，大致方法基本相同，本篇文章将主要介绍使用 kNN 进行分类。

