---
title: "CloudWatch Dashboard"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.5.2. </b> "
---

# 5.5.2. Giám sát Hệ thống với CloudWatch Dashboard & Alarms

Để đảm bảo tính sẵn sàng và kiểm soát ngân sách đám mây, hệ thống cấu hình **Amazon CloudWatch Dashboard** hiển thị thời gian thực các chỉ số vận hành chính.

---

### 1. Chỉ số giám sát cốt lõi (Key Metrics)

- **Lambda Collector Metrics**: Errors count, Duration (ms), Throttles.
- **AWS Glue Job Metrics**: `glue.driver.aggregate.elapsedTime`, `glue.ALL.s3.filesystem.write_bytes`.
- **API Gateway Metrics**: 4XXError, 5XXError, Latency.

---

### 2. Cấu hình Cảnh báo Chi phí AWS Budgets

Hệ thống cài đặt cảnh báo **AWS Budgets** gửi email ngay lập tức tại các mốc:
- **50% Ngân sách ($10 USD)**: Cảnh báo thông tin.
- **80% Ngân sách ($16 USD)**: Cảnh báo quan trọng.
- **100% Ngân sách ($20 USD)**: Cảnh báo nghiêm trọng ngắt tiến trình tự động.
