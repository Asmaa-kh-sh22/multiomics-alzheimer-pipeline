# Multi-Omics Integration Pipeline for Alzheimer's Disease

A transparent and reproducible computational pipeline that integrates 
GWAS summary statistics with post-mortem brain transcriptomic data to 
identify exploratory candidate genes for Alzheimer's disease.

## Datasets

### Included in this repository
- Notebook only; data files are publicly available (see below)

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
- **Ensembl annotation**: GRCh38 release 109, downloaded automatically in Cell 14

## Requirements

Python [3.11.4] with the following packages:
- pandas 1.5.3
- numpy 1.24.3
- scipy 1.10.1
- statsmodels 0.14.0
- scikit-learn 1.6.1
- matplotlib 3.7.1
- seaborn 0.12.2
- gseapy 1.1.10
- dask 2023.6.0

A full frozen environment is provided in `requirements.txt` (run `pip freeze > requirements.txt` after your final run to keep this current).

## Usage

1. Download all three data files from the links above
2. Place them in the same folder as the notebook
3. Open `Multiomics-alzheimer-pipeline-V2.ipynb` in Jupyter Notebook
4. Update the `data_folder` path in Cell 1 to your local directory
5. Run all cells in order (Kernel → Restart & Run All)
6. Full run time: approximately [120] minutes, due to the GWAS mapping and window-sensitivity steps (Cells 15, 20c)

## Reproducibility notes

- Random seed fixed at 42 for the permutation test (Cell 20e), ensuring exact reproducibility across reruns
- Functional enrichment (Cells 21, 21b) queries the Enrichr API live; results reflect the database state as of the access date noted in each cell's output — re-running at a later date may return slightly different results as Enrichr's underlying databases are updated
- Ensembl annotation release: 109

## Notebook structure

- Cells 1–12: data loading, QC, normalization, differential expression, FDR correction
- Cells 13–19: volcano plot, GWAS filtering, gene mapping, probe-to-gene mapping
- Cell 20: GWAS–transcriptomic set intersection
- Cells 20a–20e: sensitivity and robustness analyses (hypergeometric test, LD/APOE clustering, GWAS mapping window sensitivity, Mann-Whitney U robustness check, permutation test)
- Cell 20f: export of supplementary data files (SNP list, gene lists)
- Cells 21–21b: functional enrichment (full DEG list; candidate-gene-only)
- Cells 22–24: verification checks (candidate gene confirmation, sample composition, normalization spot-check)
- Cell 25: pipeline decision-path summary figure
- Cells 26–29: consolidated robustness table, covariate availability check, PCA restricted to analytical samples, computational environment capture

## Key Results

- 8 exploratory candidate genes with convergent GWAS and transcriptomic evidence at an uncorrected p < 0.05 threshold: *ERCC2*, *FOSB*, *OPA3*, *QPCTL*, *RELB*, *RTN2*, *SNRPD2*, *TMEM126A*
- All 8 genes downregulated in Braak V–VI entorhinal cortex vs. Braak 0
- 7 of 8 candidates confirmed under a non-parametric Mann-Whitney U test
- All 8 candidates stable across GWAS mapping windows from 20 to 200 kb
- Positional clustering indicates the 8 candidates represent as few as 2 independent genetic loci (7 genes cluster near *APOE* on chromosome 19; *TMEM126A* is independent on chromosome 11)
- Permutation test: empirical p = 0.143 (not significant); hypergeometric test with platform-restricted background: p = 0.048 (borderline, but likely inflated by unmodeled LD — see manuscript Discussion)
- No probes survived FDR correction; all reported gene lists should be treated as exploratory, not confirmatory
- KEGG enrichment (full DEG list): glycosaminoglycan biosynthesis, adjusted p = 0.026
- KEGG enrichment (8-candidate set only): osteoclast differentiation, adjusted p = 0.014 (2-gene overlap); remaining nominally significant GO terms are single-gene overlaps consistent with small-gene-set correction artifacts
- Complete SNP and gene lists are provided as supplementary CSV files (Supplementary Tables S1–S4) for independent verification

## Citation

If you use this pipeline, please cite: [PAPER CITATION — TO BE ADDED UPON PUBLICATION]
- Overlap genes: ERCC2, FOSB, OPA3, QPCTL, RELB, RTN2, SNRPD2, TMEM126A
- DEG threshold: uncorrected p < 0.05 (FDR correction yields 0 genes due to high-dimensional low-sample regime)
