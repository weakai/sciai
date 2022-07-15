# selfies

## 为什么 selfies 比 smiles 更适合生成

The SELFIES grammar and bond constraints enforce chemical valency rules, guaranteeing that generated SELFIES are syntactically and semantically valid, without requiring post-hoc corrections or complex model architectures that are difficult to train.
## 使用

```bash
pip install selfies
# check if the correct version
pip show selfies
# if not
pip install selfies --upgrade
```

```python
selfies.encoder
selfies.decoder
selfies.set_semantic_constraints
selfies.len_selfies
selfies.split_selfies
selfies.get_alphabet_from_selfie
selfies.selfies_to_encoding
selfies.encoding_to_selfies
```

## 使用

```python
import selfies
smi = 'CC[N+](C)(C)Cc1ccccc1Br'
sel = selfies.encoder(smi)
print(f'SELFIES string: {sel}')
>>> SELFIES string: [C][C][N+][Branch1_2][epsilon][C][Branch1_3][epsilon][C][C][c][c][c][c][c][c][Ring1][Branch1_1][Br]    
toks = atomwise_tokenizer(sel)
print(toks)
>>> ['[C]', '[C]', '[N+]', '[Branch1_2]', '[epsilon]', '[C]', '[Branch1_3]', '[epsilon]', '[C]', '[C]', '[c]', '[c]', '[c]', '[c]', '[c]', '[c]', '[Ring1]', '[Branch1_1]', '[Br]']

toks = kmer_tokenizer(sel, ngram=4)
print(toks)

>>> ['[C][C][N+][Branch1_2]', '[C][N+][Branch1_2][epsilon]', '[N+][Branch1_2][epsilon][C]', '[Branch1_2][epsilon][C][Branch1_3]', '[epsilon][C][Branch1_3][epsilon]', '[C][Branch1_3][epsilon][C]', '[Branch1_3][epsilon][C][C]', '[epsilon][C][C][c]', '[C][C][c][c]', '[C][c][c][c]', '[c][c][c][c]', '[c][c][c][c]', '[c][c][c][c]', '[c][c][c][Ring1]', '[c][c][Ring1][Branch1_1]', '[c][Ring1][Branch1_1][Br]']
```

## See also

- [GitHub repo](https://github.com/aspuru-guzik-group/selfies)