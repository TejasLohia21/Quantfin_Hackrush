# 📊 Curating Alphas: S&P 500 Portfolio Optimization using Technical Indicators & Modern Portfolio Theory

## 🧠 Overview

This project blends **technical indicators**, **statistical modeling**, and **Modern Portfolio Theory (MPT)** to systematically curate alpha-generating portfolios from the **S&P 500**. We leverage momentum-based indicators and optimize allocations using the **Efficient Frontier** framework to maximize returns while minimizing risk.

Our pipeline incorporates:
- Stock selection based on **EMA** and **RSI**
- Calculation of **expected returns**, **volatility**, **Sharpe Ratio**, and **Alpha**
- Soft constraints to ensure **diversification**
- Optimization using **PyPortfolioOpt**

---

## 📁 Dataset

- **Universe:** 488 Large-Cap S&P 500 stocks (2020–2024)
- **Data Format:** Daily OHLCV CSVs (one file per stock)
- **Data Source:** Yahoo Finance

---

## ⚙️ Methodology

### 📌 1. Feature Extraction

**Indicators Used:**
- **Exponential Moving Average (EMA)** – Momentum indicator
- **Relative Strength Index (RSI)** – Overbought/oversold indicator

**Other Parameters:**
- **Annualized Returns & Risk**
- **Expected Return via CAPM:**

  \[
  E(R_i) = R_f + \beta_i(R_m - R_f)
  \]

- **Sharpe Ratio:**

  \[
  \text{Sharpe} = \frac{R_i - R_f}{\sigma_i}
  \]

- **Alpha:** Excess return over CAPM

---

### 📌 2. Stock Selection

Each day:
- Stocks are ranked based on EMA & RSI logic.
- A **portfolio matrix** (with 0/1 values) indicates inclusion on that date.

---

### 📌 3. Optimization via Efficient Frontier

Using **PyPortfolioOpt**, we:
- Extract past 30-day price data
- Compute:
  - **Expected returns** (mean historical returns)
  - **Covariance matrix** (sample covariance)
- Apply `min_volatility()` optimization to get daily allocations
- Clean and store weights in a matrix (date × ticker)

---

### 📌 4. Soft Forcing (Min 20 Stocks Rule)

To ensure diversification:
- If fewer than 20 stocks pass selection criteria,
- We **append top fallback stocks** using:
  - Highest RSI
  - Largest price deviation from 50-day moving average

This helps maintain portfolio breadth without compromising selection quality.

---

## 🧮 Mathematical Background

- **Expected Return**  
  $$
  \mu_i = \frac{1}{N} \sum_{t=1}^{N} R_{i,t}
  $$

- **Covariance Matrix**  
  $$
  \Sigma = \text{Cov}(R)
  $$

- **Efficient Frontier Optimization**  
  $$
  \min_w \quad w^\top \Sigma w \quad \text{subject to} \quad \sum_i w_i = 1
  $$

- **Sharpe Maximization or Min Volatility (our choice):**

  ```python
  weights = ef.min_volatility()
