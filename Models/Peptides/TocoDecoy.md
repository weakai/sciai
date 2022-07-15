---
title: TocoDecoy
---



该文章提出了一种用于机器学习打分函数（machine-learning scoring functions, MLSFs）训练和测试的无隐藏偏差（hidden bias）数据集构建新方法。该方法引入四种技巧来消除隐藏偏差，针对特定靶标的活性分子，基于条件分子生成和分子对接，可以基于已知的活性分子高效地生成相应的诱饵分子(假定的负样本，decoys)，为 MLSFs 的训练和测评提供了相对无偏的数据集。

传统数据集用于 MLSFs 的构建/测试中的隐藏偏差主要有

- 人工富集偏差（活性/非活性分子物理化学性质差异过大，模型只需根据物理化学性质的差异即可分辨活性/非活性）
- 相似偏差（数据集中的化合物结构过于相似，模型的测试表现过于乐观/泛化能力有限）
- 域偏差（数据集中的化合物结构多样性太低，模型只适用于预测训练集中出现的特定骨架的化合物）
- 非因果偏差（模型在测试集上的表现好是因为模型学习了数据集中的构造分布，如在 DUD-E 上训练的模型可以很轻易的根据活性分子与非活性分子的结构不相似性进行分类从而取得很好的表现）。

解决方案,四种技巧:

- 在 100 万数量级的数据集上训练了条件分子生成模型，确保模型能够生成结构多样的分子（去除域偏差）
- 通过条件分子生成模型来控制生成的分子与活性分子物理化学性质相似（去除人工富集）
- 通过 T-SNE 算法把化合物映射到二维化学空间并进行格点过滤（去除相似偏差）
- 引入两种诱饵构建策略：假设与活性分子结构相似度较低的分子为负样本、假设活性分子与靶标的错误结合构象为负样本（去除非因果偏差）

## 实验步骤

1. 将“种子”配体（活性配体）的六个物理化学性质（MW，分子量；logP，油水分配系数；RB，可旋转键数量；HBA，氢键受体数量；HBD，氢键供体数量；HAL，卤键数量）输入到条件循环神经网络（conditional recurrent neural network，cRNN）模型中，以生成属性匹配的 decoys。cRNN 为每个活性配体共生成了 200 个有效且不重复的decoys。
2. 尽管生成的 decoys 可能具有相似的物理化学性质，并且其拓扑结构大概率与其种子（活性）配体不同，但总有一些例外。因此，decoys 在与活性配体的理化性质相比，Dice 相似性（DS）不符合以下要求将被过滤掉：（i）MW±40 Da，（ii）logP±1.5，（iii）RB±1，（iv）HBA±1，（v）HBD±1，（vi）HAL±1，（vii）DS<0.4。
3. 对每个分子依次计算 ECFP 和 T-SNE 向量，然后进行格点过滤，以消除由相似结构引起的相似偏差；保留的 decoys 形成拓扑诱饵集（Topology Decoys，TD），这些 decoys 的对接构象是通过对经过结构预处理的蛋白质和配体的分子对接获得的。
4. 按照表 S1 中列出的相应对接分数阈值来过滤活性配体的对接构象，对接分数低于阈值的构象被作为 decoys 构象保留，从而产生构成构象诱饵集（Conformation Decoys，CD）。
5. 最后，将 TD 和 CD 集整合为最终的 TocoDecoy 数据集。

![](https://mmbiz.qpic.cn/mmbiz_png/P4UadRgibwicYicWJFugIUoYMao6O9QYgAZYk3XicIegKwDkBrUMnqYhF5zoePzQaDEKjb0hicZSETWic9FWlMQrv92g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



## Links

- [J. Med. Chem.｜TocoDecoy:针对机器学习打分函数训练和测试的无隐藏偏差的数据集构建新方法。](https://mp.weixin.qq.com/s?__biz=MzU2ODU3Mzc4Nw==&mid=2247497437&idx=1&sn=93f1385960f0d93f9c3280d8f7b28555&chksm=fc895ce9cbfed5ff192def645670b5bba5a8c497fee1d88264404dc1d8be79f925823424a9ab&mpshare=1&scene=1&srcid=0616UjlAEeXP2KnYo8myxJ9F&sharer_sharetime=1656550295037&sharer_shareid=f66ef27ade3d509229b2afd8611df712#rd)