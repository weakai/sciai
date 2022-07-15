# fastText

fastText 是 Facebook 于 2016 年开源的一个词向量计算和文本分类工具，在学术上并没有太大创新。
但是它的优点也非常明显，在文本分类任务中，fastText（浅层网络）往往能取得和深度网络相媲美的精度，却在训练时间上比深度网络快许多数量级。
在标准的多核 CPU 上， 能够训练 10 亿词级别语料库的词向量在 10 分钟之内，能够分类有着 30 万多类别的 50 多万句子在 1 分钟之内。

#### Softmax 回归

Softmax Regression 又被称作多项逻辑回归（multinomial logistic regression），它是逻辑回归在处理多类别任务上的推广。
逻辑回归是 softmax 回归在 K=2 时的特例。

#### 分层 Softmax

标准的 Softmax 回归中，要计算 y=j 时的 Softmax 概率： $P(y=j)$ ，我们需要对所有的 K 个概率做归一化，这在 $|y|$ 很大时非常耗时。于是，分层 Softmax 诞生了，它的基本思想是使用树的层级结构替代扁平化的标准 Softmax，使得在计算 $P(y=j)$ 时，只需计算一条路径上的所有节点的概率值，无需在意其它的节点。

通过分层的 Softmax，计算复杂度一下从 $|K|$ 降低到 $log|K|$。

#### n-gram 特征

将文本内容按照字节顺序进行大小为 N 的滑动窗口操作，最终形成长度为 N 的字节片段序列。

n-gram 中的 gram 根据粒度不同，有不同的含义。

n-gram 产生的特征只是作为文本特征的候选集，你后面可能会采用信息熵、卡方统计、IDF 等文本特征选择方式筛选出比较重要特征。

#### word2vec

word2vec 的 CBOW 模型架构和 fastText 模型非常相似

![](https://pic2.zhimg.com/80/v2-73089aa56a1ae39117e1a03e874487c9_720w.jpg)

因为词库 V 往往非常大，使用标准的 softmax 计算相当耗时，于是 CBOW 的输出层采用的正是上文提到过的分层 Softmax。

#### Architecture

![](https://pic2.zhimg.com/80/v2-7f38f23e98ee89d21fd16e34d5f07d69_720w.jpg)

和CBOW一样，fastText 模型也只有三层：输入层、隐含层、输出层（Hierarchical Softmax），输入都是多个经向量表示的单词，输出都是一个特定的 target，隐含层都是对多个词向量的叠加平均。不同的是，CBOW 的输入是目标单词的上下文，fastText 的输入是多个单词及其 n-gram 特征，这些特征用来表示单个文档；CBOW 的输入单词被 onehot 编码过，fastText 的输入特征是被 embedding 过；CBOW 的输出是目标词汇，fastText 的输出是文档对应的类标。

值得注意的是，fastText 在输入时，将单词的字符级别的 n-gram 向量作为额外的特征；在输出时，fastText 采用了分层 Softmax，大大降低了模型训练时间。这两个知识点在前文中已经讲过，这里不再赘述。

#### 核心思想

将整篇文档的词及 n-gram 向量叠加平均得到文档向量，然后使用文档向量做 softmax 多分类。这中间涉及到两个技巧：字符级 n-gram 特征的引入以及分层 Softmax 分类。

#### 关于分类效果

为何 fastText 的分类效果常常不输于传统的非线性分类器？

使用词 embedding 而非词本身作为特征，这是 fastText 效果好的一个原因；另一个原因就是字符级 n-gram 特征的引入对分类效果会有一些提升。




## References

- [fastText 原理及实践](https://zhuanlan.zhihu.com/p/32965521)