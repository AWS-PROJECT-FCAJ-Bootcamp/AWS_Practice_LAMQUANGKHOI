---
title: "Financial Data Stream Demo Deployment on AWS"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

# 5.4. FINANCIAL DATA STREAM DEMO DEPLOYMENT ON AWS

### End-to-End Deployment Overview
This section provides a step-by-step walk-through of the end-to-end execution and deployment of the **Automated Vietnamese Securities Financial Data Ingestion and Analytics Platform on AWS Serverless**.

It covers the complete data pipeline lifecycle from raw ingestion, PySpark ETL transformation, Data Catalog updates, Athena SQL queries, automated email notifications via SES, to the live web dashboard hosted on AWS Amplify.

---

### Section Navigation (5.4.1 - 5.4.12)

* **[5.4.1. Source Code & Configuration Keys]({{< ref "5.4.1-source-code-keys" >}})**: Complete source code for Lambda Collector, PySpark Glue Job, and Lambda Email.
* **[5.4.2. Project Sample Datasets]({{< ref "5.4.2-sample-data" >}})**: Sample JSON payload structure and schema contract for OHLCV prices.
* **[5.4.3. Uploading Sample Data to S3 Storage]({{< ref "5.4.3-s3-sample-data-upload" >}})**: Procedures for seeding initial raw datasets into S3 Raw Bucket.
* **[5.4.4. Daily Scheduling with Amazon EventBridge]({{< ref "5.4.4-eventbridge-scheduling" >}})**: Configuring daily cron schedules triggering ingestion at 16:00 VN time.
* **[5.4.5. PROCESS LAYER (AWS ETL GLUE JOB)]({{< ref "5.4.5-process-layer-glue-job" >}})**: Executing PySpark ETL transformation from raw JSON to Snappy Parquet.
* **[5.4.6. CATALOG & QUERY LAYER (AWS GLUE CRAWLER & ATHENA)]({{< ref "5.4.6-catalog-query-layer" >}})**: Automated schema discovery and SQL querying on Amazon Athena.
* **[5.4.7. End-to-End Data Stream Verification]({{< ref "5.4.7-stream-testing" >}})**: Testing pipeline stream from external API to S3 Curated partitions.
* **[5.4.8. Frontend Web Hosting via AWS Amplify]({{< ref "5.4.8-hosting-amplify" >}})**: Deploying the React Web Dashboard on AWS Amplify.
* **[5.4.9. User Registration & Auth with AWS Cognito]({{< ref "5.4.9-user-auth-cognito" >}})**: User Pool setup and user credentials management.
* **[5.4.10. Notification Layer (LAMBDA EMAIL & AMAZON SES)]({{< ref "5.4.10-notification-layer" >}})**: SES identity verification and automated HTML report generation.
* **[5.4.11. API, Auth & Security Layer (DYNAMODB, COGNITO, API GATEWAY, WAF)]({{< ref "5.4.11-api-auth-security-layer" >}})**: REST API Gateway integration secured by AWS WAF.
* **[5.4.12. Resource Clean Up Procedures]({{< ref "5.4.12-resource-cleanup" >}})**: Step-by-step teardown guide to prevent unnecessary cloud costs.
