---
title: Prediction of Organic Reaction Outcomes Using Machine Learning
date: 2017-8-18
---

工具：
模板提取器
候选物排序
数据增强：如何添加可信的负例反应。

model:
- framework: reaction templates + neural networks (machine
learning)
- task: anticipating reaction outcomes

baseline:
- radius-2 Morgan circular fingerprints of length 1024

dataset:
- 1976−2013_USPTO: 15 000 experimental reaction records
- 格式化为 CML documents

finetuning and results:
- In a 5-fold cross-validation, the trained model assigns the major product rank 1 in 71.8% of cases, rank ≤3 in 86.7% of cases, and rank ≤5 in 90.8% of cases.

new:
- ranking a self-generated list of candidates
- data augmentation strategy chemically plausible negative reaction examples
- automatically extracted forward synthesis templates
- new reaction representation focused on the fundamental transformation at the reaction site rather than constituent reactant and product fingerprints
- diminish the bias of high-yielding reactions

## Methods

#### Canonicalization
- wrong atom numbering
- remove duplicates
- remove reactions and templates with low frequency

Finally, combined into a single reactant pool (1689 templates).

### Candidate ranking: Scoring
4 types:
H gains and loses; bond gains and loses

## knowledge

representation:
- end-to-end templates: rigid reaction templates
- graph-based representations
- reactant fingerprints

forward synthesis prediction: Forward Enumeration
- heuristics-driven algorithm(Law et al. and Bogevig et al.)
- knowledge-graph approach: half reactions

## Challenges and resolutions
positive examples challenges:
- The literature is heavily biased toward reactions with high yields, and reactions with negligible or zero yields are rarely reported except for illustrative purposes, e.g., highlighting the necessity of a catalyst.
- Proprietary electronic lab notebooks can contain numerous
unproductive reactions, including results of high-throughput
screening, but these data are not available in public or even
commercial databases.

Challenge:
A reaction is a complex data structure for which there is no universal vector-based description

Solution:
new edit-based reaction representation strategy that emphasizes the change in atom connectivity that occurs at the reaction core during a chemical reaction

Keys:
edit-based, reaction core

Questions:

edit-based?
how to get top-5 ranks acc.



