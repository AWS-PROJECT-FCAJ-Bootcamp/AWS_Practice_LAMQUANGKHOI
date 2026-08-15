---
title: "Dọn dẹp tài nguyên & Tổng kết"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

# 5.6. DỌN DẸP TÀI NGUYÊN & TỔNG KẾT

Để tránh phát sinh các chi phí dịch vụ đám mây AWS ngoài ý muốn sau khi hoàn tất xây dựng và thực hành bài workshop, nhóm mình tiến hành dọn dẹp các tài nguyên theo quy trình từng bước dưới đây.

---

### Quy trình Dọn dẹp Tài nguyên AWS

1. **Xóa AWS Amplify Application**:
   * Truy cập **AWS Amplify Console** ➔ Chọn ứng dụng Web Dashboard ➔ Nhấp **Actions** ➔ Chọn **Delete app**.

2. **Xóa Amazon API Gateway & AWS WAF**:
   * Vào **Amazon API Gateway Console** ➔ Chọn REST API `financial-data-platform-api` ➔ Nhấp **Delete**.
   * Vào **AWS WAF** ➔ Xóa Web ACL đã gắn vào API Gateway.

3. **Xóa Amazon Cognito User Pool**:
   * Vào **Amazon Cognito Console** ➔ Chọn User Pool `financial-data-user-pool-dev` ➔ Xóa App Client `financial-data-web-client` ➔ Nhấp **Delete user pool**.

4. **Xóa Amazon DynamoDB Tables**:
   * Vào **Amazon DynamoDB Console** ➔ **Tables** ➔ Chọn bảng `Users` và `UserWatchlists` ➔ Bấm **Delete table**.

5. **Xóa AWS Lambda Functions & EventBridge Schedule**:
   * Vào **AWS Lambda Console** ➔ Xóa các hàm: `financial-data-collector` và `financial-data-email`.
   * Vào **Amazon EventBridge Console** ➔ **Schedules** ➔ Chọn schedule `daily-financial-pipeline-schedule` ➔ Bấm **Delete**.

6. **Xóa AWS Glue Jobs, Crawlers & Data Catalog**:
   * Vào **AWS Glue Console** ➔ Xóa Job `ohlcv-glue-processor`.
   * Xóa Crawler `ohlcv-crawler`.
   * Vào **Data Catalog Databases** ➔ Xóa Database `financial_data_lake`.
   * Vào **IAM Console** ➔ **Roles** ➔ Xóa IAM Role `AWSGlueETLProcessorRole-dev`.

7. **Xóa dữ liệu và Amazon S3 Buckets**:
   * Truy cập **Amazon S3 Console**.
   * Chọn Raw Bucket `my-data-lake-raw-699061130094-dev` ➔ Nhấp **Empty** để xóa rỗng toàn bộ objects/versions ➔ Nhấp **Delete**.
   * Chọn Curated Bucket `my-data-lake-curated-699061130094-dev` ➔ Nhấp **Empty** ➔ Nhấp **Delete**.

---

### 📝 Tổng kết Bài thực hành Workshop
Sau khi hoàn tất toàn bộ chuỗi bài lab này, nhóm mình đã làm chủ và hoàn thiện quy trình xây dựng **Hệ thống Thu thập và Phân tích Dữ liệu Tài chính Chứng khoán Việt Nam trên nền tảng AWS Serverless** end-to-end:
* **Storage Layer**: Tạo lập Data Lake S3 Raw (`my-data-lake-raw-699061130094-dev`) lưu trữ JSON thô và S3 Curated (`my-data-lake-curated-699061130094-dev`) lưu trữ Parquet phân vùng theo ticker.
* **Ingestion Layer**: Viết Lambda Collector (`financial-data-collector`), tích hợp API chứng khoán và lập lịch Cron hàng ngày 16:00 VN trên EventBridge Scheduler (`daily-financial-pipeline-schedule`).
* **Process & Query Layer**: Cấp quyền IAM Role (`AWSGlueETLProcessorRole-dev`), viết AWS Glue PySpark Job (`ohlcv-glue-processor`) chuẩn hóa dữ liệu, tính chỉ số MA20/RSI14, tự động chạy Crawler (`ohlcv-crawler`) và truy vấn SQL trên Amazon Athena (`financial_data_lake.ohlcv`).
* **API, Auth & Security Layer**: Tạo bảng NoSQL DynamoDB (`Users`, `UserWatchlists`), xác thực Cognito User Pool (`financial-data-user-pool-dev`) và bảo mật REST API Gateway (`financial-data-platform-api`) với AWS WAF.
* **Notification Layer & UI**: Xác thực Email trên Amazon SES (`duyphong242004@gmail.com`), gửi báo cáo HTML tự động qua Lambda Email (`financial-data-email`) và triển khai ứng dụng Web Dashboard trên AWS Amplify.