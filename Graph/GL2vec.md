---
title: GL2vec
---

最近，人们提出了几种学习给定图数据集嵌入的技术。其中，Graph2vec的重要意义在于，它可以无监督地学习整个图的嵌入，这对图分类非常有用。

本文提出了一种改进 Graph2vec 的算法。首先，我们指出了Graph2vec的两个局限性
1. 无法处理边标签
2. Graph2vec 并不总是保留足够的结构信息来评估结构相似性，因为它在提取子图时捆绑了节点标签信息和结构信息。

我们的算法通过利用给定图的线图（边到顶点对偶图）克服了这些限制。具体地说，它补充了Graph2vec所遗漏的边标签信息或结构信息与线图的嵌入。我们的方法被称为GL2vec（Graph and Line Graph to vector），因为它将原始图的嵌入连接到相应的线图的嵌入。在实验上，对于许多基准数据集，GL2vec 在图形分类任务上比Graph2vec有了显著的改进。