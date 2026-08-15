---
title: "Yêu cầu chuẩn bị trước dự án"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
aliases:
  - /5-workshop/5.2-pipeline/
---

# 5.2. Yêu cầu chuẩn bị dịch vụ AWS & Phân quyền IAM (Services & IAM Preparation)

Để triển khai dự án **Hệ thống Thu thập và Phân tích Dữ liệu Tài chính Chứng khoán Việt Nam trên AWS Serverless**, việc chuẩn bị hạ tầng tập trung vào các **Dịch vụ Đám mây cốt lõi (AWS Services)** và **Cấu hình Phân quyền IAM (IAM Roles & Policies)**. 

Theo quy tắc trình bày: **Mỗi dịch vụ AWS cần chuẩn bị đi kèm đúng 01 hình ảnh minh họa thực tế liên quan đến dịch vụ đó**.

---

### 1. Dịch vụ Lưu trữ Amazon S3 (Storage Layer)

- **Mô tả dịch vụ & Mục đích**: Lưu trữ dữ liệu tài chính đa tầng (Data Lake) trên Region `ap-southeast-1` (Singapore).
  - **S3 Raw Bucket**: `my-data-lake-raw-699061130094-dev` (Lưu dữ liệu thô JSON phân vùng `ohlcv/ohlcv/year=YYYY/month=MM/day=DD/`).
  - **S3 Curated Bucket**: `my-data-lake-curated-699061130094-dev` (Lưu dữ liệu Parquet nén Snappy phân vùng Hive `ohlcv/ohlcv/ticker=XXX/`).
- **Yêu cầu Phân quyền IAM**:
  - Cấp quyền đọc/ghi (`s3:GetObject`, `s3:PutObject`, `s3:ListBucket`) cho AWS Lambda Collector và AWS Glue ETL Job.

![(Hình 5.2.1) Cấu trúc thư mục phân vùng lưu trữ dữ liệu JSON thô trên Amazon S3 Raw Bucket](/images/workshop/image3.png)

---

### 2. Dịch vụ Quản lý Nhận dạng & Phân quyền AWS IAM (Identity & Access Management)

- **Mô tả dịch vụ & Mục đích**: Quản lý truy cập và bảo mật toàn bộ tài nguyên hệ thống theo nguyên tắc quyền tối thiểu (*IAM Least-Privilege*).
- **Yêu cầu Phân quyền IAM**:
  - **IAM Role chính**: `AWSGlueETLProcessorRole-dev`
    - Trust Relationship: `glue.amazonaws.com`
    - AWS Managed Policy: `AWSGlueServiceRole`
    - Custom Inline Policy: Cấp quyền thao tác trên 02 S3 Buckets (`my-data-lake-raw-699061130094-dev`, `my-data-lake-curated-699061130094-dev`) và quyền kích hoạt `glue:StartCrawler`.

![(Hình 5.2.2) Giao diện cấu hình IAM Role AWSGlueETLProcessorRole-dev và gắn Policy bảo mật trên AWS IAM Console](/images/workshop/image9.png)

---

### 3. Dịch vụ Tính toán Serverless AWS Lambda (Ingestion Layer)

- **Mô tả dịch vụ & Mục đích**: Chạy các hàm Serverless Python cào dữ liệu chứng khoán hàng ngày (`financial-data-collector`) và gửi báo cáo email (`financial-data-email`).
- **Yêu cầu Phân quyền IAM**:
  - **Lambda Execution Role**: Cấp quyền ghi log CloudWatch (`logs:CreateLogStream`, `logs:PutLogEvents`), ghi dữ liệu vào S3 Raw Bucket và quyền gửi email qua Amazon SES (`ses:SendEmail`).
  - **Biến môi trường (Environment Variables)**: `RAW_DATA_BUCKET`, `RAW_S3_PREFIX`, `DATA_PROVIDER`.

![(Hình 5.2.3) Giao diện cấu hình mã nguồn và Biến môi trường của AWS Lambda Collector](/images/workshop/image8.png)

---

### 4. Dịch vụ Xử lý & Phân loại Dữ liệu AWS Glue (Processing & Catalog Layer)

- **Mô tả dịch vụ & Mục đích**: Chạy ETL Job PySpark (`ohlcv-glue-processor`) để làm sạch dữ liệu, tính chỉ số MA20/RSI14 và Crawler (`ohlcv-crawler`) tự động cập nhật Data Catalog.
- **Yêu cầu Phân quyền IAM**:
  - Gán IAM Role `AWSGlueETLProcessorRole-dev` cho Glue Job và Glue Crawler để đọc S3 Raw, ghi S3 Curated dạng Parquet và cập nhật Schema vào Glue Data Catalog.

![(Hình 5.2.4) Màn hình cấu hình chi tiết Job Parameters và IAM Role của AWS Glue ETL Job](/images/workshop/image11.png)

---

### 5. Trình truy vấn SQL Serverless Amazon Athena (Query Layer)

- **Mô tả dịch vụ & Mục đích**: Truy vấn dữ liệu phân tích SQL hiệu năng cao trực tiếp trên S3 Curated mà không cần hạ tầng cơ sở dữ liệu truyền thống.
- **Yêu cầu Phân quyền IAM**:
  - Quyền truy cập Athena Workgroup (`athena:StartQueryExecution`, `athena:GetQueryResults`) và quyền ghi kết quả truy vấn vào S3 Athena Query Result Bucket (`s3:GetBucketLocation`, `s3:PutObject`).

![(Hình 5.2.5) Màn hình Amazon Athena Query Editor truy vấn dữ liệu Parquet chuẩn hóa](/images/workshop/image16.png)

---

### 6. Cơ sở dữ liệu DynamoDB & Xác thực AWS Cognito (Database & Auth Security Layer)

- **Mô tả dịch vụ & Mục đích**: DynamoDB lưu trữ bảng NoSQL (`Users`, `UserWatchlists`) và AWS Cognito User Pool (`financial-data-user-pool-dev`) xác thực đăng nhập người dùng.
- **Yêu cầu Phân quyền IAM**:
  - Cognito Identity Pool IAM Roles (Authenticated/Unauthenticated Roles) và IAM permissions cấp quyền cho API Gateway/Lambda truy vấn DynamoDB (`dynamodb:GetItem`, `dynamodb:PutItem`, `dynamodb:Query`).

![(Hình 5.2.6) Màn hình Amazon Cognito User Pool Console hiển thị App Client ID và danh sách tài khoản người dùng](/images/workshop/image25.png)

---

### 7. Dịch vụ Cảnh báo SES & Lập lịch Amazon EventBridge (Notification & Automation Layer)

- **Mô tả dịch vụ & Mục đích**: Amazon SES (`duyphong242004@gmail.com`) gửi email báo cáo HTML và EventBridge Scheduler (`daily-financial-pipeline-schedule`) kích hoạt luồng tự động.
- **Yêu cầu Phân quyền IAM**:
  - EventBridge Execution Role cấp quyền kích hoạt hàm Lambda Collector (`lambda:InvokeFunction`) theo lịch biểu Cron daily.

![(Hình 5.2.7) Danh sách địa chỉ Email đã được xác thực thành công (Verified Identity) trên Amazon SES Console](/images/workshop/image18.png)
