---
title: rdkit
tags: [chemistry, rdkit, tool]
---

[官方文档](https://www.rdkit.org/docs/index.html)

## Portal

<http://www.rdkit.org/docs/GettingStartedInPython.html#>

### 读取

#### 单个读取

```py
from rdkit import Chem

m = Chem.MolFromSmiles('Cc1ccccc1')
m = Chem.MolFromMolFile('data/input.mol')

stringWithMolData = open('data/input.mol','r').read()
m = Chem.MolFromMolBlock(stringWithMolData)

# m, <rdkit.Chem.rdchem.Mol object at 0x...>
# 读取失败或者分子有错 返回 None
```

#### 批量读取

```py
# 推荐写法
with Chem.SDMolSupplier('data/5ht3ligs.sdf') as suppl:
    for mol in suppl:
        if mol is None: continue
        print(mol.GetNumAtoms())

# 另一种推荐，此方法不支持索引
inf = open('data/5ht3ligs.sdf','rb')
with Chem.ForwardSDMolSupplier(inf) as fsuppl:
    for mol in fsuppl:
    if mol is None: continue
    print(mol.GetNumAtoms())

# 解压缩写法，查看有多少个分子，此方法不支持索引
import gzip
inf = gzip.open('data/actives_5ht3.sdf.gz')
with Chem.ForwardSDMolSupplier(inf) as gzsuppl:
    ms = [x for x in gzsuppl if x is not None]
len(ms)

# 多线程读取
i = 0
with Chem.MultithreadedSDMolSupplier('data/5ht3ligs.sdf') as sdSupl:
    for mol in sdSupl:
        if mol is not None:
        i += 1
```


