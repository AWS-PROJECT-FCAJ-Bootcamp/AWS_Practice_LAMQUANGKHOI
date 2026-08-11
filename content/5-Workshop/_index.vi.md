---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# HỆ THỐNG THU THẬP VÀ PHÂN TÍCH DỮ LIỆU TÀI CHÍNH CHỨNG KHOÁN VIỆT NAM TRÊN AWS SERVERLESS

### Tổng quan bài thực hành Workshop
Trong chuỗi bài lab này, nhóm mình hướng dẫn xây dựng chi tiết **Hệ thống Thu thập và Phân tích Dữ liệu Tài chính Chứng khoán Việt Nam trên nền tảng AWS Serverless**. Hệ thống giải quyết bài toán cào dữ liệu Báo cáo Tài chính tự động, chuẩn hóa dữ liệu thô, tính toán bộ chỉ số tài chính, dự đoán nguy cơ **Kiệt quệ Tài chính (Financial Distress)** / Phá sản của các doanh nghiệp niêm yết trên 3 sàn chứng khoán Việt Nam (**HOSE, HNX, UPCOM**) và cung cấp REST API, giao diện Web Dashboard cùng hệ thống cảnh báo qua Email.

Kiến trúc toàn vẹn của bài lab được xây dựng dựa trên 100% dịch vụ Serverless đám mây AWS, giúp tự động mở rộng quy mô, tối ưu hiệu năng và tiết kiệm tối đa chi phí vận hành:
* **Thu thập dữ liệu (Ingestion Layer)**: Amazon EventBridge, AWS Step Functions, AWS Lambda / ECS Ingestor (tích hợp thư viện `vnstock`).
* **Lưu trữ Data Lake (Storage Layer)**: Amazon S3 (`S3 Raw Bucket` & `S3 Curated Bucket`).
* **Xử lý dữ liệu & Truy vấn (ETL & Query Layer)**: AWS Glue Job (PySpark/Python), AWS Glue Crawler, AWS Glue Data Catalog, Amazon Athena.
* **API & Xác thực (API & Auth Layer)**: Amazon Cognito, AWS WAF, Amazon API Gateway, AWS Lambda Backend.
* **Giao diện & Cảnh báo (Dashboard & Notification Layer)**: AWS Amplify (React/Next.js), AWS Lambda, Amazon SES.

> [!IMPORTANT]
> 🌐 **MINH CHỨNG SẢN PHẨM & TRANG WEB THỰC TẾ (LIVE DEMO WEB DASHBOARD)**
> * **Đường dẫn ứng dụng Web (Live Demo URL):** [https://feature-dashboard.dgku51j8dnv70.amplifyapp.com/login](https://feature-dashboard.dgku51j8dnv70.amplifyapp.com/login)
> * **Tài khoản thử nghiệm (Test Email):** `thoa@gmail.com`
> * **Mật khẩu thử nghiệm (Test Password):** `1111111`

---

### Danh mục bài thực hành

1. **[5.1. Giới thiệu & Kiến trúc tổng quan]({{< ref "5.1-workshop-overview" >}})**
   * Giới thiệu bài toán phân tích rủi ro tài chính doanh nghiệp niêm yết và kiến trúc 5 phân vùng AWS Serverless.
2. **[5.2. Luồng Thu thập Dữ liệu tự động (Data Ingestion Pipeline)]({{< ref "5.2-pipeline" >}})**
   * Tạo S3 Raw Bucket, lọc danh sách doanh nghiệp phi tài chính, viết Lambda cào dữ liệu BCTC từ `vnstock` và thiết lập EventBridge + Step Functions.
3. **[5.3. Xử lý dữ liệu & Data Lake (AWS Glue ETL, Data Catalog & Athena)]({{< ref "5.3-glue-config" >}})**
   * Tạo S3 Curated Bucket, viết AWS Glue Job làm sạch dữ liệu, Winsorization, tính toán chỉ số tài chính & Altman Z-Score, chạy Crawler và truy vấn Athena.
4. **[5.4. Xây dựng REST API & Xác thực người dùng (Cognito, API Gateway & Lambda API)]({{< ref "5.4-amplify" >}})**
   * Cấu hình Cognito User Pool, xây dựng REST API Gateway được bảo vệ bởi AWS WAF và viết Lambda Backend tiếp nhận request truy vấn dữ liệu từ Athena.
5. **[5.5. Triển khai Web Dashboard & Cảnh báo Email (Amplify, Lambda & SES)]({{< ref "5.5-policy" >}})**
   * Triển khai giao diện ứng dụng Web Dashboard lên AWS Amplify và cấu hình Lambda + Amazon SES tự động gửi email cảnh báo nguy cơ phá sản.
6. **[5.6. Dọn dẹp tài nguyên & Tổng kết]({{< ref "5.6-cleanup" >}})**
   * Hướng dẫn dọn dẹp các tài nguyên AWS đã khởi tạo sau khi hoàn tất bài workshop để tránh phát sinh chi phí ngoài ý muốn.