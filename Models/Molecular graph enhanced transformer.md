---
title: Molecular graph enhanced transformer for retrosynthesis prediction
url: "https://github.com/papercodekl/MolecularGET"
---

## 创新点

提出了 Graph Enhanced Transformer (GET)框架：
为了使原子表示学习具有更合理的化学约束以获得更好的性能。
分子的顺序信息和图形信息。

设计了带边和跳跃连接的图注意 (GAES)
它以一种具有键特征的自注意方式学习原子节点的高质量表示，而且它受GNN中多层叠加的副作用影响较小。

## 模型与数据

benchmark dataset: USPTO-50 K

baseline: template-free Seq2Seq-based methods

提出了四种不同的GET设计，将smile表示与从改进的图神经网络(GNN)中学习到的原子嵌入相融合。

## 知识点

canonical2 SMILES：每个化合物都有一个  canonical SMILES。应用于 SMILES 数据的 MT 模型往往忽略了自然原子连接和分子拓扑结构的信息。

目前基于模板的逆合成预测方法大多优于数据驱动的无模板方法。

## 术语

MT 机翻