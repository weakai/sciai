---
title: 1D-Convnet
---

另外一种处理 sequence 或者 timeseries 问题的方法就是使用 1 维的卷积网络，并且跟上 1 维度的池化层。
卷积或者池化的维度就是 timestep 的维度。
它可以学习到一些 local pattern，视它 window 大小而定。

优点就是简单，计算相比于 LSTM 要快很多，所以一种常用的做法就是:
1. 用 1D-Convnet 来处理简单的文本问题。比如说短肽
2. 把它和 LSTM 融合，利用 1D-Conv 轻量级，计算快的优点来得到低维度特征，然后再用 LSTM 进行学习。这对于处理 long sequence 非常有用，值得尝试。

