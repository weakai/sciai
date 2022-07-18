---
title: MOCO triplet
date: 2022-7-18
---

## MoCov1

时间拨回到 19 年末，那时 NLP 领域的 Transformer 进一步应用于 Unsupervised representation learning，产生后来影响深远的 BERT 和 GPT 系列模型，反观 CV 领域，ImageNet 刷到饱和，似乎遇到了怎么也跨不过的屏障，在不同 task 之间打转，寻求出路。
就在 CV 领域停滞不前的时候，又是那个人 Kaiming He 带着 MoCo 横空出世，横扫了包括 PASCAL VOC 和 COCO 在内的 7 大数据集，至此，CV 拉开了 Self-Supervised 的新篇章，与 Transformer 联手成为了深度学习炙手可热的研究方向。

MoCo 主要设计了三个核心操作

- Dictionary as a queue
  - 正如我之前的文章中提到的，避免退化解最好的办法就是同时满足 alignment 和 uniformity，即需要 positive pair 和 negative pair。
  - 其中 uniformity 是为了不同 feature 尽可能均匀的分布在 unit hypersphere 上，为了更有效率的达到这个目的，一个非常直观的办法是增加每次梯度更新所包含的 negative pair(即batch size)，在 MoCo 之前有很多方法对如何增加 negative pair 进行了大量研究。
- Momentum update
  - 但是 MoCo 仅仅将 dictionary as a queue 的话，并不能取得很好的效果，是因为不同 epoch 之间，encoder 的参数会发生突变，不能将多个 epoch 的数据特征近似成一个静止的大 batch 数据特征，所以 MoCo 在 dictionary as a queue 的基础上，增加了一个 momentum encoder 的操作
- Shuffling BN
  - MoCo 还发现 ResNet 里的 BN 层会阻碍模型学习一个好的特征。
  - 由于每个 batch 内的样本之间计算 mean 和 std 导致信息泄露，产生退化解。
  - MoCo 通过多 GPU 训练，分开计算 BN，并且 shuffle 不同 GPU 上产生的 BN 信息来解决这个问题。

## MoCov2

MoCov2 在 MoCov1 的基础上，增加了 SimCLR 实验成功的 tricks，然后反超 SimCLR 重新成为当时的 SOTA，FAIR 和 Google Research 争锋相对之作，颇有华山论剑的意思。

## MoCov3

MoCov3 的出发点是 NLP 领域的 Unsupervised representation learning 使用的架构都是 Transformer 的，而 CV 领域的 Self-Supervised 还在使用 CNN 架构，是不是可以在 Self-Supervised 中使用 Transformer 架构呢？于是 MoCov3 继续探索 Self-Supervised+Transformer 的上限在哪里，有金融+计算机内味了。




## 参考

- [MoCo三部曲](https://zhuanlan.zhihu.com/p/365886585)