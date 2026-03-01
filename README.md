# Options Pricing & Risk Analysis — Black-Scholes-Merton Model

A Python project that prices real equity options using the **Black-Scholes-Merton (BSM)** model, computes the Greeks, and visualizes the results with interactive 3D surfaces, a volatility smile, and Monte Carlo simulations.

##  Overview

This project fetches **live market data** for a given ticker (e.g. AAPL), retrieves the nearest at-the-money call option, and compares the **theoretical BSM price** against the **real market price**. It also runs a **Monte Carlo simulation** to validate the BSM result and exposes the model's core limitation through the **volatility smile**.

## Features

- **Live data** via `yfinance` — real spot price, strike, implied volatility, and expiry
- **BSM Pricing** — theoretical call price with dividend adjustment
- **Greeks** — Delta, Gamma, Theta
- **3D Surface Plots** — price and Greeks as a function of spot price and volatility
- **Volatility Smile** — IV across all strikes, highlighting BSM's constant-vol assumption
- **Monte Carlo Simulation** — 10,000 simulated paths to validate BSM pricing
  
## Project Structure

├── main.py         # Orchestration — runs the full analysis pipeline
├── engine.py       # BSM pricing engine — price, Greeks, Monte Carlo
├── marketData.py   # Live data fetching via yfinance
└── viz.py          # All visualizations — 3D surfaces, smile, MC paths

## Installation
```bash
pip install yfinance numpy scipy matplotlib pandas
```
Then run:
```bash
python main.py

## The Model

The **Black-Scholes-Merton** formula prices a European call option as:

C = S·e^(-qT)·N(d1) - K·e^(-rT)·N(d2)

d1 = [ln(S/K) + (r - q + σ²/2)·T] / (σ·√T)
d2 = d1 - σ·√T

Where:
- `S` = current spot price
- `K` = strike price
- `T` = time to expiration (in years)
- `r` = risk-free rate
- `q` = continuous dividend yield
- `σ` = implied volatility

## Key Insight — Model vs. Market

BSM and Monte Carlo converge to the same price (~2.26$), yet both **underestimate the real market price** (~2.88$). The volatility smile explains why: the market prices different levels of implied volatility across strikes, while BSM assumes a constant volatility. This is the fundamental limitation of the model.

##  Visualizations

| 3D Greeks Surfaces | Volatility Smile | Monte Carlo Paths |
|---|---|---|
| Price, Delta, Gamma, Theta as a function of spot & vol | IV across strikes showing the skew | 200 simulated price paths of the underlying |

##  Built With

- [Python 3](https://www.python.org/)
- [yfinance](https://github.com/ranaroussi/yfinance)
- [NumPy](https://numpy.org/)
- [SciPy](https://scipy.org/)
- [Matplotlib](https://matplotlib.org/)
