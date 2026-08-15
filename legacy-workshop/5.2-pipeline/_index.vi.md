---
title: "Luồng Thu thập & Lưu trữ Dữ liệu Thô (Data Ingestion & Storage)"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

# 5.2. LUỒNG THU THẬP & LƯU TRỮ DỮ LIỆU THÔ (DATA INGESTION & STORAGE)

Trong bài học này, nhóm mình tiến hành cấu hình **Storage Layer (Amazon S3)** và **Ingestion Layer (AWS Lambda Collector & EventBridge Scheduler)** để tự động hóa quy trình cào dữ liệu chứng khoán (OHLCV, Báo cáo Tài chính) và lưu trữ phân vùng vào Data Lake.

---

### PHẦN 1: STORAGE LAYER (AMAZON S3)

#### 1.1 Tạo S3 Raw Bucket

* **Nội dung mô tả**: Nơi lưu trữ dữ liệu JSON thô thu thập hàng ngày từ API VNStock/Yahoo. Dữ liệu phân vùng theo thời gian thu thập dạng `ohlcv/ohlcv/year=YYYY/month=MM/day=DD/batch_<timestamp>.json`.
* **Các nguyên tắc thiết kế Data Lake**:
  * **Landing Zone (Vùng hạ cánh dữ liệu thô)**: S3 Raw đóng vai trò lưu trữ nguyên bản dữ liệu JSON thu thập từ các API nguồn (`vnstock`, Yahoo Finance).
  * **Nguyên tắc Immutability (Không biến đổi)**: Dữ liệu thô trên S3 Raw tuyệt đối không bị chỉnh sửa hay xóa bỏ. Điều này đảm bảo khả năng Re-play ETL (chạy lại tiến trình xử lý dữ liệu từ đầu) khi có thay đổi trong logic kinh doanh mà không sợ mất dữ liệu gốc.
  * **Chiến lược Phân vùng (Partitioning)**: Tổ chức thư mục theo ngày thu thập dạng `ohlcv/ohlcv/year=YYYY/month=MM/day=DD/batch_<timestamp>.json` giúp quản lý vòng đời dữ liệu (S3 Lifecycle) và dễ dàng kiểm vết theo ngày.

* **Thao tác AWS Console**:
  1. Truy cập **AWS Management Console** ➔ Tìm dịch vụ **S3**.
  2. Tại trang S3 Buckets, bấm nút **Create bucket**.
  3. Cấu hình Bucket:
     * **Bucket name**: `my-data-lake-raw-699061130094-dev`
     * **AWS Region**: `Asia Pacific (Singapore) ap-southeast-1`
  4. Cuộn xuống cuối trang bấm **Create bucket**.

![(Hình 1.1) Giao diện khởi tạo Amazon S3 Bucket](/images/workshop/image1.png)

![(Hình 1.2) Giao diện cấu hình General configuration cho S3 Raw Bucket](/images/workshop/image2.png)

* **Cấu trúc thư mục phân vùng lưu trữ dữ liệu JSON thô trên Amazon S3 Raw Bucket**:

![(Hình 1.3 - Hình 1.1 trong tài liệu) Cấu trúc thư mục phân vùng lưu trữ dữ liệu JSON thô trên Amazon S3 Raw Bucket](/images/workshop/image3.png)

---

#### 1.2 Tạo S3 Curated Bucket

* **Nội dung mô tả**: Nơi lưu trữ dữ liệu sạch định dạng Parquet nén Snappy, phân vùng theo mã cổ phiếu Hive Partition (`ticker=XXX/part-000.parquet`), phục vụ cho Athena truy vấn hiệu năng cao.
* **Thao tác AWS Console**:
  1. Vào **S3** ➔ Click **Create bucket**.
  2. **Bucket name**: `my-data-lake-curated-699061130094-dev`.
  3. **AWS Region**: `ap-southeast-1`.
  4. Bấm **Create bucket**.

![(Hình 1.4) Màn hình quản lý S3 Curated Bucket my-data-lake-curated-699061130094-dev](/images/workshop/image4.png)

