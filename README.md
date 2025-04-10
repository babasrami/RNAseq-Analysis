


```markdown
# Adrenal Gland Development Single-Cell Analysis



## Overview
This repository contains the analysis pipeline investigating developmental transitions in human adrenal glands using single-cell RNA sequencing data. The goal is to identify cell states and transcriptional programs associated with neuroblastoma origins, focusing on:
- Adrenal medulla development
- Chromaffin cell differentiation
- Schwann cell precursor (SCP) transitions
- Sympathoblast lineage commitment

Analysis follows workflows from [Furlan et al. (Nature Genetics 2021)](https://doi.org/10.1038/s41588-021-00818-x).

## Data Source
- **Accession**: [GSE147821](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE147821)
- **Samples**: 13 developmental timepoints (6-14 weeks) including replicates
- **Tissues**: Adrenal glands and paraganglia
- **Platform**: 10x Genomics Chromium

## Requirements
```bash
Python 3.8+
scanpy==1.9.1
scvelo==0.2.4
anndata==0.8.0
leidenalg==0.8.10
python-igraph==0.9.11
scikit-learn==1.0.2
matplotlib==3.5.1
seaborn==0.11.2
```

## Installation
```bash
# Clone repository
git clone https://github.com/yourname/adrenal-development.git
cd adrenal-development

# Create conda environment
conda create -n adrenal-analysis python=3.8
conda activate adrenal-analysis

# Install dependencies
pip install -r requirements.txt
```

## Workflow

### 1. Data Processing
```bash
# Download and process raw data
python pipeline.py --step 1
```
- Merges 13 samples (3.2 million cells total)
- QC filtering (mtDNA <5%, genes/cell 200-2500)
- Normalization with log1p transformation
- Cell cycle scoring using S/G2M phase markers

### 2. Dimensionality Reduction
```bash
python pipeline.py --step 2
```
- PCA (n_comps=50)
- UMAP visualization
- Leiden clustering (resolution=0.5)

### 3. Cell Type Annotation
Automated annotation using marker genes:
- **SCP**: SOX10, PLP1, MPZ
- **Chromaffin**: TH, PNMT, DBH
- **Sympathoblasts**: PHOX2B, ASCL1, HAND2

![UMAP](figures/umap_clusters.png)  
*14 distinct cell populations identified across developmental stages*

### 4. Re-clustering Analysis
```bash
python pipeline.py --step 3
```
- Sub-selects adrenal medulla clusters (SCPs, Chromaffin, Sympathoblasts)
- High-resolution re-clustering for transition analysis
- Improved trajectory resolution using diffusion maps

### 5. Trajectory Analysis
```bash
python pipeline.py --step 4
```
Performs pseudotime analysis for:
1. SCP → Chromaffin transition
2. SCP → Sympathoblast transition
3. Chromaffin ↔ Sympathoblast bifurcation

Uses diffusion pseudotime (DPT) with:
- Root cells identified by ISL1+ SCPs
- Gene trend visualization across 20 pseudotime bins

![Trajectory](figures/trajectory_heatmap.png)  
*Gene expression dynamics during SCP to chromaffin differentiation*

## Key Results
- Identified 14 cell states across developmental stages
- Characterized transcriptional programs governing neural crest differentiation
- Revealed bifurcation points in sympathoadrenal lineage commitment

## Directory Structure
```
├── data/               # Processed AnnData objects
├── figures/            # Visualization outputs
├── scripts/            # Analysis modules
├── pipeline.py         # Main workflow script
└── requirements.txt    # Dependency list
```

## License
[MIT License](LICENSE)  
Copyright (c) 2025 Rami BABAS


