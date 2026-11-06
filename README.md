# Cho2017 — Minimum Viable EEG

**Reduced-montage EEG decoding on the Cho2017 motor-imagery dataset**

This repository is the Cho2017 results companion for the **Minimum Viable EEG** study. The experiment asks:

> **How far can a 64-channel EEG montage be reduced while preserving most of the decoding performance of the full montage?**

The primary task is **left-hand vs. right-hand motor imagery** across **52 subjects**. Channel selection is performed using training data only, the channel budget is selected on validation subjects, and the held-out test set is evaluated only after the budget is frozen.

---

## Headline result

The validation rule selected **16 channels** as the smallest budget retaining at least **90% of the full 64-channel validation Cohen's κ**.

| Condition | Channels | Validation κ | Held-out test κ (mean ± SD) | Test accuracy | Test retention vs. 64ch |
|---|---:|---:|---:|---:|---:|
| Ranked reduced montage | **16** | **0.2285** | **0.2588 ± 0.0141** | **0.6293** | **79.83%** |
| Full montage | 64 | 0.2493 | 0.3241 ± 0.0165 | 0.6621 | 100% |

The important methodological point is that **k = 16 was frozen from validation performance**. The lower retention observed on previously unseen test subjects does **not** trigger re-selection of the montage.

---

## Dataset and task

- **Dataset:** Cho2017
- **Subjects:** 52
- **Full montage:** 64 EEG channels
- **Task:** left-hand vs. right-hand motor imagery
- **Trials:** 10,520 total across subjects
- **Epoch:** 0–3 s
- **Band-pass:** 8–30 Hz
- **Resampling:** 160 Hz
- **Samples per epoch:** 480
- **Primary metric:** pooled Cohen's κ

The experiment uses a compact EEGNet-style decoder.

---

## Subject-wise split

A fixed subject-wise split prevents trials from the same participant from crossing train, validation, and test partitions.

| Split | Subjects | Count |
|---|---|---:|
| Train | 4, 5, 6, 8, 10, 11, 16, 17, 18, 19, 21, 22, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 35, 38, 39, 40, 41, 42, 43, 45, 46, 47, 48, 49, 50, 52 | 36 |
| Validation | 1, 7, 12, 13, 20, 23, 36, 44 | 8 |
| Test | 2, 3, 9, 14, 15, 34, 37, 51 | 8 |

Split seed: **42**.

The exact split is stored in [cho2017_artifacts/splits.json](cho2017_artifacts/splits.json).

---

## Experimental pipeline

1. **Preprocess EEG**
   - 8–30 Hz filtering
   - 0–3 s motor-imagery epochs
   - 160 Hz resampling

2. **Freeze subject-wise train / validation / test split**
   - 36 train
   - 8 validation
   - 8 held-out test

3. **Rank channels using training subjects only**
   - Fisher log-bandpower subject-mean ranking
   - no validation or test labels are used to construct the ranking

4. **Evaluate channel budgets**
   - 4, 6, 8, 12, 16, 32, 64 channels
   - five training seeds per ranked condition

5. **Select the minimum viable budget**
   - choose the smallest budget retaining at least 90% of full-montage validation κ

6. **Run controls**
   - equal-budget random subsets
   - sensorimotor montage
   - label-shuffle negative control
   - knowledge distillation
   - shuffled-teacher distillation control

7. **Freeze k = 16**

8. **Evaluate once on held-out test subjects**
   - five seeds at k = 16
   - five seeds at k = 64

---

## Validation selection

The paper-facing frozen summary reports:

- **16-channel validation κ:** 0.2285
- **64-channel validation κ:** 0.2493
- **Validation retention:** 91.7%
- **Selection threshold:** ≥ 90% of full-montage validation κ
- **Selected budget:** **k* = 16**

This budget was selected **before** examining held-out test performance.

---

## Equal-budget controls

Reduced montages were compared against random subsets at the **same channel budget** rather than against a mismatched baseline.

For the selected 16-channel budget, the exported run records contain:

- **100 random validation runs**
- 20 random channel subsets × 5 training seeds
- random-subset mean κ ≈ **0.210**
- ranked 16-channel paper-facing validation κ = **0.2285**
- sensorimotor 16-channel validation mean κ ≈ **0.253**

Random-baseline files are included for k = 4, 6, 8, 12, 16, and 32.

---

## Distillation controls

The repository includes both ordinary distillation and a shuffled-teacher control for the small channel budgets k = 4, 6, and 8.

Each condition contains five training seeds.

