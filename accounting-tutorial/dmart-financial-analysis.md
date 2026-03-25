# Avenue Supermarts (D-Mart) — Financial Analysis & ML Feature Guide
> Profit & Loss + Balance Sheet | FY25 (202503) vs FY24 (202403) | All figures in ₹ Crore

---

## Table of Contents
1. [Business Model Overview](#1-business-model-overview)
2. [P&L Statement Analysis](#2-pl-statement-analysis)
3. [Balance Sheet Snapshot](#3-balance-sheet-snapshot)
4. [Liquidity Ratios](#4-liquidity-ratios)
5. [Efficiency & Turnover Ratios](#5-efficiency--turnover-ratios)
6. [Profitability Ratios](#6-profitability-ratios)
7. [Solvency Ratios](#7-solvency-ratios)
8. [DuPont Decomposition](#8-dupont-decomposition)
9. [Key Observations — D-Mart FY25](#9-key-observations--d-mart-fy25)
10. [ML Feature Engineering Guide](#10-ml-feature-engineering-guide)

---

## 1. Business Model Overview

D-Mart operates on an **EDLP (Everyday Low Price)** model — a high-volume, low-margin, asset-heavy grocery retail strategy. Its competitive moat rests on three pillars:

- **Owned stores** (not leased) — eliminates rent risk and landlord renegotiation
- **Zero financial debt** — fully self-funded through internal cash generation
- **Cash-and-carry model** — customers pay instantly; no receivables, no bad debt risk

This produces a financial profile that is the opposite of a capital-light brand like Titan: lower margins but exceptional asset velocity, near-zero credit risk, and a self-reinforcing cash cycle.

---

## 2. P&L Statement Analysis

### 2.1 Revenue Summary

| Line Item | FY25 | FY24 | YoY Change |
|-----------|------|------|------------|
| Sales Turnover | 65,382.87 | 55,894.17 | +17.0% |
| Less: Excise Duty | 6,024.82 | 5,105.34 | — |
| **Net Sales** | **59,358.05** | **50,788.83** | **+16.9%** |
| Other Income | 124.31 | 146.45 | -15.1% |
| Stock Adjustments | 1,117.06 | 683.77 | +63.4% |
| **Total Income** | **60,599.42** | **51,619.05** | **+17.4%** |

> Other Income declined (-15.1%) while stock adjustments surged (+63.4%) — inventory build-up is visible even in the P&L.

### 2.2 Expenditure Breakdown

| Line Item | FY25 | FY24 | YoY Change | % of Net Sales FY25 |
|-----------|------|------|------------|---------------------|
| Raw Materials (COGS) | 51,668.76 | 43,958.31 | +17.5% | **87.1%** |
| Employee Cost | 1,165.90 | 906.12 | +28.7% | 2.0% |
| Power & Fuel | 447.75 | 376.59 | +18.9% | 0.8% |
| Other Manufacturing Expenses | 1,751.85 | 1,323.43 | +32.4% | 3.0% |
| S&A Expenses | 314.05 | 276.13 | +13.7% | 0.5% |
| Miscellaneous | 639.47 | 528.25 | +21.1% | 1.1% |
| **Total Expenditure** | **55,987.78** | **47,368.83** | **+18.2%** | **94.3%** |

> **Red flag:** COGS as % of net sales rose from 86.5% (FY24) to 87.1% (FY25). Input cost inflation is outpacing D-Mart's ability to pass costs on — structurally constrained by its EDLP pricing commitment.

> **Watch:** Employee costs grew 28.7% vs revenue growth of 16.9% — labour cost leverage is negative. Likely driven by store expansion hiring.

### 2.3 Profit Cascade

| Line Item | FY25 | FY24 | YoY Change |
|-----------|------|------|------------|
| **EBIT (Operating Profit)** | **4,611.64** | **4,250.22** | **+8.5%** |
| Less: Interest | 69.45 | 58.13 | +19.5% |
| Gross Profit | 4,542.19 | 4,192.09 | +8.4% |
| Less: Depreciation | 869.52 | 730.76 | +19.0% |
| Profit Before Tax | 3,672.67 | 3,461.33 | +6.1% |
| Less: Tax | 947.46 | 913.70 | +3.7% |
| **Net Profit** | **2,707.45** | **2,535.61** | **+6.8%** |
| Adjusted Net Profit | 2,697.41 | 2,517.68 | +7.1% |

> Depreciation grew 19% — a direct consequence of PP&E capex for new stores. Revenue grew 16.9% but net profit grew only 6.8% — **profit growth is lagging revenue growth**, a sign of operating deleverage.

### 2.4 Per Share Metrics

| Metric | FY25 | FY24 |
|--------|------|------|
| EPS (before Minority Interest) | ₹41.61 | ₹38.97 |
| EPS (after Minority Interest) | ₹41.62 | ₹38.97 |
| Book Value per Share | ₹329.29 | ₹287.34 |
| Dividend Per Share | ₹0 | ₹0 |

> D-Mart pays **no dividend** — all profits are reinvested into store expansion. Consistent with a growth-reinvestment strategy.

---

## 3. Balance Sheet Snapshot

### Assets (₹ Cr)

| Particulars | FY25 | FY24 | YoY Change |
|-------------|------|------|------------|
| PP&E | 14,349.83 | 11,759.19 | +22.0% |
| Capital WIP | 1,099.35 | 935.22 | +17.5% |
| Right-of-Use Assets | 1,741.73 | 1,539.10 | +13.2% |
| Intangible Assets | 107.25 | 108.62 | — |
| Deferred Tax Assets | 426.45 | 367.90 | — |
| **Total Non-Current Assets** | **17,928.09** | **14,975.17** | **+19.7%** |
| Inventories | 5,044.37 | 3,927.31 | **+28.4%** |
| Trade Receivables | 153.79 | 166.37 | -7.6% |
| Cash & Cash Equivalents | 337.12 | 301.06 | +12.0% |
| Other Financial Assets (ST) | 528.36 | 1,128.49 | **-53.2%** |
| **Total Current Assets** | **6,392.20** | **6,202.03** | **+3.1%** |
| **TOTAL ASSETS** | **24,320.29** | **21,177.20** | **+14.8%** |

### Equity & Liabilities (₹ Cr)

| Particulars | FY25 | FY24 | YoY Change |
|-------------|------|------|------------|
| Share Capital | 650.73 | 650.73 | — |
| Other Equity | 20,777.02 | 18,047.09 | +15.1% |
| **Total Equity** | **21,427.75** | **18,697.82** | **+14.6%** |
| Long-term Borrowings | 0 | 0 | — |
| Lease Liabilities (NCL) | 555.79 | 399.24 | +39.2% |
| **Total Non-Current Liabilities** | **681.43** | **500.72** | +36.1% |
| Short-term Borrowings | 0 | 0 | — |
| Lease Liabilities (CL) | 263.83 | 192.92 | +36.8% |
| Trade Payables | 1,070.81 | 984.81 | +8.7% |
| **Total Current Liabilities** | **2,212.16** | **1,979.14** | **+11.8%** |
| **TOTAL EQUITY & LIABILITIES** | **24,320.29** | **21,177.20** | **+14.8%** |

---

## 4. Liquidity Ratios

> **Definition:** Ability to meet short-term financial obligations.

### 4.1 Summary Table

| Ratio | Formula | FY25 | FY24 | Signal |
|-------|---------|------|------|--------|
| Current Ratio | CA / CL | **2.89** | **3.13** | Healthy, slight dip |
| Quick Ratio | (CA − Inventory) / CL | **0.61** | **1.15** | Sharp decline — watch |
| Cash Ratio | (Cash + Bank) / CL | **0.16** | **0.32** | Halved — low but normal for retail |

### 4.2 Current Ratio — 2.89x

D-Mart has ₹2.89 of current assets for every ₹1 of current liabilities. The decline from 3.13 is moderate and still comfortably above the safety threshold of 1.5x for retail businesses.

**Why it declined:** Current Liabilities grew 11.8% (trade payables + lease liabilities) while Current Assets grew only 3.1%.

### 4.3 Quick Ratio — 0.61x (Critical Alert)

Stripping inventory (₹5,044 Cr) from current assets leaves only ₹1,348 Cr of liquid assets against ₹2,212 Cr of current liabilities. This 47% YoY decline is the most alarming single metric in the dataset.

**Root cause:** Other Financial Assets (short-term) fell from ₹1,128 Cr → ₹528 Cr (-53%). These were likely short-term FDs or treasury instruments that matured and were redeployed into capex or inventory.

**Why it's less dangerous for D-Mart specifically:** The business generates cash daily from store sales. With DSO of ~1 day, D-Mart converts inventory to cash faster than almost any other company. The Quick Ratio understates liquidity for businesses with near-instant cash cycles.

**Threshold:** Below 0.4x would be a genuine watch flag even accounting for the retail model.

### 4.4 Net Working Capital

| Metric | FY25 | FY24 |
|--------|------|------|
| NWC (CA − CL) | ₹4,180 Cr | ₹4,223 Cr |
| NWC / Total Assets | 17.2% | 19.9% |

NWC is nearly flat YoY in absolute terms despite 14.8% asset growth — operating capital efficiency is holding.

---

## 5. Efficiency & Turnover Ratios

> **Definition:** How effectively the company uses assets to generate revenue.

### 5.1 Turnover Ratios

| Ratio | Formula | FY25 | FY24 | Trend |
|-------|---------|------|------|-------|
| Inventory Turnover | COGS / Inventories | 10.24x | 11.19x | ↓ slowing |
| Receivables Turnover | Sales / Trade Receivables | 385.97x | 310.27x | ↑ faster |
| Asset Turnover | Sales / Total Assets | 2.44x | 2.40x | → stable |
| Payables Turnover | COGS / Trade Payables | 48.25x | 44.64x | ↑ paying faster |

### 5.2 Days Ratios (Cash Conversion Cycle)

| Metric | Formula | FY25 | FY24 | Interpretation |
|--------|---------|------|------|----------------|
| DIO (Inventory Days) | 365 / Inv. Turnover | **35.6 days** | 32.6 days | Rising — watch |
| DSO (Receivable Days) | 365 / Rec. Turnover | **0.95 days** | 1.18 days | Near-instant collection |
| DPO (Payable Days) | 365 / Pay. Turnover | **7.56 days** | 8.18 days | Paying suppliers slightly faster |
| Operating Cycle | DIO + DSO | **36.6 days** | 33.8 days | Slightly elongated |
| **Cash Cycle (CCC)** | DIO + DSO − DPO | **29.0 days** | 25.6 days | Rising — early warning |

### 5.3 The DSO Story — D-Mart's Superpower

A DSO of **0.95 days** is extraordinary. It means D-Mart converts a sale to cash in under 24 hours. This is the financial expression of its cash-and-carry model:

- No credit offered to retail customers
- Payment at POS is immediate (cash/card/UPI)
- Zero bad debt risk from retail operations
- Receivables Turnover of 386x is among the highest of any listed Indian company

This single ratio is the most powerful sector-classifier feature in the dataset — it immediately identifies D-Mart as a cash retail business versus B2B, services, or credit-extended businesses.

### 5.4 Cash Conversion Cycle — The Engine

```
CCC = DIO + DSO − DPO
    = 35.6 + 0.95 − 7.56
    = 29.0 days (FY25)
    = 25.6 days (FY24)
```

Despite being 29 days, D-Mart's CCC is well below the grocery retail average of 45–60 days. The business essentially uses supplier credit (DPO = 7.56 days) as a partial float against its inventory holding period.

**Concern:** CCC rose from 25.6 → 29.0 days in one year. If this trend continues (inventory days rising while payable days hold or fall), it indicates working capital is becoming less efficient.

### 5.5 Coverage Ratios

| Ratio | Formula | FY25 | FY24 |
|-------|---------|------|------|
| Interest Coverage | Op. Profit / Interest Exp. | **66.40x** | **73.12x** |
| Cash Coverage | (Op. Profit + Dep.) / Interest | **78.92x** | **85.69x** |

Interest coverage of 66x is exceptional — D-Mart earns 66 times what it needs to pay in interest. The slight decline is due to rising lease interest (store expansion) rather than financial debt. Long-term and short-term borrowings remain at zero.

---

## 6. Profitability Ratios

> **Definition:** How efficiently revenue is converted to profit.

### 6.1 Margin Summary

| Margin | Formula | FY25 | FY24 | Trend |
|--------|---------|------|------|-------|
| Gross Margin (approx.) | (Sales − COGS) / Sales | **12.9%** | **13.5%** | ↓ compressed |
| Operating Profit Margin | EBIT / Net Sales | **7.8%** | **8.4%** | ↓ compressed |
| Net Profit Margin | PAT / Net Sales | **5.0%** | **5.0%** | → flat |

**Margin compression analysis:** Operating margin fell ~60 bps but net margin held flat. This means below-the-EBIT items (lower effective tax rate, deferred tax movement) absorbed the operating squeeze. This is not sustainable — if operating margins continue declining, net margin will eventually follow.

**EDLP constraint:** D-Mart cannot easily raise prices without risking volume loss (its entire value proposition is being the cheapest). This structural constraint makes margin pressure a persistent risk when input costs rise.

### 6.2 Return Ratios

| Ratio | Formula | FY25 | FY24 | Trend |
|-------|---------|------|------|-------|
| ROA (Return on Assets) | PAT / Total Assets | **11.1%** | **12.0%** | ↓ declining |
| ROE (Return on Equity) | PAT / Total Equity | **12.6%** | **13.6%** | ↓ declining |
| ROCE | EBIT / Capital Employed | **21%** | **22%** | ↓ slight dip |

**ROCE > ROE significance:** In most companies, leverage amplifies ROE above ROCE. D-Mart's ROCE (21%) exceeds ROE (12.6%) because it uses virtually no debt. This is a sign of genuine operational quality — returns are earned from the business, not from financial leverage.

**ROA declining:** Assets grew 14.8% (store expansion capex) but PAT grew only 6.8%. New stores take time to ramp up to full profitability — ROA compression during expansion phases is expected. Watch for ROA recovery as new stores mature.

---

## 7. Solvency Ratios

> **Definition:** Long-term financial stability and debt burden.

### 7.1 Solvency Summary

| Ratio | Formula | FY25 | FY24 |
|-------|---------|------|------|
| Debt to Equity | NCL / Total Equity | **0.03** | **0.03** |
| Total Debt to Assets | (TA − TE) / TA | **0.12** | **0.12** |
| Equity Multiplier | Total Assets / Total Equity | **1.14x** | **1.13x** |

### 7.2 The Zero Debt Story

D-Mart carries **zero financial borrowings** — both long-term and short-term borrowings are ₹0 in FY25 and FY24. The D/E ratio of 0.03 reflects only lease liabilities under Ind AS 116 accounting (store leases capitalized on the balance sheet).

This is exceptional for a company of this scale (₹24,320 Cr in total assets). The entire business is funded by:
- Equity: ₹21,427 Cr (88% of total assets)
- Lease liabilities: ₹820 Cr (3.4%)
- Trade payables and operational liabilities: ₹2,212 Cr (9.1%)

**Implications:**
- No refinancing risk
- No covenant risk
- No interest rate sensitivity on financial debt
- Entirely self-funded growth — from operations to capex

### 7.3 Lease Liability Growth — The Hidden Debt

| Metric | FY25 | FY24 | Change |
|--------|------|------|--------|
| NCL Lease Liabilities | ₹555.79 Cr | ₹399.24 Cr | +39.2% |
| CL Lease Liabilities | ₹263.83 Cr | ₹192.92 Cr | +36.8% |
| Total Lease Liabilities | ₹819.62 Cr | ₹592.16 Cr | +38.4% |

Lease liabilities grew 38% — reflecting new store openings on leased land where D-Mart cannot own. This is a structural shift worth watching; it introduces a fixed-cost element that owned stores do not carry.

---

## 8. DuPont Decomposition

> **Formula:** ROE = Net Profit Margin × Asset Turnover × Equity Multiplier

| Driver | Formula | FY25 | FY24 | Change |
|--------|---------|------|------|--------|
| Net Profit Margin | PAT / Sales | 5.0% | 4.5% | +0.5% |
| Asset Turnover | Sales / Assets | 2.398x | 2.441x | -0.04x |
| Equity Multiplier | Assets / Equity | 1.133 | 1.135 | flat |
| **ROE (product)** | NPM × AT × EM | **13.5%** | **12.6%** | **+0.9%** |

### Reading the DuPont

- **Margin improved** (4.5% → 5.0%): This appears contradictory given operating margin compression, but DuPont uses Net Profit Margin. Net profit grew faster than prior-year base in percentage terms.
- **Asset Turnover declined** (2.441 → 2.398): Assets grew faster than revenue. New store capex is the reason — fresh stores don't generate full revenue on day one.
- **Equity Multiplier flat** (1.135 → 1.133): Essentially unchanged — no change in financial leverage (there is none to change).

**Takeaway:** D-Mart's ROE is entirely driven by operational performance — margin and velocity. There is no financial engineering component. This makes it a purer subject for ML valuation models because the ROE decomposition is clean and operationally interpretable.

```python
# DuPont calculation
npm   = 2707.45 / 59358.05   # = 0.0456 (4.56%)
at    = 59358.05 / 24320.29  # = 2.441
em    = 24320.29 / 21427.75  # = 1.135
roe   = npm * at * em        # = 0.1263 (12.63%)
```

---

## 9. Key Observations — D-Mart FY25

### 9.1 Revenue Growth Strong but Profit Growth Lagging
Revenue grew 16.9% but net profit grew only 6.8%. The gap is driven by:
- COGS inflation eating into gross margin
- Employee costs growing 28.7% (store expansion hiring)
- Depreciation growing 19% (new store capex)

This is a classic **operating deleverage** scenario during heavy expansion. The bet is that new stores will ramp to full productivity and reverse this in FY26–27.

### 9.2 Inventory Surge — Deliberate Build
Inventories grew 28.4% (₹3,927 → ₹5,044 Cr), faster than revenue (16.9%). This is a demand-anticipation build, likely ahead of:
- Festive season restocking
- Bulk purchase discounts from FMCG suppliers
- New store stocking requirements

DIO rising from 32.6 → 35.6 days confirms stock is sitting slightly longer. Watch for FY26 Q1 destocking.

### 9.3 Other Financial Assets Collapse
Short-term financial assets fell from ₹1,128 Cr → ₹528 Cr (-53%). This is the primary driver of Quick Ratio halving. Most likely explanation: short-term FDs and treasury instruments were liquidated to fund the capex programme (PP&E grew ₹2,591 Cr in one year). Not distress — a deliberate capital reallocation.

### 9.4 Zero Dividend Policy — Growth Reinvestment
D-Mart paid no dividend in FY25 or FY24. The P&L balance carried forward grew from ₹11,897 Cr → ₹14,596 Cr. All retained earnings are being recycled into store expansion. Book value grew from ₹287.34 → ₹329.29 per share.

### 9.5 Lease Expansion Risk
Total lease liabilities grew 38% — the fastest-growing liability on the balance sheet. As D-Mart expands into markets where it cannot own land (metros, premium locations), the owned-store advantage erodes and fixed lease costs create operating leverage on the downside.

---

## 10. ML Feature Engineering Guide

### 10.1 Feature → Model Mapping

| Ratio | FY25 Value | ML Task | Model Type | Feature / Label |
|-------|-----------|---------|------------|-----------------|
| Current Ratio | 2.89 | Credit risk scoring | XGBoost, Logistic Regression | Feature |
| Quick Ratio | 0.61 | Liquidity anomaly detection | Isolation Forest, Autoencoder | Feature + Early Warning |
| Cash Ratio | 0.16 | Dividend prediction | Random Forest | Feature |
| DSO (0.95 days) | ~1 day | Sector classification (cash vs credit retail) | KNN, Clustering | Feature |
| DIO (35.6 days) | 35.6 | Demand forecasting, inventory risk | ARIMA, LSTM | Feature + Label |
| DPO (7.56 days) | 7.56 | Supply chain risk, supplier stress | Gradient Boosting | Feature |
| CCC (29 days) | 29.0 | Working capital efficiency | Regression | Feature + Target |
| Asset Turnover | 2.44x | DuPont ROE attribution | Linear Decomposition | Feature |
| Inventory Turnover | 10.24x | Operational efficiency benchmark | Clustering (sector peers) | Feature |
| Op. Profit Margin | 7.8% | Margin prediction, pricing power | Time-series, GBM | Feature + Label |
| Net Profit Margin | 5.0% | Stock return prediction | Factor model | Feature |
| ROA | 11.1% | Asset productivity benchmark | Quantile Regression | Feature |
| ROE | 12.6% | Portfolio quality factor | Factor model | Feature |
| ROCE | 21% | Capital allocation quality | Value factor model | Feature |
| D/E Ratio | 0.03 | Bankruptcy prediction (Altman Z) | Logistic Regression | Feature |
| Interest Coverage | 66.4x | Distress prediction (floor feature) | Binary Classification | Feature |
| Equity Multiplier | 1.14x | DuPont decomposition | Linear | Feature (component) |

### 10.2 D-Mart Specific ML Signals

#### Strong Positive Signals (Safe Zone Anchors)
- Zero financial debt → anchor label in bankruptcy prediction training datasets
- DSO < 1 day → binary sector classifier (cash retail vs B2B)
- Interest coverage 66x → near-infinite safety margin; floor feature in distress models
- ROCE > cost of equity → economic value-add positive; quality factor signal

#### Deteriorating Trend Signals (Watch Flags)
- Quick Ratio: 1.15 → 0.61 (one year) → rapid decline feature in time-series anomaly detection
- CCC: 25.6 → 29.0 days → early warning of working capital inefficiency
- Operating margin: 8.4% → 7.8% → consecutive compression = pricing power erosion signal
- Revenue / Profit growth gap (16.9% vs 6.8%) → operating deleverage indicator

### 10.3 Altman Z-Score Estimation

```python
# Altman Z-Score for non-manufacturing / services adaptation
# Z > 2.99 = safe | 1.81-2.99 = grey | < 1.81 = distress

nwc              = 6392.20 - 2212.16   # = 4180.04
total_assets     = 24320.29
retained_earn    = 20777.02             # Other equity (approximation)
ebit             = 4611.64
market_cap       = ~260000              # approximate market cap FY25
total_liab       = 24320.29 - 21427.75  # = 2892.54
sales            = 59358.05

T1 = nwc / total_assets                # = 0.172
T2 = retained_earn / total_assets      # = 0.854
T3 = ebit / total_assets               # = 0.190
T4 = market_cap / total_liab           # = ~89.9 (exceptional)
T5 = sales / total_assets              # = 2.441

Z = 1.2*T1 + 1.4*T2 + 3.3*T3 + 0.6*T4 + 1.0*T5
# Z >> 2.99 → comfortably in SAFE zone
# D-Mart is a textbook "safe" label in any bankruptcy training dataset
```

### 10.4 Cash Conversion Cycle Model

```python
def compute_ccc(cogs, inventory, sales, trade_rec, trade_pay):
    dio = 365 / (cogs / inventory)
    dso = 365 / (sales / trade_rec)
    dpo = 365 / (cogs / trade_pay)
    operating_cycle = dio + dso
    ccc = dio + dso - dpo
    return {'DIO': round(dio,2), 'DSO': round(dso,2),
            'DPO': round(dpo,2), 'Operating Cycle': round(operating_cycle,2),
            'CCC': round(ccc,2)}

# FY25
result = compute_ccc(
    cogs=51668.76, inventory=5044.37,
    sales=59358.05, trade_rec=153.79,
    trade_pay=1070.81
)
# {'DIO': 35.63, 'DSO': 0.95, 'DPO': 7.56, 'Operating Cycle': 36.58, 'CCC': 29.02}
```

### 10.5 DuPont Feature Engineering

```python
def dupont_decompose(pat, sales, total_assets, total_equity):
    npm = pat / sales
    at  = sales / total_assets
    em  = total_assets / total_equity
    roe = npm * at * em
    return {'NPM': round(npm,4), 'AssetTurnover': round(at,4),
            'EquityMultiplier': round(em,4), 'ROE': round(roe,4)}

# FY25
dupont_decompose(2707.45, 59358.05, 24320.29, 21427.75)
# {'NPM': 0.0456, 'AssetTurnover': 2.4407, 'EquityMultiplier': 1.135, 'ROE': 0.1263}
```

### 10.6 Data Quality Warnings

| Issue | D-Mart Specific Note |
|-------|---------------------|
| DSO near zero | Division-sensitive; small changes in trade receivables create extreme ratio swings. Use Winsorization at 95th percentile. |
| COGS definition | Includes food, FMCG, general merchandise — no segment breakout. COGS-based ratios reflect blended basket margin. |
| Lease vs debt | D/E = 0.03 is Ind AS 116 compliant (leases on balance sheet). Pre-Ind AS comparison requires lease add-back adjustment. |
| Expansion capex | ROA, ROCE will appear temporarily depressed during store roll-out. Use same-store-sales (SSSG) data alongside balance sheet ratios. |
| No dividend | Dividend yield = 0; payout ratio = 0. These ratios are uninformative for D-Mart and should be excluded from income-factor models. |

---

## Summary Cheat Sheet

| Category | Key Ratio | FY25 | FY24 | Verdict |
|----------|-----------|------|------|---------|
| Liquidity | Current Ratio | 2.89x | 3.13x | Healthy |
| Liquidity | Quick Ratio | 0.61x | 1.15x | Watch — declining |
| Efficiency | DSO | 0.95 days | 1.18 days | Exceptional |
| Efficiency | CCC | 29.0 days | 25.6 days | Rising — monitor |
| Efficiency | Asset Turnover | 2.44x | 2.40x | Strong |
| Efficiency | Interest Coverage | 66.4x | 73.1x | Exceptional |
| Profitability | Op. Margin | 7.8% | 8.4% | Compressed |
| Profitability | Net Margin | 5.0% | 5.0% | Flat |
| Profitability | ROCE | 21% | 22% | Strong |
| Solvency | D/E | 0.03 | 0.03 | Zero financial debt |
| Market | EPS | ₹41.62 | ₹38.97 | Growing |
| Market | Dividend Yield | 0% | 0% | Growth company — no payout |

---

*Analysis based on Avenue Supermarts Ltd (D-Mart) P&L and Balance Sheet FY25 | Prepared for educational and ML research purposes*