---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# AUTOMATED VIETNAMESE SECURITIES FINANCIAL DATA INGESTION AND ANALYTICS PLATFORM ON AWS SERVERLESS

### Workshop Overview
In this hands-on workshop series, we present a step-by-step implementation guide for building the **Automated Vietnamese Securities Financial Data Ingestion and Analytics Platform on AWS Serverless**. The system automates raw financial statement and market data crawling, normalizes datasets, calculates key financial ratios (MA20, RSI14, return_pct), partitions Data Lake storage, and delivers REST APIs, Cognito authentication, an interactive Web Dashboard, and automated email alerts.

> [!IMPORTANT]
> 🌐 **LIVE DEMO WEB APPLICATION & PROOF OF DEPLOYMENT**
> * **Live Web Application URL:** [https://feature-dashboard.dgku51j8dnv70.amplifyapp.com/login](https://feature-dashboard.dgku51j8dnv70.amplifyapp.com/login)
> * **Test Account Email:** `thoa@gmail.com`
> * **Test Account Password:** `1111111`

---

### Table of Contents

1. **[5.1. Project Overview]({{< ref "5.1-overview" >}})**
   * Background, objectives, scope, challenges, actors, use cases, and execution plan.
2. **[5.2. Pre-project Preparation Requirements]({{< ref "5.2-prerequisites" >}})**
   * Prerequisites and environment preparation before starting the project.
3. **[5.3. Infrastructure Design & Architecture of AWS Data Pipeline]({{< ref "5.3-architecture-design" >}})**
   * S3, IAM, Glue, Lambda, and IAM policy initialization and configuration.
4. **[5.4. Financial Data Stream Demo Deployment on AWS]({{< ref "5.4-demo-deployment" >}})**
   * Implementation of source code, sample data, EventBridge, Glue ETL, Glue Crawler, Athena, stream testing, Amplify hosting, Cognito auth, SES email alerts, REST API Gateway, WAF, and resource cleanup.
5. **[5.5. Project Testing & Cost Estimation]({{< ref "5.5-testing-cost-estimation" >}})**
   * Pricing Calculator estimates, CloudWatch Dashboard, Log Insights, and End-to-End testing.
6. **[5.6. Project Summary & Future Roadmap]({{< ref "5.6-summary-future-work" >}})**
   * Summary of achievements and future development roadmap.