# MolVS

MolVS is a molecule validation and standardization tool, written in Python using the RDKit chemistry framework.

```bash
conda install molvs
pip install MolVS
```

```python
from molvs import standardize_smiles
standardize_smiles('[Na]OC(=O)c1ccc(C[S+2]([O-])([O-]))cc1')
'[Na+].O=C([O-])c1ccc(CS(=O)=O)cc1'
```
