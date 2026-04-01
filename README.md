# Transcriptomic Profiling of Breast Cancer Subtypes

This project analyzes TCGA-BRCA RNA-seq data to compare major breast cancer molecular subtypes against normal tissue. So far, the work in this folder covers Phase 1 quality control / exploratory data analysis and a substantial part of Phase 2 differential expression analysis.

## What has been done so far

### 1. Preprocessing and filtering
- A pre-filtered TCGA-BRCA expression matrix was used as the starting point for this notebook.
- The dataset was already cleaned before this analysis notebook begins, including sample selection / subtype labeling needed for the five groups used here:
  - Basal: 181 samples
  - Her2: 77 samples
  - LumA: 528 samples
  - LumB: 198 samples
  - Normal: 134 samples
- The main analysis starts from `exp_data_filtered.pkl`, which represents the pre-filtered expression data handed off for downstream QC and DEA.

### 2. Phase 1: QC and exploratory analysis
- Loaded the pre-filtered TCGA-BRCA expression matrix.
- Performed gene-name deduplication by keeping the duplicate entry with the highest mean expression.
- Reduced the matrix from 60,660 rows to 59,427 unique genes across 1,118 samples.
- Generated exploratory plots for:
  - library size distributions by subtype
  - PCA subtype clustering
  - sample-sample correlation heatmap

### 3. Phase 2: differential expression analysis
- Ran PyDESeq2 subtype-vs-normal comparisons for:
  - Basal vs Normal
  - Her2 vs Normal
  - LumA vs Normal
  - LumB vs Normal
- Used significance thresholds:
  - `|log2FC| > 1.5`
  - `adjusted p-value < 0.01`
- Generated:
  - volcano plots for all four comparisons
  - DEG summary counts
  - heatmap of top DEGs across subtypes
  - subtype-specific vs shared DEG count summary

## Current results summary

- Basal vs Normal: 6,603 significant DEGs
- Her2 vs Normal: 6,372 significant DEGs
- LumA vs Normal: 4,544 significant DEGs
- LumB vs Normal: 6,737 significant DEGs
- Union of top subtype DEGs used in the heatmap: 84 genes
- Total unique significant DEGs across all four subtype comparisons: 11,786
- Shared across all four subtypes: 1,611 genes

## Files in this folder

### Main notebook
- `phase1_qc.ipynb`: main notebook for Phase 1 QC and Phase 2 DEA

### Input data
- `exp_data_filtered.pkl`: pre-filtered expression matrix used as notebook input

### Figures
- `figures/library_sizes.png`
- `figures/pca_subtypes.png`
- `figures/correlation_heatmap.png`
- `figures/volcano_plots.png`
- `figures/top_degs_heatmap.png`

### Results tables
- `results/DEGs_Basal_vs_Normal.csv`
- `results/DEGs_Her2_vs_Normal.csv`
- `results/DEGs_LumA_vs_Normal.csv`
- `results/DEGs_LumB_vs_Normal.csv`
- `results/DESeq2_full_Basal_vs_Normal.csv`
- `results/DESeq2_full_Her2_vs_Normal.csv`
- `results/DESeq2_full_LumA_vs_Normal.csv`
- `results/DESeq2_full_LumB_vs_Normal.csv`

## Project status

Completed so far:
- partner preprocessing / filtering handoff
- Phase 1 QC / EDA
- Phase 2 subtype-vs-normal DEA

Still to do:
- between-subtype comparisons, if kept in final scope
- Phase 3 functional enrichment and pathway analysis
- Phase 4 phylogenetic / homology analysis
- Phase 5 PPI / network analysis
- final integrated report and presentation

## Note

This README summarizes the current project state based on the saved notebook, generated figures, and result files in this folder. If you want a more reproducible final version later, the preprocessing section can be expanded with the exact filtering thresholds and metadata preparation steps your partner used.
