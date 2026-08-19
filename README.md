# 🌧️ Mumbai Monsoon Rainfall Forecasting

## Machine Learning Regression & Mid-Season Forecasting Project

Forecasting Mumbai's final monsoon-season rainfall using rainfall observations available by the end of August.

---

## 📌 Project Overview

Rainfall forecasting based purely on previous years can be difficult because annual rainfall has very weak year-to-year memory.

This project investigates historical Mumbai rainfall data from **1901 to 2021** and reframes the problem as a **mid-season forecasting task**.

Instead of attempting to predict the next year's rainfall using historical totals alone, the model uses rainfall already observed during the current monsoon season — particularly June, July, and August — to forecast the final seasonal rainfall total.

The project includes:

- Exploratory Data Analysis
- Trend and seasonality analysis
- Autocorrelation analysis
- Leakage-safe feature engineering
- Time-series cross-validation
- Chronological train-test split
- Regression model comparison
- Hyperparameter tuning
- Residual diagnostics
- SHAP explainability
- Business-oriented rainfall forecasting insights

---

## 🎯 Objective

The main objective is to build a machine learning model that can forecast the final seasonal rainfall total using information available during the monsoon season.

The project specifically investigates whether rainfall observed through August provides a useful signal for estimating the final seasonal total.

---

## 📊 Dataset

The dataset contains **121 years of Mumbai rainfall records from 1901 to 2021**.

The target variable is:

**Total annual rainfall**

The dataset contains monthly rainfall measurements for:

- January
- February
- March
- April
- May
- June
- July
- August
- September
- October
- November
- December

The project analysis reports:

- **121 years of data**
- **8 engineered features**
- No missing values
- Average annual rainfall of approximately **2,168 mm**
- Approximately **94.8% of annual rainfall occurs during June–September**

---

## 🔍 Exploratory Data Analysis

The analysis investigates:

- Long-term rainfall trends
- Monthly and seasonal rainfall patterns
- Monsoon dominance
- Year-to-year variability
- Autocorrelation
- Progressive within-season forecasting signal
- Historical rainfall baselines

### Important Findings

The lag-1 year autocorrelation is only approximately **0.11**, indicating weak year-to-year rainfall memory.

This means that simply using the previous year's rainfall as a forecast provides very limited predictive information.

In contrast, cumulative rainfall observed from June through August has a correlation of approximately **0.89** with the final seasonal rainfall total.

The correlation strengthens as the monsoon progresses:

- June: **0.33**
- June + July: **0.69**
- June + July + August: **0.89**

This provides the main motivation for the mid-season forecasting approach.

---

## 🛠️ Feature Engineering

The project uses leakage-safe features that only use information available before the forecasting point.

### Lag Features

- Previous year's rainfall (`lag_1`)
- Three-year rolling mean (`roll_mean_3`)

Lag features were created using shifting before rolling operations to prevent future information from entering historical observations.

### Mid-Season Rainfall Features

June, July, and August rainfall are retained as separate observed features.

These represent rainfall information already available by the end of August.

### Historical Baseline

An expanding historical mean of June–August rainfall is calculated using only years before the current year.

### Trend Feature

A normalized year feature captures the long-term rainfall trend identified during EDA.

---

## ⚠️ Preventing Data Leakage

Because this is a forecasting problem, preventing future information from entering the model is critical.

The project uses:

- Chronological train-test splitting
- No random shuffling
- TimeSeriesSplit cross-validation
- Historical expanding averages
- Shifted lag features
- Explicit leakage checks

The final split uses:

- Training period: **1904–2001**
- Test period: **2002–2021**

Five-fold TimeSeriesSplit cross-validation was used for model validation and tuning.

---

## 🤖 Models Compared

Five regression approaches were evaluated:

1. Linear Regression
2. Ridge Regression
3. Random Forest
4. XGBoost
5. LightGBM

Hyperparameter tuning was performed using GridSearchCV with TimeSeriesSplit cross-validation.

