# xAMP

2018

AMPs that kill Gram-positive and/or Gram-negative bacteria may kill fungal and viral pathogens

## Ideas

this paper is the first to propose DNN models for AMP classification

smaller alphabet allows retaining good AMP recognition ACC:

- by reducing the number of amino acids from 20 to 8 pseudo-amino acids (plus a padding character), we shrink the size of the sequence space wet-laboratory researchers have to consider when designing peptide libraries.

## Data

APD vr.3 database

- active against Gram-positive and/or Gram-negative bacteria
- filter: 1778 AMPs -> 712/354/712
  - len > 10
  - CD-HIT: identity < 90%

- negative dataset:
  1. previous work
  2. [UniProt](http://www.uniprot.org) 
     - remove entities related to `microbe`
     - fileter like job above
       - CD-HIT: identity < 90%
       - BLAT vr.35: not found to match known AMPs
         - protein-versus-protein search

### Data Analysis

- sequence length distributions
- evaluate the impact of the size and composition of the dataset on model performance

## Representation

0. Input
   - X is 0 and 20 amino acid is 1-20
   - padding peptides to 200 in length

1. Embedding
   1. 128D

2. Visualization
   1. 2D t-SNE with K-means@sk-learn: 128D -> 2D
   2. Followed by similarity analysis

3. ?Reduced alphabet construction and testing
   - K-means: padding 'X' was clustered to 9
   - Random seed varies to get a stable result

## Models

CNN and RNN: Convolutional LSTM

Keras and tensorflow

Model **tuning** and construction:

- 'training' model and 'production' model
- randomized search using the Hyperas wrapper package
- 10fold CV
- probability threshold is 0.5

## Knowledge

AMP Scanner server: Scanner is a classifier

## Links

- [AMP Scanner Server](https://www.dveltri.com/ascan/v2/ascan.html)
  - support high-throughput screening experiments
- [Zotero](zotero://select/library/items/Z86BEIHD)