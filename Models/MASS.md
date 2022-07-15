---
title: MASS
tags: [BERT, seq2seq, MASS]
---

MASS的主要贡献是提出一种新的Pre-train seq2seq任务的方法。

BERT的成功把nlp带向了pretrain+finetune时代，而对于文本生成任务（机器翻译、文本摘要、生成问答），由于语料对较少，更需要使用pretrain的模型来减少标注代价。

看到这里的读者可以先自己想一下如何pretrain seq2seq的任务。大家首先能想到的估计就是BERT+LM，因为BERT的编码能力比其他BiLM的能力强一些。但这样pretrain的问题就是，如果我们的语料是unsupervised，就要分开预训练encoder和decoder，可能会导致两者的分布不一致。

把BERT和GPT统一了起来。

## 实验细节

1. 语言：因为要应用到机器翻译任务，所以预训练模型采用4种语言，作者把四种语言同时进行Byte-Pair Encoding，生成一个60k的词表。在预训练的时候也是同时用四种语言的语料，即一个batch是32*4个句子。
2. Mask策略：从随机一个位置开始，连续mask掉句子长度50%的tokens（经过实验验证较优）。且参考BERT的策略，80%的时间用[M]，10%用随机的token，10%保留原token。
3. Decoder优化：因为预测的token都是连续的，在训练decoder时可以直接把padding去掉，但要注意保留positional encoding，这样可以节省50%的时间。
4. 预训练LR=1e-4，NMT的精调LR=1e-4。（感觉精调的LR应该小一些吧）

## 结论

1. MASS达到了机器翻译的新 SOTA
2. MASS > BERT+LM 式的预训练
3. mask 掉连续的 token，可以得到更好的语言建模能力（已实验验证）
4. 只让 decoder 从 encoder 侧获取信息，得到的模型效果更好（已实验验证）

总体来讲，MASS还是给我开阔了新的思路（毕竟我没什么思路），其实仔细想这个想法也是不难想出来的，关键还是要动手去验证，并且花心思去提升效果，细节见英雄。

## 参考

[BERT生成式之MASS解读](https://zhuanlan.zhihu.com/p/67687640)