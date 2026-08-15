---
title: "Mục tiêu"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.1.3. </b> "
---

# 5.1.3. Mục tiêu & Bộ chỉ số đo lường (Metrics)

### 1. Bộ chỉ số đo lường hiệu năng cốt lõi (Metrics)
Hệ thống đánh giá mức độ thành công dựa trên 3 nhóm chỉ số định lượng cụ thể:

1. **Chỉ số Độ chính xác & Toàn vẹn dữ liệu (Data Integrity)**:
   * Mục tiêu: **Tỷ lệ ticker bị lỗi (failed_tickers) < 1%** trong mỗi lượt cào hàng ngày.
   * Đảm bảo 100% bản ghi không bị thiếu các trường bắt buộc (`ticker`, `date`, `open`, `high`, `low`, `close`, `volume`).
2. **Chỉ số Hiệu năng & Độ trễ (Performance & Latency)**:
   * Mục tiêu: **Thời gian tải trang Dashboard < 2 giây** cho các truy vấn thông thường.
   * Mục tiêu: **Thời gian truy vấn Athena < 10 giây** cho tập dữ liệu quét dưới 100GB.
3. **Chỉ số Sẵn sàng cho AI/ML (Feature Readiness)**:
   * Mục tiêu: **100% dữ liệu Curated đạt chuẩn Parquet Feature Schema** làm đầu vào trực tiếp cho pipeline huấn luyện mô hình dự báo tài chính.

---

### 2. Ma trận Yêu cầu Chức năng (Functional Requirements - FR)

| Mã Yêu cầu | Tên Chức năng | Mô tả chi tiết Chức năng | Mức ưu tiên |
| :--- | :--- | :--- | :--- |
| **FR01** | Đăng nhập & Xác thực | Đăng ký, đăng nhập, làm mới token và đổi mật khẩu người dùng qua AWS Cognito. | Cao |
| **FR02** | Tìm kiếm mã cổ phiếu | Tra cứu và tìm kiếm thông tin các mã cổ phiếu trên sàn HOSE, HNX, UPCoM. | Cao |
| **FR03** | Xem dữ liệu tài chính | Hiển thị biểu đồ giá OHLCV, khối lượng giao dịch và các Báo cáo tài chính chi tiết. | Cao |
| **FR04** | Xem chỉ báo kỹ thuật | Tự động tính và hiển thị các chỉ báo kỹ thuật (MA20, RSI14, phần trăm biến động ngày). | Cao |
| **FR05** | Quản lý Portfolio | Tạo, cập nhật và theo dõi hiệu quả danh mục đầu tư cá nhân và lãi/lỗ tạm tính. | Trung bình |
| **FR06** | Quản lý Watchlist | Thêm/bớt các mã cổ phiếu quan tâm vào danh sách theo dõi riêng. | Trung bình |
| **FR07** | Nhận cảnh báo tự động | Gửi báo cáo tổng hợp và cảnh báo biến động giá tự động qua Amazon SES Email. | Cao |
| **FR08** | Quản trị người dùng | Admin xem danh sách user, phân quyền quản trị và xem log hoạt động hệ thống. | Trung bình |
| **FR09** | Đồng bộ dữ liệu tự động | Đặt lịch cào dữ liệu thô tự động từ các Nguồn bên ngoài vào S3 Raw Bucket hàng ngày. | Cao |
| **FR10** | Giám sát hệ thống | Theo dõi trạng thái hàm Lambda, Glue Job, số lượng file S3 và cảnh báo ngân sách AWS. | Cao |

---

### 3. Ma trận Yêu cầu Phi chức năng (Non-Functional Requirements - NFR)

| Nhóm NFR | Tiêu chuẩn & Chỉ số kỹ thuật cụ thể | Cơ chế triển khai thực tế |
| :--- | :--- | :--- |
| **Performance** | Dashboard tải `< 2s`; Athena truy vấn `< 10s`. | Lưu định dạng Parquet nén Snappy, phân vùng S3 Hive, CDN Amplify. |
| **Scalability** | Co giãn tự động hỗ trợ cào đồng thời `500+` mã cổ phiếu. | AWS Lambda Concurrency, AWS Glue Spark DPU auto-scaling. |
| **Availability** | Tỷ lệ pipeline chạy thành công hàng tháng `≥ 95%`. | EventBridge Scheduler, cơ chế tự động thử lại (retry), CloudWatch Alarm. |
| **Security** | Quyền tối thiểu IAM Least-Privilege, mã hóa S3 at-rest. | IAM Role/Policy chuẩn hóa, SSE-S3/KMS, AWS WAF bảo vệ API. |
| **Data Quality** | Không phát sinh dữ liệu hỏng; có script kiểm tra tự động. | Validate schema PySpark trong Glue, script đếm file trên S3. |
| **Cost Efficiency** | Chi phí vận hành nằm trong ngân sách thử nghiệm (`< $25/tháng`). | Cấu hình AWS Budgets cảnh báo 50%/80%/100%, Athena Workgroup Limit. |
| **Maintainability** | 100% Hạ tầng được định nghĩa bằng Code (IaC). | Cấu trúc repo Terraform phân theo module (`modules/s3`, `modules/glue`...). |
| **Observability** | Log tập trung có quy định thời gian lưu trữ (retention). | CloudWatch Log Groups (Dev lưu 14 ngày, Demo lưu 30 ngày). |
