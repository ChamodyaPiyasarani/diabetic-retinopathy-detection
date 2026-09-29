# Diabetic Retinopathy Stage Detection

Five-grade classification of retinal fundus photographs using ResNet50 transfer
learning, with a separate referral output for clinical screening.

**Author:** H. A. C. P. Ranasinghe · Coventry index 16110083 · NIBM CoBscComp24.2-030
**Module:** Computer Vision — Coursework 1
**Video:** <paste your unlisted YouTube link here>

## Results

| Metric | Value |
|---|---|
| Test accuracy | 0.7341 (95% CI 0.696–0.771) |
| Quadratic weighted kappa | 0.7783 (95% CI 0.735–0.819) |
| Majority-class baseline | 0.5106 |
| Referable disease ROC AUC | 0.958 |
| Referral sensitivity / precision | 0.877 / 0.840 |

Evaluated once on 519 held-out test images.

## Dataset

APTOS 2019 Blindness Detection (competition version), Asia Pacific
Tele-Ophthalmology Society, Kaggle, 2019.
https://www.kaggle.com/competitions/aptos2019-blindness-detection

3,662 labelled images graded 0–4 on the International Clinical Diabetic
Retinopathy Severity Scale. The unlabelled competition test set is not used.
After near-duplicate removal, 3,455 images remain.

**The dataset is not in this repository.** Download it from Kaggle; the
notebook locates it automatically.

## Reproducing

```
pip install -r requirements.txt
```

Open `notebooks/dr_detection.ipynb` and run the cells in order. The random seed
is fixed at 42. The data partition is saved to `results/splits/` and reloaded by
every later stage, so the split never changes between runs.

Download the trained model from the Releases page to skip training.

## Where each result comes from

| Report item | Notebook cell |
|---|---|
| Figure 1 — sample images | c3 |
| Figure 2 — class distribution | c4 |
| Table 3 — image resolutions | c5 |
| Duplicate detection | c6 |
| Figure 4 — near-duplicate examples | c7 |
| Figure 5 — split and leakage check | c8 |
| Table 6 — class weights | c19 |
| Figure 11 / Table 8 — architecture comparison | c26 |
| Table 9 — hyperparameter search | c27 |
| Run-to-run variability | c27b |
| Model selection and verification | c28 |
| Table 12, Figures 13–15 — evaluation | c30–c32 |
| Figure 14 — ROC, referable disease | c33 |
| Figure 18 — prototype | c35–c37 |

## Method notes

Near-duplicate detection uses 16×16 difference hashing (256-bit) with a Hamming
threshold of 8, grouped by connected components. It found 139 duplicate groups
covering 309 images, of which **37 groups carry conflicting clinician labels**.
Groups with conflicting labels were removed entirely; from the rest one image
was kept. This removed 207 images before partitioning, so the reported accuracy
is conservative with respect to train/test leakage.

Run-to-run variability was measured at 4.63 percentage points across three
seeds, which exceeds the differences the hyperparameter search was intended to
detect. The selected configuration is therefore reported as reasonable rather
than optimal.

## Licence

Coursework submission. The APTOS 2019 dataset remains subject to its own
competition rules.
