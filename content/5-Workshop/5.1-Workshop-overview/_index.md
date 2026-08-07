---
title: "Overview & System Architecture"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

# 5.1. OVERVIEW & SYSTEM ARCHITECTURE

#### 🎯 Workshop Objective
In this project, our team designed and built an **Automated Vietnamese Securities Financial Data Ingestion and Analytics Platform on AWS Serverless**. The platform automatically ingests financial statement data for listed equities across 3 Vietnamese stock exchanges (**HOSE, HNX, UPCOM**), executes scalable Glue ETL normalization pipelines, computes key financial indicators (Liquidity, Profitability, Leverage, Size), and predicts **Financial Distress** / Bankruptcy risks using Vietnamese accounting standards combined with the **Altman Z-Score** model.

#### 🏗️ AWS Cloud Serverless Architecture (5 Core Layers)

The system architecture consists of 5 decoupled processing layers:

```mermaid
flowchart TB
    subgraph INGESTION["1. Ingestion Layer"]
        EB["Amazon EventBridge (Cron Scheduler)"] --> SF["AWS Step Functions (Orchestrator)"]
        SF --> LAM_INGEST["AWS Lambda / ECS (vnstock Ingestor)"]
        LAM_INGEST <-->|Crawl API| EXT_DATA["Data Sources (HOSE, HNX, UPCOM)"]
        LAM_INGEST -->|Write Raw Data| S3_RAW["Amazon S3 (raw bucket)"]
    end

    subgraph STORAGE["2. Storage Layer (Data Lake)"]
        S3_RAW
        S3_CURATED["Amazon S3 (curated bucket)"]
    end

    subgraph PROCESS["3. Process & Query Layer (ETL & Query)"]
        S3_RAW -->|Read Raw| GLUE_JOB["AWS Glue Job (PySpark ETL)"]
        GLUE_JOB -->|Write Parquet Cleaned| S3_CURATED
        S3_CURATED -->|Scan Schema| CRAWLER["AWS Glue Crawler"]
        CRAWLER -->|Metadata| CATALOG["AWS Glue Data Catalog"]
        CATALOG <---> ATHENA["Amazon Athena (SQL Query)"]
        S3_CURATED <---> ATHENA
    end

    subgraph API_AUTH["4. API & Auth Layer"]
        COG["Amazon Cognito"] <---> APIGW["Amazon API Gateway + WAF"]
        APIGW --> LAM_API["AWS Lambda (Backend API)"]
        LAM_API --> ATHENA
    end

    subgraph UI_ALERT["5. UI & Alert Layer"]
        AMP["AWS Amplify (React Dashboard)"] <--> APIGW
        LAM_API --> LAM_SES["AWS Lambda (Email Service)"]
        LAM_SES --> SES["Amazon SES (Email Alerts)"]
    end
```

#### 🛠️ Core AWS Services Matrix:

| Processing Layer | AWS Service | Role & Responsibility in Lab |
| :--- | :--- | :--- |
| **1. Data Ingestion** | `Amazon EventBridge` | Triggers scheduled Cron workflows for quarterly/yearly financial statement ingestion |
| | `AWS Step Functions` | Orchestrates multi-threaded data ingestion workflows with retry logic and checkpointing |
| | `AWS Lambda` | Executes `vnstock` crawlers to ingest raw JSON/CSV financial reports into S3 |
| **2. Storage** | `Amazon S3 (raw)` | Stores raw Balance Sheet, Income Statement, Cash Flow, and Stock Price data |
| | `Amazon S3 (curated)` | Stores cleaned data, financial ratios, and labels in partitioned **Parquet** format |
| **3. Process & Query**| `AWS Glue Job` | Handles PySpark ETL: Indicator mapping, Winsorization, Z-Score calculations |
| | `AWS Glue Data Catalog`| Manages centralized Metadata Schemas for financial datasets |
| | `Amazon Athena` | Executes ad-hoc Serverless SQL queries directly on S3 curated Parquet files |
| **4. API & Auth** | `Amazon Cognito` | Handles user authentication, RBAC, and JWT tokens for Web UI |
| | `Amazon API Gateway` | RESTful API endpoint secured by `AWS WAF` |
| | `AWS Lambda (Backend)`| Executes backend logic, sends Athena SQL queries, and returns JSON payloads |
| **5. Dashboard & Alert**| `AWS Amplify` | Hosts and deploys the React/Next.js Web Dashboard |
| | `Amazon SES` | Sends automated Email alerts when financial distress risks are detected |