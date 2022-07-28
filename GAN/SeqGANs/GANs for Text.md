---
title: GANs for Text
date: 2022-07-25
tags: [GANs for Text]
---

- 目前针对GAN在文本生成领域的应用，主要有以下几种 GAN 改进思路：

- 微调 GAN 的结构    
  - 使用 Wasserstein-divergence 进行距离衡量
  - 通过 Gumbel-Softmax 估计（对 softmax 的一种连续化的估计）
- 通过强化算法和策略梯度（Policy Gradient）（基于强化学习的）
- 生成器结果在连续空间中传递给判别器（避免在离散空间交互）

以下分别介绍上述三种方案。

## Reference

- [调研：GAN用于文本生成](http://shihanmax.top/2021/04/09/%E6%96%87%E6%9C%ACGAN/)

