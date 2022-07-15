# 深度度量学习


深度度量学习网络(deep metric learning) 旨在将视觉相似图片投影到嵌入流形后彼此相邻，而视觉不相似图片相互分开。 学习得到的深度特征，具有很好的区分性，具有的特点是：类内方差小(差异性下)，类间方差大(差异性大). 该特征对于视觉搜索引擎至关重要.

传统的降维有两个缺陷：一是它们没有产生一个从输入到流形空间的可以用于训练关系未知的新样本的**函数（或映射）**；二是需要假设在输入空间存在一个有意义的（可计算的）距离度量。

深度度量学习的一些论文: [深度度量学习(Deep metric learning)](https://www.jianshu.com/p/a14b889a7aab)

在深度学习中，很多度量学习的方法都是使用成对成对的样本进行 loss 计算的，这类方法被称为 pair-based deep metric learning。例如，在训练模型的过程，我们随意的选取两个样本，使用模型提取特征，并计算他们特征之间的距离。如果这两个样本属于同一个类别，那我们希望他们之间的距离应该尽量的小，甚至为 0；如果这两个样本属于不同的类别，那我们希望他们之间的距离应该尽量的大，甚至是无穷大。正是根据这一原则，衍生出了许多不同类型的 pair-based loss，使用这些 loss 对样本对之间的距离进行计算，并根据生成的 loss 使用各种优化方法对模型进行更新。本文将介绍一些常见的 pair-based metric learning loss。

## Losses

- [度量学习中的 pair-based loss](https://zhuanlan.zhihu.com/p/72516633)
- [度量学习 - 损失函数汇总[译]](https://www.aiuai.cn/aifarm1697.html)

例如，有一个数据集有三种动物，分别是狗、狼 、猫，直观上狗和狼比较像，狗和猫的差异比较大，所以狗狼之间的margin应该小于狗猫之间的margin，但是Contrastive loss使用的是固定的margin，如果margin设定的比较大，模型可能无法很好的区分狗和狼，而margin设定的比较小的话，可能又无法很好的区分狗和猫。

度量学习中还有很多其他类型的 pair-based loss，通过上文可以发现，这些不同的 loss 基本上都是在 Contrastive loss 和 Triplet loss 的基础上改进而来。这些改进思想很值得我们借鉴，尤其是通过观察分析已经存在的loss的缺陷，从而提出针对性的改进，构造一个适合自己应用场景的 loss。

Contrastive loss 能够让正样本对尽可能的近，负样本对尽可能的远，这样可以增大类间差异，减小类内差异。但是其需要指定一个固定的 margin，所以这里就隐含了一个很强的假设，即每个类目的样本分布都是相同的，不过一般情况下这个强假设未必成立。

#### 

#### Triplet loss 

#### Triplet center loss

Triplet Loss是让正样本对之间的距离小于负样本对之间的距离，并且存在一定的margin。因此triplet样本的选取至关重要，如果选取的triplet对没啥难度，很容就能进行区分，那大部分的时间生成的loss都为0，模型不更新，而如果使用hard mining的方法对难例进行挖掘，又会导致模型对噪声极为敏感。为了对Triplet loss的缺点进行改进，Triplet center loss就被提出来了。

Triplet Center loss 的思想非常简单，原来的 Triplet 是计算 anchor 到正负样本之间的距离，现在 Triplet Center 是计算 anchor 到正负样本所在类别的中心的距离。类别中心就是该类别所有样本 embedding 向量的中心。

#### N-pair loss

triplet loss同时拉近一对正样本和一对负样本，这就导致在选取样本对的时候，当前样本对只能够关注一对负样本对，而缺失了对其他类别样本的区分能力。

为了改善这种情况，N-pair loss 就选取了多个负样本对，即一对正样本对，选取其他所有不同类别的样本作为负样本与其组合得到负样本对。如果数据集中有 N 个类别，则每个正样本对 $y_{ii}$ 都对应了$N-1$ 个负样本对。

![](https://pic2.zhimg.com/80/v2-1a76e4c4cd771e9282dcea2667dc0efd_720w.jpg)

#### Quadruplet loss

由两部分组成:一部分就是正常的triplet loss，这部分loss能够让模型区分出正样本对和负样本对之间的相对距离。

另一部分是正样本对和其他任意负样本对之前的相对距离。这一部分约束可以理解成最小的类间距离都要大于类内距离，不管这些样本对是否有同样的anchor。

#### Lifted Structure Loss

Lifted Structure loss 的思想是对于一对正样本对而言，不去区分这个样本对中谁是 anchor，谁是 positive，而是让这个正样本对中的每个样本与其他所有负样本的距离都大于给定的阈值。此方法能够**充分的利用 mini-batch 中的所有样本**，挖掘出所有的样本对。




