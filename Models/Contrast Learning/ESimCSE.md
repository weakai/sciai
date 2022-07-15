---
title: ESimCSE
---

ESimCSE 是 SimCSE 的增强版

---

SimCSE 的缺陷

1. dropout 构建正样本的句子长度一样大
2. 在训练 SimCSE 时，当把 batch_size 增大到一定大小时，负例增多，SimCSE 效果却下降。

由于 SimCSE 是通过调节 dropout 率构建的正例对，长度是一样大的，负例则长度不等，这会使得模型倾向于判断相同或相似长度的句子在表达上更相近。

---

为求证是否句长会影响模型效果，ESimCSE 在文中进行了训练数据的根据句长分组的实验。

由于 SimCSE 是调整 dropout 率构建正样本对，同一 batch size 中其他的样本则为负例。则从理论上应当是调整 batch size 越大，即让模型看到更多的负例，效果会更好。但实际中，batch size 设置为 64 时，模型达到了最优效果，随着 batch size 增大，效果却是变差了。

原因之一可能是将一个 batch size 中其他文本直接粗暴扩展负例不够有效，即随着 batch_size 增大，**数据中可能有有一些与正例相近的文本，划分到了负例中，这给模型训练带来误差**。

---

ESimCSE 的改进

Word-Repetition（构建正例）

通过随机复制句子中的一些单词，解决句子长度对模型的影响问题。

采用一个超参重复率 `dup_rate` 控制重复比率，N 为句子长度。随机在下式区间选取重复长度 `dup_len`，从而采取均匀分布随机选取 `dup_len` 个 `word` 进行重复。

Momentum Contrast（构建负例）

Momentum Contrast，通过维护一个负例的队列，采用一个动量模型产生负例句子入队，同时队列中“老句子”出队列，解决 batch_size 对模型的影响问题。

此处是两个 encoder，一个是模型 ESimCSE 的 encoder，另一个是用于更新负例的 momentum encoder,ESimCSE 的参数是随着模型 backward 更新， 而 momentum encoder 是通过以上方式更新负例的 encode 参数。这样的构建负例方式，在有限显存下，一个句子可对应构建更多的负例。

ESimCSE 通过以上这两种措施 Word-Repetition，Momentum Contrast，分别针对 SimCSE 中的正例及负例构建进行了重建。