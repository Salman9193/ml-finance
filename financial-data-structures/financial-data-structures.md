# Financial Data Structures & Pitfalls

---

## Table of Contents
1. [From Financial Ratios to ML](#1-from-financial-ratios-to-ml)
2. [Time-Series Data](#2-time-series-data)
3. [Cross-Sectional Data](#3-cross-sectional-data)
4. [Panel (Longitudinal) Data](#4-panel-longitudinal-data)
5. [Non-Stationarity & Market Regimes](#5-non-stationarity--market-regimes)
6. [Look-Ahead Bias](#6-look-ahead-bias)
7. [Survivorship Bias](#7-survivorship-bias)
8. [Worked Examples — Ratios Through the Data Lens](#8-worked-examples--ratios-through-the-data-lens)
9. [Best Practices Checklist](#9-best-practices-checklist)

---

## 1. From Financial Ratios to ML

Financial ratios (current ratio, ROE, D/E, asset turnover) are **static snapshots** of a firm at a point in time. They summarise accounting data into a single number. ML models, however, are hungry for **sequences** (how a ratio evolved over 8 quarters) and **cross-sections** (how 500 firms compare on the same ratio today).

The gap between the two is where most ML failures in finance live — not in the model architecture, but in how the data was structured before the model ever saw it.

| View | What it captures | What ML needs |
|------|-----------------|---------------|
| Single ratio, one year | Static snapshot | Time series of the same ratio |
| Same ratio, many firms | Cross-sectional rank | Panel: entity × time |
| Ratio at filing date | Accounting period end | Point-in-time availability date |

**Core principle:** Before tuning a model, ask whether the data structure is correct. A perfectly specified XGBoost on badly structured data will still fail.

---

## 2. Time-Series Data

### What it is
Observations for **one entity** indexed by time.

```
Titan Current Ratio:
Q1-FY22: 1.8
Q2-FY22: 1.6
Q3-FY22: 1.9
Q4-FY22: 1.5
Q1-FY23: 1.4
...
```

### Key properties
| Property | What it means | ML implication |
|----------|--------------|----------------|
| **Autocorrelation** | Today's value predicts tomorrow's | Naive i.i.d. assumption is violated |
| **Volatility clustering** | Calm periods and turbulent periods cluster | GARCH-type models needed for vol |
| **Non-stationarity in levels** | Mean and variance drift | Use returns/changes, not price levels |
| **Structural breaks** | The series shifts permanently at a point | A single model trained across the break may be wrong |

### Common ML tasks on time-series
- Return forecasting
- Volatility prediction
- Liquidity and volume modelling
- Regime detection

### Common pitfalls
1. **Random train-test split** — splitting at random rows destroys temporal order. The model "sees the future" via adjacent rows in training.
2. **Shuffling observations** — same problem as above.
3. **Leaking future information via rolling windows** — if a 12-month rolling mean at time *t* includes months *t+1* to *t+6*, the feature leaks.
4. **Ignoring temporal dependence** — treating quarterly ratios as independent observations when they are strongly autocorrelated.

**Correct approach:** Always use a time-based split. Train on data up to date *T*, validate on *T+1* to *T+k*, test on everything after.

```
Timeline:  |-------- Train --------|-- Val --|-- Test --|
                2015-2021              2022     2023-24
```

---

## 3. Cross-Sectional Data

### What it is
Observations across **many entities at one point in time**.

```
Nifty 500 firms, Q4-FY25:
Titan        ROE = 28.3%
D-Mart       ROE = 17.1%
Infosys      ROE = 30.2%
HDFC Bank    ROE = 16.8%
...
```

### Key properties
- **Relative comparisons dominate** — what matters is whether Titan's ROE is above or below its sector median, not the absolute number.
- **Market-wide shocks affect all entities** — in a crash, every stock falls. Cross-sectional residuals are correlated (common factor exposure).

### Common ML tasks
- Stock ranking and alpha selection
- Credit scoring (compare applicants against a population)
- Relative value strategies
- Peer comparison models

### Common pitfalls

| Pitfall | Example | Fix |
|---------|---------|-----|
| Using absolute levels instead of ranks | ROE = 28% vs ROE rank = top decile | Rank or winsorise features within the cross-section |
| Improper normalisation | Z-scoring using full-sample mean/std | Normalise using only past data available at that date |
| Hidden look-ahead through universe selection | Using today's Nifty 500 members to build a 2015 cross-section | Use point-in-time index membership |

**Why ranks beat levels:** Titan's ROE of 28% in FY25 sounds high. But sector context matters — in a high-inflation year where peers earned 32%, it is below median. Ranks are stable and comparable; raw levels are regime-dependent.

---

## 4. Panel (Longitudinal) Data

### What it is
**Multiple entities observed over time** — the combination of time-series and cross-section. Most realistic financial datasets are panels.

```
Panel structure (firm × quarter):
             Q1-FY23  Q2-FY23  Q3-FY23  Q4-FY23
Titan          1.8      1.6      1.9      1.5
D-Mart         0.9      0.8      1.0      0.8
HDFC Bank      1.2      1.3      1.1      1.4
...
```

### Why it is the hardest structure to model correctly

| Challenge | Description |
|-----------|-------------|
| **Firm entry and exit** | IPOs add rows mid-panel; delistings remove them. An unbalanced panel is the norm. |
| **Missing or stale fundamentals** | Small firms report late. Quarterly data may not exist for all firms for all periods. |
| **Heterogeneity across firms** | A leverage ratio of 3× means something different for an NBFC vs a software firm. |
| **Non-synchronous reporting** | Firms have different fiscal year ends. Q3 for Titan ≠ Q3 for March-year-end firms. |

### ML implications
- Features built on panel data must be constructed **entity by entity** (e.g., a rolling mean for each firm separately, not across the full panel).
- Train-test splits must be **time-based**, not row-based, to prevent data leakage across firms.
- Consider entity fixed effects — a firm's average profitability should not "explain" its future returns just because the model memorised the firm ID.

---

## 5. Non-Stationarity & Market Regimes

### Why stationarity fails in finance

Most ML algorithms assume the data-generating process is stationary — that the relationship between features and the target is stable over time. Financial data routinely violates this:

| Source of non-stationarity | Example |
|---------------------------|---------|
| Business cycles | Credit default rates in 2008 vs 2015 |
| Monetary policy shifts | Low-rate (2010–2021) vs high-rate (2022–2024) environments |
| Technological change | Fintech disruption reshaping bank NIM |
| Regulatory intervention | Demonetisation (India, 2016) instantly changed cash-based business fundamentals |

### What non-stationarity means in practice

- **Mean, variance, or correlations change over time** — the historical average ROE of a sector is not a reliable benchmark after a structural shift.
- **Relationships between features drift** — a leverage ratio that predicted distress in 2005 may behave differently in a zero-rate world where cheap debt is abundant.
- **Historical patterns may not repeat** — backtests on one regime can dramatically overstate out-of-sample performance.

### Market regimes

Financial markets cycle through distinct regimes. A model trained in one regime often **fails in another**:

| Regime pair | What changes |
|-------------|-------------|
| Bullish vs bearish | Return autocorrelation, momentum strength |
| Low vs high volatility | Correlation structure, options pricing |
| Low vs high interest rates | Valuation multiples, sector rankings |
| Liquidity-rich vs stressed | Credit spreads, factor exposures |

**Real examples of regime shifts:**
- Pre-COVID vs post-COVID: volatility spiked, correlations spiked, sector leadership flipped.
- 2022 rate hike cycle: growth stocks with high P/E ratios collapsed; value stocks outperformed — the opposite of 2019.

### Impact on ML models

| Problem | Consequence |
|---------|-------------|
| Model trained in bull market deployed in bear | Feature importance inverts — momentum signals may reverse |
| Full-sample training across multiple regimes | Model learns an "average" relationship that is correct in neither regime |
| No regime label in features | Model has no way to condition its predictions on the current environment |
| Backtests using long history | Appear robust but exploit multiple regime-specific patterns that may not co-exist again |

**Mitigation strategies:**
- Regime-aware features (e.g., include a volatility state variable)
- Walk-forward validation that spans at least one full cycle
- Stress-test model on recession sub-periods explicitly

---

## 6. Look-Ahead Bias

### What it is
Using information that **would not have been available** at the decision point in time. It is one of the most common and most damaging errors in financial ML — and it is almost always accidental.

> **Rule:** Decision time ≥ information availability time. Never use data at time *t* that was only published at time *t + k*.

### Why financial ratios are especially vulnerable

Accounting data is reported with a **delay**. The fiscal year ends on 31 March, but the audited annual report is filed weeks to months later.

```
Timeline — FY2023 (Indian listed company):

31 Mar 2023  ← Financial year ends
             ← Internally, management knows the numbers

Apr–May 2023 ← Market does NOT know the final ROE/current ratio
             ← Using FY23 ratios for April 2023 predictions = look-ahead bias

June 2023    ← Company files results; data is publicly available
             ← Earliest valid date to use FY23 fundamentals as features
```

**Wrong:** Build a feature for April 2023 using the FY23 ROE (year ended March 2023).
**Right:** Lag the feature — the FY23 ROE is available as a feature only from June 2023 onward.

### Common sources of look-ahead bias

| Source | How bias enters |
|--------|----------------|
| Revised financial statements | Using restated EPS instead of the originally reported number |
| Index constituent selection | Using today's Nifty 50 to build a 10-year backtest — firms that were removed (often because they declined) are absent |
| Full-sample normalisation | Z-scoring a feature using the full dataset mean/std leaks future distribution information into the past |
| Rolling window computation | A 6-month rolling average at date *t* that accidentally includes months *t+1* to *t+3* |
| Labels from future data | Labelling a Q1 observation as "distressed" using data that only emerged in Q3 |

### Index membership example (very common mistake)

**Scenario:** Backtest a quantitative strategy on Nifty 50 stocks from 2010 to 2024.

**Wrong approach:** Use the current (2024) Nifty 50 members as the universe for the entire history.
**What this does:** The 2024 Nifty 50 only contains companies that survived and grew large enough to be included. Any firm that was in the index in 2010 but later declined and was removed is excluded from the historical universe.
**Result:** The backtest operates on a universe of survivors — inflating historical performance.

**Right approach:** Use point-in-time index membership. At each rebalance date, use only the firms that were actually in the index on that date.

### How to avoid look-ahead bias

1. **Use point-in-time datasets** — data vendors like Bloomberg, Compustat, or Refinitiv provide "as-reported" databases that preserve original filing dates.
2. **Lag fundamentals conservatively** — for annual filings, assume a 90-day delay; for quarterly, 45 days is common.
3. **Audit feature construction** — for every feature, ask: "On the date this feature is used, would this value have been known?"
4. **Separate accounting period from availability date** in your data pipeline.

---

## 7. Survivorship Bias

### What it is
Only observing entities that **survived** to the present — and therefore excluding those that failed, were delisted, or went bankrupt. This systematically overstates historical returns and understates historical risk.

### How it enters financial datasets

| Scenario | How bias enters |
|----------|----------------|
| Downloading historical stock data | Many providers only return currently listed stocks |
| Using a mutual fund database | Funds that closed (due to poor performance) are excluded |
| Corporate fundamental databases | Bankrupt firms are often removed from standard downloads |
| Academic datasets | Papers using CRSP/Compustat must explicitly include delisted returns |

### Impact

| Effect | Description |
|--------|-------------|
| **Inflated returns** | The surviving stocks are disproportionately the ones that did well |
| **Underestimated risk** | Distress episodes are hidden because the distressed firms are gone |
| **False confidence in models** | A credit model trained without bankrupt firms never learned what distress looks like |

### Numerical illustration

Imagine a universe of 100 stocks in 2010. By 2020, 20 have been delisted (most due to bankruptcy or poor performance). If you download data in 2024:
- A survivorship-biased dataset gives you 80 stocks with 10-year returns
- The 20 delisted stocks — which likely had large negative returns — are missing
- The average 10-year return in the biased dataset is significantly higher than the true average

### How to avoid survivorship bias

1. **Include delisted securities** — explicitly download delisting return data and append it to the history.
2. **Use historical universes** — at each point in time, your universe should reflect which firms existed and were tradeable *then*.
3. **Model entry and exit explicitly** — treat IPOs as new observations and delistings as terminal observations.
4. **Be sceptical of too-good results** — if a backtest returns 30%+ CAGR with low drawdown, check whether the universe is survivorship-biased.

---

## 8. Worked Examples — Ratios Through the Data Lens

### Example 1: Current Ratio (Liquidity)

**Ratio:** Current Assets / Current Liabilities

**Time-series view:** A manufacturing firm's current ratio deteriorates from 1.6 → 0.7 over 10 years.
- In an easy-credit regime, 0.7 may be manageable — the firm can roll short-term debt cheaply.
- In a stressed regime (rising rates, credit tightening), the same ratio signals acute liquidity risk.

**Look-ahead risk:** The ratio for Q4-FY23 is only usable as a feature after the filing date (typically June 2023 for March year-end companies).

**Survivorship bias:** Firms that dropped to a current ratio of 0.4 often delisted before you could observe the outcome. Your training data may underrepresent the truly distressed end of the spectrum.

**ML lesson:** Use the *change* in current ratio (QoQ or YoY delta) rather than the level. Changes are more stationary and more predictive of distress.

---

### Example 2: ROE (Profitability)

**Ratio:** Net Income / Shareholders' Equity

**Cross-sectional view:** ROE is a strong cross-sectional signal — high-ROE firms tend to outperform low-ROE firms within the same sector. But ROE levels are not comparable across sectors (banks, asset-heavy industrials, and software firms have structurally different ROE ranges).

**Regime dependency:** The ROE–return relationship is regime-dependent.
- In a bull market with expanding multiples, even low-ROE firms may see their stocks rise.
- In a value rotation, high-ROE firms get re-rated upward.

**Look-ahead risk:** Annual filings and restatements. A firm that restated its FY22 earnings in FY23 should not have the restated number used for FY22 model training.

**Survivorship bias:** Firms with chronically low ROE are often acquired, delisted, or go bankrupt — removed from the dataset before the outcome is observed.

**ML lesson:** Use sector-relative ROE ranks and lag the feature by at least one quarter after the fiscal year end.

---

### Example 3: Debt-to-Equity (Solvency)

**Ratio:** Total Debt / Shareholders' Equity

**Non-linear dynamics:** Leverage builds slowly and collapses non-linearly. A D/E of 3× is manageable for years — then a covenant breach or refinancing wall triggers rapid deterioration.

**Regime sensitivity:** In a low-rate environment, D/E of 4× may be financially sustainable. In a 7% rate environment, the same leverage may create unsustainable interest coverage pressure.

**Look-ahead risk:** Revised EBIT (used to calculate interest coverage alongside D/E) and information about upcoming refinancing dates.

**Survivorship bias:** Defaults and bankruptcies are the most informative tail observations for a solvency model — and they are the most likely to be missing from a standard database download.

**ML lesson:** Solvency ratios predict **tail risk** (default probability), not mean returns. Treat them as features for downside risk models, not return prediction models.

---

### Example 4: Asset Turnover (Activity / Efficiency)

**Ratio:** Revenue / Total Assets

**Seasonality:** Retail firms (D-Mart) have strong Q3 (festive season) spikes. Using a single-period snapshot misses this. A rolling 4-quarter average removes seasonality.

**Cross-sectional heterogeneity:** Asset turnover for a capital-light software firm (10×) vs a capital-heavy steel manufacturer (0.6×) are not comparable. Cross-sectional comparisons require sector controls.

**Structural breaks:** Automation or outsourcing can permanently shift a firm's asset base and therefore its turnover, creating a break in the time-series that is not a signal — it is a regime change in the firm's operating model.

**Look-ahead risk:** Annualised revenue computed using quarters not yet reported.

**Survivorship bias:** Inefficient operators (low asset turnover) are acquired or fail. The surviving firms in an industry tend to be the efficient ones, biasing the distribution upward.

**ML lesson:** Seasonality and structural breaks must be explicitly modelled — not left to the model to figure out.

---

## 9. Best Practices Checklist

| Check | Description |
|-------|-------------|
| **Point-in-time data** | Every feature reflects only what was publicly known on the feature date |
| **Lagged fundamentals** | Annual filings: 90-day lag minimum. Quarterly: 45-day lag minimum |
| **Time-based splits** | No random shuffling of time-series or panel data |
| **Historical universe** | Universe membership is as-of each rebalance date, not today |
| **Include delisted firms** | Delisting returns appended to avoid survivorship bias |
| **Regime-aware evaluation** | Validate separately on bull, bear, and crisis sub-periods |
| **Bias audit** | Before modelling, explicitly check for look-ahead and survivorship bias |
| **Stationarity check** | Use returns/changes rather than levels for trending series |

> **Core takeaway:** Financial data violates standard ML assumptions on almost every dimension. Structure and timing matter as much as feature engineering. Complex models cannot fix bad data — good data discipline beats model complexity.
