# AutoML

## AutoML 介绍

AutoML 框架执行的任务可以被总结成以下几点:

1. 预处理和清理数据。
2. 选择并构建适当的特征。
3. 选择合适的模型。
4. 优化模型超参数。
5. 设计神经网络的拓扑结构（如果使用深度学习）。
6. 机器学习模型后处理。
7. 结果的可视化和展示。

### AutoML 的优势

Gartner 在之前的 AI 平台报告中也指出，对于专业数据科学家，AutoML 能够提高他们的工作效率，减少在手动调参方面的时间投入。
对于平民数据科学家，或者说更擅长业务领域知识的数据分析人员，
AutoML 能帮助他们快速应用机器学习的能力，不需要再掌握机器学习的复杂技术细节，
就能达到业界数据科学家的平均水平。在此基础上，很多之前没有机会应用机器学习的项目也开始可能出现正向的 ROI 回报，
推动各行各业的 AI 应用落地，而不只是集中在头部场景和 high tech 公司中。

## AutoML 框架

### AutoGluon

一句话点评：目前最好的 domain specific autoML 框架方案。

### Autokeras

AutoKeras 是一个基于 Keras 的 AutoML 系统，只需几行代码就可以实现神经架构搜索（NAS）的强大功能。
它由德克萨斯 A&M 大学的 DATA 实验室开发，以 TensorFlow 的 tf.keras API 和 Keras 为基础进行实现。

AutoKeras 可以支持不同的任务，例如图像分类、结构化数据分类或回归等。

### Auto-Sklearn

**本着入门一般选 SKLearn 的原则，先聊聊 AutoSKLearn**
开箱即用的自动化机器学习库。
以 scikit-learn 为基础，自动搜索正确的学习算法并优化其超参数。
通过元学习、贝叶斯优化和集成学习等搜索可以获得最佳的数据处理管道和模型。

### FLAML

A Fast and Lightweight AutoML Library

AutoML 在近年来的各类机器学习和 Kaggle 比赛中获得了不少的成功。
而 FLAML 是今年由微软主推的一个全新的高效轻量级自动化机器学习框架。

AutoML 的框架很多，与目前 SOTA 的一些 automl 框架不同的是，FLAML 重点在于 lightweight，
将 hyper-parameter, learner, sample size 都考虑到 cost 里，
而 cost 除了每一个 trial 的 CPU 时间还包括做 cross-validation 或者 hold-out 的时间。

- [微软 AutoML 框架之 FLAML](https://zhuanlan.zhihu.com/p/410362009)

### NNI

从项目的名字可以看出这个框架主要的重心还是在优化神经网络类的模型方面，而且整体的定位是偏通用框架方向。
**一句话点评：NN 模型的最佳 autoML 通用框架**

### Optuna

Optuna 是一个特别为机器学习设计的自动超参数优化软件框架。
传统搜参工具 hyperopt 或者 BayesianOptimization 的进阶版本。
轻量级，经验固化的调优 pipeline，设计简单，效率还不错

**一句话点评：如果只想要一个快速灵活的调参工具，就选 Optuna。**

主要特点

1. 轻量级、多功能和跨平台架构
2. 并行的分布式优化
3. 对不理想实验 (trial) 的剪枝 (pruning)
4. 超参数重要性
5. 集成新的 CMA-ES 采样
6. 集成 MLflow

与第三方框架的集成 (Integration):

1. LightGBM, XGBoost, PyTorch, TensorFlow
2. MLflow
3. Redis

### TPOT

TPOT（Tree-based Pipeline Optimization Tool）是一个 Python 自动化机器学习工具，
跟 hyperopt 等属于比较早期的 autoML 框架
它使用遗传算法（非贝叶斯优化）优化对机器学习的流程进行优化。
它也是基于 Scikit-Learn 提供的方法进行数据转换和机器学习模型的构建，但是它使用遗传算法编程进行随机和全局搜索。
另外 TPOT 的整体代码风格上也比较随意，不像是工业界的成熟作品，理解起来会有点困难。
**一句话点评：可变 pipeline 的构建和代码生成值得学习。**

- [TPOT 原理 | 深度解析 AutoML 框架——TPOT：一键生成ML代码，释放双手](https://zhuanlan.zhihu.com/p/85974357)

### H2O AutoML

H2O 的 AutoML 可用于在用户指定的时间限制内自动训练和调整许多模型。
H2O 提供了许多适用于 AutoML 对象（模型组）以及单个模型的可解释性方法。
可以自动生成解释，并提供一个简单的界面来探索和解释 AutoML 模型。


## Links

- [AutoML 框架概览](https://zhuanlan.zhihu.com/p/378722852)
- [走马观花 AutoML](https://zhuanlan.zhihu.com/p/212512984)
- [awesome AutoML](https://github.com/windmaple/awesome-AutoML)