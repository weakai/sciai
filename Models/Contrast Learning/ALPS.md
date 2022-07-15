---
title: ALPS
github: https://github.com/forest-snow/alps
paper: https://arxiv.org/abs/2010.09535
---

## 0 Preliminaries

### 0.1 Warm-starting required

Require a model already fine-tuned on downstream task.

A cold-starting model does not require.

## 1 Introduction

The main contribution of the paper is an active learning algorithm called ALPS (Active Learning through Processing Surprisal) that is based on  the language modeling objective.

What is cold-start?

AL strategy depends on warm-starting the model with information about the task. Thus, a fitting solution to AL for deep classifiers is a cold-start approach, one that does not rely on classification loss or confidence scores.

What is cold-start model?

The model encodes syntactic properties, acts as a database for general world knowledge, and can detect out-of-distribution examples .

ALPS model

- based on BERT-base
- only relies on a self-supervised loss function.

Baselines

warm-start methods

- Entropy
- BADGE
- BERT-KM
- FT-BERT-KM