The search included:

- Ridge: 10 alpha values
- Random Forest: 36 combinations
- XGBoost: 27 combinations
- LightGBM: 54 combinations

---

## 📈 Model Performance

Models were evaluated using:

- MAE
- RMSE
- R² Score

| Model | Test MAE (mm) | Test RMSE (mm) | Test R² |
|---|---:|---:|---:|
| **Ridge (Tuned)** | **205.4** | **263.6** | **0.781** |
| LightGBM (Tuned) | 338.1 | 419.7 | 0.444 |
| XGBoost (Tuned) | 340.8 | 423.9 | 0.433 |
| Random Forest (Tuned) | 340.5 | 424.6 | 0.431 |
| Naive Baseline | 234.9 | 296.9 | 0.722 |

Ridge Regression achieved the strongest performance across the reported metrics and was the only machine learning model to clearly outperform the naive baseline.

---

## 🏆 Final Model

### Tuned Ridge Regression

The final selected model is **Ridge Regression**.

Performance on the chronological test set:

- **MAE:** 205.4 mm
- **RMSE:** 263.6 mm
- **R²:** 0.781

The model explains approximately **78.1% of the variance** in the held-out rainfall totals.

The result also demonstrates an important modelling lesson: a simpler regularized linear model can outperform more complex tree-based ensemble models when the dataset is small and the underlying relationship is largely additive.

---

## 🧠 SHAP Explainability

SHAP was used to understand which features have the strongest influence on rainfall forecasts.

### Top Forecast Drivers

1. **July Rainfall**
2. **August Rainfall**
3. **June Rainfall**
4. **Year Trend**
5. **Historical June–August Average**
6. **Previous-Year Total**

July rainfall was identified as the strongest feature, followed by August and June rainfall.

The relatively small contribution of the previous year's total is consistent with the weak year-to-year autocorrelation found during EDA.

---

## 💡 Key Insights

### 1. Monsoon dominates Mumbai rainfall

Approximately **94.8% of annual rainfall occurs between June and September**.

### 2. Previous-year rainfall is a weak predictor

Lag-1 autocorrelation is approximately **0.11**, showing limited year-to-year memory.

### 3. Mid-season rainfall provides a strong forecasting signal

Cumulative June–August rainfall has approximately **0.89 correlation** with the final seasonal total.

### 4. Ridge Regression performed best

The tuned Ridge model achieved a test R² of **0.781**, outperforming the other evaluated models.

### 5. July and August are particularly informative

SHAP analysis identified July and August rainfall as the strongest drivers of the final forecast.

---

## 🌊 Potential Business Applications

A reliable end-of-August rainfall forecast could support:

- Reservoir release planning
- Water-supply planning
- Drought preparedness
- Early identification of below-normal rainfall conditions
- Seasonal resource planning

The project also explores translating forecasts into Deficient, Normal, and Excess rainfall categories for easier decision-making.

---

## 📁 Repository Structure

Mumbai-Monsoon-Rainfall-Forecasting/

├── data/
│   ├── README.md
│   └── mumbai-monthly-rains.csv
│
├── docs/
│   ├── README.md
│   └── Mumbai_Rainfall_Forecasting_ShivKumar.pptx
│
├── notebooks/
│   ├── README.md
│   └── Mumbai_Rainfall_Prediction_Project.ipynb
│
├── .gitignore
├── README.md
├── requirements.txt
└── source.txt

---

## 🛠️ Technologies Used

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- XGBoost
- LightGBM
- SHAP
- Jupyter Notebook

---

## ⚠️ Limitations & Future Work

The dataset contains only 121 annual observations, so model stability should be interpreted carefully.

Potential future improvements include:

- Nested cross-validation
- Additional meteorological variables
- ENSO indicators
- Indian Ocean Dipole indices
- Seasonal climate indicators
- Early-season forecasting with shorter lead times
- Classification into rainfall categories

---

## 👤 Author

**Shiv Kumar**

Data Analyst
