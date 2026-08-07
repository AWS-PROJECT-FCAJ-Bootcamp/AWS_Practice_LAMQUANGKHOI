---
title: "Resource Cleanup & Summary"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

# 5.6. RESOURCE CLEANUP & SUMMARY

To prevent incurring unnecessary AWS cloud service charges after completing the workshop lab series, our team provides step-by-step cleanup instructions below.

---

### AWS Resource Deletion Steps

1. **Delete AWS Amplify Application**:
   * Navigate to **AWS Amplify Console** ➔ select `vietnam-financial-dashboard` ➔ Click **Actions** ➔ **Delete app**.

2. **Delete Amazon API Gateway & AWS WAF**:
   * Open **Amazon API Gateway** ➔ select `vietnam-financial-api` ➔ Click **Delete**.
   * Open **AWS WAF** ➔ delete Web ACL associated with the API Gateway.

3. **Delete Amazon Cognito User Pool**:
   * Open **Amazon Cognito** ➔ select `vietnam-financial-user-pool` ➔ Delete App Client and click **Delete user pool**.

4. **Delete AWS Lambda Functions & Step Functions**:
   * Open **AWS Lambda Console** ➔ Delete functions: `lambda-ingestor-vnstock`, `lambda-backend-api`, `lambda-ses-alert`.
   * Open **AWS Step Functions** ➔ select data ingestion State Machine ➔ Click **Delete**.

5. **Delete AWS Glue Jobs, Crawlers & Data Catalog Databases**:
   * Open **AWS Glue Console** ➔ Delete Job `glue-job-pyspark-financial-etl`.
   * Delete Crawler `crawler-vietnam-financial-curated`.
   * Open **Data Catalog Databases** ➔ Delete Database `vietnam_financial_db`.

6. **Empty & Delete Amazon S3 Buckets**:
   * Open **Amazon S3 Console**.
   * Select `s3-vietnam-financial-raw-data-prod` ➔ Click **Empty** to purge all objects/versions ➔ Click **Delete**.
   * Select `s3-vietnam-financial-curated-data-prod` ➔ Click **Empty** ➔ Click **Delete**.

---

### 📝 Workshop Lab Summary
By completing this practical lab series, you have mastered building an end-to-end **Automated Vietnamese Securities Financial Data Ingestion and Analytics Platform on AWS Serverless**:
* Automated financial data crawling pipelines using Lambda, EventBridge, and Step Functions.
* Data cleaning and financial feature engineering ($CR$, $ROA$, $ROE$, $DAR$, $WCTA$, Altman Z-Score) using PySpark on AWS Glue.
* High-speed Serverless SQL querying over partitioned Parquet files with Amazon Athena.
* Secured REST APIs using Amazon Cognito, AWS WAF, and API Gateway.
* Visualizing metrics on an AWS Amplify Web Dashboard and triggering automated corporate distress email alerts with Amazon SES.