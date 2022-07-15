---
title: 降维
subtitle: Dimensionality Reduction
---

降维是数据去噪简化的一种方法

由于“维度灾难”(curse of dimensionality)的存在，很多统计方法难以应用到高维数据上。虽然收集到的数据点很多，但是它们会散布在一个庞大的、几乎不可能进行彻底探索的高维空间中。

把数据或特征的维数降低，一般分为线性降维和非线性降维，比较典型的如下：

- 线性降维：PCA(Principal Components Analysis)、LDA(Linear Discriminant Analysis)、MDS(Classical Multidimensional Scaling)
- 非线性降维：Isomap(Isometric Mapping)、LLE(Locally Linear Embedding)、LE(Laplacian Eigenmaps) 大家可能对线性降维中的一些方法比较熟悉了，但是对非线性降维并不了解，非线性降维中用到的方法大多属于流形学习方法。

## 降维技术

数据维度的降低方法主要有两种：

-   仅保留原始数据集中最相关的变量（特征选择）。
-   寻找一组较小的新变量，其中每个变量都是输入变量的组合，包含与输入变量基本相同的信息（降维）。

## t 分布

tt分布比正态分布要“胖”一些，尤其在尾部两端较为平缓。tt分布是一种典型的长尾分布。实际上，在[稳定分布](https://en.wikipedia.org/wiki/Stable_distribution)家族中，除了正态分布，其他均为长尾分布。长尾分布有什么好处呢？在处理小样本和一些异常点的时候作用就突显出来了。下文介绍 t-sne  算法时也会涉及到 t 分布的长尾特性。

## kNN 图

kNN 图 (k-Nearest Neighbour Graph) 实际上是在经典的 kNN(k-Nearest Neighbor) 算法上增加了一步构图过程。

## k-d 树与随机投影树

刚才说到 kNN 图在寻找流形的过程中非常有用，那么如何来构建一个 kNN 图呢？常见的方法一般有三类：第一类是空间分割树(space-partitioning trees)算法，第二类是局部敏感哈希(locality sensitive hashing)算法，第三类是邻居搜索(neighbor exploring techniques)算法。其中 k-d 树和随机投影树均属于第一类算法。

很多同学可能不太熟悉随机投影树(Random Projection Tree)，但一般都听说过 k-d 树。k-d 树是一种分割k维数据空间的数据结构，本质上是一棵二叉树。主要用于多维空间关键数据的搜索，如范围搜索、最近邻搜索等。那么如何使用 k-d 树搜索 k 近邻，进而构建kNN图呢？

## 参考

[从 SNE 到 t-SNE 再到 LargeVis | 4](https://bindog.github.io/blog/2016/06/04/from-sne-to-tsne-to-largevis/)
[12 种降维方法终极指南（含Python代码）| 5](https://zhuanlan.zhihu.com/p/43225794)
