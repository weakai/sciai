# 可视化

## tensorboard

Tensorboard 原本是 TensorFlow 的可视化工具，而目前在TensorboardX 的加持下，其他深度学习计算框架也可使用TensorBoard 工具进行可视化操作了。

```shell
pip install tensorboardX
```

```python
from torch.utils.tensorboard import SummaryWriter
```

- [PyTorch建模可视化工具TensorBoard的安装与使用](https://mp.weixin.qq.com/s?__biz=MzA4ODc5NzE1OA==&mid=2653606281&idx=1&sn=4206de98376ef789cb23705a158f03fd&chksm=8bfacb00bc8d42160c144f8d9b988e824164ca647266a0dcc20a9cb50b8a9e736ac002f7bc95&mpshare=1&scene=1&srcid=0329WgipzVyNHy5XYTzr9DD7&sharer_sharetime=1648522373488&sharer_shareid=f66ef27ade3d509229b2afd8611df712#rd)

## Others

```python
from matplotlib.colors import to_rgba
import matplotlib.plot as plt

@torch.no_grad()   # Decorator, same effect as "with torch.no_grad(): ..." over the whole function.
def visualize_classification(model, data, label):
    # Before plotting, fetch data into numpy from gpu onto cpu
    if isinstance(data, torch.Tensor):
        data = data.cpu().numpy()
    if isinstance(label, torch.Tensor):
        label = label.cpu().numpy()
    data_0 = data[label == 0]
    data_1 = data[label == 1]

    plt.figure(figsize=(4, 4))
    plt.scatter(data_0[:, 0], data_0[:, 1], edgecolor="#333", label="Class 0")
    plt.scatter(data_1[:, 0], data_1[:, 1], edgecolor="#333", label="Class 1")
    plt.title("Dataset samples")
    plt.ylabel(r"$x_2$")
    plt.xlabel(r"$x_1$")
    plt.legend()

    model.to(device)
    c0 = torch.Tensor(to_rgba("C0")).to(device)
    c1 = torch.Tensor(to_rgba("C1")).to(device)
    x1 = torch.arange(-0.5, 1.5, step=0.01, device=device)
    x2 = torch.arange(-0.5, 1.5, step=0.01, device=device)
    xx1, xx2 = torch.meshgrid(x1, x2)  # Meshgrid function as in numpy
    model_inputs = torch.stack([xx1, xx2], dim=-1)
    preds = model(model_inputs)
    preds = torch.sigmoid(preds)
    # Specifying "None" in a dimension creates a new one
    output_image = (1 - preds) * c0[None, None] + preds * c1[None, None]
    output_image = (
        output_image.cpu().numpy()
    )  # Convert to numpy array. This only works for tensors on CPU, hence first push to CPU
    plt.imshow(output_image, origin="lower", extent=(-0.5, 1.5, -0.5, 1.5))
    plt.grid(False)

```