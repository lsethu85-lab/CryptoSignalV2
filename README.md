# 🚀 ProSignal Dashboard – Crypto Trading Intelligence Platform

An **institution-grade crypto trading dashboard** designed for crypto swing traders.

This platform combines:
- Alpha signal discovery  
- Advanced Pro View trading analysis  
- Portfolio tracking  
- Risk & execution modeling  
- TradingView chart integration  

---

# 🧠 Core Features

## 🔍 Scanner Engine
- Real-time crypto market scanner
- Filters:
  - Liquidity (Market Cap, Volume, Velocity)
  - Momentum (EMA21, RSI, MACD)
  - Volume spike detection
- Actions:
  - Add to portfolio
  - Open chart
  - Open Pro View

---

## 🚀 Alpha Feed v2 (PASS-only setups)

Institution-grade signal engine.

### ✅ Mandatory gates:
- Market Cap ≥ $1B  
- Volume ≥ $50M  
- Velocity ≥ 2%  
- Price > EMA21 (4H)  
- RSI (4H) between 55–70  
- MACD up-cross ≤ 3 bars  
- TVL 7D ≥ +5%  
- ✅ Volume spike (required)

### 📊 Output:
- Entry Score (0–100)
- RSI Phase classification
- P(T1) probability
- PASS-only signals (high-quality setups)

---

## 🧠 Pro View (Full Trading Intelligence Panel)

### 📊 Charting
- TradingView 4H chart (tv.js)
- Auto exchange detection (Binance, Bybit, Coinbase, Kraken)

---

### 🎯 Entry & Targets
- Aggressive Entry
- Optimal Entry
- Safe Entry

- Target 1  
- Target 2  
- Target 3  

---

### ⚠️ Risk Model
- Stop loss calculation
- Risk/Reward ratio (T2)
- Trade invalidation logic

---

### 📈 Momentum Engine
- RSI phase classification
- MACD entry timing
- Volume spike confirmation

---

### 💡 Execution Plan
- Scale strategy:
  - 30% aggressive
  - 50% optimal
  - 20% breakout confirmation
- Profit-taking logic:
  - Trim at T1
  - Hold to T2/T3

---

### 💼 Position Sizing
- Account-based risk control
- Risk % input
- Real-time calculation:
  - Risk (USD)
  - Units
  - Position size

---

### 📊 Relative Strength
- BTC relative strength (proxy)

---

## 💼 Portfolio Dashboard

- Add/remove assets
- Per-coin:
  - Entry Score
  - Targets
  - Risk model
- Integrated actions:
  - Full chart
  - Pro View
  - Open news

---

## 🔔 Alert System

### Trigger Conditions:
- ✅ P(T1) crossing threshold
- ✅ RSI phase change

### Configuration:
- Enable / disable alerts
- Sound toggle
- Notification toggle
- Threshold (%)
- Cooldown (minutes)

---

# ⚙️ Architecture

## 🧩 Modular System

The app is separated into independent modules:
- Scanner Engine
- Alpha Feed v2
- Pro View
- Portfolio System

✅ Each module is isolated  
✅ No full UI crash even if one fails  

---

## 🛡 Stability Features

- Global JS error handling
- Async isolation (non-blocking UI)
- Modal cleanup logic (fixes TradingView freeze issue)
- Safe rendering cycle

```js
window.addEventListener('error', e => console.error(e));
window.addEventListener('unhandledrejection', e => console.error(e));