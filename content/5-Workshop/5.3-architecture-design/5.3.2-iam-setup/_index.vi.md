---
title: "Khởi tạo và cấu hình IAM"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---

# 5.3.2. Khởi tạo và cấu hình các IAM Roles trong dự án

Để các dịch vụ Serverless tương tác với nhau an toàn, hệ thống khởi tạo các **IAM Roles** với phân quyền tối thiểu (*Least-Privilege*).

---

### 1. Khởi tạo IAM Role cho AWS Glue ETL Job (`AWSGlueETLProcessorRole`)

- **Mục đích**: Cấp quyền cho Glue ETL PySpark Job đọc dữ liệu thô trên S3 Raw, làm sạch dữ liệu, ghi file Parquet lên S3 Curated, cập nhật Data Catalog và kích hoạt Glue Crawler.
- **Thao tác trên AWS Console**:
  1. Truy cập IAM Console -> Chọn **Roles** -> Bấm **Create role**.
  2. Select trusted entity: Chọn **AWS service** -> Chọn **Glue** -> Bấm **Next**.
  3. Attach Policy: Tìm và chọn policy `AWSGlueServiceRole`.
  4. Bấm **Create policy** (Tạo Inline Policy) cấp quyền Read/Write S3 Raw & Curated (`my-data-lake-raw-<ACCOUNT_ID>`, `my-data-lake-curated-<ACCOUNT_ID>`) và `glue:StartCrawler`.
  5. Đặt tên Role: `AWSGlueETLProcessorRole` -> Bấm **Create role**.

![(Hình 5.3.2.1) Giao diện quản lý quyền và IAM Role cho AWS Glue ETL Job](/images/workshop/image9.png)

---

### 2. Khởi tạo IAM Role cho AWS Lambda Collector (`LambdaCollectorExecutionRole`)

- **Mục đích**: Cấp quyền cho hàm `financial-data-collector` ghi log thực thi vào CloudWatch Log Groups và ghi file JSON thô lên S3 Raw Bucket.
- **Thao tác trên AWS Console**:
  1. IAM Console -> Roles -> **Create role**.
  2. Trusted entity: **Lambda** (`lambda.amazonaws.com`).
  3. Cấp quyền `AWSLambdaBasicExecutionRole` và inline policy ghi `s3:PutObject` lên `my-data-lake-raw-<ACCOUNT_ID>/*`.

*(Hình ảnh minh họa khởi tạo IAM Role cho Lambda Collector: Sẽ cập nhật)*

---

### 3. Khởi tạo IAM Role cho EventBridge Scheduler (`EventBridgeSchedulerRole`)

- **Mục đích**: Cho phép EventBridge Scheduler tự động kích hoạt hàm Lambda Collector (`lambda:InvokeFunction`) theo lịch Cron daily.
- **Thao tác trên AWS Console**:
  1. IAM Console -> Roles -> **Create role**.
  2. Trusted entity: **Custom Trust Policy** cho `scheduler.amazonaws.com`.
  3. Cấp quyền `lambda:InvokeFunction` tới ARN của hàm `financial-data-collector`.

*(Hình ảnh minh họa khởi tạo IAM Role cho EventBridge Scheduler: Sẽ cập nhật)*
