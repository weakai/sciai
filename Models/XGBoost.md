---
title: XGBoost
date: 2022-3-9
tags: [XGBoost, 机器学习, 分类, 回归, 排序, 树]
---

## 相关知识点

[理解 Bias-Variance（偏差-方差）权衡 | 控制过拟合 | 处理不平衡的数据](https://xgboost.apachecn.org/#/docs/8)

## Setup

```bash
pip install xgboost  -i https://pypi.tuna.tsinghua.edu.cn/simple
```

## All kinds of XGBoost

### DART booster

[介绍 | DART booster](https://xgboost.apachecn.org/#/docs/5)

XGBoost 主要是将大量带有较小的 Learning rate (学习率) 的回归树做了混合。
在这种情况下，在构造前期增加树的意义是非常显著的，而在后期增加树并不那么重要。
Rasmi 等人从深度神经网络社区提出了一种新的方法来增加 boosted trees 的 dropout 技术，并且在某些情况下能得到更好的结果。

[原始论文 | Rashmi Korlakai Vinayak, Ran Gilad-Bachrach. “DART: Dropouts meet Multiple Additive Regression Trees.” | JMLR](http://www.jmlr.org/proceedings/papers/v38/korlakaivinayak15.pdf)

## 参数

[参数调整注意事项](https://xgboost.apachecn.org/#/docs/8)

xgboost 的参数可以分为三类

- 通用参数/general parameters
- 集成(增强)参数/booster parameters
- 任务参数/task parameters

## 接口

XGBoost 有两大类接口

- XGBoost 原生接口 
- scikit-learn 接口

- 并且 XGBoost 能够实现**分类**和**回归**两种任务


## Portals

### Learn

- [github | xgboost](https://github.com/dmlc/xgboost)
- [XGBoost Documentation](https://xgboost.readthedocs.io/en/stable/)
- [5 | ApacheCN | XGBoost 中文文档](https://xgboost.apachecn.org/#/)

### Practice
- [Awesome XGBoost | examples, tutorials, blogs about XGBoost usecases](https://github.com/tqchen/xgboost/tree/master/demo)
- [Xgboost 简易入门教程](https://zhuanlan.zhihu.com/p/131578968)
- [基于 XGBoost 原生接口和 Scikit-learn 接口的分类和回归](https://zhuanlan.zhihu.com/p/31182879)
- [Python 软件包介绍](https://xgboost.apachecn.org/#/docs/13)


