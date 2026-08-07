---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# AUTOMATED VIETNAMESE SECURITIES FINANCIAL DATA INGESTION AND ANALYTICS PLATFORM ON AWS SERVERLESS

### Workshop Overview
In this hands-on lab series, our team presents a step-by-step implementation guide for building the **Automated Vietnamese Securities Financial Data Ingestion and Analytics Platform on AWS Serverless**. The system automates financial statement data crawling, normalizes raw datasets, calculates key financial ratios, predicts **Financial Distress** / Bankruptcy risks for companies listed across 3 Vietnamese stock exchanges (**HOSE, HNX, UPCOM**), and delivers REST APIs, an interactive Web Dashboard, and Email Alert notifications.

The entire architecture is engineered using 100% cloud-native AWS Serverless services, offering automatic scalability, high performance, and cost efficiency:
* **Data Ingestion Layer**: Amazon EventBridge, AWS Step Functions, AWS Lambda / ECS Ingestor (integrated with `vnstock`).
* **Data Lake Storage Layer**: Amazon S3 (`S3 Raw Bucket` & `S3 Curated Bucket`).
* **Processing & Query Layer (ETL & Query)**: AWS Glue Job (PySpark/Python), AWS Glue Crawler, AWS Glue Data Catalog, Amazon Athena.
* **API & Authentication Layer**: Amazon Cognito, AWS WAF, Amazon API Gateway, AWS Lambda Backend API.
* **Frontend & Alert Layer**: AWS Amplify (React/Next.js), AWS Lambda, Amazon SES.

---

### Table of Contents

1. **[5.1. Overview & System Architecture]({{< ref "5.1-workshop-overview" >}})**
   * Overview of the listed corporate financial distress prediction problem and the 5-layer AWS Serverless architecture.
2. **[5.2. Automated Data Ingestion Pipeline]({{< ref "5.2-pipeline" >}})**
   * Creating S3 Raw Buckets, filtering non-financial tickers, writing Lambda data ingestion scripts using `vnstock`, and orchestrating workflows with EventBridge & Step Functions.
3. **[5.3. Data Processing & Data Lake (AWS Glue ETL, Data Catalog & Athena)]({{< ref "5.3-glue-config" >}})**
   * Creating S3 Curated Buckets, writing AWS Glue PySpark jobs for data normalization, Winsorization, financial ratio & Altman Z-Score calculation, running Crawlers, and querying with Athena.
4. **[5.4. REST API & User Authentication (Cognito, API Gateway & Lambda API)]({{< ref "5.4-amplify" >}})**
   * Configuring Cognito User Pools, setting up REST API Gateways protected by AWS WAF, and building Lambda Backend services for Athena queries.
5. **[5.5. Web Dashboard & Email Alerts (Amplify, Lambda & SES)]({{< ref "5.5-policy" >}})**
   * Deploying React/Next.js Web Dashboards on AWS Amplify and setting up Lambda + Amazon SES for automated distress email alerts.
6. **[5.6. Resource Cleanup & Summary]({{< ref "5.6-cleanup" >}})**
   * Step-by-step guide to cleaning up provisioned AWS cloud resources to prevent unexpected billing.