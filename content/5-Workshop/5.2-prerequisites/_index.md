---
title: "Pre-project Preparation Requirements"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
aliases:
  - /5-workshop/5.2-pipeline/
---

# 5.2. AWS Services & IAM Preparation Requirements

To deploy the **Automated Vietnamese Securities Financial Data Ingestion and Analytics Platform on AWS Serverless**, system preparation focuses strictly on the core **AWS Services** and **IAM Roles & Security Permissions**.

Following the presentation rule: **Each required AWS service section is accompanied by exactly 01 relevant screenshot**.

---

### 1. Storage Infrastructure (Amazon S3)

- **Service Overview & Purpose**: Multi-tiered Data Lake storage hosted in AWS Region `ap-southeast-1` (Singapore).
  - **S3 Raw Bucket**: `my-data-lake-raw-699061130094-dev` (Stores raw JSON payloads partitioned as `ohlcv/ohlcv/year=YYYY/month=MM/day=DD/`).
  - **S3 Curated Bucket**: `my-data-lake-curated-699061130094-dev` (Stores Snappy-compressed Parquet datasets Hive-partitioned as `ohlcv/ohlcv/ticker=XXX/`).
- **IAM Requirements**:
  - Read/Write permissions (`s3:GetObject`, `s3:PutObject`, `s3:ListBucket`) granted to AWS Lambda Collector and AWS Glue ETL Job.

![(Figure 5.2.1) Raw JSON Partitioned Folder Structure on Amazon S3 Raw Bucket](/images/workshop/image3.png)

---

### 2. Identity & Access Management (AWS IAM)

- **Service Overview & Purpose**: Centralized security and access control governed by the Principle of Least-Privilege.
- **IAM Requirements**:
  - **Primary IAM Role**: `AWSGlueETLProcessorRole-dev`
    - Trust Relationship: `glue.amazonaws.com`
    - AWS Managed Policy: `AWSGlueServiceRole`
    - Custom Inline Policy: Read/Write access on both S3 Buckets (`my-data-lake-raw-699061130094-dev`, `my-data-lake-curated-699061130094-dev`) and permission to invoke `glue:StartCrawler`.

![(Figure 5.2.2) IAM Role AWSGlueETLProcessorRole-dev Configuration & Attached Policies on AWS IAM Console](/images/workshop/image9.png)

---

### 3. Serverless Compute (AWS Lambda)

- **Service Overview & Purpose**: Executes Python serverless functions for daily market data ingestion (`financial-data-collector`) and email alert distribution (`financial-data-email`).
- **IAM Requirements**:
  - **Lambda Execution Role**: CloudWatch logging permissions (`logs:CreateLogStream`, `logs:PutLogEvents`), S3 Raw Bucket write permissions, and Amazon SES email dispatch permissions (`ses:SendEmail`).
  - **Environment Variables**: `RAW_DATA_BUCKET`, `RAW_S3_PREFIX`, `DATA_PROVIDER`.

![(Figure 5.2.3) Source Code and Environment Variables Configuration Interface of AWS Lambda Collector](/images/workshop/image8.png)

---

### 4. Data Processing & Data Catalog (AWS Glue)

- **Service Overview & Purpose**: PySpark ETL Job (`ohlcv-glue-processor`) for data normalization & technical indicator calculation (MA20/RSI14) and Glue Crawler (`ohlcv-crawler`) for automatic Data Catalog updates.
- **IAM Requirements**:
  - Assigned IAM Role `AWSGlueETLProcessorRole-dev` to allow reading from S3 Raw, writing Parquet to S3 Curated, and updating table schemas in the Glue Data Catalog.

![(Figure 5.2.4) AWS Glue Job Details and Static/Dynamic Job Parameters Configuration Screen](/images/workshop/image11.png)

---

### 5. Serverless SQL Query Engine (Amazon Athena)

- **Service Overview & Purpose**: High-performance ad-hoc SQL queries on normalized S3 Parquet datasets without underlying database server management.
- **IAM Requirements**:
  - Athena Workgroup permissions (`athena:StartQueryExecution`, `athena:GetQueryResults`) and S3 query output location write permissions (`s3:GetBucketLocation`, `s3:PutObject`).

![(Figure 5.2.5) Amazon Athena Query Editor executing SQL queries on Parquet datasets](/images/workshop/image16.png)

---

### 6. NoSQL Database & User Authentication (DynamoDB & AWS Cognito)

- **Service Overview & Purpose**: DynamoDB tables (`Users`, `UserWatchlists`) for state storage and AWS Cognito User Pool (`financial-data-user-pool-dev`) for user authentication.
- **IAM Requirements**:
  - Cognito Identity Pool Roles (Authenticated/Unauthenticated Roles) and DynamoDB access permissions for API Gateway/Lambda endpoints (`dynamodb:GetItem`, `dynamodb:PutItem`, `dynamodb:Query`).

![(Figure 5.2.6) Amazon Cognito User Pool Console displaying App Client ID and User List](/images/workshop/image25.png)

---

### 7. Email Alerts & Automation Scheduler (Amazon SES & EventBridge)

- **Service Overview & Purpose**: Amazon SES (`duyphong242004@gmail.com`) for HTML report delivery and EventBridge Scheduler (`daily-financial-pipeline-schedule`) for daily cron triggers.
- **IAM Requirements**:
  - EventBridge Execution Role granting permission to invoke the Lambda Collector function (`lambda:InvokeFunction`) on a daily cron schedule.

![(Figure 5.2.7) Verified Email Identities List on Amazon SES Console](/images/workshop/image18.png)
