---
title: CTG
date: 2022-07-25
tags: [CTG]
---

上面我们主要关注一下各种 PLM 针对语言的生成，设计使用的何种预训练任务。首先我们最熟悉的 BERT，主要使用的是 MLM，所以使用BERT的话你只能做完形填空。而GPT家族则使用的经典的causal LM，本文称为SLM，因此可以不断生成流利的句子。T5/UniLM/ERINE等都使用了CTR（corrupted text reconstruction）任务，mask掉的是一个span，可以是连续的很多词，然后让模型学习去复原，因此比BERT的MLM能力更强。而mBART则更是使用了FTR（full text reconstruction），让模型的复原能力更进一步。

## Reference

- [盘点 Controllable Text Generation(CTG) 的进展](https://zhuanlan.zhihu.com/p/493321293)