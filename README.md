# 🚀 Crypto Momentum Dashboard

> **Velocity Radar · Narrative Heatmap · Portfolio Intelligence Cards · Laggard Watch**

A fully static, single-file crypto research dashboard that runs in any browser with zero backend, zero build step, and zero API keys required. Drop `index.html` anywhere — GitHub Pages, Netlify, or just open it locally — and it works.

---

## ✨ Features at a Glance

| Feature | What it does |
|---|---|
| **Scanner** | Fetches top 50–200 coins from CoinGecko, scores them 0–100, and ranks them by momentum |
| **Narrative Heatmap** | Groups coins into sectors and scores each sector's strength so you see where money is rotating |
| **Velocity Radar** | Highlights coins where volume, price action, sentiment, and RS vs BTC all align at once |
| **Portfolio Intelligence Cards** | Per-coin live intelligence (sentiment, RS 4H vs BTC, volume, velocity) + automated trade levels (Entry, TP, SL, R/R) |
| **Laggard Watch** | Finds coins that haven't moved yet inside sectors that are already running |
| **Sector Filter** | Click any sector tile to see only the top 5 momentum leaders in that narrative |
| **Export CSV** | Download the full scanner result set for offline analysis |
| **Dark / Light theme** | Persisted in localStorage |
| **Auto-refresh** | Optional 15-minute auto-refresh keeps data fresh without manual intervention |

---

## 🗂 Project Structure

```
index.html          ← The entire app — HTML, CSS (Tailwind CDN), and JS in one file
README.md           ← This file
```

No `node_modules`. No `package.json`. No build pipeline. Everything ships in `index.html`.

---

## 🔌 Data Sources

