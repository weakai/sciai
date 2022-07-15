---
title: Correction to Automated Chemical Reaction Extraction from Scientific Literature
---

By correction it means to add the missing reference in previously published paper.

## 那么漏了谁呢？
1. ChEMU, Cheminformatics Elsevier Melbourne University
2. ChemPatent, a pretrained ELMo model developed for improving NER in patents.

### What is the author's tool
```yaml
schema: product-centric
principle: consistent with existing reaction databasing
efforts such as Reaxys
```

### What is ChEMU

```yaml
aims: [reaction extraction from chemical patents]
tasks:
  - NER
  - event extraction
schema: synthesis action sequences
```

## 如何弥补

对于 ChEMU，尽管功能一致，但是设计理念不同。阐明这点
对于 ChemPatent，补测一组数据，表格展示。

## 术语

NER named entity recognition

This difference is rooted in our design principles