# Exploratory GWAS and Expression Integration in Alzheimer's Disease

A reproducible baseline workflow that combines Alzheimer's disease GWAS summary statistics (Jansen et al. 2019) with entorhinal cortex expression data (GSE131617). The workflow produces exploratory candidate genes. It does not prove that any gene is involved in the disease.

> **Note:** An earlier version of this repository (before the revision) used a mismatched genome build and an incorrect probe annotation. Its results are not valid and are kept in `archive/original_submission/` for transparency only. Please use `revision_analysis.py`.

## Data

Download these files and put them in one folder (`DATA_DIR` in the script):

- **GWAS summary statistics:** Jansen et al. 2019, https://ctg.cncr.nl/software/summary_statistics (not needed to run the script, because the 2,394 significant SNPs are already in `Supplementary_Table_S1_significant_SNPs.csv`)
- **Expression data:** GEO GSE131617, file `GSE131617-GPL5175_series_matrix.txt`
- **Platform annotation:** GEO GPL5175, file `GPL5175-3188.txt`
- **Gene annotation:** Ensembl release 87 (GRCh37). The script downloads `Homo_sapiens.GRCh37.87.gtf.gz` by itself on the first run.

## Requirements

Python 3.11.4 with numpy 1.26.4, pandas 2.3.3, scipy 1.17.1 and gseapy 1.1.10. `make_figure3.py` also uses matplotlib and scikit-learn. All versions are in `requirements.txt`.

## How to run

1. Put the input files in one folder and set `DATA_DIR` at the top of `revision_analysis.py`.
2. Run `python revision_analysis.py`. All tables are written to `revision_outputs/`.
3. Run `python make_figure3.py` to make the PCA figure.

The random seed is 42. The Enrichr step uses the internet, so its results can change a little if the database is updated. The script also prints the software versions.

## What the script does

1. Maps the GWAS SNPs to genes on GRCh37 (distance 0 inside a gene, otherwise the nearest gene within 50 kb). It checks that rs429358 falls in APOE.
2. Maps probes to gene symbols with checked Ensembl symbols and removes unclear probes.
3. Tests AD against control (Braak V to VI against Braak 0) with a moderated linear model with sex.
4. Tests the overlap with a hypergeometric test (background: the tested genes) and a sex-stratified permutation test.
5. Runs the sensitivity analyses: no sex adjustment, no APOE region, protein-coding genes only, windows of 20, 50, 100 and 200 kb, Mann-Whitney test, and a model on all 71 entorhinal cortex samples.
6. Runs a threshold-free test and a ranked gene-set analysis (KEGG and GO), and Enrichr for the candidate genes.

## Main results

- 2,394 genome-wide significant SNPs gave 128 GWAS genes (60 of them on the array).
- 14,172 genes were tested. No probe passes FDR (smallest BH p = 0.075). At p < 0.05, 945 genes were selected.
- 3 genes are in both lists (AGFG2, PVRL2, SLC24A4). About 4.0 are expected by chance (hypergeometric p = 0.77, permutation p = 0.37). The result is the same in all sensitivity analyses.
- The ranked gene-set results are exploratory.

All genes are exploratory candidates only.

## Changes from the first version

- GRCh37 gene annotation (Ensembl 87) instead of GRCh38.
- Validated probe-to-gene mapping.
- Background changed to the tested genes (14,172).
- Sex added to the model, with moderated variances.
- Sex-stratified permutation test.
- New analyses: APOE-region exclusion, protein-coding only, window sensitivity, all-sample Braak model, threshold-free test, ranked gene sets.
- The old claims about 8 genes, glycosaminoglycan and osteoclast were removed.

## Citation

If you use this workflow, please cite the paper (reference to be added after publication) and the archived version: DOI [ZENODO DOI]
