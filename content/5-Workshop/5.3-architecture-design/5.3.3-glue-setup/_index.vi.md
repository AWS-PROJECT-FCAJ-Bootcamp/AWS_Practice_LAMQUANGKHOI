---
title: "Khởi tạo và cấu hình Glue"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---

# 5.3.3. Khởi tạo và cấu hình AWS Glue ETL Job, Crawler & Athena Query

Dịch vụ **AWS Glue** chịu trách nhiệm xử lý làm sạch dữ liệu, tính toán bộ chỉ số tài chính (MA20, RSI14, return_pct) và tự động cập nhật Data Catalog cho Amazon Athena truy vấn.

---

### 1. Cấu hình AWS Glue ETL Job PySpark (`ohlcv-glue-processor`)

- **Mô tả**: Job PySpark chạy trên cụm Spark Serverless (Glue 4.0, 2 Workers G.1X) làm sạch dữ liệu thô JSON và xuất định dạng Parquet nén Snappy.
- **Thao tác trên AWS Console**:
  1. Vào AWS Glue Console -> **ETL Jobs** -> Chọn **Script editor (Spark PySpark)** -> Click **Create**.
  2. Chuyển sang tab **Job details**:
     - Name: `ohlcv-glue-processor`
     - IAM Role: Chọn `AWSGlueETLProcessorRole`
     - Type: `Spark` | Glue Version: `Glue 4.0`
     - Worker type: `G.1X` | Number of workers: `2`
  3. Thêm các tham số tĩnh/động tại mục **Job parameters**:
     - `--RAW_DATA_BUCKET`: `my-data-lake-raw-<ACCOUNT_ID>`
     - `--RAW_KEY`: `ohlcv/ohlcv/year=YYYY/month=MM/day=DD/batch_sample.json`
     - `--CURATED_DATA_BUCKET`: `my-data-lake-curated-<ACCOUNT_ID>`
     - `--CURATED_S3_PREFIX`: `ohlcv/ohlcv`
     - `--GLUE_CRAWLER_NAME`: `ohlcv-crawler`
     - `--AWS_DEFAULT_REGION`: `<REGION>`
  4. Dán mã PySpark `glue_ohlcv_processor.py` vào Script Editor -> Click **Save** và **Run**.

![(Hình 5.3.3.1) Giao diện Script Editor dán mã nguồn PySpark trên AWS Glue Console](/images/workshop/image10.png)

![(Hình 5.3.3.2) Chi tiết các tham số Job Parameters của AWS Glue ETL Job](/images/workshop/image11.png)

![(Hình 5.3.3.3) Lịch sử thực thi Glue ETL Job báo trạng thái Succeeded thành công](/images/workshop/image12.png)

---

### 2. Cấu hình AWS Glue Crawler (`ohlcv-crawler`)

- **Mô tả**: Trình Crawler quét S3 Curated Bucket và tự động tạo/cập nhật Bảng `ohlcv` trong Database `financial_data_lake`.
- **Thao tác trên AWS Console**:
  1. AWS Glue Console -> **Crawlers** -> Click **Create crawler**.
  2. Name: `ohlcv-crawler`.
  3. Add data store: Chọn S3 path `s3://my-data-lake-curated-<ACCOUNT_ID>/ohlcv/ohlcv/`.
  4. Choose IAM Role: Chọn `AWSGlueETLProcessorRole`.
  5. Target database: Chọn database `financial_data_lake` -> Click **Create crawler** và **Run crawler**.

![(Hình 5.3.3.4) Màn hình thêm S3 Data Store cho Glue Crawler](/images/workshop/image13.png)

![(Hình 5.3.3.5) Cấu hình quy tắc cập nhật Schema và xử lý dữ liệu của AWS Glue Crawler](/images/workshop/image14.png)

---

### 3. Truy vấn dữ liệu phân tích trên Amazon Athena Editor

- **Mô tả**: Chạy các câu lệnh SQL truy vấn trực tiếp trên bảng Parquet trong Data Catalog.
- **Thao tác trên AWS Console**:
  1. Mở **Amazon Athena Console** -> Query Editor.
  2. Chọn Database: `financial_data_lake`.
  3. Chạy câu lệnh SQL kiểm tra các chỉ số MA20, RSI14:
     ```sql
     SELECT ticker, date, close_price, ma20, rsi_14, return_pct 
     FROM "financial_data_lake"."ohlcv" 
     ORDER BY date DESC LIMIT 100;
     ```

![(Hình 5.3.3.6) Khung chạy câu SQL và chọn database financial_data_lake trên Athena Editor](/images/workshop/image15.png)

![(Hình 5.3.3.7) Kết quả truy vấn SQL hiển thị các cột ticker, close_price, ma20, rsi_14 trên Amazon Athena Editor](/images/workshop/image16.png)
