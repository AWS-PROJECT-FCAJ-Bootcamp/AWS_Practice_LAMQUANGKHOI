---
title: "Raw Data Ingestion & Storage Layer"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

# 5.2. RAW DATA INGESTION & STORAGE LAYER

In this section, we configure the **Storage Layer (Amazon S3)** and **Ingestion Layer (AWS Lambda Collector & EventBridge Scheduler)** to automate financial market data crawling and partitioned storage in the Data Lake.

---

### PART 1: STORAGE LAYER (AMAZON S3)

#### 1.1 Create S3 Raw Bucket

* **Description**: Raw JSON storage bucket for daily market datasets from VNStock/Yahoo APIs. Data is partitioned chronologically: `ohlcv/ohlcv/year=YYYY/month=MM/day=DD/batch_<timestamp>.json`.
* **Data Lake Architectural Principles**:
  * **Landing Zone**: S3 Raw acts as the raw, immutable landing zone for JSON payloads retrieved from source APIs.
  * **Immutability Principle**: Raw data on S3 Raw is never modified or deleted. This allows for ETL re-playability if business logic changes without losing raw source data.
  * **Partitioning Strategy**: Structuring paths by date (`ohlcv/ohlcv/year=YYYY/month=MM/day=DD/`) optimizes S3 Lifecycle policies and simplifies daily auditability.

* **AWS Console Steps**:
  1. Access **AWS Management Console** ➔ Navigate to **S3**.
  2. On the Buckets page, click **Create bucket**.
  3. Bucket Configuration:
     * **Bucket name**: `my-data-lake-raw-699061130094-dev`
     * **AWS Region**: `Asia Pacific (Singapore) ap-southeast-1`
  4. Scroll down and click **Create bucket**.

![(Figure 1.1) Amazon S3 Bucket Creation Interface](/images/workshop/image1.png)

![(Figure 1.2) General Configuration for S3 Raw Bucket](/images/workshop/image2.png)

* **Partitioned Directory Structure on Amazon S3 Raw Bucket**:

![(Figure 1.3 - Fig 1.1 in doc) Raw JSON Partitioned Folder Structure on Amazon S3 Raw Bucket](/images/workshop/image3.png)

---

#### 1.2 Create S3 Curated Bucket

* **Description**: Storage for cleaned, Snappy-compressed Parquet files, partitioned by ticker symbol Hive partitions (`ticker=XXX/part-000.parquet`), optimized for high-performance Athena SQL queries.
* **AWS Console Steps**:
  1. Navigate to **S3** ➔ Click **Create bucket**.
  2. **Bucket name**: `my-data-lake-curated-699061130094-dev`.
  3. **AWS Region**: `ap-southeast-1`.
  4. Click **Create bucket**.

![(Figure 1.4) S3 Curated Bucket Management Console](/images/workshop/image4.png)

* **Curated Parquet Partitioned Structure by Ticker**:
  Bucket `my-data-lake-curated-699061130094-dev` ➔ Directory `ohlcv/ohlcv/` displaying subfolders `ticker=ACB/`, `ticker=FPT/`.

![(Figure 1.5) Curated Folder ohlcv/ohlcv/ displaying ticker=ACB/, ticker=FPT/ partitions](/images/workshop/image5.png)

---

### PART 2: INGESTION LAYER (AWS LAMBDA COLLECTOR & EVENTBRIDGE)

#### 2.1 Configure Lambda Collector

* **Description**: Serverless Lambda function `financial-data-collector` calls market APIs, retrieves OHLCV and financial statements, packages JSON payloads, and writes directly to S3 Raw Bucket.
* **Detailed AWS Console Steps**:
  1. Navigate to **AWS Lambda Console** ➔ Click **Create function**.
  2. Choose **Author from scratch**:
     * **Function name**: `financial-data-collector`
     * **Runtime**: `Python 3.10` (or `Python 3.12`)
     * **Architecture**: `x86_64`
     * Click **Create function**.

![(Figure 2.1) AWS Lambda Functions Management Console](/images/workshop/image6.png)

