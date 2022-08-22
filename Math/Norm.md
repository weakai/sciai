---
title: 范数
date: 2022-08-5
tags: [范数]
---

## 向量范数

- 1-范数: 向量元素绝对值之和，x 到零点的曼哈顿距离
- 2-范数: 欧几里得范数，常用计算向量长度
- p-范数: 向量元素绝对值的p次方和的1/p次幂，表示x到零点的p阶闵氏距离
- inf-范数: 所有向量元素绝对值中的最大值
- -inf-范数: 所有向量元素绝对值中的最小值
- 0-范数: 向量非零元素的个数

## 矩阵范数

- 1-范数: 列和范数，即所有矩阵列向量绝对值之和的最大值
- 2-范数: 谱范数
- inf-范数: 行和范数，即所有矩阵行向量绝对值之和的最大值
- F-范数: Frobenius 范数，即矩阵元素绝对值的平方和再开平方

## PyTorch 实现

- [PyTorch 实现](https://pytorch.org/docs/stable/generated/torch.linalg.norm.html)



If dim is an int, the vector norm will be computed.
If dim is a 2-tuple, the matrix norm will be computed.
If dim=None and ord = None, A will be flattened to 1D and the 2-norm of the resulting vector will be computed.
If dim=None and ord != None, A must be 1D or 2D.

```python
torch.linalg.vector_norm() # computes a vector norm.
torch.linalg.matrix_norm() # computes a matrix norm.
torch.linalg.norm(A, ord=1, dim=(0, 1))  # torch.linalg.vector_norm(A, ord=1, dim=(0, 1))   
```


## Reference

- []()