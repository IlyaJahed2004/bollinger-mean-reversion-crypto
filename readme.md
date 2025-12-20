# Cryptocurrency Analysis

## Phase 1: Cryptocurrency Data Collection and ADF Test

In **Phase 1**, we focused on collecting the **top 50 cryptocurrencies** based on their market capitalization using the **CoinMarketCap API**. We then performed the **Augmented Dickey-Fuller (ADF) test** to identify stationary time series (cryptocurrencies that exhibit stable long-term behavior).

### Steps in Phase 1:
- Retrieved the **top 50 cryptocurrencies** using the CoinMarketCap API.
- Collected **daily closing price data** from Yahoo Finance (covering the period from 2024-12-01 to 2025-12-01).
- Performed the **ADF test** on each cryptocurrency's price series to identify which series are **stationary** (p-value < 0.05).

This phase helped identify **stationary cryptocurrencies**, which were then analyzed in **Phase 2** for their **Hurst exponent** and **half-life** values.

---

## Phase 2: Hurst Exponent, Half-Life Calculation, and Plotting Candlestick Charts

In **Phase 2**, the focus shifted to the stationary cryptocurrencies identified in Phase 1. Specifically, we implemented the **Hurst exponent** and **half-life** calculations for each stationary cryptocurrency, then visualized their price movements using **candlestick charts**. Additionally, we created a dictionary to store the OHLC data of each cryptocurrency, which facilitated plotting and analysis.

### Key Updates to Phase 1:
- **Phase 1 Code Update**: I modified the code in **Phase 1** to create a dictionary (`main_dict`) where the **key** is the cryptocurrency symbol and the **value** is the corresponding **OHLC data**.
  
  - This was necessary to efficiently manage the data required for **Phase 2** and to store the OHLC data for each cryptocurrency in a structured way.

  - Additionally, I ensured that the **cryptocurrencies were sorted** based on their **half-life** (calculated in Phase 2) to prioritize those with the shortest mean-reversion times.

### Steps in Phase 2:

1. **Hurst Exponent Calculation**:
   - The **Hurst Exponent** was computed for each stationary cryptocurrency to assess the **long-term memory** of the time series:
     - A **H < 0.5** indicates **mean-reversion** behavior.
     - A **H > 0.5** indicates **trending behavior**.

2. **Half-Life Calculation**:
   - The **Half-Life** for mean-reversion was calculated for each stationary cryptocurrency, which measures how quickly the impact of a shock decays over time.
   - The cryptocurrencies were sorted based on **half-life** to visualize them in terms of their **mean-reversion speed**.

3. **Candlestick Charts**:
   - **Candlestick charts** were created for each cryptocurrency, ordered by their **half-life** values, to visualize the price movements over time.

### Outputs of Phase 2:
- **Hurst Exponent** and **Half-Life** values were calculated and displayed for each stationary cryptocurrency.
- **Candlestick charts** were generated for each cryptocurrency, ordered by their **half-life** values (from shortest to longest).
- **A dictionary (`main_dict`)** was created where the key is the cryptocurrency symbol, and the value is the **OHLC data**, making it easier to manipulate the data for further analysis and visualization.

### Code Changes:
- **Phase 1 Changes**: The **Phase 1 code** was slightly updated to allow storing the **OHLC data** in a dictionary format (`main_dict`) for each cryptocurrency.
- The loop for collecting data was adjusted to ensure that the cryptocurrencies were only added if they had valid price data, and they were sorted based on **half-life** in Phase 2.
- **Sorting by Half-Life**: After calculating the **half-life** for each stationary cryptocurrency, they were sorted in ascending order (from shortest to longest half-life).

---

## Future Work:

1. **Non-Stationary Cryptocurrencies**: 
   - Analyze **non-stationary cryptocurrencies** by applying the **Johansen Cointegration Test** to explore relationships between them.

2. **Cointegration Models**:
   - Estimate the **cointegration coefficients** and use **Vector Error Correction Models (VECM)** to model and forecast their price movements.

3. **Long-Term Relationships**:
   - Investigate potential **long-term relationships** between cryptocurrencies for forecasting or **arbitrage strategies**.

