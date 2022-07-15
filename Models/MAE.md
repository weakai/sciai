---
title: MAE
---

[Masked Autoencoders Are Scalable Vision Learners 论文地址](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2111.06377.pdf)



恺明提出一种用于计算机视觉的可扩展自监督学习方案 Masked AutoEncoders(MAE)。所提MAE极为简单：对输入图像的随机块进行mask并对遗失像素进行重建。它基于以下两个核心设计：

- 我们设计了一种非对称编解码架构，其中解码器仅作用于可见块(无需mask信息)，而解码器则通过隐表达与 mask 信息进行原始图像重建；
- 我们发现对输入图像进行高比例 mask (比如75%) 可以产生一项重要且有意义的自监督任务。

![MAE](https://pic3.zhimg.com/80/v2-9275c286e6a291ddb60ccaa24ef59ad6_720w.jpg)

Transformer 已经逐渐成为 NLP 领域主要的深度学习模型。

在计算机视觉领域，卷积神经网络（CNN）一直占据主导地位。受 NLP 领域自注意力机制成功的启示，一些基于 CNN 的模型开始尝试通过空间或通道层面的额外自注意力层来捕获长程依赖，而另一些模型则试图用全局 [20] 或局部自注意块来彻底替代传统的卷积。虽然 Cordonnier 等人在理论上证明了自注意力块的有效性，但在主流基准上，这些纯注意力模型仍然比不上当前的 SOTA CNN 模型。



![Image](https://mmbiz.qpic.cn/mmbiz_jpg/KmXPKA19gW9qwVF5NUCPyic3FVoptPbAXblss1NkXnvpDGVAHxtbibsPNf2IZ8wEq0JXCczrpp0Bb8o3AQ9XcR0w/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## 用于分类的视觉 Transformer

Vision Transformer（ViT）最先在主流分类基准上达到了可以媲美传统 CNN 的性能。

![Image](https://mmbiz.qpic.cn/mmbiz_png/KmXPKA19gW9qwVF5NUCPyic3FVoptPbAXicFkxicw2kU6Fcv3QoYeLSspYA56dhlCNDtmM5MrMqwbRFG92GwG0eIA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

Transformer Enhanced CNN 方法，这些方法利用 Transformer 增强 CNN 骨干的长程依赖。

Transformer 在全局建模方面能力突出，但在早期阶段会忽略局部信息。

因此，CNN Enhanced Transformer 方法利用适当的卷积归纳偏置来增强 Transformer，而 Local Attention Enhanced Transformer 方法重新设计 patch 分区和注意力块，以增强 Transformer 的局部性并维持一个无卷积的架构。

此外，CNN 在性能和计算效率方面都受益于分层和深度结构。受此启发，研究者们提出了 Hierarchical Transformer 和 Deep Transformer 方法。前者用一个金字塔 stem 代替分辨率固定的柱状结构，后者可以防止注意力图过于平滑，并在较深的层中增加其多样性。此外，他们还回顾了目前可用的自监督方法。

对于分类任务，一个深度分层 Transformer 骨干可以有效降低计算复杂度，还能避免深层中的特征过于平滑。同时，早期卷积足以捕获低级特征，从而显著增强浅层的稳健性，降低计算复杂度。此外，卷积投影和局部注意力机制都可以提高 Transformer 的局部性。

## 用于检测的视觉 Transformer

用于目标检测的视觉 Transformer 可以分为两类：：作为颈部的 Transformer 和作为骨干的 Transformer。颈部检测器主要基于为 Transformer 结构指定的一个新表示，称为对象查询，即一组学习到的同等地聚合全局特征的参数。它们试图从加速收敛或提高性能的角度来解决最优融合范式。除了专门为检测任务设计的各种颈部外，一定比例的主干检测器也会考虑到特定的策略。最后，作者在表 II 和表 III 中比较了它们的性能，并分析了 Transformer 检测器的一些潜在改进。对于检测任务，Transformer 颈部得益于编码器 - 解码器结构，比只使用编码器的 Transformer 检测器计算更少。因此，解码器是必要的，但是由于收敛缓慢，它需要的 stack 极少。此外，稀疏注意力有利于降低计算复杂度，加速 Transformer 的收敛，而空间先验有利于提高 Transformer 的性能，稍微提高其收敛速度。

## 用于分割的视觉 Transformer

按照分割方式的不同，这些 Transformer 可以被分为两类：基于 patch 的 Transformer 和基于查询的 Transformer。后者可以进一步分解为带对象查询的 Transformer 和带掩码嵌入的 Transformer。对于分割任务，编码器 - 解码器 Transformer 模型可以通过一系列可学习的掩码嵌入将三个分割子任务统一为一个掩码预测问题。这种无框（box-free）方法在多个基准上实现了最新的 SOTA。此外，基于 box 的 Transformer 的特定混合任务级联模型被证明在实例分割任务中达到了更高的性能。

## 关于视觉 Transformer 的几个关键问题

### Transformer 是如何打通语言和视觉的？

Transformer 最初是为机器翻译任务而设计的。在语言模型中，句子中的每一个词都被看作表示高级、高维语义信息的一个基本单元。这些词可以被嵌入到低维向量空间表示中，叫做词嵌入。在视觉任务中，图像的每个像素都是低级、低维语义信息，与嵌入特征不匹配。因此，将 Transformer 用到视觉任务中的关键是建立图像到向量的转换，并保持图像的特点。例如，ViT[27]借助强松弛条件将图像转换为包含多个低水平信息的 patch 嵌入，Early Conv. [50] 和 CoAtNet [37] 利用卷积提取高级信息，同时降低 patch 的冗余特征。

### Transformer、自注意力和 CNN 之间的关系

从卷积的角度来看，其归纳偏置主要表现为局部性、平移不变性、权重共享和稀疏连接。这类简单的卷积核可以有效地执行模板匹配，但由于归纳偏置强，其上界要低于 Transformer。

从自注意力机制的角度来看，理论上，当给定足够数量的头时，它可以表示任何卷积层。这种 fully-attentional 操作可以交替地结合局部和全局层面的注意力，并根据特征之间的关系动态地生成注意力权重。尽管如此，它的实用性还是不如 SOTA CNN，因为其精度较低，计算复杂度较高。

从 Transformer 的角度来看，Dong 等人证明，当在没有短连接或 FFN 的深层上训练时，自注意力层表现出强大的「token uniformity」归纳偏置。结果表明，Transformer 由两个关键部分组成：一个聚合 token 之间关系的自注意力层；一个提取输入特征的 position-wise  FFN。虽然 Transformer 具有强大的全局建模能力，卷积可以有效地处理低级特征[37]，[50]，增强 Transformer 的局部性[45]，[70]，并通过填充（padding）来附加位置特征[48]，[49]，[102]。

### 不同视觉任务中的可学习嵌入



## 参考

[何恺明最新一作：简单实用的自监督学习方案MAE，ImageNet-1K 87.8%！](https://zhuanlan.zhihu.com/p/432614068)

[何恺明MAE大火之后，想梳理下视觉Transformer？这篇综述帮你梳理了100多个](https://mp.weixin.qq.com/s/QPaMWdKS-g1eBqYsf9NOWA)