---
title: "Khởi tạo và cấu hình Lambda"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.3.4. </b> "
---

# 5.3.4. Khởi tạo và cấu hình AWS Lambda Functions

Tầng Ingestion & Notification trong dự án **Financial Data Platform** sử dụng dịch vụ **AWS Lambda** để triển khai các hàm tính toán Serverless.

---

### 1. Khởi tạo hàm AWS Lambda Collector (`financial-data-collector`)

- **Mô tả**: Hàm Lambda chịu trách nhiệm tự động gọi API chứng khoán (`yfinance`), thu thập giá OHLCV & báo cáo tài chính, đóng gói JSON và ghi trực tiếp lên S3 Raw Bucket.
- **Thao tác trên AWS Console**:
  1. Truy cập AWS Lambda Console tại Region `<REGION>` -> Click **Create function**.
  2. Chọn **Author from scratch**:
     - Function name: `financial-data-collector`
     - Runtime: `Python 3.10` (hoặc `Python 3.12`)
     - Architecture: `x86_64`
  3. Change default execution role: Chọn IAM Role `LambdaCollectorExecutionRole`.
  4. Click **Create function**.
  5. Cấu hình **Biến môi trường (Environment variables)** tại tab *Configuration*:
     - `RAW_DATA_BUCKET` = `my-data-lake-raw-<ACCOUNT_ID>`
     - `RAW_S3_PREFIX` = `ohlcv/ohlcv`
     - `DATA_PROVIDER` = `YFINANCE_API`

![(Hình 5.3.4.1) Màn hình danh sách AWS Lambda Functions trên Console](/images/workshop/image6.png)

![(Hình 5.3.4.2) Màn hình tạo mới Function financial-data-collector](/images/workshop/image7.png)

![(Hình 5.3.4.3) Giao diện cấu hình mã nguồn và Biến môi trường của Lambda Collector](/images/workshop/image8.png)

---

### 2. Khởi tạo hàm AWS Lambda Email (`financial-data-email`)

- **Mô tả**: Hàm Lambda tự động tổng hợp kết quả pipeline, định dạng mẫu báo cáo HTML và gửi email cho Admin/Investor qua Amazon SES.
- **Thao tác trên AWS Console**:
  1. Lambda Console -> Click **Create function**.
  2. Function name: `financial-data-email`.
  3. Runtime: `Python 3.10`.
  4. Execution role: Chọn `LambdaEmailExecutionRole`.

*(Hình ảnh minh họa màn hình khởi tạo hàm Lambda Email: Sẽ cập nhật)*

---

### 3. Cấu hình Lambda Layer đóng gói phụ thuộc (`yfinance`, `pandas`)

- **Mục đích**: Đóng gói các thư viện mã nguồn ngoài (`yfinance`, `pandas`, `requests`) thành Lambda Layer dùng chung cho hàm Ingestion.
- **Thao tác trên AWS Console**: Lambda Console -> *Layers* -> Click *Create layer* -> Upload file ZIP thư viện Python.

*(Hình ảnh minh họa cấu hình Lambda Layer cho yfinance: Sẽ cập nhật)*
