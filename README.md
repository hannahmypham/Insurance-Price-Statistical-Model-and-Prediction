# Insurance Price Prediction

> Regression analysis to predict individual medical insurance charges, achieving R² = 0.86 through feature engineering and interaction term discovery.

![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)
![statsmodels](https://img.shields.io/badge/statsmodels-4EABE6?style=for-the-badge)
![scikit-learn](https://img.shields.io/badge/scikit--learn-%23F7931E.svg?style=for-the-badge&logo=scikit-learn&logoColor=white)

---

## Overview

An end-to-end regression study predicting individual medical insurance charges using demographic and health data. The project rigorously tests OLS assumptions, engineers features from domain knowledge, and discovers that an **obesity-smoking interaction term** captures a +$19,740 cost impact absent in all additive models.

**Key Results:**
 🎯 **R² = 0.86, Adjusted R² = 0.86** on final model (M6)
 
 💡 **obese x smoker interaction** captures **+$19,740 cost impact** not found in baseline models
 
 📊 Evaluated **7 candidate regression models** with full OLS diagnostics
 
 ✅ Course project for DSC 241: Statistical Models at UC San Diego

---

## Team & Contributions

**Hannah Pham:** Problem statement, introduction, conclusions, candidate model design (M2–M7), model comparison, interpretation of interaction effects and study limitations.

**Edith Gu:** Data description, EDA (summary statistics, visualizations, correlation analysis), baseline model (M1), diagnostic testing (Breusch-Pagan, VIF, Cook's distance, Durbin-Watson, Q-Q plots), predicted vs actual plots.

**Both:** Collaborative development and comparison of candidate models (M2–M7), model selection.

---

## Key Insight

Replacing continuous BMI with a **binary obesity indicator** (BMI >= 30) and adding an **obese x smoker interaction term** was the single biggest performance driver:

```
M1 Baseline (age + bmi + children):         R² = 0.12   RMSE = $11,355
M2 Full (all predictors, no interaction):    R² = 0.75   RMSE = $6,042
M4 BMI x smoker interaction:                R² = 0.84   RMSE = $4,860
M6 Obese x smoker interaction (FINAL):      R² = 0.86   RMSE = $4,511  ✅
```

Obese smokers face **disproportionately higher charges** -- an obese smoker pays roughly **$33,000 more per year** than a non-obese non-smoker, holding age and children fixed.

---

## Dataset

- **Source:** Kaggle Medical Insurance dataset
- **Size:** 1,338 individuals across the United States
- **Target:** Individual medical insurance charges (USD)

| Variable | Type | Description |
|---|---|---|
| age | Continuous | Age of primary beneficiary (18-64) |
| sex | Categorical | Gender: male / female |
| bmi | Continuous | Body Mass Index (kg/m²) |
| children | Discrete | Number of dependents covered |
| smoker | Categorical | Smoking status: yes / no |
| region | Categorical | US residential region (4 levels) |
| charges | Continuous | Medical insurance cost (USD) -- response variable |

---

## Methodology

### Exploratory Data Analysis
- Right-skewed bimodal distribution in charges -- signature of a hidden binary driver (smoking)
- Correlation: smoker (r = 0.79) >> age (r = 0.30) > bmi (r = 0.20) > children (r = 0.07)
- Group mean charges: non-obese non-smoker $7,977 | non-obese smoker $21,363 | obese non-smoker $8,843 | **obese smoker $41,558**

### Feature Engineering
- Binary **obese** indicator (BMI >= 30) based on WHO clinical threshold
- **obese x smoker** interaction term
- age-squared for polynomial model testing
- Log-transformed charges for M5 comparison

### OLS Assumption Testing (Baseline M1)

| Assumption | Test | Result |
|---|---|---|
| Homoscedasticity | Breusch-Pagan (BP=134.26, p<0.001) | VIOLATED |
| Normality | Q-Q plot | VIOLATED |
| Independence | Durbin-Watson (DW=2.01) | OK |
| Multicollinearity | VIF (all < 1.02) | OK |

Both violations traced to the omitted smoker variable creating two distinct subpopulations.

### Candidate Models

| Model | Formula | R² | RMSE | BP Verdict |
|---|---|---|---|---|
| M1 Baseline | charges ~ age + bmi + children | 0.12 | $11,355 | Violated |
| M2 Full | charges ~ all predictors | 0.75 | $6,042 | Violated |
| M3 Polynomial | charges ~ age + age² + bmi + smoker + children | 0.75 | $6,022 | Violated |
| M4 Interaction | charges ~ age + bmi + smoker + bmi x smoker + children | 0.84 | $4,860 | OK |
| M5 Log-transformed | log(charges) ~ age + bmi + smoker + children | 0.76 | -- | Violated |
| **M6 Obese Interaction** | **charges ~ age + obese + smoker + obese x smoker + children** | **0.86** | **$4,511** | **OK** |
| M7 Stepwise AIC | Bidirectional stepwise selection | 0.86 | $4,490 | OK |

M6 selected over M7 to preserve model hierarchy -- M7 included the obese x smoker interaction without the obese main effect, violating interpretability principles.

### Final Model (M6) Coefficients

| Term | Coefficient | Interpretation |
|---|---|---|
| age | +$266/year | Each additional year of age increases charges by ~$266 |
| obese | +$128 | Non-significant among non-smokers |
| smoker | +$13,389 | Baseline cost increase for non-obese smokers |
| obese x smoker | +$19,740 | **Key interaction: obese smokers pay dramatically more** |
| children | +$514/child | Each additional dependent adds ~$514 |

---

## Project Structure

```
insurance-price-prediction/
├── insurance.csv                    # Dataset (1,338 records)
├── insurance_regression.ipynb       # Analysis notebook (M1-M7)
├── report.pdf                       # Full written report (LaTeX)
└── README.md
```

---

## Setup

```bash
pip install pandas numpy matplotlib seaborn statsmodels scipy
```

```bash
jupyter notebook insurance_regression.ipynb
```

---

## Tech Stack

| Component | Technology |
|---|---|
| Regression modeling | statsmodels OLS |
| Feature engineering | pandas, NumPy |
| Diagnostics | Breusch-Pagan, Shapiro-Wilk, VIF, Cook's Distance, Durbin-Watson |
| Visualization | matplotlib, seaborn |
| Model selection | AIC/BIC, bidirectional stepwise selection |
| Report | LaTeX (UC San Diego DSC 241) |
