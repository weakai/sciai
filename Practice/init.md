---
title: 模型初始化
---

```python
for p in model.parameters():
        if p.dim() > 1:
            nn.init.xavier_uniform_(p)
```

```py
self.rnn = nn.LSTM(input_size=embedding_size, hidden_size=128, num_layers=1, bidirectional=False)
for name, param in self.rnn.named_parameters():
	if name.startswith("weight"):
		nn.init.xavier_normal_(param)
	else:
		nn.init.zeros_(param)
```

```python
def weights_init(m): 
    if isinstance(m, nn.Conv2d): 
        nn.init.xavier_normal_(m.weight.data) 
        nn.init.xavier_normal_(m.bias.data)
    elif isinstance(m, nn.BatchNorm2d):
        nn.init.constant_(m.weight,1)
        nn.init.constant_(m.bias, 0)
 
model.apply(weights_init)
```