* **Cấu trúc thư mục dữ liệu đã tinh chế (Curated Parquet) phân vùng theo Ticker**:
  Bucket `my-data-lake-curated-699061130094-dev` ➔ Thư mục `ohlcv/ohlcv/` hiển thị các folder `ticker=ACB/`, `ticker=FPT/`.

![(Hình 1.5) Cấu trúc thư mục ohlcv/ohlcv/ hiển thị danh sách phân vùng theo ticker=ACB/, ticker=FPT/](/images/workshop/image5.png)

---

### PHẦN 2: INGESTION LAYER (AWS LAMBDA COLLECTOR & EVENTBRIDGE)

#### 2.1 Cấu hình Lambda Collector

* **Nội dung mô tả**: Hàm Serverless Lambda `financial-data-collector` chịu trách nhiệm gọi API chứng khoán, lấy giá OHLCV/báo cáo tài chính, đóng gói theo chuẩn Data Contract JSON và ghi trực tiếp lên S3 Raw Bucket.
* **Thao tác AWS Console Chi tiết**:
  1. Truy cập **AWS Lambda Console** ➔ Click **Create function**.
  2. Chọn **Author from scratch**:
     * **Function name**: `financial-data-collector`
     * **Runtime**: `Python 3.10` (hoặc `Python 3.12`)
     * **Architecture**: `x86_64`
     * Click **Create function**.

![(Hình 2.1) Màn hình danh sách AWS Lambda Functions](/images/workshop/image6.png)

![(Hình 2.2) Màn hình tạo mới Function financial-data-collector](/images/workshop/image7.png)

  3. **Cấu hình Biến môi trường (Environment variables)**:
     * Vào tab **Configuration** ➔ Chọn **Environment variables** ➔ Bấm **Edit** ➔ Thêm các biến:
       * `RAW_DATA_BUCKET` = `my-data-lake-raw-699061130094-dev`
       * `RAW_S3_PREFIX` = `ohlcv/ohlcv`
       * `DATA_PROVIDER` = `VNSTOCK_FREE`
       * `VNSTOCK_API_KEY` = `vnstock_aa0a41907f40564d9cba547e5865f922`

![(Hình 2.3 - Hình 2.1 trong tài liệu) Giao diện cấu hình mã nguồn và Biến môi trường của Lambda Collector](/images/workshop/image8.png)

* **Mã nguồn hoàn chỉnh của AWS Lambda Collector (`lambda_function.py`)**:

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
    print(f"Bắt đầu thu thập dữ liệu tài chính cho mã chứng khoán: {symbol}")
    
    today = datetime.datetime.now()
    year_str = today.strftime("%Y")
    month_str = today.strftime("%m")
    day_str = today.strftime("%d")
    timestamp_str = today.strftime("%Y%m%d_%H%M%S")
    
    # Cào các báo cáo tài chính thô từ API vnstock
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
        print(f"Lỗi khi cào dữ liệu mã {symbol}: {str(e)}")
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
        'body': f"Đã lưu thành công dữ liệu thô mã {symbol} vào S3: s3://{RAW_DATA_BUCKET}/{s3_key}"
    }
```

---

#### 2.2 Lập lịch hàng ngày với Amazon EventBridge

* **Nội dung mô tả**: Tạo Cron EventBridge Schedule kích hoạt Lambda/Step Functions tự động vào 16:00 giờ VN (10:00 UTC, sau khi thị trường chứng khoán đóng cửa).
* **Thao tác AWS Console Chi tiết**:
  1. Truy cập **Amazon EventBridge Console** ➔ **Schedules** ➔ Bấm **Create schedule**.
  2. **Schedule name**: `daily-financial-pipeline-schedule`.
  3. **Cron expression**: `0 10 * * ? *` (`10:00 UTC = 16:00 VN`).
  4. **Target**: Chọn AWS Lambda Function `financial-data-collector` (hoặc Step Functions).
  5. Bấm **Create schedule**.

![(Hình 2.4) Giao diện cấu hình biểu thức Cron trên Amazon EventBridge Scheduler](/images/workshop/image21.png)

![(Hình 2.5 - Hình 6.2 trong tài liệu) Lịch trình kích hoạt tự động hàng ngày trên Amazon EventBridge Scheduler (trạng thái Enabled)](/images/workshop/image22.png)