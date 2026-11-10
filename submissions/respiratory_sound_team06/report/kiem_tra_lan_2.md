Progress Test 2 (Week 10) — Machine Learning-Based Classification of Respiratory Abnormalities from Lung Sound Signals

## Header

| Field | Value |
|---|---|
| Team code | team06 (class 176060, group N06) |
| Topic slug | respiratory_sound |
| Fork URL | https://github.com/MountTung/hmyt-project |
| Branch | `gd5-proposed-model` |
| Commit SHA (40 ký tự) | 
| Members (ID — role) | 20233986 — literature & gap & report; 20233952 — training |

---

## A. Literature review and research gaps

### A1. Search strategy

| Item | Value |
|---|---|
| Sources | Google Scholar, arXiv, the ICBHI challenge page / Papers with Code leaderboard for ICBHI (KIỂM TRA: ghi đúng nguồn nhóm đã dùng) |
| Queries | `respiratory sound classification ICBHI`; `lung sound device generalization`; `stethoscope domain shift respiratory sound`; `leave-one-device-out respiratory sound` (ĐIỀN thêm query thật) |
| Year range | 2019–2026 (ICBHI database paper 2019 → latest preprint 05/2026) |
| Hits screened | ĐIỀN |
| Papers kept in matrix | 11 (R1–R11 below) |

**Inclusion:** (i) uses ICBHI 2017 or another public respiratory-sound dataset; (ii) reports a quantitative result (ICBHI Score, F-score or accuracy); (iii) classification of abnormal sounds (crackle/wheeze/both/normal) or deals with device/domain shift; (iv) peer-reviewed venue or arXiv preprint with full text readable.
**Exclusion:** (i) no quantitative result; (ii) non-respiratory audio only (e.g., speech/cough-only COVID detection); (iii) full text not accessible; (iv) duplicate versions of the same work.

### A2. Literature matrix

| # | Citation | DOI / link | Dataset (n, source) | Method | Main result | External validation? | Main limitation |
|---|---|---|---|---|---|---|---|
| R1 | Rocha et al., "An open access database for the evaluation of respiratory sound classification algorithms", Physiol. Meas. 2019 | 10.1088/1361-6579/ab03ea | ICBHI 2017: 920 recordings, 126 patients, 6,898 cycles, 4 stethoscope devices | Dataset paper; defines official 60/40 split and ICBHI Score = (Sp+Se)/2 | Benchmark definition (no model result) | Not applicable — dataset paper | Device is confounded with patient cohort (see our EDA, C3); split file not in the Kaggle copy |
| R2 | Gairola et al., "RespireNet", EMBC 2021 | arXiv:2011.00196 · 10.48550/arXiv.2011.00196 | ICBHI | ResNet-34 on Mel-spectrograms; device-specific fine-tuning | ICBHI Score 56.2 (60/40 split) | No (same devices in train and test) | Needs device label at inference; no unseen-device test |
| R3 | Bae et al., "Patch-Mix Contrastive Learning with AST…", Interspeech 2023 | arXiv:2305.14032 · 10.48550/arXiv.2305.14032 | ICBHI | AST + Patch-Mix augmentation + contrastive loss | ICBHI Score 62.37 | No | ICBHI only; device shift not addressed |
| R4 | "Multi-View Spectrogram Transformer for Respiratory Sound Classification", 2023 | arXiv:2311.09655 · 10.48550/arXiv.2311.09655 | ICBHI | Multi-view patch ViT with gated fusion | ICBHI Score 66.55 | No | Split protocol unclear (KIỂM TRA) → number may not be comparable |
| R5 | Kim et al., "Stethoscope-guided Supervised Contrastive Learning (SG-SCL)", ICASSP 2024 | arXiv:2312.09603 · 10.48550/arXiv.2312.09603 | ICBHI | AST; stethoscope type as domain, supervised contrastive loss | 61.71 ± 1.61 (domain-adversarial variant 59.81) | No (devices seen in training) | No per-device scores; no unseen-device test |
| R6 | Kim et al., "BTS: Bridging Text and Sound Modalities for Metadata-Aided Respiratory Sound Classification", Interspeech 2024 | arXiv:2406.06786 · 10.48550/arXiv.2406.06786 | ICBHI | Audio + text metadata (incl. device) | 63.54 (−0.88 without metadata) | No | Metadata needed at inference; no held-out device |
| R7 | Ge et al., "Lungmix: A Mixup-Based Strategy for Generalization in Respiratory Sound Classification", 2024 | arXiv:2501.00064 · 10.48550/arXiv.2501.00064 | ICBHI + other respiratory datasets (KIỂM TRA danh sách) | Waveform-level mixup on AST | Up to +3.55 in cross-dataset tests | Yes — cross-dataset | Dataset-level shift, not device-level; gains are mixed |
| R8 | Nguyen & Pernkopf, "Crackle Detection in Lung Sounds Using Transfer Learning and Multi-Input CNN", 2021 | arXiv:2104.14921 · 10.48550/arXiv.2104.14921 | ICBHI + private recordings | Multi-input CNN, pre-train on ICBHI, fine-tune on private data | +9.84% F-score after transfer | Partial (private data) | Binary crackle task only |
| R9 | Hsu et al., "Benchmarking of eight RNN variants … HF_Lung_V1", PLOS ONE 2021 | 10.1371/journal.pone.0254134 | HF_Lung_V1 (self-collected, open access) | Eight RNN variants for breath-phase and adventitious-sound detection | ĐIỀN (lấy số chính từ bài) | No (KIỂM TRA) | Event-detection task, different from cycle classification; no device analysis |
| R10 | Koo et al., "Mitigating Stethoscope-Induced Shortcuts in Respiratory Sound Classification under Federated Domain Generalization…", arXiv 2026 | arXiv:2605.29862 · 10.48550/arXiv.2605.29862 | ICBHI + SPRSound; 5 target devices | BTS-CAFE (CLAP-based, device-style intervention) in federated leave-one-device-out | Avg OOD ICBHI Score 52.81 → 56.50 (+3.69 over BTS); per target: AKG 52.82, Meditron 54.60, Yunting 65.69, LittC2SE 66.24, Litt3200 43.15 | Yes — held-out device | 5-page preprint; most targets near or below the 50 constant-predictor reference; Littmann devices pooled in one setting; patients with >1 stethoscope removed; no classical-ML/CNN/CRNN baselines |
| R11 | SPRSound: SJTU paediatric respiratory sound database (IEEE DataPort) | 10.21227/nfkk-1x47 | Paediatric recordings (ĐIỀN n) | Dataset | Not applicable — dataset | Not applicable | Paediatric population, different device (Yunting) |

