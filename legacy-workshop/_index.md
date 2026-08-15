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

The entire architecture is engineered using 100% cloud-native AWS Serverless services, offering automatic scalability, high performance, and cost efficiency:
* **Storage Layer (Amazon S3)**: S3 Raw Bucket (`my-data-lake-raw-699061130094-dev`) & S3 Curated Bucket (`my-data-lake-curated-699061130094-dev`).
* **Ingestion Layer (AWS Lambda Collector & EventBridge)**: Lambda Collector (`financial-data-collector`), EventBridge Scheduler (`daily-financial-pipeline-schedule`).
* **Process & Query Layer (AWS Glue ETL & Athena)**: IAM Role (`AWSGlueETLProcessorRole-dev`), Glue ETL Job PySpark (`ohlcv-glue-processor`), Glue Crawler (`ohlcv-crawler`), Amazon Athena (`financial_data_lake.ohlcv`).
* **API, Auth & Security Layer (DynamoDB, Cognito, API Gateway & WAF)**: DynamoDB (`Users`, `UserWatchlists`), Cognito User Pool (`financial-data-user-pool-dev`), REST API (`financial-data-platform-api`) & AWS WAF.
* **Notification Layer & Web Dashboard (SES, Lambda Email & Amplify)**: Amazon SES (`duyphong242004@gmail.com`), Lambda Email (`financial-data-email`), AWS Amplify Web Dashboard.

> [!IMPORTANT]
> 🌐 **LIVE DEMO WEB APPLICATION & PROOF OF DEPLOYMENT**
> * **Live Web Application URL:** [https://feature-dashboard.dgku51j8dnv70.amplifyapp.com/login](https://feature-dashboard.dgku51j8dnv70.amplifyapp.com/login)
> * **Test Account Email:** `thoa@gmail.com`
> * **Test Account Password:** `1111111`

---

### Table of Contents

1. **[5.1. Overview & System Architecture]({{< ref "5.1-workshop-overview" >}})**
   * Architectural overview of the 5-layer AWS Serverless design and lab objectives.
2. **[5.2. Raw Data Ingestion & Storage Layer]({{< ref "5.2-pipeline" >}})**
   * Creating S3 Raw & Curated Buckets, writing Lambda Collector scripts, and scheduling daily cron jobs via EventBridge Scheduler.
3. **[5.3. Data Processing & Data Lake (AWS Glue ETL, Data Catalog & Athena)]({{< ref "5.3-glue-config" >}})**
   * Configuring Glue IAM Roles, writing AWS Glue PySpark jobs for data normalization, running Crawlers, and querying with Athena.
4. **[5.4. REST API, Database & Authentication (DynamoDB, Cognito & API Gateway)]({{< ref "5.4-amplify" >}})**
   * Creating DynamoDB NoSQL tables, setting up Cognito User Pools, and securing REST API Gateways with AWS WAF & Cognito Authorizers.
5. **[5.5. Email Alerts & Web Dashboard Deployment (SES, Lambda & Amplify)]({{< ref "5.5-policy" >}})**
   * Verifying administrator emails on Amazon SES, setting up HTML report Lambda functions, and hosting React Web Dashboards on AWS Amplify.
6. **[5.6. Resource Cleanup & Summary]({{< ref "5.6-cleanup" >}})**
   * Step-by-step guide to cleaning up provisioned AWS cloud resources to prevent unexpected billing.