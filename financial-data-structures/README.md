# Financial Data Structures & ML Evaluation

Covers Sessions 3 and 4 — the two foundational sessions on how financial data is structured and how ML models should be evaluated in a finance context.

---

## Contents

### [financial-data-structures.md](financial-data-structures.md)
**Financial Data Structures & Pitfalls**

Covers the three data structures that underpin all financial ML work — time-series, cross-sectional, and panel data — and the two biases that silently destroy model performance: look-ahead bias and survivorship bias. Each concept is grounded in worked examples using financial ratios (current ratio, ROE, D/E, asset turnover).

Topics:
- Time-series properties: autocorrelation, volatility clustering, non-stationarity
- Cross-sectional data: relative ranks, normalisation pitfalls
- Panel data: firm entry/exit, stale fundamentals, non-synchronous reporting
- Non-stationarity and market regimes (COVID, rate cycles)
- Look-ahead bias: the filing date delay problem, index membership trap
- Survivorship bias: delisted stocks, bankrupt firms, fund closures
- Best practices checklist for bias-free data pipelines

---

### [ml-model-evaluation.md](ml-model-evaluation.md)
**Finance for ML Practitioners & Model Evaluation**

Covers the core shift from ML accuracy to financial economic value. Demonstrates through worked examples why the model with the best AUC is often not the best financial decision. Includes threshold optimisation, stability vs peak performance trade-offs, explainability requirements under Indian regulation (RBI, IRDAI), and model governance.

Topics:
- Why accuracy ≠ economic value in finance
- Worked example: credit approval — Model A (90% accuracy) vs Model B (85% accuracy, higher profit)
- Decision thresholds: loan default and fraud detection examples
- Short-term vs long-term model stability (3-year AUC degradation case)
- Explainability requirements: RBI digital lending guidelines, IRDAI oversight
- Model governance: three lines of defence, model lifecycle, types of model risk
- Live case: "The Profitable but Risky Model" — board-level deployment decision

---

## Key Takeaways Across Both Sessions

| Session 3 | Session 4 |
|-----------|-----------|
| Financial data violates i.i.d. assumptions | Accuracy is not the financial objective |
| Look-ahead bias silently inflates performance | Economic value weights errors by money at stake |
| Survivorship bias inflates returns and hides risk | Stability over time beats peak backtest performance |
| Structure and timing matter as much as features | Governance and explainability are non-negotiable |
| Good data discipline beats complex models | Model deployment opens monitoring, not closes it |
