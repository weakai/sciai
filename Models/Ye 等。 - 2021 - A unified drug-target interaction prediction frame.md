## Preface

Applications:

- virtual screening
- drug repurposing
- identification of potential drug side effects 

shortcomings :

- high sparsity of DTI datasets
- the cold start problem  

pipeline

- learns a low-dimensional representation for various entities in the KG
- integrates the multimodal information via neural factorization machine (NFM)  

three realistic scenarios  

- cold start for proteins  

results

- a **framework**
- while encountering new proteins , it handles = high extendibility for DTI prediction  

silico DTI prediction:

1. structure-based approaches : depends on three-dimensional (3D) structures of target proteins   
2. ligand-based approaches: depends on bioactivity data  
3. hybrid approaches : more promising 
    1. proteochemometrics (PCM)
    2. network-based methods  

## PCM

PCM covers a range of computational approaches developed based on the information of drugs and proteins represented by feature vectors and usually formulate DTI prediction to binary classification

introduced ML, based on molecular fingerprints and protein descriptors derived from protein sequences  

- SVM 
- RF  

introduced end-to-end methods based on deep learning (DL)
**large-scale**

- DeepDTI
- GraphDTA

## network-based methods  

incorporating multiple data sources :

- drug–target interactions
- drug–drug interactions
- protein–protein interactions
- omics data = heterogeneous data 
    - side-effects
    - drug-disease associations
    - genomics data  

nodes: drugs or proteins
edges: the **indicators** for the interactions or **similarities** between the connected nodes 

Papers:

- DTINet：applied an unsupervised method to learn low-dimensional feature representations of drugs and target proteins from heterogenous data and predicted DTI using inductive matrix completion 

- NeoDTI： integrate diverse information from heterogeneous network data and automatically learn topologypreserving representations of drugs and targets  

- Thafar et al. combined graph embedding and similarity-based techniques for DTI prediction  

## KG

ML models can built upon knowledge graph (KG)  

How: extract the fine-grained multi-modal knowledge elements from omics data and formulate the problem as the link prediction in KG  

Papers: 

- Mohamed et al. proposed a specific knowledge graph embedding (KGE) model, **TriModel**, to learn the vector representations for all drugs and proteins and then, consequently, infer new DTI based on the scores computed by the model 
- Zhu et al.  a comprehensive review of existing KG-based methods .

## recommendation systems  

popular and widely applied in various fields 

- e-commerce : consists of users and objects , in the form of web-based software 
- users can be modeled as drugs while the items can be modeled as targets.

 mainstream methods:

- collaborative filtering: has already been integrated with the network-based methods such as dual regularized one-class collaborative filtering  

ticks:

- matrix decomposition : 
    - extracting functional information from heterogeneous data
    - reducing the noise in heterogeneous networks 

shortcomings :

- highly similarity-dependent -> activity cliff: small structural changes can cause large differences in activity  
- no universal definition of similarity for all kinds of omics data collected from various sources  
    - KEGG Pathway
    - protein domain
    - protein binding site 
- time-consuming to calculate the pairwise similarities for large-scaled data sets
- walk off real-world scenarios: severe limiting 

## multi-omics resources  in a unified format

Due to the inevitable noises in the biomedicine data and existing problems stated above

several works such as PharmKG, BioKG, and Hetionet have provided compilations of curated relational data in a unified format, which enables the utilization of multi-omics resources 



The approaches of utilizing knowledge graph

1. end-to-end methods based on a comprehensive KG (e.g., DistMult) or a specifically crafted KG focusing on particular downstream tasks (e.g., the work of Zheng et al .designed for drug repurposing and target identification )
2. integration of a pre-trained KGE model and a prediction model toward a specific downstream task 

is there be a framework 

## KGE_NFM =  KG + recommendation system
a unified framework for DTI prediction，powerful and robust  

a pre-trained model based on knowledge graph and is integrated with a recommendation system tailored for a specific downstream task

captures the latent information from heterogeneous networks using KGE without any similarity matrix and then applies neural factorization machine (NFM) based on recommendation system to enforce the feature representation for a specific downstream task, which is the DTI prediction in this work  

## terminology

DTI=drug-target interactions 