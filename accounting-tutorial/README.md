# Accounting Tutorial

A practical accounting and financial analysis series built around two real Indian listed companies — **Titan Company** and **Avenue Supermarts (D-Mart)**. Each guide pairs traditional accounting concepts with ML feature engineering insights, making them useful for both finance learning and building quantitative models.

---

## Contents

### [titan-balance-sheet-pl.md](titan-balance-sheet-pl.md)
**Titan Company — Financial Analysis & ML Feature Guide**

Deep-dive into Titan's FY25 standalone balance sheet and P&L. Covers foundational accounting concepts and conventions (business entity, going concern, accrual, etc.) before walking through the full ratio suite — liquidity, leverage, efficiency, profitability, and market/valuation ratios — all grounded in Titan's actual numbers. Ends with an ML feature engineering guide showing how each ratio translates into a model-ready signal.

---

### [dmart-financial-analysis.md](dmart-financial-analysis.md)
**Avenue Supermarts (D-Mart) — Financial Analysis & ML Feature Guide**

Analysis of D-Mart's FY25 vs FY24 financials. Starts with D-Mart's EDLP (Everyday Low Price) business model — owned stores, zero debt, cash-and-carry — then covers the P&L, balance sheet, and ratios (liquidity, efficiency, profitability, solvency). Includes a DuPont decomposition and an ML feature engineering guide tailored to high-volume, low-margin retail dynamics.

---

### [dmart-vs-titan.md](dmart-vs-titan.md)
**Titan vs D-Mart — Side-by-Side Comparison & ML Guide**

Head-to-head comparison of two fundamentally different business models: Titan (premium brand, capital-light, franchise channel) vs D-Mart (EDLP grocery, asset-heavy, cash-and-carry). Covers P&L and balance sheet side by side, a ratio scorecard across all categories, DuPont comparison, and key strategic differences. The ML comparative feature guide shows how cross-company ratio deltas can be used as features for sector classification or relative-value models.

---

### [cogs-inventory.md](cogs-inventory.md)
**COGS & Inventory Mechanics — Deep Explainer**

Concept-focused guide on how Cost of Goods Sold is actually calculated and why inventory movement matters. Walks through the core formula (`COGS = Opening Stock + Purchases − Closing Stock`), contrasts an inventory-building company vs an inventory-destacking company with worked numerical examples, and maps the mechanics to Titan and D-Mart real-world data. Includes an ML feature engineering guide on inventory-derived signals.

---

## Supporting File

- **Ratio Analysis_Case_ML_Finance_14_02_26-1.xlsx** — Excel workbook with the ratio analysis case study data used across the guides.

---

## Learning Path

If you are new to financial analysis, follow this order:

1. [cogs-inventory.md](cogs-inventory.md) — understand the mechanics of cost and inventory before reading statements
2. [titan-balance-sheet-pl.md](titan-balance-sheet-pl.md) — learn accounting concepts and the full ratio suite on a single company
3. [dmart-financial-analysis.md](dmart-financial-analysis.md) — apply the same framework to a contrasting business model
4. [dmart-vs-titan.md](dmart-vs-titan.md) — consolidate by comparing both companies side by side
