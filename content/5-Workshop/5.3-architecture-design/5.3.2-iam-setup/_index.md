---
title: "IAM Initialization and Configuration"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---

# 5.3.2. IAM Roles Initialization and Configuration

To enable secure inter-service communication across the serverless architecture, dedicated **IAM Roles** are provisioned following the Principle of Least Privilege.

---

### 1. Provisioning IAM Role for AWS Glue ETL Job (`AWSGlueETLProcessorRole`)

- **Purpose**: Grants AWS Glue PySpark ETL Jobs permissions to read S3 Raw data, process clean datasets, write Parquet to S3 Curated, update Data Catalog, and trigger Glue Crawlers.
- **Console Steps**:
  1. Navigate to IAM Console -> Select **Roles** -> Click **Create role**.
  2. Trusted entity: Select **AWS service** -> Choose **Glue** -> Click **Next**.
  3. Attach Policy: Search and attach `AWSGlueServiceRole`.
  4. Create custom Inline Policy granting S3 Raw/Curated Read/Write and `glue:StartCrawler`.
  5. Role Name: `AWSGlueETLProcessorRole` -> Click **Create role**.

![(Figure 5.3.2.1) IAM Role Permissions Management Interface for AWS Glue ETL Job](/images/workshop/image9.png)

---

### 2. Provisioning IAM Role for AWS Lambda Collector (`LambdaCollectorExecutionRole`)

- **Purpose**: Grants `financial-data-collector` function permissions to write CloudWatch execution logs and write raw JSON payloads to S3 Raw Bucket.
- **Console Steps**:
  1. IAM Console -> Roles -> **Create role**.
  2. Trusted entity: **Lambda** (`lambda.amazonaws.com`).
  3. Attach `AWSLambdaBasicExecutionRole` and inline policy for `s3:PutObject` on `my-data-lake-raw-<ACCOUNT_ID>/*`.

*(Screenshot of IAM Role Creation for Lambda Collector: To be updated)*

---

### 3. Provisioning IAM Role for EventBridge Scheduler (`EventBridgeSchedulerRole`)

- **Purpose**: Allows EventBridge Scheduler to automatically invoke the Lambda Collector function (`lambda:InvokeFunction`) on a daily cron schedule.
- **Console Steps**:
  1. IAM Console -> Roles -> **Create role**.
  2. Trusted entity: **Custom Trust Policy** for `scheduler.amazonaws.com`.
  3. Grant `lambda:InvokeFunction` targeting the `financial-data-collector` ARN.

*(Screenshot of IAM Role Creation for EventBridge Scheduler: To be updated)*
