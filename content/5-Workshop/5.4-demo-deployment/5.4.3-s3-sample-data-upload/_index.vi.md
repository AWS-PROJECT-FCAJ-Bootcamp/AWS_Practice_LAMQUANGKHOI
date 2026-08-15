---
title: "Triển khai dữ liệu mẫu lên S3 của hệ thống"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.4.3. </b> "
---

# 5.4.3. Triển khai Dữ liệu Mẫu lên S3 Storage

Quy trình nạp dữ liệu mồi (Seed Data) ban đầu vào S3 Raw Bucket giúp kích hoạt thử nghiệm toàn bộ tiến trình biến đổi ETL Spark và kiểm tra tính nhất quán của Schema.

---

### Các bước đẩy dữ liệu mẫu lên S3 Console

1. Mở **Amazon S3 Console** -> Truy cập Bucket `my-data-lake-raw-<ACCOUNT_ID>`.
2. Chuyển vào đường dẫn thư mục: `ohlcv/ohlcv/year=2026/month=08/day=15/`.
3. Bấm **Upload** -> Chọn file `batch_sample.json` từ máy tính cục bộ.
4. Bấm **Upload** để hoàn tất quá trình lưu trữ.

![(Hình 5.4.3.1) Giao diện cấu hình General configuration và nạp dữ liệu mẫu cho S3 Raw Bucket](/images/workshop/image2.png)
