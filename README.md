# Transcriptomic Profiling of Breast Cancer Subtypes

A complete four-phase RNA-seq analysis pipeline applied to TCGA-BRCA data, comparing Basal, Her2, LumA, and LumB breast cancer subtypes against normal tissue. The project spans preprocessing and QC through differential expression, functional enrichment, and protein–protein interaction network analysis.

---

## Project Overview

| Phase | Notebook | Description |
|-------|----------|-------------|
| 1 | `phase1_preprocessing_qc.ipynb` | Data loading, gene deduplication, QC / EDA |
| 2 | `phase2_differential_expression.ipynb` | PyDESeq2 subtype-vs-normal DEA |
| 3 | `phase3_functional_enrichment.ipynb` | GO BP + KEGG pathway enrichment |
| 4 | `phase4_network_analysis.ipynb` | STRING PPI network construction and analysis |

---

## Dataset

- **Source:** TCGA-BRCA RNA-seq (pre-filtered expression matrix)
- **Samples:** 1,118 total across five groups
  - Basal: 181 | Her2: 77 | LumA: 528 | LumB: 198 | Normal: 134
- **Starting matrix:** 60,660 genes → 59,427 unique genes after deduplication

---

## Phase 1 — Preprocessing and QC

- Loaded pre-filtered TCGA-BRCA expression matrix (`exp_data_filtered.pkl`)
- Deduplicated gene names by retaining the entry with the highest mean expression
- Generated a strict, DEA-ready matrix (`exp_data_filtered_strict.pkl`) and matching metadata
- Exploratory plots: library size distributions, PCA subtype clustering, sample–sample correlation heatmap

---

## Phase 2 — Differential Expression Analysis

Four subtype-vs-normal comparisons using **PyDESeq2**:

| Comparison | Significant DEGs |
|---|---|
| Basal vs Normal | 6,603 |
| Her2 vs Normal | 6,372 |
| LumA vs Normal | 4,544 |
| LumB vs Normal | 6,737 |

**Thresholds:** \|log2FC\| > 1.5, adjusted p-value < 0.01

- Total unique DEGs across all subtypes: **11,786**
- Shared across all four subtypes: **1,611 genes**
- Top-84 union DEGs visualized in a cross-subtype heatmap

---

## Phase 3 — Functional Enrichment

GO Biological Process and KEGG pathway enrichment on subtype-specific DEG lists (activated and suppressed gene sets analyzed separately):

- **GO BP:** subtype-level dot plots highlighting enriched biological processes
- **KEGG:** pathway-level enrichment tables for all four subtypes
- Results split into activated / suppressed CSVs per subtype, plus a merged all-subtypes table

---

## Phase 4 — PPI Network Analysis

STRING-based protein–protein interaction networks built from the top 100 DEGs per subtype:

| Subtype | Connected genes | Edges | Density | Top hub gene |
|---|---|---|---|---|
| Basal | 39 | 187 | 0.038 | CCNB1 |
| Her2 | 41 | 153 | 0.031 | CDK1 |
| LumA | 14 | 10 | 0.002 | ESR1 |
| LumB | 49 | 307 | 0.062 | CDK1 |

- Community detection via Louvain algorithm
- Hub gene ranking by weighted degree, betweenness, and closeness centrality
- Network visualizations saved per subtype

---

## Repository Structure

```
├── phase1_preprocessing_qc.ipynb
├── phase2_differential_expression.ipynb
├── phase3_functional_enrichment.ipynb
├── phase4_network_analysis.ipynb
│
├── exp_data_filtered.pkl          # filtered expression matrix (Phase 1 output)
├── exp_data_filtered_strict.pkl   # strict DEA-ready matrix
├── metadata.pkl                   # subtype annotation table
├── metadata_strict.pkl            # strict metadata table
│
├── figures/
│   ├── library_sizes.png
│   ├── pca_subtypes.png
│   ├── correlation_heatmap.png
│   ├── volcano_plots.png
│   ├── top_degs_heatmap.png
│   ├── phase3/
│   │   ├── phase3_go_bp_Basal_dot.png
│   │   ├── phase3_go_bp_Her2_dot.png
│   │   ├── phase3_go_bp_LumA_dot.png
│   │   └── phase3_go_bp_LumB_dot.png
│   └── phase4/
│       ├── Basal_ppi_network.png
│       ├── Her2_ppi_network.png
│       ├── LumA_ppi_network.png
│       ├── LumB_ppi_network.png
│       └── network_summary.png
│
└── results/
    ├── DEGs_<Subtype>_vs_Normal.csv          # significant DEGs per subtype
    ├── DESeq2_full_<Subtype>_vs_Normal.csv   # full DESeq2 output per subtype
    ├── phase3/
    │   ├── go_bp_all_subtypes.csv
    │   ├── go_bp_<Subtype>_activated.csv
    │   ├── go_bp_<Subtype>_suppressed.csv
    │   ├── kegg_all_subtypes.csv
    │   ├── kegg_<Subtype>_activated.csv
    │   └── kegg_<Subtype>_suppressed.csv
    ├── phase4/
    │   ├── network_summary.csv
    │   ├── hub_genes_<Subtype>.csv
    │   ├── community_summary_<Subtype>.csv
    │   ├── selected_genes_<Subtype>.csv
    │   └── string_edges_<Subtype>.csv
    └── qc/
        ├── expression_matrix_audit.md
        ├── expression_sample_audit.csv
        ├── strict_filter_candidates.csv
        └── suspect_noninteger_samples.csv
```

---

## Key Findings

- **Basal** is the most transcriptionally divergent subtype, with strong upregulation of cell-cycle and DNA-repair genes (CCNB1, BUB1, EXO1) and the densest PPI network among proliferative subtypes.
- **LumA** shows the fewest DEGs and the sparsest network, consistent with its well-differentiated, hormone-driven phenotype; ESR1 is the dominant hub.
- **LumB** has the most edges and highest network density, with CDK1 as its top hub — reflecting its more aggressive proliferative character relative to LumA.
- **Her2** shares proliferative hub genes (CDK1) with LumB but has a distinct enrichment profile reflecting ERBB2 pathway activation.
- Shared DEGs across all four subtypes (1,611 genes) likely represent a pan-cancer transcriptional signature common to breast tumors regardless of subtype.
