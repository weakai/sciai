# Learning rate 对模型训练的影响

RMSprop -> Adam -> AdamW -> learning rate scheduling

## 学习率和 batchsize 的关系

- batchsize 变大 k 倍，学习率也要相应变大 $\sqrt{k}$ 倍，本质是为了梯度的方差保持不变；
- 学习率直接影响模型的收敛状态，batchsize则影响模型的泛化性能。
- 学习率决定了权重迭代的步长，因此是一个非常敏感的参数，它对模型性能的影响体现在两个方面，第一个是初始学习率的大小，第二个是学习率的变换方案。
- 衰减学习率可以通过增加 batchsize 来实现类似的效果，这实际上从 SGD 的权重更新式子就可以看出来两者确实是等价的，文中通过充分的实验验证了这一点。
- 如果增加了学习率，那么 batch size 最好也跟着增加，这样收敛更稳定。
- 尽量使用大的学习率，因为很多研究都表明更大的学习率有利于提高泛化能力。如果真的要衰减，可以尝试其他办法，比如增加 batch size，学习率对模型的收敛影响真的很大，慎重调整。

## 参考

- [【他山之石】如何选择模型训练的batch size和learning rate](https://mp.weixin.qq.com/s/B7Uo7QxOHKI3StVwTXWUQg)