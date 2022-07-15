# DeepHlApan

ZJU, 2019

## Ideas

Considering Both HLA-Peptide Binding and Immunogenicity

## Data

Benchmark dataset: IEDB(Immune Epitope Database)

independent mass spectrometry dataset(previous studies)

Neoantigen Dataset Used for Evaluating Immunogenicity Model(previous studies)

- 64 neoantigens with 6,400 random peptides
- generated based on mutation data extracted from The Cancer Genome Atlas (TCGA) database.

CD8+ T-Cell Epitopes

- 2,023 assayed single-nucleotide variants from 17 patients, including 26 mutations with pre-existing T-cell responses from Bulik-Sullivan et al.
- 

## Models

RNN-based: GRU with attention

model parallel:

- binding model: predicting the probability of the peptide being presented to the tumor cell membrane by HLA
- immunogenicity model: predicting the potential of pHLA eliciting T-cell activation

more binding data -> immunogenicity model as a filter and rank the prediction scores of the binding model for high-confidence neoantigen identification.

### binding model

one-hot

the HLA alleles were transformed into pseudo-sequences as presented by the NetMHCpan

padding X to 49 in length

Loss Function: BCE

Dropout

### Comparison

12 models on IEDB

## Knowledge

Cancer cells are different, through somatic mutations, from normal cells and can therefore be recognized being a foreign cell by the immune system.
Among all foreign elements in cancer cells, neoantigens are the most widely recognized elements derived from mutated genes.
Neoantigens have therefore been acknowledged as ideal targets for cancer immunotherapies, such as cancer vaccines and T-cell immunotherapies.
Recent studies also indicated that neoantigens are closely related to the therapeutic effect of immune checkpoint blockade therapies.
However, only a small number of somatic mutations can generate neoantigens.
It is still challenging to identify somatic mutations that can generate effective neoantigens.

whole exome sequencing combined with bioinformatic prediction has been widely used for candidate neoantigen identification

- TSNAD
- pVAC-Seq
- INTEGRATE-neo

### Previous prediction methods

three groups

1. PSSM-based
2. ML-based
3. structure-based
4. **consensus** methods

### Related dataset

TSNAdb

TCIA

### Related Researches

epitope prediction based on mass spectrometry (MS) profiling of HLA-peptide sequences.

NetMHCpan

- trained based on both binding affinities and MS data of HLA-peptide binding

## Links

- [GitHub](https://github.com/jiujiezz/deephlapan)
- [Web server](http://biopharm.zju.edu.cn/deephlapan/)
- [WebLogo](https://weblogo.berkeley.edu/logo.cgi)
- [Zotero](zotero://select/library/items/WNGPMWNN)