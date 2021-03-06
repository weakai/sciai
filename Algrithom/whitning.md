---
title: whitening
---

## 0 基础

### 0.1 PCA 与白化

白化，听起来高大上，却是一个比 PCA 稍微高级一点的算法。

其实我们之前学的 PCA 算法中，可能 PCA 给我们的印象是一般用于降维操作。
然而其实 PCA 如果不降维，而是仅仅使用 PCA 求出特征向量，然后把数据 $X$ 映射到新的特征空间，这样的一个映射过程，其实就是满足了我们白化的第一个性质： 除去特征之间的相关性。因此白化算法的实现过程，第一步操作就是 PCA，求出新特征空间中 $X$ 的新坐标，然后再对新的坐标进行方差归一化操作。

## 1 概述

### 1.1 白化的目的

白化的目的是去除输入数据的冗余信息。假设训练数据是图像，由于图像中相邻像素之间具有很强的相关性，所以用于训练时输入是冗余的。

输入数据集X，经过白化处理后，新的数据 $X^\prime$ 满足两个性质：

1. 特征之间相关性较低
2. 所有特征具有相同的方差

### 1.2 白化的分类

#### 1.2.3 PCA 白化

通过PCA将数据X降维以后得到Z，可以看出Z中每一维都是独立的(去相关)，这时再除Z中每一维对应的方差，得到每一维方差均为1**。如果k<n，就是PCA降维，如果k=n，则降低特征间相关性。**这里的k是指PCA处理过后的特征数。

#### 1.2.4 ZCA 白化

只是在**PCA白化的基础上做了一个旋转操作，使得白化之后的数据更加接近原始数据**。ZCA 白化首先通过PCA去除了各个特征相关性，然后是输入特征具有单位方差，此时得到PCA白化后的结果，然后再将数据旋转回去，得到ZCA白化结果

## 参考

- [机器学习（七）白化whitening](https://blog.csdn.net/hjimce/article/details/50864602)
- [深度学习之8——数据预处理](https://zhuanlan.zhihu.com/p/69439309)
