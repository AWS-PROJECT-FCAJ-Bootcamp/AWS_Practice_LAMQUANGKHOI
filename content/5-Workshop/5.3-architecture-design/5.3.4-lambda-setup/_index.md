---
title: "Lambda Initialization and Configuration"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.3.4. </b> "
---

# 5.3.4. AWS Lambda Functions Initialization and Configuration

The Ingestion & Notification layers of the **Financial Data Platform** rely on **AWS Lambda** to execute serverless Python tasks.

---

### 1. Provisioning AWS Lambda Collector Function (`financial-data-collector`)

- **Description**: Serverless Python function responsible for calling market APIs (`yfinance`), retrieving OHLCV price ticks & financial reports, formatting JSON payloads, and writing directly to S3 Raw Bucket.
- **Console Steps**:
  1. Navigate to AWS Lambda Console in `<REGION>` -> Click **Create function**.
  2. Choose **Author from scratch**:
     - Function name: `financial-data-collector`
     - Runtime: `Python 3.10` (or `Python 3.12`)
     - Architecture: `x86_64`
  3. Execution role: Select `LambdaCollectorExecutionRole`.
  4. Click **Create function**.
  5. Configure **Environment variables** under *Configuration* tab:
     - `RAW_DATA_BUCKET` = `my-data-lake-raw-<ACCOUNT_ID>`
     - `RAW_S3_PREFIX` = `ohlcv/ohlcv`
     - `DATA_PROVIDER` = `YFINANCE_API`

![(Figure 5.3.4.1) AWS Lambda Functions Management Console](/images/workshop/image6.png)

![(Figure 5.3.4.2) Create Function Page for financial-data-collector](/images/workshop/image7.png)

![(Figure 5.3.4.3) Source Code & Environment Variables Configuration Interface for Lambda Collector](/images/workshop/image8.png)

---

### 2. Provisioning AWS Lambda Email Function (`financial-data-email`)

- **Description**: Serverless function summarizing daily pipeline status, formatting HTML report templates, and triggering Amazon SES email dispatches.
- **Console Steps**:
  1. Lambda Console -> Click **Create function**.
  2. Function name: `financial-data-email`.
  3. Runtime: `Python 3.10`.
  4. Execution role: Select `LambdaEmailExecutionRole`.

*(Screenshot of Lambda Email Function Creation: To be updated)*

---

### 3. Configuring Lambda Layer (`yfinance`, `pandas` Dependencies)

- **Purpose**: Packages external Python dependencies (`yfinance`, `pandas`, `requests`) into reusable Lambda Layers.
- **Console Steps**: Lambda Console -> *Layers* -> Click *Create layer* -> Upload Python ZIP package.

*(Screenshot of Lambda Layer Configuration for yfinance: To be updated)*