### A3. Research gaps

| Gap | Evidence from A2 | How our project addresses it |
|---|---|---|
| **G1 — Protocol gap.** Unseen-device evaluation on ICBHI is rare and not reported with a patient-disjoint, per-device protocol and a constant-predictor reference. | R2, R3, R4, R5, R6 evaluate only on devices seen in training; R10 is the only held-out-device study and it removes multi-device patients and pools Littmann devices; R7 shifts datasets, not devices. | Leave-one-device-out (LODO) with patient-disjoint folds, plus a pooled-Littmann variant comparable to R10 and an AKG ↔ Litt3200 pair (both COPD-only cohorts); always report the constant-"Normal" reference (ICBHI Score = 50). Evidence: EDA sections 8–10. |
| **G2 — Benchmark gap.** No head-to-head of classical ML, CNN and CRNN under the same device-shift protocol. | R10 compares only AST-based models (AST-CE, Patch-Mix CL, SG-SCL, BTS); R2–R6 report in-distribution scores only. | Same LODO folds for logreg / SVM / RF (hand-crafted features), CNN and CRNN (log-Mel), plus the constant baseline. Evidence: `icbhi_train.py`, `results.csv`. |
| **G3 — Preprocessing gap.** The effect of simple preprocessing choices (resampling, band-pass, normalization, cycle length) on device generalization is untested. | R10 tests mean subtraction / whitening on embeddings only; R2–R6 fix their front-end without ablation by device. | Ablation of band-pass on/off, peak vs default normalization, sample rate/duration, and a COPD-only control to separate device from diagnosis. Evidence: `--no_bandpass`, `--norm peak`, `--copd_only`. |

*Scope note:* the gaps are narrow because R10 already evaluates unseen devices; we claim the three differences above, not a general first.

### A4. PROBAST+AI appraisal of the most important paper

**Paper appraised:** R10, Koo et al. (2026).
**Why this paper:** it is the closest work to our gap and the only one in A2 that tests held-out devices on ICBHI.
**Draft appraisal (KIỂM TRA từng dòng với PDF trước khi nộp — đây là bản nháp, hãy đổi rating nếu bạn đọc thấy khác):**

