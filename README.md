# 📊 Market Sentiment & Trader Performance Analysis  
**Exploring the relationship between market mood and trader profitability**

![Python](https://img.shields.io/badge/Python-3.10+-blue)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Processing-green)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Visualization-orange)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

---

## 🚀 Project Overview

This project analyzes how **market sentiment (Fear/Greed Index)** correlates with **trader performance (PnL, trade size, behavior)** using data from *Bitcoin sentiment history* and *Hyperliquid trader execution records*.

The goal is to uncover:
- **How sentiment affects trader behavior**
- **When traders tend to perform better or worse**
- **Hidden behavioral patterns across sentiment states**
- **Insights that can support smarter trading strategies**

> **Core finding:**  
> **Traders take larger risks during Fear and earn selectively during Extreme Greed**,  
> while sentiment itself has weak predictive power on PnL — *strategy beats emotion*.

---

## 🎯 Objective

> **"Explore the relationship between market sentiment & trader performance, uncover hidden patterns, and derive insights to support smarter trading strategies."**

---


---

## 🧹 Data Preparation

```python
# convert columns to numeric
trades['Size USD'] = pd.to_numeric(trades['Size USD'], errors='coerce')
trades['Closed PnL'] = pd.to_numeric(trades['Closed PnL'], errors='coerce')

# convert timestamps
trades['date'] = pd.to_datetime(trades['Timestamp IST'], format='%d-%m-%Y %H:%M')
sentiment['date'] = pd.to_datetime(sentiment['timestamp'], unit='s')

# extract daily date
trades['date_only'] = trades['date'].dt.date
sentiment['date_only'] = sentiment['date'].dt.date

# aggregate daily pnl
daily_pnl = trades.groupby('date_only')['Closed PnL'].sum().reset_index()

# merge with sentiment
merged = pd.merge(sentiment[['date_only','value','classification']],
                  daily_pnl,
                  on='date_only',
                  how='inner')


