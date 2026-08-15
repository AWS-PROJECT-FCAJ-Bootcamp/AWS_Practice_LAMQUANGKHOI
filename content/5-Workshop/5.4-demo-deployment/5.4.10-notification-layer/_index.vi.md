---
title: "Notification Layer (LAMBDA EMAIL & AMAZON SES)"
date: 2024-01-01
weight: 10
chapter: false
pre: " <b> 5.4.10. </b> "
---

# 5.4.10. NOTIFICATION LAYER — LAMBDA EMAIL & AMAZON SES

Tầng thông báo **Notification Layer** tự động hóa việc tổng hợp kết quả chạy pipeline và gửi email báo cáo HTML trực quan cho nhà đầu tư/quản trị viên qua **Amazon SES (Simple Email Service)**.

---

### 1. Xác thực Email trên Amazon SES

1. Mở **Amazon SES Console** -> Select **Verified identities** -> Click **Create identity**.
2. Identity type: Select **Email address**.
3. Email address: Nhập địa chỉ email nhận báo cáo `<VERIFIED_EMAIL>` -> Click **Create identity**.
4. Mở hộp thư email, click vào đường link xác nhận từ Amazon Web Services gửi đến.

![(Hình 5.4.10.1) Màn hình khởi tạo Verified Identity địa chỉ Email trên Amazon SES Console](/images/workshop/image17.png)

![(Hình 5.4.10.2) Danh sách địa chỉ Email đã xác thực thành công trên Amazon SES Console](/images/workshop/image18.png)

---

### 2. Mã nguồn Lambda Email Notification (`lambda_function.py`)

```python
import boto3
import json
import os

ses_client = boto3.client('ses', region_name=os.environ.get('AWS_DEFAULT_REGION', 'ap-southeast-1'))
ADMIN_EMAIL = os.environ.get('ADMIN_EMAIL', '<VERIFIED_EMAIL>')

def lambda_handler(event, context):
    pipeline_status = event.get('status', 'SUCCESS')
    processed_tickers = event.get('processed_tickers', 119)
    failed_tickers = event.get('failed_tickers', 0)
    
    subject = f"📊 [AWS Financial Data Platform] Report: Pipeline Execution Status ({pipeline_status})"
    
    html_body = '''
    <html>
    <body>
        <h2>Báo Cáo Tự Động Luồng Dữ Liệu Tài Chính AWS</h2>
        <p>Tiến trình cào dữ liệu và ETL Glue đã hoàn tất thành công.</p>
        <p>Trạng thái Pipeline: SUCCESS</p>
        <p>Số lượng Ticker thành công: 119</p>
    </body>
    </html>
    '''
    
    response = ses_client.send_email(
        Source=ADMIN_EMAIL,
        Destination={'ToAddresses': [ADMIN_EMAIL]},
        Message={
            'Subject': {'Data': subject, 'Charset': 'UTF-8'},
            'Body': {'Html': {'Data': html_body, 'Charset': 'UTF-8'}}
        }
    )
    
    return {'statusCode': 200, 'body': 'Email report sent successfully!'}
```

![(Hình 5.4.10.3) Giao diện cấu hình mã nguồn hàm Lambda financial-data-email](/images/workshop/image19.png)

![(Hình 5.4.10.4) Hộp thư đến Gmail mở Email báo cáo pipeline có bảng màu sắc HTML hiển thị kết quả](/images/workshop/image20.png)
