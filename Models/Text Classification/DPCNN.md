# DPCNN

如今深度学习已经成为 NLP 领域的标配技术，在图像中大为成功的卷积神经网络（CNN）也开始广泛渗透到文本分类、机器翻译、机器阅读等 NLP 任务中。
但是，在 ACL2017 以前，word-level 的文本分类模型（以单词为语义单位）自 2014 Kim 等人提出的 TextCNN 模型后，就没有再出现过显著有效的 CNN 系模型，尤其是深层模型。

而在 **2017** 年的 ACL 上有这么一篇文章，Deep Pyramid Convolutional Neural Networks for Text Categorization，
论文中提出的深层金字塔卷积网（DPCNN）是严格意义上第一个 word-level 的广泛有效的深层文本分类卷积神经网

![](https://pic3.zhimg.com/80/v2-1a791b349b18ebd072d57a3956afe106_720w.jpg)

当然，这篇论文里还使用了 two-view embedding 来进一步提升模型性能，
不过从模型性能上纵向比较来看，其也比经典的 TextCNN（表格第二行 ShallowCNN）有了明显提高，
在 Yelp 五分类情感分类任务中提升了近 2 个百分点。也是第一次真正意义上证明了在 word-level 的文本分类问题上，深层 CNN 依然是有想象空间的。

DPCNN的底层貌似保持了跟TextCNN一样的结构
让网络学习长距离复杂模式: Block + ResNet
Block = 等长卷积+等长卷积+1/2池化 



## 参考

- [从经典文本分类模型 TextCNN 到深度模型 DPCNN](https://zhuanlan.zhihu.com/p/35457093)