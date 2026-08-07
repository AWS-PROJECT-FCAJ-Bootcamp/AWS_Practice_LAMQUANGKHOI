---
title: "Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Vietnam Financial Distress Prediction System  
## AWS Cloud & Serverless Platform for Automated Financial Data Ingestion, Analysis, and Early Financial Distress Prediction  

### 1. Executive Summary  
The **Vietnam Financial Distress Prediction System** is designed as an end-to-end solution built on AWS Cloud to automate the ingestion, normalization, and analysis of financial statement data and market prices for non-financial companies listed on Vietnam's three main stock exchanges (HOSE, HNX, UPCOM). The system leverages a Serverless AWS Architecture combined with Machine Learning models (Logistic Regression, Random Forest, XGBoost, LightGBM, CatBoost) to calculate financial ratio matrices (Liquidity, Profitability, Leverage, Size & Growth), automate risk labeling (Rule-based & Altman Z-Score Emerging Market variant), and provide early warning alerts for financial distress/bankruptcy risk. The platform integrates a fullstack Web Dashboard (AWS Amplify, Next.js/React), secured by Amazon Cognito, governed by Amazon API Gateway, and features automated email notifications via Amazon SES for investors, financial analysts, and risk managers.

### 2. Problem Statement  
*Current Issues*  
Financial statement data in Vietnam is scattered across multiple heterogeneous sources (vnstock, TCBS, CafeF, Vietstock, VCI, MAS, KBS) with inconsistent data formats (Long-form vs. Wide-form) and fragmented indicator definitions. Manual data collection and financial analysis are highly time-consuming, error-prone, and unscalable for tracking over 1,600+ listed companies. Furthermore, current analytical tools lack automated regulatory labeling and specialized early-warning distress prediction models tailored to the Vietnamese stock market.  

*Solution*  
The platform utilizes **Amazon EventBridge** and **AWS Step Functions** to orchestrate automated ingestion pipelines via **AWS Lambda / ECS Tasks** that crawl/fetch data from financial APIs and store raw datasets in **Amazon S3 (raw)**. ETL pipelines driven by **AWS Glue Jobs** perform schema normalization, missing value handling, and Winsorization, storing cleaned datasets in Parquet format in **Amazon S3 (curated)**. **AWS Glue Crawlers** and **Glue Data Catalog** maintain updated metadata schemas for instant SQL querying with **Amazon Athena**. A specialized ratio and labeling engine calculates financial metrics, applying both Vietnamese regulatory rules (consecutive 2-year net loss, negative equity, EBIT < Interest Expense for 2 years, negative OCF for 3 years) and Altman Z-Score models. A fullstack web dashboard hosted on **AWS Amplify** with **Amazon Cognito** authentication provides interactive visual analytics, while **Amazon SES** automatically dispatches email alerts when companies enter the Distress Zone.  

*Benefits and Return on Investment (ROI)*  
- **100% Data Pipeline Automation**: Encapsulates the entire workflow from ingestion, cleansing, Data Lake storage, and feature engineering to ML prediction.  
- **Early & Accurate Alerts**: Empowers investors and financial institutions to detect bankruptcy risk 1–2 years in advance with high Recall rates.  
- **Cost-Optimized Serverless Architecture**: Operates on a pay-as-you-go AWS Serverless model, costing approximately $1.50 – $3.00 USD/month for production workloads.  
- **High Scalability**: Seamlessly scales to process historical and quarterly data for 1,600+ listed companies across HOSE, HNX, and UPCOM.  

### 3. Solution Architecture  
The platform follows a 5-tier AWS Serverless Architecture:  

![Vietnam Financial Distress System Architecture](/images/2-Proposal/architecture_overview.png)  

![Data Pipeline Architecture](/images/2-Proposal/pipeline_architecture.png)  

*AWS Services Used*  
- **Amazon EventBridge**: Triggers scheduled cron jobs for quarterly/annual financial data ingestion.  
- **AWS Step Functions**: Orchestrates multi-threaded ingestion workflows, retries, and checkpointing.  
- **AWS Lambda / ECS**: Executes crawler scripts/API calls to vnstock, TCBS, CafeF, and serves backend API requests.  
- **Amazon S3**: Data Lake storage containing 2 buckets (S3 raw for JSON/CSV and S3 curated for Parquet files).  
- **AWS Glue**: Runs Python/Spark ETL jobs for data transformation, Glue Crawlers for schema discovery, and Glue Data Catalog for metadata storage.  
- **Amazon Athena**: Provides serverless SQL queries directly on S3 curated Parquet data.  
- **AWS Amplify**: Hosts the web frontend dashboard (React/Next.js).  
- **Amazon Cognito**: Manages user authentication, authorization (Admin / Guest / Analyst), and JWT tokens.  
- **Amazon API Gateway**: Secures RESTful API endpoints between Frontend and Backend Lambda.  
- **AWS WAF**: Protects API Gateway and Amplify against web vulnerabilities (DDoS, SQL Injection).  
- **Amazon SES**: Sends automated email notifications when financial distress risks are flagged.  

*Component Design*  
- **Ingestion Layer**: EventBridge triggers Step Functions to call Lambda/ECS Tasks for pulling 3 main financial statements (Balance Sheet, Income Statement, Cash Flow) and stock prices, strictly filtering out financial sectors (Banks, Securities, Insurance, Investment Funds).  
- **Storage Layer**: S3 Raw stores unparsed JSON/CSV files; S3 Curated stores normalized, cleaned, and labeled Parquet data partitioned by year/quarter.  
- **Processing & ETL Layer**: AWS Glue Jobs normalize financial indicator names, filter companies with at least 5 years of continuous data, apply Winsorization (1%-99%), calculate financial metrics (CR, WCTA, ROA, ROE, EBIT_REV, DAR, STDR, LTDR, LogAsset, MC_Debt), and assign distress labels.  
- **Query & ML Layer**: Glue Crawlers update Data Catalog schemas; Athena serves ad-hoc SQL queries and backend API calls. Machine Learning models (Logistic Regression, XGBoost, Random Forest, CatBoost) are trained using time-series split strategies (2018-2022 train, 2023-2025 test).  
- **User Interface & Alerting**: Amplify hosts the interactive Web Dashboard; API Gateway + Lambda service manage application state; Cognito enforces access control; SES triggers real-time risk alerts.  

