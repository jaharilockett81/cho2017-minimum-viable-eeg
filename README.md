# Cho2017 — Minimum Viable EEG

Reduced-channel motor-imagery EEG decoding on the Cho2017 dataset.

## Key findings

- **16 channels met the predefined 90% validation-retention criterion.**
  - 16-channel validation kappa: **0.2285**
  - 64-channel validation kappa: **0.2493**
  - validation retention: **91.7%**

- **Performance changed sharply between 12 and 16 channels.**
  - 12 channels: kappa ≈ **0.108**
  - 16 channels: kappa ≈ **0.226**
  - 32 channels: kappa ≈ **0.248**
  - 64 channels: kappa ≈ **0.249**

- **The unrestricted channel ranking was not consistently superior to equal-budget alternatives.**
  - At 16 channels:
    - ranked: kappa ≈ **0.226**
    - random mean: kappa ≈ **0.210**
    - sensorimotor: kappa ≈ **0.253**

- **Sensorimotor-informed montages outperformed the ranked montage at several small budgets.**
  - k=8: ranked ≈ **0.091**, sensorimotor ≈ **0.232**
  - k=12: ranked ≈ **0.108**, sensorimotor ≈ **0.252**

- **Knowledge distillation improved the small-budget Cho2017 models in the committed validation runs.**
  - k=4: 0.0468 → **0.0966**
  - k=6: 0.0724 → **0.1037**
  - k=8: 0.0915 → **0.1063**

## Validation budget curve

| Channels | Ranked validation kappa | Retention vs. 64ch |
|---:|---:|---:|
| 4 | 0.0468 | 18.8% |
| 6 | 0.0724 | 29.1% |
| 8 | 0.0915 | 36.7% |
| 12 | 0.1076 | 43.2% |
| **16** | **0.2263** | **90.8%** |
| 32 | 0.2483 | 99.6% |
| 64 | 0.2493 | 100% |

The paper-facing summary files report the selected 16-channel validation value as **0.2285** and validation retention as **0.917**.

## Ranked vs. random vs. sensorimotor

| Channels | Ranked | Random mean | Sensorimotor |
|---:|---:|---:|---:|
| 4 | 0.0468 | 0.1136 | 0.0922 |
| 6 | 0.0724 | 0.1320 | 0.1346 |
| 8 | 0.0915 | 0.1500 | 0.2320 |
| 12 | 0.1076 | 0.2021 | 0.2522 |
| 16 | 0.2263 | 0.2096 | 0.2534 |

Random-baseline values are means over the committed equal-budget random validation runs.

## Distillation

| Channels | Scratch | Distillation | Shuffled-teacher control |
|---:|---:|---:|---:|
| 4 | 0.0468 | 0.0966 | 0.0510 |
| 6 | 0.0724 | 0.1037 | 0.0739 |
| 8 | 0.0915 | 0.1063 | 0.0502 |

## Comparison with EEGMMIDB

Using the same 90% validation-retention criterion:

| Dataset | Full channels | Selected validation budget | Validation retention |
|---|---:|---:|---:|
| EEGMMIDB | 64 | **32** | **94.8%** |
| Cho2017 | 64 | **16** | **~91%** |

The two repositories support substantial channel reduction on both datasets, while the exact small-budget channel rankings differ substantially.

EEGMMIDB reference: [jaw039/min-viable-eeg](https://github.com/jaw039/min-viable-eeg)

## Current held-out test status

The Cho2017 held-out test export is currently being reconciled because the committed test-channel metadata does not match the committed Cho2017 channel-ranking artifact.

For that reason, the validation and control findings above are highlighted separately from the held-out test result until that provenance mismatch is resolved.

The current summary files remain available below, but the test claim should not be treated as finalized until the artifact reconciliation is complete.

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

## Files

### Summary

- [final_summary.json](final_summary.json)
- [cho2017_paper_table.csv](cho2017_paper_table.csv)

### Artifacts

- [cho2017_artifacts/](cho2017_artifacts/)

### Run-level results

- [cho2017_results/](cho2017_results/)
