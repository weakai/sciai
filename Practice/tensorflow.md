## Preface

深度学习框架

TensorFlow 2 不兼容 TensorFlow 1.x 的代码。Google 即将停止更新 TensorFlow 1.x，不建议学习TensorFlow 1.x 版本。

TensorFlow 2 支持动态图优先模式，在计算时可以同时获得计算图与数值结果，可以代码中调试并实时打印数据，搭建网络也像搭积木一样，层层堆叠，非常符合软件开发思维。

## Portal

[龙书 Github 开源地址](https://github.com/dragen1860/Deep-Learning-with-TensorFlow-book)

> 此书不能深入看，不建议新手直接看。

## tensorflow的三种保存格式总结

- [tensorflow的三种保存格式总结-1(.ckpt) - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/60064947)

### 1.1文件结构介绍：

---checkpoint

---model.ckpt-240000.data-00000-of-00001

---model.ckpt-240000.index

---model.ckpt-240000.meta
Tensorflow模型主要包括两个方面内容：
1. 神经网络的结构图graph
2. 已训练好的变量参数。

因此Tensorflow模型主要包含两个文件：
1. 元数据图（meta graph）：
它保存了tensorflow完整的网络图结构。这个文件以 `*.meta`为拓展名；

2. 检查点文件（checkpoint file）
这是一个二进制文件，它包含权重变量，biases变量和其他变量。这个文件以 `*.ckpt` 为拓展名； PS：从 `0.11` 版本之后就不是单单一个 `.ckpt` 文件，除此之外还有一个 `.index` 文件。其中 `.data` 文件是包含训练变量的文件； `.index` 是描述 `variable` 中 `key` 和 `value` 的对应关系；`checkpoint` 文件是列出保存的所有模型以及最近模型的相关信息。因此，如果tensorflow版本高于0.10的话。



