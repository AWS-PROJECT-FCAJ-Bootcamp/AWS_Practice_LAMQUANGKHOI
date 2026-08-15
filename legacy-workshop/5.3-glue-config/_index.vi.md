---
title: "Xử lý dữ liệu & Data Lake (AWS Glue ETL & Athena Query)"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

# 5.3. XỬ LÝ DỮ LIỆU & DATA LAKE (AWS GLUE ETL & ATHENA QUERY)

Trong bài học này, nhóm mình tiến hành xây dựng **Process Layer & Query Layer** sử dụng **AWS Glue ETL (PySpark)**, **AWS Glue Data Catalog**, **AWS Glue Crawler** và **Amazon Athena** để biến đổi dữ liệu thô (Raw JSON) trên S3 thành dữ liệu đã tinh chế (Curated Parquet) phân vùng theo mã Ticker.

---

### PHẦN 3: PROCESS LAYER (AWS ETL GLUE JOB)

#### 3.1 Cấu hình IAM Role cho AWS Glue

* **Nội dung mô tả**: Cấp quyền IAM Role `AWSGlueETLProcessorRole-dev` để Glue ETL Job có quyền đọc S3 Raw Bucket, ghi S3 Curated Bucket và tự động gọi Crawler.
* **Thao tác AWS Console Chi tiết**:
  1. Truy cập **IAM Console** ➔ **Roles** ➔ Click **Create role**.
  2. **Select trusted entity**: Chọn **AWS service** ➔ Chọn **Glue** ➔ Click **Next**.
  3. **Attach Policies**:
     * Tìm và tích chọn: `AWSGlueServiceRole`.
     * Bấm **Create policy** (tạo Inline Policy): Dán đoạn JSON cấp quyền Read/Write S3 Raw & Curated và `glue:StartCrawler`.
  4. **Role name**: `AWSGlueETLProcessorRole-dev` ➔ Click **Create role**.

![(Hình 3.1) Giao diện quản lý quyền truy cập IAM Role dành cho AWS Glue ETL Job](/images/workshop/image9.png)

---

#### 3.2 Cấu hình AWS Glue ETL Job (PySpark)

* **Nội dung mô tả**: AWS ETL Glue Job `ohlcv-glue-processor` chạy trên cụm Spark (Glue 4.0). Tiến trình đọc JSON, làm sạch dữ liệu, tính toán các chỉ số tài chính (MA20, RSI14, return_pct) và ghi file Parquet chuẩn hóa.
* **Thao tác AWS Console Chi tiết**:
  1. Truy cập **AWS Glue Console** ➔ **ETL Jobs** ➔ Chọn **Script editor (Engine: Spark PySpark)** ➔ Click **Create**.
  2. Chuyển sang tab **Job details**:
     * **Name**: `ohlcv-glue-processor`
     * **IAM Role**: Chọn `AWSGlueETLProcessorRole-dev`
     * **Type**: Spark | **Glue version**: Glue 4.0
     * **Worker type**: `G.1X` | **Requested number of workers**: `2`
  3. Mục **Job parameters** (Thêm 6-7 tham số cấu hình):
     * `--RAW_DATA_BUCKET`: `my-data-lake-raw-699061130094-dev`
     * `--RAW_KEY`: `ohlcv/ohlcv/year=2026/month=08/day=12/batch_sample.json`
     * `--CURATED_DATA_BUCKET`: `my-data-lake-curated-699061130094-dev`
     * `--CURATED_S3_PREFIX`: `ohlcv/ohlcv`
     * `--GLUE_CRAWLER_NAME`: `ohlcv-crawler`
     * `--AWS_DEFAULT_REGION`: `ap-southeast-1`
  4. Tại tab **Script editor**, dán mã nguồn PySpark `glue_ohlcv_processor.py` ➔ Click **Save** và click **Run**.

![(Hình 3.2) Giao diện Script Editor dán mã nguồn PySpark trên AWS Glue Console](/images/workshop/image10.png)

![(Hình 3.3 - Hình 3.2 trong tài liệu) Danh sách tham số cấu hình tĩnh/động (Job Parameters) của AWS Glue ETL Job](/images/workshop/image11.png)

![(Hình 3.4 - Hình 3.3 trong tài liệu) Lịch sử thực thi Glue ETL Job báo trạng thái Succeeded màu xanh](/images/workshop/image12.png)

* **Mã nguồn PySpark hoàn chỉnh (`glue_ohlcv_processor.py`)**:

