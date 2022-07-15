# 分布式计算框架

实际科研、业务中，气象数据往往体积大得惊人。
为了更快速高效地处理这些数据，我们需要使用一些并行的手段。

## 分布式计算框架 历史

分布式计算框架大家一定都耳熟能详，诸如离线计算的 Hadoop（map-reduce），spark，流式计算的 strom,Flink 等。
相对而言，这些计算框架都依赖于其他大数据组件，安装部署也相对复杂。

## Dask

Dask 是 Anaconda 的产品，背后的主要贡献者是 Matthew Rocklin。
Dask 是一个并行计算库，可扩展现有的 Python 生态系统。
Dask 和核心是弥补 python 在数据科学中的不足，主要是性能上。
Python 单机的能力不能够支持数据科学中大数据集的快速计算。
Dask 提供了基础的数据结构，底层是分布式计算架构。
数据结构包括：Array 、Dataframe。Array 兼容 numpy 的ndarray，Dataframe 兼容 Pandas 的 Dataframe 。

Dask由两部分组成：
针对计算优化的动态任务调度。这与 Airflow，Luigi，Celer 或 Make 类似，但针对交互式计算工作负载进行了优化。

## RAY

Ray 是 UC Berkeley RISELab 出品的机器学习分布式框架。
具有比Spark更优异的计算性能，而且部署和改造更简单，同时支持机器学习和深度学习的分布式训练。
支持主流的深度学习框架（pytorch，tensorflow，keras等）
UC Berkeley 教授 Ion Stoica 写了一篇文章：The Future of Computing is Distributed。
里面详细说了 Ray 产生的原由。总结一下，就是由于 AI 和大数据的快速发展，对于应用和硬件能力的要求提出了更高的挑战。

RAY 架构的简单介绍可以参考
- [机器学习分布式框架 Ray](#Links)
- [Paper | 2017](https://arxiv.org/abs/1712.05889)

Ray 目前库支持超参数调优 Ray tune， 梯度下 降Ray SGD，推理服务 RaySERVE, 分布式数据 Dataset 以及分布式增强学习 RLlib。
详细内容请看 [机器学习分布式框架 Ray](#Links)

### Apache Arrow 和 Plasma

Apache Arrow 是列式内存数据结构，已经成为数据处理领域最通用的数据结构。
Ray 的底层内存数据结构的基础是 Apache Arrow（Hugging Face 的 Datasets 库的基础也是 Apache Arrow）。
而 Dask 是 xarray/xray。
xray 是 NumFocus 赞助的开源类似 numpy 的数据结构。
Apache Arrow 拥有更好的生态，被大部分的数据处理系统接受，非常有利于和其他系统的融合。
Apache Arrow 是列式内存数据结构，已经成为数据处理领域最通用的数据结构。
Arrow 最突出的特点就是生态非常丰富和性能出众。
Arrow 的背后还有一位 committer 和 co-creator，叫 Wes McKinney。
Wes McKinney 是 pandas 的作者，所以 Arrow 对 Pandas 的支持也非常好。

Ray 团队基于 Arrow 开发了一个内存数据服务，叫做 Plasma。
Plasma 在 Linux 共享内存创建了 Arrow 封装的对象，单独作为一个进程运行。
其他进程可以通过 Plasma Client Library 来访问这块共享内存里的 Arrow 存储。
这个功能是 Ray 团队开发的，贡献给 Arrow 作为 Arrow 生态的一部分。
除了 Plasma ，Ray 团队还贡献了一个叫做 Modin 的功能，就是基于 Ray 的分布式能力，提供了 Pandas 的实现。
类似于 Dask 的 Dataframe。
这个功能已经从 Ray 独立，成为一个独立的项目。

## Links

- [Dask tutorial | zh | by ](https://blog.perillaroc.wang/post/2021/07/2021-07-10-dask-tutorial-overview/)
- [GitHub Ray](https://github.com/ray-project/ray)
- [7 年 AI 大佬告诉你 Hadoop 与 Spark：有什么区别？](https://zhuanlan.zhihu.com/p/395444161)
- [开源史海钩沉系列 [1] Ray：分布式计算框架 | 墙裂推介阅读](http://gaocegege.com/Blog/why-do-i-like-ray)
- [机器学习分布式框架 Ray](https://jishuin.proginn.com/p/763bfbd656e3)
- [高性能分布式执行框架——Ray | 入门推介](https://www.cnblogs.com/fanzhidongyzby/p/7901139.html)
- [Ray 分布式计算框架详解 | 进阶推介](https://cloud.tencent.com/developer/news/685491)
- [分布式计算框架 Ray 论文导读 | 进阶推介](https://juejin.cn/post/7068672574531731469)