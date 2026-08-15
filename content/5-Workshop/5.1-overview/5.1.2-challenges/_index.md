---
title: "Challenges and Difficulties"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.1.2. </b> "
---

# 5.1.2. Challenges and Difficulties

### 1. Operational & Technical Challenges
Building an automated financial data ingestion and analytics pipeline on AWS presents several key technical hurdles:

1. **Data Source Heterogeneity & Access Constraints**:
   * Financial data providers (VNStock, CafeF, SSI iBoard, Fireant, HOSE, HNX) expose drastically different data formats (JSON payloads, HTML tables, raw Excel/PDF files).
   * Strict rate limiting, access token expirations, and unexpected API structural changes risk breaking ingestion scripts.
   * Legal terms of service (ToS) and copyright limits dictate how data can be stored and repurposed.
2. **Data Ingestion Library Incompatibility on AWS & Migration to `yfinance`**:
   * **Unsupported `VNStock` on AWS Lambda**: As originally planned, the team intended to use the `VNStock` Python library. However, during actual deployment on AWS Serverless runtime, `VNStock` proved incompatible (native binary dependency issues, Cloudflare/datacenter IP blocking when called from AWS Lambda subnets).
   * **Migration to `yfinance` Library**: The team proactively adjusted the architecture and migrated to the `yfinance` library (Yahoo Finance API) to ingest Vietnamese market OHLCV prices (tickers formatted as `VIC.VN`, `VNM.VN`, `FPT.VN`, etc.).
   * **Historical Data Depth Limitations**: Due to switching to `yfinance`, historical market data could not be retrieved as far back as originally anticipated, requiring the system to adjust historical data ingestion windows to fit the new API constraints.
3. **Multi-Tenant Team Workspace & Governance**:
   * Establishing a shared AWS workspace for team members without violating security best practices.
   * Enforcing IAM Least-Privilege access, separating Dev and Prod environments, and avoiding hardcoded credentials.
4. **Cost Optimization & Budget Controls**:
   * Running daily ETL jobs, interactive Athena queries, and continuous hosting services without exceeding strict intern/lab budget constraints.
   * Requiring proactive AWS Budgets, CloudWatch Alarms, and Athena query scan limits.

---

### 2. Comprehensive Data Provider Analysis Matrix

| Data Provider | Data Coverage & Focus | Ingestion Method & Format | Key Advantages | Technical Challenges & Risks | Recommendation |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **yfinance** *(New Primary)* | Historical OHLCV market price (format `XXX.VN`) | Python package calling Yahoo Finance API | Runs reliably on AWS Lambda, immune to datacenter IP blocking | Limited historical depth for older timeframes compared to original plan | **Primary Ingestion Source** replacing VNStock on AWS |
| **VNStock** *(Legacy Planned)* | Historical OHLCV, financial statements | Python package (Pandas DataFrame / JSON) | Free, rich Vietnamese financial attributes | Incompatible with AWS Lambda runtime; IP block risks | **Restricted** (legacy option for local runs only) |
| **SSI iBoard** | Real-time / near real-time price ticks, order book depth | Web/App WebSocket, unofficial internal endpoints | High quality, official securities firm reliability | No public API; high risk of IP block; fragile against UI updates | Avoid scraping in MVP; consider only if official API is available |
| **Fireant** | Financial news, technical indicators, social sentiment | Web API (restricted/paid tier) | Includes pre-calculated technical indicators & news feeds | Rate-limited on free tier; news text requires NLP filtering | Secondary source for financial news & sentiment monitoring |
| **CafeF** | Financial news, corporate filings, stock price history | Web scraping (HTML parsing) | Popular financial news, rapid news updates | Unofficial; HTML structure changes break scrapers; legal ToS risks | Use with retry & rate-limiting for auxiliary news ingestion |
| **HOSE / HNX** | Official disclosure files, corporate announcements | PDF / Excel downloads on official exchange portals | 100% official authority, highest data reliability | No automated API; manual file structure; non-realtime | Use as **Ground-Truth validation** source for audit checks |

> [!IMPORTANT]
> **Production Data Source Strategy on AWS**:
> * **Primary Ingestion Engine**: Switched to `yfinance` library packaged inside AWS Lambda Collector for daily automated OHLCV market data ingestion, overcoming AWS Serverless compatibility issues.
> * **Data Range Window**: Adjusted historical timeframe parameters to optimize for `yfinance` API retrieval depth.
> * **Audit Verification**: Periodic manual sampling against `HOSE/HNX` official announcements.
