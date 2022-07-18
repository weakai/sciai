---
title: SimCLR
subtitle: A Simple Framework for Contrastive Learning of Visual Representations
paper: https://arxiv.org/abs/2002.05709
github: https://github.com/google-research/simclr
starts: 3k
---

今年 (2020年) Google 发布了SimCLR，与 Facebook AI 团队 (FAIR) 提出的 MoCo 都是近年 self-supervised learning 的重要里程碑。

Google Brain 团队的 SimCLR 在 ImageNet 的分类问题上展示了 SimCLR 训练出的线性分类器达到了 76.5％ 的 top-1 准确性，相对于以前的最新技术，相对改进了 7％，与监督的 ResNet-50 的性能相匹配。当仅对 1％ 的标签进行微调时，我们可以达到 85.8％ 的 top-5 精度，其标签数量减少了 100 倍，超过了 AlexNet。

SimCLR 这篇论文不仅仅是简单易懂的方法，更是一个认识 self-supervised learning 与 contrastive learning 的大门。

## Links

- [ICML 2020 | SimCLR](https://zhuanlan.zhihu.com/p/197802321)
- [【他山之石】SimCLR：用于视觉表征的对比学习框架](https://toutiao.io/posts/s1ncqgr/preview)