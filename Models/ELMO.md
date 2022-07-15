### ELMO

- Embedding from Language Models
- 论文：Deep contextualized word representation
- 它通过**无监督**的方式对语言模型进行预训练来学习单词表示
- 思路：用深度的双向 Language Model 在大量未标注数据上训练语言模型，如下图所示



在deep contextualized这个短语，一个是deep，一个是context，其中context更关键。在此之前的Word Embedding本质上是个静态的方式，所谓静态指的是训练好之后每个单词的表达就固定住了，以后使用的时候，不论新句子上下文单词是什么，这个单词的Word Embedding不会跟着上下文场景的变化而改变，所以对于比如Bank这个词，它事先学好的Word Embedding中混合了几种语义 ，在应用中来了个新句子，即使从上下文中（比如句子包含money等词）明显可以看出它代表的是“银行”的含义，但是对应的Word Embedding内容也不会变，它还是混合了多种语义。这是为何说它是静态的，这也是问题所在。

ELMO的本质思想是：我事先用语言模型学好**一个单词**的 Word Embedding，此时多义词无法区分，不过这没关系。在我实际使用Word Embedding的时候，单词已经具备了特定的上下文了，这个时候我可以根据上下文单词的语义去调整单词的Word Embedding表示，这样经过调整后的Word Embedding更能表达在这个上下文中的具体含义，自然也就解决了多义词的问题了。所以ELMO本身是个根据当前上下文对Word Embedding动态调整的思路。

> RLMO 模型

![RLMO 模型](https://pic4.zhimg.com/80/v2-fe335ea9fdcd6e0e5ec4a9ac0e2290db_720w.jpg)

ELMO采用了典型的两阶段过程，第一个阶段是利用语言模型进行预训练；
第二个阶段是在做下游任务时，从预训练网络中提取对应单词的网络各层的Word Embedding作为新特征补充到下游任务中。

网络结构：双层双向LSTM

目前语言模型训练的任务目标是根据单词 $W_i$ 的上下文去正确预测单词 $W_i$ ， $W_i$ 之前的单词序列 Context-before 称为上文，之后的单词序列 Context-after 称为下文。
图中左端的前向双层 LSTM 代表正方向编码器，输入的是从左到右顺序的除了预测单词外 $W_i$ 的上文 Context-before；
右端的逆向双层LSTM代表反方向编码器，输入的是从右到左的逆序的句子下文Context-after；每个编码器的深度都是两层LSTM叠加。这个网络结构其实在NLP中是很常用的。使用这个网络结构利用大量语料做语言模型任务就能预先训练好这个网络，如果训练好这个网络后，输入一个新句子 $S_{new}$ ，句子中每个单词都能得到对应的三个 Embedding:最底层是单词的 Word Embedding，往上走是第一层双向LSTM中对应单词位置的Embedding，这层编码单词的句法信息更多一些；再往上走是第二层LSTM中对应单词位置的Embedding，这层编码单词的语义信息更多一些。也就是说，**ELMO的预训练过程不仅仅学会单词的Word Embedding，还学会了一个双层双向的LSTM网络结构，而这两者后面都有用**。

> Feature-based Pre-Training

ELMO给下游提供的是每个单词的特征形式

QA 问题，对问句 X，我们可以先将句子 X 作为预训练好的 ELMO 网络的输入，这样句子 X 中每个单词在 ELMO 网络中都能获得对应的三个 Embedding，之后给予这三个 Embedding 中的每一个 Embedding 一个权重 a，这个权重可以学习得来，根据各自权重累加求和，将三个Embedding 整合成一个。然后将整合后的这个 Embedding 作为 X 句在自己任务的那个网络结构中对应单词的输入，以此作为补充的新特征给下游任务使用。对于上图所示下游任务QA中的回答句子Y来说也是如此处理。

ELMO 引入上下文动态调整单词的 embedding 后解决了多义词问题效果超过预期。原因是第一层 LSTM 编码了很多句法信息，这在这里起到了重要作用。

![效果](https://pic3.zhimg.com/80/v2-360d10468d7a8878627c68780fe6d502_720w.jpg)

> ELMO 有什么值得改进的缺点

首先，一个非常明显的缺点在**特征抽取器**选择方面，ELMO 使用了 LSTM 而不是新贵 Transformer
Transformer 是谷歌在17年做机器翻译任务的 “Attention is all you need” 的论文中提出的，引起了相当大的反响，很多研究已经证明了Transformer 提取特征的能力是要远强于LSTM的。
如果 ELMO 采取 Transformer 作为特征提取器，那么估计 Bert 的反响远不如现在的这种火爆场面。
另外一点，ELMO 采取**双向拼接**这种**融合特征的能力**可能比 Bert **一体化的融合特征方式**弱，但是，这只是一种从道理推断产生的怀疑，目前并没有具体实验说明这一点。

ELMO 的训练方法和图像领域的预训练方法在模式还是有很大差异的。除了以 ELMO 为代表的这种基于特征融合的预训练方法外，NLP 里还有一种典型做法，这种做法和图像领域的方式就是看上去一致的了，一般将这种方法称为“基于Fine-tuning的模式”，而 GPT 就是这一模式的典型开创者。

