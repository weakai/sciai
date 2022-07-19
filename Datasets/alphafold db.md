---
date: 2021-12-26
---

[AlphaFold DB的蛋白质数据库](https://alphafold.ebi.ac.uk)

- 可公开访问的高精度蛋白质结构预测数据库
- 数据库建设者：DeepMind 与 EMBL-欧洲生物信息学研究所 (EMBL-EBI)
- 基于 AlphaFold 2 ，覆盖更多蛋白质序列空间
- 提供可编程访问以及交互式可视化功能
  - 包括预测的原子坐标、每个残基和成对模型置信度的估计，以及预测的对齐误差

## 数据

AlphaFold DB 存档并提供对 PDB 和 mmCIF 格式的原子坐标、JSON格式的 PAE和JSON格式的相应元数据的访问。

- 的数据文件版本化，网页将始终显示最新版本，低版本数据快照将通过FTP提供。

数据访问

- 通过FTP批量下载
- API
- 可视化交互

## 背景

通用的蛋白质资源 (UniProt) 于蛋白质结构数据库 (PDB) 数据量的不对等

- 存储了近 2.2 亿个独特的蛋白质序列
- 蛋白质结构数据库 (PDB) 仅包含超过55000种不同蛋白质的180000多个3D结构

## 参考

[Nucleic Acids Res. | AlphaFold DB：大规模扩展蛋白质序列空间的结构覆盖范围](https://mp.weixin.qq.com/s?__biz=MzU2ODU3Mzc4Nw==&mid=2247495497&idx=1&sn=c93a5f84f82c0dc07836c25d8e1767f3&chksm=fc89457dcbfecc6ba4a1122535a4caa920fa4a85d44497e0cbb849ef921f9cd67357b4692eb2&mpshare=1&scene=1&srcid=1126GaWDgsjYKF9RVIZTOSFD&sharer_sharetime=1637895437500&sharer_shareid=3b6f19f7d8d23c4abdce81bf2c4bf7d3#rd)