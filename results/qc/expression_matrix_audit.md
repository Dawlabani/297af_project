# Expression Matrix Audit And Strict Cleanup

- Source matrix: `exp_data_filtered.pkl`
- Source shape: `60,660 genes x 1,118 samples`
- Duplicate gene-name rows in source: `1,233`
- Suspect samples with non-integer counts: `5`
- Deduplicated shape: `59,427 genes x 1,118 samples`
- Strict filter applied: `count > 10` in at least `10` samples after dropping suspect non-integer samples
- Strict output shape: `33,349 genes x 1,113 samples`
- Strict matrix written to: `exp_data_filtered_strict.pkl`
- Strict metadata written: `True`

## Suspect Non-Integer Samples

- `TCGA-A7-A13E-01A`: `22,375` non-integer entries
- `TCGA-A7-A26E-01A`: `21,450` non-integer entries
- `TCGA-A7-A0DB-01A`: `21,314` non-integer entries
- `TCGA-A7-A13D-01A`: `20,998` non-integer entries
- `TCGA-A7-A26J-01A`: `20,302` non-integer entries

## Recommendation

- Use the strict matrix for any rerun of Phase 2 onward.
- Treat the original `exp_data_filtered.pkl` as the lightly filtered handoff copy.
- If raw source files are available, the ideal fix is to rebuild the five suspect samples instead of averaging duplicate sample columns.
