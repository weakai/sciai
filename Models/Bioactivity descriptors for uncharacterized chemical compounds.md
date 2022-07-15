---
title: "Bioactivity descriptors for uncharacterized chemical compounds"
zotero: zotero://select/items/1_89HDDWU8
doi: 10.1038/s41467-021-24150-4
---

化学信息学的核心：chemical descriptors。但是缺少bioactivity descriptors。

## Chemical Checker

该方法是基于 Chemical Checker（CC）开发的

从本质上讲，Chemical Checker是存放在公共领域的小分子数据的替代表示，因此，它也受到实验数据的可应用性以及源数据库覆盖范围的限制。

从每个分子的 25 种生物活性空间中收集信息

然而，对于大多数分子来说，这种关于作用机制的高度详细的信息是不完整的；这意味着对于特定的分子，可能仅存在1-2种生物活性空间的信息

研究人员将所有可用的实验信息与深度机器学习方法相结合，开发了新工具。该工具可以对分子的所有活动概况（从化学到临床水平）进行预测分析。

该团队将主要的化学、基因组学和药物数据库整合到Chemical Checker的单一资源中，这是迄今为止可用的最大的小分子生物活性特征集合

### CC signaturizers

简单的 128D-vectors，与使用多维描述符来表示分子结构的 CDD工具包兼容。

这种兼容性，可以将C C signaturizers输出的生物信息融入相似性搜索、化学空间的可视化、聚类和属性预测，以及其他使用广泛的 CDD 任务

CC signaturizers推断的生物活性特征可用于注释大部分未表征的化学库，并丰富了化合物集合中针对药物靶标的活性信息

## Demo

该工具在一个基本无特征的化合物库中识别化合物方面的附加值，并通过实现一系列特征-活性关系（signature–activity relationship，SigAR）模型来预测分子的生物物理学信息和生理特性。

## Computational drug discovery

For short: CDD

分子的有效数学表示是所有 CDD 方法的关键，二维结构指纹图谱是许多情况下的默认选择。

## 评估与测试

为了评估结果的稳健性，研究人员将化学描述符的集合扩展到 ECFP（extended connectivity fifingerprints）之外；特别是，加入了 Daylight-like (RDKit) 指纹、MACCS 密钥和一个名为 CDDD 的数据驱动相关的先进描述符。此外，研究人员使用基于 AutoML TPOT的「模型不可知方法」重复了 SigAR 任务预测。

