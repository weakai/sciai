---
title:
key:
  - Transformer
---

# Transformer

## 简介

Transformer 近两年非常火爆，内容也很多，要想讲清楚，还涉及一些基于该结构的预训练模型，例如著名的 BERT，GPT，以及刚出的 DALL·E 等。

它们都是基于 Transformer 的上层应用，因为 Transformer 很难训练，巨头们就肩负起了造福大众的使命，开源了各种好用的预训练模型。

我们都是站在巨人肩膀上学习，用开源的预训练模型在一些特定的应用场景进行迁移学习。

### 为什么放弃 CNN 与 RNN？

RNN 相关算法只能从左向右依次计算或者从右向左依次计算，这种机制带来了两个问题：

1. 时间片 $t$ 的计算依赖 $t-1$ 时刻的计算结果，这样限制了模型的并行能力；
2. 顺序计算的过程中信息会丢失，尽管 LSTM 等门机制的结构一定程度上缓解了长期依赖的问题，但是对于特别长期的依赖现象,LSTM 依旧无能为力。

Attention 机制，将序列中的任意两个位置之间的距离是缩小为一个常量；其次它不是类似 RNN 的顺序结构，因此具有更好的并行性，符合现有的 GPU 框架。论文中给出 Transformer 的定义是：Transformer is the first transduction model relying entirely on self-attention to compute representations of its input and output without using sequence aligned RNNs or convolution。

- Transformer
  - 抛弃了传统的 CNN 和 RNN，整个网络结构完全是由 Attention 机制组成
    - 由且仅由 self-Attenion 和 Feed Forward Neural Network 组成
  - 一个基于 Transformer 的可训练的神经网络可以通过堆叠 Transformer 的形式进行搭建，作者的实验是通过搭建编码器和解码器各 6 层，总共 12 层的 Encoder-Decoder，并在机器翻译中取得了 BLEU 值得新高。

## 解码器与编码器

**编码器**负责把自然语言序列映射成为隐藏层（上图第 2 步），即含有自然语言序列的数学表达。

**解码器**把隐藏层再映射为自然语言序列，从而使我们可以解决各种问题，如情感分析、机器翻译、摘要生成、语义关系抽取等。

## Transformer 详解

### 高层 Transformer

