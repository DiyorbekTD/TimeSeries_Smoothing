# Time Series Smoothing (Moving Average, EWMA, Holt-Winters)

This repo is a **hands-on practice project** for smoothing and forecasting time series using:

- **Simple baseline**: mean (simple average)
- **Moving Average (MA)** smoothing
- **Exponentially Weighted Moving Average (EWMA)** (custom weighted version)
- **Single Exponential Smoothing (SES)** via `statsmodels`
- **Double Exponential Smoothing (Holt)** via `statsmodels`
- **Triple Exponential Smoothing (Holt–Winters)** via `statsmodels`

The main work lives in the notebook:

- `smoothing.ipynb`

---

## What this project does

1. **Generates synthetic time series** (stationary noise, trend, seasonality, and combinations).
2. **Visualizes** raw vs. smoothed series.
3. **Splits** a series into train/test (last 5 points in one example).
4. **Forecasts** the test window using multiple smoothing methods.
5. **Evaluates** models using **Sum of Squared Error** (named `mse` in the notebook).

---

## Methods + formulas 

### 1) Mean baseline (Simple Average)

Forecast every future value using the historical mean:

$$
\hat{y}_t = \bar{y} = \frac{1}{T}\sum_{i=1}^{T} y_i
$$

---

### 2) Error metric used in the notebook

The notebook function `mse(observations, estimates)` computes **sum of squared errors** (SSE):

$$
\text{SSE} = \sum_{t=1}^{n}(y_t - \hat{y}_t)^2
$$

> Note: Standard **MSE** usually means average squared error:
$$
\text{MSE} = \frac{1}{n}\sum_{t=1}^{n}(y_t - \hat{y}_t)^2
$$

If you want true MSE, divide the current output by `n`.

---

### 3) Moving Average (MA) smoothing

For window size \(k\), the moving average at time \(t\) is:

$$
\hat{y}_t = \frac{1}{k}\sum_{i=0}^{k-1}y_{t-i}
$$

In the notebook:
- A fast rolling sum is computed using cumulative sums.
- With `forecast=True`, the function pads the beginning with zeros for alignment.

---

### 4) Custom EWMA (weighted 3-point smoother)

The notebook’s `ewma()` uses fixed weights on the last 3 observations:

Let weights be:
$$
w = (w_1, w_2, w_3) = (0.160, 0.294, 0.543)
$$

Then for \(t \ge 3\):

$$
\hat{y}_t = w_1 y_{t-2} + w_2 y_{t-1} + w_3 y_t
$$

This is a **finite weighted moving average** (not the classic recursive EMA form).

> Classic recursive EMA (common definition) is:
$$
S_t = \alpha y_t + (1-\alpha)S_{t-1}
$$

---

### 5) Single Exponential Smoothing (SES)

SES is useful when the series has **no trend** and **no seasonality**.

Recursive form:
$$
S_t = \alpha y_t + (1-\alpha)S_{t-1}
$$

Forecast:
$$
\hat{y}_{t+h} = S_t
$$

### 6) Holt’s Linear Trend (Double Exponential Smoothing)
  Holt adds a trend component.
### 7) Holt–Winters (Triple Exponential Smoothing)
 Adds seasonality (additive form)


## What you should expect to see in outputs
Running the notebook produces:
1) Plots of raw series vs smoothed series  
2) Forecast plots over the test window  
3) A small summary table comparing error across methods (baseline vs SES vs Holt vs Holt–Winters)

This comparison highlights a key lesson:
- If the data has **trend**, SES will typically underperform Holt.
- If the data has **seasonality**, Holt–Winters should outperform non-seasonal methods.

---

## Requirements
Python packages used:
- `numpy`
- `pandas`
- `matplotlib`
- `seaborn`
- `statsmodels`
