---
title: "Resource Cleanup & Summary"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

# 5.6. RESOURCE CLEANUP & SUMMARY

To prevent incurring unexpected AWS cloud charges after building and testing the workshop application, follow the step-by-step cleanup procedure below.

---

### AWS Resource Teardown Procedure

1. **Delete AWS Amplify Application**:
   * Open **AWS Amplify Console** ➔ Select the Web Dashboard application ➔ Click **Actions** ➔ Select **Delete app**.

2. **Delete Amazon API Gateway & AWS WAF**:
   * Open **Amazon API Gateway Console** ➔ Select REST API `financial-data-platform-api` ➔ Click **Delete**.
   * Open **AWS WAF** ➔ Delete Web ACL rules attached to the API Gateway.

3. **Delete Amazon Cognito User Pool**:
   * Open **Amazon Cognito Console** ➔ Select User Pool `financial-data-user-pool-dev` ➔ Delete App Client `financial-data-web-client` ➔ Click **Delete user pool**.

4. **Delete Amazon DynamoDB Tables**:
   * Open **Amazon DynamoDB Console** ➔ **Tables** ➔ Select tables `Users` and `UserWatchlists` ➔ Click **Delete table**.

5. **Delete AWS Lambda Functions & EventBridge Schedule**:
   * Open **AWS Lambda Console** ➔ Delete functions: `financial-data-collector` and `financial-data-email`.
   * Open **Amazon EventBridge Console** ➔ **Schedules** ➔ Select schedule `daily-financial-pipeline-schedule` ➔ Click **Delete**.

6. **Delete AWS Glue Jobs, Crawlers & Data Catalog**:
   * Open **AWS Glue Console** ➔ Delete Job `ohlcv-glue-processor`.
   * Delete Crawler `ohlcv-crawler`.
   * Open **Data Catalog Databases** ➔ Delete Database `financial_data_lake`.
   * Open **IAM Console** ➔ **Roles** ➔ Delete IAM Role `AWSGlueETLProcessorRole-dev`.

7. **Delete Objects & Amazon S3 Buckets**:
   * Access **Amazon S3 Console**.
   * Select Raw Bucket `my-data-lake-raw-699061130094-dev` ➔ Click **Empty** to purge all objects/versions ➔ Click **Delete**.
   * Select Curated Bucket `my-data-lake-curated-699061130094-dev` ➔ Click **Empty** ➔ Click **Delete**.

---

### 📝 Workshop Summary
By completing this hands-on workshop, you have mastered building an end-to-end **Automated Vietnamese Securities Financial Data Ingestion and Analytics Platform on AWS Serverless**:
* **Storage Layer**: Created S3 Raw (`my-data-lake-raw-699061130094-dev`) for raw JSON Landing Zone and S3 Curated (`my-data-lake-curated-699061130094-dev`) for Snappy Parquet partitioned by ticker.
* **Ingestion Layer**: Developed Lambda Collector (`financial-data-collector`) for market API crawling and scheduled daily 16:00 VN executions on EventBridge Scheduler (`daily-financial-pipeline-schedule`).
* **Process & Query Layer**: Configured IAM Role (`AWSGlueETLProcessorRole-dev`), authored AWS Glue PySpark Job (`ohlcv-glue-processor`) for indicator computation (MA20, RSI14), ran Glue Crawler (`ohlcv-crawler`), and queried datasets via Amazon Athena (`financial_data_lake.ohlcv`).
* **API, Auth & Security Layer**: Provisioned DynamoDB NoSQL tables (`Users`, `UserWatchlists`), authenticated users with Cognito User Pool (`financial-data-user-pool-dev`), and secured REST API Gateway (`financial-data-platform-api`) with AWS WAF.
* **Notification Layer & UI**: Verified email identities on Amazon SES (`duyphong242004@gmail.com`), dispatched HTML operational report emails via Lambda (`financial-data-email`), and hosted React Web Dashboards on AWS Amplify.