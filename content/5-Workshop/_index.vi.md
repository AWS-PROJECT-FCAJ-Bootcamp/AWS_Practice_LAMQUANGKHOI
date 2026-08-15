---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# HỆ THỐNG THU THẬP VÀ PHÂN TÍCH DỮ LIỆU TÀI CHÍNH CHỨNG KHOÁN VIỆT NAM TRÊN AWS SERVERLESS

### Tổng quan bài thực hành Workshop
Trong chuỗi bài lab hướng dẫn này, nhóm hướng dẫn chi tiết cách xây dựng **Hệ thống Thu thập và Phân tích Dữ liệu Tài chính Chứng khoán Việt Nam trên nền tảng AWS Serverless**. Hệ thống giải quyết bài toán cào dữ liệu thô (OHLCV, Báo cáo Tài chính), chuẩn hóa dữ liệu, tính toán bộ chỉ số tài chính (MA20, RSI14, return_pct) và phân vùng lưu trữ trên Data Lake, cung cấp REST API, xác thực Cognito, giao diện Web Dashboard và hệ thống gửi Email cảnh báo tự động.

> [!IMPORTANT]
> 🌐 **MINH CHỨNG SẢN PHẨM & TRANG WEB THỰC TẾ (LIVE DEMO WEB DASHBOARD)**
> * **Đường dẫn ứng dụng Web (Live Demo URL):** [https://feature-dashboard.dgku51j8dnv70.amplifyapp.com/login](https://feature-dashboard.dgku51j8dnv70.amplifyapp.com/login)
> * **Tài khoản thử nghiệm (Test Email):** `thoa@gmail.com`
> * **Mật khẩu thử nghiệm (Test Password):** `1111111`

---

### Danh mục các bài thực hành

1. **[5.1. Tổng quan dự án]({{< ref "5.1-overview" >}})**
   * Bối cảnh, mục tiêu, phạm vi, thách thức, actor, use cases và kế hoạch thực hiện.
2. **[5.2. Yêu cầu chuẩn bị trước dự án]({{< ref "5.2-prerequisites" >}})**
   * Các yêu cầu chuẩn bị và cấu hình môi trường trước khi bắt đầu dự án.
3. **[5.3. Thiết kế và kiến trúc hạ tầng của dự án pipeline lấy về dữ liệu trên AWS]({{< ref "5.3-architecture-design" >}})**
   * Khởi tạo và cấu hình S3, IAM, Glue, Lambda và các IAM Policy.
4. **[5.4. Triển khai demo Financial Data Stream trên AWS]({{< ref "5.4-demo-deployment" >}})**
   * Triển khai chi tiết gồm mã nguồn, dữ liệu mẫu, S3, EventBridge, Glue ETL, Glue Crawler, Athena, kiểm thử luồng, Amplify hosting, Cognito, SES email, API Gateway, WAF và dọn dẹp tài nguyên.
5. **[5.5. Kiểm thử dự án & Ước lượng chi phí]({{< ref "5.5-testing-cost-estimation" >}})**
   * Ước lượng chi phí qua Pricing Calculator, CloudWatch Dashboard, Log Insights và kiểm thử end-to-end.
6. **[5.6. Tổng kết dự án & Định hướng phát triển]({{< ref "5.6-summary-future-work" >}})**
   * Tổng kết kết quả đạt được và định hướng phát triển dự án.