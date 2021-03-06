# 预测具有多模态图神经网络癌症患者的存活率

本文设计了一种新的多模态图神经网络(Multimodal Graph Neural Network，MGNN)预测肿瘤生存率的框架。

文章主要有一下四部分组成：

1. multimodal data
2. the bipartite graph
3. the GNN layer
4. and the multimodal fusion neural layer.

## Idea

现有单模态数据的特征提取无法“完整”表征病人 -> modalities: intramodality coordinated representations + inter-modality joint representations

## 方法与数据

首先构造患者和多模态数据之间的二部图，以探索其内在联系。
然后，利用图神经网络得到每个患者在不同的二部图上的嵌入。
最后，提出了一种多模态融合神经层，用于融合不同模态数据的医学特征。
作者在真实世界的数据集上进行了全面的实验，证明了模型的优越性。
此外，提出的 MGNN 算法在其他四个癌症数据集上具有更强的鲁棒性。

![An example of construction patient-CNA bipartite graph.](https://cdn.jsdelivr.net/gh/xxzhai123/img/img20220516220528.png)

多模式数据包括临床数据，基因表达谱和 CNA 谱。

![The overall architecture of the proposed MGNN](https://cdn.jsdelivr.net/gh/xxzhai123/img/img20220516220953.png)

我们首先构建了利用基因表达曲线或 DNA 拷贝数（CNA）曲线的两部分图，该图探索了它们之间的固有关系。
基于双方图，我们提出了一个新的图神经网络，以获得每个患者的嵌入表示。
最后，输出是根据给定阈值（乳腺癌通常在社区中采用 5 年生存）的短期生存或长期生存分类。

For patient CNA profile, we directly utilize the original data with five discrete values:

1. homozygous deletion (-2)
2. hemizygous deletion (-1)
3. neutral/no change (0)
4. gain (1)
5. high level amplification(2)

If the CNA is not neutral/no change (0), we will establish an edge between the patient and the CNA,
so we can obtain a patient-gene bipartite graph with edge weights.

## 总结与展望

本文有以下贡献：

1. 多模态
2. 统一模型框架及对多模态数据的利用
3. MGNN 的性能 SOTA

该方法具有拓展性，可以通过对其他癌症数据集进行评估来证明，包括乳房浸润性癌，转移性结直肠癌，小儿急性淋巴白血病-II期和泛肺癌。

未来工作的方向包括使用更多的多模态数据（MRI，CT等）来学习患者的表现。
将实验扩展到其他数据集并探索患者与多模式医学数据之间的结构信息。

众所周知，基因和疾病是密切相关的，充分探索基因中隐藏的信息对癌症预测是绝对有意义的。
此外，癌症是特别复杂的，而且极难预测和治疗。因此，癌症预后预测在临床工作中具有重要作用。

## 相关工作

- 基于支持向量机的递归特征消除方法，用于基因表达数据的预测
- 孙等人提出了一个多模式深层神经网络的整合多维数据预测乳腺癌预后。

深度学习的可提升点:

1. 如何利用患者与多模态医学数据之间的结构信息。 以往的研究大多忽略了患者与医学数据之间的内在联系。
2. 如何利用不同模式(如基因表达和临床)的医学数据特征。 现有的研究大多直接将不同类型的数据组合成深度学习模型，而没有利用多模态数据的丰富信息。

### Multimodal Representation

多模态表征已经成为生物信息学研究中的一个重要概念，它反映了癌症预后预测是多模态的融合。
多模态医学数据的简单拼接，由于多模态数据的异质性，使得参数空间复杂化，不利于发现不同模式数据之间的互补性。
因此，有必要开发一种先进的方法，从集成的多通道数据中提取深层信息。
我们在半监督环境下利用图形神经网络对图形学习进行癌症预后预测研究，并利用多模态医学数据推导出一般化模型。

### Graph Embedding

同时，图形嵌入已成为用于图形结构数据的有效表示学习方法。
虽然，最近的图形嵌入已成为代表低维矢量节点的范式，旨在弥合图形分析和机器学习技术之间的差距。
早期的图形嵌入方法（也称为网络嵌入）被研究为减少维度问题。
但是，许多方法都集中在成对相似性上。
如何保护高阶接近度最近成为一个吸引研究问题。
例如，DeepWalk 使用从截短的随机步道获得的局部信息，通过将步行视为等效的句子来学习潜在的表示。
Node2Vec 扩展了这个想法，并提出了一个有偏见的二阶随机步行模型。
线优化了旨在保留本地和全球网络结构的目标函数。
M-NMF 是一个统一的框架，它使节点的学说可以保留微观和社区结构。
此外，在 SDNE 中提出了一个半监督的深层模型，该模型具有多个非线性函数，从而能够捕获高度非线性的网络结构。
此外，最近的一些工作，将关联节点属性属于网络，并将属性信息和拓扑结构平滑地嵌入到低维表示中。

### Graph Neural Networks

大多数现有的图形神经网络通过以端到端的方式递归从局部社区汇总的特征向量来学习节点表示。
Graph 卷积网络（GCN）定义了一种基于直接在图形上运行的卷积神经网络的有效变体的半监督学习方法的可扩展方法。

GCN 的卷积是图形上拉普拉斯平滑的一种特殊形式，该形式解释了许多卷积层带来的过度平滑现象。
图形注意力网络（GAT）提出了在图形结构数据上运行的新型神经网络体系结构，利用掩盖的自发层来解决基于图形卷积或其近似值的先前方法的缺点。

## 其他

- 基因表达水平的数据分析在很大程度上促进了肿瘤的诊断和治疗。

- 乳腺癌已成为继甲状腺肿瘤之后生存率最高的恶性肿瘤。

- 早期乳腺癌可以完全治愈，中晚期乳腺癌患者也可以通过靶向治疗、内分泌治疗和其他系统治疗达到长期生存。