| Source | What's fetched | Rate limit (free tier) |
|---|---|---|
| [CoinGecko v3 API](https://www.coingecko.com/en/api) | Market prices, 24H change, volume, OHLC, market charts | ~30 requests / minute |
| [DeFiLlama API](https://defillama.com/docs/api) | TVL data for DeFi protocols | No strict limit |

No API keys are needed. Both APIs are called directly from the browser using public free-tier endpoints.

> **Tip — Rate limits:** The portfolio tab batches all coins into a single `/coins/markets` call and then enriches each coin sequentially with a 650 ms pause, keeping well within CoinGecko's free-tier limit even with 10+ coins in your portfolio.

---

## 📊 Engine Score (0–100)

Each coin is given a composite momentum score built from seven signals:

| Signal | Points | Logic |
|---|---|---|
| Price above EMA 20 | +20 | Bullish structure |
| RS 4H vs BTC > 0 | +18 | Outperforming Bitcoin on 4H |
| Volume increase > 50% | +18 | Unusual buying activity |
| Sentiment score > 70 | +18 | Positive price-action sentiment |
| Has DeFi TVL | +10 | Protocol is active and used |
| 24H price change > 0 | +10 | Positive near-term momentum |
| RSI ≤ 78 | +6 | Not deeply overbought |

**Score guide:** ≥ 70 = strong momentum · 45–69 = watch list · < 45 = weak / bearish

---

## 🎯 Portfolio Intelligence Cards

When you add a coin to **My Portfolio**, the card fetches and displays two panels:

### Live Intelligence Panel

| Metric | How it's calculated |
|---|---|
| **Sentiment Pulse** | `clamp(50 + price_change_24h × 4, 0, 100)` — scaled to 0–100. ≥ 70 = Bullish, < 40 = Risk-Off |
| **RS 4H vs BTC** | Asset 4H return minus BTC 4H return. Positive = outperforming Bitcoin |
| **Volume Increase %** | Compares latest 24H volume to the volume 25 periods ago from the market chart |
| **Velocity Radar** | Active when volume > 50%, price > EMA 20, sentiment > 70, and RS vs BTC > 0 simultaneously |

### Automated Trade Levels Panel

The dashboard implements a **confluence-based TP/SL framework**:

```
Entry     = Current market price

Target TP = max(
              Fib 1.272 extension from last swing range,
              Entry + 2.5 × ATR
            )
            — always capped above Entry

Stop Loss = min(
              Recent swing low (last 20 candles),
              Entry − 1.5 × ATR
            )
            — always capped below Entry
            — tightens to break-even buffer if sentiment < 40

Risk/Reward = (Target − Entry) / (Entry − Stop)
```

**ATR (Average True Range)** is calculated from 30 days of OHLC data (14-period). If the OHLC endpoint is unavailable, ATR falls back to `4% of current price` using a proxy derived from the market chart.

**Fibonacci levels** are computed from the 20-candle swing low/high range. The 1.272 Fib extension acts as an aggressive TP that catches extended moves.

---

## 📐 Trading Framework Reference

The trade levels follow a multi-confluence approach. Use these rules together — never rely on a single indicator.

### 1. Sentiment Filter (Go / No-Go)

Check the Fear & Greed Index before entering any trade:

| Reading | Interpretation | Action |
|---|---|---|
| < 20 — Extreme Fear | Market oversold | Look for **long** entries |
| 20–80 — Neutral / Greed | Normal conditions | Use technical signals |
| > 80 — Extreme Greed | Market overbought | Tighten stop losses or look for **shorts** |

**Volume confirmation:** sentiment must be backed by rising volume. If price rises but volume falls, the move is suspect and a reversal is likely.

---

### 2. Entry Confluence (3-Signal Rule)

Wait for **all three** to align on the 4H or Daily timeframe:

**A. Bollinger Bands**
- Long entry: price touches or wicks outside the **Lower Band**
- Short entry: price touches or wicks outside the **Upper Band**

**B. RSI**
- Oversold: RSI < 30 (long signal)
- Overbought: RSI > 70 (short signal)
- **RSI Divergence** (price makes lower low but RSI makes higher low) = strongest long signal

**C. Fibonacci**
- Price sitting on the **Golden Pocket** (0.5 or 0.618 retracement) = high-probability entry zone

---

### 3. Stop Loss Placement

> Place your SL where the trade idea is *officially proven wrong*.

| Method | Rule |
|---|---|
| **Wick-Plus Rule** | 1–2% below the recent swing low (longs) or above swing high (shorts) |
| **ATR Method** | Entry ± (2 × ATR) — protects against being stopped out by normal volatility noise |

---

### 4. Take Profit — Scale Out in Stages

Never exit all at once. Use three levels:

| Level | Target | Notes |
|---|---|---|
| **TP 1 — Conservative** | Bollinger Band Middle (20 SMA) | Take 30–40% off the table here |
| **TP 2 — Technical** | Upper Bollinger Band or 0.382/0.236 Fib extension | Take another 40% here |
| **TP 3 — Moonshot** | Previous major daily resistance | Let the final 20% run |

---

### 5. Chart Pattern Confluence

| Pattern | Technical Alignment | Signal |
|---|---|---|
| Double Bottom | 0.618 Fib + RSI oversold | Long entry |
| Head & Shoulders | Upper BB + declining volume | Short entry |
| Bull Flag | Consolidates at Middle BB + neutral RSI | Trend continuation long |

---

### 6. Risk Management Rules

| Rule | Detail |
|---|---|
| **Risk/Reward minimum** | Never enter a trade with less than 1:2 R/R (profit must be ≥ 2× the risk) |
| **1% Rule** | Never risk more than 1% of total capital on a single trade |
| **Trailing Stop** | Once TP 1 is hit, move SL to break-even — the trade becomes risk-free |

> **Example:** $1,000 portfolio → maximum loss per trade = $10.
> If your SL is $0.05 away from entry, maximum position size = 200 units.

---

## 🔍 Velocity Radar

The Velocity Radar badge appears on coins where **four conditions fire simultaneously**:

```
Volume Increase > 50%
AND Price > EMA 20
AND Sentiment Score > 70
AND RS 4H vs BTC > 0%
```

This is designed to catch early-stage breakouts before they become obvious on the chart. Toggle the **Velocity Radar** button in the scanner to filter to only these candidates.

---

## 🗺 Narrative Heatmap

The heatmap groups all scanned coins into sectors and scores each sector:

```
Sector Strength Score = clamp(50 + (avg top-10 24H%) × 5, 0, 100)
```

- **Click any sector tile** to filter the scanner to the top 5 coins in that sector by Engine Score
- Sector tiles show: coin count, velocity candidate count, average 24H performance, and the sector leader
- Use **Find Laggards** to automatically surface coins that haven't moved yet inside a strong sector

### Laggard Rule

A coin is flagged as a laggard when:
- Sector strength ≥ 55
- Sector leader 24H move > 1%
- Candidate 24H move < `max(2%, leader × 0.45%)` (has not pumped yet)
- Engine Score ≥ 25 (not a dead coin)
- RS 4H vs BTC > −5% (not in complete collapse)

---

## 🚀 Deployment

### GitHub Pages (recommended)

```bash
# 1. Create a new repo (or use an existing one)
git init
git add index.html README.md
git commit -m "Initial commit"
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
git push -u origin main

# 2. Go to repo Settings → Pages → Source: main branch → / (root)
# 3. Your dashboard will be live at:
#    https://YOUR_USERNAME.github.io/YOUR_REPO/
```

### Local (instant)

Just open `index.html` in Chrome, Firefox, or any modern browser. No server needed.

### Netlify / Vercel

Drag and drop the folder containing `index.html` into the Netlify or Vercel dashboard. Done.

---

## ⚙️ Configuration

All user-configurable options live in the scanner toolbar inside the app — no code editing needed:

| Setting | Default | Description |
|---|---|---|
| Rows scanned | Top 100 | How many coins to fetch from CoinGecko |
| Rank limit | 200 | Hide coins ranked below this number |
| Min market cap | 0 | Filter out small caps |
| Min volume increase | 0% | Minimum volume spike to show |
| Min score | 0 | Minimum Engine Score to show |
| Show bearish movers | ✅ On | Include coins with negative 24H change |
| Only projects with TVL | ☐ Off | Show only DeFi protocols with TVL data |
| Auto-refresh | ✅ On | Refresh scanner every 15 minutes |

---

## 🛠 Tech Stack

| Layer | Technology |
|---|---|
| UI framework | [Tailwind CSS](https://tailwindcss.com/) (CDN) |
| Icons | [Font Awesome 6](https://fontawesome.com/) (CDN) |
| Charts | [Lightweight Charts v4](https://tradingview.github.io/lightweight-charts/) by TradingView |
| Data | CoinGecko REST API v3 · DeFiLlama REST API |
| Storage | `localStorage` (portfolio persists across sessions) |
| Runtime | Vanilla JavaScript (ES2022) — no frameworks, no bundler |

---

## 📝 Known Limitations

- **CoinGecko free tier:** ~30 requests/minute. With many portfolio coins, enrichment is sequential with a 650 ms gap per coin to stay within limits. If you see "Estimates shown", the API returned an error and calculated values are being used instead.
- **No server-side caching:** Every page load fetches fresh data. Heavy use may temporarily exhaust the free-tier rate limit.
- **OHLC data:** Available for most top-200 coins. Less-liquid coins may fall back to the volatility proxy for ATR/swing levels.
- **Sentiment score:** Currently a proxy derived from price action (`50 + 24H_change × 4`), not a live social sentiment feed.

---

## 📄 License

MIT — free to use, fork, and modify.

---

*Built for independent crypto researchers. Not financial advice. Always do your own research.*
