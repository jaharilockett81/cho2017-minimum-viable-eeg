# Cho2017 Minimum Viable EEG

This repository contains the saved Cho2017 experiment result files.

The summary below is taken directly from:

- [final_summary.json](final_summary.json)
- [cho2017_paper_table.csv](cho2017_paper_table.csv)

No additional claims or interpretation are added here.

## Experiment summary

| Field | Value |
|---|---:|
| Dataset | Cho2017 |
| Subjects | 52 |
| Full channel count | 64 |
| Task | left_vs_right_motor_imagery |
| Train subjects | 36 |
| Validation subjects | 8 |
| Test subjects | 8 |
| Selected k | 16 |
| Selection rule | smallest budget retaining >=90% of full validation kappa |

## Reported results

| Condition | Channels | Validation kappa | Test kappa mean | Test kappa std | Test accuracy mean | Retention vs 64ch |
|---|---:|---:|---:|---:|---:|---:|
| Ranked reduced montage | 16 | 0.2285 | 0.2587806 | 0.0140831 | 0.629302 | 0.7983448 |
| Full montage | 64 | 0.2493 | 0.3241464 | 0.0164954 | 0.6620732 | 1.0 |

## Validation summary

From `final_summary.json`:

- k16 mean kappa: **0.2285**
- k64 mean kappa: **0.2493**
- retention: **0.917**

## Test summary

From `final_summary.json`:

- k16 mean kappa: **0.2587806**
- k16 kappa std: **0.0140831**
- k16 mean accuracy: **0.629302**
- k64 mean kappa: **0.3241464**
- k64 kappa std: **0.0164954**
- k64 mean accuracy: **0.6620732**
- retention: **0.7983448**

## Files

### Artifacts

- [cho2017_artifacts/](cho2017_artifacts/)

### Results

- [cho2017_results/](cho2017_results/)

### Summary files

- [final_summary.json](final_summary.json)
- [cho2017_paper_table.csv](cho2017_paper_table.csv)
