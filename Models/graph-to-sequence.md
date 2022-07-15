---
title: P ERMUTATION INVARIANT GRAPH-TO- SEQUENCE MODEL FOR TEMPLATE-FREE RETROSYNTHESIS AND REACTION PREDICTION
---

## 模型

Graph2SMILES
双编码器：
- 编码器：an attention-augmented directed message passing neural network (D-MPNN) captures local chemical environments + Gated Recurrent Units (GRUs)
- 全局注意力编码器：long-range and inter-molecular interactions, enhanced by graph-aware positional embedding.

## D-MPNN

attention-augmented directed message passing neural network


### graph-aware positional embedding

Transformer-based global attention encoder 

## 结果

Graph2SMILES improves the top-1 accuracy of the Transformer baselines by 1.7% and 1.9% for reaction outcome prediction on USPTO_480k and USPTO_STEREO datasets respectively, and by 9.8% for one-step retrosynthesis on the USPTO_50k dataset.

优点：
- D-MPNN captures the local chemical context
- the global attention encoder allows for global-level information exchange
- the graph-aware positional embedding enables the attention encoder to make use of topological information more explicitly




## 方法

原理参考 Somnath et al. (2020)
smiles2graphs  RDKit Landrum, 2016)
regex tokenizer Schwaller et al. (2019). Molecular transformer

## 创新

结合了 Transformer models 在文本生成的优势
图编码的分子排列不变性，图编码可以扩增输入的数据

用 standard Transformer model (Vaswani et al., 2017) 中的 encoder 替换了 Molecular Trans-
former (Schwaller et al., 2019)。用的是 D-MPNN encoder。再加了一个基于图注意力的嵌入层

## 知识点

用翻译模式处理分子（smiles）很火。
- NLP 领域现成模型多，发展好。

双射：
SMILES representations do not provide bijective mappings to molecular structure

Bjerrum, 2017 使用化学等价的 SMILES 可以提升准确度，进行数据增强已成为提高实证绩效的一种常见做法，幅度为 0.8% to 4.3%。Graph2SMILES 相比 SMILES 扩增法速度更快，简化了预处理的过程。

多倍 SMILES 存在巨大的冗余即无效性the ineffectiveness of the SMILES representation：
Wang et al.,2021b 用了 9 倍扩增
Tetko et al., 2020 用了 100 倍扩增
结果差不多。


## 问题

## 术语

data augmentation
bijective mapping
chemically-equivalent SMILES 化学等价的 SMILES
graph-aware 图注意力

