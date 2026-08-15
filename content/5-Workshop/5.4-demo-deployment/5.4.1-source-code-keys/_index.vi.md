---
title: "Mã nguồn và config các key của dự án"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---

# 5.4.1. Mã nguồn và Cấu hình các Key của Dự án

Trong bước này, hệ thống tập hợp đầy đủ mã nguồn triển khai và các thông số cấu hình môi trường cho 03 thành phần cốt lõi của pipeline: **AWS Lambda Collector**, **AWS Glue ETL PySpark**, và **AWS Lambda Email Notification**.

---

### 1. Mã nguồn AWS Lambda Collector (`lambda_function.py`)

Hàm Serverless chịu trách nhiệm thu thập giá chứng khoán OHLCV từ thư viện `yfinance`, đóng gói JSON thô và đẩy trực tiếp lên S3 Raw Bucket.

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
    print(f"Bắt đầu thu thập dữ liệu chứng khoán cho mã: {ticker_symbol}")
    
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
        print(f"Lỗi khi thu thập dữ liệu mã {symbol}: {str(e)}")
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
        'body': f"Đã lưu thành công dữ liệu thô mã {symbol} vào S3: s3://{RAW_DATA_BUCKET}/{s3_key}"
    }
```

![(Hình 5.4.1.1) Giao diện cấu hình mã nguồn và Biến môi trường của AWS Lambda Collector trên Console](/images/workshop/image8.png)

---

### 2. Cấu hình các biến môi trường (Environment Variables)

| Tên biến môi trường | Giá trị ví dụ | Mô tả chức năng |
| :--- | :--- | :--- |
| `RAW_DATA_BUCKET` | `my-data-lake-raw-<ACCOUNT_ID>` | Tên S3 Bucket chứa dữ liệu thô JSON |
| `RAW_S3_PREFIX` | `ohlcv/ohlcv` | Đường dẫn thư mục phân vùng trong Bucket |
| `DATA_PROVIDER` | `YFINANCE_API` | Tên nhà cung cấp nguồn dữ liệu chứng khoán |
| `AWS_DEFAULT_REGION` | `<REGION>` | Mã Region triển khai dự án (`ap-southeast-1`) |
