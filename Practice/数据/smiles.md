# SMILES

[SmilesPE](https://pypi.org/project/SmilesPE/)

```shell
pip install SmilesPE
```

```python
from SmilesPE.pretokenizer import atomwise_tokenizer

smi = 'CC[N+](C)(C)Cc1ccccc1Br'
toks = atomwise_tokenizer(smi)
print(toks)
```

2. K-mer Tokenzier

```python
from SmilesPE.pretokenizer import kmer_tokenizer

smi = 'CC[N+](C)(C)Cc1ccccc1Br'
toks = kmer_tokenizer(smi, ngram=4)
print(toks)
```
