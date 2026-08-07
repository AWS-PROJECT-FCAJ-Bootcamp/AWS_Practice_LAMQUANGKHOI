---
title: "Automated Data Ingestion Pipeline"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

# 5.2. AUTOMATED DATA INGESTION PIPELINE

In this module, our team sets up the **Data Ingestion Layer** to automatically scrape and store raw financial report datasets across 3 Vietnamese stock exchanges (**HOSE, HNX, UPCOM**) into an **Amazon S3 Raw Bucket**.

---

### Step 1: Create Amazon S3 Raw Bucket
1. Navigate to **AWS Management Console** ➔ select **S3**.
2. Click **Create bucket**.
3. Bucket Name: `s3-vietnam-financial-raw-data-prod`.
4. AWS Region: `ap-southeast-1` (Singapore).
5. Default Encryption: **SSE-S3 (AES-256)** with **Bucket Versioning** enabled.
6. Click **Create bucket**.

---

### Step 2: Filter Non-Financial Listed Tickers
Because the financial statement structures of banks and financial institutions differ significantly from commercial/manufacturing enterprises, our team filters out:
- ❌ **Banks**
- ❌ **Securities Firms**
- ❌ **Insurance Companies**
- ❌ **Financial Investment Funds**

✅ Retaining non-financial listed corporate tickers on HOSE, HNX, and UPCOM.

---

### Step 3: Write AWS Lambda Data Ingestor Script with `vnstock`

Python script to crawl 5-year historical financial statements:

```python
import json
import boto3
import pandas as pd
from vnstock import financial_report

s3_client = boto3.client('s3')
BUCKET_NAME = 's3-vietnam-financial-raw-data-prod'

def lambda_handler(event, context):
    symbol = event.get('symbol', 'VNM')
    print(f"Starting ingestion for ticker: {symbol}")
    
    # 1. Scrape Balance Sheet
    balance_sheet = financial_report(symbol=symbol, report_type='BalanceSheet', frequency='Yearly')
    # 2. Scrape Income Statement
    income_statement = financial_report(symbol=symbol, report_type='IncomeStatement', frequency='Yearly')
    # 3. Scrape Cash Flow Statement
    cash_flow = financial_report(symbol=symbol, report_type='CashFlow', frequency='Yearly')
    
    # Write payload to S3 Raw Bucket
    raw_payload = {
        'symbol': symbol,
        'balance_sheet': balance_sheet.to_dict(orient='records'),
        'income_statement': income_statement.to_dict(orient='records'),
        'cash_flow': cash_flow.to_dict(orient='records')
    }
    
    s3_key = f"raw/yearly/{symbol}_financial_data.json"
    s3_client.put_object(
        Bucket=BUCKET_NAME,
        Key=s3_key,
        Body=json.dumps(raw_payload, ensure_ascii=False),
        ContentType='application/json'
    )
    
    return {
        'statusCode': 200,
        'body': f"Successfully ingested raw financial data for {symbol} to S3: {s3_key}"
    }
```

---

### Step 4: Configure Workflow Orchestration with Step Functions & EventBridge

1. **AWS Step Functions**: Create a State Machine to iterate through ticker batches, execute Lambda Ingestor functions concurrently, handle rate limits with exponential backoff retries, and write progress checkpoints.
2. **Amazon EventBridge Scheduler**: Set up a Cron Rule to trigger the State Machine automatically (e.g., 00:00 on the 1st day of every month) to fetch updated quarterly/yearly financial reports.