| Domain | Risk of bias | Reasoning |
|---|---|---|
| 1. Participants and data sources | Unclear → High | Patients with >1 stethoscope are removed (reduces leakage but changes the population); Littmann devices pooled in one setting; AKG and Litt3200 cohorts are COPD-only in ICBHI, so device and diagnosis are confounded and the paper's device-shift results may partly reflect cohort shift (our EDA: AKG vs Litt3200 spectral distance 15.57 dB). |
| 2. Predictors | Low | Predictors are audio-derived (CLAP embeddings); the device label is not required at test time. Cycle boundaries come from manual annotation, which is not available in deployment (applicability issue). |
| 3. Outcome | Low–Unclear | Four-class cycle labels from ICBHI annotators; label noise not quantified. |
| 4. Analysis | High | Several target devices score near or below the 50 constant-predictor reference (Litt3200 43.15); FedAvg on AKG reaches Sp 99.23 / Se 1.29, i.e., almost always "Normal"; no calibration reported; number of seeds / confidence intervals ĐIỀN/KIỂM TRA; small test sets for LittC2SE and Litt3200. |

**Applicability to our problem:** Partly applicable — same dataset and device-shift question; differs by federated setting, CLAP backbone and inclusion of SPRSound (Yunting).

---

## B. Results and timeline

### B1. Current results

*Rule: every number must come from a real output file. Fill from `results.csv` produced by `icbhi_train.py` (`--summary`). If a cell has no run yet, write `Not yet — <reason>`.*

**Evaluation sets:** (a) official ICBHI split (patient-disjoint); (b) leave-one-device-out, patient-disjoint folds (AKGC417L, Meditron, Littmann pooled).
**Main metric:** ICBHI Score = (Sp + Se)/2 with 95% bootstrap CI resampled by patient. **Secondary:** Se, Sp, macro-F1, per-class recall.

**Milestone 2 baselines**

| Model | Protocol | ICBHI Score (95% CI) | Se | Sp | macro-F1 | Seeds | Source file |
|---|---|---|---|---|---|---|---|
| Constant "Normal" (reference) | any | 50.0 (Sp = 100, Se = 0 by definition) | 0 | 100 | — | — | analytic; EDA section 10 |
| Logistic regression (MFCC + delta + spectral) | official | ĐIỀN | ĐIỀN | ĐIỀN | ĐIỀN | ĐIỀN | `results.csv` (ĐIỀN path) |
| SVM | official | ĐIỀN | | | | | |
| Random forest | official | ĐIỀN | | | | | |
| Logistic regression | LODO (per device) | ĐIỀN (one row per held-out device) | | | | | |

**Milestone 3 proposed model** (ĐIỀN tên mô hình, ví dụ CRNN log-Mel với tiền xử lý đã chọn)

| Model | Protocol | ICBHI Score (95% CI) | Se | Sp | macro-F1 | Seeds | Source file |
|---|---|---|---|---|---|---|---|
| CNN | official | ĐIỀN | | | | | |
| CRNN | official | ĐIỀN | | | | | |
| CRNN | LODO | ĐIỀN | | | | | |

### B2. Ablation (Milestone 3)

| Variant | Setting changed | Protocol | ICBHI Score (95% CI) | Δ vs default | Source |
|---|---|---|---|---|---|
| Default | band-pass on, default normalization, sr 4000 Hz, 5.0 s | ĐIỀN | ĐIỀN | — | |
| No band-pass | `--no_bandpass` | ĐIỀN | ĐIỀN | ĐIỀN | |
| Peak normalization | `--norm peak` | ĐIỀN | ĐIỀN | ĐIỀN | |
| COPD-only | `--copd_only` (separates device from diagnosis) | ĐIỀN | ĐIỀN | ĐIỀN | |
| Segment length | ĐIỀN (EDA: median cycle 2.4–2.9 s, Q3 ≈ 3.3–3.6 s) | ĐIỀN | ĐIỀN | ĐIỀN | |

### B3. Comparison with the literature (and why it is not like-for-like)

