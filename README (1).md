# What Drives House Prices in California?

Linear regression, model diagnostics and influential-point analysis on the 1990 California Housing census data
(20,640 districts).

![Geography of house values](figures/geography.png)

## Question

How well does neighbourhood income explain median house value? How much do we gain by adding location, housing age
and household structure? And can the linear model be trusted?

## Key results

| Model | Test R² | Test RMSE | Test MAE |
|---|---|---|---|
| Income only | 0.42 | $76,059 | $57,911 |
| + housing characteristics | 0.57 | $65,539 | $49,701 |
| **+ location** | **0.65** | **$58,946** | $44,032 |
| + location, log(value) | 0.62 | $61,537 | **$41,662** |

*Evaluated on a held-out 20% test set. The multiple models exclude the 965 districts at the $500k census cap.*

- **Income alone explains 47% of the variation.** Each extra $10,000 of median income goes with about $42,000 higher median house value.
- **The simple model breaks key assumptions.** Its residuals are skewed and their variance is not constant (Breusch–Pagan p < 0.001), so the multiple models use robust (HC3) standard errors.
- **1,230 districts (6%) are influential by Cook's distance.** 59% of them sit at the $500k cap. Dropping them barely changes the income effect.
- **Adding location and household structure** cuts prediction error by about $17k per district. Distance to San Francisco or Los Angeles and household crowding are the strongest factors after income.

![Model comparison](figures/model_comparison.png)

## What the notebook covers

1. **Data loading and feature engineering** from the raw census files: per-household averages, distance to major cities using the haversine formula, and log transforms
2. **Data-quality audit:** missing values, duplicates, censored (top-coded) variables, and outliers from group quarters such as dorms
3. **Exploratory analysis:** distributions, correlation matrix, and a geographic map of house values
4. **Simple OLS regression** with `statsmodels`, with coefficient interpretation and confidence intervals
5. **Diagnostics:** residuals vs fitted, Q–Q plot, Breusch–Pagan test
6. **Influence analysis:** Cook's distance, leverage vs studentized residuals, profile of influential districts, sensitivity refit
7. **Censoring:** how the $500k cap biases the slope
8. **Multiple regression** with multicollinearity checks (VIF) and robust standard errors
9. **Out-of-sample validation:** train/test split, R², RMSE, MAE, and standardised effect sizes
10. **Findings, limitations and next steps** written for a non-technical reader

## Tools

Python · pandas · NumPy · statsmodels · scikit-learn · SciPy · matplotlib · seaborn

## Run it yourself

```bash
git clone https://github.com/<your-username>/california-housing-regression.git
cd california-housing-regression
pip install -r requirements.txt
jupyter notebook notebooks/california_housing_regression.ipynb
```

## Repository structure

```
├── data/
│   ├── cal_housing.data       # raw census data (20,640 rows)
│   └── cal_housing.domain     # column descriptions
├── figures/                   # charts exported by the notebook
├── notebooks/
│   └── california_housing_regression.ipynb
├── requirements.txt
└── README.md
```

## Data source

Pace, R. K. and Barry, R. (1997). *Sparse Spatial Autoregressions.* Statistics and Probability Letters, 33, 291–297.
Also distributed with scikit-learn as
[`fetch_california_housing`](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.fetch_california_housing.html).

## Author

**Bashir Omondi Nuru**: geography educator and data science learner (Moringa School). Interested in economic
geography, spatial data and sustainability.
