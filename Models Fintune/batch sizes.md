# Batch size

#### Batch size 对模型训练的影响

- 大的 batch size 其实是比较有运行效率的
- 小的 batch 其实对 testing acc 是有帮助的
  - 小的 batch 其实是更有利于收敛到 Flat minima，因为在计算 gradient 的时候会有很多的 Noisy，这些 Noisy 的 gradient 使得曲线很有可能跳出 Sharp minima，而优化到 Flat minima 的位置
  - ![](https://mmbiz.qpic.cn/mmbiz_jpg/AIR6eRePgjMZu15JEHsNZiaVZXs0A9CAsKw1LXpdyNl1ePb4Kx6dGr8iaz8o8hZonAMsqMkia8GqUHUxcromcLuTg/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1&wx_co=1)

![](https://mmbiz.qpic.cn/mmbiz_png/AIR6eRePgjMZu15JEHsNZiaVZXs0A9CAsBwmS4vqS7rAUmDbWIFj9GnqtGZiaSkUrx3FdC8pH17STxg4pZecXgIQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

## 参考

- [【他山之石】如何选择模型训练的batch size和learning rate](https://mp.weixin.qq.com/s/B7Uo7QxOHKI3StVwTXWUQg)