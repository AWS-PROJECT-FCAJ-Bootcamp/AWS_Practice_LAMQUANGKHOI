---
title: "Source Code & Configuration Keys"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---

# 5.4.1. Project Source Code and Configuration Keys

This section consolidates all deployment source code and environment variables for the core components of the pipeline: **AWS Lambda Collector**, **AWS Glue ETL PySpark**, and **AWS Lambda Email Notification**.

---

### 1. AWS Lambda Collector Source Code (`lambda_function.py`)

Serverless Python function fetching market OHLCV prices via `yfinance`, packaging JSON payloads, and uploading to S3 Raw Bucket.

```python
import json
import boto3
import os
import datetime
import yfinance as yf

s3_client = boto3.client('s3')

RAW_DATA_BUCKET = os.environ.get('RAW_DATA_BUCKET', 'my-data-lake-raw-<ACCOUNT_ID>')
RAW_S3_PREFIX = os.environ.get('RAW_S3_PREFIX', 'ohlcv/ohlcv')
DATA_PROVIDER = os.environ.get('DATA_PROVIDER', 'YFINANCE_API')

def lambda_handler(event, context):
    symbol = event.get('symbol', 'FPT')
    ticker_symbol = f"{symbol}.VN"
    print(f"Ingesting market data for ticker: {ticker_symbol}")
    
    today = datetime.datetime.now()
    year_str = today.strftime("%Y")
    month_str = today.strftime("%m")
    day_str = today.strftime("%d")
    timestamp_str = today.strftime("%Y%m%d_%H%M%S")
    
    try:
        ticker = yf.Ticker(ticker_symbol)
        df_hist = ticker.history(period="1mo")
        
        records = []
        for idx, row in df_hist.iterrows():
            records.append({
                "symbol": symbol,
                "trading_date": idx.strftime("%Y-%m-%d"),
                "open_price": float(row["Open"]),
                "high_price": float(row["High"]),
                "low_price": float(row["Low"]),
                "close_price": float(row["Close"]),
                "volume": int(row["Volume"])
            })
            
        raw_payload = {
            'symbol': symbol,
            'provider': DATA_PROVIDER,
            'collected_at': today.isoformat(),
            'records': records
        }
    except Exception as e:
        print(f"Error fetching ticker {symbol}: {str(e)}")
        raw_payload = {'symbol': symbol, 'error': str(e), 'records': []}
        
    s3_key = f"{RAW_S3_PREFIX}/year={year_str}/month={month_str}/day={day_str}/batch_{symbol}_{timestamp_str}.json"
    
    s3_client.put_object(
        Bucket=RAW_DATA_BUCKET,
        Key=s3_key,
        Body=json.dumps(raw_payload, ensure_ascii=False),
        ContentType='application/json'
    )
    
    return {
        'statusCode': 200,
        'body': f"Raw payload for {symbol} saved to S3: s3://{RAW_DATA_BUCKET}/{s3_key}"
    }
```

![(Figure 5.4.1.1) Source Code and Environment Variables Interface of AWS Lambda Collector](/images/workshop/image8.png)

---

### 2. Environment Variables Configuration

| Variable Key | Sample Value | Description |
| :--- | :--- | :--- |
| `RAW_DATA_BUCKET` | `my-data-lake-raw-<ACCOUNT_ID>` | S3 Raw Bucket name storing JSON payloads |
| `RAW_S3_PREFIX` | `ohlcv/ohlcv` | S3 folder prefix path |
| `DATA_PROVIDER` | `YFINANCE_API` | Market data provider name |
| `AWS_DEFAULT_REGION` | `<REGION>` | Deployment AWS Region (`ap-southeast-1`) |