### 4. Technical Implementation  
*Implementation Phases*  
The project is executed across 4 main phases:  
1. **Research & Architecture Design (Month 1)**: Survey financial statement structures across VCI, MAS, KBS, and vnstock; define the 5-tier AWS Serverless architecture; establish Rule-based and Z-Score labeling criteria.  
2. **Data Pipeline & Storage Construction (Month 2)**: Provision S3 Raw/Curated buckets; develop Lambda Crawlers, Step Functions workflows, Glue ETL Jobs, and Glue Data Catalog + Athena query layer.  
3. **Engine Development & ML Pipeline (Month 3)**: Implement Ratio Engine, Distress Labeling Engine, and train/evaluate ML models (XGBoost, Random Forest) evaluated primarily on Recall & AUC-ROC metrics.  
4. **Web Dashboard, Auth & Alerting Deployment (Month 4)**: Build React/Next.js UI on Amplify; integrate Cognito Auth, API Gateway backend, Amazon SES notifications, and conduct End-to-End testing.  

*Technical Requirements*  
- **Data Engine**: Python 3.11+, `vnstock`, `pandas`, `pyarrow`, `numpy`, `scikit-learn`, `xgboost`, `lightgbm`, `catboost`.  
- **AWS Services & Infrastructure**: AWS CDK / SAM / Terraform for Infrastructure as Code (IaC).  
- **Backend & Web App**: FastAPI / Node.js for Lambda Service, React / Next.js for Frontend, Zustand for State Management, Tailwind CSS / Vanilla CSS for styling.  
- **Security & Standards**: SSL/TLS encryption, WAF, OAuth2 / JWT with Amazon Cognito, and Least Privilege IAM roles.  

### 5. Timeline & Milestones  
- **Milestone 1 (Month 1)**: Architecture proposal approved, BCTC schema normalized, and data lake design finalized.  
- **Milestone 2 (Month 2)**: Data Ingestion pipeline (Step Functions + Lambda + S3) and Glue ETL Jobs deployed.  
- **Milestone 3 (Month 3)**: Ratio Engine, Distress Labeling (Altman Z-Score), and ML training pipeline operational.  
- **Milestone 4 (Month 4)**: Web Dashboard (Amplify + Cognito), API Gateway backend, SES Alerts integrated, and E2E verified.  

### 6. Budget Estimation  
You can view the estimation on the [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=621f38b12a1ef026842ba2ddfe46ff936ed4ab01).  
Or download the [Budget Estimation File](../attachments/budget_estimation.pdf).  

*Infrastructure Costs*  
- Amazon S3 Standard (Raw & Curated 10 GB, 5,000 requests): $0.30 USD/month  
- AWS Lambda (Inbound Ingestion & Backend API 50,000 requests): $0.20 USD/month  
- AWS Step Functions & EventBridge: $0.10 USD/month  
- AWS Glue ETL Jobs & Crawlers (scheduled monthly/quarterly): $0.80 USD/month  
- Amazon Athena (Scanned < 10 GB Parquet/month): $0.05 USD/month  
- AWS Amplify & Amazon Cognito (Guest & User access): $0.35 USD/month  
- Amazon API Gateway & AWS WAF: $0.15 USD/month  
- Amazon SES (Email Alerts < 1,000 emails/month): $0.05 USD/month  

*Total*: ~$2.00 USD/month (~$24.00 USD/12 months)  
- **Data & Licensing Costs**: $0 USD (leveraging open-source `vnstock` and open libraries).  

### 7. Risk Assessment  
*Risk Matrix*  
- Breaking changes in source financial APIs/Websites (vnstock/CafeF/TCBS): High impact, medium probability.  
- Missing or inaccurate financial data from sources: Medium impact, high probability.  
- Label imbalance (Imbalanced Dataset between healthy and distressed firms): High impact, high probability.  
- Uncontrolled AWS costs due to unoptimized Athena queries/Glue runs: Medium impact, low probability.  

*Mitigation Strategies*  
- Data Sources: Build multi-source ingestion mechanisms (VCI, MAS, KBS) with automated fallback parsers.  
- Missing Data: Require a strict 5-year continuous dataset threshold, filter out financial sectors, and apply Winsorization.  
- Imbalanced Dataset: Utilize SMOTE / Class Weighting and prioritize Recall optimization for Class 1 (Distress).  
- AWS Costs: Set up AWS Budget Alerts and enforce Parquet partitioning to minimize Athena scanned data.  

*Contingency Plans*  
- Maintain Local/S3 Parquet backups for rapid recovery in case of ETL failure.  
- Use AWS CloudFormation / CDK scripts to redeploy the entire Serverless infrastructure on demand.  

### 8. Expected Outcomes  
*Technical Improvements*: Automated 100% of the Data Lake & ML Pipeline on AWS, eliminating manual BCTC analytical workflows. Early prediction of financial distress achieves Recall > 85% and AUC-ROC > 0.88 on test datasets.  
*Long-term Value*: Provides a standardized financial dataset for Vietnamese equities, scalable for future valuation, Credit Scoring, or Quantitative Trading research.