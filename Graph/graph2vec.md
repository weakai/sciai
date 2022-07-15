---
title: graph2vec
---

## Graph Kernel 方法

Graph Kernel 方法将机器学习中的核方法（Kernel Methods）拓展到了图结构数据上，是一类计算图与图之间相似度的方法。再利用图核方法比较两个图的相似度时，需要将两个图用一定方法（如最小路径，随机游走，小图等）分解成更小的子结构，再通过定义一个核函数来的到两个图的相似度（如计算两个图中相似的子结构个数）

为什么是 Graph Kernels 而不是用 Graph Embedding？  
因为后者将结构化数据降维到向量空间时，损失了大量结构化信息。而前者直接面向图结构的数据，保留了核函数高效计算的优点，又包含了结构化的信息。

## 参考

[graph2vec：图级别的表示学习模型](https://dreamhomes.top/posts/202101181459/)

[读论文——graph2vec:图的分布式表示学习](https://zhuanlan.zhihu.com/p/228559209)