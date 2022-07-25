---
title: f-GAN
date: 2022-7-22
---

Wasserstein GAN（简称WGAN），其影响力似乎达到了原版 GAN 的高度，在国内也有一篇与其影响力相当的博文——“令人拍案叫绝的 Wasserstein GAN”，不过在看这篇论文之前，还要推荐另外一篇论文“*f-GAN*”，这篇论文利用芬切尔共轭（Fenchel Conjugate）的性质证明了任何 ![[公式]](https://www.zhihu.com/equation?tex=f-Divergence) 都可以作为原先 GAN 中 ![[公式]](https://www.zhihu.com/equation?tex=KL-Divergence) （或者说 ![[公式]](https://www.zhihu.com/equation?tex=JS-Divergence) )的替代方案。

一种通用的 GAN 框架

请直接阅读 [生成对抗网络——FGAN 和 WGAN@2018](https://alberthg.github.io/2018/05/13/wgan/)

