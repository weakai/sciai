---
title: optimizers
---

```python
# most cases
def configure_optimizers(self):
    opt = Adam(self.parameters(), lr=1e-3)
    return opt

# multiple optimizer case (e.g.: GAN)
def configure_optimizers(self):
    generator_opt = Adam(self.model_gen.parameters(), lr=0.01)
    disriminator_opt = Adam(self.model_disc.parameters(), lr=0.02)
    return generator_opt, disriminator_opt

# example with learning rate schedulers
def configure_optimizers(self):
    generator_opt = Adam(self.model_gen.parameters(), lr=0.01)
    disriminator_opt = Adam(self.model_disc.parameters(), lr=0.02)
    discriminator_sched = CosineAnnealing(discriminator_opt, T_max=10)
    return [generator_opt, disriminator_opt], [discriminator_sched]

# example with step-based learning rate schedulers
def configure_optimizers(self):
    gen_opt = Adam(self.model_gen.parameters(), lr=0.01)
    dis_opt = Adam(self.model_disc.parameters(), lr=0.02)
    gen_sched = {'scheduler': ExponentialLR(gen_opt, 0.99),
                 'interval': 'step'}  # called after each training step
    dis_sched = CosineAnnealing(discriminator_opt, T_max=10) # called every epoch
    return [gen_opt, dis_opt], [gen_sched, dis_sched]

# example with optimizer frequencies
# see training procedure in `Improved Training of Wasserstein GANs`, Algorithm 1
# https://arxiv.org/abs/1704.00028
def configure_optimizers(self):
    gen_opt = Adam(self.model_gen.parameters(), lr=0.01)
    dis_opt = Adam(self.model_disc.parameters(), lr=0.02)
    n_critic = 5
    return (
        {'optimizer': dis_opt, 'frequency': n_critic},
        {'optimizer': gen_opt, 'frequency': 1}
    )

```

```python
# 设置优化器
def configure_optimizers(self):
    weight_decay = 1e-6  # l2正则化系数
    # 假如有两个网络，一个 encoder 一个 decoder
    optimizer = optim.Adam([{'encoder_params': self.encoder.parameters()}, {'decoder_params': self.decoder.parameters()}], lr=learning_rate, weight_decay=weight_decay)
    # 同样，如果只有一个网络结构，就可以更直接了
    optimizer = optim.Adam(my_model.parameters(), lr=learning_rate, weight_decay=weight_decay)
    # 我这里设置 2000 个 epoch 后学习率变为原来的 0.5，之后不再改变
    StepLR = torch.optim.lr_scheduler.MultiStepLR(optimizer, milestones=[2000], gamma=0.5)
    optim_dict = {'optimizer': optimizer, 'lr_scheduler': StepLR}
    return optim_dict
```

https://mp.weixin.qq.com/s?__biz=MzAwNjU0NjA3Ng==&mid=2247493007&idx=1&sn=a7d10c4de9e0f7562145f679d8da9582&chksm=9b09127cac7e9b6a9923111bafb87cd1d3ebf7b7c0b8adcd2c745affbd9e8cdd558ba12f5a9d&mpshare=1&scene=1&srcid=0621ufsUrQcky5OqBodaj4Ew&sharer_sharetime=1655769650495&sharer_shareid=f66ef27ade3d509229b2afd8611df712#rd

- https://www.jianshu.com/p/26a7dbc15246