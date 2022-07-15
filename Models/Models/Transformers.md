# A Survey of Transformers

2022-4-17

## Preface

Three types of Transformers

1. Encoder-Decoder 常用于 Sequence to Sequence 的建模（如机器翻译)
2. Encoder-Only 只用了前面的编码器，常用于分类，序列标注问题
   - Bert: 使用了掩码语言建模
   - Next sentence prediction(NSP): 自监督训练目标
   - RoBERTa: 删除了 NSP 因为 NSP 会降低下游任务的性能
3. Decoder-Only 只用了后面的解码器，常用于序列生成任务
   - GPT Series
   - BART: 在 BERT 基础上引入去噪目标

![transformers family tree](https://cdn.jsdelivr.net/gh/xxzhai123/img/imgchapter03_transformers-compact.png)

### Basic Knowledge about Transformers

归纳偏置

CNN 使用卷积核，引入图像局部性。
RNN 将时间不变性引入。
而 Transformer 抛弃了这些归纳偏置，一方面能让其足够通用灵活，另一方面 Transformer 很容易对小规模数据过拟合。
另一个与其相关的是 GNN 图网络，Transformer 可以被看作一个完全有向图(带自环)上的 GNN，其中每个输入都是图中的一个节点(PS: 笔者对GNN不理解，这里翻译比较僵硬)。

## Transformer

### Self Attention

Pros of self-attention

1. 它与全连接层有相同的最大路径长度，适合长距离建模。
2. 相比卷积有限的感受野（在CNN中，需要堆叠很多层才能得到全局感受野），self-attention 能以有限的层数建模长期依赖关系。
3. 相比于循环层（RNN 系列），self-attention 的并行度更高。

Cons of self-attention

1. 复杂度，在处理长序列的时候计算开销很大
2. 结构先验，抛弃了所有归纳偏置，导致其在小型数据容易过拟合

Improvements can be:

1. Sparse Attention，将稀疏偏置引入到注意力计算
2. Linearized Attention，将注意力矩阵和特征映射分离，降低至线性复杂度
3. 显存压缩，减少 QKV 的数量来减小注意力矩阵
4. 低秩 self Attention，这类工作主要是抓住自注意力的低秩性
5. 带有先验的 Attention，使用预先注意力分配来补充标准的自注意力机制
6. 改进 Multi-head 机制

#### Sparse Attention

在一些训练好的 Transformer 模型中，可以观察到注意力矩阵通常是稀疏的，因此可以通过限制 query-key 对的数量来减少计算复杂度。
我们可以将稀疏化方法进一步分成两类，基于位置信息 Position-based 和基于内容 Content-based 两种方法。

主要来说有以下五种稀疏注意力的基本模式

1. Global Attention 为了增强模型模拟长距离依赖关系，我们可以加入一些全局节点。
2. Band Attention 大部分数据都带有局部性，我们限制 query 只与相邻的几个节点进行交互
3. Dilated Attention 跟 CNN 中的 Dilated Conv 类似，通过增加空隙以获取更大的感受野
4. Random Attention 通过随机采样，提升非局部的交互
5. Block Local Attention 使用多个不重叠的 Block 来限制信息交互

![基础的稀疏注意力模式](https://cdn.jsdelivr.net/gh/xxzhai123/img/imgv2-3e6aa0e9d6b0adfc60adc3acf520e35c_720w.jpg)

此外，Sparse Adaptive Connection 将序列看作是一个图，自适应学习稀疏连接。
Sparse Sinkhorn Attention 使用一个排序网络，给 query，key 分到多个块内，并给每个 query 块分配一个key块，每个 query 只允许和对应的 key 进行交互。

#### Linearized Attention

这类方法主要是使用 $\Phi(Q)\Phi(K)^T$ 来近似或替代计算 Attention 中的 $QK^T$，以降低计算复杂度至 $O(T)$。(其中 $\Phi$ 是在行方向上的特征映射)

### 其他模块级别的修改

#### 位置表达

1. 绝对位置编码
2. 相对位置编码
   - Transformer-XL 重新设计 attention score 计算方式，并引入正弦编码
3. 其他位置表示方式
   - TUPE 重新设计计算注意力方式，并且引入一个 bias 来表征位置信息
   - Roformer 使用的是旋转位置编码，通过向量的旋转来表示相对位置
   - 没有采取显式编码的位置表示: R-Transformer 每一个块里，首先输入到一个 RNN 中，再进入到注意力模块。RNN 能给输入信息带上位置信息。
   - Decoder 中的位置表示: Decoder 中的 masked self-attention 并不是排列不变的，也有研究者发现移除了 decoder 部分的位置编码能够提升模型性能

#### Layer Normalization

LN 放置的位置: post-LN and post-LN

LN 的一些替代品: AdaNorm

#### Point-wise 前馈层

激活函数
- Ramachandran 使用 swish 激活函数替代
- GPT 中使用了 GELU
- Shazeer 等人使用了 GLU(Gated Linear Unit)

获取更大容量的 FFN

丢弃 FFN

### 架构级别的变体

轻量化 Transformer
- LiteTransformer: 将原始 Attention 分为两个分支，一个分支继续采用 attention 机制捕捉长距离上下文，另一个分支使用 depthwise 卷积捕捉局部信息
- Funnel Transformer: 引入池化操作减少序列长度，随后使用上采样恢复序列
- DelighT: 替换了原始的 Block

增强跨网络块的连接

自适应调整计算时间(Adaptive Computation Time)

使用分治策略的 Transformer
- 循环Transformer
- 层级Transformer

- [A Survey of Transformer 一份 Transformer 综述](https://zhuanlan.zhihu.com/p/380510105)

## BERT

bert 处理的序列有长度限制，如最大长度 512 个字符，如果更长的文本怎么处理呢？

NLP主要分为NLU和NLG两大类任务（很多NLP大咖这样描述过）



- [后 Bert 时代 NLP 相关进展](https://zhuanlan.zhihu.com/p/67314622)

## BART

Bidirectional and Auto-Regressive

发表时间: 2019.10.29

背景：由于基于 BERT 类的生成模型大多在预训练和下游任务之间存在一定的差异，导致生成模型的结果并不理想，本文提出了 BART 模型，更加充分去利用上下文信息和自回归特点。

主要贡献： BART提出了一个结合双向和自回归的预训练模型。
BART 模型首先使用任意噪声来破坏原文本，然后学习模型重构原文本。
这样使得，BART 不仅很好的处理文本生成任务，同时理解任务上的表现也不错。

- [生成式预训练之 BART](https://zhuanlan.zhihu.com/p/173858031)
- [BART论文解读](https://zhuanlan.zhihu.com/p/270925325)

## DeBERTa

微软

Transformer 已经成为神经语言建模中最有效的神经网络结构。

DeBERTa 对 BERT 模型做了两个修改：
1. DeBERTa 使用一种分离的注意机制来进行自我注意
2. DeBERTa 在预训练时增强了 BERT 的输出层
  - 在模型预训练过程中，将 BERT 的输出 Softmax 层替换为一个增强的掩码解码器（EMD）来预测被屏蔽的令牌

训练 trick
1. 加入了一些数据扰动
2. mask 策略中不替换词，变为替换成词的 pos embedding

文章要解决的问题: 改善 BERT 和 RoBERTa 预训练效率，并提升下游指标

最终效果: DeBERTa比RoBERTa和BERT更高效，在多数 NLP 任务上也获取更好的结果。DeBERTa 仅用一半预训练数据即可在众多 NLP 任务中超越 RoBERTa-Large

## M2M-100

Facebook 开源了 M2M-100 模型，据称这是“第一个可以直接在任何 100 种语言之间进行翻译的多语言机器翻译（MMT）模型”。

- [ M2M-100 GitHub](https://github.com/pytorch/fairseq/tree/main/examples/m2m_100)

## BigBird

它使用 block sparse attention 替换了原类似 Bert 一样的全注意力机制，在与 BERT 一样的计算力情况下，可以处理的序列长度达到 4096。

它已经在很多长文本序列的任务上达到 SOTA 效果，例如长文本摘要、长文本问答。

## T5

T5 一统江湖?

### 解决 Text-to-Text 的三类结构

1. Encoder-Decoder@T5
   - 适合 NLG
2. Language model
   - 适合 NLU
3. Prefix LM
   - 适合 NLU


### 注意力掩码机制

#### 1 Casual

常见的掩码机制，有从左到右和从右到做两种，当前点仅能看到改点之前的信息，看不到后面的信息。
如果要看到两个方向的信息只能将两个信息 concat（如BiLSTM模型），GPT就是采用这种掩码机制。

#### 2 Casual prefix

Casual prefix 可以看成是 Casual 的扩展，应该有些常见 prefix 固定，UniLM 采用这种掩码机制。

#### 3 Fully-visible

建模时同时看两边。在F ully-visible 之前大部分模型都是选择 Casual 这种。
Bert 采用 Fully-visible 还是很有创新意义。

### T5 简介

T5 是一个集大成之作，将 Bert 之后的很多进展进行了融合。
但从模型上说，它的创新度并不是很高，主要是 Encoder-Decoder、Fully visible、Bert-span等。
这些单项之前在某些模型都有涉及。
但 T5 能将它们各种组合在一起，选出了一个最优组合方案。

T5 也叫叫“Bert-2”，大方向是更复杂的模型和更多的数据。但是，它只把 NLG 和 NLU 融合了。
其中，NLG 问题 UniLM 和 MASS 两篇文章也是往这个方向。




- [Google 预训练语言模型 T5](https://zhuanlan.zhihu.com/p/88727133)



