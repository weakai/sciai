---
titile: protein-protein interactions
subtitle: PPI
---

预测 PPI 的方法主要包括生物学和计算学方法

传统生物学领域，互作数据的收集方法：

- 酵母双杂交
- 蛋白质芯片
- 合成致死分析

特点：耗时又费力，预测效率不足，假阴性和假阳性

## PPI 预测所需的各种蛋白质相关数据库

分成五类

1. 蛋白质相互作用网络（通过生物学方法验证，高准确性）
    - BIND
    - DIP
    - MINT
    - BioGRID
    - HPRD
    - IntAct
    - STRING
2. 蛋白质序列
    - UniProt
    - PIR
    - SWISS-PROT
    - NRL3D
    - TrEMBL
3. 高级结构
    - PDB
    - SCOP
4. 基因组信息
    - MIPS（哺乳动物相关的基因信息）
    - CGD（其他多种生物）
5. 基因本体论
    - GO database
    - QuickGO

## 计算学模型

计算学预测模型（根据不同的预测信息）

1. 基于网络结构的模型
    - 无权无向图
    - 具有相互作用，True
2. 基于序列的模型
3. 基于结构的模型
4. 基于基因组的模型
5. 基于基因本体论的模型

### 基于网络结构的模型

常见的能够被用于预测相互作用的网络结构信息包括共同邻居、网络路径、全局网络结构和几何嵌入四种。这四类方法能够从局部和全局的角度衡量蛋白质对拓扑相似性，以获取更高的预测性能。

### 基于序列的模型

- 基于序列，多种角度预测相互作用
    - 序列相似性
    - 共同进化信息，等等

### 基于基因组的模型

- 生物基因组测序
- 基于基因融合的预测模型认为能够在另一个组织中彼此融合的两个蛋白质，更有可能彼此发生相互作用。
- 基于基因顺序的模型认为基因之间的顺序表明了其在功能上的相关性，而功能相关的蛋白质对更可能发生相互作用；系统发育图谱通常存在于系统发育树中，基于该信息的模型认为蛋白质系统发育图谱的相似性表明了其在功能上的相关性，因此具有相似发育图谱的蛋白质更可能发生相互作用。虽然基于基因组的模型历史悠久且具有较好的性能，但是对于那些没有发现基因融合事件，基因顺序无法获取，以及不能很好的映射其系统发育图谱的蛋白质对，它们之间的相互作用无法利用此类模型进行预测。

### 基于基因本体论的模型

- 基因本体论是由研究人员提出的用于描述基因及其产物特性的术语，而蛋白质就是后者的代表物质。
- 该术语能够从细胞组分、分子功能和生物学过程三个角度描述蛋白质。由于具有相似功能的两个蛋白质更可能通过彼此发生相互作用，来承担细胞体内的同一个生理功能，而基因本体论又提供了获取蛋白质功能信息的相应途径，所以，可以通过某些方法计算蛋白质对的基因本体论相似性，从而评估它们的功能相似性，进而预测蛋白质相互作用。虽然有研究表明基于基因本体论的特征比基于序列的频谱计数特征具有更好的性能。但是，此类模型多数没有考虑细胞组分信息以及基因本体论类别层次结构中不相等的深度对于蛋白质相互作用预测带来的影响。

### 基于深度学习的模型

已经很了解了

### 大规模预测模型

目前，已经鉴定出的蛋白质相互作用的数据还不到整个相互作用组的20％。

大规模需要分布式并行预测。

获取蛋白质相互作用数据：高通量技术。

## Protocol

为了实现准确预测蛋白质相互作用，现有的计算模型通常遵循有监督的学习框架，准备相互作用和非相互作用的蛋白质对数据。

- 相互作用数据是阳性样品，非相互作用是阴性样品。
    前者可以从数据库中明确提取，后者可利用随机生成策略、细胞定位策略和 Negatome 2.0 获取。

一旦获得了实验数据，下一步就是选择合适的方案进行性能评估。通常，实验数据可分为训练集和测试集，前者用于训练模型，后者用于验证模型性能。常被用来划分训练集和测试集的方案包括三种：随机子抽样验证、K折交叉验证、留一法交叉验证。

### 性能评估

- Matthew相关系数（MCC）
- F1得分
- AUC
- PR-AUC

MCC是一种平衡度量指标，不仅可以指示预测结果与真实结果之间的相关系数，还可以处理数据集不平衡的情况；F1得分是为了平衡查全率和查准率而被提出的，是它们的调和平均数；AUC是ROC曲线与坐标轴所围面积的值，而ROC曲线是以FPR为横轴，TPR为纵轴所做；PR-AUC是 PR曲线与坐标轴所围面积的值，而PR曲线是以查全率为横轴，查准率为纵轴所做。

## 在线预测工具

除了OpenPPI_predictor需要下载外，其他工具都直接提供了Web页面。

- BIPS
- OpenPPI_predictor
- PrePPI
- PIP
- PSOPIA
- HIPPIE
- MEGADOCK-Web

## 未来发展方向

数据的质量和多模态

- 目前绝大多数预测模型都是遵循监督学习的范式提出的，训练集数据的质量是决定蛋白质相互作用预测准确性的关键问题。因此我们有必要在实验过程中对相互作用数据进行预处理，以确保数据的质量，从而更加准确地对预测模型性能进行评估。同时，除了相互作用数据外，我们还可以考虑蛋白质其他的生物学信息，来抵消对相互作用数据高度依赖导致的负面影响。因此，如何有效地整合多种生物信息资源以进行蛋白质相互作用预测仍然是未来需要解决的主要挑战之一。

蛋白质互作的周期性（动态数据）

- 蛋白质相互作用在不同的细胞中和同一个细胞的不同周期中都是不同的，未来研究应该集中在预测动态相互作用上面。随着热邻近共聚技术（Thermal Proximity Coaggregation，TPCA）和下一代测序技术如RNA-seq的发展，我们可以利用TPCA特征和RNA-seq的时间序列数据设计新的动态预测模型。

不同物种之间的蛋白质互作

- 多数模型通常使用酵母或人类数据集评估蛋白质相互作用预测模型的性能，而存在共同进化的证据表明，相互作用的某些进化模式在不同物种之间是保守的。因此，未来的工作可集中在根据进化模式预测不同物种中的相互作用。

如何整合多组学数据

- 由于高通量技术的发展，多组学数据可能为预测蛋白质相互作用提供额外的证据，但是相互作用与多组学数据之间的关系仍有待深入研究，如何有效地将多组学数据与机器学习技术结合起来是成功预测蛋白质相互作用的关键步骤。

算法突破，对生物学信息有效建模

- 在基于网络预测蛋白质相互作用时，很少有模型会考虑局部网络结构和全局网络结构的互补性。此外，如何将蛋白质的生物学信息整合到相互作用网络中仍然是蛋白质相互作用预测需要解决的难题。随着属性图聚类算法的兴起，将局部和全局网络结构与属性图聚类算法结合可能是未来蛋白质相互作用预测的重点。

## 参考

[基于计算学方法的蛋白质相互作用预测综述](https://mp.weixin.qq.com/s/FWZ-La3eOg_1_tuOV8CkWg)