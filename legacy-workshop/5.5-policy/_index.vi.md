---
title: "Cảnh báo Email & Triển khai Web Dashboard (SES, Lambda Email & Amplify)"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

# 5.5. CẢNH BÁO EMAIL & TRIỂN KHAI WEB DASHBOARD (SES, LAMBDA EMAIL & AMPLIFY)

Trong bài học này, nhóm mình tiến hành xây dựng **Notification Layer & UI Layer** sử dụng **Amazon SES**, **AWS Lambda Email** và **AWS Amplify** để tự động hóa quy trình gửi email báo cáo vận hành pipeline dữ liệu và triển khai ứng dụng Web Dashboard.

---

### PHẦN 5: NOTIFICATION LAYER (LAMBDA EMAIL & AMAZON SES)

#### 5.1 Cấu hình Amazon SES (Simple Email Service)

* **Nội dung mô tả**: Xác thực địa chỉ Email quản trị viên (`duyphong242004@gmail.com`) trên Amazon SES để làm địa chỉ gửi và nhận báo cáo tự động từ hệ thống.
* **Thao tác AWS Console Chi tiết**:
  1. Truy cập **Amazon SES Console** ➔ **Verified identities** ➔ Click **Create identity**.
  2. **Identity type**: Chọn **Email address**.
  3. **Email address**: Nhập `duyphong242004@gmail.com` ➔ Click **Create identity**.
  4. Đăng nhập vào hộp thư Gmail `duyphong242004@gmail.com`, mở email xác thực do Amazon Web Services gửi đến và bấm vào đường link xác nhận.

![(Hình 5.1) Màn hình khởi tạo Verified Identity địa chỉ Email trên Amazon SES](/images/workshop/image17.png)

![(Hình 5.2 - Hình 5.1 trong tài liệu) Danh sách địa chỉ Email đã xác thực thành công trên Amazon SES (trạng thái Verified màu xanh)](/images/workshop/image18.png)

---

#### 5.2 Lambda Email Notification

* **Nội dung mô tả**: Hàm Serverless Lambda `financial-data-email` nhận dữ liệu callback tổng hợp từ pipeline, đóng gói giao diện HTML Email báo cáo có màu sắc trực quan hiển thị số lượng Ticker xử lý thành công/thất bại và gửi qua Amazon SES.
* **Thao tác AWS Console Chi tiết**:
  1. Vào **AWS Lambda Console** ➔ Click **Create function**.
  2. **Function name**: `financial-data-email`.
  3. **Runtime**: `Python 3.10` / `Python 3.12`.
  4. Tab **Configuration** ➔ **Environment variables** ➔ Khai báo: `ADMIN_EMAIL` = `duyphong242004@gmail.com`.

![(Hình 5.3) Giao diện cấu hình mã nguồn hàm Lambda financial-data-email](/images/workshop/image19.png)

![(Hình 5.4 - Hình 5.2 trong tài liệu) Hộp thư đến Gmail mở Email báo cáo pipeline có bảng màu sắc HTML hiển thị kết quả tự động](/images/workshop/image20.png)

* **Mã nguồn Python hoàn chỉnh cho Lambda Email Notification (`lambda_function.py`)**:

