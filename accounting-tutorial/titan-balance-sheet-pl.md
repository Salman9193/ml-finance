# Titan Company — Financial Analysis & ML Feature Guide
> Standalone Balance Sheet as at 31st March 2025 | All figures in ₹ Crore

---

## Table of Contents
1. [Accounting Concepts & Conventions](#1-accounting-concepts--conventions)
2. [Balance Sheet Snapshot](#2-balance-sheet-snapshot)
3. [Liquidity Ratios](#3-liquidity-ratios)
4. [Leverage Ratios](#4-leverage-ratios)
5. [Efficiency / Turnover Ratios](#5-efficiency--turnover-ratios)
6. [Profitability Ratios](#6-profitability-ratios)
7. [Market / Valuation Ratios](#7-market--valuation-ratios)
8. [Key Observations — Titan FY25](#8-key-observations--titan-fy25)
9. [ML Feature Engineering Guide](#9-ml-feature-engineering-guide)

---

## 1. Accounting Concepts & Conventions

### Concepts (Fundamental Assumptions)

These are the theoretical foundations every financial statement is built on. When you read a balance sheet or P&L, each line item exists *because* of one of these rules.

| # | Concept | Plain English | Why It Matters in ML |
|---|---------|---------------|----------------------|
| 1 | **Business Entity** | The company and its owner are financially separate | Ensures financial data reflects the business, not personal transactions |
| 2 | **Going Concern** | Assume business continues into the foreseeable future | Violated assumption = distress signal; used in bankruptcy prediction |
| 3 | **Money Measurement** | Only monetisable transactions are recorded | Limits data to quantifiable signals — brand value, morale are excluded |
| 4 | **Accounting Period** | Life of business divided into time slices (quarterly, annual) | Enables time-series modelling; creates seasonality and periodicity |
| 5 | **Historical Cost** | Assets recorded at acquisition price, not market value | Creates book value vs market value divergence — core to Price-to-Book ratio |
| 6 | **Accrual** | Income/expense recorded when incurred, not when cash moves | Explains why profit ≠ cash flow; crucial for cash flow prediction models |
| 7 | **Matching** | Expenses matched to the revenue they helped generate | Foundation of margin analysis; mismatches signal accounting manipulation |
| 8 | **Realisation** | Revenue recorded only when earned and collectible | Governs trade receivables; affects DSO and revenue recognition timing |
| 9 | **Dual Aspect** | Every transaction has two effects (debit and credit) | Basis of double-entry; assets always = liabilities + equity (balance check) |

### Conventions (Practical Guidelines)

| Convention | Rule | ML Signal |
|------------|------|-----------|
| **Consistency** | Same accounting methods used year over year | Method change = restatement risk; anomaly in time-series data |
| **Conservatism (Prudence)** | Anticipate losses; recognise profits only when certain | Explains write-downs and provisions — impairment detection |
| **Full Disclosure** | All material information must be disclosed | Absence of disclosure = audit risk feature |
| **Materiality** | Only significant items need detailed treatment | Threshold for feature significance in financial models |

---

## 2. Balance Sheet Snapshot

### Assets (₹ Cr)

| Particulars | Mar-25 | Mar-24 | YoY Change |
|-------------|--------|--------|------------|
| PP&E (Net) | 1,474 | 1,380 | +6.8% |
| Capital WIP | 86 | 81 | — |
| Right-of-use Assets | 1,449 | 1,225 | +18.3% |
| Intangible Assets | 95 | 85 | — |
| Investments (Non-current) | 6,386 | 6,178 | — |
| **Total Non-Current Assets** | **10,631** | **10,169** | **+4.5%** |
| Inventories | 25,047 | 16,074 | **+55.8%** |
| Trade Receivables | 984 | 917 | +7.3% |
| Cash & Equivalents | 243 | 272 | -10.7% |
| **Total Current Assets** | **30,444** | **22,693** | **+34.2%** |
| **TOTAL ASSETS** | **41,075** | **32,062** | **+28.1%** |

### Equity & Liabilities (₹ Cr)

| Particulars | Mar-25 | Mar-24 | YoY Change |
|-------------|--------|--------|------------|
| Equity Share Capital | 89 | 89 | — |
| Other Equity | 16,722 | 14,368 | +16.4% |
| **Total Equity** | **16,811** | **14,457** | **+16.3%** |
| Long-term Borrowings | 420 | 3,139 | **-86.6%** |
| **Total Non-Current Liabilities** | **2,619** | **5,043** | -48.1% |
| Short-term Borrowings | 7,483 | 2,670 | **+180.3%** |
| Trade Payables | 1,314 | 777 | +69.1% |
| Other Current Liabilities | 3,069 | 3,891 | — |
| **Total Current Liabilities** | **21,645** | **15,362** | **+40.9%** |
| **TOTAL EQUITY & LIABILITIES** | **41,075** | **32,062** | **+28.1%** |

> Dividend Paid: ₹1,276 Cr (Mar-25) vs ₹1,152 Cr (Mar-24)

---

## 3. Liquidity Ratios

> **Definition:** Measures the company's ability to meet short-term financial obligations.

### 3.1 Current Ratio

| Item | Formula | Mar-25 | Mar-24 |
|------|---------|--------|--------|
| Current Ratio | CA / CL | ~1.41 | ~1.48 |

**What it tells you:** For every ₹1 of short-term debt, Titan has ₹1.41 in current assets to cover it. A ratio above 1 is generally healthy. Slight decline YoY worth monitoring.

**Interpretation thresholds:**
- < 1.0 → Potential liquidity stress
- 1.0–1.5 → Adequate (typical for retail/jewellery)
- > 2.0 → Very comfortable (may signal idle assets)

---

### 3.2 Quick Ratio (Acid Test)

| Formula | Note |
|---------|------|
| (CA − Inventories) / CL | Strips out least liquid asset |

**What it tells you:** Titan's inventory is massive (₹25,047 Cr = 82% of current assets). Quick Ratio will be significantly below Current Ratio — this is expected for jewellery companies that hold gold stock. The gap between Current Ratio and Quick Ratio is itself an informative signal.

---

### 3.3 Cash Ratio

| Formula | Mar-25 Cash | Mar-24 Cash |
|---------|------------|------------|
| (Cash + Bank) / CL | ₹243 Cr | ₹272 Cr |

**What it tells you:** Most conservative liquidity test. Cash declined slightly — the company is deploying cash into inventory and investments rather than holding it idle.

---

### 3.4 Net Working Capital (NWC)

| Formula | Mar-25 | Mar-24 |
|---------|--------|--------|
| CA − CL | ₹8,799 Cr | ₹7,331 Cr |

**What it tells you:** Absolute rupee buffer between current assets and current liabilities. NWC improved by ~₹1,468 Cr YoY — a positive signal despite the large inventory build.

---

### 3.5 Inventory to NWC Ratio

| Formula | Interpretation |
|---------|---------------|
| Inventory / NWC | Proportion of working capital tied up in inventory |

**What it tells you:** With inventory nearly tripling NWC, Titan's liquidity is highly dependent on the ability to sell or liquidate gold. This concentration creates both risk and opportunity.

---

## 4. Leverage Ratios

> **Definition:** Measures how much debt the company uses to finance its assets and operations.

### 4.1 Debt-to-Equity Ratio

| Formula | Mar-25 | Mar-24 |
|---------|--------|--------|
| Long-term Debt (NCL) / Total Equity | ~0.16 | ~0.35 |

**What it tells you:** Long-term D/E ratio dropped sharply — Titan paid down long-term borrowings significantly. However, short-term borrowings surged, so total leverage picture is different from long-term D/E alone.

---

### 4.2 Total Debt-to-Asset Ratio

| Formula | Interpretation |
|---------|---------------|
| Total Debt / Total Assets | Fraction of assets financed by borrowing |

**What it tells you:** With total assets of ₹41,075 Cr and combined borrowings of ~₹7,900 Cr, the D/A ratio is moderate (~0.19). Most assets are funded by equity and trade payables.

---

### 4.3 Interest Coverage Ratio

| Formula | Mar-25 | Mar-24 |
|---------|--------|--------|
| Interest Coverage / Total Operating Revenue × 100 | **6.84x** | **10.60x** |

**What it tells you:** The company's ability to pay interest from operating earnings has deteriorated. Still comfortably above the danger zone (< 1.5x) but the downward trend warrants attention — especially given rising short-term borrowings which carry higher/variable interest.

**Interpretation thresholds:**
- < 1.5x → High distress risk
- 1.5–3x → Watch zone
- 3–7x → Healthy
- > 7x → Very comfortable

---

## 5. Efficiency / Turnover Ratios

> **Definition:** Measures how effectively the company uses its assets to generate revenue.

### 5.1 Asset Turnover

| Formula | Interpretation |
|---------|---------------|
| Total Revenue / Total Assets | Revenue per ₹1 of assets |

**What it tells you:** Higher is better. Retail/consumer companies typically show high asset turnover because they have relatively lean fixed asset bases compared to revenue.

---

### 5.2 Fixed Asset Turnover

| Formula | Note |
|---------|------|
| OP Revenue / Fixed Assets | PP&E = ₹1,474 Cr |

**What it tells you:** Titan's fixed asset base is small relative to its revenue — characteristic of an asset-light retail brand where stores are leased (showing as Right-of-use assets, not owned PP&E).

---

### 5.3 Working Capital Turnover

| Formula | Interpretation |
|---------|---------------|
| OP Revenue / NWC | Revenue generated per rupee of working capital |

**What it tells you:** Indicates operational efficiency. Decline YoY (NWC grew faster than revenue) signals capital is being deployed into working capital faster than it's converting to sales.

---

### 5.4 Accounts Receivable (DSO — Days Sales Outstanding)

| Formula | Titan Context |
|---------|--------------|
| 365 / (OP Rev / Trade Receivables) | Trade Rec: ₹984 Cr |

**What it tells you:** Number of days to collect money after a sale. Jewellery retail is largely cash/card-based, so DSO should be relatively low. Institutional/B2B sales may inflate this.

---

### 5.5 Inventory Days (DIO — Days Inventory Outstanding)

| Formula | Titan Context |
|---------|--------------|
| 365 / (COGS / Inventory) | Inventory: ₹25,047 Cr |

**What it tells you:** How long inventory sits before being sold. For a jewellery company, DIO is structurally high because gold is simultaneously inventory and a strategic commodity. The 56% inventory jump in Mar-25 will have significantly extended DIO — this is a key ratio to watch for Mar-26.

---

### 5.6 Trade Payable Days (DPO — Days Payable Outstanding)

| Formula | Mar-25 Trade Payables |
|---------|----------------------|
| 365 / (COGS / Trade Payables) | ₹1,314 Cr (up from ₹777 Cr) |

**What it tells you:** How long Titan takes to pay its suppliers. DPO increased significantly — Titan is stretching its payable cycle, using supplier credit to partially fund its inventory build. A fine balance; excessive DPO strains supplier relationships.

---

## 6. Profitability Ratios

> **Definition:** Measures how efficiently the company converts revenue into profit at various levels.

### 6.1 Profitability Margin Summary

| Metric | Formula | Mar-25 | Mar-24 | Trend |
|--------|---------|--------|--------|-------|
| EBITDA Margin | EBITDA / Total Op Rev × 100 | **10.55%** | **11.75%** | ↓ |
| EBIT Margin | EBIT / Total Op Rev × 100 | **9.57%** | **0.11%** | ↑ (anomaly in FY24) |
| Net Profit Margin | PAT / Total Op Rev × 100 | **6.03%** | **7.44%** | ↓ |
| Other Income Change | % Change YoY | **-3.33%** | — | Decreases |

**Reading the EBIT anomaly:** EBIT Margin in Mar-24 appears near zero (0.11%) while EBITDA was 11.75%. This extreme D&A gap suggests a large one-time impairment, write-off, or restructuring charge in FY24. Always check notes to accounts for such anomalies before using ratios in models.

---

### 6.2 ROCE (Return on Capital Employed)

| Formula | Interpretation |
|---------|---------------|
| EBIT / Capital Employed | Capital Employed = Total Equity + NCL |

**What it tells you:** The return generated on every rupee of long-term capital. ROCE above WACC (Weighted Average Cost of Capital) means the company is creating value. Titan's strong brand and distribution typically support high ROCE.

---

### 6.3 ROTA (Return on Total Assets)

| Formula | Interpretation |
|---------|---------------|
| PAT / Total Assets | Net profit per rupee of assets |

**What it tells you:** Comprehensive measure of asset productivity. Denominator grew 28% (total assets expansion) — so ROTA will show compression even if PAT grew, unless earnings kept pace.

---

### 6.4 ROE (Return on Equity)

| Formula | Interpretation |
|---------|---------------|
| PAT / Total Equity | Earnings per rupee of shareholder funds |

**What it tells you:** The ultimate measure of value creation for shareholders. Equity grew from ₹14,457 Cr → ₹16,811 Cr, meaning ROE will compress unless PAT grew proportionally.

**DuPont Decomposition of ROE:**
```
ROE = Net Profit Margin × Asset Turnover × Equity Multiplier
    = (PAT/Revenue)  ×  (Revenue/Assets)  ×  (Assets/Equity)
```
This decomposition separates operational efficiency from financial leverage — critical for attribution analysis in ML models.

---

## 7. Market / Valuation Ratios

> **Definition:** Links financial performance to stock market pricing.

### Market Data (Mar-25)

| Metric | Value |
|--------|-------|
| Market Capitalisation | ₹3,17,108 Cr |
| Share Price (Mar-25) | ₹3,563.01 (implied) |
| Shares Outstanding | 89 Cr |
| Dividend Paid | ₹1,276 Cr |

---

### 7.1 P/E Ratio (Price-to-Earnings)

| Formula | Interpretation |
|---------|---------------|
| Market Price / EPS | Market pays X times earnings |

**What it tells you:** A higher P/E reflects higher growth expectations or lower perceived risk (quality premium). Titan typically trades at a significant premium to market P/E due to brand strength, consistent earnings, and Tata group parentage.

---

### 7.2 EPS (Earnings Per Share)

| Formula | Interpretation |
|---------|---------------|
| PAT / Total Shares Outstanding | Per-share profitability |

**What it tells you:** Normalises profit for share count — allows comparison across companies of different sizes. EPS growth is the primary driver of long-term stock price appreciation.

---

### 7.3 Dividend Payout Ratio

| Formula | Mar-25 | Mar-24 |
|---------|--------|--------|
| DPS / EPS × 100 | Dividends: ₹1,276 Cr | ₹1,152 Cr |

**What it tells you:** What percentage of earnings is distributed as dividends. Titan is consistently growing its dividend — signals management confidence in future earnings sustainability.

---

### 7.4 Dividend Yield

| Formula | Interpretation |
|---------|---------------|
| DPS / Market Price × 100 | Cash return relative to share price |

**What it tells you:** At Titan's premium valuation, dividend yield will be low (typical for growth stocks). Investors buy Titan for capital appreciation, not income.

---

## 8. Key Observations — Titan FY25

### 8.1 The Inventory Story
Inventories surged from ₹16,074 Cr to ₹25,047 Cr — a **55.8% jump in one year**. This is almost certainly a strategic gold procurement decision:
- Gold prices rose sharply in FY25
- Titan may be hedging future cost inflation by holding physical gold
- This build-up inflates the balance sheet, compresses asset productivity ratios, and signals confidence in demand

**Risk:** If gold prices fall or consumer demand slows, this inventory becomes a liability. Watch for inventory write-downs in FY26 disclosures.

### 8.2 Debt Maturity Shift
- Long-term borrowings: ₹3,139 Cr → ₹420 Cr (−87%)
- Short-term borrowings: ₹2,670 Cr → ₹7,483 Cr (+180%)

This is a classic **maturity shortening** — the company replaced long-term debt with cheaper (but riskier) short-term facilities, likely to fund the inventory build. This exposes Titan to **refinancing risk** if credit markets tighten.

### 8.3 Margin Compression
Both EBITDA and Net Profit margins declined YoY despite revenue growth. Possible causes:
- Gold price inflation in COGS outpacing selling price increases
- Higher operating costs (store expansion, lease renewals)
- Interest cost rise from short-term borrowings

### 8.4 Interest Coverage Decline
From 10.60x to 6.84x — still healthy but directionally concerning. With significantly higher short-term debt in FY25, FY26 interest costs will rise further. Watch for ICR dropping below 5x as a caution signal.

### 8.5 Equity Growth (Positive)
Equity grew organically from ₹14,457 Cr to ₹16,811 Cr (+16.3%) — entirely from retained earnings. No dilution. This is a strong signal of genuine value creation.

---

## 9. ML Feature Engineering Guide

### 9.1 Feature Categories

```
Financial Ratio Features
│
├── Liquidity Features      → Credit risk, bankruptcy prediction
├── Leverage Features       → Debt capacity, rating models
├── Efficiency Features     → Operational quality, demand forecasting
├── Profitability Features  → Stock return prediction, valuation
└── Market Features         → Momentum, factor models, sentiment
```

---

### 9.2 Ratio → ML Use Case Mapping

| Ratio | ML Task | Model Type | Label / Feature |
|-------|---------|------------|-----------------|
| Current Ratio | Credit Risk Scoring | Logistic Regression, XGBoost | Feature |
| Quick Ratio | Liquidity Anomaly Detection | Isolation Forest, Autoencoder | Feature |
| Cash Ratio | Dividend Prediction | Random Forest | Feature |
| D/E Ratio | Credit Rating Classification | XGBoost, SVM | Feature |
| Interest Coverage | Financial Distress Prediction | LSTM (time-series) | Feature + Early Warning |
| Debt-to-Asset | Bankruptcy Prediction (Altman Z) | Logistic Regression | Feature |
| Asset Turnover | DuPont ROE Decomposition | Linear Regression | Feature (component) |
| Fixed Asset Turnover | Capex Cycle Prediction | Gradient Boosting | Feature |
| DSO | Cash Flow Forecasting | ARIMA, Prophet | Feature |
| DIO | Demand Forecasting, Inventory Risk | LSTM, XGBoost | Feature + Label |
| DPO | Supply Chain Risk | Clustering (K-Means) | Feature |
| EBITDA Margin | Peer Benchmarking | Cosine Similarity, Clustering | Feature + Label |
| Net Profit Margin | Stock Return Prediction | Factor Models, Ridge Regression | Feature |
| ROCE | Quality Factor Model | Quantile Regression | Feature |
| ROE | Portfolio Optimisation | Mean-Variance Optimisation | Feature |
| P/E Ratio | Momentum / Value Factor | XGBoost, Neural Net | Feature |
| EPS Growth | Earnings Surprise Model | Ensemble Methods | Feature |
| Dividend Payout | Dividend Cut Prediction | Binary Classification | Feature + Label |
| Dividend Yield | Income Factor Model | Linear Factor Model | Feature |

---

### 9.3 Feature Engineering Patterns

#### Time-Series Delta Features
```python
# Year-over-year change signals trend
df['current_ratio_delta'] = df['current_ratio'] - df['current_ratio'].shift(4)  # quarterly
df['inventory_growth_pct'] = df['inventory'].pct_change(4) * 100
df['icr_trend'] = df['icr'].rolling(4).mean()  # smoothed ICR trend
```

#### DuPont Decomposition
```python
# Decompose ROE into its three drivers for attribution
df['net_margin']      = df['pat'] / df['revenue']
df['asset_turnover']  = df['revenue'] / df['total_assets']
df['equity_multiplier'] = df['total_assets'] / df['total_equity']
df['roe_check']       = df['net_margin'] * df['asset_turnover'] * df['equity_multiplier']
```

#### Cash Conversion Cycle (CCC)
```python
# CCC = DIO + DSO - DPO
# Lower (even negative) CCC = company receives cash before paying suppliers
df['dio'] = 365 / (df['cogs'] / df['inventory'])
df['dso'] = 365 / (df['revenue'] / df['trade_receivables'])
df['dpo'] = 365 / (df['cogs'] / df['trade_payables'])
df['ccc'] = df['dio'] + df['dso'] - df['dpo']
```

#### Altman Z-Score (Bankruptcy Prediction)
```python
# Classic model; Z > 2.99 = safe, 1.81–2.99 = grey zone, < 1.81 = distress
df['z_score'] = (
    1.2 * (df['nwc'] / df['total_assets']) +
    1.4 * (df['retained_earnings'] / df['total_assets']) +
    3.3 * (df['ebit'] / df['total_assets']) +
    0.6 * (df['market_cap'] / df['total_liabilities']) +
    1.0 * (df['revenue'] / df['total_assets'])
)
```

---

### 9.4 Anomaly Signals from Titan FY25

| Signal | Observation | ML Implication |
|--------|-------------|----------------|
| Inventory spike (+56%) | Sudden large build-up | Flag as outlier in inventory forecasting model; requires domain context |
| EBIT vs EBITDA gap (FY24) | Near-zero EBIT despite healthy EBITDA | Impairment detection feature — large non-cash charge |
| Debt maturity shift | LT debt → ST debt conversion | Feature for refinancing risk classifier |
| ICR decline (10.6 → 6.84) | Rising interest burden | Time-series early warning feature |
| Equity growth without dilution | Retained earnings compounding | Quality signal in stock selection models |

---

### 9.5 Data Quality Warnings for ML

When using financial ratios as ML features, always handle:

1. **Division by zero:** Equity, assets, or revenue can be zero/negative for distressed companies
2. **Outlier ratios:** Extremely high P/E (loss-making) or negative D/E (negative equity) need special treatment
3. **One-time items:** FY24 EBIT anomaly shows why raw ratios need adjustment for exceptional items
4. **Survivorship bias:** If training on listed companies only, distressed/delisted firms are absent
5. **Look-ahead bias:** Financial data is published weeks after period end — use filing date, not period end date, for feature timing

```python
# Safe ratio computation
def safe_ratio(numerator, denominator, fill_value=np.nan):
    return np.where(denominator != 0, numerator / denominator, fill_value)

df['current_ratio'] = safe_ratio(df['current_assets'], df['current_liabilities'])
```

---

## Summary Cheat Sheet

| Category | Key Ratios | Formula (shorthand) | ML Use |
|----------|-----------|---------------------|--------|
| Liquidity | Current, Quick, Cash, NWC | CA/CL, (CA-Inv)/CL | Credit risk, distress |
| Leverage | D/E, D/A, ICR | Debt/Equity, EBIT/Interest | Rating models, bankruptcy |
| Efficiency | DSO, DIO, DPO, CCC | 365/Turnover | Forecasting, supply chain |
| Profitability | EBITDA%, EBIT%, PAT%, ROE, ROCE | Profit/Revenue, Profit/Capital | Factor models, valuation |
| Market | P/E, EPS, Div Yield, Payout | Price/EPS, DPS/Price | Momentum, income factors |

---

*Analysis based on Titan Company Standalone Balance Sheet — March 2025 | Prepared for educational and ML research purposes*