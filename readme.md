# Cryptocurrency Time Series Analysis

This project analyzes cryptocurrency price time series using statistical methods to identify stationarity, mean reversion, and cointegration relationships, with the ultimate goal of building a statistical arbitrage trading strategy.

---

## Phase 1: Data Collection and Stationarity Testing

In this phase, the top 50 cryptocurrencies by market capitalization were collected using the CoinMarketCap API. Daily OHLC price data was downloaded from Yahoo Finance for the period **2024-12-01 to 2025-12-01**.

The Augmented Dickey-Fuller (ADF) test was applied to the closing price of each cryptocurrency to determine whether the series is stationary.

### Key Points
- Stationary series were identified using **ADF test (p-value < 0.05)**.
- A dictionary called `main_dict` was created:
  - **Key**: cryptocurrency symbol  
  - **Value**: OHLC price DataFrame
- This structure was reused in all subsequent phases.

---

## Phase 2: Hurst Exponent, Half-Life, and Visualization

In Phase 2, stationary cryptocurrencies identified in Phase 1 were analyzed further.

### Methods
- **Hurst Exponent** was computed to measure long-term dependence:
  - `H < 0.5`: mean-reverting behavior  
  - `H > 0.5`: trending behavior
- **Half-life of mean reversion** was estimated using an AR(1) framework.
- Cryptocurrencies were sorted based on their half-life values.

### Visualization
- Candlestick charts were generated for stationary cryptocurrencies.
- Charts were ordered from shortest to longest half-life to highlight faster mean-reverting assets.

---

## Phase 3: Cointegration of Non-Stationary Cryptocurrencies

Phase 3 focuses on identifying stationary linear combinations of **non-stationary** cryptocurrencies using the **Johansen cointegration test**.

### Methodology
- Stationary cryptocurrencies were excluded from this phase.
- All **3-asset combinations** were generated using `itertools.combinations`.
- Combinations were skipped if:
  - Time series lengths were inconsistent
  - Numerical issues occurred (e.g., singular matrices, NaNs)
- The Johansen test was applied at the **95% confidence level**.
- The strongest cointegrating vector was selected and normalized.
- Cointegrated spreads were constructed as linear combinations of prices.
- The **half-life of each spread** was calculated.
- The **5 spreads with the shortest half-life** were selected.

### Visualization
For each selected spread:
- Each cryptocurrency price series was plotted separately for clarity.
- The cointegration spread was plotted using a candlestick chart.
- Plots were stacked vertically for clear visual interpretation.

---

## Future Work: Trading Strategy Implementation

The next step of this project is to **implement and evaluate a statistical arbitrage trading strategy** based on the cointegrated spreads identified in Phase 3.

Planned extensions include:
- Defining **entry and exit rules** based on spread deviations (e.g., z-score thresholds).
- Using **half-life** to set optimal holding periods.
- Position sizing based on cointegration weights.
- Backtesting the strategy with realistic assumptions (transaction costs, slippage).
- Evaluating performance using metrics such as Sharpe ratio, drawdown, and win rate.

---
