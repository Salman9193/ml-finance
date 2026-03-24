# Financial Ratios, Cash Flow, and Machine Learning Integration

Financial ratios are mathematical relationships between financial variables that help interpret a company's financial results. By standardizing raw numbers into ratios, analysts can compare companies of different sizes, track performance trends over time, and benchmark against industry peers.

However, modern financial analysis and Machine Learning (ML) models do not rely on static ratios alone — they integrate cash flow realities and time-series dynamics to predict real-world outcomes like credit defaults and stock returns.

---

## Table of Contents

- [1. The Importance of Cash Flow vs. Profit](#1-the-importance-of-cash-flow-vs-profit)
- [2. Categories of Financial Ratios](#2-categories-of-financial-ratios)
- [3. Deep Dive: Types of Ratios, Formulas, and Examples](#3-deep-dive-types-of-ratios-formulas-and-examples)
  - [A. Profitability Ratios](#a-profitability-ratios)
  - [B. Liquidity Ratios](#b-liquidity-ratios)
  - [C. Leverage Ratios](#c-leverage-ratios)
  - [D. Activity / Turnover Ratios](#d-activity--turnover-ratios)
  - [E. Valuation & Dividend Ratios](#e-valuation--dividend-ratios)
- [4. Earnings Quality and Accruals](#4-earnings-quality-and-accruals)
- [5. Real-World Use Cases & Machine Learning Integration](#5-real-world-use-cases--machine-learning-integration)

---

## 1. The Importance of Cash Flow vs. Profit

While profitability ratios are important, equating a company's success solely to its reported profits is a dangerous pitfall. **It is quite possible for a company to report profits but go out of business.**

- **Profits can be manipulated:** Profit figures include non-cash line items such as depreciation or goodwill write-offs. Businesses can use these to offset large capital expenditures, making the firm look profitable "on paper."
- **The cash gap:** Profit reports are based on sales income, but recorded revenue is often greater than actual cash received — especially when sales are made on credit. The collection period on accounts receivable can last 2–3 months. A company must have actual cash to float operations (pay leases, utilities, replenish inventory) during this gap.
- **Why lenders and ML models look at cash flow:** If a business runs out of cash, operations cease. Lenders rely on current and projected cash flows to determine if a company can afford debt. ML models consequently weight cash flow metrics heavily to evaluate true financial health.

> **Raw financial data, like a profit margin of 15%, conveys very little on its own. Ratios give it context.**

---

## 2. Categories of Financial Ratios

Financial ratios are broadly classified into five categories, each measuring a specific aspect of a company's business:

1. **Profitability Ratios** — Measure the ability to generate profits and the return on invested capital.
2. **Liquidity (Short-term Solvency) Ratios** — Assess a firm's ability to meet immediate, maturing obligations with available cash or liquid assets.
3. **Leverage (Long-term Solvency) Ratios** — Measure the extent to which a company relies on debt versus owner funds, and its ability to sustain operations long term.
4. **Activity / Turnover Ratios** — Evaluate how efficiently a business manages its inventory, receivables, and assets to generate revenue.
5. **Valuation Ratios** — Compare a company's stock price to its earnings or book value to gauge whether it is cheap or expensive.

<img src="diagrams/ratio-taxonomy.svg" alt="Financial ratio taxonomy" width="700">

---

## 3. Deep Dive: Types of Ratios, Formulas, and Examples

### A. Profitability Ratios

These ratios evaluate how well a company generates income relative to its revenue, assets, or equity.

- **EBITDA Margin:** `(EBITDA × 100) / Total Operating Revenue`. Indicates operational efficiency before accounting rules and financing costs skew the numbers.
  - *Example:* Amara Raja Batteries (ARBL) had EBITDA of ₹560 Cr on revenue of ₹3,436 Cr → EBITDA Margin = **16.3%**

- **Net Profit Margin (PAT Margin):** `(Profit After Tax × 100) / Total Revenue`. Shows final, overall profitability.
  - *Example:* ARBL's PAT was ₹367 Cr on revenues of ₹3,482 Cr → PAT Margin = **10.5%**

- **Return on Equity (ROE):** `Net Profit / Shareholders' Equity`. Shows the return generated on shareholders' capital.
  - *The DuPont Model* breaks ROE into three components to reveal the *quality* of returns:

    > **ROE = Net Profit Margin × Asset Turnover × Financial Leverage**

  - *Example:* ARBL's NPM (9.2%) × Asset Turnover (1.75) × Financial Leverage (1.61) = ROE of **~25.9%**

<img src="diagrams/dupont_roe.svg" alt="DuPont ROE breakdown" width="700">

- **Return on Assets (ROA) / Return on Total Assets (ROTA):** `(Net Income + Interest × (1 − Tax Rate)) / Total Average Assets`. Evaluates how effectively assets are used to generate profits.
  - *Example:* ARBL Net Income (367.4) + Tax-shielded Interest (4.76) / Avg Assets (1,955) = **19.03%**

- **Return on Capital Employed (ROCE):** `EBIT / Capital Employed`, where Capital Employed = Equity + Non-current Liabilities (or Short-term Debt + Long-term Debt + Equity).

- **Earnings per Share (EPS):** `PAT Attributable to Shareholders / Average Number of Equity Shares`. Indicates the profit available per equity share.

- **Economic Value Added (EVA):** `NOPAT − (Capital × Cost of Capital)`. The true economic profit after deducting the cost of capital — a company can be profitable but still destroy value if returns don't exceed the cost of capital.

---

### B. Liquidity Ratios

These determine if a company has enough liquid assets to cover its short-term liabilities.

- **Current Ratio:** `Current Assets / Current Liabilities`
  - *Example (Prufrock Corp):* $708M / $540M = **1.31×**

- **Quick Ratio:** `(Current Assets − Inventory − Prepaid Expenses) / Current Liabilities`. Removes the least liquid current assets.
  - *Example:* ($708M − $422M) / $540M = **0.53×**

- **Cash Ratio:** `Cash / Current Liabilities`. The most conservative measure — only counts cash and equivalents.

- **Inventory to Net Working Capital:** `Inventory / Working Capital`. Indicates the proportion of inventory tied up in working capital.

---

### C. Leverage Ratios

These measure debt reliance and long-term default risk.

- **Debt-to-Equity Ratio:** `Long-term Debt / Shareholders' Equity`. Indicates the proportion of funds from lenders versus owners.
  - *Example:* $0.28M / $0.72M = **0.38×**

- **Debt to Asset Ratio:** `Total Debt / Total Assets`. Indicates the proportion of total assets financed by debt.

- **Interest Coverage Ratio (Times Interest Earned):** `EBIT / Interest Expense`. Measures how comfortably a company can pay its interest obligations.
  - *Example:* EBIT of $600M / Interest of $141M = **4.26×**

---

### D. Activity / Turnover Ratios

These reflect management efficiency in utilizing resources.

- **Total Asset Turnover:** `Total Revenue / Average Total Assets`
  - *Example:* $2,311M / $3,588M = **0.64×**

- **Working Capital Turnover:** `Operating Revenue / Average Working Capital`

- **Inventory Turnover:** `Cost of Goods Sold (COGS) / Average Inventory`
  - *Example:* $1,435M / $422M = **3.40×**

- **Days of Inventory:** `Average Inventory / (COGS / 365)`. How many days it takes to clear inventory.

- **Accounts Receivable Turnover:** `Credit Sales / Average Accounts Receivable`

- **Average Collection Period:** `Average Accounts Receivable / (Credit Sales / 365)`. How many days it takes to collect cash from credit sales.

---

### E. Valuation & Dividend Ratios

These compare stock price against company fundamentals.

- **Price-to-Earnings (P/E) Ratio:** `Market Price per Share / EPS`
  - *Example:* ARBL Share Price (₹661) / EPS (₹21.49) = **30.76×**

- **Price-to-Book Value (P/BV) Ratio:** `Current Market Price / Book Value per Share`. Book Value = (Share Capital + Reserves excl. revaluation) / Total Shares. Acts as the salvage value if the firm were liquidated.
  - *Example:* ARBL Price (₹661) / BV per Share (₹79.8) = **8.3×**

- **Enterprise Value (EV) Multiple:** `EV / EBITDA`, where EV = Market Cap + Debt − Cash.
  - *Example:* Prufrock EV ($3,459M) / EBITDA ($876M) = **3.95×**

- **Dividend Payout Ratio:** `Dividend per Share / EPS`. The percentage of profit paid out to shareholders.

- **Dividend Yield:** `Dividend per Share / Market Price per Share`. The dividend rate of return at the current market price.

---

## 4. Earnings Quality and Accruals

Because "Cash is King", analysts and ML models must evaluate the *quality* of reported earnings. A firm might report high Net Income but have poor cash flow.

- **Cash Flow to Net Income (CFO/NI):** `Cash Flow from Operations / Net Income`
  - *Example:* If CFO = 110 and Net Income = 140 → CFO/NI = **0.79**. When CFO < NI, the company is not converting all profits into cash.

- **Accruals:** `Net Income − CFO`
  - *Example:* 140 − 110 = **30 in Accruals**

- **Accrual Intensity:** `Accruals / Total Assets`
  - *Example:* 30 / 1,200 = **2.5%**. High accrual intensity means profits are driven by accounting adjustments, not cash generation — a major red flag for ML anomaly models.

---

## 5. Real-World Use Cases & Machine Learning Integration

Raw accounting data is transformed into structured ratios to serve as **input features** for ML algorithms (Random Forest, XGBoost, Logistic Regression). Ratios eliminate scale bias — allowing comparison of large and small firms — and reduce multicollinearity compared to raw financials.

<img src="diagrams/ml_pipeline.svg" alt="ML pipeline for financial ratios" width="700">

### Mapping to ML Feature Vectors

A typical ML feature vector combines multiple signals non-linearly:

| Feature type | Example features |
|---|---|
| Profitability | Gross Margin (0.40), Operating Margin (0.20), ROA (0.117) |
| Risk & Leverage | Debt/Equity (0.71), Interest Coverage (5×) |
| Quality | CFO/NI (0.79), Accrual Intensity (0.025) |

### Time-Series Features (Δ Features)

Static ratios alone are insufficient. ML models rely on Δ features and rolling averages to smooth noise and spot momentum:

- **Growth features:** ΔRevenue Growth — e.g., (1,000 − 900) / 900 = +11.1%
- **Momentum features:** ΔROA (e.g., 10.4% → 11.7% = +1.3%), ΔMargins
- **Quality trends:** Δ(CFO/NI), ΔAccruals
  - *Red flag example:* If Accruals in FY23 were −10 and in FY24 became +30, ΔAccruals = +40. This sharp reversal signals earnings growth driven by accruals rather than cash — a common red flag in anomaly and credit models.
- **Leverage trends:** ΔDebt/Assets

### Model Interpretation in Real-World Scenarios

**1. Credit Risk & Loan Default Prediction**
- *Algorithm:* Random Forest
- *Static view:* Moderate leverage + strong interest coverage → low default risk
- *Dynamic view:* If the model detects improving ROA but deteriorating cash quality (CFO < Net Income) alongside rising leverage (ΔDebt/Assets), it triggers an **early warning signal** for default. The firm is profitable on paper but cash-poor.

**2. Equity Alpha (Stock Return) Prediction**
- *Algorithm:* XGBoost
- *Static view:* High profit margins signal strong pricing power → stock is attractive
- *Dynamic view:* If rapid earnings growth is driven by a sharp rise in positive accruals (rather than operating cash), the model identifies **mean reversion risk** and will downweight or short the stock despite apparent growth. Time dynamics often dominate static levels.

**3. Fraud and Bankruptcy Detection**
- *Algorithm:* Logistic Regression
- *Use case:* Financial regulators use anomaly models to flag window dressing. Extremely high Accrual Intensity combined with poor Cash Ratios easily flags a company for impending bankruptcy — proving that actual cash flow is far more important for survival than reported profit.

---

> By combining engineered ratio features with macroeconomic data and transaction-level data, ML models become robust, data-driven engines that excel across various economic cycles.