| Budget | Distillation mean κ | Shuffled-teacher mean κ |
|---:|---:|---:|
| 4 | 0.0966 | 0.0510 |
| 6 | 0.1037 | 0.0739 |
| 8 | 0.1063 | 0.0502 |

These controls test whether any benefit depends on informative teacher structure rather than simply adding a teacher-loss term.

---

## Held-out test result

After freezing **k = 16** from validation:

| Condition | Mean κ | SD κ | Mean accuracy |
|---|---:|---:|---:|
| Ranked 16-channel montage | **0.2587806** | **0.0140831** | **0.629302** |
| Full 64-channel montage | **0.3241464** | **0.0164954** | **0.6620732** |

Final held-out retention:

~~~text
0.2587806 / 0.3241464 = 0.7983448
~~~

So the validation-selected 16-channel montage retained approximately **79.83%** of full-montage κ on the unseen test subjects.

This validation-to-test gap is part of the result: the selection criterion succeeded on validation but did not fully carry over to new subjects. The test set is therefore treated as an evaluation set, **not a second opportunity to choose k**.

---

## Repository contents

~~~text
.
├── cho2017_artifacts/
│   ├── budgets.json
│   ├── channel_ranking.json
│   └── splits.json
├── cho2017_results/
│   ├── ranked_k64_val.jsonl
│   ├── ranked_val_all_remaining.jsonl
│   ├── random_k4_val.jsonl
│   ├── random_k6_val.jsonl
│   ├── random_k8_val.jsonl
│   ├── random_k12_val.jsonl
│   ├── random_k16_val.jsonl
│   ├── random_k32_val.jsonl
│   ├── sensorimotor_k16_val.jsonl
│   ├── sensorimotor_remaining_val.jsonl
│   ├── distill_val.jsonl
│   ├── distill_shuffled_teacher_val.jsonl
│   ├── test_k16.jsonl
│   └── test_k64.jsonl
├── cho2017_paper_table.csv
└── final_summary.json
~~~

### Paper-facing files

For manuscript tables and headline numbers, use:

- [final_summary.json](final_summary.json)
- [cho2017_paper_table.csv](cho2017_paper_table.csv)

### Audit-level files

The JSONL files in [cho2017_results/](cho2017_results/) preserve individual exported run records, including seeds, κ, accuracy, subject-level statistics, runtime, environment, and provenance fields where available.

---

## Provenance note

This repository was assembled from saved experiment outputs after notebook-session recovery. The compact files **final_summary.json** and **cho2017_paper_table.csv** are the **frozen paper-facing aggregates**.

Some recovered intermediate JSONL/artifact exports are retained for transparency and auditability and may differ slightly from the frozen aggregate values or from one another in recovered channel-order metadata. They should not be used to retroactively alter the validation-selected budget or the reported held-out result.

In particular:

- **k* = 16 remains fixed from validation**
- the test set is never used for channel-budget selection
- raw/processed EEG arrays and model caches are intentionally not stored in this repository

---

## Reproducibility principles

The experiment is designed around the following safeguards:

- subject-wise splitting
- training-only feature/channel ranking
- frozen validation-based model-selection rule
- equal-budget random baselines
- domain-informed sensorimotor baseline
- negative controls
- multiple training seeds
- explicit run-level provenance
- held-out test evaluation only after selection

This repository is currently a **results and provenance companion**, not a standalone distribution of the Cho2017 dataset or the full training environment.

---

## Key interpretation

The central result is not that 16 channels reproduce the 64-channel model perfectly.

Instead, Cho2017 shows that a montage can satisfy a predefined compression criterion on validation subjects and still experience a meaningful performance drop on unseen subjects. That gap is scientifically useful: **channel compression, channel selection, and cross-subject generalization are distinct problems**.

---

## Status

**Cho2017 experiment: complete.**

- [x] preprocessing
- [x] subject-wise split
- [x] training-only channel ranking
- [x] ranked validation sweep
- [x] validation-based budget selection
- [x] equal-budget random baselines
- [x] sensorimotor controls
- [x] negative controls
- [x] distillation controls
- [x] held-out 16-channel test
- [x] held-out 64-channel reference test
- [x] results packaging

---

## Citation

A formal paper citation will be added when the associated manuscript is publicly available.

For now, please cite this repository as:

> Lockett, J. E. *Cho2017 Minimum Viable EEG: Reduced-Montage Motor-Imagery EEG Decoding.* GitHub repository, 2026.

---

## Author

**Jahari Eve Lockett**

Research focus: machine learning, EEG/BCI, efficient neural decoding, and reproducible evaluation.
