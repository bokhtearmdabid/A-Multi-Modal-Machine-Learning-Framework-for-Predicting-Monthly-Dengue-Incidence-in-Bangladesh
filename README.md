# Dengue Incidence Forecasting in Bangladesh 

A multi-model machine learning and time-series framework for predicting **monthly national dengue incidence in Bangladesh**, using climate and epidemiological data from January 2008 to October 2025.

This repository contains the source code and dataset behind the thesis:

> **"A Multi-Modal Machine Learning Framework for Predicting Monthly Dengue Incidence in Bangladesh: A Comparative Analysis Using Climate and Demographic Variables"**
> Bokhtear Md. Abid, Mukit Hasan, Israt Jahan — Dept. of CSE, University of Asia Pacific (July 2026)
> Supervised by Alif Ruslan · Co-supervised by Tahira Alam

---

## 🔍 Overview

Dengue is one of Bangladesh's most pressing public health challenges, with 2023 and 2025 both producing record-breaking case counts. This project builds and compares **twelve model families** to forecast monthly dengue cases one month ahead, using only climate and case-count data that would realistically be available at prediction time.

Every model is evaluated under a **strictly prospective, leakage-audited protocol**: trained only on 2008–2024 data and tested on the genuinely unseen, record-breaking Jan–Oct 2025 outbreak — not a random or k-fold split.

## 🏆 Headline Result

**SARIMAX(1,0,1)(1,0,1)₁₂** was the best-performing model overall:

| Metric | Value |
|---|---|
| MAE | 1,912 cases |
| RMSE | 3,060 cases |
| R² | 0.752 |
| MAPE | 22.55% |

Tree-based and boosting models (Random Forest, XGBoost) failed structurally on the 2025 outbreak — their predictions are bounded by the maximum case count seen during training, so they could not extrapolate to record-breaking values. Linear/seasonal autoregressive models like SARIMAX have no such ceiling.

## 🧠 Models Implemented

**Classical statistical time-series**
- ARIMAX
- SARIMAX (selected best model)
- Vector Autoregression (VAR)
- Facebook Prophet

**Regularized & kernel regression**
- Ridge Regression
- ElasticNet
- Support Vector Regression (linear & RBF kernel)
- Negative Binomial Regression (GLM)

**Tree ensembles & gradient boosting**
- Random Forest
- XGBoost

**Deep learning**
- Shallow LSTM

## 📊 Dataset

`dengue_climate_monthly.csv` — 214 monthly observations (Jan 2008 – Oct 2025) combining:

| Column | Description |
|---|---|
| `DATE` | First day of the month |
| `MIN` | Monthly minimum temperature (°C) |
| `MAX` | Monthly maximum temperature (°C) |
| `HUMIDITY` | Monthly average relative humidity (%) |
| `RAINFALL` | Monthly total rainfall (mm) |
| `DENGUE` | Monthly reported dengue case count |

All engineered features (lags, rolling means, seasonal encodings) are built from strictly past values only — no same-month or future information is used, ensuring the reported accuracy reflects real forecasting conditions.

## ⚙️ Methodology Highlights

- **Train/test split:** Strict chronological split — train on 2008–2024, test on Jan–Oct 2025 (no shuffling, no k-fold).
- **Walk-forward forecasting:** Statistical models are re-fit/updated using only data through month *t−1* to forecast month *t*.
- **Target transformation:** Log1p transform applied to stabilize variance across low- and high-transmission months.
- **Evaluation metrics:** MAE, RMSE, R², and MAPE, computed on the original case-count scale.


This loads `dengue_climate_monthly.csv`, engineers leakage-safe features, trains all twelve model families on the chronological train/test split, and prints/saves the final comparison table (ranked by RMSE).

## 📄 Citation

If you use this code or dataset, please cite the thesis.

## 🙏 Acknowledgements

This work was completed as an undergraduate thesis at the **Department of Computer Science & Engineering, University of Asia Pacific**, under the supervision of **Alif Ruslan** and co-supervision of **Tahira Alam**.
