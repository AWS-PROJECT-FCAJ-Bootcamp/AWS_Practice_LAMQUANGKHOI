---
title: "Luồng Thu thập Dữ liệu tự động (Data Ingestion Pipeline)"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

# 5.2. LUỒNG THU THẬP DỮ LIỆU TỰ ĐỘNG (DATA INGESTION PIPELINE)

Trong phần này, nhóm mình tiến hành thiết lập **Ingestion Layer** để tự động cào và lưu trữ dữ liệu Báo cáo Tài chính từ 3 sàn chứng khoán Việt Nam (**HOSE, HNX, UPCOM**) vào **Amazon S3 Raw Bucket**.

---

### Bước 1: Tạo Amazon S3 Raw Bucket
1. Truy cập vào **AWS Management Console** ➔ chọn dịch vụ **S3**.
2. Nhấp **Create bucket**.
3. Đặt tên Bucket: `s3-vietnam-financial-raw-data-prod`.
4. Chọn AWS Region: `ap-southeast-1` (Singapore).
5. Giữ nguyên cấu hình mã hóa **SSE-S3 (AES-256)** và bật tính năng **Bucket Versioning**.
6. Nhấp **Create bucket**.

---

### Bước 2: Lọc danh sách Doanh nghiệp phi tài chính
Do cấu trúc Báo cáo Tài chính của ngành ngân hàng và tài chính có bản chất khác biệt hoàn toàn so với doanh nghiệp sản xuất/thương mại, nhóm mình thực hiện lọc bỏ hoàn toàn các nhóm:
- ❌ **Ngân hàng (Banks)**
- ❌ **Công ty Chứng khoán (Securities)**
- ❌ **Công ty Bảo hiểm (Insurance)**
- ❌ **Công ty Tài chính & Quỹ đầu tư**

✅ Chỉ giữ lại danh sách doanh nghiệp phi tài chính niêm yết trên 3 sàn HOSE, HNX, UPCOM.

---

### Bước 3: Viết mã nguồn AWS Lambda Ingestor với `vnstock`

Mã nguồn Python cào dữ liệu thô báo cáo tài chính 5 năm liên tục:

```python
import json
import boto3
import pandas as pd
from vnstock import financial_report

s3_client = boto3.client('s3')
BUCKET_NAME = 's3-vietnam-financial-raw-data-prod'

def lambda_handler(event, context):
    symbol = event.get('symbol', 'VNM')
    print(f"Bắt đầu cào dữ liệu cho mã: {symbol}")
    
    # 1. Cào Bảng cân đối kế toán
    balance_sheet = financial_report(symbol=symbol, report_type='BalanceSheet', frequency='Yearly')
    # 2. Cào Báo cáo Kết quả Kinh doanh
    income_statement = financial_report(symbol=symbol, report_type='IncomeStatement', frequency='Yearly')
    # 3. Cào Báo cáo Lưu chuyển Tiền tệ
    cash_flow = financial_report(symbol=symbol, report_type='CashFlow', frequency='Yearly')
    
    # Ghi dữ liệu thô vào S3 Raw Bucket
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
        'body': f"Đã lưu thành công dữ liệu thô mã {symbol} vào S3: {s3_key}"
    }
```

---

### Bước 4: Khởi tạo Workflow điều phối AWS Step Functions & EventBridge

1. **AWS Step Functions**: Tạo State Machine để duyệt qua danh sách danh mục mã cổ phiếu, gọi Lambda Ingestor xử lý song song, hỗ trợ cơ chế Retry khi gặp Rate Limit và tự động ghi log Checkpoint.
2. **Amazon EventBridge Scheduler**: Cấu hình quy tắc Cron Job kích hoạt Step Functions định kỳ (ví dụ: vào 00:00 ngày đầu tiên của mỗi tháng) để tự động thu thập báo cáo tài chính quý/năm mới nhất.

![Sơ đồ Workflow điều phối AWS Step Functions liên kết với AWS Glue Job](/images/StepFunction.png)

*Mô tả luồng điều phối Step Functions State Machine trong hệ thống:*
* **Khởi chạy Workflow**: Triggers tự động từ Amazon EventBridge Cron Scheduler hoặc gọi thủ công để kích hoạt toàn bộ pipeline dữ liệu.
* **Tác vụ cào & lưu trữ dữ liệu thô**: Gọi AWS Lambda / ECS Ingestor cào dữ liệu BCTC từ `vnstock`, thực hiện checkpointing và lưu file JSON vào `S3 Raw Bucket`.
* **Tác vụ AWS Glue ETL Job**: Step Functions chuyển sang bước gọi tác vụ **AWS Glue StartJobRun**, tự động kích hoạt tiến trình PySpark ETL làm sạch dữ liệu thô, tính toán các chỉ số tài chính ($CR$, $ROA$, $ROE$, $DAR$, $WCTA$) và gán nhãn nguy cơ phá sản **Altman Z-Score**.
* **Cập nhật Catalog & Hoàn tất**: Sau khi Glue Job hoàn tất thành công, Step Functions tiếp tục kích hoạt AWS Glue Crawler quét metadata cập nhật vào Glue Data Catalog. Nếu phát sinh lỗi, hệ thống tự động thực hiện cơ chế Retry và chuyển sang trạng thái cảnh báo Fail State.