---
title: "AWS Pricing Calculator"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.5.1. </b> "
---

# 5.5.1. Cost Estimation via AWS Pricing Calculator

The platform follows a **Serverless & Pay-as-you-go** model, keeping operational cloud costs under **$15 USD/month** for lab/PoC scale workloads.

---

### Itemized Monthly Cost Breakdown Table

| AWS Service | Monthly Usage Scale | Estimated Cost (USD/Month) |
| :--- | :--- | :--- |
| **Amazon S3** | 10 GB Raw JSON + 20 GB Curated Parquet | ~$0.75 |
| **AWS Lambda** | 300 requests/month, 512MB RAM, 10s runtime | ~$0.00 (AWS Free Tier) |
| **AWS Glue ETL** | 1 daily run, 2 Workers G.1X, 5 min runtime | ~$7.50 |
| **Amazon Athena** | 50 GB data scanned/month | ~$0.25 |
| **Amazon DynamoDB** | 2 tables (On-demand mode), 1,000 read/write | ~$0.25 |
| **Amazon Cognito** | < 1,000 Monthly Active Users (MAU) | ~$0.00 (Free Tier 50k MAU) |
| **Amazon SES** | < 1,000 report emails/month | ~$0.10 |
| **AWS Amplify** | Hosting React App, < 5 GB bandwidth | ~$1.50 |
| **TOTAL ESTIMATED COST** | **Full daily automated data pipeline** | **~$10.35 USD / Month** |
