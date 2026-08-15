---
title: "Tổng quan dự án"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

# HỆ THỐNG DỮ LIỆU TÀI CHÍNH CHỨNG KHOÁN TỰ ĐỘNG TRÊN AWS SERVERLESS

### Tổng quan dự án
**Hệ thống Thu thập và Phân tích Dữ liệu Tài chính Chứng khoán Việt Nam** là hệ thống Đám mây Serverless toàn diện (end-to-end), được thiết kế để giải quyết bài toán cào dữ liệu rời rạc, tổng hợp thủ công và thiếu nơi lưu trữ dữ liệu tài chính chuẩn hóa lâu dài. Xây dựng trên hạ tầng 100% AWS Serverless và quản lý bằng Infrastructure as Code (Terraform), hệ thống tự động thu thập dữ liệu thô (OHLCV, Báo cáo Tài chính), chuẩn hóa schema, tính toán bộ chỉ số kỹ thuật (MA20, RSI14, return_pct), phân vùng Data Lake đa tầng (Raw, Curated, Feature Store), đồng thời cung cấp REST API, xác thực Cognito, giao diện Web Dashboard và hệ thống gửi Email cảnh báo tự động.

> [!NOTE]
> Chương 5.1 được tổng hợp chi tiết dựa trên tài liệu **PRD Version 2.0** (Product Requirements Document), làm rõ bối cảnh bài toán, quy trình nghiệp vụ, thách thức triển khai, mục tiêu đo lường, phân tích Actor/Use Case và kế hoạch triển khai theo các giai đoạn.

---

### Danh mục các nội dung chi tiết

* **[5.1.1. Bối cảnh, mục tiêu và phạm vi]({{< ref "5.1.1-context-target-scope" >}})**: Bài toán thực tế, quy trình As-Is & To-Be, phương pháp tiếp cận tổng quan và đối tượng khách hàng mục tiêu.
* **[5.1.2. Thách thức và khó khăn]({{< ref "5.1.2-challenges" >}})**: Phân tích rủi ro nguồn dữ liệu, giới hạn truy xuất (rate limit), bảo mật IAM chuẩn doanh nghiệp và quản lý chi phí.
* **[5.1.3. Mục tiêu]({{< ref "5.1.3-objectives" >}})**: Bộ chỉ số đo lường hiệu năng (Metrics), các Yêu cầu Chức năng (FR) và Yêu cầu Phi chức năng (NFR).
* **[5.1.4. Actor và use cases]({{< ref "5.1.4-actors-use-cases" >}})**: Phân biệt Actor và Stakeholder, sơ đồ Use Case và C4 Model Level 1 (System Context Diagram).
* **[5.1.5. Kế hoạch thực hiện dự kiến (bắt đầu từ tuần 4)]({{< ref "5.1.5-execution-plan" >}})**: Lộ trình triển khai 5 giai đoạn, pipeline CI/CD và kế hoạch phân bổ công việc từ Tuần 4.
