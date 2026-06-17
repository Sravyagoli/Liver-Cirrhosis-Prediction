# 🫀 Liver Cirrhosis Prediction

A machine learning project that predicts patient outcomes (Censored, Censored-Liver Transplant, or Deceased) based on clinical biomarkers collected during the Mayo Clinic Primary Biliary Cholangitis (PBC) randomized trial.

---

## 📋 Table of Contents
- [Overview](#overview)
- [Dataset](#dataset)
- [Project Workflow](#project-workflow)
- [Models Used](#models-used)
- [Results](#results)
- [Technologies Used](#technologies-used)
- [How to Run](#how-to-run)
- [Project Structure](#project-structure)

---

## Overview

This project tackles a **multi-class classification problem**: given a set of clinical features, predict a cirrhosis patient's outcome — Censored (`C`), Censored-Liver Transplant (`CL`), or Deceased (`D`). It walks through the complete data science pipeline — from data cleaning and exploratory analysis to model training and evaluation — comparing four classification algorithms to find the best performer.

---

## Dataset

- **Source:** `cirrhosis.csv` (Mayo Clinic Primary Biliary Cholangitis Trial dataset)
- **Target variable:** `Status` (C = Censored, CL = Censored-Liver Transplant, D = Deceased)

**Features used:**

| Feature | Description |
|---|---|
| `N_Days` | Number of days between registration and the earlier of death, transplantation, or study analysis |
| `Drug` | Type of drug administered (D-penicillamine or Placebo) |
| `Age` | Patient age in days |
| `Sex` | Male / Female |
| `Ascites` | Presence of ascites (Y/N) |
| `Hepatomegaly` | Presence of hepatomegaly (Y/N) |
| `Spiders` | Presence of spider angioma (Y/N) |
| `Edema` | Edema condition (N / S / Y) |
| `Bilirubin` | Serum bilirubin level (mg/dl) |
| `Cholesterol` | Serum cholesterol level (mg/dl) |
| `Albumin` | Albumin level (gm/dl) |
| `Copper` | Urine copper level (ug/day) |
| `Alk_Phos` | Alkaline phosphatase level (U/liter) |
| `SGOT` | SGOT enzyme level (U/ml) |
| `Tryglicerides` | Triglyceride level (mg/dl) |
| `Platelets` | Platelet count per cubic ml / 1000 |
| `Prothrombin` | Prothrombin time (seconds) |
| `Stage` | Histologic stage of disease (1–4) |

---

## Project Workflow

1. **Data Loading** — Load the CSV dataset using pandas.
2. **Exploratory Data Analysis (EDA)**
   - Inspect shape, data types, and missing values.
   - Visualize target class distribution (Status).
   - Plot feature correlation heatmap.
   - Analyze key biomarker distributions and boxplots by patient status.
   - Explore categorical features vs. target using crosstab bar charts.
3. **Data Preprocessing**
   - Drop the non-informative `ID` column.
   - **Target encoding:** C → 0, CL → 1, D → 2.
   - **Missing value imputation:** median for numerical columns, mode for categorical columns.
   - **Outlier capping:** IQR-based clipping (1.5× IQR rule) applied to all numerical features.
   - **Label Encoding** of categorical columns: `Drug`, `Sex`, `Ascites`, `Hepatomegaly`, `Spiders`, `Edema`.
   - **Feature Scaling:** StandardScaler applied to all features.
4. **Feature/Target Split** — Features (`X`) and target (`y`) extracted from the dataset.
5. **Stratified K-Fold Cross-Validation** — 5-fold stratified split to handle class imbalance.
6. **Model Training & Evaluation** — Four classifiers trained and compared.

---

## Models Used

| Model | Library | Key Parameters |
|---|---|---|
| Decision Tree | `sklearn.tree` | `max_depth=8`, `class_weight='balanced'` |
| Random Forest | `sklearn.ensemble` | `n_estimators=200`, `max_depth=8`, `class_weight='balanced'` |
| Gradient Boosting | `sklearn.ensemble` | `n_estimators=200`, `max_depth=3`, `learning_rate=0.1` |
| XGBoost | `xgboost` | `n_estimators=200`, `max_depth=6`, `learning_rate=0.1` |

Each model is evaluated using:
- **Stratified 5-Fold Cross-Validation** (accuracy per fold + mean ± std)
- **Confusion Matrix** (visualized with color map)
- **Classification Report** (precision, recall, F1-score per class)
- **Feature Importance** (horizontal bar chart)

---

## Results

All four classifiers were trained and evaluated using stratified 5-fold cross-validation, then re-evaluated on a held-out test fold. **Gradient Boosting** achieved the best test accuracy, narrowly ahead of Random Forest and XGBoost. All three ensemble methods substantially outperformed the single Decision Tree. All models struggle most with the minority class (CL — only 25 samples) due to significant class imbalance.

| Model | CV Mean Accuracy | Test Accuracy |
|---|---|---|
| Decision Tree | 63.86% ± 2.85% | 59.04% |
| Random Forest | 75.35% ± 4.51% | 67.47% |
| **Gradient Boosting** | 74.88% ± 3.93% | **69.88%** |
| XGBoost | 75.36% ± 5.40% | 67.47% |

**Top predictive biomarkers** (consistent across models): **Bilirubin**, **Age**, and **Prothrombin time** were the strongest predictors of patient outcome across nearly all four models, with `N_Days` (time since registration) also ranking highly.

---

## Technologies Used

- **Python 3**
- **NumPy** — Numerical operations
- **Pandas** — Data loading and manipulation
- **Matplotlib / Seaborn** — Data visualization
- **Scikit-learn** — Preprocessing, cross-validation, Decision Tree, Random Forest, Gradient Boosting, and evaluation metrics
- **XGBoost** — Gradient boosted classifier (separate library implementation)

---

## How to Run

1. **Clone the repository**
```
git clone https://github.com/Sravyagoli/Liver-Cirrhosis-Prediction-.git
cd Liver-Cirrhosis-Prediction-
```

2. **Install dependencies**
```
pip install numpy pandas matplotlib seaborn scikit-learn xgboost
```

3. **Add the dataset** — Place `cirrhosis.csv` in the same directory as the notebook (or update the file path inside the notebook).

4. **Run the notebook** — Open `Liver_Cirrhosis_Prediction.ipynb` in Jupyter Notebook or Google Colab and run all cells.

---

## Project Structure

```
Liver-Cirrhosis-Prediction-/
│
├── Liver_Cirrhosis_Prediction.ipynb   # Main notebook
├── cirrhosis.csv                      # Dataset
└── README.md                          # Project documentation
```

---

## 📌 Notes

- The dataset can be downloaded from [Kaggle](https://www.kaggle.com/datasets/fedesoriano/cirrhosis-prediction-dataset/data).
- Missing values are handled by median/mode imputation; future improvements could explore advanced imputation strategies like KNN imputation.
- The dataset is class-imbalanced (232 Censored vs 25 Censored-Liver Transplant vs 161 Deceased); future work could explore SMOTE or other oversampling techniques to improve minority class prediction.
