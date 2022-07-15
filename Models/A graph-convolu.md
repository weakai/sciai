---
title: A graph-convolutional neural network model for
the prediction of chemical reactivity
---

372 左边最后一段

## 思考

模板忽略了哪些信息？全局信息吗？或者说是互信息？如何破解？

手动注释相关信息？或者是利用各种启发式的算法生成相关的信息？

## 知识点

## reaction families

针对某个 reaction families，我们可以利用 historical reaction data 得到 informed quantitative prediction.

### reaction templates library

有哪些现成的库？

Reaction templates are the classic approach to codifying the “rules” of chemistry

### LHASA

Logic and Heuristics Applied to Synthetic Analysis 就是用的反应模板

作者：Corey's synthesis planning program

small molecule discovery：
- iterative cycle underlying： the hypothesize-make-measure 

Reaction evaluation 在 hypothesize 和 measure 环节很重要

高通量删选 make a lot, but lags the measurement  

A recent Merck study employed MALDI-TOF MS for rapid online analysis or 1536 well plates in just 10 minutes.

Yet with these high throughput analyses, one must specify which masses to quantify or inspect results manually – in the case of the Merck study, there were “nearly 400” distinct mass peaks to extract.


CAMEO: 1980, Jorgensen, 首个计算机辅助有机反应机理推断
以及更多的专家启发式程序：
EROS
IGOR
SOPHIA
Robia11

Ugi？

## 模型

### 数据

patent literature

### 任务

supervised learning

predict the products of organic reactions given their reactants, reagents, and solvent(s).
方式：计算relative likelihoods.

### 结果

the neural model makes informed predictions of chemical reactivity.
主产物 acc 85%
快，100 ms per example on a single consumer GPU -> 可用于高通量筛选和逆合成分析

### 算法

reactant molecules -> a single molecular graph
atoms and bonds -> nodes and edges

WLN，Weisfeiler-Lehman Network
图卷积网路f分析反应图预测似然函数（从原子到原则）

workflow

原子的特征化：
atomic number
formal charge
degree of connectivity
explicit and implicit valence
aromaticity.
键的特征化：键序和成环与否




### 别人的模型

都不能加分析试剂对反应的影响

#### ReactionPredictor
Baldi
learns mechanisms from expert-encoded rules as a supervised
learning problem

#### ELECTRO
Bradshaw
reproduces pseudo-mechanistic steps as defined by an expert-
encoded heuristic function.

### translation model
Schwaller
can illustrate which reactant tokens inform which product tokens, which is useful for predicting atom-to-atom mapping, but does not reveal chemical understanding and is not aligned with how humans describe chemical reactivity.
注意力机制能够完成原子映射，但是无法揭示化学家对反应的类似的理解


## 创新点

模仿专家：
寻找最可能的位点

## 术语

chemical reactivity？
evaluating their relative likelihoods?似然函数
hundreds of thousands of reaction precedents 反应先例
a broad range of reaction types
performs on par with expert chemists with years of formal training 媲美
The prediction of reaction outcomes
we address the task of 我们解决了什么任务
impurity identification and quantification 杂质的定性和定量
particularly in the context of drug substance manufacturing？原料药制造？
play central role in 
There is a rich history of computer-assistance
the chemistry community
Major developments
data availability
have enabled new approaches to this problem
we describe a chemically-informed model that incorporates
domain expertise through its architecture. describe 引出作者的工作。informed 与 with 意思相近，但是程度更深。  
off-the-shelf machine translation models 现成的
this observation would need to be revisited 关于这一点，我们待会再说。
Mathematical details can be found in the ESI.
The WLN starts with, as input, the reactant graph. 从什么开始
forgo 放弃使用

