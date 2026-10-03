# FIFA 22 Player Rating Prediction

[![Python Version](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![Framework](https://img.shields.io/badge/Machine%20Learning-Scikit--Learn-orange.svg)](https://scikit-learn.org/)
[![Analysis](https://img.shields.io/badge/Data%20Analysis-Pandas%20%7C%20NumPy-green.svg)](https://pandas.pydata.org/)
[![Visualization](https://img.shields.io/badge/Visualization-Matplotlib%20%7C%20Seaborn-lightblue.svg)](https://seaborn.pydata.org/)

An end-to-end machine learning project predicting EA Sports **FIFA 22 Player Overall Ratings (`overall`)** using physical attributes, technical skills, mental ratings, and market valuations.

---

## Project Overview

This project analyzes 19,000+ players across 110 raw attributes from FIFA 22 to build predictive regression models. Using tabular feature engineering, exploratory data analysis, and hyperparameter tuning, the best model predicts player overall ratings within **+-0.31 rating points**.

### Key Results

| Metric | Score | Detail |
| :--- | :--- | :--- |
| **Best Model** | **Gradient Boosting Regressor (Tuned)** | Selected via 5-Fold Cross-Validation |
| **Test R^2** | **0.9952** | Explains 99.52% of rating variance |
| **Test MAE** | **0.314** | Average error under +-0.31 rating points |
| **Test RMSE** | **0.467** | Evaluated on held-out 15% test set |

---

## Dataset & Preprocessing

- **Dataset:** Kaggle FIFA 22 Dataset (`players_22.csv`), containing 19,239 records and 110 features.
- **Feature Selection:** Dropped identifiers and non-predictive attributes (`sofifa_id`, `player_url`, `short_name`, `long_name`, `dob`, `player_tags`, `player_traits`).
- **Feature Engineering:**
  - `bmi`: `weight_kg / (height_cm / 100)^2`
  - `log_value`: `log(1 + value_eur)`
  - `log_wage`: `log(1 + wage_eur)`
- **Preprocessing:** Median imputation for missing values, standard scaling via `StandardScaler`, and a 70% Train / 15% Validation / 15% Test split.

---

## Model Evaluation

| Model | Val MAE | Val RMSE | Val R^2 | 5-Fold CV R^2 |
| :--- | :---: | :---: | :---: | :---: |
| **Gradient Boosting** | **0.372** | **0.543** | **0.9937** | **0.9943** |
| **Random Forest** | 0.317 | 0.545 | 0.9936 | 0.9940 |
| **Decision Tree** | 0.428 | 0.755 | 0.9878 | 0.9868 |
| **Ridge Regression** | 0.788 | 1.066 | 0.9756 | 0.9776 |
| **Linear Regression** | 0.788 | 1.066 | 0.9756 | 0.9776 |

### Hyperparameter Tuning

- **Gradient Boosting (Tuned):** `learning_rate=0.1`, `max_depth=6`, `n_estimators=200` -> **Test R^2: 0.9952**
- **Random Forest (Tuned):** `max_depth=None`, `min_samples_split=2`, `n_estimators=200` -> **Val R^2: 0.9936**

---

## Key Findings

- **Mental Attributes Lead:** `movement_reactions` and `mentality_composure` are the strongest drivers of overall ratings.
- **Tree Ensembles Excel:** Non-linear tree ensembles significantly outperform linear models (R^2 0.995 vs 0.975).

---

## Repository Structure

```text
fifa22-player-rating-prediction/
├── ML_Project_FIFA.ipynb   # Main Jupyter Notebook
├── requirements.txt        # Project dependencies
├── .gitignore              # Git ignore rules
└── README.md               # Project documentation
```

---


