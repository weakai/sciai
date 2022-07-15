# QAs

## 2022-7-15

### NLP 与生物序列的区别有哪些?

- 社会学上讲
  - 资源倾斜程度不同，NLP 同 CV 一样是具有实际应用价值的，所以商业意义大
  - AI 先在 NLP, CV 领域取得突破，然后辐射到科学领域，需要一波波开拓者

- 科学
  - 生物序列语义上更丰富，单词表达更精炼，是能量的低点

- 历史
  - 生物的语言是自然选择的产物，而语言是人类生活（劳动）的产物

## 2022-6-21

1. 注意损失函数带不带 softmax。 withlogits。
2. 严格按照番茄时间作息，舒缓的心才能带来良好的生产力，好事多磨。
3. 使用有背书的代码，并给代码写注释。

## 2022-5-20

> ? 如何描述你遇到的问题
> 何时何地和物发生了什么咋么发生的导致了什么结果

## 2022-5-17

- 没有能力的人才会追求公平，企图获得环境的保护。
- 不要埋怨公平与否，也不要幻想环境的改变。因为这个世界就是不公平的。
- 如果你的想法与现实相违背，那么大概率是你错了。
- 统治者都是伪善的。

? float(tensor) and tensor.float()

? 模型输出的索引
? bool type

```python
out = model()
out[batch.train_mask]
```

? GINConv

? F.elu

## 2022-4-26

Q: 为什么众多 Python 优秀开源项目的作者都是外国人
A:
  1. 有钱有闲有追求?

为什么科技是第一生产力，为什么那么多企业还是做着苦力活，而不是追求技术的进步?
追求进步的永远是那么少部分人。
因为这个世界就是金字塔型分布（二八定律）。
所以科技是第一生产力是悖论。应该加上限制。
生产力的发展依赖科技的进步。
科技的进步不是依赖不断地重复，而是创新。
商人的本质是逐利，尤其是第境界的商人，其目光长远不了。

## 2022-4-18

?

## 2022-4-17

? 现在的人工智能界的研究大方向是什么
- 如何判断前沿方向
  - 可以看大牛@Bengio的文章，研究的主要内容是什么
  - 多个模型之间的协作，组成一个模型的集群，然后实现超模型功能@Strategy。
    - 单个模型的改造能力有限，可以让不同模型实现一个系统的某一部分功能，然后加强他们之间的协作
      - 主动学习
      - 联邦学习
      - 强化学习
- 如何判断主流方向
- 综述文章，重点提出的技术，以及论文中被用到的各种技术

? 关于 AMOP 的可提升点
1. 代码杂乱？是否可以引入规范模板代码
2. 双模型的组合？能否向主动学习靠近
3. 重要性评估?能否用 SHAP 代替 Attention
4. 模型容量?如何鉴别你的模型是否大材小用

---

? What is the relationships between SHAP and Attention Visualization
? How to adopt it into Bio task

---

? Bayes and life-long learning
? How can model update its jude patterns

---

? Why transfer learning is useful
? How are main streaming of transfer learning technology
? What framework that I can adopt for my task

---

? How to design a general fingerprint for common usage
? How to design a learnable fingerprint for a specific task
? What is GNN's read-out

## 2022-4-16

? Is there exists a fingerprint for everything

## 2022-4-15

? Is NLP good for Drug discovery

- If good enough? Why Tangjian resort to Graph Representation

---

? torch.optim.lr_scheduler._LRScheduler

---

? The ecosystem of Hugging Face and Pytorch Lightning is overlaped, which one is better for me? 

> 1. To Combine them together pytorchlighting transformers

---

? How to judge a model that is over-fit

> the learning curve or loss curve to achieve a maximum

---

? The pros and cons of three types of transformer-like models: 

- Encoder
- Decoder
- Both of Encoder and Decoder

---

> Sparse
> Norm 0 is low means the matrix is sparse
> 两者搭配使用

```python
loss=tf.keras.losses.SparseCategoricalCrossentropy(from_logits=True)
metrics=tf.metrics.SparseCategoricalAccuracy()
```
