---
title: reformer
---

LSH (locality-sensitive hashing attention)

- 解决了计算速度的问题

借鉴了 RevNet 的思想

- 解决内存消耗的问题

一个单层网络通常需要占用 GB 级别的内存，但是当我们训练一个多层模型时，需要保存每一层的激活值和隐变量，以便在反向传播时使用。这极大地提高了内存的占用量。Reformer不保留中间残差连接部分的输入了，取而代之的，是应用一种“可逆层”的思路。

