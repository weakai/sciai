# Circle Loss

- [如何理解与看待在 cvpr2020 中提出的 circle loss？](https://www.zhihu.com/question/382802283)
- [从最优化的角度看待 Softmax 损失函数](https://zhuanlan.zhihu.com/p/45014864)
- [Circle Loss 感想](https://zhuanlan.zhihu.com/p/121803965)

首先第一点 circle loss 是基于 triplet loss，改进而来的一种新型的 loss 定义。它的改进点在于，原本 triplet loss 对于正负样本采用平均用力的方式进行优化。使得在模型收敛的时候，对于正负样本的区分力度不够。Circle loss 在正负样本对加入了一个权重，控制正负样本对各自的梯度贡献，最后就可以得到一个更有区分力度的模型。
随后第二点，circle loss 在实现的过程中，同样借鉴了arc face 定义的夹角 margin。增大了模型对于各个分类也就是人脸id在训练过程的难度. 最终体现出来其模型有更强的区分力度。
接下来第三点，虽然circle loss是基于triplet loss的一种升级版的 loss 定义。在实现的过程中，如果数据不是成对的pairs ，根据作者的定义，也可以将这些基于分类的数据转化为正负样本对的Pairs. 因此circle loss。也可以叫做是一种提出来新型的Unified loss.
