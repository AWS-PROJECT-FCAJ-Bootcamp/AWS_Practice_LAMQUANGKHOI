---
title: "Triển khai demo Financial Data Stream trên AWS"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

# 5.4. TRIỂN KHAI DEMO FINANCIAL DATA STREAM TRÊN AWS

### Tổng quan quy trình triển khai End-to-End
Chương này hướng dẫn chi tiết toàn bộ quy trình thực thi thực tế (Execution & Demo Deployment) của **Hệ thống Thu thập và Phân tích Dữ liệu Tài chính Chứng khoán Việt Nam trên AWS Serverless**. 

Nhóm thực hiện luồng tích hợp liên tục từ khâu thu thập dữ liệu thô, biến đổi ETL Spark, cập nhật Data Catalog, truy vấn SQL Athena, gửi thông báo cảnh báo qua Email và hiển thị giao diện Web Dashboard trên AWS Amplify.

---

### Danh mục các bước triển khai (5.4.1 - 5.4.12)

* **[5.4.1. Mã nguồn và config các key của dự án]({{< ref "5.4.1-source-code-keys" >}})**: Tổng hợp toàn bộ mã nguồn Python cho Lambda Collector, Lambda Email và ETL PySpark.
* **[5.4.2. Dữ liệu mẫu của dự án]({{< ref "5.4.2-sample-data" >}})**: Cấu trúc Data Payload mẫu JSON cho dữ liệu giá chứng khoán OHLCV và Báo cáo tài chính.
* **[5.4.3. Triển khai dữ liệu mẫu lên S3 của hệ thống]({{< ref "5.4.3-s3-sample-data-upload" >}})**: Hướng dẫn đẩy dữ liệu mẫu ban đầu vào S3 Raw Bucket.
* **[5.4.4. Lập lịch hàng ngày với Amazon EventBridge]({{< ref "5.4.4-eventbridge-scheduling" >}})**: Cấu hình Cron Schedule tự động kích hoạt pipeline vào 16:00 VN.
* **[5.4.5. PROCESS LAYER (AWS ETL GLUE JOB)]({{< ref "5.4.5-process-layer-glue-job" >}})**: Thực thi tiến trình Glue PySpark biến đổi JSON thành Parquet.
* **[5.4.6. CATALOG & QUERY LAYER (AWS GLUE CRAWLER & ATHENA)]({{< ref "5.4.6-catalog-query-layer" >}})**: Trích xuất Schema tự động và thực thi SQL phân tích trên Athena.
* **[5.4.7. Kiểm thử luồng từ data source đến S3 curated]({{< ref "5.4.7-stream-testing" >}})**: Kiểm tra luồng dữ liệu tự động từ API về đến S3 Curated.
* **[5.4.8. Hosting giao diện thông qua Amplify]({{< ref "5.4.8-hosting-amplify" >}})**: Triển khai ứng dụng Web Dashboard React trên AWS Amplify.
* **[5.4.9. Đăng ký người dùng với AWS Cognito]({{< ref "5.4.9-user-auth-cognito" >}})**: Tạo User Pool và cấp tài khoản truy cập ứng dụng.
* **[5.4.10. Notification Layer (LAMBDA EMAIL & AMAZON SES)]({{< ref "5.4.10-notification-layer" >}})**: Xác thực Email SES và gửi báo cáo HTML tự động.
* **[5.4.11. API, Auth & Security Layer (DYNAMODB, COGNITO, API GATEWAY, WAF)]({{< ref "5.4.11-api-auth-security-layer" >}})**: Kết nối API REST Gateway và bảo vệ bởi WAF.
* **[5.4.12. Dọn dẹp tài nguyên và clean up data]({{< ref "5.4.12-resource-cleanup" >}})**: Quy trình thu hồi tài nguyên để tránh phát sinh chi phí ngoài ý muốn.
