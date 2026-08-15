---
title: "Glue Initialization and Configuration"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---

# 5.3.3. AWS Glue ETL Job, Crawler & Athena Query Configuration

**AWS Glue** performs automated data cleaning, computes financial indicators (MA20, RSI14, return_pct), and maintains the AWS Glue Data Catalog for Amazon Athena querying.

---

### 1. Configuring AWS Glue PySpark ETL Job (`ohlcv-glue-processor`)

- **Description**: PySpark script running on Serverless Spark cluster (Glue 4.0, 2 Workers G.1X) to clean raw JSON payloads and output Snappy-compressed Parquet datasets.
- **Console Steps**:
  1. Open AWS Glue Console -> **ETL Jobs** -> Select **Script editor (Spark PySpark)** -> Click **Create**.
  2. Navigate to **Job details** tab:
     - Name: `ohlcv-glue-processor`
     - IAM Role: Select `AWSGlueETLProcessorRole`
     - Type: `Spark` | Glue Version: `Glue 4.0`
     - Worker type: `G.1X` | Number of workers: `2`
  3. Add static/dynamic job parameters under **Job parameters**:
     - `--RAW_DATA_BUCKET`: `my-data-lake-raw-<ACCOUNT_ID>`
     - `--RAW_KEY`: `ohlcv/ohlcv/year=YYYY/month=MM/day=DD/batch_sample.json`
     - `--CURATED_DATA_BUCKET`: `my-data-lake-curated-<ACCOUNT_ID>`
     - `--CURATED_S3_PREFIX`: `ohlcv/ohlcv`
     - `--GLUE_CRAWLER_NAME`: `ohlcv-crawler`
     - `--AWS_DEFAULT_REGION`: `<REGION>`
  4. Paste PySpark script `glue_ohlcv_processor.py` into Script Editor -> Click **Save** and **Run**.

![(Figure 5.3.3.1) AWS Glue Script Editor pasting PySpark script](/images/workshop/image10.png)

![(Figure 5.3.3.2) Job Details & Static/Dynamic Job Parameters List](/images/workshop/image11.png)

![(Figure 5.3.3.3) Execution Runs History displaying Succeeded status](/images/workshop/image12.png)

---

### 2. Configuring AWS Glue Crawler (`ohlcv-crawler`)

- **Description**: Crawler scanning S3 Curated Bucket to create/update table `ohlcv` inside database `financial_data_lake`.
- **Console Steps**:
  1. AWS Glue Console -> **Crawlers** -> Click **Create crawler**.
  2. Name: `ohlcv-crawler`.
  3. Add data store: Set S3 path `s3://my-data-lake-curated-<ACCOUNT_ID>/ohlcv/ohlcv/`.
  4. Choose IAM Role: Select `AWSGlueETLProcessorRole`.
  5. Target database: Select database `financial_data_lake` -> Click **Create crawler** and **Run crawler**.

![(Figure 5.3.3.4) S3 Data Store Configuration Page for Glue Crawler](/images/workshop/image13.png)

![(Figure 5.3.3.5) Schema Update Rules & Crawler Configuration](/images/workshop/image14.png)

---

### 3. Querying Financial Analytics via Amazon Athena Query Editor

- **Description**: Interactive SQL queries against Parquet table datasets inside Glue Data Catalog.
- **Console Steps**:
  1. Open **Amazon Athena Console** -> Query Editor.
  2. Select Database: `financial_data_lake`.
  3. Run SQL query to verify MA20 and RSI14 financial indicators:
     ```sql
     SELECT ticker, date, close_price, ma20, rsi_14, return_pct 
     FROM "financial_data_lake"."ohlcv" 
     ORDER BY date DESC LIMIT 100;
     ```

![(Figure 5.3.3.6) Amazon Athena Query Editor interface selecting financial_data_lake database](/images/workshop/image15.png)

![(Figure 5.3.3.7) SQL Query Results displaying ticker, close_price, ma20, rsi_14 columns](/images/workshop/image16.png)
