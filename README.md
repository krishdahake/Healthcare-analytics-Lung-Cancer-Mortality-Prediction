# Multimodal Lung Cancer Mortality Prediction

Clinical + CT-derived feature fusion with explainable machine learning on the NSCLC-Radiomics (Lung1) cohort.

**Authors:** Krish Dahake (PRN 23070521076), Ansh Parate (PRN 23070521017)
**Project:** CA3 Mini Project

## Overview
A reproducible pipeline that predicts patient mortality (`deadstatus.event`) from 422 NSCLC patients. It fuses clinical variables (age, T/N/M stage) with three CT-style tumour descriptors (volume, sphericity, entropy), benchmarks five classifiers, and explains the results with Gini and permutation importance.

> **Note:** Raw CT scans were not used. The three radiomic features are **synthetic proxies** generated from T stage plus noise (seed 42). The project validates the fusion, evaluation and explainability pipeline, not the clinical value of imaging.

## Dataset
| File | Description |
|---|---|
| `NSCLC-Radiomics-Lung1_clinical-version3-Oct-2019.csv` | Lung1 clinical data (422 patients) |
| `Lung3_metadata.xls` | Lung3 gene-expression metadata (89 samples), exploratory use only |

Source: The Cancer Imaging Archive (TCIA), NSCLC-Radiomics collection.

## Methods
- **Preprocessing:** median / "Unknown" imputation, encoding, scaling, train-only fitting (no leakage); `Survival.time` is dropped.
- **Fusion:** early (feature-level) concatenation of 7 features.
- **Models:** Logistic Regression, SVM (RBF), Random Forest, HistGradientBoosting, MLP, all with balanced class weights.
- **Evaluation:** stratified 80/20 holdout plus 5x5 repeated CV; ROC-AUC, MCC, balanced accuracy, F1.
- **Explainability:** Gini and permutation importance.

## Key Results
- Holdout accuracy reaches 0.87, but this matches the 88% majority-class rate.
- ROC-AUC is 0.43 to 0.63 across all models, and MCC is about 0.
- Only age and N stage show a positive held-out permutation importance.
- Weighted LR finds 58% of survivors, versus 0% unweighted.

## Setup and Run
```bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
pip install pandas numpy scikit-learn matplotlib scipy xlrd openpyxl
jupyter notebook Lung_Cancer_Analytics.ipynb
```
Or open the notebook in Google Colab and upload the data files.

## Repository Structure
```
├── Lung_Cancer_Analytics.ipynb   # full pipeline
├── NSCLC-Radiomics-Lung1_clinical-version3-Oct-2019.csv
├── Lung3_metadata.xls
├── report/                       # IEEE report (.docx)
└── presentation/                 # project slides (.pptx)
```

## Future Work
Real PyRadiomics features from Lung1 CT, time-to-event (Cox) models, tuning and calibration, and external validation.

## References
Aerts et al., *Nat. Commun.*, 2014 (Lung1 / radiomics); full reference list is in the IEEE report.
