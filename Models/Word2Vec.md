# Word2Vec

## 简介

Word2Vec 是 google 在 2013 年推出的一个 NLP 工具，它的特点是能够将单词转化为向量来表示，这样词与词之间就可以定量的去度量他们之间的关系，挖掘词之间的联系。

$$文字 \rightarrow 字向量的转换$$

## History(历史)

最早的词向量采用 One-Hot 编码^[优：表示简单；缺：词汇表维护困难，且词汇之间无关系]，又称为一位有效编码，每个词向量维度大小为整个词汇表的大小，对于每个具体的词汇表中的词，将对应的位置置为 1。比如我们有下面的 5 个词组成的词汇表，
![One-Hot 编码](https://cdn.jsdelivr.net/gh/xxzhai123/img/img/v2-7722e8088339ef04b13d7707704304c0_720w.png)

`Distributed representation` 可以解决 `One-Hot` 编码存在的问题，它的思路是通过训练，将原来 `One-Hot` 编码的每个词都映射到一个较短的词向量上来，而这个较短的词向量的维度可以由我们自己在训练时根据任务需要来自己指定。

下图是采用 `Distributed representation` 的一个例子，我们将词汇表里的词用`"Royalty"`,`"Masculinity"`, `"Femininity"` 和 `"Age"` 4 个维度来表示，`King` 这个词对应的词向量可能是`(0.99,0.99,0.05,0.7)`。当然在实际情况中，我们并不能对词向量的每个维度做一个很好的解释。
![`Distributed representation](https://cdn.jsdelivr.net/gh/xxzhai123/img/img/v2-534146aea511f5a8563805683e45417d_720w.png)

有了用 Distributed Representation 表示的较短的词向量，我们就可以较容易的分析词之间的关系了，比如我们将词的维度降维到 2 维，有一个有趣的研究表明，用下图的词向量表示我们的词时，我们可以发现：

$$ \overrightarrow{King} - \overrightarrow{Man} + \overrightarrow{Woman} \approx \overrightarrow{Queen}$$
![有趣](https://cdn.jsdelivr.net/gh/xxzhai123/img/img/v2-f4b43e442a714addfc58658527012849_720w.png)

## Word2Vec 原理

Word2Vec 的训练模型本质上是只具有一个隐含层的神经元网络（如下图）。
![Word2Vec](https://cdn.jsdelivr.net/gh/xxzhai123/img/img/v2-acb489ea3c71bcf8d9ec46b3e47e6c25_720w.png)
Google 的 Mikolov 在关于 Word2Vec 的论文中提出了 CBOW 和 Skip-gram 两种模型，CBOW 适合于数据集较小的情况，而 Skip-Gram 在大型语料中表现更好。其中 CBOW 如下图左部分所示，使用围绕目标单词的其他单词（语境）作为输入，在映射层做加权处理后输出目标单词。与 CBOW 根据语境预测目标单词不同，Skip-gram 根据当前单词预测语境，如下图右部分所示。假如我们有一个句子“There is an apple on the table”作为训练数据，CBOW 的输入为（is,an,on,the），输出为 apple。而 Skip-gram 的输入为 apple，输出为（is,an,on,the）。
![](https://cdn.jsdelivr.net/gh/xxzhai123/img/img/v2-0d3bbbe2ab92b36d40ff0acb9170f311_720w.png)

[Word2Vec 详解](https://zhuanlan.zhihu.com/p/61635013)
