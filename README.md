# Insurance Price Prediction

> Regression analysis to predict individual medical insurance charges, achieving R² = 0.86 through feature engineering and interaction term discovery.

![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)
![statsmodels](https://img.shields.io/badge/statsmodels-4EABE6?style=for-the-badge)
![scikit-learn](https://img.shields.io/badge/scikit--learn-%23F7931E.svg?style=for-the-badge&logo=scikit-learn&logoColor=white)

---

## Overview

An end-to-end regression study predicting individual medical insurance charges using demographic and health data. The project goes beyond baseline modeling -- rigorously testing OLS assumptions, engineering features from domain knowledge, and discovering that an **obesity-smoking interaction term** explains a cost impact absent in all additive models.

**Key Results:**
- 🎯 **R² = 0.86, Adjusted R² = 0.86** on final model (M6)
- 💡 **obese x smoker interaction** captures **+$19,740 cost impact** not found in baseline models
- 📊 Evaluated **7 candidate regression models** with full OLS diagnostics
- ✅ Individual project -- all analysis, coding, and writing by Hannah Pham

---

## Key Insight

Replacing continuous BMI with a **binary obesity indicator** (BMI >= 30) and adding an **obese x smoker interaction term** was the single biggest performance driver:

```
M1 Baseline (age + bmi + children):         R² = 0.12
M2 Full (all predictors, no interaction):    R² = 0.75
M4 BMI x smoker interaction:                R² = 0.84
M6 Obese x smoker interaction (FINAL):      R² = 0.86  ✅
```

Obese smokers face **disproportionately higher charges** than either obese non-smokers or non-obese smokers -- a synergistic effect that continuous BMI models fail to capture cleanly.

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

### 1. Exploratory Data Analysis
- Identified right-skewed bimodal distribution in charges -- signature of a hidden binary driver (smoking)
- Correlation analysis: smoker (r = 0.79) >> age (r = 0.30) > bmi (r = 0.20) > children (r = 0.07)
- Scatter plots revealed a BMI threshold effect at 30 -- only visible among smokers

### 2. Feature Engineering
- Created binary **obese** indicator (BMI >= 30) based on WHO clinical threshold
- Created **obese x smoker** interaction term
- Created age-squared for polynomial model testing
- Log-transformed charges for M5 comparison

### 3. OLS Assumption Testing (Baseline M1)

| Assumption | Test | Result |
|---|---|---|
| Homoscedasticity | Breusch-Pagan | VIOLATED |
| Normality | Shapiro-Wilk | VIOLATED |
| Independence | Durbin-Watson | OK (DW ~2) |
| Linearity | RESET test | OK |
| Multicollinearity | VIF | OK (all VIF < 5) |

Both violations traced back to the omitted smoker variable creating two distinct subpopulations in the data.

### 4. Candidate Models

| Model | Formula | R² |
|---|---|---|
| M1 Baseline | charges ~ age + bmi + children | 0.12 |
| M2 Full | charges ~ all predictors | 0.75 |
| M3 Polynomial | charges ~ age + age² + bmi + smoker + children | 0.78 |
| M4 Interaction | charges ~ age + bmi + smoker + bmi x smoker + children | 0.84 |
| M5 Log-transformed | log(charges) ~ age + bmi + smoker + children | n/a |
| M6 Obese Interaction | charges ~ age + obese + smoker + obese x smoker + children | 0.86 ✅ |
| M7 Stepwise AIC | Bidirectional stepwise (AIC-based) | 0.86 |

### 5. Final Model (M6) Key Coefficients

| Term | Interpretation |
|---|---|
| smoker | Large baseline shift for smokers vs non-smokers |
| obese | Modest effect among non-smokers |
| obese x smoker | Amplified charges for obese smokers -- the key interaction |
| age | Positive linear effect per additional year |
| children | Small positive effect per dependent |

---

## Project Structure

```
insurance-price-prediction/
├── insurance.csv                    # Dataset (1,338 records)
├── insurance_regression.ipynb       # Full analysis notebook
│   ├── Section 4: EDA + feature engineering
│   ├── Section 5: Baseline diagnostics (M1)
│   ├── Section 6: Candidate models (M2-M7) + comparisons
│   └── Section 7: Conclusions + final model summary
└── README.md
```

---

## Setup

```bash
pip install pandas numpy matplotlib seaborn statsmodels scipy
```

## Usage

```bash
jupyter notebook insurance_regression.ipynb
```

Update the data path in the first cell if needed:
```python
df = pd.read_csv('insurance.csv')
```

---

## Tech Stack

| Component | Technology |
|---|---|
| Regression modeling | statsmodels OLS |
| Feature engineering | pandas, NumPy |
| Diagnostics | Breusch-Pagan, Shapiro-Wilk, RESET, VIF, Cook's Distance, Durbin-Watson |
| Visualization | matplotlib, seaborn |
| Model selection | AIC/BIC, bidirectional stepwise selection |
