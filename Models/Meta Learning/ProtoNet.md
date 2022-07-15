---
title: Prototypical Networks
paper: https://arxiv.org/abs/1703.05175
code: https://github.com/jakesnell/prototypical-networks
stars: 0.9k
year: 2018
---

- Metric-based 的元学习
  - 利用了一些**人为度量的先验知识**，比如度量**类别与类别之间的距离**的一些指标（欧式距离或者余弦距离等）。
- Prototypical Network 晚于 Matching Network 算法设计上更加简单，更加容易理解一些。
- 该网络直接求解每个类别在浅层空间的向量表示的 `prototypes`，使用各个类别中所有样本的均值表示，也就是图中的 $c1,c2,c3$，那么一旦训练好一个好的空间映射关系（对图像来说，就是 CNN 网络），那么对于一个没有见过的样本 x，其类别由其在浅层空间距离最近的 `prototype` 的类别所决定。

![](https://pic4.zhimg.com/80/v2-bb5b6927b7bb3a81e0cca11ff77d5833_720w.jpg)

- 原型网络方法虽然结构设计比较简单，但是却能达到很好的效果。
- 且作者对于自己的方法做了很多数学分析，进行深入探究，值得我们学习。

## links

- [【Meta Learning】对Prototypical Network的解析](https://zhuanlan.zhihu.com/p/268824689)