# Volatility Targeting with Kalman Filters

This project explores a simple question:

> Can a more advanced model (Kalman Filter) improve volatility targeting compared to a simple benchmark (EWMA)?

---

## Motivation

Volatility is the key variable in many systematic strategies.

Instead of trying to predict returns, we:
- estimate volatility
- scale positions to keep risk constant

This is known as **volatility targeting**.

---

## Approach

### 1. Benchmark: EWMA

We start with a simple and robust model:

$$
\sigma_t = \sqrt{\lambda \sigma^2_{t-1} + (1 - \lambda) r_t^2}
$$

- Parameter chosen via **half-life = 36**
- Reason: prioritize **stability over precision**

---

### 2. Kalman Filter Model

We model log-volatility using a state-space framework:

\[
h_t = F h_{t-1} + \eta_t
\]
\[
y_t = \mu + H h_t + \epsilon_t
\]

Key idea:
- separate **short-term** and **long-term** volatility
- explicitly model **noise**

---

### 3. Volatility Targeting

Position sizing:

\[
contracts_t = \frac{V_{target}}{price_{t-1} \cdot \sigma_{t-1}}
\]

Key design choices:
- **Lagged inputs** → avoid lookahead bias
- **Inertia threshold (10%)** → reduce turnover
- Daily data → focus on robustness

---

### 4. Evaluation

We evaluate models using:

- **QLIKE** (forecast accuracy)
- **Sharpe Ratio**
- **Max Drawdown**
- **Turnover** (critical in practice)

---

## Results

### EWMA (Benchmark)

- Strong improvement in **risk control**
- Lower drawdowns across assets
- Stable behavior

Example (from backtest):
- BTC: Sharpe 0.56 → 0.79, MaxDD reduced significantly :contentReference[oaicite:0]{index=0}  

---

### Kalman Filter

- Slightly better **forecast accuracy (QLIKE)** :contentReference[oaicite:1]{index=1}  
- More reactive to market changes
- BUT:

> **Turnover explodes**

This leads to:
- higher trading costs
- less practical usability

---

## Key Insight

> **Better model ≠ better strategy**

- Kalman is **more precise**
- EWMA is **more robust**

In a volatility targeting framework:
- robustness matters more than precision

---

## My Decisions

This project is not just about models, but about choices:

- Chose **EWMA as benchmark** (not random)
- Fixed parameters instead of overfitting
- Used **QLIKE**, not MSE
- Focused on **turnover**, not only Sharpe
- Rejected Kalman for this use case despite better accuracy

Final conclusion:

> I prefer a simpler model that works in practice over a more complex one that looks better in theory.

---

## Takeaways

- Volatility targeting improves **risk profile**, not returns :contentReference[oaicite:2]{index=2}  
- Model complexity increases risk of **overfitting**
- Evaluation metrics must match the **use case**
- Financial models should be judged by **implementation cost**, not just accuracy

---

## Repository Structure

- `notebook.ipynb` → full implementation and results
- (optional) PDF → extended analysis

---

## Final Thought

> Don’t fall in love with a model.  
> Fall in love with what works.

This project is an example of that.

