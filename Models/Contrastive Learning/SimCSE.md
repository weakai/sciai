---
title: SimCES
github: https://github.com/princeton-nlp/SimCSE
paper: https://arxiv.org/abs/2104.08821
stars: 
---

本文将对比学习的思想引入了 sentence embedding，刷爆了无监督与有监督语义相似度计算任务 SOTA，是一篇非常难得的高水平论文。

## 前言

SimCSE 的全称是 *Simple Contrastive Learning of Sentence Embeddings*，**S 代表 Simple**。文中的方法完全对得起题目，它是真的**简单**！简单在哪儿呢？

1. 它简单地**用 dropout 替换了传统的数据增强方法**，将同一个输入 dropout 两次作为对比学习的正例，而且效果甚好。
2. 它简单地将 NLI 的数据用于监督对比学习，效果也甚好。

**SimCSE** 有两个变体：**Unsupervised SimCSE**和**Supervised SimCSE**，主要不同在于对比学习的**正负例的构造**。

### 对比学习和度量学习？

度量学习和对比学习的思想是一样的，都是去拉近相似的样本，推开不相似的样本。但是对比学习是无监督或者自监督学习方法，而度量学习一般为有监督学习方法。而且对比学习在loss设计时，为单正例多负例的形式，因为是无监督，数据是充足的，也就可以找到无穷的负例，但如何构造有效正例才是重点。而度量学习多为二元组或三元组的形式，如常见的 Triplet 形式（anchor，positive，negative），Hard Negative 的挖掘对最终效果有较大的影响。



## Introduction

学习自然语言的通用语义表示一直是 NLP 的基础任务之一，然而当 2019 年的 SBERT 出现之后，语义相似度计算和语义匹配模型似乎就没有什么突破性进展了，2020 年出现的 BERT-flow 和 BERT-whitening 主要是对无监督语义匹配的改进，而有监督语义匹配的 SOTA 一直是 SBERT。
SBERT 本身并不复杂，仅仅是一个基于 BERT 的孪生网络而已，想要在 SBERT 的基础上改进指标，只能从训练目标下手，而本篇论文就将对比学习的思想引入了 SBERT，大幅刷新了有监督和无监督语义匹配 SOTA，更让人惊叹的是，无监督 SimCSE 的表现在 STS 基准任务上甚至超越了包括 SBERT 在内的所有有监督模型。

## 原理

### 对比学习

### 正样本

使用对比损失最关键的问题是如何构造 $(x_i, x_i^+)$。对比学习最早起源于 CV 领域的原因之一就是图像的 $x^+$ 非常容易构造，裁剪、翻转、扭曲和旋转都不影响人类对图像语义的理解。而结构高度离散的自然语言则很难构造语义一致的 $x^+$。前人采用了一些数据增强方法来构造，比如同义词替换，删除不重要的单词，语序重排等，但这些方法都是离散的操作，很难把控，容易引入负面噪声，模型也很难通过对比学习的方式从这样的样本中捕捉到语义信息，性能提升有限。

### Alignment and uniformity

对比学习的目标是从数据中学习到一个优质的语义表示空间，那么如何评价这个表示空间的质量呢？

