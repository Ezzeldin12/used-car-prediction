# 🚗 Used Car Price Prediction

> Predicting used car listing prices from Craigslist data using 5 regression models — Linear Regression, LASSO, Ridge, GAM, and an Ensemble — with full EDA, preprocessing, and model evaluation in R.

![R](https://img.shields.io/badge/R-276DC3?style=for-the-badge&logo=r&logoColor=white)
![RMarkdown](https://img.shields.io/badge/RMarkdown-009ABC?style=for-the-badge&logo=r&logoColor=white)
![caret](https://img.shields.io/badge/caret-FF6B35?style=for-the-badge)
![glmnet](https://img.shields.io/badge/glmnet-7B2FF7?style=for-the-badge)

---

## 📌 Overview

This project builds and compares 5 regression models that predict used car prices from Craigslist listings. The full pipeline — data cleaning, imputation, EDA, feature engineering, modeling, and evaluation — is implemented in a single **R Markdown notebook** (`finaaaal1.Rmd`), producing a reproducible, documented analysis with code, output, and visualizations integrated.

---

## 🗂️ Dataset

| Property | Value |
|----------|-------|
| Source | Craigslist used car listings |
| Raw file | `v1.csv` (~1.4 GB) |
| Cleaned file | `imputed_data.csv` (~69 MB, after preprocessing) |
| Target variable | `price` (USD listing price) |
| Original columns | 26 |

### Features Used (after dropping irrelevant columns)

| Feature | Type | Description |
|---------|------|-------------|
| `year` | Numeric | Model year |
| `manufacturer` | Categorical | Car brand (top 5 kept) |
| `condition` | Categorical | Vehicle condition |
| `cylinders` | Categorical | Engine cylinders (top 3 kept) |
| `fuel` | Categorical | Fuel type |
| `odometer` | Numeric | Mileage |
| `title_status` | Categorical | Title condition |
| `transmission` | Categorical | Manual / automatic |
| `drive` | Categorical | FWD / RWD / 4WD |
| `type` | Categorical | Vehicle type (top 4 kept) |
| `paint_color` | Categorical | Exterior color (top 4 kept) |
| `lat` / `long` | Numeric | Listing location |

**Dropped columns**: `county`, `description`, `region_url`, `url`, `image_url`, `size`, `VIN`, `id`, `region`, `posting_date`, `model`, `state`

---

## 🔧 Preprocessing Pipeline

```
Raw v1.csv
    │
    ├── Drop irrelevant / high-missing columns
    ├── Convert character columns → factors
    ├── Remove exact duplicates
    ├── Hot-deck imputation (VIM) for missing values
    ├── Lump rare categories (top N per feature)
    │
    ├── Train / Validation / Test split  →  90% / 5% / 5%
    │
    └── IQR outlier removal on training set
            (price, year, odometer, lat, long)
```

---

## 📊 Exploratory Data Analysis

| Analysis | What it shows |
|----------|--------------|
| Missing value heatmap | % missing per column before and after cleaning |
| Car type distribution | Bar chart of SUV, sedan, truck, pickup counts |
| Price histogram | Distribution of prices under $50,000 |
| Geographic scatter | Listing locations by latitude / longitude |
| Listings by model year | Line chart of listing volume per year |
| Correlation heatmap | Pearson correlations among numeric features |
| ANOVA | Price differences across categorical groups |
| VIF analysis | Multicollinearity check on regression features |

---

## 🤖 Models

All models use **5-fold cross-validation** on the training set and are evaluated on both validation and test sets.

| Model | Package | Key Details |
|-------|---------|-------------|
| Linear Regression (OLS) | `caret` / `lm` | Baseline model; stepwise forward feature selection |
| LASSO | `glmnet` | L1 regularization (α = 1); λ tuned over 100 values |
| Ridge | `glmnet` | L2 regularization (α = 0); λ tuned over 100 values |
| GAM | `mgcv` | Smooth spline terms for `year`, `odometer`, `lat`, `long` |
| Ensemble | `caretEnsemble` | `caretStack` combining LM + LASSO + Ridge with a meta-learner LM |

---

## 📈 Evaluation Metrics

All metrics are computed on both the **Validation set** and the **Test set**:

| Metric | Description |
|--------|-------------|
| **MAE** | Mean Absolute Error — average dollar difference |
| **RMSE** | Root Mean Squared Error — penalizes large errors more |
| **R²** | Coefficient of determination — variance explained |
| **MAPE** | Mean Absolute Percentage Error — % error relative to actual price |

---

## ⚙️ How to Run

**Requirements:** R ≥ 4.0 and RStudio (recommended)

```r
# 1. Install pacman (handles all other package installs automatically)
install.packages("pacman")

# 2. Open finaaaal1.Rmd in RStudio

# 3. Update dataset paths to point to your local files:
#    - Line 29:  path to v1.csv
#    - Line 209: path to imputed_data.csv

# 4. Click "Knit" to run the full analysis, or run chunks individually
```

**Packages installed automatically via `pacman`:**
`tidyverse`, `skimr`, `GGally`, `janitor`, `naniar`, `VIM`, `caret`, `ggplot2`, `car`, `forcats`, `corrplot`, `glmnet`, `mgcv`, `caretEnsemble`, `fastDummies`

---

## 📁 Project Structure

```
used_car_prediction/
└── finaaaal1.Rmd    # Complete analysis: EDA → preprocessing → modeling → evaluation
```

**Dataset files** (stored separately due to size):
```
predictive project/
├── v1.csv            # Raw Craigslist dataset (~1.4 GB)
└── imputed_data.csv  # Cleaned & imputed dataset (~69 MB)
```
