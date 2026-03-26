# Finance for ML Practitioners & Model Evaluation

---

## Table of Contents
1. [Why Accuracy Is Not Enough in Finance](#1-why-accuracy-is-not-enough-in-finance)
2. [Economic Value vs Prediction Quality](#2-economic-value-vs-prediction-quality)
3. [Decision Thresholds & Asymmetric Costs](#3-decision-thresholds--asymmetric-costs)
4. [Short-Term Performance vs Long-Term Stability](#4-short-term-performance-vs-long-term-stability)
5. [Explainability & Regulatory Requirements](#5-explainability--regulatory-requirements)
6. [Model Governance in Financial Institutions](#6-model-governance-in-financial-institutions)
7. [Live Case — The Profitable but Risky Model](#7-live-case--the-profitable-but-risky-model)

---

## 1. Why Accuracy Is Not Enough in Finance

In a standard ML course, the goal is to maximise accuracy, minimise RMSE, or push AUC as high as possible. The model with the best metric wins.

Finance breaks this rule completely.

| Standard ML | Finance |
|-------------|---------|
| Better metric = better model | Better metric ≠ better outcome |
| Errors are symmetric | Errors have different monetary costs |
| Predictions are the end goal | Predictions feed decisions that allocate capital |
| Optimise for the test set | Optimise for risk-adjusted return over time |

The key shift is:

```
Prediction  →  Decision  →  Economic Outcome
```

A prediction is only valuable if it leads to a good decision. A decision is only good if it generates value after accounting for risk, cost, and regulatory constraints.

### What changes when ML enters finance

**ML objective:**
- Minimise loss function (RMSE, cross-entropy)
- Maximise accuracy / AUC

**Financial objective:**
- Maximise risk-adjusted return
- Protect capital during downturns
- Meet regulatory constraints
- Ensure model stability and auditability over time

> **Models in finance allocate capital, not just predict outcomes.** A 1% improvement in AUC means nothing if it comes with a 20% increase in tail losses during a recession.

---

## 2. Economic Value vs Prediction Quality

### The core problem: accuracy treats all errors equally

Accuracy = Correct Predictions / Total Predictions

Finance weights errors by the **money at stake**. A false negative that approves a defaulting borrower costs ₹60 lakh. A false positive that rejects a good borrower costs ₹0 in credit loss (just opportunity cost).

**Economic Value = (TP × Gain) − (FP × Cost) − (FN × Opportunity Cost)**

Finance cares about the second equation, not the first.

---

### Worked Example: Credit Approval Model

**Setup:** A bank uses an ML model to approve or reject loans.
- Dataset: 1,000 applicants
- True bad borrowers (will default): 100
- True good borrowers: 900
- Profit from approving a good borrower: +$200
- Loss from approving a bad borrower (default): −$1,000
- Rejecting any borrower: $0

**Two competing models:**

| Metric | Model A | Model B |
|--------|---------|---------|
| Accuracy | **90%** | 85% |
| Bad borrowers correctly rejected (TP) | 40 | 80 |
| Bad borrowers approved (FN) | 60 | 20 |
| Good borrowers rejected (FP) | 40 | 130 |
| Good borrowers approved (TN) | 860 | 770 |

From a pure ML view, **Model A wins** (90% accuracy).

**Economic calculation:**

Model A:
- Good approvals: 860 × $200 = **+$172,000**
- Bad approvals: 60 × $1,000 = **−$60,000**
- **Net Economic Value = $112,000**

Model B:
- Good approvals: 770 × $200 = **+$154,000**
- Bad approvals: 20 × $1,000 = **−$20,000**
- **Net Economic Value = $134,000**

**Model B has lower accuracy but generates $22,000 more profit.**

Why? Because in this problem, each bad approval costs 5× more than each missed good approval. Model B's more conservative stance — catching 40 more defaulters even at the cost of 90 more rejections — is the right financial trade-off.

> **Accuracy rewards being mostly right. Finance rewards being right where money is at risk.**

---

## 3. Decision Thresholds & Asymmetric Costs

### Thresholds are a financial decision, not a model parameter

ML classification models output a probability (e.g., probability of default = 0.42). A threshold converts this into a binary decision: approve or reject. The default threshold of 0.5 is statistically neutral — it minimises errors equally. But in finance, errors are not equal.

**The optimal threshold is the one that maximises expected economic value, not accuracy.**

---

### Example A: Loan Default Model

**Setup:**
- Loan value = $100,000
- Profit if repaid = $10,000
- Loss if default = −$60,000
- Model outputs probability of default (PD)

**Case A — Threshold = 0.5 (accuracy-focused):**

| | Actual Good | Actual Bad |
|--|------------|-----------|
| Approve | 70 | 10 |
| Reject | 10 | 10 |

- Accuracy = 80%
- 70 good approvals → 70 × $10k = **+$700k**
- 10 bad approvals → 10 × −$60k = **−$600k**
- **Net = +$100k**

**Case B — Threshold = 0.3 (more conservative):**

| | Actual Good | Actual Bad |
|--|------------|-----------|
| Approve | 60 | 5 |
| Reject | 20 | 15 |

- Accuracy = 75%
- 60 good approvals → 60 × $10k = **+$600k**
- 5 bad approvals → 5 × −$60k = **−$300k**
- **Net = +$300k**

**Lower accuracy, 3× higher profit.** Because the asymmetry is extreme (default loss = 6× profit), even aggressive threshold tightening is economically justified.

---

### Example B: Credit Card Fraud Detection

**Setup:**
- Fraud transaction loss: −$1,000
- Legitimate transaction wrongly blocked: −$10 (customer friction)

**Threshold = 0.5:**
- 20 fraud cases missed → 20 × −$1,000 = **−$20,000**
- 100 legitimate blocked → 100 × −$10 = **−$1,000**
- **Total loss = −$21,000**

**Threshold = 0.3 (more aggressive fraud detection):**
- 5 fraud cases missed → 5 × −$1,000 = **−$5,000**
- 300 legitimate blocked → 300 × −$10 = **−$3,000**
- **Total loss = −$8,000**

**The more "accurate" model (threshold 0.5) loses 2.6× more money** because the fraud cost is 100× the friction cost.

**Rule:** When false negative cost >> false positive cost, push threshold down to catch more positives (fraud, defaults). When false positive cost >> false negative cost, push threshold up.

---

### How to set the optimal threshold

```
Threshold* = Cost(FP) / [Cost(FP) + Cost(FN)]
```

In the loan example:
- Cost(FP) = $0 (rejecting a good borrower costs nothing in credit loss)
- Cost(FN) = $60,000 (approving a bad borrower causes a default loss)
- Threshold* ≈ 0 / (0 + 60,000) → very low threshold (be very conservative)

In the fraud example:
- Cost(FP) = $10 (blocking a good transaction)
- Cost(FN) = $1,000 (missing fraud)
- Threshold* = 10 / (10 + 1,000) ≈ 0.01 → push threshold very low

---

## 4. Short-Term Performance vs Long-Term Stability

### The trade-off

A model that achieves top performance today may be exploiting temporary patterns — regime-specific correlations that will not persist. A model that is more stable but slightly less accurate is often more valuable in production.

| Dimension | High-Performance Model | Stable Model |
|-----------|----------------------|-------------|
| AUC (Year 1) | Higher | Slightly lower |
| Feature set | Many, including behavioural / alternative data | Fewer, economically interpretable |
| Retraining frequency | Frequent (every 3 months) | Annual |
| Performance over time | Degrades as regime shifts | Persists |
| Regulatory approval | Harder | Easier |

---

### Worked Example: Credit Risk Model Lifecycle

**Model A — High Performance:**
- AUC = 0.87 (Year 1)
- Uses behavioural features and short-term trend signals
- Retrained quarterly

**Model B — Stable:**
- AUC = 0.83 (Year 1)
- Uses economically interpretable features (income, leverage, payment history)
- Retrained annually

**3-year AUC comparison:**

| Year | Model A | Model B |
|------|---------|---------|
| 1 | 0.87 | 0.83 |
| 2 | 0.78 | 0.82 |
| 3 | 0.74 | 0.81 |

Model A degrades because:
- The behavioural signals it relied on were regime-specific (low-rate bull market)
- Economic cycle shifted; new customers behave differently
- Short-term patterns that looked predictive were noise

**Financial impact:**

| | Model A | Model B |
|--|---------|---------|
| Year 1 profit | +$10M | +$8M |
| Year 2–3 impact | −$6M (unexpected defaults) | Steady +$8M/year |
| Loss volatility | High | Low |
| Regulatory friction | Higher | Lower |

Model A's 3-year cumulative profit: $10M − $6M = $4M
Model B's 3-year cumulative profit: $24M

> **In finance, consistency often beats peak accuracy. Peak performance frequently comes from exploiting temporary patterns. Stability comes from modelling structural relationships.**

---

### Why models degrade in finance

1. **Data drift** — the distribution of input features shifts as the customer base changes or economic conditions evolve.
2. **Concept drift** — the relationship between features and the target changes. A feature that predicted default in 2019 may behave differently in 2023.
3. **Overfitting to recent patterns** — a model retrained quarterly will overfit to whatever regime was most recent.
4. **Regime change** — a model trained in a low-rate environment has never seen what high rates do to leveraged borrowers.

**Mitigation:**
- Include features that are economically grounded (they tend to be more stable than behavioural proxies)
- Monitor Population Stability Index (PSI) for input features
- Monitor performance stability metrics (not just level) over rolling windows
- Build regime indicators as explicit features

---

## 5. Explainability & Regulatory Requirements

### Why explainability is a financial constraint, not a preference

In most ML applications, a black-box model is acceptable if it is accurate. In finance, black-box models create **legal, regulatory, and reputational risk**.

### Indian regulatory context

**RBI's fair practices codes and digital lending guidelines (2022):**
Customers must be informed of reasons for rejection and key risk factors. Saying "the model score was too low" is not sufficient.

If an ML model denies credit based on:
- Cash flow patterns
- Alternative data (mobile usage, app behaviour)
- Thin-file risk proxies

The institution must be able to state in plain language:
- "Income volatility concerns"
- "Insufficient credit history"
- "High leverage indicators"

**IRDAI (Insurance Regulatory and Development Authority):**
ML models used in motor pricing, health underwriting, or fraud detection must demonstrate:
- Actuarial justification for pricing variation
- Non-arbitrary treatment across geographies/demographics
- Statistical validity of risk drivers

**Account Aggregator & UPI ecosystem:**
As banks and fintechs use bank statement cash flows, GST filings, and transaction-level behavioural signals for MSME lending:
- They must justify why volatility increased probability of default
- They must explain why certain transaction patterns reduce eligibility

> **Complex embeddings without clear reasoning increase audit risk. Simple, interpretable models reduce regulatory friction.**

---

### Three levels of explainability required in practice

| Level | Question | Example |
|-------|----------|---------|
| **Customer-level** | Why was this applicant rejected? | "Your debt-to-income ratio of 0.68 exceeds our threshold of 0.50" |
| **Portfolio-level** | Why does a segment have lower approval rates? | "MSME sector PD rose 3pp due to cash flow volatility post-rate hike" |
| **Regulatory reproducibility** | Can the validation team replicate the model? | Full documentation of features, transformations, training data, and validation methodology |

### Practical tools for explainability

| Tool | What it does | When to use |
|------|-------------|-------------|
| **SHAP values** | Per-prediction feature contribution scores | Customer-level explanations |
| **LIME** | Local linear approximation of black-box | Ad-hoc decision explanation |
| **Logistic Regression** | Inherently interpretable | When regulatory clarity is paramount |
| **Decision Trees** | Rule-based, fully transparent | Scorecard-style models |
| **Feature importance plots** | Global ranking of model drivers | Portfolio-level justification |

---

## 6. Model Governance in Financial Institutions

### Models are regulated assets

In finance, a model is not just a technical artifact — it is a **regulated asset** subject to documentation, validation, audit, and periodic review. Governance effort in mature financial institutions is roughly equal to development effort.

### Three lines of defence

| Line | Role |
|------|------|
| **1st — Model Development Team** | Builds the model, documents assumptions, validates internally |
| **2nd — Independent Model Validation** | Separate team that challenges the model's assumptions, data, and performance claims |
| **3rd — Internal Audit** | Periodic review of whether governance processes are being followed |

### Model lifecycle

```
1. Development
      ↓
2. Independent Validation
      ↓
3. Approval Committee (sign-off)
      ↓
4. Deployment (often with exposure limits)
      ↓
5. Ongoing Monitoring (PSI, performance metrics)
      ↓
6. Periodic Review (annual or trigger-based)
```

### Types of model risk

| Risk type | Description | Example |
|-----------|-------------|---------|
| **Specification risk** | Wrong functional form or wrong features | Using linear model for non-linear relationship |
| **Data quality risk** | Errors, gaps, or bias in training data | Survivorship-biased training set |
| **Implementation risk** | Model is correct but the production code has bugs | Feature scaling applied in training but not in inference |
| **Drift risk** | Model degrades as the world changes | Credit model trained pre-COVID fails post-COVID |
| **Regulatory risk** | Model cannot be explained or audited | Black-box ensemble denied approval |

> **Key insight:** In finance, governance effort ≈ development effort. A brilliant model that cannot be documented, validated, and explained will not be deployed.

---

## 7. Live Case — The Profitable but Risky Model

This case integrates all concepts: economic value, stability, explainability, and governance.

### Context
A retail bank's data science team has built a new credit approval model.

**Current Model (Model A — Production):**
- AUC: 0.86
- Stable 5-year performance
- Fully documented and approved by regulator
- Annual profit: $120M
- Loss volatility: Low

**New Model (Model B — Proposed):**
- AUC: 0.90
- Backtested profit: $150M
- Uses alternative data + complex ensemble methods
- Limited data covering recession periods
- Harder to explain
- Validation team raises stability concerns

**During validation of Model B:**
- Performance drops 30% in simulated recession
- Slight increase in approvals to thin-file customers
- Higher capital usage under stress scenarios
- Explainability tools only partially available
- Regulator informal feedback: *"Needs stronger documentation"*

---

### The three stakeholder perspectives

**Group 1 — Growth-Focused Directors:**
- Focus on the +$30M revenue uplift
- Competitive pressure from fintechs using similar models
- Market share concerns if deployment is delayed

**Group 2 — Risk Committee:**
- Focus on the 30% performance drop in recession simulation
- Capital preservation if stress scenario materialises
- Higher capital buffer requirements may offset the profit gain

**Group 3 — Regulatory & Reputation Committee:**
- Compliance risk if alternative data raises fair lending questions
- Media / public scrutiny if thin-file approvals go wrong
- Regulatory friction if model cannot be fully documented

---

### The three deployment options

**Option 1 — Full Deployment (Aggressive):**
- Immediate $30M revenue uplift
- Accept higher volatility and potential stress losses
- Competitive positioning vs fintechs
- **Risk:** Capital stress during a downturn could exceed the accumulated profit

**Option 2 — Phased Deployment (Balanced — most realistic):**
- Run Model B as a challenger for 12 months alongside Model A
- Cap exposure to 30% of the portfolio
- Monitor stress sensitivity in live data
- Strengthen explainability documentation before full rollout
- Gate full deployment on meeting documentation and stability thresholds

**Option 3 — Reject Model B (Conservative):**
- Preserve stability and regulatory standing
- Avoid potential fair lending exposure
- Lose the $30M opportunity and cede ground to competitors

---

### Debrief — what metric should drive the decision?

| Metric | What it captures | Limitation |
|--------|-----------------|------------|
| AUC | Discrimination ability | Doesn't capture monetary asymmetry or stability |
| Backtested profit | Expected gain | Overfit to bull-market period; survivorship-biased history |
| Stress loss | Downside in adverse scenario | May be too conservative if recession probability is low |
| Capital volatility | Uncertainty around capital adequacy | Hard to quantify precisely |
| Regulatory exposure | Probability of adverse regulator action | Qualitative, difficult to price |

The correct answer depends on the bank's **risk appetite**, **capital position**, **regulatory environment**, and **strategic horizon**. This is why financial ML ≠ generic ML.

### The final twist

Six months after deployment under Option 2:
- Default rates are slightly higher than expected
- Media questions the fairness of alternative data in approvals
- Regulator requests a full audit of the alternative data features

**Model evaluation is continuous, not one-time.** A deployment decision is not the end of the governance process — it is the beginning of the monitoring phase.

---

## Summary

| Concept | Core lesson |
|---------|------------|
| Accuracy vs economic value | Optimise for expected profit, not correct predictions |
| Asymmetric costs | Set thresholds based on cost(FP) vs cost(FN), not 0.5 |
| Stability vs peak performance | Consistency over 3 years beats peak AUC in year 1 |
| Explainability | Regulatory requirement, not optional — especially in India (RBI, IRDAI) |
| Model governance | Models are regulated assets; governance effort ≈ development effort |
| Continuous evaluation | Model deployment opens a monitoring process, not closes one |

> **Finance rewards being right where money is at risk — not just being right on average.**
