---
title: "Overview & System Architecture"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

# 5.1. OVERVIEW & SERVERLESS SYSTEM ARCHITECTURE

#### 🎯 Workshop Objectives
In this hands-on workshop, you will step-by-step build and deploy the **Automated Vietnamese Securities Financial Data Ingestion and Analytics Platform on AWS Serverless**. The system automates market data crawling across stock exchanges (**HOSE, HNX, UPCOM**), stores raw JSON datasets on S3 Data Lake, calculates key technical & financial indicators (MA20, RSI14, return_pct), exposes secure REST APIs, authenticates users with Cognito, and sends automated operational report emails via SES.

#### 🏗️ AWS Cloud Architecture (5 Core Layers)

The overall system architecture consists of 5 decoupled processing layers designed following AWS Well-Architected Framework principles:

![(Figure 5.1.1) 5-Layer AWS Serverless Architecture Diagram](/images/3layer_v1.0.drawio.png)

#### 🛠️ AWS Core Services & Resources Matrix:

| Architectural Layer | AWS Service | Role & Provisioned Resources in Lab |
| :--- | :--- | :--- |
| **1. Storage Layer** | `Amazon S3 (raw)` | Raw Bucket (`my-data-lake-raw-699061130094-dev`) storing raw JSON partitioned by `ohlcv/ohlcv/year=YYYY/month=MM/day=DD/` |
| | `Amazon S3 (curated)` | Curated Bucket (`my-data-lake-curated-699061130094-dev`) storing Snappy Parquet partitioned by ticker `ohlcv/ohlcv/ticker=XXX/` |
| **2. Ingestion Layer**| `AWS Lambda` | Function `financial-data-collector` (Python 3.10/3.12) crawling APIs and writing raw JSON to S3 Raw |
| | `Amazon EventBridge` | EventBridge Schedule (`daily-financial-pipeline-schedule`) executing cron daily at 16:00 VN (10:00 UTC) |
| **3. Process & Query**| `IAM Role` | Role `AWSGlueETLProcessorRole-dev` granting Glue ETL access to S3 Raw, Curated & Glue Crawlers |
| | `AWS Glue Job` | Glue PySpark Job (`ohlcv-glue-processor`) performing ETL, calculating MA20, RSI14, return_pct |
| | `AWS Glue Crawler` | Crawler (`ohlcv-crawler`) discovering 10-column schemas and syncing to Glue Data Catalog |
| | `Amazon Athena` | Serverless SQL Querying directly on database `financial_data_lake.ohlcv` |
| **4. API, Auth & Security**| `Amazon DynamoDB` | NoSQL Tables `Users` (PK: email) and `UserWatchlists` (PK: user_id, SK: symbol) |
| | `Amazon Cognito` | User Pool (`financial-data-user-pool-dev`) & App Client (`financial-data-web-client`) |
| | `Amazon API Gateway` | REST API (`financial-data-platform-api`) secured by Cognito Authorizer & AWS WAF |
| **5. Notification & UI**| `Amazon SES` | Verified Email Identity (`duyphong242004@gmail.com`) for automated report delivery |
| | `AWS Lambda (Email)` | Function `financial-data-email` generating HTML reports and dispatching via SES |
| | `AWS Amplify` | Hosting & CI/CD pipeline deployment for React Web Dashboard |