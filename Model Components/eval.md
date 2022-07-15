# torch.eval()

#### [PyTorch 51.BatchNorm 和 Dropout 层的不协调现象](https://mp.weixin.qq.com/s/U2xB4y6ArjVC6lzQJGVwmQ)

model.eval() 主要是用在模型前向过程中，通过设置成 eval 模型，告诉所有层你在 eval 模式，其中涉及到 batchnorm 和 dropout 层，
这些层在训练和测试的表现是不一样的，比如 dropout 在训练中可能是0-1 间的数，但在 eval 模式则为不使用 dropout 层。

torch.no_grad() 会关闭自动求导引擎的， 因此能节省显存，和加速。

model.train() 用于在训练阶段，model.eval() 用在验证和测试阶段，他们的区别是对于 Dropout 和 Batch Normlization 层的影响。
在 train 模式下，dropout 网络层会按照设定的参数 p 设置保留激活单元的概率（保留概率=p);
batchnorm 层会继续计算数据的 mean 和 var 等参数并更新。
在 val 模式下，dropout 层会让所有的激活单元都通过，而 batchnorm 层会停止计算和更新 mean 和 var，直接使用在训练阶段已经学出的 mean 和 var 值。

#### BN 和 Dropout 共同使用时会出现的问题

BN 和 Dropout 单独使用都能减少过拟合并加速训练速度，但如果一起使用的话并不会产生 1+1>2 的效果，相反可能会得到比单独使用更差的效果。
相关的研究参考论文： <https://arxiv.org/abs/1801.05134>
