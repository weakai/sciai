---
title: DeepAR
---

DeepAR 是 Amazon 于 2017 年提出的基于深度学习的[时间序列](https://so.csdn.net/so/search?q=时间序列&spm=1001.2101.3001.7020)预测方法，目前已集成到 [Amazon SageMaker](https://aws.amazon.com/cn/sagemaker/) 和 [GluonTS](https://arxiv.org/abs/1906.05264) 中。前者是 [AWS](https://so.csdn.net/so/search?q=AWS&spm=1001.2101.3001.7020) 的机器学习云平台，后者是 Amazon 开源的时序预测工具库。

传统的时间序列预测方法（ARIMA、Holt-Winters’ 等）往往针对一维时间序列本身建模，难以利用额外特征。此外，传统方法的预测目标通常是序列在每个时间步上的取值。与之相比，基于神经网络的 DeepAR 方法可以很方便地将额外的特征纳入考虑，且其预测目标是序列在每个时间步上取值的概率分布。在特定场景下，概率预测比单点预测更有意义。以零售业为例，若已知商品未来销量的概率分布，则可以利用运筹优化方法推算在不同业务目标下的最优采购量，从而辅助决策（详见《如何在商品采购中考虑不确定性》、《报童问题》以及《报童问题的简单解法》）。

本文将简要地介绍一下 DeepAR 模型，并给出一个 demo。如果希望了解更多细节，建议直接阅读 Amazon 的论文 Deep AR: Probabilistic Forecasting with Autoregressive Recurrent Networks。
