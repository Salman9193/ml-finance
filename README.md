# Machine Learning in Finance

This repository provides a foundational overview of the financial ecosystem and how Machine Learning (ML) serves as a decision-support engine within it.

## 1. Philosophy & Goals

* **Finance as a Decision System:** Finance is fundamentally a system of decisions, and ML is positioned as a powerful decision-support engine. 
* **Focus on Explainability:** A strong emphasis is placed on robustness, interpretability, and governance. Every concept must answer: *"How does this improve a real financial decision?"*.

* The ultimate learning outcome is model-driven financial decision-making with explainability. 

## 2. The Financial System & Flow of Funds
- The financial system exists because savers and investors are different economic agents. 

- It mobilizes idle savings into productive investments, transfers and diversifies risk, and reduces information gaps. 
- Without it, savings remain idle, high-risk projects go unfunded, individuals bear excessive risk, and economic growth slows down.

### Lender-Savers vs. Borrower-Spenders
* **Lender-Savers:** Entities with excess funds (Households, Business firms, Government, Foreigners).

* **Borrower-Spenders:** Entities needing to raise funds for productive projects (Business firms, Government, Households, Foreigners).

### Mechanisms of Flow
Capital moves between these groups via two primary channels:
1. **Direct Finance:** Funds are raised directly from investors via equity and bond markets using market-based pricing.
2. **Indirect Finance:** Financial intermediaries (like banks and NBFCs) intermediate funds, largely through relationship-based lending.

## 3. Core Pillars of the Financial System
The financial system operates on four main pillars [5, 6]:
1. **Corporate Finance:** Focuses on firm-level investment and funding decisions (e.g., project selection, financing, liquidity management, shareholder returns, and M&A).
2. **Banking & Credit:** Accepts deposits, creates liquidity by transforming short-term deposits into long-term loans, monitors borrowers, manages default risk, and maintains financial stability.
3. **Insurance & Pensions:** Pools risks across individuals/firms, provides protection against low-probability events, and collects long-term savings for heavy market investments.
4. **Financial Markets:** Platforms that enable price discovery, provide investor liquidity, and allocate capital across sectors (includes equity, bond, FX, and derivatives markets).

## 4. Banking and Financial Institutions
Financial institutions act as intermediaries that reduce monitoring costs, increase liquidity, and lower price risk.

* **Depository Institutions (Commercial Banks):** Operate on a for-profit basis, accepting deposits and lending to the public/businesses.
* **Investment Banks:** Help firms raise capital, trade securities, and provide M&A consultancy. 
* **Universal Banks:** Combine commercial and investment banking.
* **Niche Banks (India Context):**
  * *Small Finance Banks:* Target financial inclusion for unorganized sectors.
  * *Payments Banks:* Cannot lend; can only accept deposits and offer payment services (investing deposits in safe government securities).
  * *Regional Rural Banks (RRBs):* Serve rural and agricultural sectors.
* **NBFCs:** Regulated entities that, unlike banks, cannot accept demand deposits and are not part of the payment settlement system.

## 5. Financial Instruments & Asset Classes
Financial instruments are tools to trade capital, divided by maturity into **Money Markets** (< 1 Year) and **Capital Markets** (> 1 Year).

### Major Asset Classes
* **Equity Markets:** Represent ownership claims in firms. They carry higher risk but higher expected returns, have no guaranteed returns, and generate rich price and volume data.
* **Debt Markets:** Fixed repayment obligations (lower risk than equity) whose prices reflect interest rates and credit risk. Central to banking and credit systems.
* **FX & Commodity Markets:** FX enables international trade and reflects macroeconomic conditions. Commodities enable price discovery for raw materials and are used to hedge inflation/supply risks.
* **Derivatives Markets:** Contracts derived from underlying assets (futures, options, and swaps) widely used by firms and institutions as risk transfer contracts to hedge price, rate, and credit risks.

## 6. Machine Learning in Finance
Financial markets generate massive amounts of data, which enhances decision-making across the sector.

### Data Sources
Financial ML models leverage vast amounts of unstructured and structured data, including:
* Transactions and payments
* Market prices and volumes
* Balance sheets and cash flows
* Credit histories
* News, filings, and other text data

### Key ML Applications
ML serves as a data-driven engine in several crucial areas :
* Credit scoring and default prediction
* Fraud detection
* Algorithmic trading
* Portfolio optimization
* Stress testing and risk forecasting

### Finance Thinking vs. ML Thinking
* **Traditional Finance:** Model-based, theory-driven, and relies heavily on strong assumptions.
* **Machine Learning:** Purely data-driven, focusing heavily on pattern recognition and prediction .