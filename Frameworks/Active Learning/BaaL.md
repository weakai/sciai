# BaaL

## Monte-Carlo Dropout

- [Good | Monte-Carlo Dropout，蒙特卡罗 dropout](https://www.cnblogs.com/wuliytTaotao/p/11509634.html)

一种从贝叶斯理论出发的 Dropout 理解方式，将 Dropout 解释为高斯过程的贝叶斯近似。

但其实，MC dropout 用起来就简单了，不需要修改现有的神经网络模型，只需要神经网络模型中带 dropout 层，无论是标准的 dropout 还是其变种，如 drop-connect，都是可以的。

在训练的时候，MC dropout 表现形式和 dropout 没有什么区别，按照正常模型训练方式训练即可。

在测试的时候，在前向传播过程，神经网络的 dropout 是不能关闭的。这就是和平常使用的唯一的区别。

MC dropout 的 MC 体现在我们需要对同一个输入进行多次前向传播过程，这样在 dropout 的加持下可以得到“不同网络结构”的输出，将这些输出进行平均和统计方差，即可得到模型的预测结果及 uncertainty。而且，这个过程是可以并行的，所以在时间上可以等于进行一次前向传播。

### 神经网络产生的 softmax 概率不能表示 uncertainty？

其实我们在很多时候都拿了 softmax 的概率计算 uncertainty，比如主动学习查询策略中的 least confident、margin、entropy。
在 entropy 策略下，softmax 的概率越均匀熵越大，我们就认为 uncertainty 越大；反之，在 softmax 某一维接近 1，其它都接近 0 时，uncertainty 最小。

但是，softmax 值并不能反应该样本分类结果的可靠程度。

> A model can be uncertain in its predictions even with a high softmax output.

以 MNIST 分类为例，当模型在验证集上面效果很烂的时候，将一张图片输入到神经网络，我们仍然可以得到很高的 softmax 值，这个时候分类结果并不可靠；
当模型在验证集上效果很好了，在测试集上甚至都很好，这个时候，我们将一张图片加入一些噪声，或者手写一个数字拍成照片，输入到网络中，这个时候得到一个较高的 softmax 值，我们就认为结果可靠吗？
我们这个时候可以理解为，在已知的信息中，模型认为自己做的挺好，而模型本身并不能泛化到所有样本空间中去，对于它没有见过的数据，它的泛化能力可能不是那么强，这个时候模型仍然是以已知的信息对这个没有见过的数据有很强的判断（softmax 某一维值很大），当然有时候判断很好，但有时候判断可能就有误，而模型并不能给出对这个判断有多少 confidence。

而 MC dropout 可以给出一个预测值，并给出对这个预测值的 confidence，这也就是贝叶斯深度学习的优势所在。



## Links

- [DOCS](https://baal.readthedocs.io/en/latest/user_guide/index.html)