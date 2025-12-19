# Algorithmic Trading
## Phase 1: Cryptocurrency Stationarity Analysis

---

## Exercise Description

This exercise is part of the Algorithmic Trading course and focuses on the statistical analysis of cryptocurrency price series.

The main objective of the homework is to identify financial assets that exhibit **mean-reverting behavior**, which is a fundamental assumption in many quantitative trading strategies. As a prerequisite for detecting mean reversion, the **stationarity** of asset price series must be examined.

In this phase, the task is to analyze the **top 50 cryptocurrencies based on market capitalization** and determine which of them have **stationary daily price series** over a specified time period. Stationarity is evaluated using the **Augmented Dickey–Fuller (ADF) test** at a **95% confidence level**.

The results of this phase provide the statistical foundation for the next stages of the homework, including half-life estimation, Hurst exponent calculation, cointegration analysis, and trading strategy construction.

---

## Objective

- Select the **top 50 cryptocurrencies by market capitalization**
- Retrieve their **daily closing price data**
- Apply the **ADF test** to each price series
- Identify cryptocurrencies that are **stationary with probability greater than 95%**
- Report the stationary cryptocurrencies along with their **p-values**

---

## Data Sources

- **Market Capitalization Ranking:**  
  CoinMarketCap API (`/v1/cryptocurrency/listings/latest`)

- **Price Data:**  
  Yahoo Finance (accessed via the `yfinance` Python package)

---

## Time Range and Frequency

- **Start Date:** 2024-12-01  
- **End Date:** 2025-12-01  
- **Frequency:** Daily (1-day timeframe)

---

## Methodology

1. The top cryptocurrencies are retrieved based on market capitalization using the CoinMarketCap API.  
   To ensure that exactly **50 cryptocurrencies with valid price data** are analyzed, the top **70** cryptocurrencies are initially requested.

2. For each cryptocurrency, daily closing prices are downloaded from Yahoo Finance for the period **2024-12-01 to 2025-12-01**.

3. Cryptocurrencies are **automatically excluded** if:
   - No historical price data is available on Yahoo Finance
   - All retrieved closing prices are missing (`NaN`)

   The data collection process continues until **50 cryptocurrencies with valid price series** are obtained.

4. The Augmented Dickey–Fuller (ADF) test is applied to each valid price series.

5. The ADF test is **skipped** for:
   - Series with fewer than **30 observations** (insufficient data length)
   - Series with **constant or near-constant values**, which commonly occur in stablecoins

6. A cryptocurrency is classified as **stationary** if:
   - `p-value < 0.05`, corresponding to a 95% confidence level

All exclusions, validations, and statistical decisions are handled programmatically to ensure reproducibility and robustness of the analysis.

---

## Outputs

- List of cryptocurrencies excluded due to missing Yahoo Finance data
- List of cryptocurrencies skipped due to constant price behavior
- A dictionary containing ADF test statistics for stationary cryptocurrencies:
  - ADF statistic
  - p-value
  - 5% critical value
- Final output reporting **stationary cryptocurrencies and their p-values**

---

## Notes

- No manual or hard-coded exclusions are used.
- Stablecoins often exhibit constant price behavior, making the ADF test inapplicable.
- A verbosity flag (`VERBOSE`) is used to control debugging and intermediate output.
- This phase focuses solely on **stationarity testing**; no trading strategy is implemented at this stage.

---

## Technologies Used

- Python
- pandas
- numpy
- statsmodels
- yfinance
- CoinMarketCap API

---

## Next Steps

- Estimate the **half-life of mean reversion**
- Compute the **Hurst exponent**
- Identify cointegrated cryptocurrency pairs
- Construct and evaluate the trading strategy

---

**Phase 1 completed: Stationary cryptocurrencies identified using the ADF test**
