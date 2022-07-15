# GLUE

[GLUE基准数据集介绍及下载](https://zhuanlan.zhihu.com/p/135283598)

自然语言处理（NLP）
主要自然语言理解（NLU）
自然语言生成（NLG）。

为了让NLU任务发挥最大的作用，来自纽约大学、华盛顿大学等机构创建了一个多任务的自然语言理解基准和分析平台，也就是 GLUE（General Language Understanding Evaluation）。

GLUE包含九项NLU任务，语言均为英语。

GLUE九项任务涉及到自然语言推断、文本蕴含、情感分析、语义相似等多个任务。像BERT、XLNet、RoBERTa、ERINE、T5等知名模型都会在此基准上进行测试。目前，大家要把预测结果上传到官方的网站上，官方会给出测试的结果。

GLUE的[论文](https://zhuanlan.zhihu.com/p/135283598#ref_1)为：GLUE: A Multi-Task Benchmark and Analysis Platform for Natural Language Understanding

GLUE的官网为：https://gluebenchmark.com/

二、任务介绍

GLUE共有九个任务，分别是CoLA、SST-2、MRPC、STS-B、QQP、MNLI、QNLI、RTE、WNLI。如下图图2所示，可以分为三类，分别是单句任务，相似性和释义任务，

![](https://pic2.zhimg.com/80/v2-2948db2c0d3a56b2282a01c954277d15_720w.jpg)

图2：GLUE九大任务的描述和统计。所有任务都是单句或者句子对分类，除了STS-B是一个回归任务。MNLI有3个类别，所有其他分类任务都是2个类别。测试集中加粗的表示测试集中标签从未在公共论坛等场所展示过

## CoLA

CoLA(The Corpus of Linguistic Acceptability，语言可接受性语料库)，单句子分类任务，语料来自语言理论的书籍和期刊，每个句子被标注为是否合乎语法的单词序列。本任务是一个二分类任务，标签共两个，分别是0和1，其中0表示不合乎语法，1表示合乎语法。

样本个数：训练集8, 551个，开发集1, 043个，测试集1, 063个。

任务：可接受程度，合乎语法与不合乎语法二分类。

评价准则：Matthews correlation coefficient。

注意到，这里面的句子看起来不是很长，有些错误是性别不符，有些是缺词、少词，有些是加s不加s的情况，各种语法错误。但我也注意到，有一些看起来错误并没有那么严重，甚至在某些情况还是可以说的通的。

## SST-2

SST-2(The Stanford Sentiment Treebank，斯坦福情感树库)，单句子分类任务，包含电影评论中的句子和它们情感的人类注释。这项任务是给定句子的情感，类别分为两类正面情感（positive，样本标签对应为1）和负面情感（negative，样本标签对应为0），并且只用句子级别的标签。也就是，本任务也是一个二分类任务，针对句子级别，分为正面和负面情感。

样本个数：训练集67, 350个，开发集873个，测试集1, 821个。

任务：情感分类，正面情感和负面情感二分类。

评价准则：accuracy。

注意到，由于句子来源于电影评论，又有它们情感的人类注释，不同于CoLA的整体偏短，有些句子很长，有些句子很短，长短并不整齐划一。

## MRPC

MRPC(The Microsoft Research Paraphrase Corpus，微软研究院释义语料库)，相似性和释义任务，是从在线新闻源中自动抽取句子对语料库，并人工注释句子对中的句子是否在语义上等效。类别并不平衡，其中68%的正样本，所以遵循常规的做法，报告准确率（accuracy）和F1值。

样本个数：训练集3, 668个，开发集408个，测试集1, 725个。

任务：是否释义二分类，是释义，不是释义两类。

评价准则：准确率（accuracy）和F1值。

## STSB

STSB(The Semantic Textual Similarity Benchmark，语义文本相似性基准测试)，相似性和释义任务，是从新闻标题、视频标题、图像标题以及自然语言推断数据中提取的句子对的集合，每对都是由人类注释的，其相似性评分为0-5(大于等于0且小于等于5的浮点数，原始paper里写的是1-5，可能是作者失误）。任务就是预测这些相似性得分，本质上是一个回归问题，但是依然可以用分类的方法，可以归类为句子对的文本五分类任务。

样本个数：训练集5, 749个，开发集1, 379个，测试集1, 377个。

任务：回归任务，预测为1-5之间的相似性得分的浮点数。但是依然可以使用分类的方法，作为五分类。

评价准则：Pearson and Spearman correlation coefficients。