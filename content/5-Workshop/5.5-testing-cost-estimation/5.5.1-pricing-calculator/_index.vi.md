---
title: "Pricing Calculator"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.5.1. </b> "
---

# 5.5.1. Ước lượng Chi phí Dự án với AWS Pricing Calculator

Hệ thống được thiết kế theo kiến trúc **Serverless & Pay-as-you-go**, giúp tối ưu hóa chi phí vận hành ở mức tối thiểu (quy mô PoC / Intern Lab dưới $20 USD/tháng).

---

### Bảng phân tích chi tiết chi phí từng dịch vụ hàng tháng

| Dịch vụ AWS | Quy mô sử dụng ước tính | Chi phí ước tính (USD/Tháng) |
| :--- | :--- | :--- |
| **Amazon S3** | 10 GB Raw JSON + 20 GB Curated Parquet | ~$0.75 |
| **AWS Lambda** | 300 requests/tháng, 512MB RAM, 10s execution | ~$0.00 (Miễn phí Free Tier) |
| **AWS Glue ETL** | 1 job/ngày, 2 Workers G.1X, chạy 5 phút/ngày | ~$7.50 |
| **Amazon Athena** | 50 GB dữ liệu scan/tháng | ~$0.25 |
| **Amazon DynamoDB** | 2 tables (On-demand mode), 1,000 read/write | ~$0.25 |
| **Amazon Cognito** | < 1,000 Monthly Active Users (MAU) | ~$0.00 (Free Tier 50k MAU) |
| **Amazon SES** | < 1,000 emails/tháng | ~$0.10 |
| **AWS Amplify** | Hosting React App, < 5 GB bandwidth/tháng | ~$1.50 |
| **TỔNG CHI PHÍ VẬN HÀNH** | **Hoàn thành toàn bộ luồng tự động hàng ngày** | **~$10.35 USD / Tháng** |
