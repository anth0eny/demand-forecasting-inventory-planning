# Demand Forecasting & Operational Planning

## Overview

This project develops a machine-learning forecasting system for predicting
store-level retail sales using the Rossmann Store Sales dataset.

The objective is to demonstrate how historical demand, promotions, calendar
patterns, and store characteristics can be combined to forecast short-term
sales and translate those predictions into operational demand signals.

The final model achieved a **WAPE of 8.96%** and reduced **Mean Absolute Error
(MAE) by approximately 71.5%** compared with a naive seven-day forecasting
baseline.

---

## Business Problem

Retail businesses need to anticipate future demand to make effective
operational decisions.

Poor demand forecasting can contribute to:

- Stockouts and missed sales opportunities
- Excess inventory
- Inefficient staffing
- Poor promotional preparation
- Inefficient allocation of operational resources

This project builds a store-level sales forecasting system and converts the
resulting predictions into interpretable demand signals that can support
operational planning.

---

## Dataset

The project uses the **Rossmann Store Sales** dataset.

The data contains historical daily sales observations together with
information such as:

- Store
- Sales
- Promotions
- Store operating status
- State and school holidays
- Store type
- Assortment
- Competition information
- Promotional program information

The modeling dataset contains historical observations across more than
1,000 retail stores.

---

## Exploratory Data Analysis

Several important demand patterns were identified.

### Weekly Demand Patterns

Sales vary substantially across the week. Among open stores, Sunday and
Monday recorded the highest average sales, while Saturday recorded the
lowest.

### Promotions

Average sales were approximately:

- **8,228** on promotional days
- **5,929** on non-promotional days

Promotional periods were therefore associated with approximately **38.8%
higher average sales**.

### Store Characteristics

Sales also differed across store types. Store Type B recorded the highest
average sales, although it contained considerably fewer observations than
the other store types.

### Demand Volatility

Individual stores exhibited different levels of sales volatility,
demonstrating the importance of incorporating store-specific historical
behavior into forecasting.

---

## Feature Engineering

Historical sales were transformed into forecasting features including:

### Lag Features

- Sales 1 day earlier
- Sales 7 days earlier
- Sales 14 days earlier
- Sales 28 days earlier

### Rolling Demand Features

- 7-day rolling average
- 14-day rolling average
- 28-day rolling average
- Rolling standard deviation
- Recent minimum and maximum sales

### Calendar Features

- Day of week
- Month
- Week of year
- Quarter
- Cyclical month features
- Cyclical day-of-week features

Calendar-based lag construction was used to preserve the actual time
relationship between observations where historical date gaps existed.

Same-day customer count was excluded from the forecasting predictors because
future customer traffic would not normally be known at prediction time.

---

## Validation Strategy

A chronological train-validation split was used instead of a random split.

**Training period:**  
January 29, 2013 – June 19, 2015

**Validation period:**  
June 20, 2015 – July 31, 2015

The evaluation represents a **rolling one-step-ahead forecasting setting**,
where recently observed sales can be incorporated into lag and rolling
features for subsequent forecasts.

It should not be interpreted as a fixed-origin 42-day-ahead forecast.

---

## Models

Three forecasting approaches were compared:

1. Naive 7-Day Lag
2. Linear Regression
3. HistGradientBoosting Regressor

### Model Performance

| Model | MAE | RMSE | WAPE |
|---|---:|---:|---:|
| Naive 7-Day Lag | 1,887.99 | 2,629.31 | 31.45% |
| Linear Regression | 693.44 | 1,048.44 | 11.55% |
| **HistGradientBoosting** | **538.06** | **852.93** | **8.96%** |

HistGradientBoosting achieved the strongest performance, reducing MAE by
approximately **71.5% relative to the naive baseline**.

---

## Model Interpretation

Permutation importance identified several influential predictors.

The most important included:

1. Store operating status
2. Promotion status
3. 28-day rolling average sales
4. 14-day lagged sales
5. 1-day lagged sales
6. 28-day lagged sales
7. 7-day lagged sales
8. Day of week
9. Recent maximum sales
10. Week of year

The results indicate that forecasting performance depends heavily on a
combination of operating conditions, promotions, recent demand behavior,
historical sales patterns, and calendar effects.

---

## Demand Planning Framework

The final forecasts were translated into an interpretable store-level demand
signal.

Each forecast was compared with the corresponding store's median historical
sales.

The following illustrative rules were used:

| Demand Signal | Rule |
|---|---|
| Closed | Store is not scheduled to operate |
| Low Demand | Forecast < 80% of typical sales |
| Normal Demand | Forecast between 80% and 120% |
| High Demand | Forecast > 120% of typical sales |

### Planning Results

Across the validation period:

- **48.2%** of store-days were classified as Normal Demand
- **31.4%** as High Demand
- **6.4%** as Low Demand
- **14.0%** as Closed

These signals can help operational teams prioritize stores expected to
experience unusually high or low demand.

---

## Business Recommendations

- Prioritize High Demand stores for operational preparation.
- Incorporate planned promotions into forecasting workflows.
- Evaluate demand relative to each store's historical behavior.
- Monitor unusually volatile stores and extreme demand periods.
- Use demand forecasts to support staffing, replenishment readiness,
  promotional preparation, and resource allocation.

---

## Limitations

The Rossmann dataset contains **store-level sales value rather than SKU-level
unit demand**.

Therefore, the model provides store-level demand signals rather than exact
product-order quantities. A true inventory optimization system would require
additional information such as:

- SKU-level unit sales
- Current inventory
- Product prices
- Supplier lead times
- Stockout history
- Safety-stock requirements
- Replenishment costs

The Low, Normal, and High Demand thresholds used in this project are
illustrative business rules rather than optimized inventory policies.

---

## Project Structure

```text
demand-forecasting-inventory-planning/
├── data/
├── models/
│   └── final_demand_forecasting_model.pkl
├── notebook/
│   └── 01_demand_forecasting.ipynb
├── outputs/
│   └── demand_planning_signals.csv
├── src/
├── visualizations/
├── README.md
├── requirements.txt
└── .gitignore
```

---

## Technologies

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- Joblib
- Jupyter Notebook

---

## Key Result

The final HistGradientBoosting model achieved:

**MAE:** 538.06  
**RMSE:** 852.93  
**WAPE:** 8.96%  
**MAE improvement over baseline:** 71.5%

The project demonstrates how machine-learning forecasts can be translated
from raw predictions into interpretable demand signals for retail operational
planning.