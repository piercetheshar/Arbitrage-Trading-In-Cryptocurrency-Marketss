# Johansen Cointegration Crypto Pairs Backtesting

This project implements a **pairs trading strategy on cryptocurrencies** using:

- The **Johansen cointegration test** to check if two assets move together in the long run  
- A **Kalman Filter–based dynamic hedge ratio**
- A **Z-score–based trading rule** for entering and exiting long/short positions
- A simple **backtesting engine** to evaluate performance

The example notebook focuses on a BTC–ETH (and other) crypto pair using data pulled from **Binance US** via `ccxt`.

---

## Repository Structure

```text
.
├── .github/workflows/        # GitHub Actions workflow(s) for packaging / CI
├── notebook/                 # Jupyter notebook(s) with full E2E analysis
│   └── johansen_test_backtesting.ipynb
├── src/                      # Python source files (library-style code)
├── .gitignore
├── LICENSE
└── README.md                 # You are here
