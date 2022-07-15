---
title: apex
---

NVIDIA's Apex tool
Generally GPUs are good at doing 32-bit(single precision) math, not at 16-bit(half) nor 64-bit(double precision). 
Therefore traditionally deep learning model trainings are done in 32-bit.
By switching to 16-bit, we’ll be using half the memory and theoretically less computation at the expense of the available number range and precision.
However, pure 16-bit training creates a lot of problems for us (imprecise weight updates, gradient underflow and overflow). **Mixed precision training, with Apex, alleviates these problems**.

## 安装

```bash
cd /tmp
git clone https://github.com/NVIDIA/apex
cd ./apex
pip install -v --no-cache-dir /tmp/apex
```