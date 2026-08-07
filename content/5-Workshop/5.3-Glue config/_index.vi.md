---
title: "Xử lý dữ liệu & Data Lake (AWS Glue ETL & Athena Query)"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

# 5.3. XỬ LÝ DỮ LIỆU & DATA LAKE (AWS GLUE ETL & ATHENA QUERY)

Trong module này, nhóm mình thực hiện xây dựng **ETL Process & Query Layer** để biến đổi dữ liệu thô (Raw JSON) thành tập chỉ số tài chính chuẩn hóa (Curated Parquet Dataset), phục vụ truy vấn SQL tốc độ cao và làm dữ liệu đầu vào cho các mô hình Machine Learning.

---

### Bước 1: Tạo Amazon S3 Curated Bucket
1. Vào **Amazon S3** ➔ **Create bucket**.
2. Đặt tên Bucket: `s3-vietnam-financial-curated-data-prod`.
3. Chọn Region: `ap-southeast-1`.
4. Tạo thư mục lưu trữ: `curated/parquet/financial_features/`.

---

### Bước 2: Viết AWS Glue ETL Job (PySpark / Python)

Đoạn mã PySpark trên AWS Glue thực hiện chuẩn hóa chỉ tiêu, xử lý nhiễu Winsorization và tính điểm **Altman Z-Score**:

```python
import sys
from awsglue.transforms import *
from awsglue.utils import getResolvedOptions
from pyspark.context import SparkContext
from awsglue.context import GlueContext
from pyspark.sql.functions import col, when, expr, log

sc = SparkContext()
glueContext = GlueContext(sc)
spark = glueContext.spark_session

RAW_PATH = "s3://s3-vietnam-financial-raw-data-prod/raw/yearly/*.json"
OUTPUT_PATH = "s3://s3-vietnam-financial-curated-data-prod/curated/parquet/financial_features/"

# 1. Đọc dữ liệu JSON thô từ S3 Raw Bucket
df_raw = spark.read.option("multiline", "true").json(RAW_PATH)

# 2. Xử lý chuẩn hóa tên chỉ tiêu và tính chỉ số tài chính (Features)
df_features = df_raw.select(
    col("symbol"),
    col("year"),
    # Current Ratio (CR) = Current Assets / Current Liabilities
    (col("current_assets") / col("short_term_debt")).alias("CR"),
    # ROA = Net Profit / Total Assets
    (col("net_profit") / col("total_assets")).alias("ROA"),
    # ROE = Net Profit / Equity
    (col("net_profit") / col("equity")).alias("ROE"),
    # DAR = Total Debt / Total Assets
    (col("total_debt") / col("total_assets")).alias("DAR"),
    # WCTA = Working Capital / Total Assets
    ((col("current_assets") - col("short_term_debt")) / col("total_assets")).alias("WCTA"),
    # EBIT_TA = EBIT / Total Assets
    (col("ebit") / col("total_assets")).alias("EBIT_TA"),
    # MC_Debt = Market Cap / Total Debt
    (col("market_cap") / col("total_debt")).alias("MC_Debt")
)

# 3. Tính toán điểm số Altman Z-Score (Emerging Market Variant)
# Z = 6.56*WCTA + 3.26*ROA + 6.72*EBIT_TA + 1.05*MC_Debt
df_zscore = df_features.withColumn(
    "Z_Score",
    (6.56 * col("WCTA")) + (3.26 * col("ROA")) + (6.72 * col("EBIT_TA")) + (1.05 * col("MC_Debt"))
).withColumn(
    "distress_zone",
    when(col("Z_Score") <= 1.23, "Distress Zone (High Risk)")
    .when((col("Z_Score") > 1.23) & (col("Z_Score") <= 2.9), "Grey Zone (Medium Risk)")
    .otherwise("Safe Zone (Low Risk)")
)

# 4. Ghi kết quả dạng Parquet phân trang theo Năm vào S3 Curated Bucket
df_zscore.write \
    .mode("overwrite") \
    .partitionBy("year") \
    .parquet(OUTPUT_PATH)

print("Hoàn tất Glue Job xử lý dữ liệu và ghi Parquet!")
```

---

### Bước 3: Tạo AWS Glue Crawler & Data Catalog
1. Vào **AWS Glue Console** ➔ chọn **Crawlers** ➔ Nhấp **Add crawler**.
2. Đặt tên Crawler: `crawler-vietnam-financial-curated`.
3. Chỉ định nguồn dữ liệu S3: `s3://s3-vietnam-financial-curated-data-prod/curated/parquet/`.
4. Tạo mới Glue Database: `vietnam_financial_db`.
5. Chạy Crawler. Sau khi quét xong, bảng `financial_features` sẽ tự động hiển thị trong **AWS Glue Data Catalog**.

---

### Bước 4: Truy vấn SQL Serverless trên Amazon Athena
Mở **Amazon Athena**, chọn database `vietnam_financial_db` và chạy truy vấn thống kê danh sách doanh nghiệp có Z-Score nguy cơ phá sản cao:

```sql
SELECT 
    symbol, 
    year, 
    round(z_score, 2) AS z_score, 
    distress_zone, 
    round(roa * 100, 2) AS roa_percent, 
    round(dar, 2) AS debt_ratio
FROM vietnam_financial_db.financial_features
WHERE z_score <= 1.23
ORDER BY year DESC, z_score ASC;
```