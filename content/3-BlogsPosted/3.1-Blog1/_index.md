---
title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# AUTOMATED VIETNAMESE FINANCIAL DATA INGESTION AND ANALYSIS PLATFORM ON AWS SERVERLESS

During the development of our financial data analytics project covering companies listed on Vietnam's stock exchanges (HOSE, HNX, UPCOM), our team realized that manually ingesting and normalizing financial statements from scattered providers represented the primary operational bottleneck. To solve this challenge, our team built an automated data ingestion, cleansing, and ratio calculation platform based on AWS Serverless Cloud Architecture. Here are the core key takeaways our team wants readers to remember about this platform:

* **100% Automated Multi-Source Ingestion Pipeline**: Our team utilized Amazon EventBridge for quarterly/annual cron scheduling, AWS Step Functions for parallel workflow orchestration with retry mechanisms, and AWS Lambda / ECS Tasks wrapping `vnstock` scripts to extract full Balance Sheets, Income Statements, Cash Flow Statements, and stock prices from VCI, MAS, and KBS into an Amazon S3 Raw Data Lake.
* **ETL Cleansing and Financial Data Normalization**: Our team implemented AWS Glue Jobs to filter out specialized financial sectors (Banks, Securities, Insurance), map hundreds of heterogeneous indicator names into a unified canonical schema, enforce a minimum continuous 5-year data threshold, and apply Winsorization (1%-99%) to eliminate statistical noise.
* **Optimized Data Lake Storage and High-Speed SQL Queries**: Cleaned data is written into Snappy-compressed Apache Parquet format partitioned by year and quarter within an S3 Curated bucket. Integrated with the AWS Glue Data Catalog, our system enables Amazon Athena to execute serverless SQL queries with exceptional speed and minimal cost.
* **Financial Ratio Matrix and Corporate Distress Labeling Engine**: Our team engineered automated calculation engines for liquidity, profitability, leverage, and growth ratio matrices. Concurrently, the platform applies dual distress labeling mechanisms combining Vietnamese regulatory rules (consecutive net losses, negative equity, insufficient EBIT) with Altman Z-Score models for emerging markets.
* **Web Dashboard UI and Automated Risk Notification System**: All backend services are exposed via Amazon API Gateway, authenticated through Amazon Cognito, visualized on an interactive Web Dashboard hosted on AWS Amplify, and configured with Amazon SES to dispatch automated email alerts whenever monitored equities enter the financial distress zone.

Through this article, our team hopes to provide a clear perspective on leveraging AWS Serverless architecture to solve large-scale financial data processing challenges in an automated, cost-effective, and highly reliable manner.

![AWS Serverless Architecture Diagram](/images/3layer_v1.0.drawio.png)

---

### Resources & Publication Links:
* **Official Post on AWS Study Group**:
![Official Post on AWS Study Group](/images/Post_Blog_1.png)
* **Live System Demo Link**: [Live System Link]()
* **Detailed Installation & Operational Guide**: [User & Deployment Guide Link]()