**[Wang and Isola (2020)](https://link.zhihu.com/?target=http%3A//proceedings.mlr.press/v119/wang20k/wang20k.pdf)**提出了衡量对比学习质量的两个指标：alignment和uniformity，其中alignment计算![[公式]](https://www.zhihu.com/equation?tex=x_i)和![[公式]](https://www.zhihu.com/equation?tex=x_%7Bi%7D%5E%7B%2B%7D)的平均距离：

![](https://www.zhihu.com/equation?tex=%5Cell_%7B%5Ctext+%7Balign+%7D%7D+%5Ctriangleq+%5Cunderset%7B%5Cleft%28x%2C+x%5E%7B%2B%7D%5Cright%29+%5Csim+p_%7B%5Ctext+%7Bpos+%7D%7D%7D%7B%5Cmathbb%7BE%7D%7D%5Cleft%5C%7Cf%28x%29-f%5Cleft%28x%5E%7B%2B%7D%5Cright%29%5Cright%5C%7C%5E%7B2%7D+%5C%5C)

![](https://pic3.zhimg.com/80/v2-1e82a83ced2193eeea88de52cb79dab6_720w.jpg)

而 uniformity 计算向量整体分布的均匀程度：

![](https://www.zhihu.com/equation?tex=%5Cell_%7B%5Ctext+%7Buniform+%7D%7D+%5Ctriangleq+%5Clog+%5Cunderset%7Bx%2C+y%5Csim+p_%7B%5Ctext%7Bdata%7D%7D%7D%7B%5Cmathbb%7BE%7D%7D+e%5E%7B-2%5C%7Cf%28x%29-f%28y%29%5C%7C%5E%7B2%7D%7D+%5C%5C)

![](https://pic1.zhimg.com/80/v2-00663c0ed44da5eeac9e28d741d0a95c_720w.jpg)

我们希望这两个指标都尽可能低，也就是一方面希望正样本要挨得足够近，另一方面语义向量要尽可能地均匀分布在超球面上，因为均匀分布信息熵最高，分布越均匀则保留的信息越多，“拉近正样本，推开负样本”实际上就是在优化这两个指标。在后文中，作者将会使用这两个指标来评价模型。

### Unsupervised SimCSE

众所周知，直接用 BERT 句向量做无监督语义相似度计算效果会很差，任意两个句子的 BERT 句向量的相似度都相当高，其中一个原因是向量分布的非线性和奇异性，前不久的 BERT-flow 通过 normalizing flow 将向量分布映射到规整的高斯分布上，更近一点的 BERT-whitening 对向量分布做了 PCA 降维消除冗余信息，但是标准化流的表达能力太差，而白化操作又没法解决非线性的问题，有更好的方法提升表示空间的质量吗？正好，对比学习的目标之一就是学习到分布均匀的向量表示，因此我们可以借助对比学习间接达到规整表示空间的效果，这又回到了正样本构建的问题上来，而本文的创新点之一正是无监督条件下的正样本构建。

本文作者提出可以通过随机采样 dropout mask 来生成![[公式]](https://www.zhihu.com/equation?tex=x_%7Bi%7D%5E%7B%2B%7D)，回想一下，在标准的Transformer中，dropout mask被放置在全连接层和注意力求和操作上，设![[公式]](https://www.zhihu.com/equation?tex=%5Cmathbf%7Bh%7D_%7Bi%7D%5E%7Bz%7D%3Df_%7B%5Ctheta%7D%5Cleft%28x_%7Bi%7D%2C+z%5Cright%29)，其中![[公式]](https://www.zhihu.com/equation?tex=z)是随机生成的dropout mask，由于dropout mask是随机生成的，所以在训练阶段，将同一个样本分两次输入到同一个编码器中，我们会得到两个不同的表示向量![[公式]](https://www.zhihu.com/equation?tex=z%2Cz%5E%5Cprime)，将 $z^\prime$ 作为正样本，则模型的训练目标为

$$\ell_{i}=-\log+\frac{e^{\operatorname{sim}\left(\mathbf{h}_{i}^{z_{i}},+\mathbf{h}_{i}^{z_{i}^{\prime}}\right)/\tau}}{\sum_{j=1}^{N}+e^{\operatorname{sim}\left(\mathbf{h}_{i}^{z_{i}},\mathbf{h}_{j}^{{z_{j}}^{\prime}}\right)/\tau}}\\$$

这种通过改变 dropout mask 生成正样本的方法可以看作是**数据增强的最小形式**，因为**原样本和生成的正样本的语义是完全一致的**(注意语义一致和语义相关的区别)，**只是生成的 embedding 不同而已**。

#### Dropout noise as data augmentation

那么这种简单的正样本生成方式和其他**显式的数据增强方式有明显的优势**吗？

是的 能提升大约 5 个点

Why does it work?

作者首先尝试改变了 dropout rate，发现默认的 $p=0.1$ 性能是最好的，去掉 dropout 或者固定 dropout mask 后模型性能都会出现大幅的下降，因为这时候 $x^+$ 就和 $x_i$ 一模一样，模型几乎什么都学不到。

之前提到了衡量对比学习质量的指标：alignment 和 uniformity，作者用这两个指标测试了不同模型训练过程中保存的 checkpoints，可视化结果如下图所示，可以发现所有模型的 uniformity 都有所改进，表明预训练 BERT 的语义向量分布的奇异性被逐步减弱，而与 fix dropout 和 no dropout 相比， SimCSE 能在规整分布的同时保持正样本的对齐，与 Delete one word 相比，虽然 alignment 比不上 delete one  word，但由于其分布更加均匀，因此 SimCSE 总体性能更高。另外，这幅图其实还有很多可以探讨的问题，这里不再细说。

![](https://pic3.zhimg.com/80/v2-da7a45c8d044ebc9d6779996c7579ada_720w.jpg)

### Supervised SimCSE

上面论述了 SimCSE 在无监督语义相似度任务上的优秀表现，在有监督的条件下，SimCSE 是否能够超越 SBERT呢？在 SBERT 原文中，作者将 NLI 数据集作为一个三分类任务来训练，这种方式忽略了正样本与负样本之间的交互(实际上 SBERT 文档中早已引入了对比损失，只是没有在原文中发表)，而对比损失则可以让模型学习到更丰富的细粒度语义信息。

首先我们来考虑怎么构造训练目标，其实很简单，直接将数据集中的正负样本拿过来用就可以了，将 NLI(SNLI+MNLI) 数据集中的 entailment 作为正样本， conradiction 作为负样本，加上原样本 premise 一起组合为 $(x_i,x^+_i,x_i^-)$，并将损失函数改进为

![](https://www.zhihu.com/equation?tex=-%5Clog+%5Cfrac%7Be%5E%7B%5Coperatorname%7Bsim%7D%5Cleft%28%5Cmathbf%7Bh%7D_%7Bi%7D%2C+%5Cmathbf%7Bh%7D_%7Bi%7D%5E%7B%2B%7D%5Cright%29+%2F+%5Ctau%7D%7D%7B%5Csum_%7Bj%3D1%7D%5E%7BN%7D%5Cleft%28e%5E%7B%5Coperatorname%7Bsim%7D%5Cleft%28%5Cmathbf%7Bh%7D_%7Bi%7D%2C+%5Cmathbf%7Bh%7D_%7Bj%7D%5E%7B%2B%7D%5Cright%29+%2F+%5Ctau%7D%2Be%5E%7B%5Coperatorname%7Bsim%7D%5Cleft%28%5Cmathbf%7Bh%7D_%7Bi%7D%2C+%5Cmathbf%7Bh%7D_%7Bj%7D%5E%7B-%7D%5Cright%29+%2F+%5Ctau%7D%5Cright%29%7D+%5C%5C)

这里的![[公式]](https://www.zhihu.com/equation?tex=x_%7Bi%7D%5E%7B-%7D)可以被看作是 hard negatives。另外，作者分别尝试在不同的语义匹配数据集上(QQP、Flickr、ParaNMT)训练模型，并在 STS-B 上测试模型，结果如下表所示，其中 sample 列表示采样相等样本量训练的结果，作者发现 NLI 训练出来的模型性能是最好的，这是因为 NLI 数据集的质量本身很高，正样本词汇重合度非常小(39%)且负样本足够困难，而 QQP 和 ParaNMT 数据集的正样本词汇重合度分别达到了 60% 和 55%。

### Connection to Anisotropy

近几年不少研究都提到了语言模型生成的语义向量分布存在各向异性的问题，这极大地限制了语义向量的表达能力，缓解这个问题的一种简单方法是加入后处理步骤，比如 BERT-flow 和 BERT-whitening 将向量映射到各向同性的分布上，而本文作者证明了对比学习的训练目标可以隐式地压低分布的奇异值，提高 uniformity。

对比学习可以潜在地解决表示退化的问题，提升表示向量分布的 uniformity，之前的 BERT-flow 和 BERT-whitening 仅仅致力于寻找各向同性的表示分布，而在优化对比损失时，对比损失的第二项能够达到同样的规整分布的效果，同时，对比损失的第一项还能确保正样本的对齐，这两项起到了相互制约和促进的作用，最终使得 SimCSE 在 BERT 和 BERT-flow 之间找到了平衡点，这也是 SimCSE 如此有效的关键之处。

## 结果分析

作者借助 SentEval 在多个数据集上评估了 SimCSE 的性能，如下表所示，可以发现，在 STS 系列任务上，SimCSE 在无监督和有监督的条件下均大幅超越了之前的 SOTA 模型，而在迁移任务上，SimCSE 也有着可观的性能提升，具体的实验细节可参考原文。

值得说明的是，在迁移任务上，作者尝试了加入 MLM 损失的 trick：![[公式]](https://www.zhihu.com/equation?tex=%5Cell%2B%5Clambda+%5Ccdot+%5Cell%5E%7B%5Cmathrm%7Bmlm%7D%7D)，**直观上这缓解了模型对 token 级别信息的灾难性遗忘**，实验结果表明这一额外损失能够提升模型迁移任务上的表现，但对 STS 任务没有明显帮助。

另外，作者没有比较 BERT-flow 和 BERT-whitening，因为作者发现这些基于后处理的模型在迁移任务上表现还不如 BERT，主要原因是这些任务的主要目标并不是学习可用于聚类的语义向量。

![](https://pic2.zhimg.com/80/v2-7e2eda5589fa63026b77dbe863115351_720w.jpg)

## Links

- [丹琦女神新作：对比学习，简单到只需要 Dropout 两下](https://mp.weixin.qq.com/s/BpbI_S9lXofVFdu8qffIkg)
- [超细节的对比学习和 SimCSE 知识点](超细节的对比学习和SimCSE知识点)

