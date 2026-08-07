---
title: "Dọn dẹp tài nguyên & Tổng kết"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

# 5.6. DỌN DẸP TÀI NGUYÊN & TỔNG KẾT

Để tránh phát sinh các chi phí dịch vụ AWS đám mây ngoài ý muốn sau khi hoàn tất xây dựng và kiểm thử bài thực hành workshop, nhóm mình hướng dẫn quy trình dọn dẹp tài nguyên theo từng bước dưới đây.

---

### Các bước Dọn dẹp Tài nguyên AWS

1. **Xóa AWS Amplify Application**:
   * Truy cập **AWS Amplify Console** ➔ chọn app `vietnam-financial-dashboard` ➔ Nhấp **Actions** ➔ **Delete app**.

2. **Xóa Amazon API Gateway & AWS WAF**:
   * Vào **Amazon API Gateway** ➔ chọn `vietnam-financial-api` ➔ Nhấp **Delete**.
   * Vào **AWS WAF** ➔ xóa Web ACL đã gắn vào API Gateway.

3. **Xóa Amazon Cognito User Pool**:
   * Vào **Amazon Cognito** ➔ chọn `vietnam-financial-user-pool` ➔ Xóa App Client và nhấp **Delete user pool**.

4. **Xóa các AWS Lambda Functions & Step Functions**:
   * Vào **AWS Lambda Console** ➔ Xóa các hàm: `lambda-ingestor-vnstock`, `lambda-backend-api`, `lambda-ses-alert`.
   * Vào **AWS Step Functions** ➔ chọn State Machine cào dữ liệu ➔ Nhấp **Delete**.

5. **Xóa AWS Glue Jobs, Crawlers & Data Catalog**:
   * Vào **AWS Glue Console** ➔ Xóa Job `glue-job-pyspark-financial-etl`.
   * Xóa Crawler `crawler-vietnam-financial-curated`.
   * Vào **Data Catalog Databases** ➔ Xóa Database `vietnam_financial_db`.

6. **Xóa dữ liệu và Amazon S3 Buckets**:
   * Truy cập **Amazon S3 Console**.
   * Chọn `s3-vietnam-financial-raw-data-prod` ➔ Nhấp **Empty** để làm rỗng toàn bộ objects/versions ➔ Nhấp **Delete**.
   * Chọn `s3-vietnam-financial-curated-data-prod` ➔ Nhấp **Empty** ➔ Nhấp **Delete**.

---

### 📝 Tổng kết Bài thực hành Workshop
Sau khi hoàn tất toàn bộ chuỗi bài lab, bạn đã làm chủ quy trình xây dựng **Hệ thống Thu thập và Phân tích Dữ liệu Tài chính Chứng khoán Việt Nam trên nền tảng AWS Serverless** end-to-end:
* Xây dựng luồng cào dữ liệu tài chính tự động với Lambda, EventBridge và Step Functions.
* Làm sạch và tính toán bộ chỉ số tài chính ($CR$, $ROA$, $ROE$, $DAR$, $WCTA$, Altman Z-Score) bằng PySpark trên AWS Glue.
* Truy vấn SQL dữ liệu phân trang Parquet tốc độ cao với Amazon Athena.
* Bảo mật API với Amazon Cognito, AWS WAF và REST API Gateway.
* Trực quan hóa dữ liệu trên Web Dashboard AWS Amplify và phát Email cảnh báo rủi ro phá sản bằng Amazon SES.