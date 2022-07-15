---
title: PyTorch 张量
---

## 创建张量

### 属性张量

Pytorch 中定义了 8 种张量类型

```python
torch.Tensor(2, 3, 4)
torch.tensor([2, 3])  # 只能输入对象

torch.FloatTensor(2, 3)  # 可以输入 size
torch.FloatTensor([2, 3])  # 也可以是对象, 将 [2, 3] 变成张量
torch.LongTensor(2, 3)

# 不常用
torch.DoubleTensor(2, 3)
torch.HalfTensor(2, 3)
torch.ByteTensor(2, 3)
torch.CharTensor(2, 3)
torch.ShortTensor(2, 3)
torch.IntTensor(2, 3)

```

### 类型张量

```python
torch.zeros(Size)  # size; -> float
# 上三角
torch.triu(对象, diagonal=1).type(torch.uint8)
torch.ones_like(x)  # -> 和 x 同 size 的全 1 张量
torch.zeros_like(x)  # -> 和 x 同 size 的全 0 张量
torch.where(x > 0.5, torch.ones_like(x), torch.zeros_like(x))  # if true select 1, else 0
```

## 张量操作

```python
torch.stack([x1, x2], dim=-1)  # 最后一维堆叠
torch.cat([x1, x2], dim=1)  # dim 不能超过 x 的维度
torch.rand(*size, )  # uniformly sampled between 0 and 1 
torch.randn  # normal distribution with mean 0 and variance 1
torch.randint(low=0, high=2, size=(self.size, 2), dtype=torch.float32)
torch.arange  # from n to m torch.Tensor (input list)
torch.empty(*size, )  # 未初始化的

x2.add_(x1)  # 原位
torch.matmul(x, W)

torch.bmm()  # b,n,m @ b,m,p -> b,n,p
torch.einsum()  # matrix multiplications
torch.clamp(torch.randn(4), min=-0.5, max=0.5)  # 限制值在 +-0.5 

x.view(2, 3)  # reshape
x.requires_grad_(True)  # 默认的矩阵不求导

y.backward()
x.grad

# Size is same as Shape
x.shape
x.size()

x.where()
```

矩阵拼接

```python
torch.stack((x, x))  # (3, 5) -> (2, 3, 5)
torch.hstack((x, x))  # (3, 5) -> (3, 10)
torch.vstack((x, x))  # (3, 5) -> (6, 5)
```

矩阵拆分

```python
import torch

x = torch.rand([500, 10])
z = torch.zeros([10])
# 比下面快 ~30%
# torch.unbind(x) -> Tuple[Tensor]
for x_i in torch.unbind(x):
    z += x_i
for i in range(500):
    z += x[i]
# 当然求和最快的是 sum，快 ~100%
z = torch.sum(x, dim=0)
```

广播

```python
a = torch.rand([5, 1, 6])
a.repeat([1, 3, 1])  # [5, 1, 6] -> [5, 3, 6]

a.expand(5, 3, 6)  # -> [5, 3, 6]
a.expand(-1, 3, -6)  # 同上
```

矩阵乘法

```python
torch.mm(x, y)
x @ y  # 同上
```

张量比较

```python
x == y  # x, y size 要相等，每个位置上 True or False
```

张量求导

```python
from torch.autograd import Variable

Variable(x)  # 已经归入 tensor
```

张量属性

```python
x = torch.Tensor
x.data  # data 用索引下表访问
x.detatch()  # 推荐使用
```

数据类型转换

```python
torch.from_numpy(np_arr1)
Tensor.numpy()
Tensor.tolist()
```

其他

```python
x.cpu()
x.zero_()  # 归 0 化
```

修改类型

```python
torch.tensor(1).type(torch.uint8)  # 创建的是 scale
torch.zeros(2, 3).as_type(Tensor)  # 根据 Tensor 类型创建
```

detach

PyTorch0.4 中，`.data` 仍保留，但建议使用 `.detach()`

区别在于 `.data` 返回和 `x` 的相同数据 tensor, 但不会加入到 `x` 的计算历史里，且 `require s_grad = False`, 这样有些时候是不安全的, 因为 `x.data` 不能被 `autograd`
追踪求微分 。 `.detach()` 返回相同数据的 `tensor` ,且 `requires_grad=False` ,但能通过 `in-place `操作报告给 `autograd` 在进行反向传播的时候.

x.data 和 x.detach() 都是从原有计算中分离出来的一个 tensor 变量 ，并且都是 inplace operation（浅拷贝）.在进行 autograd 追踪求倒时，两个的常量是相同。

不同：.data时属性，detach()是方法。 x.data不是安全的，x.detach()是安全的。

```python
x.detach()
x.data
```

#### torch.gather

输出维度是 `idx.size()`
-1 维度: 在最后一维，按索引选择 `src` 组成同 idx.size() 张量

```python
torch.gather(src, -1, idx)
```

#### torch.max and torch.sort

```python
value, idx = torch.max(torch.IntTensor(3, 2, 2), dim=1)  # 如果带 dim 参数，返回值和索引
value, idx = torch.max(torch.IntTensor(3, 2, 2), dim=1)  # 如果带 dim 参数，返回值和索引
```

pytorch masked_fill

mask 必须是一个 ByteTensor 而且 shape 的最大维度必须和 a 一样 并且元素只能是 0 或者 1 ，
是将 mask 中为 1 的元素所在的索引，在 a 中相同的的索引处替换为 value

```python
b = a.masked_fill(mask, value=torch.tensor(-1e9))
scores = scores.masked_fill(mask == 0, -1e9)  # 反向 mask
```

掩码维度计算方式，1 是个特殊的表示，该维度采取右边维度的掩码方式进行扩充。因此，mask 与 src 的维度要不匹配，要不 mask 的诶度选 1，然后利用扩充机制。

```python
# 全掩盖
mask = torch.ones(4, 5, 6)
score = torch.arange(120).view(4, 5, 6).masked_fill(mask == 1, -1e9)
score
```

| mask  | score | work |
| ----- | ----- | ---- |
| 4,5,6 | 4,5,6 | 1    |
| 4,5,5 | 4,5,6 | 0    |
| 4,5,1 | 4,5,6 | 1    |
| 4,1,6 | 4,5,6 | 1    |
| 4,1,1 | 4,5,6 | 1    |

#### 张量重载运算符号

不支持 `and`, `or`, `not`

```python
z = -x  # z = torch.neg(x)
z = x + y  # z = torch.add(x, y)
z = x - y
z = x * y  # z = torch.mul(x, y)
z = x / y  # z = torch.div(x, y)
z = x // y
z = x % y
z = x ** y  # z = torch.pow(x, y)
z = x @ y  # z = torch.matmul(x, y)
z = x > y
z = x >= y
z = x < y
z = x <= y
z = abs(x)  # z = torch.abs(x)
z = x & y
z = x | y
z = x ^ y  # z = torch.logical_xor(x, y)
z = ~x  # z = torch.logical_not(x)
z = x == y  # z = torch.eq(x, y)
z = x != y  # z = torch.ne(x, y)
```

#### 张量索引

```python
Tensor[:, :, None]  # unsqueeze(2)
```