```python
import boto3
import json
import os

ses_client = boto3.client('ses', region_name=os.environ.get('AWS_DEFAULT_REGION', 'ap-southeast-1'))
ADMIN_EMAIL = os.environ.get('ADMIN_EMAIL', 'duyphong242004@gmail.com')

def lambda_handler(event, context):
    pipeline_status = event.get('status', 'SUCCESS')
    processed_tickers = event.get('processed_tickers', 119)
    failed_tickers = event.get('failed_tickers', 0)
    
    subject = f"📊 [AWS Financial Data Platform] Report: Pipeline Execution Status ({pipeline_status})"
    
    html_body = f"""
    <html>
    <head>
        <style>
            body {{ font-family: Arial, sans-serif; line-height: 1.6; color: #333; }}
            .container {{ max-width: 600px; margin: 0 auto; padding: 20px; border: 1px solid #e0e0e0; border-radius: 8px; }}
            .header {{ background-color: #232f3e; color: #ffffff; padding: 15px; text-align: center; border-radius: 6px 6px 0 0; }}
            .badge-success {{ background-color: #28a745; color: white; padding: 4px 8px; border-radius: 4px; font-weight: bold; }}
            .table {{ width: 100%; border-collapse: collapse; margin-top: 15px; }}
            .table th, .table td {{ border: 1px solid #ddd; padding: 10px; text-align: left; }}
            .table th {{ background-color: #f4f4f4; }}
        </style>
    </head>
    <body>
        <div class="container">
            <div class="header">
                <h2>Báo Cáo Tự Động Luồng Dữ Liệu Tài Chính AWS</h2>
            </div>
            <div class="content">
                <p>Kính gửi Quản trị viên,</p>
                <p>Tiến trình cào dữ liệu, xử lý ETL Glue và cập nhật Data Catalog đã hoàn tất thành công.</p>
                <table class="table">
                    <tr><th>Trạng thái Pipeline</th><td><span class="badge-success">{pipeline_status}</span></td></tr>
                    <tr><th>Số lượng Ticker thành công</th><td><b>{processed_tickers}</b></td></tr>
                    <tr><th>Số lượng Ticker thất bại</th><td><b>{failed_tickers}</b></td></tr>
                    <tr><th>S3 Raw Bucket</th><td><code>my-data-lake-raw-699061130094-dev</code></td></tr>
                    <tr><th>S3 Curated Bucket</th><td><code>my-data-lake-curated-699061130094-dev</code></td></tr>
                </table>
                <p>Bạn có thể truy vấn bảng dữ liệu <code>financial_data_lake.ohlcv</code> trên Amazon Athena Editor.</p>
            </div>
        </div>
    </body>
    </html>
    """
    
    response = ses_client.send_email(
        Source=ADMIN_EMAIL,
        Destination={'ToAddresses': [ADMIN_EMAIL]},
        Message={
            'Subject': {'Data': subject, 'Charset': 'UTF-8'},
            'Body': {'Html': {'Data': html_body, 'Charset': 'UTF-8'}}
        }
    )
    
    return {
        'statusCode': 200,
        'body': json.dumps({'message': 'Email báo cáo đã được gửi thành công!', 'MessageId': response['MessageId']})
    }
```

---

### PHẦN 6: TRIỂN KHAI WEB DASHBOARD TRÊN AWS AMPLIFY

* **Nội dung mô tả**: Triển khai ứng dụng Web Dashboard (React/Next.js) lên **AWS Amplify**, kết nối với REST API Gateway và Cognito User Pool đã khởi tạo.
* **Thao tác AWS Console Chi tiết**:
  1. Truy cập **AWS Amplify Console** ➔ Bấm **Host web app**.
  2. Kết nối với GitHub Repository chứa mã nguồn ứng dụng Web Dashboard.
  3. Cấu hình biến môi trường Build (Environment Variables):
     * `REACT_APP_API_URL`: URL của REST API Gateway `financial-data-platform-api` (`dev`).
     * `REACT_APP_COGNITO_USER_POOL_ID`: `financial-data-user-pool-dev` User Pool ID.
     * `REACT_APP_COGNITO_CLIENT_ID`: `financial-data-web-client` App Client ID.
  4. Bấm **Save and deploy**. AWS Amplify sẽ tự động thực thi luồng CI/CD build ứng dụng và cấp phát tên miền HTTPS.

> [!TIP]
> 🌐 **TRANG WEB DEMO THỰC TẾ & TÀI KHOẢN THỬ NGHIỆM**
> * **Đường dẫn ứng dụng Web (Live URL):** [https://feature-dashboard.dgku51j8dnv70.amplifyapp.com/login](https://feature-dashboard.dgku51j8dnv70.amplifyapp.com/login)
> * **Tài khoản thử nghiệm (Test Email):** `thoa@gmail.com`
> * **Mật khẩu (Password):** `1111111`

