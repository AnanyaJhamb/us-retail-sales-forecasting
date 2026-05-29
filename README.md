# U.S. Retail Sales Forecasting

SARIMA time series forecasting model for U.S. food & beverage retail sales using 27 years of monthly data from the Federal Reserve Economic Data (FRED).

## Overview

Retail sales forecasting supports inventory planning, seasonal staffing, and revenue projection. This project builds a SARIMA model on monthly U.S. food & beverage retail sales and evaluates 12-month-ahead forecast accuracy on a held-out test year.

## Data

- **Source:** U.S. Census Bureau via FRED (series MRTSSM445USN)
- **Period:** January 1992 – December 2019 (336 monthly observations)
- **Training set:** Jan 1992 – Dec 2018 (324 observations)
- **Test set:** Jan 2019 – Dec 2019 (12 observations)

## Methodology

1. **Stationarity testing** with KPSS on three transformations:
   - Original series: p = 0.000 (not stationary)
   - First-differenced: p = 0.205 (stationary)
   - Log + first-differenced: p = 0.766 (most stationary, selected)
2. **Seasonal differencing** at lag 12 to remove yearly cycle
3. **Order identification** via ACF / PACF inspection
4. **Model comparison** across 3 candidate specifications
5. **Diagnostics** via Ljung-Box residual test

## Model Selection

| Model | Specification | AIC | Ljung-Box p | Outcome |
|-------|--------------|-----|-------------|---------|
| 1 | SARIMA(1,1,1)(1,1,1)₁₂ | −1755.0 | 0.18 | **Selected** |
| 2 | SARIMA(0,1,1)(0,1,1)₁₂ | −1708.4 | ≈ 0.00 | Eliminated |
| 3 | SARIMA(1,1,1)(0,1,1)₁₂ | −1745.2 | 0.07 | Borderline |

Final model: **SARIMA(1,1,1)(1,1,1)₁₂** fit on log(Xₜ), back-transformed to original scale.

## Results

| Metric | Value |
|--------|-------|
| MAPE (12-month horizon) | **1.26%** |
| Forecast accuracy | **98.7%** |
| Forecasts within 95% CI | **12 / 12 (100%)** |
| Coefficient significance | All p < 0.01 |

## Managerial Implications

- **Inventory planning:** MAPE < 2% supports confident 1-month-ahead inventory orders with minimal over/understock risk
- **Seasonal staffing:** Model captures December peaks and February troughs reliably
- **Revenue forecasting:** Provides actionable monthly projections for budget cycles

## Limitations & Future Work

SARIMA is univariate and learns only from its own history, so it is blind to inflation, recessions, and shocks like COVID. Three extensions to make it more robust:

1. **SARIMAX** — add CPI, unemployment, holidays, and income as exogenous regressors
2. **Post-COVID stress test** — re-train through 2019, forecast 2020–2024 to quantify break impact
3. **Hybrid SARIMA + ML** — let SARIMA handle seasonality while an ML model fits residuals to catch non-linear shocks

## Tech Stack

Python · statsmodels · pandas · NumPy · matplotlib

## Files

- `retail_sales_forecasting.ipynb` — full pipeline: data prep, stationarity testing, model selection, forecasting, diagnostics
- `requirements.txt` — dependencies

## Team

Group 18 — Ananya Jhamb [+ teammates]
Johns Hopkins Carey | MS Business Analytics & AI
Course: Forecasting Models for Business Intelligence
