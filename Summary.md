---
title: 炼丹心术
---

## 指标

### torch 的 auroc 和 accuracy 的输入

- torch 的 auroc 只能输入 logits, 维度是 [batch, n_class]，二分类时的维度是 [batch]，其中的单维是预测整类的 logit
- torch 的 accuracy 的输入可以是 auroc 的形式，也可以是 preds 的形式，也就是经过 argmax 之后的输出，维度是 [batch]，元素是标签索引。

#### PyTorch lightning and Torch Metrics

- epoch 结束时 acc, auc 要 reset

- acc_best 在开头的时候 reset
- acc prog_bar = True
- 一般 on_step = False, on_epoch = Ture
- `__call__` = update + compute

## 损失函数

- 输入进入损失函数前必须进行归一化
  - 二分类 -> sigmoid: $[b\in[-\infin, \infin]] \rightarrow [b\in[0,1]]$
  - 多分类 -> softmax: $[b,n_{class}] \rightarrow [b,n_{probs}]$
- BCELossWithLogits = Sigmoid + BCELoss
  - label 必须是 float
- CrossEntropyLoss = LogSoftmax + NLLLoss 
  - [为什么 LogSoftmax 比 Softmax 更好？](https://blog.csdn.net/m0_48742971/article/details/123495391?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-123495391-blog-82878083.pc_relevant_antiscanv3&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-123495391-blog-82878083.pc_relevant_antiscanv3&utm_relevant_index=1)
  - LogSoftmax 是 $log(softmax(\cdot))$
  - Log 操作把 $[0,1]$ 映射到 $[-\infin, 0]$
  - label 是标签索引
  - NLLLoss 就是把 LogSoftmax 后的输出与 Label 对应的那个值拿出来，再去掉负号，再求均值
  - 使用方法是直接将模型输出的 Logits 传入 CrossEntropyLoss

## 排序

```python
torch.sort() -> value, index  # default dim is -1
torch.max() -> value, index
```

```python
np.argmin()  # 返回索引
np.min()  # 返回值
```



## 嵌入层

嵌入层取出的时候为什么要加一层 relu?



## dropout

用于衡量不确定度的 dropout + linear

## 运行效率

### 并行与并发，多线程优化

- cpu_time = user_time + sys_time
- CPU time 一般比 wall time 小，当我们使用多线程的时候，程序的 CPU time 是各个线程的 CPU time 之和。
- cpu_time ~= wall_time: 计算密集型，未并行执行
- cpu_time < wall_time: I/O 密集型，多核并行执行优势并不明显 
- cpu_time > wall_time: 计算密集型，利用多核处理器的并行执行优势

- 优化方式
  - 计算型 -> 并行 = 多进程
  - I/O 型 -> 并发 = 多线程

## 科研

发论文的欲望，是遇到阻力是更大的压力，也是进展顺利时的鼓励。遇沟壑，莫要轻易否定自己，因为眼前的苦难会朦胧了我们的双眼。也要有大局观，放弃没有意义和价值的方向。

#### 数据驱动

- 闭环: 生成数据集 -> 跑 baseline -> badcase study -> 更新策略 -> 重新生成数据集



