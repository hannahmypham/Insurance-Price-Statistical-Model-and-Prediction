# Insurance-Price-Statistical-Model-and-Prediction 

A statistical modeling project that develops and compares regression models to predict individual medical insurance costs using demographic and health-related variables.

## Overview

This project applies classical linear regression diagnostics and model selection techniques to identify the main drivers of insurance charges. The analysis begins with exploratory data analysis and a baseline regression model, followed by iterative refinement using polynomial terms, interaction effects, and transformations.

The goal is to select a model that provides strong predictive performance while maintaining interpretability and satisfying regression assumptions.

## Data

- **Source:** `insurance.csv`
- **Observations:** 1338 individuals
- **Variables:**
  - `age` – age of the insured individual
  - `sex` – gender
  - `bmi` – body mass index
  - `children` – number of dependents
  - `smoker` – smoking status
  - `region` – residential region in the US
  - `charges` – annual medical insurance cost (USD)

The response variable in all models is **charges**.

## Project Structure

1. **Data Description and Exploratory Data Analysis**
   - Variable summaries and distributions
   - Scatter plots and correlation analysis
   - Identification of nonlinear patterns and subpopulations

2. **Data Engineering**
   - Creation of additional features (e.g., obese indicator)
   - Encoding categorical variables

3. **Baseline Model and Preliminary Diagnostics**
   - Initial regression model (M1)
   - Diagnostic checks:
     - Residual plots
     - Breusch–Pagan test (heteroscedasticity)
     - Variance Inflation Factor (multicollinearity)
     - Cook's distance (influential points)
     - Durbin–Watson test (autocorrelation)

4. **Candidate Regression Models**
   - **M2:** Full model
   - **M3:** Polynomial model (age²)
   - **M4:** Interaction model
   - **M5:** Log-transformed model

5. **Model Comparison**
   Models are evaluated using:

   - R²
   - Adjusted R²
   - RMSE
   - AIC
   - BIC

6. **Conclusions**
   The best-performing model is selected based on predictive performance, diagnostic improvement, and interpretability.

## Final Model (M4)

The selected model is the **interaction model**, which captures the joint effect of BMI and smoking status:

charges = β₀ + β₁ age + β₂ bmi + β₃ smoker + β₄ (bmi × smoker) + β₅ children

This model reveals that:

- Smoking status is the strongest predictor of insurance costs.
- BMI has a modest effect among non-smokers.
- The effect of BMI increases substantially for smokers, indicating an interaction between lifestyle risk factors.

## Key Insights

- Smoking dramatically increases expected medical insurance charges.
- Higher BMI is associated with increased costs, particularly among smokers.
- Accounting for the **BMI × smoker interaction** substantially improves model fit and reduces unexplained variance.

## Repository Contents

- `FinalProject2.ipynb` – full analysis notebook
- `insurance.csv` – dataset
- `README.md` – project description

## Tools

- Python
- pandas
- statsmodels
- seaborn / matplotlib
