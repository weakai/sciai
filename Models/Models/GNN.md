# 图神经网络

## 知识点

### GCN 和 GNN的区别？

GCN 为图卷积神经网络，其为 GNN 的一种，区别主要在于采用卷积算子进行信息汇聚。

## 图嵌入

传统表示学习面临的挑战

- 高计算复杂度
- 低并行
- 框架少

传统基于拓扑的表示学习

- 使用简单的邻接矩阵，包含噪声和冗余信息

嵌入向量: 节点信息和边信息一同编码进嵌入向量

基于嵌入的表示

- 旨在学习低维空间中的密集和连续的节点表示，以减少噪声或冗余信息，并可以保留固有的结构信息。
- 作为模型的输入，比传统基于拓扑的表示学习更具有使用价值，且具有普遍性。

### 图嵌入的分类

1. network structure and properties preserving network embedding
   1. 什么是网络结构？比如说 triangle closure 是网络中链路形成的驱动力
2. network embedding with side information
   1. side information 包括
      1. node content or labels
      2. node and edge attributes in social networks,
      3. node types in heterogeneous networks
3. advanced information preserving network embedding

图嵌入的目的

1. 网络重构: 主流，研究丰富
2. 网络推理

> end-to-end solution:
> Directly designing a framework of representation learning for a particular target scenario is also known as an end-to-end solution

In general, network structures and properties are the fundamental factors that need to be considered in network embedding.
Meanwhile, side information on nodes and links, as well as advanced information from target problem is helpful to enable
the learned network embedding work well in real applications.

### 图嵌入的方法和模型

> Matrix Factorization:
> 
> 邻接矩阵一般用来表示网络的拓扑结构。网络嵌入旨在学习网络的低维空间，也就是寻找 low-rank 空间来表示网络。比如，SVD: 游戏

## GraphSAGE

GCN -> GraphSAGE(an inductive version of GCN)
improves traditional GCN by using different aggregator functions instead of only convolutional function

## 参考

- [A Survey on Network Embedding](https://ieeexplore.ieee.org/document/8392745/)