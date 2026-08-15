---
title: "Kiểm thử luồng từ data source đến S3 curated"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 5.4.7. </b> "
---

# 5.4.7. Kiểm thử Luồng Tự động hóa từ Data Source đến S3 Curated

Để xác nhận tính toàn vẹn của luồng Data Stream, nhóm tiến hành kiểm thử End-to-End từ bước cào dữ liệu qua Lambda Collector đến bước phân vùng Parquet trên S3 Curated.

---

### Quy trình xác minh tính đúng đắn dữ liệu

1. Kích hoạt thủ công hàm Lambda Collector `financial-data-collector` với event test `{"symbol": "FPT"}`.
2. Kiểm tra log CloudWatch xác nhận ghi dữ liệu thô thành công vào `s3://my-data-lake-raw-<ACCOUNT_ID>/ohlcv/ohlcv/year=2026/...`.
3. Chạy Glue ETL Job `ohlcv-glue-processor` và Glue Crawler `ohlcv-crawler`.
4. Kiểm tra S3 Curated Bucket xuất hiện thư mục phân vùng mới `ticker=FPT/` chứa file Parquet.

![(Hình 5.4.7.1) Cấu trúc thư mục ohlcv/ohlcv/ hiển thị danh sách phân vùng theo Ticker Parquet trên S3 Curated](/images/workshop/image5.png)
