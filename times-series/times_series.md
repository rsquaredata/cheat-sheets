```
title: "Time Series Analysis: Complete Methodology"
author: "rsquaredata"
date: 2026-01-21
date-format: "YYYY-MM-DD"
format: html
```

# 1. Data Preparation & Object Creation

Before any math, the data must be in a format R understands.
 - Cleaning: Handle missing values or format issues.
 - Function: `ts(data, frequency = n, start = c(year, period))`
 - Note: The frequency $n$ is the number of observations per "unit" (e.g., 4 for quarterly, 12 for monthly, 96 for 15-min daily).

# 2. Descriptive Analysis (Visualizing the Components)

Decompose the series to see what the model needs to capture.
 - Time Plot: Check for outliers, level shifts, or changing variance.
   - Function: `plot()` or `autoplot()`.
 - Decomposition: Separate Trend, Seasonality, and Noise.
   - Function: `stl(x, s.window = "periodic")` (for additive) or `decompose()`.
   - Visual:

# 3. Stationarity Testing & Transformation

Essential for ARIMA-style modeling.
 - Unit Root Tests:
   - Function (ADF): `tseries::adf.test()` (H_0: Non-stationary).
   - Function (KPSS): `tseries::kpss.test()` (H_0: Stationary).
 - Differencing: If the series isn't stationary.
   - Function:`diff(x, differences = d)` (Non-seasonal) or `diff(x, lag = f)` (Seasonal).
 - Visual:

# 4. Identification (Box-Jenkins Methodology)

Deciding the $p$ (AR) and $q$ (MA) orders.
 - Correlation Plots:
   - Function: Acf(x) and Pacf(x) or ggtsdisplay().
 - Logic: Look for spikes that exceed the significance blue lines at non-seasonal and seasonal lags.
 - Visual:

# 5. Estimation (Model Fitting)

Applying the different families of models.
 - Exponential Smoothing:
   - Simple (SES): `ses(x, h = 96)`.
   - Holt (Trend):`holt(x, h = 96)`.
   - Holt-Winters (Trend + Seasonality): `HoltWinters(x)` or `.
 - ARIMA:
   - Manual: `Arima(x, order = c(p,d,q), seasonal = c(P,D,Q))`.
   - Automatic: `auto.arima(x, seasonal = TRUE)`.

# 6. Validation (Residual Diagnostics)
Checking if the model "left any information on the table."
 - White Noise Check: Residuals should have no pattern.
   - Function: `checkresiduals(model_fit)`.
 - Ljung-Box Test: Statistical test for remaining autocorrelation.
   - Goal: p-value > 0.05 (meaning residuals are random).

# 7. Evaluation (Comparative Accuracy)

Splitting the data to find the "winner."
 - Splitting:
   - Functions: `head(x, n - h)` and `tail(x, h)`.
 - Error Metrics: Compare RMSE, MAE, and MAPE.
   - Function: `accuracy(forecast_object, test_set)`.

# 8. Multivariate Analysis (Optional - Course Section 4)

If you have external predictors.
 - Granger Causality: Does variable A help predict B?
   - Function: `lmtest::grangertest(Y ~ X)`.
 - VAR Models: Vector Auto-Regression for co-dependent variables.
   - Function: `vars::VAR(data_matrix)`.

# 9. Production Forecast

Once the best model is found, re-train it on the entire dataset and project the future.
 - Forecasting: `forecast(winning_model, h = 96)`.
 - Exporting: `write.csv()` or `writexl::write_xlsx()`.

# Tutorial Summary Table
| Step | Goal | Main R Function |
|---|---|---|
| Ingest | Prepare ts | ts() |
| Describe | Decomposition | stl() |
| Test | Stationarity | adf.test() & kpss.test() |
| Fit (ES) | Smoothing | HoltWinters() |
| Fit (BJ) | ARIMA | auto.arima() |
| Validate | Residuals | checkresiduals() |
| Compare | Error metrics | accuracy() |










