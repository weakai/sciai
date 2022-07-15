# molcloud

https://github.com/whitead/molcloud

安装 [pygraphviz](https://pygraphviz.github.io/documentation/stable/install.html)

molcloud

```bash
pip install molcloud
pip install molcloud[all]  # RNA
molcloud [smiles-file]
molcloud [smiles-file] --output-file [output-file] --width 10 --node-size 25
rnacloud [fasta-file]
```

```text
O=C(OC)C=1C=CC2=NC=C(C(=O)OCC)C(NCC(O)C)=C2C1
O=C1C2=NC=CC3=C(OC)C=4OCOC4C(C=5C=C(OC)C(OC)=C(OC)C15)=C23
```

```text
>seq_0
UUCCAGCACCUGAUGUUCGAAUUUAAAUCGGCUCAACGAG
(((.((((.....)))).)))......(((......))).
```

