# VAT

它主要关心的是模型在小扰动下的稳健性。

## 对抗训练

- 对抗攻击: 想办法造出更多的对抗样本
- 对抗防御: 就是想办法让模型能正确识别更多的对抗样本
- 对抗训练: 属于对抗防御的一种，它构造了一些对抗样本加入到原数据集中
  - 希望增强模型对对抗样本的鲁棒性
  - 通常还能提高模型的表现

#### Min-Max

$$
\begin{equation}\min_{\theta}\mathbb{E}_{(x,y)\sim\mathcal{D}}\left[\max_{\Delta x\in\Omega}L(x+\Delta x, y;\theta)\right]\label{eq:min-max}\end{equation}
$$

整个优化过程是 max 和 min 交替执行，这确实跟 GAN 很相似

#### 快速梯度 Fast Gradient Method（FGM）

一种对抗训练方法, 它由 GAN 之父 Goodfellow 在论文[《Explaining and Harnessing Adversarial Examples》](https://arxiv.org/abs/1412.6572)首先提出。

此外，对抗训练还有一种方法，叫做**Projected Gradient Descent（PGD）**，其实就是通过多迭代几步来达到让 *L*(*x*+Δ*x*,*y*;*θ*) 更大的$\Delta x$如果迭代过程中模长超过了*ϵ*ϵ，就缩放回去，细节请参考[《Towards Deep Learning Models Resistant to Adversarial Attacks》](https://arxiv.org/abs/1706.06083)。）。但本文不旨在对对抗学习做完整介绍，而且笔者认为它不如 FGM 漂亮有效，所以本文还是以 FGM 为重点。

#### 回到 NLP

对于 CV 领域的任务，上述对抗训练的流程可以顺利执行下来，因为图像可以视为普通的连续实数向量，Δx也是一个实数向量，因此x+Δx依然可以是有意义的图像。但 NLP 不一样，NLP 的输入是文本，它本质上是 one hot 向量（如果还没认识到这一点，欢迎阅读[《词向量与Embedding究竟是怎么回事？》](https://kexue.fm/archives/4122)），而两个不同的 one hot 向量，其欧氏距离恒为2 √2，因此对于理论上不存在什么“小扰动”。

一个自然的想法是像论文[《Adversarial Training Methods for Semi-Supervised Text Classification》](https://arxiv.org/abs/1605.07725)一样，将扰动加到Embedding 层。这个思路在操作上没有问题，但问题是，扰动后的 Embedding 向量不一定能匹配上原来的 Embedding 向量表，这样一来对Embedding 层的扰动就无法对应上真实的文本输入，这就不是真正意义上的对抗样本了，因为**对抗样本依然能对应一个合理的原始输入**。

那么，在 Embedding 层做对抗扰动还有没有意义呢？有！实验结果显示，在很多任务中，在 Embedding 层进行对抗扰动能有效提高模型的性能。

#### 实验

由于每一步算对抗扰动也需要计算梯度，因此每一步训练一共算了两次梯度，因此每步的训练时间会翻倍。

同所有正则化手段一样，对抗训练也不能保证每一个任务都能有提升，但从目前大多数“战果”来看，它是一种非常值得尝试的技术手段。

此外，BERT 的 finetune 本身就是一个非常玄乎（靠人品）的过程，前些时间论文[《Fine-Tuning Pretrained Language Models: Weight Initializations, Data Orders, and Early Stopping》](https://arxiv.org/abs/2002.06305)换用不同的随机种子跑了数百次 finetune 实验，发现最好的结果能高出好几个点，所以如果你跑了一次发现没提升，不妨多跑几次再下结论。

## References

- [苏神 | 对抗训练浅谈：意义、方法和思考（附Keras实现）](https://kexue.fm/archives/7234)
