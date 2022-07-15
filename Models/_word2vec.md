---
title: word2vec
---



### Word Embedding 考古史

> Word Embedding 之前

一般NLP里面做预训练一般的选择是用语言模型任务来做。

NNLM 神经网络语言模型的思路
现在看其实很简单，见过RNN、LSTM、CNN后的你们回头再看这个网络甚至显得有些简陋。

2013年最火的用语言模型做Word Embedding的工具是Word2Vec，后来又出了Glove。

Word2Vec 的网络结构其实和 NNLM 是基本类似的

> Word2Vec 有两种训练方法

CBOW，核心思想是从一个句子里面把一个词抠掉，用这个词的上文和下文去预测被抠掉的这个词

Skip-gram，和 CBOW 正好反过来，输入某个单词，要求网络预测它的上下文单词。

而你回头看看，NNLM是怎么训练的？是输入一个单词的上文，去预测这个单词。这是有显著差异的。为什么 Word2Vec 这么处理？原因很简单，因为Word2Vec 和 NNLM 不一样，NNLM 的主要任务是要学习一个解决语言模型任务的网络结构，语言模型就是要看到上文预测下文，而 word embedding 只是无心插柳的一个副产品。但是 Word2Vec 目标不一样，它单纯就是要 word embedding 的，这是主产品，所以它完全可以随性地这么去训练网络。

使用Word2Vec或者Glove，通过做语言模型任务，就可以获得每个单词的Word Embedding

Word Embedding 本质上是一种标准的预训练过程。

> 下游任务如何使用 Word Embedding

Word Embedding 等价于什把 Onehot 层到 embedding 层的网络用预训练好的参数矩阵 Q 初始化了。
这跟前面讲的图像领域的低层预训练过程其实是一样的，区别无非 Word Embedding 只能初始化第一层网络参数，再高层的参数就无能为力了。
下游 NLP 任务在使用 Word Embedding 的时候也类似图像有两种做法，一种是Frozen，就是Word Embedding那层网络参数固定不动；另外一种是 Fine-Tuning，就是 Word Embedding 这层参数使用新的训练集合训练也需要跟着训练过程更新掉。

> Word Embedding 存在什么问题，所以效果并没有显著性

笼罩的乌云：多义词问题
word embedding无法区分多义词的不同语义，这就是它的一个比较严重的问题

ELMO 提供了一种简洁优雅的解决方案

### 