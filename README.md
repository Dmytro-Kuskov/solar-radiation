# ☀️ Solar Radiation Modeling Project

## Overview
This project explores the prediction of **solar radiation flux** using reanalysis data (AgERA5), topography (GMTED2010 DEM), and astronomical calculations.  
The workflow includes dataset preparation, feature engineering, and modeling of both linear and non-linear regression models.

---

## Dataset
✅ Final dataset columns:

| Column               | Description                          | Units        | Source             |
|----------------------|--------------------------------------|--------------|--------------------|
| lat, lon             | Coordinates                          | °            | 30k land points    |
| time                 | Random day                           | YYYY-MM-DD   | AgERA5             |
| Solar_Radiation_Flux | Incident solar radiation             | MJ/m²        | AgERA5 (converted) |
| Cloud_Cover          | Cloud fraction                       | 0-1          | AgERA5 (converted) |
| elevation            | Terrain height                       | m            | GMTED2010 DEM      |
| sunlight_hours       | Actual sunlight hours (twilight)     | h            | Astronomical calc  |
| daylength            | Astronomical day length (FAO-56)     | h            | FAO-56 eq.         |
| H0_MJ                | Extraterrestrial radiation           | MJ/m²/day    | FAO-56 eq.         |
| doy_sin, doy_cos     | Seasonal cycle features              | unitless     | Derived            |

---

## Data Preprocessing
- Missing values are **dropped**.  
- Two sets of features are used:
  - **Transformed features** for linear models: `H0_MJ`, `daylength`, `lat_cos`, `lon_cos`, `Cloud_Cover_Mean_24h`, `elevation_log`, `doy_sin`, `doy_cos`  
  - **Raw features** for non-linear models: `H0_MJ`, `daylength`, `lat`, `lon`, `Cloud_Cover_Mean_24h`, `elevation`, `doy_sin`, `doy_cos`

---

## Models

### 1. Linear Models (with transformed features)
- Linear Regression  
- Ridge Regression  
- Lasso Regression  

### 2. Tree-based / Boosting Models (with raw features)
- Random Forest (`n_estimators=200`, `max_depth=20`, `random_state=42`)  
- Gradient Boosting (`n_estimators=200`, `max_depth=5`, `learning_rate=0.1`)  
- XGBoost (`n_estimators=300`, `max_depth=6`, `learning_rate=0.1`, `subsample=0.8`, `colsample_bytree=0.8`)  

---

## Results
## 📊 Cross-Validation Results (Linear Models)

| Model              | R² Mean | R² Std | MAE Mean | MAE Std | RMSE Mean | RMSE Std |
|--------------------|---------|--------|----------|---------|-----------|----------|
| Linear Regression  | 0.8484  | 0.0033 | 2.2716   | 0.0095  | 3.0570    | 0.0300   |
| Ridge Regression   | 0.8484  | 0.0033 | 2.2715   | 0.0095  | 3.0570    | 0.0300   |
| Lasso Regression   | 0.8440  | 0.0039 | 2.2927   | 0.0135  | 3.1014    | 0.0347   |

---

## 📊 Model Comparison (Tree-based & Boosting)

| Model             | R² Test | RMSE Test | CV R² Mean | CV R² Std |
|-------------------|---------|-----------|------------|-----------|
| XGBoost           | 0.9053  | 2.4220    | 0.9036     | 0.0016    |
| Gradient Boosting | 0.9020  | 2.4637    | 0.9005     | 0.0015    |
| Random Forest     | 0.8985  | 2.5072    | 0.8977     | 0.0016    |

---
## Project Structure
```
/solar-radiation/
├── README.md
├── data/
│   ├── raw/         # Raw data (downloading from Google Drive)
│   └── processed/   # Processed data
├── notebooks/
│   ├── 00-download-data.ipynb   # Downloading datasets from Google Drive
│   ├── 01-exploration.ipynb     # Exploratory analysis
│   └── 02-modeling.ipynb        # Models
├── outputs/
│   └── figures/
├── requirements.txt
├── Written Report_ Predicting Solar Radiation Flux by Dmytro Kuskov.pdf
└── .gitignore
```
---
## Notes

- The code automatically downloads the required data files from **Google Drive** during preprocessing.

## Attribution

This project uses data from **AgERA5** (Agrometeorological indicators from reanalysis),  
provided by the **Copernicus Climate Change Service (C3S)** via the [Climate Data Store (CDS)](https://cds.climate.copernicus.eu/datasets/sis-agrometeorological-indicators).  

> Data from Copernicus Climate Change Service (C3S). Neither the European Commission nor ECMWF is responsible for any use that may be made of the Copernicus information or data it contains.

