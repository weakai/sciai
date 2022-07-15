---
title: BART
tags: [BART, model]
---

Facebook AI

[Denoising Sequence-to-Sequence Pre-training for Natural Language Generation, Translation, and Comprehension](https://www.aminer.cn/pub/5dbab2523a55acea3c05b02b?conf=acl2020)

[github fairseq](https://github.com/pytorch/fairseq/tree/master/examples/bart)

预训练 sequence-to-sequence 的去噪自编码器

训练步骤：

1. 使用任意噪声函数破坏文本

2. 模型学习重建原始文本

文中评估了多种噪声方法，最终发现通过随机打乱原始句子的顺序，再使用首创的新型文本填充方法(即用单个 mask token 替换文本片段，换句话说不管是被mask掉多少个token，都只用一个特定的 mask token 表示该位置有 token 被遮蔽了)能够获取最优性能。

BART 尤其擅长处理文本生成任务，不过在自然语言理解任务中也颇有可圈可点之处。

## BART 模型

BART = Transformer 架构 + BERT(双向编码器) + GPT(从左至右的解码器)

1. BERT 双向和 GPT 的自回归
2. Transformer 对模型进行预训练

[BART, GPT and BERT](https://image.jiqizhixin.com/uploads/editor/05c40e91-031b-4811-8372-a87e9ad59521/640.png)

BERT: 用掩码替换随机 token，双向编码文档。由于缺失 token 被单独预测，因此 BERT 较难用于生成任务

GPT: 使用自回归方式预测 token，这意味着GPT可用于生成任务。但是，该模型仅基于左侧上下文预测单词，无法学习双向交互。

BART：编码器输入与解码器输出无需对齐，即允许任意噪声变换。使用掩码符号替换文本段，从而破坏文本。使用双向模型编码被破坏的文本（左），然后使用自回归解码器计算原始文档的似然（右）。至于微调，未被破坏的文档是编码器和解码器的输入，研究者使用来自解码器最终隐藏状态的表征。

### 交叉关注 cross-attention

Just as in Transformer sequence to sequence model

## 参考

[ACL2020 | BART：请叫我文本生成领域的老司机](https://www.jiqizhixin.com/articles/2020-09-24)