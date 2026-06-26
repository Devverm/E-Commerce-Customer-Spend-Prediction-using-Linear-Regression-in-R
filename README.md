# E-Commerce Customer Spend Prediction using Linear Regression in R

![R](https://img.shields.io/badge/R-Statistical%20Computing-276DC3?logo=r)
![ggplot2](https://img.shields.io/badge/ggplot2-Visualization-orange)
![Status](https://img.shields.io/badge/Status-Complete-success)

An end-to-end regression analysis in R that predicts how much an e-commerce customer will spend yearly, based on their app usage, website behaviour, and membership duration. The project progresses from exploratory data analysis through simple linear regression to a multiple regression model — with full residuals diagnostics and model evaluation.

---

## Table of Contents

- [Business Problem](#business-problem)
- [Key Results](#key-results)
- [Key Insights](#key-insights)
- [Dataset](#dataset)
- [Project Structure](#project-structure)
- [Workflow](#workflow)
- [Tech Stack](#tech-stack)
- [Installation & Setup](#installation--setup)
- [Usage](#usage)
- [Future Improvements](#future-improvements)

---

## Business Problem

An e-commerce company wants to understand which customer behaviours drive annual spend, and how accurately it can be predicted. Specifically:

- Does time spent on the **app** or **website** matter more?
- How important is **membership duration** to revenue?
- Can we build a reliable predictor of **Yearly Amount Spent** to support targeted marketing and retention strategies?

---

## Key Results

| Model | R² | RSE ($) |
|---|---|---|
| Simple Linear Regression (Length of Membership only) | 0.65 | 47.14 |
| **Multiple Linear Regression (all 4 features)** | **0.98** | **9.97** |

Adding three more behavioural features drove R² from **0.65 to 0.98** and reduced prediction error from **$47 to under $10** per customer.

---

## Key Insights

### EDA — What Correlates with Yearly Spend?

A pairplot of all continuous variables revealed one clear standout: **Length of Membership** is the strongest correlate with `Yearly.Amount.Spent` — even before modelling, the scatter plot shows a clean positive linear trend.

`Time on App` also shows meaningful positive correlation with spend, while `Time on Website` appears largely flat and uncorrelated.

### Simple Linear Regression — Length of Membership

```
Yearly.Amount.Spent = intercept + 64.22 × Length.of.Membership
```

| Metric | Value |
|---|---|
| β₁ (coefficient) | **64.22** |
| Interpretation | Every additional year of membership adds ~$64 to annual spend |
| p-value | Significant (below significance level) |
| R² | 0.65 |
| RSE | $47.14 |

**Residuals Diagnostics (Shapiro-Wilk Test):**
- p-value > 0.05 → normality of residuals cannot be rejected ✓
- Linear model assumption holds — the model is correctly specified

### Multiple Linear Regression — All 4 Features

**Coefficient Rankings (most → least impactful):**

| Rank | Feature | Relative Impact | Business Meaning |
|---|---|---|---|
| 1 | Length of Membership | **Highest** | Loyal customers spend significantly more |
| 2 | Time on App | ~1.5× lower than membership | App engagement directly drives revenue |
| 3 | Avg Session Length | ~2.4× lower than membership | Session quality matters, but less so |
| 4 | Time on Website | **Minimal / Non-significant** | Website time has little effect on spend |

**Key finding:** `Time on Website` is not a meaningful predictor. The company should focus on the **mobile app experience** and **long-term membership retention** rather than website engagement.

### Model Comparison

| Metric | Simple LR | Multiple LR |
|---|---|---|
| R² | 0.65 | **0.98** |
| RSE | $47.14 | **$9.97** |
| Features used | 1 | 4 |

The multiple regression model explains **98% of variance** in yearly spend with under $10 prediction error — strong accuracy for a revenue predictor.

### Business Recommendations

1. **Invest in the mobile app** — Time on App is the second strongest revenue driver; improving app UX directly boosts spend
2. **Prioritise membership retention** — Length of Membership is the #1 predictor; every additional year retained adds ~$64 in annual revenue per customer
3. **Don't over-invest in the website** — Time on Website shows near-zero impact; redirect those resources elsewhere
4. **Target new members early** — since membership duration drives spend, reducing early churn maximises lifetime value

---

## Dataset

**File:** `data/customer-data`
**Source:** Kaggle — [Ecommerce Customers Dataset](https://www.kaggle.com/datasets/srolka/ecommerce-customers)

| Property | Details |
|---|---|
| Domain | E-commerce |
| Target variable | `Yearly.Amount.Spent` (continuous, USD) |
| Split | 80% train / 20% test (`set.seed(1)`) |

### Features

| Feature | Type | Description |
|---|---|---|
| `Avg..Session.Length` | Numeric | Average in-store style advice session length (mins) |
| `Time.on.App` | Numeric | Time spent on mobile app (mins) |
| `Time.on.Website` | Numeric | Time spent on website (mins) |
| `Length.of.Membership` | Numeric | Years as a member |
| `Yearly.Amount.Spent` | Numeric | **Target** — annual spend in USD |

---

## Project Structure

```
ecommerce-r-project/
├── main.R                        # Full analysis script
├── ecommerce-r-project.Rproj     # RStudio project file
├── data/
│   └── customer-data             # Raw dataset (CSV)
└── README.md
```

---

## Workflow

### 1. Data Loading & Exploration
- Loaded CSV with `read.csv()`
- Inspected structure via `str()` and `summary()`

### 2. Exploratory Data Analysis
- Scatter plots of `Time.on.Website` and `Avg.Session.Length` vs `Yearly.Amount.Spent`
- Pairplot of all continuous variables — identified `Length.of.Membership` as the strongest correlate
- Distribution analysis via histogram and boxplot (base R + ggplot2)

### 3. Simple Linear Regression
- Fitted `Yearly.Amount.Spent ~ Length.of.Membership`
- Interpreted coefficient (β₁ = 64.22, positive and significant)
- Plotted regression line over scatter

### 4. Residuals Diagnostics
- QQ-plot for visual normality check
- Shapiro-Wilk test: p > 0.05 → residuals are normally distributed ✓

### 5. Model Evaluation — Simple LR
- 80/20 train-test split (`set.seed(1)`)
- Computed RMSE, MAPE, R² on test set → R² = 0.65, RSE = $47.14

### 6. Multiple Linear Regression
- Fitted all 4 features: `Avg.Session.Length`, `Time.on.App`, `Time.on.Website`, `Length.of.Membership`
- `Time.on.Website` identified as non-significant
- Coefficient ranking: Membership > App > Session Length >> Website

### 7. Model Evaluation — Multiple LR
- Same 80/20 split and seed
- R² = **0.98**, RSE = **$9.97** — a dramatic improvement over the simple model

---

## Tech Stack

| Tool | Purpose |
|---|---|
| R (base) | Data loading, modelling, residuals analysis |
| `ggplot2` | Visualizations (scatter, histogram, boxplot) |
| `stats` (base R) | `lm()`, `shapiro.test()`, `predict()` |

---

## Installation & Setup

### Prerequisites
- R ≥ 4.0
- RStudio (recommended)

### Steps

```r
# 1. Clone the repository
# git clone https://github.com/<your-username>/ecommerce-r-project.git

# 2. Open ecommerce-r-project.Rproj in RStudio

# 3. Install required package
install.packages("ggplot2")

# 4. Download the dataset from Kaggle and place in data/ folder:
# https://www.kaggle.com/datasets/srolka/ecommerce-customers
# Rename the file to: customer-data
```

---

## Usage

Open `main.R` in RStudio and run section by section, or all at once:

```r
source("main.R")
```

The script is divided into clearly labelled sections:

| Section | What it does |
|---|---|
| Import Data | Load and inspect the dataset |
| Create Plots | Scatter plots and pairplot for EDA |
| Explore Variables | Histogram and boxplot of key predictor |
| Simple Linear Model | Fit and interpret single-variable regression |
| Residuals Analysis | QQ-plot and Shapiro-Wilk test |
| Evaluate Simple Model | RMSE, MAPE, R² on test set |
| Multiple Regression | Fit and interpret 4-variable model |
| Evaluate Multiple Model | Compare performance vs simple model |

---

## Future Improvements

- [ ] Stepwise feature selection with `step()` for automated variable selection
- [ ] Test polynomial and interaction terms for non-linear relationships
- [ ] Include customer demographic features (age, location, gender)
- [ ] K-fold cross-validation instead of a single 80/20 split
- [ ] Build a Shiny app for interactive spend prediction
- [ ] Export model as `.rds` file for deployment

---

## License

This project is for educational purposes. Dataset sourced from Kaggle.
