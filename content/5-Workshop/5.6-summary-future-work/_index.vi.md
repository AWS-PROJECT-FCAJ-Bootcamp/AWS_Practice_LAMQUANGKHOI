---
title: "Tổng kết dự án & Định hướng phát triển"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

# 5.6. TỔNG KẾT DỰ ÁN VÀ ĐỊNH HƯỚNG PHÁT TRIỂN

### 1. Tổng kết Kết quả Đạt được (Project Achievements)

Dự án **Hệ thống Thu thập và Phân tích Dữ liệu Tài chính Chứng khoán Việt Nam trên AWS Serverless** đã hoàn thành 100% các mục tiêu đề ra:
- **Kiến trúc Data Lake đa tầng**: Phân tách rõ ràng giữa phân vùng `Raw` (JSON thô) và `Curated` (Parquet nén Snappy), tối ưu 80% dung lượng lưu trữ.
- **Tự động hóa toàn diện**: Tích hợp EventBridge Scheduler, AWS Lambda, Glue ETL PySpark và Glue Crawler vận hành không cần sự can thiệp thủ công.
- **Phân tích hiệu năng cao**: Amazon Athena thực thi câu lệnh SQL trực tiếp trên S3 với tốc độ phản hồi dưới 2 giây.
- **Giao diện & Bảo mật**: Web Dashboard hosted trên AWS Amplify, bảo mật với Amazon Cognito User Pool và API Gateway WAF.
- **Tối ưu chi phí**: Chi phí vận hành duy trì ở mức chỉ khoảng **~$10.35 USD/Tháng**.

---

### 2. Bài học Kinh nghiệm (Lessons Learned)

- **Xử lý tính tương thích môi trường Serverless**: Việc chủ động chuyển đổi từ thư viện `VNStock` sang `yfinance` giúp hàm Lambda chạy ổn định trên hạ tầng AWS mà không bị chặn IP Datacenter.
- **Quản lý phân vùng Spark**: Phân vùng Hive theo `ticker` giúp cải thiện vượt bậc tốc độ đọc của Glue ETL và Athena.

---

### 3. Định hướng Phát triển Mở rộng (Future Roadmap)

1. **Tích hợp Machine Learning Feature Store**: Triển khai AWS SageMaker để huấn luyện các mô hình dự báo xu hướng giá cổ phiếu.
2. **Hệ thống Cảnh báo Tín hiệu Giao dịch Real-time**: Tích hợp Amazon Kinesis Data Streams và AWS SNS gửi tin nhắn cảnh báo qua Telegram/Zalo Bot.
3. **Mở rộng Hạ tầng Multi-Region**: Thiết lập cơ chế S3 Cross-Region Replication để tăng cường khả năng khắc phục sự cố (Disaster Recovery).
