---
title: "PROCESS LAYER (AWS ETL GLUE JOB)"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.4.5. </b> "
---

# 5.4.5. PROCESS LAYER — AWS ETL GLUE JOB (PYSPARK)

The core data processing layer **Process Layer** utilizes **AWS Glue ETL (Spark 4.0 Engine)** to ingest raw JSON payloads from S3 Raw, perform data cleansing, compute technical indicators (MA20, RSI14, return_pct), and write Snappy Parquet files partitioned by ticker to S3 Curated Bucket.

---

### 1. PySpark ETL Script (`glue_ohlcv_processor.py`)

```python
import sys
from awsglue.transforms import *
from awsglue.utils import getResolvedOptions
from pyspark.context import SparkContext
from awsglue.context import GlueContext
from pyspark.sql.functions import col, avg
from pyspark.sql.window import Window

args = getResolvedOptions(sys.argv, [
    'JOB_NAME',
    'RAW_DATA_BUCKET',
    'CURATED_DATA_BUCKET',
    'CURATED_S3_PREFIX'
])

sc = SparkContext()
glueContext = GlueContext(sc)
spark = glueContext.spark_session

raw_bucket = args['RAW_DATA_BUCKET']
curated_bucket = args['CURATED_DATA_BUCKET']
curated_prefix = args['CURATED_S3_PREFIX']

raw_path = f"s3://{raw_bucket}/*.json"
output_path = f"s3://{curated_bucket}/{curated_prefix}/"

df_raw = spark.read.option("multiline", "true").json(raw_path)

window_spec_20 = Window.partitionBy("symbol").orderBy("trading_date").rowsBetween(-19, 0)

df_curated = df_raw.select(
    col("symbol").alias("ticker"),
    col("trading_date"),
    col("open_price"),
    col("high_price"),
    col("low_price"),
    col("close_price"),
    col("volume"),
    avg(col("close_price")).over(window_spec_20).alias("ma20")
)

df_curated.write     .mode("overwrite")     .partitionBy("ticker")     .parquet(output_path)

print(f"ETL Job finished writing Parquet outputs to {output_path}!")
```

![(Figure 5.4.5.1) AWS Glue Script Editor pasting PySpark script](/images/workshop/image10.png)

![(Figure 5.4.5.2) Static and Dynamic Job Parameters Configuration](/images/workshop/image11.png)

![(Figure 5.4.5.3) Execution Runs History displaying Succeeded status](/images/workshop/image12.png)
