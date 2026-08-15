---
title: "Objectives"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.1.3. </b> "
---

# 5.1.3. Objectives & System Metrics

### 1. Key Performance Metrics & Benchmarks
The platform measures operational success across three core quantitative dimensions:

1. **Data Accuracy & Integrity Metric**:
   * Target: **Failed Ticker Rate < 1%** per daily ingestion batch.
   * Mandatory zero-missing-field constraint on core attributes (`ticker`, `date`, `open`, `high`, `low`, `close`, `volume`).
2. **Dashboard Performance & Latency Metric**:
   * Target: **Dashboard Page Load < 2 seconds** for standard user queries.
   * Target: **Amazon Athena Ad-hoc Query Latency < 10 seconds** for datasets under 100GB scan limit.
3. **AI/ML Feature Readiness Metric**:
   * Target: **100% Parquet Feature Schema Compliance** for downstream Machine Learning training pipelines (e.g., Financial Distress prediction).

---

### 2. Functional Requirements (FR) Matrix

| Requirement ID | Module Name | Functional Description | Priority |
| :--- | :--- | :--- | :--- |
| **FR01** | User Authentication | User registration, login, token refresh, and password reset via AWS Cognito User Pool. | High |
| **FR02** | Ticker Search | Real-time ticker search and lookup across Vietnamese stock exchanges (HOSE, HNX, UPCoM). | High |
| **FR03** | Financial Data View | Interactive display of historical OHLCV prices, volume, and balance sheet / income statements. | High |
| **FR04** | Technical Indicators | Display pre-calculated technical indicators (MA20, RSI14, daily return percentage). | High |
| **FR05** | Portfolio Management | Create, update, and monitor custom investment portfolios and profit/loss metrics. | Medium |
| **FR06** | Watchlist Tracking | Add/remove stock tickers to personal watchlists with custom tags. | Medium |
| **FR07** | Automated Notifications | Trigger automated email reports and price alert notifications via Amazon SES & Lambda. | High |
| **FR08** | User Administration | Admin capability to view registered users, modify permissions, and inspect activity logs. | Medium |
| **FR09** | Data Synchronization | Automated scheduled daily data ingestion from external data providers into S3 Raw Bucket. | High |
| **FR10** | System Monitoring | Real-time monitoring of Lambda executions, Glue ETL status, S3 counts, and cloud budget alerts. | High |

---

### 3. Non-Functional Requirements (NFR) Matrix

| NFR Category | Specific System Standard & Benchmark | Implementation Mechanism |
| :--- | :--- | :--- |
| **Performance** | Dashboard queries render in `< 2s`; Athena queries run in `< 10s`. | Pre-partitioned S3 Parquet format, DynamoDB indexing, static Amplify CDN assets. |
| **Scalability** | Serverless auto-scaling supporting horizontal ingestion of 500+ stock tickers. | AWS Lambda concurrency, AWS Glue Spark DPU auto-scaling. |
| **Availability** | Daily ingestion pipeline success rate `≥ 95%`. | EventBridge Scheduler, automated retry logic, CloudWatch Alarms. |
| **Security** | Strict IAM Least-Privilege, data encryption at rest (SSE-S3), Secrets Manager. | AWS IAM Policies, KMS/SSE-S3 encryption, AWS WAF rate-limiting. |
| **Data Quality** | Zero corruption; automated verification script post-ETL run. | Glue PySpark schema validation, automated object count verification. |
| **Cost Efficiency** | Operational cost strictly controlled within lab budget limit (`< $25/month`). | AWS Budgets set at 50%/80%/100%, Athena Workgroup scan caps. |
| **Maintainability** | 100% Infrastructure as Code (IaC) deployment. | Terraform modular repository structure (`modules/s3`, `modules/glue`, etc.). |
| **Observability** | Centralized logging with explicit retention periods. | CloudWatch Log Groups (14-day retention for Dev, 30-day for Demo). |
