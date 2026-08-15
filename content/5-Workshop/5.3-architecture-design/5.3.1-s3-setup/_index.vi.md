---
title: "Khởi tạo và cấu hình S3"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

# 5.3.1. Khởi tạo và cấu hình Amazon S3 Storage Layer

Trong kiến trúc **Financial Data Platform trên AWS**, dịch vụ Amazon S3 đóng vai trò là tầng lưu trữ trung tâm (Data Lake Storage Layer), được chia làm 02 Bucket riêng biệt đại diện cho 2 phân vùng dữ liệu `Raw` và `Curated`.

---

### 1. Khởi tạo S3 Raw Bucket (`my-data-lake-raw-<ACCOUNT_ID>`)

- **Mục đích**: Nơi lưu trữ dữ liệu JSON thô thu thập hàng ngày từ API chứng khoán (`yfinance`/`VNStock`). Dữ liệu thô tuân thủ nguyên tắc *Immutability* (không bị sửa/xóa) giúp hệ thống có khả năng Re-play ETL khi logic kinh doanh thay đổi.
- **Thao tác trên AWS Management Console**:
  1. Truy cập AWS S3 Console tại Region `<REGION>` (ví dụ: `ap-southeast-1`).
  2. Bấm nút **Create bucket**.
  3. Nhập **Bucket name**: `my-data-lake-raw-<ACCOUNT_ID>` (thay `<ACCOUNT_ID>` bằng AWS Account ID của bạn để đảm bảo tên duy nhất trên toàn cầu).
  4. Giữ nguyên cấu hình mặc định (Block all public access = ON, SSE-S3 Encryption).
  5. Bấm **Create bucket**.

![(Hình 5.3.1.1) Giao diện khởi tạo Amazon S3 Bucket trên AWS Console](/images/workshop/image1.png)

![(Hình 5.3.1.2) Giao diện cấu hình General configuration cho S3 Raw Bucket](/images/workshop/image2.png)

- **Cấu trúc phân vùng dữ liệu thô (Partition Structure)**:
  - Thư mục: `ohlcv/ohlcv/year=YYYY/month=MM/day=DD/batch_<timestamp>.json`

![(Hình 5.3.1.3) Cấu trúc thư mục phân vùng lưu trữ dữ liệu JSON thô trên Amazon S3 Raw Bucket](/images/workshop/image3.png)

---

### 2. Khởi tạo S3 Curated Bucket (`my-data-lake-curated-<ACCOUNT_ID>`)

- **Mục đích**: Lưu trữ dữ liệu tài chính sạch đã qua xử lý chuẩn hóa dưới định dạng **Parquet nén Snappy**, phân vùng theo Ticker (`ticker=XXX/`), tối ưu hiệu năng truy vấn cho Amazon Athena.
- **Thao tác trên AWS Management Console**:
  1. Tại màn hình S3 Buckets, bấm **Create bucket**.
  2. Nhập **Bucket name**: `my-data-lake-curated-<ACCOUNT_ID>`.
  3. Bấm **Create bucket**.

![(Hình 5.3.1.4) Màn hình quản lý S3 Curated Bucket trên AWS Management Console](/images/workshop/image4.png)

- **Cấu trúc dữ liệu tinh chế Parquet (Curated Partitioning)**:
  - Thư mục: `ohlcv/ohlcv/ticker=ACB/part-000.parquet`, `ohlcv/ohlcv/ticker=FPT/part-000.parquet`

![(Hình 5.3.1.5) Cấu trúc thư mục ohlcv/ohlcv/ hiển thị danh sách phân vùng theo Ticker Parquet](/images/workshop/image5.png)

---

### 3. Cấu hình Vòng đời Dữ liệu S3 Lifecycle Policy (Tùy chọn)

- **Mục đích**: Tự động chuyển dữ liệu thô cũ hơn 30 ngày sang S3 Standard-IA hoặc Glacier để tối ưu chi phí lưu trữ.
- **Thao tác trên Console**: Vào tab *Management* của S3 Raw Bucket -> Bấm *Create lifecycle rule*.

*(Hình ảnh minh họa màn hình cấu hình S3 Lifecycle Policy: Sẽ cập nhật)*
