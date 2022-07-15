# Grad-CAM

开山之作：利用 GAP 获取 CAM

更通用的做法：Grad-CAM: Visual Explanations from Deep Networks via Gradient-based Localization

CAM 只能用于最后一层特征图和输出之间是 GAP 的操作，grad-CAM 可适用非 GAP 连接的网络结构；
CAM只能提取最后一层特征图的热力图，而 gard-CAM 可以提取任意一层；

因为存在丢失精度的风险。所以目标类别 score 建议使用不经过 softmax 的值。

比 Grad-CAM 更进一步：Grad-CAM++ 

结合梯度平滑策略： Smooth Grad-CAM++

gradient-free 的做法： score-CAM 和 ss-CAM

利用 Ablation 分析的方法：Ablation-CAM



## References

- [万字长文：特征可视化技术(CAM)](https://zhuanlan.zhihu.com/p/269702192)