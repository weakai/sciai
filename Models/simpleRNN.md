---
title: SimpleRNN
---

RNN 是 Recurrent Neural Network 的缩写，它就是实现了我们来维护中间信息，记录之前看到信息这样最简单的一个概念的模型。

Recurrent Neural Network = A network with a loop

## 优缺点

优点。处理 a sequence 或者 a timeseries of data points 效果比普通的 DNN 要好。中间状态理论上维护了从开头到现在的所有信息；
缺点。不能处理 long sequence/timeseries 问题。原因是梯度消失，网络几乎不可训练。所以也只是理论上可以记忆任意长的序列。