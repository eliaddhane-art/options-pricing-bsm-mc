#  Options Pricing & Risk Analysis — Black-Scholes-Merton Model

A Python project that prices real equity options using the **Black-Scholes-Merton (BSM)** model, computes the Greeks, and visualizes the results with 3D surfaces, a volatility smile, and Monte Carlo simulations.

---

##  Overview

This project fetches **live market data** for a given ticker (e.g. AAPL), retrieves the nearest at-the-money call option, and compares the **theoretical BSM price** against the **real market price**. It also runs a **Monte Carlo simulation** to validate the BSM result and exposes the model's core limitation through the **volatility smile**.

---

##  Features

- **Live data** via `yfinance` — real spot price, strike, implied volatility, and expiry
- **BSM Pricing** — theoretical call price with dividend adjustment
- **Greeks** — Delta, Gamma, Theta
- **3D Surface Plots** — price and Greeks as a function of spot price and volatility
- **Volatility Smile** — IV across all strikes, highlighting BSM's constant-vol assumption
- **Monte Carlo Simulation** — 10,000 simulated paths to validate BSM pricing

---
## 📸 Visualizations

![Volatility smile](images/smile.png)
![Monte Carlo Paths](images/mc_paths.png)
![3D Greeks Surfaces](images/greeks_3d.png)








##  Project Structure

```
├── main.py         # Orchestration — runs the full analysis pipeline
├── engine.py       # BSM pricing engine — price, Greeks, Monte Carlo
├── marketData.py   # Live data fetching via yfinance
└── viz.py          # All visualizations — 3D surfaces, smile, MC paths
```

---

##  Installation

```bash
pip install yfinance numpy scipy matplotlib pandas
```

Then run:

```bash
python main.py
```

---

##  The Model

The **Black-Scholes-Merton** formula prices a European call option as:

```
C = S·e^(-qT)·N(d1) - K·e^(-rT)·N(d2)

d1 = [ln(S/K) + (r - q + σ²/2)·T] / (σ·√T)
d2 = d1 - σ·√T
```

Where:
- `S` = current spot price
- `K` = strike price
- `T` = time to expiration (in years)
- `r` = risk-free rate
- `q` = continuous dividend yield
- `σ` = implied volatility

---

##  Model vs. Market Price Gap

BSM and Monte Carlo both price the AAPL call at ~2.26$ vs. a market price of ~2.88$ — a gap of ~22%. This is expected and explained by several factors:

- **Volatility smile** — BSM uses a single IV for all strikes; the market prices each strike differently
- **Liquidity premium** — even using bid/ask midpoint rather than `lastPrice`, a gap remains; this reflects a residual risk premium that BSM cannot capture
- **Jump risk** — the market prices in the possibility of sudden moves that BSM ignores
- **Demand for protection** — OTM puts (and by put-call parity, calls) carry a risk premium that BSM does not capture

---

## ⚠️ Model Limitations

- **Constant volatility** — BSM assumes σ is constant across strikes and time, which the volatility smile directly contradicts
- **Simplistic volatility smile** — the smile displayed here is derived from yfinance implied volatilities which can be noisy (stale prices, low liquidity on OTM strikes). A proper smile would use bid/ask midpoints and filter illiquid strikes
- **No jumps** — the model assumes continuous price movements; in reality, stocks can gap (earnings, news)
- **European options only** — BSM cannot price American options which can be exercised early
- **Dividends** — only a continuous dividend yield approximation is used; discrete dividends are not modeled
- **Constant risk-free rate** — r is assumed fixed over the life of the option

---

## 🛠️ Built With

- [Python 3](https://www.python.org/)
- [yfinance](https://github.com/ranaroussi/yfinance)
- [NumPy](https://numpy.org/)
- [SciPy](https://scipy.org/)
- [Matplotlib](https://matplotlib.org/)
