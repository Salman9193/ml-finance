# COGS & Inventory Mechanics — Deep Explainer
> How Cost of Goods Sold is calculated, what inventory movement means for profit, and why it matters for financial analysis and ML

---

## Table of Contents
1. [The Core Formula](#1-the-core-formula)
2. [Two Ways to Calculate COGS](#2-two-ways-to-calculate-cogs)
3. [Company A — The Inventory Builder](#3-company-a--the-inventory-builder)
4. [Company B — The Inventory Destacker](#4-company-b--the-inventory-destacker)
5. [Head-to-Head Comparison (2025)](#5-head-to-head-comparison-2025)
6. [The Key Mechanic — Situation → Impact](#6-the-key-mechanic--situation--impact)
7. [Key Learnings](#7-key-learnings)
8. [Real World Examples](#8-real-world-examples-titan--d-mart)
9. [ML Feature Engineering Guide](#9-ml-feature-engineering-guide)

---

## 1. The Core Formula

```
COGS = Opening Stock + Purchases − Closing Stock
```

**What this means in plain English:**

You started the year with some stock. You bought more. At the end of the year, you still have some left. Whatever you don't have left — that's what you sold (or consumed). That cost is COGS.

COGS is **not** simply "what you bought this year." It is what you actually consumed or sold — adjusted for inventory movement.

### Why This Matters

| If you buy ₹100 of goods and sell all of it | COGS = ₹100 (no inventory left) |
|---------------------------------------------|----------------------------------|
| If you buy ₹100 but keep ₹30 unsold | COGS = ₹70 (₹30 stays on Balance Sheet as asset) |
| If you buy ₹100 but also sell ₹20 of old stock | COGS = ₹120 (old inventory cost flows into P&L) |

---

## 2. Two Ways to Calculate COGS

Both methods give **identical results** — the document shows both as a reconciliation check.

### Method 1 — Balance Sheet Approach
```
COGS = Opening Stock + Purchases − Closing Stock
```

### Method 2 — P&L Approach
```
COGS = Purchases + Change in Inventory
     where: Change in Inventory = Opening Stock − Closing Stock
```

If Closing Stock > Opening Stock → Change in Inventory is **negative** → COGS < Purchases

If Closing Stock < Opening Stock → Change in Inventory is **positive** → COGS > Purchases

### Verification Example (Company A, 2025)
```
Method 1: 2341 + 3457 − 3163 = 2635 ✓
Method 2: 3457 + (2341 − 3163) = 3457 − 822 = 2635 ✓
```

---

## 3. Company A — The Inventory Builder

> Closing Stock > Opening Stock every year → Accumulating inventory → COGS < Purchases

### Raw Data

| Item | 2025 | 2024 | 2023 |
|------|------|------|------|
| Opening Stock | 2,341 | 1,954 | 1,736 |
| Purchases | 3,457 | 4,381 | 5,250 |
| Closing Stock | **3,163** | **2,341** | **1,954** |
| **COGS** | **2,635** | **3,994** | **5,032** |

### P&L Reconciliation

| Item | 2025 | 2024 | 2023 |
|------|------|------|------|
| Purchases | 3,457 | 4,381 | 5,250 |
| Change in Inventory (Op − Cl) | **-822** | **-387** | **-218** |
| **COGS** | **2,635** | **3,994** | **5,032** |

### What is Happening

Every single year, Closing Stock exceeds Opening Stock. The company is consistently **building up inventory** — buying more than it sells.

**The negative Change in Inventory** means a portion of purchase cost is being deferred to the Balance Sheet rather than recognised in the P&L. In 2025, ₹822 of cost was deferred this way.

### Year-by-Year Inventory Build

| Year | Inventory Build (Cl − Op) | Cost Deferred |
|------|--------------------------|---------------|
| 2023 | 1,954 − 1,736 = **+218** | ₹218 deferred |
| 2024 | 2,341 − 1,954 = **+387** | ₹387 deferred |
| 2025 | 3,163 − 2,341 = **+822** | ₹822 deferred |

The deferral is **accelerating** — each year more cost is being pushed forward. Meanwhile, purchases are falling (5,250 → 4,381 → 3,457). This means Company A is relying more and more on old, accumulated inventory and deferring the cost recognition aggressively.

### Financial Impact on Company A

- COGS is significantly lower than purchases each year
- Gross profit and net profit appear **higher than cash reality**
- Cash has been spent on purchasing goods — but that cash is now locked in unsold inventory (₹3,163 by 2025)
- **Profit ≠ Cash Flow** — this is the critical divergence

---

## 4. Company B — The Inventory Destacker

> Closing Stock < Opening Stock every year → Drawing down inventory → COGS > Purchases

### Raw Data

| Item | 2025 | 2024 | 2023 |
|------|------|------|------|
| Opening Stock | 589 | 1,198 | 1,476 |
| Purchases | 2,438 | 3,570 | 4,563 |
| Closing Stock | **410** | **589** | **1,198** |
| **COGS** | **2,617** | **4,179** | **4,841** |

### P&L Reconciliation

| Item | 2025 | 2024 | 2023 |
|------|------|------|------|
| Purchases | 2,438 | 3,570 | 4,563 |
| Change in Inventory (Op − Cl) | **+179** | **+609** | **+278** |
| **COGS** | **2,617** | **4,179** | **4,841** |

### What is Happening

Every single year, Closing Stock is below Opening Stock. Company B is consistently **drawing down old inventory** — selling more than it buys in that period.

**The positive Change in Inventory** means old inventory cost from prior years is flowing into this year's P&L. In 2025, ₹179 of old stock cost was recognised as current-year expense.

### Year-by-Year Inventory Draw-Down

| Year | Inventory Change (Cl − Op) | Old Cost Recognised |
|------|-----------------------------|---------------------|
| 2023 | 1,198 − 1,476 = **-278** | ₹278 of old stock expensed |
| 2024 | 589 − 1,198 = **-609** | ₹609 of old stock expensed |
| 2025 | 410 − 589 = **-179** | ₹179 of old stock expensed |

### Financial Impact on Company B

- COGS is **higher** than purchases each year — old inventory cost is recognised
- Profit appears **lower** than if they had bought the same goods this year
- But cash position is healthier — they are converting old inventory to cash
- Inventory has shrunk from ₹1,476 → ₹410 over 3 years — cash is being freed up

---

## 5. Head-to-Head Comparison (2025)

| Metric | Company A | Company B | Insight |
|--------|-----------|-----------|---------|
| Purchases | 3,457 | 2,438 | A bought ₹1,019 more |
| Opening Stock | 2,341 | 589 | A carried more old stock |
| Closing Stock | 3,163 | 410 | A holds 7.7x more inventory |
| Change in Inventory | **-822** | **+179** | Opposite directions |
| **COGS** | **2,635** | **2,617** | **Almost identical!** |

### The Shocking Takeaway

Despite Company A spending ₹1,019 more on purchases, both companies reported **virtually the same COGS** (₹2,635 vs ₹2,617).

- Company A bought more but deferred ₹822 as inventory — so COGS appears low
- Company B bought less but recognised ₹179 of old stock — so COGS appears slightly higher

**If you only looked at COGS, you would think these companies had identical cost structures. They don't.**

---

## 6. The Key Mechanic — Situation → Impact

| Situation | Accounting Impact | Cash Impact | Risk |
|-----------|------------------|-------------|------|
| Higher Closing Inventory | Lower COGS | Cash locked in stock | Inventory write-down risk |
| Lower Closing Inventory | Higher COGS | Cash freed up | Restocking risk |
| Inventory accumulating (like A) | Profit boosted | Cash consumed | Earnings quality concern |
| Inventory shrinking (like B) | Profit reduced | Cash released | May signal demand slowdown |

### The Profit vs Cash Divergence — Critical Concept

```
When Closing Stock RISES:
  ┌─────────────────────────────────────────────────┐
  │  Cash spent on purchases    → Cash OUTFLOW      │
  │  Cost not yet in P&L        → Profit HIGHER     │
  │  Inventory on Balance Sheet → Asset HIGHER      │
  │  Operating Cash Flow        → LOWER than profit │
  └─────────────────────────────────────────────────┘

When Closing Stock FALLS:
  ┌─────────────────────────────────────────────────┐
  │  Old inventory sold         → Cash INFLOW       │
  │  Old cost now in P&L        → Profit LOWER      │
  │  Inventory on Balance Sheet → Asset LOWER       │
  │  Operating Cash Flow        → HIGHER than profit│
  └─────────────────────────────────────────────────┘
```

This is why **Cash Flow from Operations** is always compared against **Net Profit** in financial analysis. A sustained gap where profit >> operating cash flow is a red flag — it often points to inventory accumulation (or aggressive receivables management).

---

## 7. Key Learnings

### When Closing Inventory is Higher (Company A pattern)

1. The firm produced or purchased more than it sold
2. Some cost is carried forward as an asset (inventory on Balance Sheet)
3. Therefore, expense recognised this year is lower
4. Profit increases — but it's partly an accounting effect, not a cash reality
5. Cash is locked in unsold stock and cannot be used for operations or dividends

### When Closing Inventory is Lower (Company B pattern)

1. The firm sold more than it produced or purchased
2. Old inventory cost is now being recognised as current expense
3. COGS is higher than purchases
4. Profit is lower — but cash is actually being freed up
5. This can also signal demand is strong (selling fast) or a deliberate destocking strategy

### The Matching Principle Connection

This entire mechanic flows from the **Matching Concept** of accounting:

> Expenses must be matched to the revenue they helped generate — in the same accounting period.

If inventory was purchased but not yet sold, it hasn't generated revenue yet. Therefore its cost cannot be an expense yet. It waits on the Balance Sheet until the sale happens. This is precisely why COGS deducts unsold closing stock.

---

## 8. Real World Examples — Titan & D-Mart

### Titan (FY25) — Extreme Version of Company A

| Item | FY25 | FY24 | Change |
|------|------|------|--------|
| Inventories | ₹25,047 Cr | ₹16,074 Cr | +55.8% |
| Short-term Borrowings | ₹7,483 Cr | ₹2,670 Cr | +180% |

Titan built up ₹8,973 Cr of additional inventory in one year — funded by short-term borrowings. This:
- Deferred massive COGS (gold cost not yet in P&L)
- Made reported profit look better than cash reality
- Locked cash in gold stock
- Created refinancing risk on short-term debt used to fund inventory

**The inventory build IS the balance sheet story for Titan FY25.**

### D-Mart (FY25) — Moderate Company A Pattern

| Item | FY25 | FY24 | Change |
|------|------|------|--------|
| Inventories | ₹5,044 Cr | ₹3,927 Cr | +28.4% |
| DIO (Inventory Days) | 35.6 days | 32.6 days | Rising |

D-Mart also built inventory (new store stocking) but at a much healthier pace. Its CCC of 29 days and DSO of ~1 day mean inventory converts to cash quickly — far less concerning than Titan's gold accumulation.

---

## 9. ML Feature Engineering Guide

### 9.1 COGS-Derived Features

```python
# Core COGS calculation — always verify both methods match
def compute_cogs(opening_stock, purchases, closing_stock):
    cogs_method1 = opening_stock + purchases - closing_stock
    change_in_inv = opening_stock - closing_stock
    cogs_method2 = purchases + change_in_inv
    assert abs(cogs_method1 - cogs_method2) < 0.01, "COGS reconciliation failed"
    return cogs_method1

# Inventory change direction feature
def inventory_direction(opening, closing):
    delta = closing - opening
    if delta > 0:
        return 'building'    # Cost deferred → profit boosted
    elif delta < 0:
        return 'destocking'  # Old cost recognised → profit reduced
    else:
        return 'stable'
```

### 9.2 Key Ratios from COGS + Inventory

```python
# Inventory Turnover — how fast stock moves
inventory_turnover = cogs / avg_inventory
# where avg_inventory = (opening + closing) / 2

# Days Inventory Outstanding (DIO)
dio = 365 / inventory_turnover

# COGS as % of Sales — gross margin health
cogs_pct = cogs / net_sales

# Purchase to COGS ratio — signals inventory accumulation
# > 1.0 → building inventory (like Company A)
# < 1.0 → destocking (like Company B)
# = 1.0 → stable inventory
purchase_to_cogs = purchases / cogs

# Inventory build rate
inv_build_rate = (closing_stock - opening_stock) / opening_stock
```

### 9.3 Red Flag Detection

```python
# Red Flag 1: Profit-Cash Divergence
# Sustained inventory build → profit >> cash flow from operations
def profit_cash_divergence(net_profit, cfo, threshold=0.20):
    divergence = (net_profit - cfo) / net_profit
    if divergence > threshold:
        return 'WARNING: Profit significantly exceeds cash flow — check inventory'
    return 'OK'

# Red Flag 2: Accelerating Inventory Build
# Each year, more cost is being deferred — like Company A
def inventory_acceleration(inv_changes: list):
    # inv_changes = [218, 387, 822] for Company A
    if all(inv_changes[i] < inv_changes[i+1] for i in range(len(inv_changes)-1)):
        return 'WARNING: Inventory deferral is accelerating year over year'
    return 'OK'

# Red Flag 3: Inventory grows faster than revenue
def inventory_vs_revenue_growth(inv_growth_pct, rev_growth_pct):
    if inv_growth_pct > rev_growth_pct * 1.5:
        return 'WARNING: Inventory growing 1.5x faster than revenue — demand concern'
    return 'OK'

# Example — Titan FY25
profit_cash_divergence(net_profit=3496, cfo=1200)     # approximate
inventory_vs_revenue_growth(inv_growth_pct=0.558, rev_growth_pct=0.179)
# → WARNING: Inventory growing 1.5x faster than revenue
```

### 9.4 ML Use Cases

| Feature | What It Detects | Model Type |
|---------|----------------|------------|
| `purchase_to_cogs` ratio | Inventory accumulation vs destocking | Anomaly detection |
| `inv_build_rate` YoY | Accelerating deferral pattern | Time-series (LSTM) |
| `profit - cfo` gap | Earnings quality risk | Classification (fraud/quality) |
| `cogs_pct_sales` trend | Gross margin compression | Regression / forecasting |
| `dio` trend | Inventory efficiency change | Demand forecasting |
| `inventory_direction` | Buy-more vs sell-through signal | Clustering |
| Inventory / Total Assets % | Balance sheet concentration | Credit risk scoring |

### 9.5 Earnings Quality Score — Inventory Component

```python
def inventory_earnings_quality_score(data: dict) -> float:
    """
    Score from 0 (poor quality) to 1 (high quality)
    Based on inventory-related signals
    data keys: purchases, cogs, opening_stock, closing_stock,
               net_profit, cfo, revenue
    """
    score = 1.0

    # Penalty 1: Inventory building faster than revenue
    inv_growth = (data['closing_stock'] - data['opening_stock']) / data['opening_stock']
    rev_growth  = data.get('revenue_growth', 0)
    if inv_growth > rev_growth * 1.5:
        score -= 0.25

    # Penalty 2: Purchase to COGS ratio well above 1
    p2c = data['purchases'] / data['cogs']
    if p2c > 1.20:
        score -= 0.20

    # Penalty 3: Profit significantly exceeds operating cash flow
    if data['cfo'] < data['net_profit'] * 0.70:
        score -= 0.30

    # Penalty 4: DIO rising significantly
    avg_inv = (data['opening_stock'] + data['closing_stock']) / 2
    dio = 365 / (data['cogs'] / avg_inv)
    if dio > data.get('prior_dio', dio) * 1.15:
        score -= 0.15

    return max(0.0, round(score, 2))
```

---

## Summary Cheat Sheet

| Concept | Formula | Signal |
|---------|---------|--------|
| COGS | Op. Stock + Purchases − Cl. Stock | Core cost of revenue |
| Change in Inventory | Op. Stock − Cl. Stock | Negative = building; Positive = destocking |
| Purchase to COGS | Purchases / COGS | > 1 = inventory build; < 1 = destocking |
| Inventory Turnover | COGS / Avg Inventory | Higher = faster moving stock |
| DIO | 365 / Inventory Turnover | Lower = more efficient |
| Profit-Cash Gap | Net Profit − CFO | Large positive gap = earnings quality risk |
| Earnings Quality | CFO / Net Profit | < 0.7 = warning zone |

---

## One-Page Mental Model

```
Every rupee of goods you buy has two possible destinations:

    ┌──────────────────────────────────┐
    │         Purchases (₹X)          │
    └──────────────┬───────────────────┘
                   │
         ┌─────────┴──────────┐
         │                    │
    ┌────▼─────┐         ┌────▼──────────┐
    │   SOLD   │         │  UNSOLD       │
    │          │         │  (Closing     │
    │ → COGS   │         │   Stock)      │
    │ → P&L    │         │ → Balance     │
    │ → Profit │         │   Sheet       │
    │   impact │         │ → Future COGS │
    └──────────┘         └───────────────┘

The COGS formula simply routes each rupee to the right destination.
```

---

*Prepared as part of financial analysis and ML feature engineering learning series*