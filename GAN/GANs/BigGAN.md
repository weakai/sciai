---
title: BigGAN
date: 2022-7-22
tags: []
---

这是 GAN 中用于图像生成的最新进展。
一个谷歌的实习生和谷歌 DeepMind 部门的两名研究人员发表了一篇“Large Scale GAN Training for High Fidelity Natural Image Synthesis”的论文。
本文是来自 Heriot-Watt 大学的 Andrew Brock 与来自 DeepMind 的 Jeff Donahue 和 Karen Simonyan 合作的实习项目。
这些图像都是由 BigGAN 生成，正如你看到的，图像的质量足以以假乱真。
这是 GAN 首次生成具有高保真度和低品种差距的图像。
之前的最高初始得分为 52.52，BigGAN 的初始得分为 166.3，比现有技术（SOTA）好 100％。
此外，他们将 Frechet 初始距离（FID）得分从 18.65 提高到 9.6。
这些都是非常令人印象深刻的结果。
它最重要的改进是对**生成器的正交正则化**。

## Reference

- []()