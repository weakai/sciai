---
title: LSTM
---

LSTM 就是用来解决 RNN 中梯度消失问题的，从而可以处理 long-term sequences。

LSTM 是 SimpleRNN 的变体，它解决了梯度消失的问题。

## 优缺点

优点。解决了SimpleRNN梯度消失的问题，可以处理long-term sequence。
缺点。计算复杂度高，想想谷歌翻译也只是 7-8 层 LSTM 就知道了；自己跑代码也有明显的感觉，比较慢。

## 最佳实践指南

 RNN 表达能力
有的时候RNN的表达能力有限，为了增加RNN的表达能力，我们可以stack rnn layers来增加其表达能力。希望大家了解这是一种常用的做法。 当然了，中间的RNN layer必须把每个时刻的输出都记录下来，作为后面RNN层的输入。

过拟合
RNN LSTM 同样会过拟合。这个问题直到 2015 年，在博士 Yarin Gal 在他的博士论文里给出了解决办法：类似 dropout。但是在整个 timesteps 上使用同一个固定的 drop mask。






## 参考

[LSTM 原理与实践，原来如此简单](https://zhuanlan.zhihu.com/p/51870057)