---
title: "Molecular Representation: Going Long on Fingerprints"
---

## Abstract

Machine learning for chemistry requires a strategy for representing (featurizing) molecules.


## 知识点

In theory, calculation of molecular fingerprints is a lossy procedure.
所有的表示法都是一种信息压缩，从稀疏矩阵到特征向量。

good molecular representaion and models:
 - accuracy
 - generalizability: out-of-the-box performance
 - interpretability

特征工程势必要从 informed machine learning 分离

## 创新点

### MFF-RF model
MFF representation 结合 RF 和 dense-neural network architectures for regression with a noted preference for the former because of the ease of hyperparameter optimization and lower tendency to overfit.

同时对多个 public data 进行 four diverse tasks。用来评估模型的质量

## 数据集

### QM9 dataset

HOMO-LUMO gaps calculated with DFT
效果: MFF-RF > ECFP4-RF (acc: MAE)

```yaml
model: atomistic SchNet
results: 50% lower
```

### Prediction of enantioselectivity
Denmark group’s work. Sandfort et al.

1,075 N,S-acetal formations with different combinations of substrates and chiral phosphoric acid catalysts

原作者用：voxel-derived descriptors, quantify steric occupancy of the query structures 量化查询结构的立体空间占有

如何做？作者按照原作者的分数据方式进行比对。
略胜
优点在于：开箱即用性

### 产率预测

Doyle et. al.

yields of 4,140 p-toluidine C–N cross-coupling reactions

MFF 跟 DFT 做比较

分数据集随机分和按添加剂分

The dataset represents a combinatorial screen of 15 aryl halides, 4 Pd catalysts, 3 organic bases, and 23 isoxazole additives. 

3:1 略胜

### 反应转化率预测

```yaml
items: 1,536 Pd-catalyzed C–N cross-coupling reactions
source: Merck, nanomole-scale HTE
info: base and ligand combinations
baseline: one-hot encoding
datasets: [ random, catalyst-specific]
```


## Introduction

multiple-fingerprint feature (MFF) representation

Two core components make up QSPR models: representation and regression.

The most naive option of representation is a one-hot encoding

Functional-group representations
- MACCS, a molecular access system key fingerprint
	- simply count the number of expert-defined substructures present
	- based on group-additivity relationships

- Atom pair or Morgan circular fingerprints
	- considering all substructures of a certain size and type

Other representations focus less on molecular structure and more on function by computing descriptors

Descriptors: basic properties
- estimated lipophilicity
- DFT descriptors:
	- sophisticated, expert-designed
	- correlate with complex readouts such as enantioselectivity.

past 5 years: based on precomputed feature vectors
now: operate directly on 2D molecular graphs or 3D molecular conformers
- including message-passing neural networks
- atom-centered convolutional networks

Morgan fingerprints 也进化了
- require specification of the maximum neighborhood radius, folded vector length, and which atom features should be used to differentiate two atomic environments. 

## 术语

the query molecule
bond order
aroma-ticity
heuristic descriptor
out-of-the-box performance
mean absolute error (MAE)
a low-data setting
in-house organic structures?内部的
the seminal work of 谁的开创性的工作
high-throughput experimentation HTE
in the context of 在什么的大环境下
informed machine learning？富信息机器学习