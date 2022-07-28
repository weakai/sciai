---
title: CycleGAN
date: 2022-7-22
tags: []
---

CycleGAN 有一些非常有趣的用例，例如将照片转换为绘画，将夏季拍摄的照片转换为冬季拍摄的照片，或将马的照片转换为斑马照片，或者相反。
CycleGANs 由 Jun-Yan Zhu，Taesung Park，Phillip Isola 和 Alexei A. Efros 在题为“Unpaired Image-to-Image Translation using Cycle-Consistent Adversarial Networks”的论文中提出。
CycleGAN 用于不同的图像到图像翻译。

![](https://pic1.zhimg.com/80/v2-bf37ac016da8f41dd6941220b8f167c8_720w.jpg)

许多名画造假者费尽毕生的心血，试图模仿出艺术名家的风格。如今，CycleGAN 就可以初步实现这个神奇的功能。

![](https://pic1.zhimg.com/80/v2-a0b491f3983739c6aadb892254d34b00_720w.jpg)

这属于是无配对数据（unpaired）产生的图片，也就是说你有一些名人名家的作品，也有一些你想转换风格的真实图片，这两种图片是没有任何交集的。在之前的文章（用AI增强人类想象力）中提到的 Pix2Pix 方法的关键是提供了在这两个域中有相同数据的训练样本。CycleGAN 的创新点在于能够在源域和目标域之间，无须建立训练数据间一对一的映射，就可实现这种迁移想要做到这点，有两个比较重要的点，第一个就是双判别器。如上图 a 所示，两个分布 X,Y，生成器 G，F 分别是 X 到 Y 和 Y 到 X 的映射，两个判别器 Dx,Dy 可以对转换后的图片进行判别。第二个点就是 cycle-consistency loss，用数据集中其他的图来检验生成器，这是防止 G 和 F 过拟合，比如想把一个小狗照片转化成梵高风格，如果没有cycle-consistency loss，生成器可能会生成一张梵高真实画作来骗过 Dx，而无视输入的小狗。

CycleGAN的一个重要应用领域是Domain Adaptation（域迁移：可以通俗的理解为画风迁移），比如可以把一张普通的风景照变化成梵高化作，或者将游戏画面变化成真实世界画面等等。

#### CycleGAN 的优势

其实在 CycleGAN 之前，就已经有了 Domain Adaptation 模型，比如 Pix2Pix ，不过Pix2Pix要求训练数据必须是成对的，而现实生活中，要找到两个域(画风)中成对出现的图片是相当困难的，因此CycleGAN诞生了，它只需要两种域的数据，而不需要他们有严格对应关系，这使得 CycleGAN 的应用更为广泛。

#### 核心思想及其 Loss 函数

CycleGAN的主要目的是实现Domain Adaptation，这里我们以风景照片和梵高画作为例，假设现在有两个数据集 ![[公式]](https://www.zhihu.com/equation?tex=X) 和 ![[公式]](https://www.zhihu.com/equation?tex=Y) 分别存放风景照片和梵高画作。我们希望训练出一个生成器 ![[公式]](https://www.zhihu.com/equation?tex=G) ，它吃一个风景照，吐出一个梵高画作，即 ![[公式]](https://www.zhihu.com/equation?tex=G%28x%29%3Dy%27%2C+x%E2%88%88X) ；同时，我们还希望训练出另一个生成器 ![[公式]](https://www.zhihu.com/equation?tex=F) ，它吃一个梵高画作，吐出一个风景照，即 ![[公式]](https://www.zhihu.com/equation?tex=G%28y%29%3Dx%27%2C+y%E2%88%88Y) 。为了达到这个目的，我们还需要训练两个判别器 ![[公式]](https://www.zhihu.com/equation?tex=D_X)和![[公式]](https://www.zhihu.com/equation?tex=D_Y) ，分别判断两个生成器生成图片的好坏：如果生成器产生的**图片** ![[公式]](https://www.zhihu.com/equation?tex=y%27) 不像**数据集** ![[公式]](https://www.zhihu.com/equation?tex=Y) 里的**图片** ![[公式]](https://www.zhihu.com/equation?tex=y) ，此时判别器![[公式]](https://www.zhihu.com/equation?tex=D_Y)应给它低分（规定最低分为0），反之如果**图片** ![[公式]](https://www.zhihu.com/equation?tex=y%27) 像**数据集** ![[公式]](https://www.zhihu.com/equation?tex=Y) 里的**图片** ![[公式]](https://www.zhihu.com/equation?tex=y)，则此时判别器![[公式]](https://www.zhihu.com/equation?tex=D_Y)应给他高分（规定最高分为1）。此外，判别器![[公式]](https://www.zhihu.com/equation?tex=D_Y)应该永远给**真实图片** ![[公式]](https://www.zhihu.com/equation?tex=y) 高分。对于判别器![[公式]](https://www.zhihu.com/equation?tex=D_X)也是同理。整个CycleGAN的模型框架如下图所示：

![](https://pic2.zhimg.com/80/v2-ff3999df8ceb91013149ce7e8b295119_720w.jpg)

在训练过程中，判别器和生成器是分别训练的，整个过程有点像进化论中捕食者和被捕食者迭代进化的过程：

当我们固定住生成器的参数训练判别器时，判别器便能学到更好的判别技巧，当我们固定住判别器参数训练生成器时，生成器为了骗过现在更厉害的判别器，被迫产生出更好质量的图片。两者便在这迭代学习的过程中逐步进化，最终达到动态平衡。

 这是一开始GAN被提出时的思想，而在CycleGAN中，我们不仅想要生成器产生的**图片**![[公式]](https://www.zhihu.com/equation?tex=y%27)跟**数据集**![[公式]](https://www.zhihu.com/equation?tex=Y)中的图片**画风一样**，我们还希望生成器产生的**图片**![[公式]](https://www.zhihu.com/equation?tex=y%27)跟他的输入**图片** ![[公式]](https://www.zhihu.com/equation?tex=x) **内容一样**（比如我们输入一张带房子的照片，我们希望产生梵高画风的房子图片，而不是其他内容的图片，比如向日葵、星夜等等，否则意义就不是很大了。因为在一些APP中，我们通常想把自己拍摄的照片变成梵高画作嘛~）。

为了做到这一点，论文作者提出了**Cycle Consistency Loss**，即将**图片**![[公式]](https://www.zhihu.com/equation?tex=y%27)再放入生成器 ![[公式]](https://www.zhihu.com/equation?tex=F) 中，产生的**新图片** ![[公式]](https://www.zhihu.com/equation?tex=x%27) 和最开始的**图片** ![[公式]](https://www.zhihu.com/equation?tex=x) 尽可能的相似

即我们希望 ![[公式]](https://www.zhihu.com/equation?tex=F%28G%28x%29%29%3Dx) 

有了这样的约束，我们才能希望生成器将我们的输入照片的风格转化为梵高画风，而不是从梵高的作品中随便挑一张图片来应付我们了事。**Cycle Consistency Loss**在原论文中的插图解释为

![](https://pic1.zhimg.com/80/v2-f3beda73aadd7815cd2e868c5e292974_720w.jpg)

下图很清晰的解释了 CycleGAN 中的 Loss 构成：

![](https://pic2.zhimg.com/80/v2-69b59087eef33a842603bc00f0318929_720w.jpg)

![](https://pic2.zhimg.com/80/v2-735fd768da36e9089fd600bd958a269d_720w.jpg)

#### 一些值得注意的细节

- 图片使用了**Instance Normalization**而非经典DCGAN中所使用的**Batch Normalization**
- 写代码时，并没有使用上面Loss中的 **log likelihood** 形式，而是使用的**least-squares loss**
- 判别器采用的**70×70 PatchGAN**形式
- 生成器网络使用了**residual blocks**
- 训练时的**Batch Size**为1。这一点我一直很困惑，不知道是因为当时显存限制，还是说在风格迁移里，就应该一张图一张图地训练，欢迎讨论！
- **学习率**在前100个epochs不变，在后面的epochs**线性衰减**
- 使用了**Reflection padding**而非普通的Zero padding
- 生成器各层**激活函数**主要为ReLU，判别器各层激活函数主要为LeakyReLU
- 训练判别器时还会用到生成器产生的**历史数据**

## Reference

- []()