---
title: "PROCESS LAYER (AWS ETL GLUE JOB)"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.4.5. </b> "
---

# 5.4.5. PROCESS LAYER — AWS ETL GLUE JOB (PYSPARK)

Tầng xử lý dữ liệu trung tâm **Process Layer** sử dụng dịch vụ **AWS Glue ETL (Engine Spark 4.0)** để trích xuất dữ liệu JSON thô từ S3 Raw, làm sạch, tính toán bộ chỉ số tài chính (MA20, RSI14, return_pct) và ghi file Parquet nén Snappy phân vùng theo Ticker lên S3 Curated Bucket.

---

### 1. Mã nguồn PySpark hoàn chỉnh (`glue_ohlcv_processor.py`)

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
df_curated.write     .mode("overwrite")     .partitionBy("ticker")     .parquet(output_path)

print(f"Hoàn tất Glue Job xử lý dữ liệu và ghi Parquet tới {output_path}!")
```

![(Hình 5.4.5.1) Giao diện Script Editor dán mã nguồn PySpark trên AWS Glue Console](/images/workshop/image10.png)

![(Hình 5.4.5.2) Danh sách tham số cấu hình tĩnh/động (Job Parameters) của AWS Glue ETL Job](/images/workshop/image11.png)

![(Hình 5.4.5.3) Lịch sử thực thi Glue ETL Job báo trạng thái Succeeded thành công](/images/workshop/image12.png)
