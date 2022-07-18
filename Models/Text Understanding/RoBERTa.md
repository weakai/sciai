# RoBERTa

RoBERTa 的全称叫做 Robustly optimized BERT approach。RoBERTa 之于 BERT 的改动很简单，主要是用了更多的数据，训练上，采用动态【MASK】、去掉下一句预测的 NSP 任务、更大的 batch_size、文本编码。

## 动态 MASK

预训练的每一个 step，是重新挑选 15% token进行【MASK】的，而 BERT 是固定的，就是对于同一个输入样本，在不同的 epoch，输入是一样的，实验结果见下图，有很微弱的提升吧。

## 去 NSP 任务

## 更大 batch_size