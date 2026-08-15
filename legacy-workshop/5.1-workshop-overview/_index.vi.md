---
title: "Giới thiệu & Kiến trúc tổng quan"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

# 5.1. GIỚI THIỆU & KIẾN TRÚC HỆ THỐNG SERVERLESS

#### 🎯 Mục tiêu bài workshop
Trong chuỗi bài thực hành workshop này, nhóm mình hướng dẫn chi tiết cách nhóm mình xây dựng và triển khai thành công **Hệ thống Thu thập và Phân tích Dữ liệu Tài chính Chứng khoán Việt Nam trên nền tảng AWS Serverless**. Hệ thống thực hiện cào dữ liệu chứng khoán tự động từ các sàn (**HOSE, HNX, UPCOM**), lưu trữ nguyên bản trên Data Lake S3, tính toán các chỉ số phân tích kỹ thuật và tài chính (MA20, RSI14, return_pct), cung cấp các dịch vụ REST API bảo mật, xác thực người dùng bằng Cognito và tự động gửi email báo cáo vận hành hệ thống.

#### 🏗️ Kiến trúc Hệ thống AWS Cloud (5 Phân vùng chính)

Mô hình kiến trúc tổng quan của dự án bao gồm 5 lớp xử lý độc lập, tối ưu theo các chuẩn thiết kế AWS Well-Architected Framework:

![(Hình 5.1.1) Kiến trúc Hệ thống 5 Phân vùng AWS Serverless](/images/3layer_v1.0.drawio.png)

#### 🛠️ Danh mục các Dịch vụ AWS chính được sử dụng:

| Lớp Phân Vùng | Dịch vụ AWS | Trách nhiệm & Tài nguyên khởi tạo trong Bài Lab |
| :--- | :--- | :--- |
| **1. Storage Layer** | `Amazon S3 (raw)` | Raw Bucket (`my-data-lake-raw-699061130094-dev`) lưu JSON phân vùng `ohlcv/ohlcv/year=YYYY/month=MM/day=DD/` |
| | `Amazon S3 (curated)` | Curated Bucket (`my-data-lake-curated-699061130094-dev`) lưu Parquet phân vùng theo mã Ticker `ohlcv/ohlcv/ticker=XXX/` |
| **2. Ingestion Layer**| `AWS Lambda` | Function `financial-data-collector` (Python 3.10/3.12) cào dữ liệu từ API và ghi trực tiếp S3 Raw Bucket |
| | `Amazon EventBridge` | EventBridge Schedule (`daily-financial-pipeline-schedule`) chạy Cron 16:00 VN hàng ngày |
| **3. Process & Query**| `IAM Role` | Role `AWSGlueETLProcessorRole-dev` cấp quyền cho Glue ETL đọc S3 Raw, ghi S3 Curated & gọi Crawler |
| | `AWS Glue Job` | Glue PySpark Job (`ohlcv-glue-processor`) xử lý ETL, tính chỉ số MA20, RSI14 |
| | `AWS Glue Crawler` | Crawler (`ohlcv-crawler`) tự động cập nhật Schema 10 cột vào Glue Data Catalog |
| | `Amazon Athena` | Truy vấn SQL Serverless trực tiếp trên database `financial_data_lake.ohlcv` |
| **4. API, Auth & Security**| `Amazon DynamoDB` | Bảng NoSQL `Users` (PK: email) và `UserWatchlists` (PK: user_id, SK: symbol) |
| | `Amazon Cognito` | User Pool (`financial-data-user-pool-dev`) & App Client (`financial-data-web-client`) |
| | `Amazon API Gateway` | REST API (`financial-data-platform-api`) bảo vệ bởi Cognito Authorizer & AWS WAF |
| **5. Notification & UI**| `Amazon SES` | Verified Email Identity (`duyphong242004@gmail.com`) gửi/nhận email báo cáo tự động |
| | `AWS Lambda (Email)` | Function `financial-data-email` nhận callback, tạo HTML Email và phát tin qua SES |
| | `AWS Amplify` | Hosting và quản lý luồng CI/CD cho ứng dụng Web Dashboard React |

