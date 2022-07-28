---
title: Pix2pix
date: 2022-7-22
tags: []
---

pix2pix 网络由 Phillip Isola，Jun-Yan Zhu，Tinghui Zhou 和 Alexei A. Efros 在他们的题为“Image-to-Image Translation with Conditional Adversarial Networks”的论文中提出。

无论是将夜间图像转换为白天的图像还是给黑白图像着色，或者将草图转换为逼真的照片等等，Pix2pix 在这些例子中都表现非常出色。

pix2pix 是用 cGAN 做图像到图像的翻译任务的鼻祖

Pix2Pix 模型解决了有 Pair 对数据的图像翻译问题；CycleGAN解决了Unpaired数据下的图像翻译问题。

#### pix2pix 的网络架构和目标函数

![](https://pic1.zhimg.com/80/v2-a5a31419c44aef2d6103e5cc0291c0dc_720w.jpg)

如上图所示，生成器 G 用到的是 Unet 结构，输入的轮廓图 $x$ 编码再解码成真是图片，判别器 D 用到的是作者自己提出来的条件判别器PatchGAN，判别器 D 的作用是在轮廓图 x 的条件下，对于生成的图片 $G(x)$ 判断为假，对于真实判断为真。

#### 为什么选择 Unet？

作者提到，输入和输出图像的外表面(surface appearance)应该不同而潜在的结构(underlying  structure)应该相似，对于image  translation的任务来说，输入和输出应该共享一些底层的信息，因此使用Unet这种跳层连接(skip  connection)的方法，这里说的跳层连接是 ![[公式]](https://www.zhihu.com/equation?tex=i) 层直接与 ![[公式]](https://www.zhihu.com/equation?tex=n-i) 层相加，如下所示：

![](https://pic2.zhimg.com/80/v2-3a4ef90d88c237bc242fa73b3b720cbd_720w.jpg)

#### 为什么选择 PatchGAN?

之前在介绍 AE 和 VAE 的时候有说，用 L1 和 L2  loss 重建的图像很模糊，也就是说 L1 和 L2 并不能很好的恢复图像的高频部分(图像中的边缘等)，但能较好地恢复图像的低频部分(图像中的色块)。为了能更好得对图像的局部做判断，作者提出 patchGAN 的结构，也就是说把图像等分成patch，分别判断每个 Patch 的真假，最后再取平均！作者最后说，文章提出的这个 PatchGAN 可以看成所以另一种形式的纹理损失或样式损失。在具体实验时，不同尺寸的 patch，最后发现 70x70 的尺寸比较合适。

#### 目标函数

按照之前介绍的GAN的基本原理，可以很容易写出object function:

![[公式]](https://www.zhihu.com/equation?tex=L_%7BcGAN%7D+%28G%2CD%29+%3D+%5Cmathbb%7BE%7D_%7Bx%2Cy%7D%5B%5Clog+D%28x%2Cy%29%5D%2B%5Cmathbb%7BE%7D_%7Bx%2Cz%7D%5B%5Clog+%281-D%28x%2CG%28x%2Cz%29%29%29%5D+%5C%5C) 

我们知道，G的作用是为了迷惑(fool)D，同时产生与ground truth很像的图像。因此，再加一个与ground truth的L1 loss（注意，这是个坑！！！我在后边的介绍中会说明这样做的致命缺点）:

![[公式]](https://www.zhihu.com/equation?tex=L_%7BL1%7D%28G%29%3D%5Cmathbb%7BE%7D_%7Bx%2Cy%2Cz%7D%5B%5Cparallel+y-G%28x%2Cz%29+%5Cparallel_1+%5D+%5C%5C) 

所以最终的目标函数为：

![[公式]](https://www.zhihu.com/equation?tex=G%5E%7B%2A%7D%3Darg%5Cmin_G+%5Cmax_D+L_%7BcGAN%7D%28G%2CD%29%2B%5Clambda+L_%7BL1%7D%28G%29+%5C%5C) 

之前已经反复多次提到，GAN其实是一种相对于L1 loss更好的判别准则或者loss。有时候单独使用GAN loss效果好，有时候与L1  loss配合起来效果好。在pix2pix中，作者就是把L1 loss 和GAN loss相结合使用，因为作者认为L1 loss  可以恢复图像的低频部分，而GAN loss可以恢复图像的高频部分。文章在实验部分也给出了相应的证明，如下所示：

![](https://pic2.zhimg.com/80/v2-a12486b03b008480eb76e811223f5379_720w.jpg)

只用L1 会模糊，只用GAN会shape，两个加起来正好

## Reference

- []()