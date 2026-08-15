---
title: "Thiết kế và kiến trúc hạ tầng của dự án pipeline lấy về dữ liệu trên AWS"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

# THIẾT KẾ VÀ KIẾN TRÚC HẠ TẦNG PIPELINE LẤY DỮ LIỆU TÀI CHÍNH TRÊN AWS

### Tổng quan hạ tầng kỹ thuật
Trong chương này, nhóm hướng dẫn chi tiết quy trình khởi tạo, thiết kế và triển khai hạ tầng kỹ thuật cho **Hệ thống Thu thập và Phân tích Dữ liệu Tài chính Chứng khoán Việt Nam trên AWS Serverless**. Toàn bộ kiến trúc được phân tách thành các phân vùng dịch vụ độc lập, giúp đảm bảo tính tự động hóa, khả năng mở rộng quy mô linh hoạt và tuân thủ các tiêu chuẩn bảo mật chuẩn doanh nghiệp.

---

### Danh mục các bước triển khai chi tiết

* **[5.3.1. Khởi tạo và cấu hình S3]({{< ref "5.3.1-s3-setup" >}})**: Hướng dẫn tạo S3 Raw Bucket lưu trữ dữ liệu JSON thô và S3 Curated Bucket lưu trữ dữ liệu Parquet phân vùng theo Ticker.
* **[5.3.2. Khởi tạo và cấu hình IAM]({{< ref "5.3.2-iam-setup" >}})**: Hướng dẫn tạo IAM Roles, Trust Relationships và cấp quyền bảo mật cho các dịch vụ thực thi tự động.
* **[5.3.3. Khởi tạo và cấu hình Glue]({{< ref "5.3.3-glue-setup" >}})**: Cấu hình AWS Glue PySpark ETL Job làm sạch dữ liệu, tính chỉ số MA20/RSI14, chạy Glue Crawler và truy vấn Athena.
* **[5.3.4. Khởi tạo và cấu hình Lambda]({{< ref "5.3.4-lambda-setup" >}})**: Khởi tạo hàm AWS Lambda Collector trích xuất dữ liệu thị trường và hàm Lambda Email báo cáo tự động.
* **[5.3.5. Khởi tạo và cấu hình IAM policy]({{< ref "5.3.5-iam-policy" >}})**: Tổng hợp toàn bộ bộ mã JSON IAM Policies chuẩn bảo mật và không chứa dữ kiện riêng.
