<!---
title: "Time series"
author: "rsquaredata"
last updated: 2026-01-25
--->

💡 To run the present file as a notebook:
1. Save this file locally, as well as [varicelle](https://raw.githubusercontent.com/rsquaredata/cheat-sheets/refs/heads/main/time-series/varicelle.csv) and [sanfran](https://raw.githubusercontent.com/rsquaredata/cheat-sheets/refs/heads/main/time-series/sanfran.csv) data
2. Rename this file to `notebook.qmd` ([Quarto](https://quarto.org/) file)
3. Open the file with [RStudio](https://posit.co/download/rstudio-desktop/).
4. 
5. Enjoy

</br>
</br>
</br>

# 1. Introduction to Time Series

## 1.1. What is a time series?

A **time series** is a sequence of observations indexed by time: $(x_t)_{1 \le t \le n} = (x_1, \ldots, x_n)$

Properties:
-   observations are **ordered in time**
-   usually **equally spaced**
-   time can be days, months, years, etc.

**Main objective**: forecast future values $x_{n+1}, x_{n+2}, \ldots$

## 1.2. Typical components of a time series

A time series can contain:

| **component**   | **meaning**                              |
|-----------------|------------------------------------------|
| **trend**       | long-term increase or decrease          |
| **seasonality** | regular pattern with fixed period        |
| **cycle**       | irregular oscillations (no fixed period) |
| **noise**       | random fluctuations                      |

### Visual exploration (always first step)

<details><summary>HOW TO: plot a `ts`</summary>

```r
plot(serie,
    type="l",
    xlab="time",
    ylab="dataset_name",
    sub="Figure 1: Description of the plot")
```
</details>

```{r}
plot(USAccDeaths)
plot(AirPassengers)
plot(sunspots)
plot(EuStockMarkets[, "CAC"])
```
Visual inspection already gives strong hints about:
-   presence of a trend
-   presence of a seasonality
-   appropriateness of forecasting models

## 1.3. Why linear regression is not ideal for time series

### Linear regression treats all observations equally

```{r}
t <- 1:100
model <- lm(x ~ t)
```

Problems:
-   old observations get the **same weight** as recent ones
-   poorly adapts to **structural changes**
-   unsuitable when recent history is more informative

→ This motivates **exponential smoothing**.

## 1.4. Time series objects in R (`ts`)

<details><summary>HOW TO: forecast wih predict</summary>

```r
# fetch tabular data from url and create a data frame
data <- read.table(file="https://example.com/dataset.csv")
# assuming the relevant values are in the variable column 'Var1'
plot(data$Var1)
# create time vector (say from 1 to 100)
t <- 1:100
# create the data vector from observed values
x <- data$Var1
# fit a linear regression
model <- lm(x ~ t)
# create a df with a a column t for new times 101 to 120
newt <- data.frame(t=101:120)
# forecast 20 steps ahead
p <- predict(model, newdata = newt)
# plot with additional red line for the predictions
lines(newt$t,p,col="red",lwd=2)
```
</details>

R uses the `ts` class to store time series metadata.

### Creating a ts object

<details><summary>HOW TO: create a ts object</summary>

```r
# fetch data
data <- read.csv(file="http://example.com")
# check variable/column names
names(data)
# visualize data (assuming x is the relevant variable)
plot(data$x)
# create the ts object
## yearly seasonality → freq=12
## assuming data start in January 2000 and end in December 2001
series <- ts(data$x, start=c(2000,1), end=c(2001,12))
plot(series)
```
</details>

```{r}
data <- read.csv(file="https://raw.githubusercontent.com/rsquaredata/cheat-sheets/refs/heads/main/time-series/varicelle.csv")
varicelle <- ts(
  data$x,
  start = c(1931, 1),
  frequency = 12
)
```
Key parameters:
-   `start`: first observation (year, sub-period)
-   `frequency`: number of observations per cycle

### Visualization with forecast

<details><summary>HOW TO: plot with forecast</summary>

```r
library(forecast)
library(ggplot2)
autoplot(series) +
    ggtitle("Title") +
    xlab("year") +
    ylab("values")

# seasonal plot
ggseasonplot(series, year, labels=TRUE, year.labels.left=TRUE)

# polar seasonal plot
ggsesonplot(series, polar=TRUE)
```
</details>

```{r}
library(forecast)
autoplot(varicelle)
```

Seasonal plots:

```{r}
ggseasonplot(varicelle)
ggseasonplot(varicelle, polar = TRUE)
```
→ Useful to assess **strength and stability of seasonality**.

## 1.5. Handling missing values

<details><summary>HOW TO: handle missing values</summary>

```r
#distribution of missing values
library(imputeTS)
ggplot_na_distribution(series)

# imputation by interpolation
timeseries <- na_interpolation(series)
ggplot_na_distribution(series)
```
</details>

Missing values are common in time series.

```{r}
library(imputeTS)
x <- ts(c(2, 3, 4, 5, 6, NA, 7, 8))
x <- na_interpolation(x)
```

## 1.6. Descriptive statistics for time series

<details><summary>HOW TO: do descriptive stats on time series</summary>

```r
library(forecast)
# empirical mean
mean(series)
# empirical variance
var(series)
# empirical covariance
tmp <- acf(series, type="cor", plot=FALSE)
# plot the correlogram
plot(tmp)
```
</details>

### Mean and variance

```{r}
mean(varicelle)
var(varicelle)
```

### Autocovariance and autocorrelation

<details><summary>HOW TO: interpret auto-correlations</summary>

### ACF

```r
# series with a pure linear trend: x_t = ax+b
series <- 2*(1:100)+4
par(mfrow=c(1,2))
plot(ts(series))
acf(series)
```
```r
# series with a pure seasonal pattern, e.g. x_t = a*cos(2t*pi/T)
series <- cos(2*pi/12*(1:100))
par(mfrow=c(1,2))
plot(ts(series))
acf(series)
```

| ACF pattern                  |observed behavior | interpretation |
|-------------------------------|-------------------|-----------------|
| rapid decay to zero           | autocorrelations become insignificant after a few lags | stationary series, short range dependence |
| slow monotonic decay          | high autocorrelation over many lags | trend / non-stationarity |
| persistent sinusoidal pattern | regular oscillation with stable amplitude | non-stationary seasonality |
| damped sinusoidal pattern     | oscillations with decreased amplitude | oscillatory AR process (complex roots) |
| sharp cutoff after lag $q$    | autocorrelation drops to zero after $q$ | $MA_q$ process |
| significant spikes at multiples of $T$ | peaks at lags $T$, $2T$, $3T$, ...) | seasonality with period $T$ |
| no significant autocorrelations | all lags within confidence bounds | white noise |

#### PACF

```{r}
pacf(series, type="cor")
```

| PACF pattern | observed behavior | interpretation | typical action |
|--------------|-------------------|----------------|----------------|
| sharp cutoff after lag $p$ | significant partial autocorrelation up to lag $p$, then none | $AR_p$ process | $p$ = $AR$ order |
| gradual decay | significant partial autocorrelations decrease slowly | $MA_q$ or $ARMA_{(p,q)}$ | use ACF to identify $q$ |
| significant spike at lag 1 only | only lag 1 is significant | $AR_1$ | $p$ = 1 |
| significant spikes at lags 1 and 2 | Two significant partial autocorrelations | $AR_2$ | $p$ = 2 |
| significant spikes at multiples of $T$ | peaks at lags $T$, $2T$, ... | seasonal $AR$ component | seasonal AR term |
| no significant partial autocorrelations | all values within confidence bounds | no $AR$ structure / white noise | no $AR$ term |
</details>

**Autocovariance (lag $h$)**: $\hat{\sigma}(h)$

**Autocorrelation (lag $h$)**: $\hat{\phi}(h) = \frac{\hat{\sigma}(h)}{\hat{\sigma}(0)}$

```{r}
acf(varicelle)
pacf(varicelle)
```
Interpretation:
-   slow decay → trend or non-stationarity
-   spikes are multiples of period → seasonality
-   PACF → useful to identify AR structure later

## 1.7. Statistical tests for time series

### 1.7.1 Autocorrelation tests

<details><summary>HOW TO: test the significance of autocorrelations</summary>

```r
Box.test(series, lag=h, type="Box-Pierce")
```
- h: degrees of freedom; h should be high enough to reckon with all significant autocorrelations but low enough so that the test remains sufficiently powerful
- the p-value is the probability to observe a result which is at least as extreme as the result obtained if the null $H_0$ were true.
- a p-value < 0.05 means the time series is autocorrelated

```{r}
Box.test(series, lag=h, type="Ljung-Box")
```
- same test, more powerful
</details>

**Box-Pierce / Ljung-Box**

```{r}
Box.test(varicelle, lag = 10, type = "Ljung-Box")
```
-   $H_0$: no autocorrelation
-   small p-value → time dependence exists.

### 1.7.2. Trend tests

#### Linear trend (robust to autocorrelation)

```{r}
library(funtimes)
t <- time(varicelle)
wavk_test(varicelle ~ t)$p.value
```

#### Monotonic trend (non-parametric)

```{r}
library(Kendall)
MannKendall(varicelle)$sl
```

#### Any type of trend (global approach)

```{r}
wavk_test(varicelle ~ t)$p.value
```

<details><summary>HOW TO: do the same tests without `funtimes`</summary>

1. Testing for a linear trend (linear regression)

```{r}
t <- time(series)
p_trend <- summary(lm(series ~ t))$coefficients["t", "Pr(>|t|)"]
cat("p-value:", formatC(p_trend, format = "e", digits = 3), "\n")
```
- the p-value extracted here is the p-value of the t-test on the slope coefficient $\beta_1$
- p-value < 0.05 means there is a statistically significant linear trend / the time series is not stationary

2. Testing for a monotonic trend (Mann-Kendall test)

```{r}
library(Kendall)
mk_test <- MannKendall(series)
p_mk <- mk_test$sl
cat("Mann–Kendall p-value:", formatC(p_mk, format = "e", digits = 3), "\n")
```
p-value < 0.05 is evidence of a monotonic trend (not necessarily linear)

3. Testing for any kind of trend (combination of tests)

```{r}
t <- time(varicelle)

# --- Linear deterministic trend ---
p_linear <- summary(lm(series ~ t))$coefficients["t", "Pr(>|t|)"]

# --- Nonlinear deterministic (polynomial) trend ---
fit_poly <- lm(series ~ poly(t, 2, raw = TRUE))
p_poly <- anova(fit_poly)[["Pr(>F)"]][1]

# --- Monotonic non-parametric trend ---
library(Kendall)
p_mk <- MannKendall(series)$sl

# --- Summary table ---
trend_tests <- data.frame(
  Test = c("Linear trend (LM)",
           "Polynomial trend (degree 2)",
           "Monotonic trend (Mann–Kendall)"),
  p_value = c(p_linear, p_poly, p_mk),
  Significant_5pct = c(p_linear < 0.05,
                       p_poly < 0.05,
                       p_mk < 0.05)
)

print(trend_tests)
```

4. Testing for a specific (e.g. polynomial) trend

```{r}
t <- time(series)
p_poly <- anova(lm(series ~ poly(t, 2, raw = TRUE)))[["Pr(>F)"]][1]
cat("Polynomial trend test p-value:", formatC(p_poly, format = "e", digits = 3), "\n")
```
</details>

# 2. Exponential smoothing

## 2.1. Motivation: why exponential smoothing?

Linear regression assigns the **same weights** to all observations, regardless of their age.  

This may lead to poor forecasts when:
-   the most recent observations are more informative,
-   the trend changes over time,
-   the process stabilizes or saturates

```{r}
set.seed(123)

temps <- 1:100
serie <- c(
  temps[1:80] + rnorm(80, sd = 6),
  rep(80, 20) + rnorm(20, sd = 4)
)

mod <- lm(serie ~ temps)

plot(ts(serie), xlim = c(1,120), ylim = c(0,120))
abline(mod$coefficients, col = "red", lwd = 2)
```
This example shows that linear regression extrapolates a trend even when the series has clearly stabilized.

## 2.2. Principle of exponential smoothing

Exponential smoothing assigns **exponentially decreasing weights** to past observations:
-   recent observations have more influence,
-   older observations progressively lose importance,

This family of models is **deterministic**: it explicitly models level, trend, and seasonality.

## 2.3. Simple Exponential Smoothing (SES)

SES is suitable for series with no trend and no seasonality. The forecast is **constant** across horizons.

```{r}
serie <- ts(serie)

# SES with fixed alpha
LES_01 <- HoltWinters(serie, alpha = 0.1, beta = FALSE, gamma = FALSE)
LES_09 <- HoltWinters(serie, alpha = 0.9, beta = FALSE, gamma = FALSE)

plot(serie, xlim = c(1,120), ylim = c(0,120))
lines(predict(LES_01, n.ahead = 20), col = "blue")
lines(predict(LES_09, n.ahead = 20), col = "red")
```
-   small $\alpha$: strong smoothing, slow adaptation,
-   large $\alpha$: fast adaptation to recent changes.

### Automatic choice of $\alpha$

```{r}
LES_auto <- HoltWinters(serie, alpha = NULL, beta = FALSE, gamma = FALSE)
LES_auto$alpha
```

## 2.4. Forecast evaluation: train/test split

Model performance must be evaluated **out-of-sample**.

```{r}
n <- length(serie)
h <- 20

train <- window(serie, end = n - h)
test  <- window(serie, start = n - h + 1)
```

Once the best model is selected, it must be **re-estimated on the full dataset** before producing final forecasts.

## 2.5. Forecast accuracy metrics

Two common metrics:

```{r}
rmse <- function(actual, forecast) {
  sqrt(mean((actual - forecast)^2))
}

mape <- function(actual, forecast) {
  mean(abs((actual - forecast) / actual)) * 100
}
```
-   RMSE penalizes large errors,
-   MAPE is scale-free but unstable near zero.

## 2.6. Extensions of exponential smoothing models

### 2.6.1. Holt's linear trend model

Used when the series exhibits a trend but no seasonality.

```{r}
fit_holt <- HoltWinters(serie, alpha = NULL, beta = NULL, gamma = FALSE)

plot(serie, xlim = c(1,120), ylim = c(0,120))
lines(predict(fit_holt, n.ahead = 20), col = 2)
```

### 2.6.2. Damped trend Holt model

Used when the trend is expected to **flatten over time**.

```{r}
library(forecast)

fit_holt_damped <- holt(serie, damped = TRUE, h = 20)
autoplot(fit_holt_damped)
```
The damping parameter $\phi \lt 1$ reduces long-term trend extrapolation.

## 2.7. Seasonal Holt-Winters models

### 2.7.1. Additive seasonality

Appropriate when seasons fluctuations have **constant amplitude**.

```{r}
serie_seasonal <- ts(
  0.5*(1:100) + rnorm(100) + 3*cos(pi/6*(1:100)),
  frequency = 12
)

fit_hw_add <- HoltWinters(serie_seasonal, alpha = NULL, beta = NULL, gamma = NULL)

plot(serie_seasonal)
lines(predict(fit_hw_add, n.ahead = 24), col = 2)
```

### 2.7.2. Multiplicative seasonality

used when seasonal fluctuations **scale with the level**.

```{r}
serie_mult <- ts(
  5*(1:100) + rnorm(100) + cos(pi/6*(1:100))*(1:100),
  frequency = 12
)

fit_hw_mult <- HoltWinters(
  serie_mult,
  alpha = NULL, beta = NULL, gamma = NULL,
  seasonal = "multiplicative"
)

plot(serie_mult)
lines(predict(fit_hw_mult, n.ahead = 24), col = 2)
```

Multiplicative models are not suitable when the data contain zeros.

## 2.8. Model comparison via time series cross-validation

To avoid dependence on a single split, rolling-origin CV can be used.

```{r}
library(forecast)

e_ses  <- tsCV(serie, ses, h = 1)
e_holt <- tsCV(serie, holt, h = 1)
e_dhw  <- tsCV(serie, holt, damped = TRUE, h = 1)

mean(e_ses^2, na.rm = TRUE)
mean(e_holt^2, na.rm = TRUE)
mean(e_dhw^2, na.rm = TRUE)
```
The model with the lowest average error is selected.

## 2.9. Interpretation of smoothing parameters

The estimated parameters provide insight into the data structure:
-   small $\alpha$: noisy series, stable level,
-   small $\beta$: weak or absent trend,
-   small $\gamma$: stable seasonality,
-   $\phi \lt 1$: trend expected to stabilize.

These values help validate the coherence of the selected model.

## 2.10 Key takeaway

| **Model**         | **Components**                  |
|-------------------|---------------------------------|
| SES               | level                           |
| Holt              | level + trend                   |
| HW additive       | level + trend + additive season |
| HW multiplicative | level x season                  |

Exponential smoothing models:
-   capture **deterministic components** (level, trend, seasonality),
-   weight observations according to their recency,
-   are interpretable and robust for short-to-medium time series,
-   should be selected based on **out-of-sample performance**.

They provide a natural bridge between exploratory analysis and more complex stochastic models (ARIMA).

# 3. ARIMA Models

## 3.1. Objective of ARIMA modeling

ARIMA models are designed to model the **stochastic component** of a time series.

Before fitting an ARIMA model, the **deterministic structure** (trend and seasonal pattern) must be removed. Only after this step can we model the remaining dependent structure.

## 3.2. Decomposition of a time series

We assume an **additive decomposition** $x_t = T_t + S_t + \varepsilon_t$, where $T_t$ is the trend, $S_t$ is the seasonal pattern (period $T$) and $\varepsilon_t$ is the stochastic component.

If the decomposition is multiplicative, taking logs yields an additive decomposition.

## 3.3. Removing trend and seasonality

Two main approaches are used.

### 3.3.1. Moving average estimation

A moving average of order $m = 2k + 1$ estimates the trend: $\hat{T}_t = \frac{1}{m} \sum_{j=-k}^k x_{t+j}$
-   large $m$ → stronger smoothing
-   for seasonal series: choose $m \gt T$

```{r}
library(fpp2)

autoplot(co2, series = "Data") +
  autolayer(ma(co2, 6), series = "6-MA") +
  autolayer(ma(co2, 12), series = "12-MA") +
  xlab("Year") +
  ylab("CO2 concentration")
```

After estimating $T_t$, the seasonal pattern is obtained by averaging residuals within each season.

```{r}
autoplot(decompose(co2, type = "additive"))
```

**Limitation**: moving-average decomposition is descriptive and **cannot be used directly for forecasting**.

### 3.3.2. Differencing

Differencing removes deterministic components while preserving forecastability.

#### Regular differencing

$\Delta x_t = x_t - x_{t-1}$.

Reduces polynomial trend degree by 1.

```{r}
par(mfrow=c(2,1))
plot(co2)
plot(diff(co2))
```

Applying differencing twice removes a quadratic trend:

```{r}
par(mfrow=c(2,1))
plot(co2)
plot(diff(co2, differences=2))
```

#### Seasonal differencing

$\Delta_T x_t = x_t - x_{t-T}$.

Removes seasonal pattern of period $T$.

```{r}
par(mfrow=c(2,1))
plot(co2)
plot(diff(co2, lag=12))
```

**Practical rule**:
1.   apply seasonal differencing first,
2.   then apply regular differencing if needed,
3.   keep differencing order small.

```{r}
par(mfrow=c(3,1))
plot(co2)
plot(diff(co2, lag=12))
plot(diff(diff(co2, lag=12)))
```

## 3.4. Stationarity

A time series is **stationary** if its distribution does not depend on time:
-   no trend
-   no seasonality
-   constant variance

ARMA models apply **only to stationary series**.

## 3.5. White noise check

A stationary series may still contain no useful structure.

White noise:
-   zero mean
-   constant variance
-   no autocorrelation

```{r}
Box.test(diff(diff(co2, lag=12)), lag=10, type="Ljung-Box")
```
-   small p-value → reject white noise → further modeling required
-   large p-value → no structure left → deterministic models sufficient

## 3.6. Illustrative exercises

### AirPassengers

```{r}
plot(AirPassengers)
```
-   strong trend
-   seasonal pattern (period 12)
-   increasing variance

Differencing:
```{r}
par(mfrow=c(3,1))
plot(AirPassengers)
plot(diff(AirPassengers, lag=12))
plot(diff(diff(AirPassengers, lag=12)))
```
The differenced series appears stationary but not white noise.

### Google stock price

```{r}
library(fpp2)
plot(goog200)
```
No seasonality, strong trend.

```{r}
par(mfrow=c(3,1))
plot(goog200)
plot(diff(goog200))
plot(diff(diff(goog200)))
```

```{r}
acf(diff(goog200))
```
ACF shows no significant correlations → white noise.
**Conclusion**: $ARIMA_{(0,1,0)}$ is appropriate.

## 3.7. AR models $AR_p$

$x_t = c + \sum_{j=1}^p a_j x_{t-j} + \varepsilon_t$

Stationarity requires roots of the characteristic polynomial to lie outside the unit circle.

### Identification rules

-   ACF: exponential decay
-   PACF: cutoff after lag $p$

```{r}
set.seed(123)
ar1 <- arima.sim(list(ar=0.8), n=1000)

autoplot(ar1)
autoplot(acf(ar1, plot=FALSE))
autoplot(pacf(ar1, plot=FALSE))
```

## 3.8. MA models $MA_q$

$x_t = c + \varepsilon_t \sum_{j=1}^q b_j \varepsilon_{t-j}$

### Identification rules

-   ACF: cutoff after lag $q$
-   PACF: exponential decay

```{r}
set.seed(123)
ma1 <- arima.sim(list(ma=-0.8), n=1000)

autoplot(ma1)
autoplot(acf(ma1, plot=FALSE))
autoplot(pacf(ma1, plot=FALSE))
```

## 3.9 ARMA models

$x_t = c +  \sum_{k=1}^p a_k x_{t-k} + \sum_{j=1}^q b_j \varepsilon_{t-j}$

-   both ACF and PACF decay
-   no sharp cutoff

```{r}
set.seed(123)
arma22 <- arima.sim(list(ar=c(0.5,-0.8), ma=c(1.2,0.4)), n=1000)

par(mfrow=c(3,1))
plot(arma22)
acf(arma22)
pacf(arma22)
```

## 3.10 ARIMA models

A process is $ARIMA_{(p,d,q)}$ if $\Delta^d x_t$ is $ARMA_{(p,q)}$.

Special cases:
-   $ARIMA_{(0,0,0)}$: white noise
-   $ARIMA_{(0,1,0)}$: random walk

## 3.11. Model selection

Orders $(p,d,q)$ are selected using:
-   visual inspection
-   ACF / PACF
-   information criteria: $AIC$, $BIC$, $AIC_C$ (preferred for small samples).

```{r}
library(forecast)
auto.arima(uschange[,"Consumption"])
```

Manual comparison:

```{r}
fit1 <- Arima(uschange[,"Consumption"], order=c(3,0,0))
fit2 <- Arima(uschange[,"Consumption"], order=c(1,0,1))

summary(fit1)$aicc
summary(fit2)$aicc
```

## 3.12. Residual diagnostics

A valid ARIMA model must leave **uncorrelated residuals**.

```{r}
checkresiduals(fit1)
```
p-value > 0.05 → residual = white noise.

## 3.13. Forecasting

```{r}
forecast(fit1, h=10) %>% autoplot()
```

Forecast behavior depends on $d$ and intercept $c$:
-   $d=1$, $c \neq 0$: linear trend
-   $d=0$, $c \neq 0$: convergence to mean

## 3.14. Seasonal ARIMA (SARIMA)

$ARIMA_{(p,d,q)(P,D,Q)_T}$

Includes additional seasonal AR, MA, and differencing terms.

```{r}
ggtsdisplay(diff(diff(euretail, lag=4)))
```

Example:

```{r}
fit <- Arima(euretail, order=c(0,1,3), seasonal=c(0,1,1))
checkresiduals(fit)
forecast(fit, h=12) %>% autoplot()
```

## 3.15. Variance stabilization

When variance changes over time:
-   log transform
-   Box-Cox transform:
```{r}
auto.arima(AirPassengers, lambda="auto")
```

## 3.16. Final takeaway

ARIMA methodology follows a strict pipeline:
1.   remove deterministic structure (trend, seasonality),
2.   ensure stationarity,
3.   identify $p$ and $q$ using ACF/PACF,
4.   estimate via MLE,
5.   select using penalized criteria,
6.   validate residuals,
7.   forecast.

This framework models the **stochastic dynamics** left after exponential smoothing.

# 4. Machine Learning Methods for Time Series

## 4.1. Positioning of ML in time series forecasting

Machine learning methods differ fundamentally from classical models (ETS, ARIMA):
-   no explicit stochastic model,
-   no assumptions of stationarity or linearity,
-   forecasting is formulated as a **supervised learning problem**.

General idea: $x_t = f(x_{t-1}, x_{t-2}, \ldots, x_{t-T}$ where $f$ is learned from data.

## 4.2. Neural Networks for times series

### 4.2.1. The neuron

A neuron computes $y = g \left( \alpha_0 + \sum_{j=1}^p \alpha_j x^j \right)$:
-   linear combination of inputs,
-   followed by an activation function.

Special case: $g(x) = x \quad \Rightarrow \quad \text{linear regression}$

### 4.2.2. Neural networks

A neural network is a composition of neurons, defined by;
-   architecture (layers),
-   number of neurons,
-   activation functions,
-   learning objective.

### 4.2.3. Neural Network AutoRegression (NNAR)

NNAR models are neural-networks extensions of AR models.

#### Non-seasonal case

$NNAR_{p,k}$
-   inputs: $x_{t-1}, \ldots, x_{t-p}$
-   one hidden layer with $k$ neurons
-   sigmoid activation
-   $NNAR_{p,0}$ = $AR_p$

#### Seasonal case (period $T$)

$NNAR_{(p,P,k)_T}$
-   inputs: $x_{t-1}, \ldots, x_{t-p}, x_{t-T}, \ldots, x_{t-PT}$
-   $NNAR_{(p,P,0)_T}$ = $SARIMA_{(p,0,0)(P,0,0)_T}$

#### Estimation with `nnetar`

```{r}
library(forecast)
fit <- nnetar(x)
```

Defaults:
-   $p$: selected via AIC of linear AR model,
-   $P=1$
-   $k = (p+P+1)/2$

Options:
-   `xreg`: external regressors,
-   `lambda`: Box-Cox transformation,
-   `PI=TRUE`: bootstrap prediction intervals.

#### Pros and cons of NNAR

**Pros**
-   flexible, nonlinear,
-   captures cycles and complex patterns.

**Cons**
-   no explicit stochastic structure,
-   prediction intervals via bootstrap only,
-   differencing cannot be integrated.

#### Example:Sunspots

```{r}
autoplot(sunspotarea)

fit <- nnetar(sunspotarea)
fc  <- forecast(fit, h = 30)

autoplot(sunspotarea) +
  autolayer(fc$mean, series = "NNAR forecast")
```
NNAR successfully models **asymmetric cyclic behavior**, which linear SARIMA cannot.

## 4.3. Deep Neural Networks

### 4.3.1. Recurrent Neural Networks (RNN)

RNN process sequences using internal memory:
-   output depends on current input **and past hidden states**,
-   suitable for sequential data.

Used architectures:
-   Simple RNN,
-   LSTM,
-   GRU,
-   Transformers.

### 4.3.2. RNN workflow in R (Keras)

#### Data split

```{r}
train_data <- window(co2, end = c(1995,12))
test_data  <- window(co2, start = c(1996,1))
```

#### Normalization

```{r}
m <- mean(train_data)
s <- sd(train_data)

train_data_norm <- (train_data - m) / s
test_data_norm  <- (test_data  - m) / s
```

#### Sequence creation

```{r}
create_sequences <- function(data, window_size) {
  data <- as.numeric(data)
  X <- list(); Y <- list()
  for (i in 1:(length(data) - window_size)) {
    X[[i]] <- data[i:(i + window_size - 1)]
    Y[[i]] <- data[i + window_size]
  }
  X <- array(unlist(X), dim = c(length(X), window_size, 1))
  list(X = X, Y = unlist(Y))
}
```

#### RNN model definition

```{r}
library(keras3)

model <- keras_model_sequential() |>
  layer_simple_rnn(units = 50, activation = "tanh",
                   input_shape = c(12,1)) |>
  layer_dense(units = 1)

model |> compile(optimizer = "adam", loss = "mse")
summary(model)
```

## 4.4. Other Machine Learning models

All ML models require **data reshaping** into lagged input-output matrices.

### 4.4.1. Data preparation (lagged matrix)

```{r}
vec <- as.vector(sanfran_train)
data <- vec[1:13]
for (i in 1:(length(vec) - 13)) {
  data <- rbind(data, vec[(i+1):(i+13)])
}
```
Each row:
-   first 12 columns → inputs,
-   last column → target.

### 4.4.2. Random Forest

```{r}
library(randomForest)

fitRF <- randomForest(x = data[, -13], y = data[, 13])
```

Sequential forecasting:
```{r}
pred <- rep(NULL,36)
newdata <- tail(sanfran_train,12)
for (t in 1:36){
  pred[t] <- predict(fitRF, newdata)
  newdata <- c(newdata[-1], pred[t])
}
```

### 4.4.3. Gradient Boosting (XGBoost)

```{r}
library(xgboost)

model <- xgboost(
  data = data[,1:12], label = data[,13],
  max_depth = 10, eta = 0.5,
  nrounds = 100, objective = "reg:squarederror"
)
```
Sequential forecasting identical to RF.

### 4.4.4. Support Vector Machines (SVM)

```{r}
library(e1071)

fitSVM <- svm(x = data[, -13], y = data[, 13])
```

### 4.4.5. Linear model (baseline)

```{r}
data <- data.frame(data)
colnames(data) <- c(paste0("x",1:12),"y")
fitLM <- lm(y ~ ., data = data)
```
⚠️ **Assumes i.i.d. observations → violated for time series.**

## 4.5. Input selection

using all past lags is not optimal.

```{r}
pacf(sanfran_train)
```

Select only significant lags.

```{r}
data <- as.vector(sanfran_train)[c(1,13,14,17,24,25)]
for (i in 1:(length(sanfran_train)-25)){
  data <- rbind(data,
    as.vector(sanfran_train)[i+c(1,13,14,17,24,25)])
}
```

This often improves ML performance.

## 4.6. Multivariate-output forecasting

instead of recursive forecasting, predict multiple horizons at once.

### Multivariate linear model

```{r}
fitMLM <- lm(
  cbind(x36,x35,x34,x33,x32,x31,x30,x29,x28,x27,x26,x25) ~ .,
  data = data
)
```

Sequential or block forecasting can be implemented.

## 4.7 Conclusion

**When ML works well**
-   nonlinear trends,
-   smooth dynamics,
-   weak or irregular seasonality,
-   large datasets.

**When ML fails**
-   strong deterministic seasonality,
-   short samples,
-   high noise,
-   extrapolation far beyond training range.

**Final hierarchy (practical)**
1.   ETS /ARIMA → strong structure, interpretability
2.   NNAR → nonlinear patterns, moderate compelxity
3.   Deep learning → large data, complexe dynamics
4.   Tree-based ML -< flexible but fragile in extrapolation

# 5. Multivariate Models for Time Series

## 5.1. Time series regression models (TSLM)

### 5.1.1. Principle

A time series regression explains a target series $x_t$ using **external covariates**: $x_t = c + \beta_1 z_{1t} + \ldots + \beta_k z_{kt} + \varepsilon_t

Key assumption (standard regression): residuals $\varepsilon_t$ are **i.i.d.**.  
This assumption is often **violated** in time series.

### 5.1.2. Trend and seasonality in regression

Time series can include deterministic components:
-   **Trend**: $x_t = c + \beta_0t + \varepsilon_t$
-   **Seasonality (period $T$)**: $x_t = c + \beta_0t + \sum_{j=2}^T \delta_j d_{jt} + \varepsilon_t$
-   seasonal dummies model **fixed calendar effects**
-   first season is absorbed in the intercept

### 5.1.3. Estimation with `tslm`

```{r}
library(fpp2)

fit <- tslm(
  Consumption ~ Income + Production + Unemployment + Savings,
  data = uschange
)

summary(fit)
```

With trend and seasonality:

```{r}
fit <- tslm(
  Consumption ~ Income + Production + Unemployment + Savings + trend + season,
  data = uschange
)

summary(fit)
```

## 5.2. Feature selection in TSLM

Feature selection used **classical regression criteria**:
-   AIC / AICc / BIC
-   Adjusted $R^2$
-   Cross-validation error

```{r}
CV(fit)
```

If a variable is not significant, compare nested models:

```{r}
fit2 <- tslm(
  Consumption ~ Income + Unemployment + Savings + trend + season,
  data = uschange
)

CV(fit2)
```
⚠️ No automatic stepwise procedure exists for `tslm`; stepwise selection must be done via `lm`.

## 5.3. Residual diagnostics

Even if coefficient are significant, **residuals must be checked**.

```{r}
checkresiduals(fit, test = FALSE)
```

Test for autocorrelation:

```{r}
checkresiduals(fit, test = "LB", plot = FALSE, lag = 12)
```
If residuals are autocorrelated → **TSLM is not sufficient**.

## 5.4. Dynamic regression (Regression + ARIMA errors)

### 5.4.1. Motivation

In practice:
-   covariates explain part of the signal,
-   residuals still contain **temporal dependence**.

Dynamic regression models this explicitly: $x_t = c + \beta^T z_t + u_t, \quad u_t \sim ARIMA_{(p,d,q)}$

### 5.4.2. Estimation with `Arima(xreg=...)`

```{r}
fit <- Arima(
  uschange[,"Consumption"],
  xreg = uschange[,"Income"],
  order = c(1,0,2)
)

summary(fit)
```

Residual validation:

```{r}
checkresiduals(fit)
```

If residuals are white noise → model is valid.

### 5.4.3. Forecasting with covariates

⚠️ Future values of covariates **must be known or forecasted**.

```{r}
forecast(fit, xreg = rep(mean(uschange[,2]), 4))
```

## 5.5. Machine learning models with covariates

All ML models seen earlier can incorporate covariates.

### 5.5.1. Neural networks with covariates

```{r}
fit <- nnetar(
  uschange[,"Consumption"],
  xreg = uschange[,"Income"]
)
```

NNETAR automatically combines:
-   lagged target values,
-   seasonal lags,
-   external regressors.

### 5.5.2. Tree-based and kernel methods

For Random Forests, XGBoost, SVM: covariates are added as **extra columns** in the design matrix.

General structure: $x_{t+1} = f(x_t, \ldots, x_{t-T}, z_t, z_{t+1}$

## 5.6. Electricity demand case study (methodological pipeline)

This exercise illustrates **model escalation logic**.

### Step 1 - Baseline multivariate TSLM

```{r}
fit_lin <- tslm(
  Demand ~ Temperature + WorkDay + trend + season,
  data = train_data
)
```

Findings:
-   significant covariates,
-   **high residual autocorrelation** → model incomplete.

### Step 2 - Quadratic enhancement

```{r}
fit_quad <- tslm(
  Demand ~ Temperature + I(Temperature^2) + WorkDay + trend + season,
  data = train_data
)
```

Results:
-   captures **U-shaped temperature effect**,
-   strong RMSE reduction,
-   residual autocorrelation still present.

### Step 3 - Dynamic regression

```{r}
fit_arima <- auto.arima(
  train_data[,"Demand"],
  xreg = train_xreg
)
```
-   ARIMA captures remaining temporal structure,
-   better in-sample fit,
-   not necessarily better out-of-sample.

### Step 4 - NNETAR with covariates

```{r}
fit_nnet <- nnetar(
  train_data[,"Demand"],
  xreg = train_xreg
)
```
-   extremely flexible,
-   risk of overfitting,
-   not always superior to well-specified parametric models.

**Conclusion (electricity demand)**
> A parsimonious, physically-motivated quadratic TSLM outperformed more complex dynamic and machine-learning models on the test set.

## 5.7 Multivariate time series models (VAR)

### 5.7.1. Vector AutoRegressive model

For a multivariate series $X_t \in \mathbb{R}^k$: $X_t = c + A_1 X_{t-1} + \ldots + A_p X_{t-p} + \varepsilon_t$

Each variable depends on:
-   its own past,
-   the past of all other variables.

### 5.7.2. Cross-correlation analysis

```{r}
ccf(usconsumption[,"Consumption"],
    usconsumption[,"Income"])
```

Formal test:

```{r}
library(testcorr)
cc.test(usconsumption[,"Consumption"],
        usconsumption[,"Income"],
        max.lag = 1)
```

### 5.7.3. VAR order selection

```{r}
library(vars)
VARselect(us_app, lag.max = 8, type = "const")
```
-   AIC → more complex models,
-   BIC / HQ → more parsimonious models.

## 5.7.4. VAR estimation and diagnostics

```{r}
fit_var <- VAR(us_app, p = 5, type = "const", season = 4)
serial.test(fit_var, lags.pt = 10)
```
Residuals must be white noise.

## 5.7.5. VAR forecasting

```{r}
fc <- forecast(fit_var, h = 8)
plot(fc)
```

Comparison with univariate ARIMA often shows:
-   VAR helps when **cross-dependence exists**,
-   otherwise univariate models may outperform.

## 5.8. Granger causality

Granger causality tests whether one variable's past improves prediction of another.

```{r}
causality(fit_var, cause = c("Income","Production"))
```
Interpretation:
-   p-value < 0.05 → Granger causality present
-   p-value > 0.0.5 → no predictive gain from multivariate modeling

## 5.9. Key takeaways

### Regression-based models

-   excellent when **covariates are meaningful**
-   easy to interpret
-   weak if residual autocorrelation is ignored

### Dynamic regression

-   combines interpretability + time dependence
-   requires future covariates

### Machine learning with covariates

-   flexible
-   prone to overfitting
-   needs careful validation

### VAR models

-   powerful when variables truly interact
-   inefficient if relationships are weak
-   Granger tests guide model choice

# Final methodological rule

> Always start simple, diagnose residuals, and increase model complexity **only when statistically justified**.

