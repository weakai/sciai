# PyTroch 数据

## Tricks

1. 测试集合的 `shuffle` 和 `drop_last` 设置为 `False`

```python
test_data_loader = data.DataLoader(
    test_dataset,
    batch_size=128,
    shuffle=False,
    drop_last=False,
    num_workers=conf.workers,  # 加快载入速度
)
```

## 使用现成的数据

```python
from torch.utils.data import Dataset
from torchvision import datasets
from torchvision.transforms import ToTensor

training_data = datasets.FashionMNIST(
    root="data",
    train=True,
    download=True,
    transform=ToTensor()
)

test_data = datasets.FashionMNIST(
    root="data",
    train=False,
    download=True,
    transform=ToTensor()
)
```

## 自定义数据

创建一个 `Dataset` 对象，实现 `__getitem__()` 和 `__len__()` 这两个方法
`__getitem__()` 返回一条数据的内容和标签

```python
from torch.utils.data import DataLoader
# training_data 是 Dataset
train_dataloader: DataLoader = DataLoader(training_data, batch_size=64, shuffle=True)
```

### 多数据集的模型

用 Concatenated Dataset 实现

```python
from pytorch_lightning import LightningModule
from torch.utils.data import ConcatDataset

class LitModel(LightningModule):
    def train_dataloader(self):
            concat_dataset = ConcatDataset(datasets.ImageFolder(traindir_A), datasets.ImageFolder(traindir_B))

            loader = DataLoader(
                concat_dataset,
                batch_size=args.batch_size,
                shuffle=True,
                num_workers=args.workers,
                pin_memory=True  # 载入内存
            )
            return loader
    ...
```

### 使用数据加载器来训练模型

```python
for epoch in range(100):
    for i, data in enumerate(train_loader):
        inputs, labels = data
```

### 数据分割

### 方法一

```python
train_size = int(0.8 * len(full_dataset))
test_size = len(full_dataset) - train_size
train_dataset, test_dataset = torch.utils.data.random_split(full_dataset, [train_size, test_size])
```

### 方法二

```python
dataset = MyCustomDataset(my_path)
batch_size = 16
validation_split = .2
shuffle_dataset = True
random_seed= 42

# Creating data indices for training and validation splits:
dataset_size = len(dataset)
indices = list(range(dataset_size))
split = int(np.floor(validation_split * dataset_size))
if shuffle_dataset :
    np.random.seed(random_seed)
    np.random.shuffle(indices)
train_indices, val_indices = indices[split:], indices[:split]

# Creating PT data samplers and loaders:
train_sampler = SubsetRandomSampler(train_indices)
valid_sampler = SubsetRandomSampler(val_indices)

train_loader = torch.utils.data.DataLoader(dataset, batch_size=batch_size, 
                                           sampler=train_sampler)
validation_loader = torch.utils.data.DataLoader(dataset, batch_size=batch_size,
                                                sampler=valid_sampler)

# Usage Example:
num_epochs = 10
for epoch in range(num_epochs):
    # Train:   
    for batch_index, (faces, labels) in enumerate(train_loader):
        # ...
```

#### torch 带的数据处理函数

```text
class torch.utils.data.Dataset: 一个抽象类， 所有其他类的数据集类都应该是它的子类。而且其子类必须重载两个重要的函数：len(提供数据集的大小）、getitem(支持整数索引)。
class torch.utils.data.TensorDataset: 封装成 tensor 的数据集，每一个样本都通过索引张量来获得。
class torch.utils.data.ConcatDataset: 连接不同的数据集以构成更大的新数据集。
class torch.utils.data.Subset(dataset, indices): 获取指定一个索引序列对应的子数据集。
class torch.utils.data.DataLoader(dataset, batch_size=1, shuffle=False, sampler=None, batch_sampler=None, num_workers=0, collate_fn=<function default_collate>, pin_memory=False, drop_last=False, timeout=0, worker_init_fn=None): 数据加载器。组合了一个数据集和采样器，并提供关于数据的迭代器。
torch.utils.data.random_split(dataset, lengths): 按照给定的长度将数据集划分成没有重叠的新数据集组合。
class torch.utils.data.Sampler(data_source):所有采样的器的基类。每个采样器子类都需要提供 iter 方-法以方便迭代器进行索引 和一个 len方法 以方便返回迭代器的长度。
class torch.utils.data.SequentialSampler(data_source):顺序采样样本，始终按照同一个顺序。
class torch.utils.data.RandomSampler(data_source):无放回地随机采样样本元素。
class torch.utils.data.SubsetRandomSampler(indices)：无放回地按照给定的索引列表采样样本元素。
class torch.utils.data.WeightedRandomSampler(weights, num_samples, replacement=True): 按照给定的概率来采样样本。
class torch.utils.data.BatchSampler(sampler, batch_size, drop_last): 在一个batch中封装一个其他的采样器。
class torch.utils.data.distributed.DistributedSampler(dataset, num_replicas=None, rank=None):采样器可以约束数据加载进数据集的子集。
```

## 数据保存与加载

### numpy

```python
# 保存字典对象
np.save(path, experimental_dict)
np.load(pwd + 'experimental_dataset_dict.npy', allow_pickle = True)
```