| Paper | Their result | Our comparable result | Why the comparison is not fair |
|---|---|---|---|
| R6 BTS (Kim et al. 2024) | 63.54 on the official split | ĐIỀN (official split) | Pretrained AST backbone (audio-pretrained) vs our models trained on ICBHI only; they use device metadata; different seeds/runs; our sample rate is 4 kHz. |
| R5 SG-SCL (Kim et al. 2024) | 61.71 ± 1.61 | ĐIỀN | Same issues; uses device labels in the loss. |
| R10 Koo et al. (2026) | Avg OOD 56.50; per device given in A2 | ĐIỀN (LODO, pooled Littmann) | Federated training, CLAP backbone, includes Yunting (SPRSound), removes multi-device patients; our folds, preprocessing and model families differ, so only the trend (near-50 scores on unseen devices) is comparable. |

### B4. Timeline

| Task | Owner | Milestone | Due | Status | Evidence |
|---|---|---|---|---|---|
| Part I–II (problem, literature, gap) | ĐIỀN | M2 | done | Done | slides 1–9 (ĐIỀN link/path) |
| EDA (device, label, split, LODO, spectra) | ĐIỀN | M2 | done | Done (ĐIỀN: output file) | `icbhi_deep_eda.py`, `eda_outputs/` |
| Classical baselines + official split | ĐIỀN | M2 | ĐIỀN | In progress / ĐIỀN | `results.csv` |
| CNN/CRNN + LODO + pooled | ĐIỀN | M3 (week 10) | ĐIỀN | ĐIỀN | |
| Ablation, calibration (AUC, Brier) | ĐIỀN | M4 (week 12) | ĐIỀN | Not yet | |
| Final report + datasheet + checklists | ĐIỀN | Final (week 15) | ĐIỀN | Not yet | |

### B5. Risks, blockers, questions

**Risks**
1. Device and diagnosis are confounded: AKG and Litt3200 patients are all COPD (EDA), so a LODO score cannot be attributed to the device alone. *Mitigation:* AKG ↔ Litt3200 pair and COPD-only control.
2. Small data: 126 patients; Litt3200 and LittC2SE have few recordings and few patients, so CIs are wide and some LODO folds leave few minority-class cycles (e.g., only ≈321 crackles when AKG is held out).
3. Official split details unverified: the Kaggle copy lacks `ICBHI_challenge_train_test.txt`; per-device train/test counts in the literature disagree (Kim et al. vs Koo et al. on Littmann devices).
4. Scores near 50: the constant-Normal reference scores exactly 50, so any LODO score close to 50 means little.
5. Probability outputs and calibration are not yet produced by the training pipeline.

**Questions for the instructor**
1. How are the sections weighted?
2. Our task is four-class sound classification on pre-segmented cycles. Which parts of C4 item 8 (AUC, calibration, Brier) are expected — one-vs-rest per class?
3. Is TRIPOD+AI the expected guideline for a signal-classification model, or STARD-AI?
4. Is the Kaggle copy of ICBHI acceptable if we document version and checksum?

---

## C. Self-assessment (FAIR and reporting guidelines)

### C1. FAIR — data

| Item | Value |
|---|---|
| Source | ICBHI 2017 Respiratory Sound Database (Rocha et al., 2019). Copy used: ĐIỀN (Kaggle dataset link / official page) |
| Version | ĐIỀN |
| Download date | ĐIỀN |
| Checksum of the training archive | ĐIỀN — run `sha256sum respiratory-sound-database.zip` and paste the full hash |
| Licence / data use | KIỂM TRA on the original page and on the copy used; write the exact licence name and link. Do not assume. |
| Patient identifiers | Recording filenames contain a numeric patient ID, location, acquisition mode and device; no names or direct identifiers (KIỂM TRA the diagnosis file). We do not publish any raw audio in the repo. |

