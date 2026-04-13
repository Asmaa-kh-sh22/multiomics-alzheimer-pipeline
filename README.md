# Multi-Omics Integration Pipeline for Alzheimer's Disease

A transparent and reproducible computational pipeline that integrates 
GWAS summary statistics with post-mortem brain transcriptomic data to 
identify candidate genes for Alzheimer's disease.

## Datasets

### Included in this repository
- Notebook only — data files are publicly available (see below)

### Download required before running
- **GWAS summary statistics**: Jansen et al. 2019 — download from
  https://ctg.cncr.nl/software/summary_statistics
  File name: AD_sumstats_Jansenetal_2019sept.txt (~1.8 GB)
- **Transcriptomics**: GEO accession GSE131617 — download from
  https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE131617
  File name: GSE131617-GPL5175_series_matrix.txt
- **Platform annotation**: GPL5175 — download from
  https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GPL5175
  File name: GPL5175-3188.txt
- **Ensembl annotation**: Downloaded automatically in Cell 14

## Requirements

Python 3.12 with the following packages:
- pandas 1.5.3
- numpy 1.24.3
- scipy 1.10.1
- statsmodels 0.14.0
- scikit-learn 1.6.1
- matplotlib 3.7.1
- seaborn 0.12.2
- gseapy 1.1.10
- dask 2023.6.0

## Usage

1. Download all three data files from the links above
2. Place them in the same folder as the notebook
3. Open Multiomics-alzheimer-pipeline.ipynb in Jupyter Notebook
4. Update the data_folder path in Cell 1 to your local directory
5. Run all cells in order (Kernel → Restart & Run All)

## Key Results

- 8 candidate genes with convergent GWAS and transcriptomic evidence
- All 8 genes downregulated in Braak V-VI entorhinal cortex vs Braak 0
- Significant KEGG pathway: Glycosaminoglycan biosynthesis (adj p = 0.026)
- Overlap genes: ERCC2, FOSB, OPA3, QPCTL, RELB, RTN2, SNRPD2, TMEM126A

## Citation

[To be added after publication]
