# AntiCP2

## Idea

predict ACPs

Benchmark: SVM

## Materials and methods

main datasets

- previous studies that include ACP-DL, ACPP, ACPred-FL, AntiCP and iACP
- extract ACP database CancerPPD
- removing small, long, identical and non-natural peptides
- 970 unique ACPs having 4 or more residues and 50 or fewer residues

- experimentally validated ACPs are taken as positive class and AMPs as nonACPs or negative class.

alternate datasets

- ...

### Feature Generation

1. ACC(Amino acidc composition)
2. DPC(Dipeptide composition)
3. Terminus composition
   - 5, 10 and 15 residues at N- and C-terminus of the protein/peptides
   - also considers N5C5, N10C10 and N15C15
   - also compute its composition
4. ?Binary profile? is this onehot
   - backward: no fixed-length@extract from terminus
5. Hybrid Feature

## Analysis

### Percent Analysis

Bar plot shows the percent AAC of anticancer.

### Positional preference of residues

sample logos is `词频率图`

Two sample logos generated from (A) N-terminus (first 10 residues) and (B) C-terminus (last 10 residues) of peptides.

### Motif analysis

analysis with MERCI software

- `LAKLA, AKLAK, FAKL and LAKL` exclusively in ACPs
- `GLW, CKIK, DLV and AGKG` exclusively in non-ACPs

## Models

- SVC
- KNN
- RF
- MLP
- ETree
- Ridge Classifier

## Links

- [GitHub](https://github.com/raghavagps/anticp2/)
- [Zotero](zotero://select/library/items/GGANGVVP)
- [Web Server](https://webs.iiitd.edu.in/raghava/anticp2)
- [docker server](https://webs.iiitd.edu.in/gpsrdocker/)
- [Data Download](https://webs.iiitd.edu.in/raghava/anticp2/download.php)
