# 时序任务

时序模型相比一般的回归模型有着特殊性,比如传统方法中要求稳定性(Jenkins-box方法的使用)和样本采样的限制.

## 基本规则法

抽取时序的周期进行拟合

## 传统参数法

传统的参数预测方法可以分为两种，一种拟合标准时间序列的餐顺方法，包括移动平均，指数平均等；另一种是考虑多因素组合的参数方法，即AR,MA,ARMA等模型。这类方法比较适用于小规模，单变量的预测，比如某门店的销量预测等。总的来说，基于此类方法的建模步骤是：

首先需要对观测值序列进行平稳性检测，如果不平稳，则对其进行差分运算直到差分后的数据平稳；
在数据平稳后则对其进行白噪声检验，白噪声是指零均值常方差的随机平稳序列；
如果是平稳非白噪声序列就计算ACF（自相关系数）、PACF（偏自相关系数），进行ARMA等模型识别，
对已识别好的模型，确定模型参数，最后应用预测并进行误差分析。

这类方法一般是统计或者金融出身的人用的比较多，对统计学或者随机过程知识的要求比较高。而在数据挖掘的场景中比较难适用，因为需要大量的参数化建模。比如有一个连锁门店的销售数据，要预测每个门店的未来销量，用这类方法的话就需要对每个门店都建立模型， 这样就很难操作了。

## 时间序列分解

时间序列分解法是数年来一直非常有用的方法，一个时间序列往往是一下几类变化形式的叠加或耦合：

长期趋势(Secular trend, T)：长期趋势指现象在较长时期内持续发展变化的一种趋向或状态。
季节变动(Seasonal Variation, S)：季节波动是由于季节的变化引起的现象发展水平的规则变动
循环波动(Cyclical Variation, C)：循环波动指以若干年为期限，不具严格规则的周期性连续变动
不规则波动(Irregular Variation, I): 不规则波动指由于众多偶然因素对时间序列造成的影响

> Facebook 所服务化的时间序列预测工具，Prophet，具体可以参考官网说明。该方法类似于 STL 时序分解的思路，增加考虑节假日等信息对时序变化的影响。

## 机器学习

近年来时间序列预测方法，多采用机器学习方式。机器学习的方法，主要是构建样本数据集，采用“时间特征”到“样本值”的方式，
通过有监督学习，学习特征与标签之前的关联关系，从而实现时间序列预测。

## 深度学习

深度学习方法近年来逐渐替代机器学习方法，成为人工智能与数据分析的主流，对于时间序列的分析，有许多方法可以进行处理，包括：循环神经网络-LSTM模型/卷积神经网络/基于注意力机制的模型（seq2seq）

### 循环神经网络

### 卷积神经网络

传统的卷积神经网络（CNN）一般认为不太适合时序问题的建模，这主要由于其卷积核大小的限制，不能很好的抓取长时的依赖信息。但是最近也有很多的工作显示，特定的卷积神经网络结构也可以达到很好的效果，通常将时间序列转化为图像，再应用基于卷积神经网络的模型做分析。

### 时间卷积网络

### 基于注意力机制的模型

###  结合 CNN+RNN+Attention，作用各不相同互相配合

## 参考

- [时间序列预测方法最全总结！](https://cloud.tencent.com/developer/article/1800614)