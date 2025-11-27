# Johansen Cointegration Crypto Pairs Backtesting

This project implements a **pairs trading strategy on cryptocurrencies** using:

- The **Johansen cointegration test** to identify long-run equilibrium relationships between asset prices  
- A **Kalman Filter–based dynamic hedge ratio** for modeling time-varying relationships  
- A **Z-score–based trading rule** to enter/exit long–short positions  
- A lightweight **backtesting engine** to evaluate strategy performance on hourly crypto data  

The main analysis focuses on BTC–ETH, BTC–LTC, ETH–LTC, ADA–DOT, and other crypto pairs using data pulled from **Binance US** via `ccxt`.

---

## ✨ Project Overview

Cryptocurrency markets frequently show **short-term mispricings** between correlated assets.  
This project builds a systematic pairs-trading pipeline to exploit those deviations:

1. **Hourly price data collection** (Binance US → CCXT)  
2. **Johansen cointegration testing** to filter out valid trading pairs  
3. **Kalman Filter** to compute a *dynamic hedge ratio (βₜ)* instead of fixed OLS β  
4. **Spread construction and Z-score standardization**  
5. **Mean-reversion trading logic** using a ±2σ threshold  
6. **Backtesting & performance evaluation**  

Pairs like **BTC–ETH** and **DOT–ADA** demonstrate strong and stable mean-reversion behavior.

---

## 📁 Repository Structure

```text
.
├── .github/workflows/        # GitHub Actions workflow(s) for packaging / CI
├── notebook/                 # Jupyter notebook(s) with full E2E analysis
│   └── johansen_test_backtesting.ipynb
├── src/                      # Python source files (Johansen, Kalman Filter, Strategy)
│   ├── johansen.py
│   ├── kalman_filter.py
│   ├── trading_rules.py
│   ├── backtester.py
│   └── utils.py
├── .gitignore
├── LICENSE
└── README.md                 # You are here
```

---

## 📊 Methodology

### **1. Johansen Cointegration Test**
Used to determine whether two assets maintain a **long-run equilibrium** relationship.

**Decision Rule:**  
`Trace Statistic > Critical Value (5%) → Cointegrated`

Example Results:

| Pair      | Trace Stat | Critical Value | Result         |
|-----------|------------|----------------|----------------|
| BTC–ETH   | 23.17      | 15.49          | Cointegrated   |
| BTC–LTC   | 30.84      | 15.49          | Cointegrated   |
| ETH–LTC   | 34.47      | 15.49          | Cointegrated   |
| DOT–ADA   | 36.41      | 15.49          | Cointegrated   |
| XRP–XLM   | 13.06      | 15.49          | Not Cointegrated |

**Why Johansen?**
- No need to choose dependent/independent variable  
- Robust for multi-asset systems  
- Extracts long-term equilibrium vectors directly  

---

### **2. Dynamic Hedge Ratio (Kalman Filter)**

We estimate the relationship:

\[
Y_t = a_t + b_t X_t + e_t
\]

The Kalman Filter:
- Updates the hedge ratio `(bₜ)` at each timestep  
- Adapts to changing market regimes  
- Produces a smoother response compared to static OLS  

Observations:
- BTC–ETH and ETH–LTC → **stable co-movement**  
- BTC–LTC → **regime shifts**  
- DOT–ADA → **strong correlation**  

---

### **3. Z-Score Trading Logic**

Spread:

\[
s_t = Y_t - (a_t + b_t X_t)
\]

Z-score:

\[
z_t = \frac{s_t - \mu_t}{\sigma_t}
\]

**Trading Rules:**
- **Long (Y), Short (X)** when `z < -2`  
- **Short (Y), Long (X)** when `z > +2`  
- **Exit** when `z` crosses 0  

This captures short-term mean reversion around the equilibrium spread.

---

## 🧪 Backtesting Results (Hourly)

| Pair      | Profit / Loss % |
|-----------|------------------|
| **DOT–ADA** | **24.48%** |
| **BTC–ETH** | **23.52%** |
| **ETH–LTC** | **11.84%** |
| **BTC–LTC** | **7.46%** |
| XRP–XLM     | N/A (not cointegrated) |

**Key Takeaway:**  
Pairs that pass Johansen and maintain a stable Kalman hedge ratio are the most profitable.

---

## 🚀 Future Enhancements

- Rolling (60-day) Johansen cointegration windows  
- Automated universe expansion (SOL–BNB, ADA–DOT, etc.)  
- Volatility-adjusted position sizing  
- Real-time deployment (websocket feeds + execution layer)  

---

## 📚 References

- Fischer, Krauss & Deinert (2019). *Statistical Arbitrage in Cryptocurrency Markets*  
- Avellaneda & Lee (2010). *Statistical Arbitrage in U.S. Equities*  

