---
title: "Kiểm thử end - to - end"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.5.4. </b> "
---

# 5.5.4. Ma trận Kiểm thử Hệ thống End-to-End

Quy trình kiểm thử End-to-End được thực hiện nhằm kiểm tra toàn bộ luồng xử lý từ khi cào dữ liệu đến khi hiển thị biểu đồ trên giao diện người dùng.

---

### Bảng Kết Quả Kiểm Thử (Testing Verification Matrix)

| Mã testcase | Kịch bản kiểm thử | Kết quả kỳ vọng | Trạng thái |
| :--- | :--- | :--- | :--- |
| **TC-01** | Kích hoạt Lambda Collector cào mã `FPT` | Ghi file JSON thô thành công lên S3 Raw Bucket | **PASSED** |
| **TC-02** | Chạy Glue ETL Job `ohlcv-glue-processor` | Xuất file Parquet nén Snappy phân vùng `ticker=FPT/` | **PASSED** |
| **TC-03** | Chạy Glue Crawler `ohlcv-crawler` | Cập nhật bảng `ohlcv` trong Glue Data Catalog | **PASSED** |
| **TC-04** | Truy vấn SQL trên Amazon Athena Editor | Trả về kết quả chứa các cột `ma20`, `rsi_14` dưới < 2s | **PASSED** |
| **TC-05** | Đăng nhập Cognito trên Web Dashboard | Trả về Token JWT hợp lệ và mở giao diện Dashboard | **PASSED** |
| **TC-06** | Gửi email báo cáo qua Amazon SES | Nhận email định dạng HTML thành công trong Gmail | **PASSED** |
