---
title: Barlow Twins
date: 2021-3
paper: https://arxiv.org/abs/2103.03230
---

时间线

![](https://pic1.zhimg.com/80/v2-26d76f2cf52fa30a3646f3f6d0dbbea8_720w.jpg)



1. 首先是 2020 年初的 **SimCLR**,  这个文章的核心贡献有二：一是提供了使用 google 的丰富的计算资源和强大的工程能力，使用高达 4096 的 mini-batch size，把 SSL 的效果推到了 supervised 方法差不多的效果（预训练模型做下游任务）；二是细致地整理了一些对 SSL 效果提升很有用的 tricks: 如更长的训练，多层 MLP 的 projector 以及更强的 data augmentations。这些有用的 trick 在后来的 SSL 的论文中一直被沿用，是 SSL 发展的基石，而第一个点，则是指出了大 batch-size 出奇迹，为未来的论文指出了改进的路，或者树立了一个进击的靶子。 
2. MoCo 共有两版本，原始版本是 2019 年末放出来的。在 SimCLR 出现后之后，又吸收 SimCLR 的几个 SSL 小技巧，改进出了 V2 版，但是整体方法的核心是没有变化的，V2 仅仅是一个 2 页试验报告。相比于 SimCLR 大力出奇迹，恺明设计了一个巧妙的 momentum encoder 和 dynamic queue 去获得大量的负样本。这里的 momentum encoder 采用了动量更新机制，除了文章本身的分析，另一层的理解是：其实momentum encoder 相当于是 teacher, 而 dynamic 里是来自不同 mini-batch 的样本，所以 teacher 需要在时间维度上对于同一个样本的输出具有一致性，否则，要学习的 encoder 也就是 student，会没有一个稳定的学习目标，难以收敛；当然另一方面，teacher 也不能一直不变，如果teacher 一直不变，student 就是在向一个随机的 teacher 学习。综上，动量更新机制是一个相当好理解的选择。
3. **阶段小结：**抛开细节，SimCLR 和 MoCo 的核心点，都是认为 **negatives（负样本）**非常重要，一定要有足够多的负样本，只不过实现方式略有不同。SimCLR 拿着 TPU，直接把 batch size 搞到 4096，一力降十会；恺明则是巧妙设计 Momentum 机制，避开了硬件工程的限制，做出了可以飞入寻常百姓家的 MoCo。再次重申，这时候的认识，还是停留在需要大量的负样本，来提升 SSL model 的效果这个历史局限里。
4. **BYOL** 是 Deep Mind 在 2020 年发布的工作，文章的核心点就是要破除**“负样本迷信”**，BYOL 认为不使用负样本，照样可以训练出效果拔群的 SSL model。但是如果直接抛弃负样本，只拉近正样本对的话，model 会容易陷入**平凡解**：对于任意样本，输出同样的 embedding。为了在没有负样本的帮助下，解决这个问题。BYOL 在 Projector 之上，增加了一个新的模块，取名 Predictor。整体可以理解为在 MoCo 的基础上，但是不再直接拉近正样本对（即同一个样本，不同增强后的输出）的距离，而是通过 Predictor 去学习online encoder 到  target encoder (即 moco 里的 momentum encoder)的映射。另外，对target network 梯度不会传递，即 **Stop-Gradient**。（注：在 MoCo 中，momentum encoder也是没有梯度回传的，不过 MoCo 这么没有给 momentum  encoder 回传梯度是因为 queue 里面的负样本来自过去的mini-batch，其计算图已经丢失，没有办法回传梯度，而如果只回传正样本对的梯度，会很不合理。而  BYOL 是只考虑正样本对，如果梯度对于 online encoder 和 target encoder 都回传，不存在这个不合理的点，因此，Stop-Gradient 是 BYOL 的一个特别的设计。）
5. **SimSiam** 是在 BYOL 的再次做减法，这里在 BYOL 的基础上去除了 momentum 更新的 target encoder,  直接让 target encoder = online encoder。指出了 predictor+stop-gradinent 是训练出强大 SSL encoder 的一个充分条件。
6. **再次的阶段小结**：在这个阶段，认识进展到了可以没有负样本的阶段，但是不使用负样本，模型就会有陷入平凡解的风险。为此，BYOL 设计了predictor 模块，并为之配套了 stop-gradient 技巧；SimSiam 通过大量的试验和控制变量，进一步做减法，去除了 momentum  update。让模型进一步变得简单。再次总结，就是 predictor 模块，避免了直接拉近正样本对，对于梯度的直接回传，让模型陷入平凡解。BYOL 和 SimSiam 在方法上都是很不错的，试验也做得很可信充分，可是对于方法的解释并没有那么深刻置信，可能要寻求一个扎实的解释也确实很难。可以参见[从动力学角度看优化算法（六）：为什么SimSiam不退化？ - 科学空间|Scientific Spaces](https://link.zhihu.com/?target=https%3A//spaces.ac.cn/archives/7980)，也是另一个角度的解释，颇为有趣合理。此时已经进入到了摆脱了负样本了，但是在不使用负样本的情况，要想成功训练好一个 SSL model，需要引入新的 trick:  即predictor+stop-gradient。这样子来看，难免有点像左手换右手的无用功，但是整体的技术认识是进步了很多的。
7. 最后，终于到了这次的主角：**Barlow Twins**。 在不考虑数据增强这种大家都有的 trick 的基础上， Barlow Twins 既没有使用负样本，没有动量更新，也没有 predictor 和 stop gradient 的奇妙操作。Twins 所做的是换了一种视角去学习表示，从 embeddig 本身出发，而不是从样本出发。优化目标是使得不同视角下的特征的相关矩阵接近恒等矩阵，即让不同的维度的特征尽量表示不同的信息，从而提升特征的表征能力。这种做法，和以前传统降维（如 PCA）的方法是有共通之处的，甚至优化的目标可以说非常一致。

![](https://pic4.zhimg.com/80/v2-221c68142f83b4ebbb774d9e5b2436c3_720w.jpg)

那么 Twins 方法和以上的基于正负样本对的所有方法的区别，不严格（抛去特征normalize，BN等操作来说）的来说，可以用一句话，或者说两个式子来概括。

过去的方法大多基于 InfoNCE loss 或者类似的对比损失函数，其目的是为了是的样本相关阵接近恒等矩阵，即

![[公式]](https://www.zhihu.com/equation?tex=Z_a%2A%7BZ_b%7D%5ET+%5Crightarrow+%5Cmathcal%7BI%7D_N) 

而 Twins 的目的是为了让特征相关阵接近恒等，即：

![[公式]](https://www.zhihu.com/equation?tex=%7BZ_a%7D%5ET%2AZ_b+%5Crightarrow+%5Cmathcal%7BI%7D_D) 

对于对比损失类方法，比如 SimCLR 或 MoCo 需要很大的 Batchsize 或者**用 queue 的方式去模拟很大的batchsize**, 而 Twins 需要极大的特征维度（8192）。这种特性和以上两个公式是完全对应且对称的。一个需要大 ![[公式]](https://www.zhihu.com/equation?tex=N) ,一个需要大 ![[公式]](https://www.zhihu.com/equation?tex=D) 。

![](https://pic3.zhimg.com/80/v2-4373c8eefb1268c1e0e49e609e63817a_720w.jpg)

**总结**：从历史线上来看，从 SimCLR 和 MoCo 说一定要有大量的负样本，到 BYOL 和 SimSiam 通过神奇操作（stop-grad+predictor）验证了负样本并非不可或缺，最终到了 Twins 切换了一直以来从对比学习去训练 SSL 的视角，转向从特征本身出发，推开了另一扇大门。对比而言，相比于最简单的**裸InfoNCE**，Twins 仅仅是换了一个 loss function (+大维度的特征)。不过，大的维度相比于增加 batchsize 的代价要小得多，就是多占一点的显存。

## Links

- [最简单的 self-supervised 方法](https://zhuanlan.zhihu.com/p/355523266)