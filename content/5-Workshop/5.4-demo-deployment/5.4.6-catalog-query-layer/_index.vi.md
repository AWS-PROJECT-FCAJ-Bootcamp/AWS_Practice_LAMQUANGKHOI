---
title: "CATALOG & QUERY LAYER (AWS GLUE CRAWLER & ATHENA)"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.4.6. </b> "
---

# 5.4.6. CATALOG & QUERY LAYER — AWS GLUE CRAWLER & ATHENA

Tầng **Catalog & Query Layer** kết hợp **AWS Glue Crawler** để tự động cập nhật Schema dữ liệu Parquet và **Amazon Athena** để thực thi các câu lệnh SQL phân tích dữ liệu hiệu năng cao.

---

### 1. Trích xuất Schema tự động với AWS Glue Crawler

Crawler `ohlcv-crawler` tự động quét S3 Curated Bucket, nhận diện các cột dữ liệu mới và cập nhật bảng `ohlcv` trong database `financial_data_lake`.

![(Hình 5.4.6.1) Màn hình thêm S3 Data Store cho Glue Crawler](/images/workshop/image13.png)

![(Hình 5.4.6.2) Cấu hình quy tắc cập nhật Schema và xử lý dữ liệu của AWS Glue Crawler](/images/workshop/image14.png)

---

### 2. Truy vấn dữ liệu phân tích trên Amazon Athena Editor

Nhà phân tích hoặc ứng dụng Web Dashboard có thể trực tiếp thực thi SQL truy vấn bảng Parquet:

```sql
SELECT ticker, date, close_price, ma20, rsi_14, return_pct 
FROM "financial_data_lake"."ohlcv" 
ORDER BY date DESC LIMIT 100;
```

![(Hình 5.4.6.3) Khung chạy câu SQL và chọn database financial_data_lake trên Athena Editor](/images/workshop/image15.png)

![(Hình 5.4.6.4) Kết quả truy vấn SQL hiển thị các cột ticker, close_price, ma20, rsi_14 trên Amazon Athena Editor](/images/workshop/image16.png)
