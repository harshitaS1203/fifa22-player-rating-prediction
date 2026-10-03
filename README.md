<<<<<<< HEAD
# ⚽ FIFA 22 Player Rating Prediction

[![Python Version](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![Framework](https://img.shields.io/badge/Machine%20Learning-Scikit--Learn-orange.svg)](https://scikit-learn.org/)
[![Analysis](https://img.shields.io/badge/Data%20Analysis-Pandas%20%7C%20NumPy-green.svg)](https://pandas.pydata.org/)
[![Visualization](https://img.shields.io/badge/Visualization-Matplotlib%20%7C%20Seaborn-lightblue.svg)](https://seaborn.pydata.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

An end-to-end Machine Learning project that predicts EA Sports **FIFA 22 Player Overall Ratings (`overall`)** using player physical attributes, technical skills, mental characteristics, and monetary statistics.

---

## 📌 Project Overview

In EA Sports FIFA 22, a player's **Overall Rating** (`overall`) is a critical benchmark used by gamers, scouts, and analysts to evaluate player quality. This project constructs a machine learning pipeline to systematically analyze 19,000+ players across 110 raw attributes, perform feature selection and engineering, compare baseline algorithms, tune hyperparameter grids, and achieve highly accurate player rating predictions.

### 🎯 Key Performance Scorecard (Final Test Set)

| Metric | Score | Interpretation |
| :--- | :--- | :--- |
| **Model** | **Gradient Boosting Regressor (Tuned)** | Best ensemble regressor selected via 5-Fold Cross Validation |
| **Test $R^2$** | **`0.9952`** | Explains **99.52%** of the variance in player ratings |
| **Test MAE** | **`0.314`** | Average prediction error is within **$\pm 0.31$ rating points** |
| **Test RMSE** | **`0.467`** | Root Mean Squared Error on completely unseen test data |

---

## 📊 Dataset & Features

- **Source:** Kaggle — [FIFA 22 Complete Player Dataset](https://www.kaggle.com/datasets/stefanoleone992/fifa-22-complete-player-dataset) (`players_22.csv`)
- **Dimensions:** 19,239 records × 110 features
- **Target Variable:** `overall` (Numerical rating ranging from 47 to 93)

### 🧹 Preprocessing & Feature Engineering

1. **Feature Dropping:** Identifiers, player URLs, free-text tags, and name attributes (`sofifa_id`, `player_url`, `short_name`, `long_name`, `dob`, `player_tags`, `player_traits`) were removed to eliminate memorisation risk and high-cardinality noise.
2. **Feature Creation:**
   - **`bmi`**: Body Mass Index derived from physical statistics ($\text{weight\_kg} / (\text{height\_cm}/100)^2$).
   - **`log_value`**: Log-transformed monetary market value ($\log(1 + \text{value\_eur})$) to normalize heavy right skewness.
   - **`log_wage`**: Log-transformed weekly wage ($\log(1 + \text{wage\_eur})$).
3. **Imputation & Scaling:** Missing numeric values were imputed using median strategy, followed by feature standardization via `StandardScaler`.
4. **Data Partitioning:** 
   - **Train Set (70%):** 13,467 players
   - **Validation Set (15%):** 2,886 players
   - **Test Set (15%):** 2,886 players (Held out strictly for final unbiased evaluation)

---

## 🤖 Model Comparison & Evaluation

Five distinct regression models were evaluated on the validation set and cross-validated across 5 stratified folds:

| Model | Val MAE | Val RMSE | Val $R^2$ | 5-Fold CV $R^2$ | Description / Role |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **Gradient Boosting** | **0.372** | **0.543** | **0.9937** | **0.9943** | Sequential ensemble (Baseline) |
| **Random Forest** | 0.317 | 0.545 | 0.9936 | 0.9940 | Parallel tree ensemble (Baseline) |
| **Decision Tree** | 0.428 | 0.755 | 0.9878 | 0.9868 | Non-linear single tree baseline |
| **Ridge Regression** | 0.788 | 1.066 | 0.9756 | 0.9776 | L2 Regularised linear regression |
| **Linear Regression** | 0.788 | 1.066 | 0.9756 | 0.9776 | Standard linear baseline |

---

## ⚙️ Hyperparameter Tuning

Both ensemble models (Random Forest and Gradient Boosting) were subjected to **GridSearchCV (3-Fold CV)**:

* **Random Forest Grid:**
  - `n_estimators`: `[100, 200]`
  - `max_depth`: `[10, 20, None]`
  - `min_samples_split`: `[2, 5]`
  - *Best Parameters:* `{'max_depth': None, 'min_samples_split': 2, 'n_estimators': 200}` $\rightarrow$ **Val $R^2$: `0.9936`**

* **Gradient Boosting Grid:**
  - `n_estimators`: `[100, 200]`
  - `learning_rate`: `[0.05, 0.1]`
  - `max_depth`: `[4, 5, 6]`
  - *Best Parameters:* `{'learning_rate': 0.1, 'max_depth': 6, 'n_estimators': 200}` $\rightarrow$ **Val $R^2$: `0.9946`**

---

## 🔍 Key Findings & Feature Importance

- **Mental & Reaction Attributes Dominate:** `movement_reactions` and `mentality_composure` emerged as top drivers of overall rating, reflecting FIFA's underlying rating formula which heavily weights cognitive awareness.
- **Economic Indicators:** Market value (`log_value`) exhibits strong correlation with overall rating, serving as a reliable secondary signal.
- **Non-Linear Advantage:** Tree-based ensemble methods outperformed linear baselines by a significant margin ($R^2 > 0.994$ vs $0.975$), confirming complex non-linear feature interactions in player skill profiles.

---

## 📈 Sample Test Set Predictions

| Player | Actual Rating | Predicted Rating | Error |
| :--- | :---: | :---: | :---: |
| R. Tunnicliffe | 66 | 66 | +0.08 |
| M. Hedges | 72 | 72 | -0.14 |
| L. Abubakar | 69 | 68 | -0.55 |
| Pan Ximing | 51 | 50 | -0.82 |
| M. Dávila | 66 | 66 | +0.09 |
| Olasagasti | 66 | 66 | +0.20 |
| C. Dagba | 76 | 75 | -0.86 |

---

## 📁 Repository Structure

```text
fifa22-player-rating-prediction/
├── ML_Project_FIFA.ipynb   # Main Jupyter Notebook containing EDA, ML Pipeline & Tuning
├── requirements.txt        # Python dependencies required to execute project
├── .gitignore              # Ignores byte code, checkpoints, OS files & datasets
├── LICENSE                 # MIT Open Source License
└── README.md               # Project documentation and summary report
```

---

## 🚀 How to Run locally

### 1. Clone the Repository
```bash
git clone https://github.com/your-username/fifa22-player-rating-prediction.git
cd fifa22-player-rating-prediction
```

### 2. Set Up a Virtual Environment (Optional but Recommended)
```bash
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

### 4. Download Dataset & Launch Jupyter Notebook
1. Download `players_22.csv` from [Kaggle FIFA 22 Dataset](https://www.kaggle.com/datasets/stefanoleone992/fifa-22-complete-player-dataset) and place it in the project root directory.
2. Launch Jupyter Notebook:
   ```bash
   jupyter notebook ML_Project_FIFA.ipynb
   ```

---

## 📜 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.
=======
# fifa22-player-rating-prediction
>>>>>>> d69a2f98bed9e491d711d7e4f998b0dadefa6a48
