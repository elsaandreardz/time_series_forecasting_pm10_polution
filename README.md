# Forecasting the (In)visible: A Time Series Analysis of PM10 Pollution in Monterrey, Mexico
This repo is for academic purposes, as it is part of the final project of the Time Series Forecasting class at CEU. 

**Author:** Elsa Andrea Rodríguez, Student ID: 2500008  
**Course:** Time Series Analysis (Final Project)

## Project Overview

This project analyzes and forecasts PM10 particulate matter pollution levels in the Monterrey Metropolitan Area, Mexico, using daily air quality data recorded between 2015 and 2019. PM10 refers to inhalable particles with a diameter of 10 microns or less, small enough to get deep into the lungs and trigger serious adverse health effects.

Monterrey faces a particularly critical air quality challenge, as heavy industry operates in close proximity to densely populated residential neighborhoods, often without sufficient environmental regulation. According to official sources, air pollution in Nuevo León causes approximately 2,500 deaths per year. This analysis aims to identify pollution patterns and produce data-driven forecasts to support evidence-based advocacy and policy intervention.

---

## Repository Structure

```
├── TimeSeries_Final_Project_PM10.ipynb   
├── stations_daily.csv                   
├── README.md
├── requirements.txt                         
```
---

## Dataset Description
- **Source:** [Kaggle — Mexico Hourly Air Pollution (2010–2021)](https://www.kaggle.com/datasets/elianaj/mexico-air-quality-dataset)
- **Original source:** SINAICA (Sistema Nacional de Información de la Calidad del Aire)
- **Records:** 200,000+ hourly readings across all Mexican monitoring stations
- **Variable analyzed:** PM10 (µg/m³) — particulate matter ≤ 10 microns in diameter

### Filtering applied
| Step | Description |
|------|-------------|
| Station filter | Retained only the 15 stations within the Monterrey Metropolitan Area |
| Year filter | Restricted to 2015–2019 |
| PM10 range | Values constrained to 0–250 µg/m³ to remove outliers |
| Granularity | Aggregated to daily frequency (one record per day) |
| Missing values | Removed via `dropna()` |

**Monterrey stations included:** 139, 140, 141, 142, 143, 144, 145, 146, 147, 148, 424, 425, 426, 446, 479  
*(Source: [SINAICA — INECC](https://sinaica.inecc.gob.mx/))*

---

## Analysis Pipeline

### 1. Exploratory Data Analysis (EDA)
- Time series plot of PM10 over the full period
- Monthly, weekly, and daily average patterns
- Weekday vs. weekend pollution comparison
- Distribution and descriptive statistics

### 2. Seasonal Decomposition
- Additive decomposition into trend, seasonality, and residual components
- Confirmed presence of a recurring **weekly seasonal pattern**
- Trend shows cyclical behavior, no sustained improvement over the 2015–2019 period
- Large residual spikes identify extreme pollution events

### 3. Stationarity Testing: ADF Test
- Augmented Dickey-Fuller (ADF) test confirmed the series is **stationary**
- Justified use of ARIMA and SARIMA without additional differencing

### 4. Feature Engineering
Three categories of features were constructed for the machine learning models:

| Category | Features |
|----------|----------|
| **Calendar** | Quarter, Is_weekend |
| **Lag** | lag_1, lag_7, lag_30 |
| **Rolling statistics** | rolling_mean_7, rolling_mean_30, rolling_std_7, rolling_std_30, rolling_max_7, rolling_min_7 |

### 5. Modelling: 5 Models Compared

| Model | Description |
|-------|-------------|
| ARIMA  | Classical univariate time series model |
| SARIMA  | Seasonal extension of ARIMA with weekly period |
| SARIMA + Exog Features | SARIMA with calendar-based exogenous variables |
| XGBoost, Basic Lags | Gradient boosting with lag and rolling mean features |
| XGBoost, Full Features | Gradient boosting with all engineered features |

---

## Requirements (also mentioend in txt file)

```bash
pip install numpy pandas matplotlib seaborn statsmodels scikit-learn xgboost ydata-profiling
```

### Libraries used
| Library | Purpose |
|---------|---------|
| `pandas` | Data manipulation and time series handling |
| `numpy` | Numerical operations |
| `matplotlib` / `seaborn` | Data visualization |
| `statsmodels` | ARIMA, SARIMA, ADF test, seasonal decomposition |
| `scikit-learn` | Error metrics (MAE, RMSE, MAPE) |
| `xgboost` | XGBoost regression models |
| `ydata-profiling` | Automated EDA profiling report |

### Python version
```
Python 3.12.4
```

---

## How to Run

1. Clone or download this repository
2. Install the required libraries:
   ```bash
   pip install -r requirements.txt
   ```
3. Place `stations_daily.csv` in the same directory as the notebook
4. Open `TimeSeries_Final_Project_PM10.ipynb` in Jupyter Notebook, JupyterLab, or VS Code
5. Run all cells sequentially from top to bottom

---

## Key Conclusions
- PM10 levels in Monterrey consistently exceed WHO safe thresholds throughout the 2015–2019 period
- A clear weekly seasonal pattern is present.
- Machine learning models (XGBoost) significantly outperform classical statistical models (ARIMA/SARIMA) for this dataset
- Without regulatory intervention, forecasts suggest pollution levels will continue to pose a serious public health risk to the population of Monterrey and Nuevo León
---

## References
- SINAICA — Sistema Nacional de Información de la Calidad del Aire: https://sinaica.inecc.gob.mx/
- Kaggle Dataset: https://www.kaggle.com/datasets/elianaj/mexico-air-quality-dataset
- Milenio — Air pollution in Nuevo León causes 2,500 deaths per year (Secretaría de Medio Ambiente)
- El Universal — Monterrey recorded 188 days above healthy PM10 levels in 2019
---
