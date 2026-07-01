# Single-Cell RNA-seq Analysis with Scanpy

## Overview
This project implements a complete, standard single-cell RNA-seq (scRNA-seq) analysis pipeline using Scanpy, the most widely used Python toolkit in the single-cell field. The pipeline covers everything from raw 10x Genomics data through quality control, doublet detection, normalization, dimensionality reduction, and unsupervised clustering.

## Biological Question
Can we identify distinct cell populations within peripheral blood mononuclear cell (PBMC) samples using only their single-cell gene expression profiles, while properly accounting for technical artifacts like low-quality cells and doublets?

## Data
- **Two 10x Genomics PBMC samples** (`s1d1`, `s1d3`), provided via the official Scanpy tutorial dataset
- Hosted on Figshare and accessed programmatically via `pooch` (no manual download needed)
- Original tutorial reference: https://scanpy.readthedocs.io/en/1.11.x/tutorials/basics/clustering.html

## Methods & Tools

**Language:** Python 3.10+

| Library | Purpose |
|---|---|
| `scanpy` | Core single-cell analysis (QC, normalization, PCA, UMAP, clustering) |
| `anndata` | Annotated data structure for single-cell data |
| `pooch` | Dataset downloading and caching |
| `igraph` | Backend for fast Leiden clustering |
| `scikit-misc` | Required for `seurat_v3` highly variable gene selection |

**Pipeline:**
1. Load and concatenate two 10x Genomics samples into a single AnnData object
2. Compute QC metrics: mitochondrial, ribosomal, and hemoglobin gene percentages; total counts; genes per cell
3. Filter low-quality cells (min 100 genes) and rarely-detected genes (min 3 cells)
4. Detect doublets per-sample using Scrublet
5. Identify the top 10,000 highly variable genes across samples
6. Run PCA, then build a neighbor graph and compute a UMAP embedding
7. Cluster cells using the Leiden algorithm
8. Cross-validate clusters against QC metrics and doublet scores

## Key Results
- Successfully combined two PBMC samples while preserving sample-level batch information for downstream batch-aware analysis
- QC filtering and doublet detection identified and flagged technical artifacts before clustering
- UMAP visualization shows reasonable overlap between samples, suggesting cell population structure is driven by genuine biology rather than batch effects
- Leiden clustering identified multiple distinct cell populations, cross-validated against QC metrics to rule out clusters driven purely by technical quality differences

## How to Run

**Option 1 — Google Colab (recommended)**
1. Upload `Single_Cell_RNAseq_Scanpy.ipynb` to Google Colab
2. Run all cells top to bottom — the dataset downloads automatically via `pooch`

**Option 2 — Local environment**
```bash
git clone https://github.com/yourusername/single-cell-rnaseq-scanpy
cd single-cell-rnaseq-scanpy
pip install -r requirements.txt
jupyter notebook Single_Cell_RNAseq_Scanpy.ipynb
```

## Project Structure
```
single-cell-rnaseq-scanpy/
├── README.md
├── Single_Cell_RNAseq_Scanpy.ipynb
└── requirements.txt
```