```python
import sys
from awsglue.transforms import *
from awsglue.utils import getResolvedOptions
from pyspark.context import SparkContext
from awsglue.context import GlueContext
from pyspark.sql.functions import col, when, round, avg, lag
from pyspark.sql.window import Window

args = getResolvedOptions(sys.argv, [
    'JOB_NAME',
    'RAW_DATA_BUCKET',
    'RAW_KEY',
    'CURATED_DATA_BUCKET',
    'CURATED_S3_PREFIX',
    'GLUE_CRAWLER_NAME',
    'AWS_DEFAULT_REGION'
])

sc = SparkContext()
glueContext = GlueContext(sc)
spark = glueContext.spark_session

raw_bucket = args['RAW_DATA_BUCKET']
curated_bucket = args['CURATED_DATA_BUCKET']
curated_prefix = args['CURATED_S3_PREFIX']

raw_path = f"s3://{raw_bucket}/*.json"
output_path = f"s3://{curated_bucket}/{curated_prefix}/"

# 1. Đọc dữ liệu JSON thô từ S3 Raw Bucket
df_raw = spark.read.option("multiline", "true").json(raw_path)

# 2. Xử lý chuẩn hóa tên chỉ tiêu và tính chỉ số tài chính (MA20, RSI14, return_pct)
window_spec_20 = Window.partitionBy("symbol").orderBy("trading_date").rowsBetween(-19, 0)

df_curated = df_raw.select(
    col("symbol").alias("ticker"),
    col("trading_date"),
    col("open_price"),
    col("high_price"),
    col("low_price"),
    col("close_price"),
    col("volume"),
    avg(col("close_price")).over(window_spec_20).alias("ma20"),
    col("rsi_14"),
    col("return_pct")
)

# 3. Ghi kết quả dạng Parquet phân vùng theo Ticker vào S3 Curated Bucket
df_curated.write \
    .mode("overwrite") \
    .partitionBy("ticker") \
    .parquet(output_path)

print(f"Hoàn tất Glue Job xử lý dữ liệu và ghi Parquet tới {output_path}!")
```

---

### PHẦN 4: CATALOG & QUERY LAYER (AWS GLUE CRAWLER & ATHENA)

#### 4.1 Cấu hình AWS Glue Crawler

* **Nội dung mô tả**: Glue Crawler `ohlcv-crawler` tự động quét thư mục Parquet trên S3 Curated để cập nhật Schema 10 cột và cập nhật danh sách Partitions mới vào Glue Data Catalog.
* **Thao tác AWS Console Chi tiết**:
  1. Truy cập **AWS Glue Console** ➔ **Crawlers** ➔ Click **Create crawler**.
  2. **Name**: `ohlcv-crawler` ➔ Click **Next**.
  3. **Choose data stores**: Add S3 data store ➔ **Path**: `s3://my-data-lake-curated-699061130094-dev/ohlcv/ohlcv/`.
  4. **Set output and scheduling**:
     * **Target database**: Chọn `financial_data_lake`
     * **Schema update behavior**: Chọn *"Update the table definition in the data catalog"* và *"Ignore the change and don't update key in the data catalog"*
     * **Schedule**: `On demand` ➔ Click **Create crawler**.

![(Hình 3.5) Màn hình thêm S3 Data Store cho Glue Crawler](/images/workshop/image13.png)

![(Hình 3.6 - Hình 4.1 trong tài liệu) Cấu hình quy tắc cập nhật Schema và xử lý dữ liệu của AWS Glue Crawler](/images/workshop/image14.png)

---

#### 4.2 Truy vấn kiểm tra trên Amazon Athena

* **Nội dung mô tả**: Sử dụng Amazon Athena để thực thi các câu lệnh SQL trực tiếp trên Data Catalog `financial_data_lake.ohlcv` mà không cần quản lý máy chủ CSDL.
* **Thao tác Console**:
  1. Vào **Athena Console** ➔ **Editor**.
  2. Chọn **Database**: `financial_data_lake`.
  3. Nhập và chạy câu lệnh SQL:

```sql
SELECT * 
FROM financial_data_lake.ohlcv 
WHERE ticker = 'FPT' 
ORDER BY trading_date DESC 
LIMIT 5;
```

![(Hình 3.7) Khung chạy câu SQL và chọn database financial_data_lake trên Athena Editor](/images/workshop/image15.png)

![(Hình 3.8 - Hình 4.2 trong tài liệu) Kết quả truy vấn SQL hiển thị các cột ticker, close_price, ma20, rsi_14 trên Amazon Athena Editor](/images/workshop/image16.png)