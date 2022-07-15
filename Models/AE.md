---
title: AE
subtitle: 自编码器
---

对先验数据分布进行建模

包括两部分：encoder and decoder

编码器将数据分布的高级特征映射到数据的低级表征，低级表征叫作本征向量（latent vector）。解码器吸收数据的低级表征，然后输出同样数据的高级表征。

![VAE visialization](https://image.jiqizhixin.com/uploads/wangeditor/9600b7f4-10a2-4757-8e89-dc81a91ea840/06859image%20(12).png)

VAE 与标准的自编码器的区别：对本征向量的约束

标准自编码器关注重建损失（reconstruction loss）

变分自编码器关注 latent vector 遵循特定的分布（unit Gaussian distribution），从而优化损失。因为损失函数中还存在其他项，因此要 trade-off 本征向量的分布与单位高斯分布的接近程度。由两个超参数$λ_1$ 和 $λ_2$ 来控制

## 参考

[【全】一文带你了解自编码器（AutoEncoder）](https://zhuanlan.zhihu.com/p/80377698)

[优秀 | 神经网络（neural network）的应用——自编码器（Autoencoder）](https://skyfaker.cc/2019/04/16/autoencoder/)





