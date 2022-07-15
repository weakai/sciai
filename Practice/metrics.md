# Metrics

## Top-k 实现代码

- [推荐阅读 | 月来客栈 | Top-K 准确率介绍与实现](#links)

### Numpy 实现

```python
def get_top_k_result(logits, k=3, sorted=True):
    indices = np.argsort(logits, axis=-1)[:, -k:]  # 取概率最大的前K个所对应的预测标签
    if sorted:  # np.argsort 默认返回的顺序是从小到大，sorted=True 可以返回从大到小
        tmp = []
        for item in indices:
            tmp.append(item[::-1])
        indices = np.array(tmp)
    values = []
    for idx, item in zip(indices, logits):  # 取所有预测值所对应的概率值
        p = item.reshape(1, -1)[:, idx].reshape(-1)
        values.append(p)
    values = np.array(values)
    return values, indices

logits = np.array([[0.1, 0.3, 0.2, 0.4],
                   [0.5, 0.01, 0.9, 0.4]])
y = np.array([3, 0])
print(get_top_k_result(logits))

>>> (array([[0.4, 0.3, 0.2],
       [0.9, 0.5, 0.4]]), array([[3, 1, 2],
       [2, 0, 3]], dtype=int64))

def calculate_top_k_accuracy(logits, targets, k=2):
    values, indices = get_top_k_result(logits, k=k, sorted=False)
    y = np.reshape(targets, [-1, 1])
    correct = (y == indices) * 1.  # 对比预测的K个值中是否包含有正确标签中的结果
    top_k_accuracy = np.mean(correct) * k  # 计算最后的准确率
    return top_k_accuracy

print(calculate_top_k_accuracy(logits, y, k=2)) # 1.0
print(calculate_top_k_accuracy(logits, y, k=1)) # 0.5
```

### Tensorflow

```python
import tensorflow as tf

logits = tf.constant([[0.1, 0.3, 0.2, 0.4],
                      [0.5, 0.01, 0.9, 0.4]], shape=[2, 4], dtype=tf.float32)
y = tf.constant([3, 0], tf.int32)

def calculate_top_k_accuracy(logits, targets, k=2):
    values, indices = tf.math.top_k(logits, k=k, sorted=True)
    y = tf.reshape(targets, [-1, 1])
    correct = tf.cast(tf.equal(y, indices), tf.float32)
    top_k_accuracy = tf.reduce_mean(correct) * k
    return top_k_accuracy

sess = tf.Session()
print(sess.run(calculate_top_k_accuracy(logits, y, k=2)))# 1.0
print(sess.run(calculate_top_k_accuracy(logits, y, k=1)))# 0.5
```

### Pytorch 中的实现

```python

import torch
logits = torch.tensor([[0.1, 0.3, 0.2, 0.4],
                       [0.5, 0.01, 0.9, 0.4]])
y = torch.tensor([3, 0])
def calculate_top_k_accuracy(logits, targets, k=2):
    values, indices = torch.topk(logits, k=k, sorted=True)
    y = torch.reshape(targets, [-1, 1])
    correct = (y == indices) * 1.  # 对比预测的K个值中是否包含有正确标签中的结果
    top_k_accuracy = torch.mean(correct) * k  # 计算最后的准确率
    return top_k_accuracy

print(calculate_top_k_accuracy(logits, y, k=2).item())# 1.0
print(calculate_top_k_accuracy(logits, y, k=1).item())# 0.5
```

## Links

- [Top-K 准确率介绍与实现](https://mp.weixin.qq.com/s?__biz=MzAwNjU0NjA3Ng==&mid=2247488387&idx=1&sn=d5a015988cfc037cf7101301fc725e25&chksm=9b0ae470ac7d6d66bcf27f36feaca4018890c9501bc958eaf81e8d692c15a6778ea281b98fd8&mpshare=1&scene=1&srcid=0412fwvkqRtqykjmfLVFDsy4&sharer_sharetime=1649721590505&sharer_shareid=f66ef27ade3d509229b2afd8611df712#rd)