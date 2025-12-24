## Phase 1: Data Collection and Stationarity Testing

In Phase 1, the top 50 cryptocurrencies by market capitalization were selected using the CoinMarketCap API. Daily OHLC price data was collected from Yahoo Finance for the period 2024-12-01 to 2025-12-01.

The Augmented Dickey-Fuller (ADF) test was applied to the closing price series of each cryptocurrency to determine stationarity.

Steps:
- Selected top 50 cryptocurrencies by market capitalization
- Collected daily OHLC data
- Applied ADF test on closing prices
- Separated stationary and non-stationary cryptocurrencies

---

## Phase 2: Hurst Exponent, Half-Life Estimation, and Visualization

In Phase 2, the stationary cryptocurrencies were analyzed to evaluate their mean-reverting behavior.

Steps:
- Computed the Hurst exponent for stationary cryptocurrencies
- Calculated the half-life of mean reversion
- Stored OHLC data in a structured dictionary (`main_dict`)
- Sorted assets by half-life
- Visualized price movements using candlestick charts

---

## Phase 3: Johansen Cointegration and Spread Construction

In Phase 3, non-stationary cryptocurrencies were analyzed using multivariate cointegration techniques.

Steps:
- Generated all 3-asset combinations using `itertools.combinations`
- Skipped combinations with insufficient data length or mismatched timestamps
- Applied the Johansen cointegration test
- Handled numerical issues such as singular matrices and invalid inputs
- Extracted and normalized cointegrating vectors
- Constructed cointegrated spreads
- Computed half-life for each spread
- Selected the top 5 spreads with the shortest half-life

---

## Phase 4: Mean-Reversion Trading Strategy Implementation and Backtesting

In Phase 4, a complete mean-reversion trading strategy was implemented and backtested on the selected cointegrated spreads obtained in Phase 3.

The strategy was applied to the **top 5 cointegrated spreads with the shortest half-life**, using daily data over the period **2024-12-01 to 2025-12-01**.

---

### 4.1 Spread Construction

For each selected spread:
- Three cryptocurrencies were combined using the **Johansen cointegration vector (β)**.
- A synthetic **spread OHLC series** was constructed as a linear combination of the individual asset OHLC prices:

  - Spread Open  = β₁·Open₁ + β₂·Open₂ + β₃·Open₃  
  - Spread High  = β₁·High₁ + β₂·High₂ + β₃·High₃  
  - Spread Low   = β₁·Low₁  + β₂·Low₂  + β₃·Low₃  
  - Spread Close = β₁·Close₁ + β₂·Close₂ + β₃·Close₃  

Only timestamps common to all three assets were used to avoid missing data.

---

### 4.2 Lookback Window Selection

The lookback window was dynamically determined based on the half-life of each spread:

- Lookback = 2 × Half-Life
- If (2 × Half-Life) < 30 days, the lookback was increased to the **smallest integer multiple of Half-Life ≥ 30**

This ensures statistical stability of the indicators while respecting the mean-reversion speed of each spread.

---

### 4.3 Indicator Calculation

Based on the latest amendment of the project specification:

- **EMA (Exponential Moving Average)** was calculated on the **spread close price**
- **Standard Deviation (STD)** was also calculated on the **spread close price**

Indicators:
- EMA = EMA(spread_close, lookback)
- STD = rolling_std(spread_close, lookback)

These indicators form the dynamic trading bands used for entry and exit signals.

---

### 4.4 Trading Rules

#### Entry Rules

**Short Positions**
- When spread **high > EMA + 1·STD**, enter a short position with **50% of capital**
- When spread **high > EMA + 2·STD**, add the remaining **50% of capital**

**Long Positions**
- When spread **low < EMA − 1·STD**, enter a long position with **50% of capital**
- When spread **low < EMA − 2·STD**, add the remaining **50% of capital**

Only one directional position (long or short) can be open at a time.

---

#### Exit Rules

- **Exit Short Position** when spread close price ≤ EMA
- **Exit Long Position** when spread close price ≥ EMA

Upon exit, the entire position is closed.

---

### 4.5 Position Management

- Progressive position sizing: 0 → 50% → 100%
- No leverage is assumed
- No transaction costs are applied at this stage
- Trades are evaluated on candle close prices

---

### 4.6 Backtesting Output

For each spread, the backtest logs:
- Entry and exit timestamps
- Trade type (FirstLong, SecondLong, FirstShort, SecondShort, Exit)
- Position size
- Trade sequence ID per spread

Trades are printed in chronological order and grouped by spread for clarity.

---

### 4.7 Key Assumptions and Limitations

- Perfect liquidity and execution at candle close prices
- No slippage or transaction fees
- Daily frequency only
- Strategy tested on a fixed historical window (1 year)

These assumptions will be relaxed in future phases.


---

## Future Work: Performance Evaluation and Visualization

The next step is to evaluate the performance of the implemented strategy.

Planned tasks:
- Compute **Sharpe Ratio** for each spread and for the overall strategy
- Calculate **Maximum Drawdown (Max-Drawdown)**
- Build and plot the **Equity Curve** to visualize strategy performance
- Report cumulative and periodic **returns**
- Compare performance across the 5 selected mean-reverting spreads

These metrics will provide a comprehensive assessment of the strategy’s risk-adjusted performance.
