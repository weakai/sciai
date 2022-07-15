---
title: XLNet
---

## Good blog post

[XLNet:运行机制及和Bert的异同比较](https://zhuanlan.zhihu.com/p/70257427)

## 创新

Permutation Language Model，因为排列语言模型开启了自回归语言模型如何引入下文的一个思路。

XLNet 融合自回归语言模型（AR LM）和自编码语言模型（DAE LM）的优点。

XLNet = Bert + GPT 2.0 + Transformer XL

首先，它通过PLM预训练目标，吸收了Bert的双向语言模型；然后，GPT2.0的核心其实是更多更高质量的预训练数据，这个明显也被XLNet吸收进来了；再然后，Transformer XL的主要思想也被吸收进来，它的主要目标是解决Transformer对于长文档NLP应用不够友好的问题。

Bert这种自编码语言模型的好处是：能够同时利用上文和下文，所以信息利用充分。

XLNet 仍然遵循两阶段的过程，第一个阶段是语言模型预训练阶段；第二阶段是任务数据 Fine-tuning 阶段。它主要希望改动第一个阶段，就是说不像 Bert 那种带 Mask 符号的 Denoising-autoencoder 的模式，而是采用自回归 LM 的模式。
就是说，看上去输入句子 X 仍然是自左向右的输入，看到 Ti 单词的上文 Context_before，来预测 Ti 这个单词。但是又希望在 Context_before 里，不仅仅看到上文单词，也能看到 Ti 单词后面的下文 Context_after 里的下文单词，这样的话，Bert里面预训练阶段引入的 Mask 符号就不需要了，于是在预训练阶段，看上去是个标准的从左向右过程，Fine-tuning 当然也是这个过程，于是两个环节就统一起来。当然，这是目标。剩下是怎么做到这一点的问题。
那么，怎么能够在单词 Ti 的上文中 Contenxt_before 中揉入下文 Context_after 的内容呢？你可以想想。XLNet 是这么做的，在预训练阶段，引入 Permutation Language Model 的训练目标。什么意思呢？就是说，比如包含单词 Ti 的当前输入的句子 X，由顺序的几个单词构成，比如x1,x2,x3,x4四个单词顺序构成。我们假设，其中，要预测的单词 Ti 是 x3，位置在 Position 3，要想让它能够在上文 Context_before 中，也就是 Position 1 或者 Position 2 的位置看到 Position 4 的单词x4。可以这么做：假设我们固定住 x3 所在位置，就是它仍然在 Position 3，之后随机排列组合句子中的 4 个单词，在随机排列组合后的各种可能里，再选择一部分作为模型预训练的输入 X。比如随机排列组合后，抽取出 x4,x2，x3,x1这一个排列组合作为模型的输入 X。于是，x3就能同时看到上文 x2，以及下文 x4 的内容了。这就是 XLNet 的基本思想，所以说，看了这个就可以理解上面讲的它的初衷了吧：看上去仍然是个自回归的从左到右的语言模型，但是其实通过对句子中单词排列组合，把一部分 Ti 下文的单词排到 Ti 的上文位置中，于是，就看到了上文和下文，但是形式上看上去仍然是从左到右在预测后一个单词。

## 双流自注意力模型

通过Attention掩码，这里简单说下“双流自注意力机制”，一个是内容流自注意力，其实就是标准的Transformer的计算过程；主要是引入了Query流自注意力，这个是干嘛的呢？其实就是用来代替Bert的那个[Mask]标记的，因为XLNet希望抛掉[Mask]标记符号，但是比如知道上文单词x1,x2，要预测单词x3，此时在x3对应位置的Transformer最高层去预测这个单词，但是输入侧不能看到要预测的单词x3，Bert其实是直接引入[Mask]标记来覆盖掉单词x3的内容的，等于说[Mask]是个通用的占位符号。而XLNet因为要抛掉[Mask]标记，但是又不能看到x3的输入，于是Query流，就直接忽略掉x3输入了，只保留这个位置信息，用参数w来代表位置的embedding编码。其实XLNet只是扔了表面的[Mask]占位符号，内部还是引入Query流来忽略掉被Mask的这个单词。和Bert比，只是实现方式不同而已。