**Datasheet (short)**
- *Composition:* 920 recordings, 126 patients, 6,898 respiratory cycles; labels Normal 3,642 / Crackle 1,864 / Wheeze 886 / Both 506; devices AKGC417L (4,346 cycles, 646 rec), Meditron (1,456 / 127), LittC2SE (594 / 87), Litt3200 (502 / 60).
- *Collection:* two research teams, annotated by clinicians (see R1). Device is parsed from the 5th filename field (`patient_rec_location_mode_device`).
- *Known issues found in EDA:* device ↔ diagnosis confounding (Cramér's V ≈ 0.17); only 4/126 patients recorded with more than one device (112, 158, 218, 226; 412 cycles); class imbalance; crackle cycles come mostly from AKG (≈83%).
- *Recommended use:* method development and benchmarking; not for clinical decision-making.

### C2. Reporting guideline

- **Research question:** Do respiratory-abnormality classifiers trained on some stethoscope devices keep their ICBHI Score on an unseen device, and which preprocessing/model choices help?
- **Matching guideline:** TRIPOD+AI (development and evaluation of a prediction/classification model). *Reason:* we develop and internally/externally (by device) evaluate an ML model. STARD-AI would fit if we only measured diagnostic accuracy of a fixed model; DECIDE-AI applies to early clinical deployment, which we do not do.
- **Checklist progress:** ĐIỀN (X of Y TRIPOD+AI items addressed, file path).
- **Definitions**
  - *Outcome / label:* four-class label per respiratory cycle (Normal, Crackle, Wheeze, Both) from ICBHI annotations.
  - *Prediction time T0:* Not applicable — offline classification of recorded, pre-segmented cycles; there is no forecasting horizon.
  - *Variables available at prediction:* the cycle audio segment (and its annotated start/end). Caveat: cycle boundaries are manual annotations, which would not exist in a real deployment.

### C3. PROBAST+AI self-appraisal

| Domain | Risk of bias | Evidence in repo (file : line) | Mitigation / weakness |
|---|---|---|---|
| Participants and data | High | EDA sections 8–10 (device × split, patient–device overlap); `icbhi_deep_eda.py` ĐIỀN line | Device–cohort confound; 126 patients; Kaggle copy without official split file. We report it and add AKG ↔ Litt3200 and COPD-only controls. |
| Predictors | Low–Unclear | `icbhi_train.py` ĐIỀN (feature extraction, band-pass, normalization) | Annotated cycle boundaries are used (not available in deployment). |
| Outcome | Unclear | ICBHI annotations | Label noise not measured; class imbalance handled by ĐIỀN (class weights / sampling). |
| Analysis | Unclear → High | `icbhi_train.py` ĐIỀN: patient-wise validation split from train only; LODO folds with overlapping patients removed; bootstrap CI resampled by patient | No calibration or probabilistic metrics yet; small test sets for Littmann devices; hyper-parameters tuned only on patient-wise validation (ĐIỀN confirm). |

**Applicability:** the results apply to ICBHI-like recordings, adult patients, the four devices; they do not apply to other populations (e.g., paediatric) or to automatic cycle segmentation.

### C4. Quick checklist

| # | Item | Status | Evidence / what is missing |
|---|---|---|---|
| 1 | Datasheet | Met (short version) | Short datasheet in C1 (composition, collection, known issues, recommended use); full version ĐIỀN path |
| 2 | Data version and checksum | Not yet | Need `sha256sum` output (C1) |
| 3 | No identifiers; basis for data use | Not yet | Identifiers: filenames have numeric IDs only; licence still to confirm (C1) |
| 4 | Reporting guideline chosen + checklist completed | Not yet | Guideline chosen (C2); checklist progress ĐIỀN |
| 5 | Outcome, label, T0 defined | Met (T0 = Not applicable) | C2 |
| 6 | Patient-level split; preprocessing fitted on training data only | Met for the split; KIỂM TRA preprocessing | `icbhi_train.py` ĐIỀN line; confirm any scaler/normalization is fitted on train only |
| 7 | Logistic-regression baseline (or justified alternative) | Met if the logreg rows in B1 exist | `results.csv` ĐIỀN |
| 8 | AUC, calibration (curve, in-the-large, slope), Brier, with 95% CI | Not yet | Pipeline outputs labels, not probabilities; planned for M4 |
| 9 | Sample size sufficiency and applicability limits | Partly | B5 risk 2 and C3 applicability; no formal sample-size calculation |

---

## D. AI use declaration

| Tool | Part of the work supported | How the team verified the output |
|---|---|---|
| Claude (Anthropic) | Literature comparison and gap framing; building the Part II slides; drafting the EDA script and the structure of this report; reading and summarizing Koo et al. (2026); reviewing the training script | ĐIỀN — confirm what was really done, e.g., read the source papers, checked numbers against the PDFs, ran the scripts on Colab, compared outputs with the numbers in the slides |
| ĐIỀN (other tools, if any) | ĐIỀN | ĐIỀN |

*Statement:* AI tools were used as listed above; the team is responsible for all numbers and citations, each traceable to a DOI/link or a file in this repository.