![(Figure 2.2) Function Creation Page for financial-data-collector](/images/workshop/image7.png)

  3. **Configure Environment Variables**:
     * Go to **Configuration** tab ➔ Select **Environment variables** ➔ Click **Edit** ➔ Add variables:
       * `RAW_DATA_BUCKET` = `my-data-lake-raw-699061130094-dev`
       * `RAW_S3_PREFIX` = `ohlcv/ohlcv`
       * `DATA_PROVIDER` = `VNSTOCK_FREE`
       * `VNSTOCK_API_KEY` = `vnstock_aa0a41907f40564d9cba547e5865f922`

![(Figure 2.3 - Fig 2.1 in doc) Source Code & Environment Variable Settings of Lambda Collector](/images/workshop/image8.png)

* **Complete Source Code for AWS Lambda Collector (`lambda_function.py`)**:

```python
import json
import boto3
import os
import datetime
from vnstock import financial_report

s3_client = boto3.client('s3')

RAW_DATA_BUCKET = os.environ.get('RAW_DATA_BUCKET', 'my-data-lake-raw-699061130094-dev')
RAW_S3_PREFIX = os.environ.get('RAW_S3_PREFIX', 'ohlcv/ohlcv')
DATA_PROVIDER = os.environ.get('DATA_PROVIDER', 'VNSTOCK_FREE')
VNSTOCK_API_KEY = os.environ.get('VNSTOCK_API_KEY', 'vnstock_aa0a41907f40564d9cba547e5865f922')

def lambda_handler(event, context):
    symbol = event.get('symbol', 'FPT')
    print(f"Starting data collection for symbol: {symbol}")
    
    today = datetime.datetime.now()
    year_str = today.strftime("%Y")
    month_str = today.strftime("%m")
    day_str = today.strftime("%d")
    timestamp_str = today.strftime("%Y%m%d_%H%M%S")
    
    try:
        balance_sheet = financial_report(symbol=symbol, report_type='BalanceSheet', frequency='Yearly')
        income_stmt = financial_report(symbol=symbol, report_type='IncomeStatement', frequency='Yearly')
        cash_flow = financial_report(symbol=symbol, report_type='CashFlow', frequency='Yearly')
        
        raw_payload = {
            'symbol': symbol,
            'provider': DATA_PROVIDER,
            'collected_at': today.isoformat(),
            'balance_sheet': balance_sheet.to_dict(orient='records') if hasattr(balance_sheet, 'to_dict') else [],
            'income_statement': income_stmt.to_dict(orient='records') if hasattr(income_stmt, 'to_dict') else [],
            'cash_flow': cash_flow.to_dict(orient='records') if hasattr(cash_flow, 'to_dict') else []
        }
    except Exception as e:
        print(f"Error crawling symbol {symbol}: {str(e)}")
        raw_payload = {'symbol': symbol, 'error': str(e)}
        
    s3_key = f"{RAW_S3_PREFIX}/year={year_str}/month={month_str}/day={day_str}/batch_{symbol}_{timestamp_str}.json"
    
    s3_client.put_object(
        Bucket=RAW_DATA_BUCKET,
        Key=s3_key,
        Body=json.dumps(raw_payload, ensure_ascii=False),
        ContentType='application/json'
    )
    
    return {
        'statusCode': 200,
        'body': f"Successfully saved raw data for {symbol} to S3: s3://{RAW_DATA_BUCKET}/{s3_key}"
    }
```

---

#### 2.2 Daily Scheduling with Amazon EventBridge

* **Description**: Configures Cron EventBridge Schedule to trigger the ingestion pipeline automatically daily at 16:00 VN (10:00 UTC, after market close).
* **Detailed AWS Console Steps**:
  1. Access **Amazon EventBridge Console** ➔ **Schedules** ➔ Click **Create schedule**.
  2. **Schedule name**: `daily-financial-pipeline-schedule`.
  3. **Cron expression**: `0 10 * * ? *` (`10:00 UTC = 16:00 VN`).
  4. **Target**: Select AWS Lambda Function `financial-data-collector` (or Step Functions).
  5. Click **Create schedule**.

![(Figure 2.4) EventBridge Schedule Cron Expression Setup](/images/workshop/image21.png)

![(Figure 2.5 - Fig 6.2 in doc) Enabled Daily Scheduler on EventBridge Console](/images/workshop/image22.png)