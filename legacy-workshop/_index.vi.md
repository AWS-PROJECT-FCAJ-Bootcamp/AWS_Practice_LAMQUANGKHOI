---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# HỆ THỐNG THU THẬP VÀ PHÂN TÍCH DỮ LIỆU TÀI CHÍNH CHỨNG KHOÁN VIỆT NAM TRÊN AWS SERVERLESS

### Tổng quan bài thực hành Workshop
Trong chuỗi bài lab hướng dẫn này, nhóm mình hướng dẫn chi tiết cách xây dựng **Hệ thống Thu thập và Phân tích Dữ liệu Tài chính Chứng khoán Việt Nam trên nền tảng AWS Serverless**. Hệ thống giải quyết bài toán cào dữ liệu thô (OHLCV, Báo cáo Tài chính), chuẩn hóa dữ liệu, tính toán bộ chỉ số tài chính (MA20, RSI14, return_pct) và phân vùng lưu trữ trên Data Lake, cung cấp REST API, xác thực Cognito, giao diện Web Dashboard và hệ thống gửi Email cảnh báo tự động.

Kiến trúc toàn vẹn của bài lab được xây dựng dựa trên 100% dịch vụ Serverless đám mây AWS, giúp tự động mở rộng quy mô, tối ưu hiệu năng và tiết kiệm tối đa chi phí vận hành:
* **Storage Layer (Amazon S3)**: S3 Raw Bucket (`my-data-lake-raw-699061130094-dev`) & S3 Curated Bucket (`my-data-lake-curated-699061130094-dev`).
* **Ingestion Layer (AWS Lambda Collector & EventBridge)**: Lambda Collector (`financial-data-collector`), EventBridge Scheduler (`daily-financial-pipeline-schedule`).
* **Process & Query Layer (AWS Glue ETL & Athena)**: IAM Role (`AWSGlueETLProcessorRole-dev`), Glue ETL Job PySpark (`ohlcv-glue-processor`), Glue Crawler (`ohlcv-crawler`), Amazon Athena (`financial_data_lake.ohlcv`).
* **API, Auth & Security Layer (DynamoDB, Cognito, API Gateway & WAF)**: DynamoDB (`Users`, `UserWatchlists`), Cognito User Pool (`financial-data-user-pool-dev`), REST API (`financial-data-platform-api`) & AWS WAF.
* **Notification Layer & Web Dashboard (SES, Lambda Email & Amplify)**: Amazon SES (`duyphong242004@gmail.com`), Lambda Email (`financial-data-email`), AWS Amplify Web Dashboard.

> [!IMPORTANT]
> 🌐 **MINH CHỨNG SẢN PHẨM & TRANG WEB THỰC TẾ (LIVE DEMO WEB DASHBOARD)**
> * **Đường dẫn ứng dụng Web (Live Demo URL):** [https://feature-dashboard.dgku51j8dnv70.amplifyapp.com/login](https://feature-dashboard.dgku51j8dnv70.amplifyapp.com/login)
> * **Tài khoản thử nghiệm (Test Email):** `thoa@gmail.com`
> * **Mật khẩu thử nghiệm (Test Password):** `1111111`

---

### Danh mục các bài thực hành

1. **[5.1. Giới thiệu & Kiến trúc tổng quan]({{< ref "5.1-workshop-overview" >}})**
   * Tổng quan kiến trúc hệ thống 5 phân vùng AWS Serverless và mục tiêu của bài workshop.
2. **[5.2. Luồng Thu thập & Lưu trữ Dữ liệu Thô (Data Ingestion & Storage)]({{< ref "5.2-pipeline" >}})**
   * Khởi tạo S3 Raw Bucket, S3 Curated Bucket, viết Lambda Collector cào dữ liệu chứng khoán và đặt lịch Cron trên EventBridge Scheduler.
3. **[5.3. Xử lý dữ liệu & Data Lake (AWS Glue ETL, Data Catalog & Athena)]({{< ref "5.3-glue-config" >}})**
   * Cấu hình IAM Role Glue, viết AWS Glue PySpark Job làm sạch dữ liệu, tính chỉ số MA20/RSI14, chạy Glue Crawler và truy vấn SQL trên Athena.
4. **[5.4. REST API, Cơ sở dữ liệu & Xác thực (DynamoDB, Cognito & API Gateway)]({{< ref "5.4-amplify" >}})**
   * Khởi tạo bảng NoSQL DynamoDB, cấu hình Cognito User Pool, bảo mật REST API Gateway bằng Cognito Authorizer & AWS WAF.
5. **[5.5. Cảnh báo Email & Triển khai Web Dashboard (SES, Lambda Email & Amplify)]({{< ref "5.5-policy" >}})**
   * Xác thực địa chỉ Email trên Amazon SES, cấu hình Lambda Email gửi báo cáo HTML tự động và triển khai Web Dashboard trên AWS Amplify.
6. **[5.6. Dọn dẹp tài nguyên & Tổng kết]({{< ref "5.6-cleanup" >}})**
   * Hướng dẫn dọn dẹp các tài nguyên AWS đã khởi tạo sau khi hoàn tất bài workshop để tránh phát sinh chi phí ngoài ý muốn.