编码器由 6 个编码 block 组成，同样解码器是 6 个解码 block 组成。与所有的生成模型相同的是，编码器的输出会作为解码器的输入
![编码解码](https://cdn.jsdelivr.net/gh/xxzhai123/img/img/20210508084219.png)
在 Transformer 的 encoder 中，数据首先会经过一个叫做`self-attention`的模块得到一个加权之后的特征向量 $Z$ ，这个 $Z$ 便是：
$$ Attention(Q,K,V)=softmax(\frac{QK^T}{\sqrt{d_k}})V=Z $$
得到 $Z$ 之后，它会被送到 encoder 的下一个模块，即 `Feed Forward Neural Network(前馈神经网络)`。这个全连接有两层，第一层的激活函数是`ReLU`，第二层是一个线性激活函数，可以表示为：

$$ FFN(Z)=max(0,ZW_1+b_1)W_2+b_2 $$
Decoder 的结构如图 5 所示，它和 encoder 的不同之处在于 Decoder 多了一个 Encoder-Decoder Attention，两个 Attention 分别用于计算输入和输出的权值：

Self-Attention：当前翻译和已经翻译的前文之间的关系；
Encoder-Decnoder Attention：当前翻译和编码的特征向量之间的关系。

![Encoder-Decoder](https://cdn.jsdelivr.net/gh/xxzhai123/img/img/20210508084956.png)

### 输入编码

### Q,K,V 概念

Query，Key，Value 的概念取自于信息检索系统，举个简单的搜索的例子来说。当你在某电商平台搜索某件商品（年轻女士冬季穿的红色薄款羽绒服）时，你在搜索引擎上输入的内容便是 Query，然后搜索引擎根据 Query 为你匹配 Key（例如商品的种类，颜色，描述等），然后根据 Query 和 Key 的相似度得到匹配的内容（Value)。

### 损失层

解码器解码之后，解码的特征向量经过一层激活函数为 $softmax$ 的全连接层之后得到反映每个单词概率的输出向量。此时我们便可以通过 CTC 等损失函数训练模型了。

而一个完整可训练的网络结构便是 encoder 和 decoder 的堆叠（$N=6$ ）：
![完整的Transformer的结构](https://cdn.jsdelivr.net/gh/xxzhai123/img/img/20210508090635.png)

### 位置编码

截止目前为止，我们介绍的 Transformer 模型并没有捕捉顺序序列的能力，也就是说无论句子的结构怎么打乱，Transformer 都会得到类似的结果。换句话说，Transformer 只是一个功能更强大的词袋模型而已。

为了解决这个问题，论文中在编码词向量时引入了位置编码（Position Embedding）的特征。具体地说，位置编码会在词向量中加入了单词的位置信息，这样 Transformer 就能区分不同位置的单词了。

| 数据                  | 维度                                             |
| :-------------------- | :----------------------------------------------- |
| 输入数据 $X$          | batch size, sequence length                      |
| 字向量$X_{embedding}$ | batch size, sequence length, embedding dimension |

embedding dimension 的大小由 Word2Vec 算法决定，Tranformer 采用 512 长度的字向量。

文字的位置信息很重要，Tranformer 没有类似 RNN 的循环结构，没有捕捉顺序序列的能力。

为了保留这种位置信息交给 Tranformer 学习，我们需要用到位置嵌入
加入位置信息的方式非常多，最简单的可以是直接将绝对坐标 0,1,2 编码。

Tranformer 采用的是 sin-cos 规则，使用了 sin 和 cos 函数的线性变换来提供给模型位置信息：

$$
PE_{(pos,2i)}=sin(pos/10000^{2i/dmodel} ) \\
PE_{(pos ,2i+1)}=cos( pos /10000^{2i/dmodel} )
$$

上式中 `pos` 指的是句中字的位置，取值范围是 `[0, 𝑚𝑎𝑥 𝑠𝑒𝑞𝑢𝑒𝑛𝑐𝑒 𝑙𝑒𝑛𝑔𝑡ℎ)`，`i` 指的是字嵌入的维度, 取值范围是 `[0, 𝑒𝑚𝑏𝑒𝑑𝑑𝑖𝑛𝑔 𝑑𝑖𝑚𝑒𝑛𝑠𝑖𝑜𝑛)`。 就是 `𝑒𝑚𝑏𝑒𝑑𝑑𝑖𝑛𝑔 𝑑𝑖𝑚𝑒𝑛𝑠𝑖𝑜𝑛` 的大小。

最后，将 $X_{embedding}$ 和 位置嵌入 相加，送给下一层。
![笔记](https://cdn.jsdelivr.net/gh/xxzhai123/img/img/dl-basics-3-10.png)

多头的意义在于，Q，K，T 三个矩阵 合称注意力矩阵，它可以表示每个字与其他字的相似程度。因为，向量的点积值越大，说明两个向量越接近。
![注意力矩阵](https://cdn.jsdelivr.net/gh/xxzhai123/img/img/dl-basics-3-11.png)

需要注意的是，在上面 𝑠𝑒𝑙𝑓 𝑎𝑡𝑡𝑒𝑛𝑡𝑖𝑜𝑛 的计算过程中，我们通常使用 𝑚𝑖𝑛𝑖 𝑏𝑎𝑡𝑐ℎ，也就是一次计算多句话，上文举例只用了一个句子。

每个句子的长度是不一样的，需要按照最长的句子的长度统一处理。对于短的句子，进行 Padding 操作，一般我们用 0 来进行填充。
![填充](https://cdn.jsdelivr.net/gh/xxzhai123/img/img/dl-basics-3-12.png)

### 差链接和层归一化

加入了残差设计和层归一化操作，目的是为了防止梯度消失，加快收敛。

1. 残差设计
   我们在上一步得到了经过注意力矩阵加权之后的 𝑉， 也就是 𝐴𝑡𝑡𝑒𝑛𝑡𝑖𝑜𝑛(𝑄, 𝐾, 𝑉)，我们对它进行一下转置，使其和 𝑋𝑒𝑚𝑏𝑒𝑑𝑑𝑖𝑛𝑔 的维度一致, 也就是 [𝑏𝑎𝑡𝑐ℎ 𝑠𝑖𝑧𝑒, 𝑠𝑒𝑞𝑢𝑒𝑛𝑐𝑒 𝑙𝑒𝑛𝑔𝑡ℎ, 𝑒𝑚𝑏𝑒𝑑𝑑𝑖𝑛𝑔 𝑑𝑖𝑚𝑒𝑛𝑠𝑖𝑜𝑛] ，然后把他们加起来做残差连接，直接进行元素相加，因为他们的维度一致:
   $$X_{embedding}+Attention(Q, K, V)$$
   在之后的运算里，每经过一个模块的运算，都要把运算之前的值和运算之后的值相加，从而得到残差连接，训练的时候可以使梯度直接走捷径反传到最初始层：
   $$X+SubLayer(X)$$
2. 层归一化
   作用是把神经网络中隐藏层归一为标准正态分布，也就是 𝑖.𝑖.𝑑 独立同分布， 以起到加快训练速度， 加速收敛的作用。
   $$\mu_i=\frac{1}{m}\sum_{i=1}^mx_{ij}$$
   上式中以矩阵的行 (𝑟𝑜𝑤) 为单位求均值：
   $$\sigma_j^2=\frac{1}{m}\sum_{i=1}^m(x_{ij}-\mu_j)^2$$
   上式中以矩阵的行 (𝑟𝑜𝑤) 为单位求方差：
   $$LayerNorm(x)=\alpha\odot\frac{x_{ij}-\mu_i}{\sqrt{\sigma_i^2+\epsilon}}+\beta$$
   然后用每一行的每一个元素减去这行的均值，再除以这行的标准差，从而得到归一化后的数值，ϵ 是为了防止除 0；

之后引入两个可训练参数$\alpha$,$\beta$ 来弥补归一化的过程中损失掉的信息，注意 ⊙ 表示元素相乘而不是点积，我们一般初始化$\alpha$为全 1，而 $\beta$ 为全 0。

### 向前网络

其实就是两层线性映射并用激活函数激活，比如说 ReLU

## Glossary

1. 注意力（Attention）机制

   - 2014 年，由 Bengio 团队提出

2. BERT 算法

   - 用于生成词向量

3. 隐藏层

   - 单个隐藏层
     - 隐藏层的意义就是把输入数据的特征，抽象到另一个维度空间，来展现其更抽象化的特征，这些特征能更好的进行线性划分。
   - 多个隐藏层
     - 多个隐藏层其实是对输入特征多层次的抽象，最终的目的就是为了更好的线性划分不同类型的数据（隐藏层的作用）。
   - 输入层是一些列的像素节点，然后刚开始这些层回答了关于输入像素点的很简单、很具体的问题，然后经过很多层，建立了更复杂和抽象的概念，这种带有两个或多个隐含层的神经网络，称为深度神经网络，deep neural networks，简称为 **DNN**。
     $$ 像素点 \rightarrow 若干像素点有机组合并形成一定意义(隐藏层) $$

4. 多头(heads)
   - 一组分割后的 Q，K，V
     $$
