# deepsmiles

```python
import deepsmiles
converter = deepsmiles.Converter(rings=True, branches=True)
smi = 'CC[N+](C)(C)Cc1ccccc1Br'
deepsmi = converter.encode(smi)
print(f'DeepSMILES string: {deepsmi}')> >> DeepSMILES string: CC[N+]C)C)Ccccccc6Br
toks = atomwise_tokenizer(deepsmi)
print(toks)

>>> ['C', 'C', '[N+]', 'C', ')', 'C', ')', 'C', 'c', 'c', 'c', 'c', 'c', 'c', '6', 'Br']

toks = kmer_tokenizer(deepsmi, ngram=4)
print(toks)

>>> ['CC[N+]C', 'C[N+]C)', '[N+]C)C', 'C)C)', ')C)C', 'C)Cc', ')Ccc', 'Cccc', 'cccc', 'cccc', 'cccc', 'ccc6', 'cc6Br']
```