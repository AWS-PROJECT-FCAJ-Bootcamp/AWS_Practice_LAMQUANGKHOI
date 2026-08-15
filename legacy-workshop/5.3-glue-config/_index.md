---
title: "Data Processing & Data Lake (AWS Glue ETL & Athena Query)"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

# 5.3. DATA PROCESSING & DATA LAKE (AWS GLUE ETL & ATHENA QUERY)

In this module, we construct the **Process Layer & Query Layer** using **AWS Glue ETL (PySpark)**, **AWS Glue Data Catalog**, **AWS Glue Crawler**, and **Amazon Athena** to transform raw S3 JSON objects into curated Snappy-compressed Parquet datasets partitioned by ticker symbol.

---

### PART 3: PROCESS LAYER (AWS ETL GLUE JOB)

#### 3.1 Configure IAM Role for AWS Glue

* **Description**: Grants IAM Role `AWSGlueETLProcessorRole-dev` necessary permissions to read S3 Raw Bucket, write to S3 Curated Bucket, and trigger Glue Crawlers.
* **Detailed AWS Console Steps**:
  1. Access **IAM Console** ➔ **Roles** ➔ Click **Create role**.
  2. **Select trusted entity**: Select **AWS service** ➔ Select **Glue** ➔ Click **Next**.
  3. **Attach Policies**:
     * Search and select: `AWSGlueServiceRole`.
     * Click **Create policy** (Inline Policy): Paste JSON granting S3 Raw & Curated Read/Write access and `glue:StartCrawler`.
  4. **Role name**: `AWSGlueETLProcessorRole-dev` ➔ Click **Create role**.

![(Figure 3.1) IAM Role Permissions Management Interface for AWS Glue ETL Job](/images/workshop/image9.png)

---

#### 3.2 Configure AWS Glue ETL Job (PySpark)

* **Description**: AWS Glue PySpark Job `ohlcv-glue-processor` executes on a Glue 4.0 Spark cluster. The job reads JSON files, cleans data, computes indicators (MA20, RSI14, return_pct), and writes partitioned Parquet files.
* **Detailed AWS Console Steps**:
  1. Navigate to **AWS Glue Console** ➔ **ETL Jobs** ➔ Select **Script editor (Engine: Spark PySpark)** ➔ Click **Create**.
  2. Switch to **Job details** tab:
     * **Name**: `ohlcv-glue-processor`
     * **IAM Role**: Select `AWSGlueETLProcessorRole-dev`
     * **Type**: Spark | **Glue version**: Glue 4.0
     * **Worker type**: `G.1X` | **Requested number of workers**: `2`
  3. **Job parameters** section (Add environment key-value pairs):
     * `--RAW_DATA_BUCKET`: `my-data-lake-raw-699061130094-dev`
     * `--RAW_KEY`: `ohlcv/ohlcv/year=2026/month=08/day=12/batch_sample.json`
     * `--CURATED_DATA_BUCKET`: `my-data-lake-curated-699061130094-dev`
     * `--CURATED_S3_PREFIX`: `ohlcv/ohlcv`
     * `--GLUE_CRAWLER_NAME`: `ohlcv-crawler`
     * `--AWS_DEFAULT_REGION`: `ap-southeast-1`
  4. On the **Script editor** tab, paste the PySpark script `glue_ohlcv_processor.py` ➔ Click **Save** and **Run**.

![(Figure 3.2) AWS Glue Script Editor pasting PySpark script](/images/workshop/image10.png)

![(Figure 3.3 - Fig 3.2 in doc) Job Details & Static/Dynamic Job Parameters List](/images/workshop/image11.png)

![(Figure 3.4 - Fig 3.3 in doc) Execution Runs History displaying Succeeded status](/images/workshop/image12.png)

* **Complete PySpark Script (`glue_ohlcv_processor.py`)**:

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

# 1. Read raw JSON objects from S3 Raw Bucket
df_raw = spark.read.option("multiline", "true").json(raw_path)

# 2. Compute financial ratios and indicators (MA20, RSI14, return_pct)
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

# 3. Write output to S3 Curated Bucket partitioned by ticker
df_curated.write \
    .mode("overwrite") \
    .partitionBy("ticker") \
    .parquet(output_path)

print(f"Glue Job execution successful. Parquet files written to {output_path}")
```

---

### PART 4: CATALOG & QUERY LAYER (AWS GLUE CRAWLER & ATHENA)

#### 4.1 Configure AWS Glue Crawler

* **Description**: Glue Crawler `ohlcv-crawler` automatically scans Curated S3 Parquet paths to infer 10-column table schemas and register new Hive partitions in the Glue Data Catalog.
* **Detailed AWS Console Steps**:
  1. Access **AWS Glue Console** ➔ **Crawlers** ➔ Click **Create crawler**.
  2. **Name**: `ohlcv-crawler` ➔ Click **Next**.
  3. **Choose data stores**: Add S3 data store ➔ **Path**: `s3://my-data-lake-curated-699061130094-dev/ohlcv/ohlcv/`.
  4. **Set output and scheduling**:
     * **Target database**: Choose `financial_data_lake`
     * **Schema update behavior**: Choose *"Update the table definition in the data catalog"* and *"Ignore the change and don't update key in the data catalog"*
     * **Schedule**: `On demand` ➔ Click **Create crawler**.

![(Figure 3.5) S3 Data Store Configuration Page for Glue Crawler](/images/workshop/image13.png)

![(Figure 3.6 - Fig 4.1 in doc) Schema Update Rules & Crawler Configuration](/images/workshop/image14.png)

---

#### 4.2 Query & Verification on Amazon Athena

* **Description**: Executes serverless SQL queries against `financial_data_lake.ohlcv` using Amazon Athena without managing database servers.
* **AWS Console Steps**:
  1. Open **Athena Console** ➔ **Editor**.
  2. Select **Database**: `financial_data_lake`.
  3. Execute SQL query:

```sql
SELECT * 
FROM financial_data_lake.ohlcv 
WHERE ticker = 'FPT' 
ORDER BY trading_date DESC 
LIMIT 5;
```

![(Figure 3.7) Amazon Athena Query Editor interface selecting financial_data_lake database](/images/workshop/image15.png)

![(Figure 3.8 - Fig 4.2 in doc) SQL Query Results displaying ticker, close_price, ma20, rsi_14 columns](/images/workshop/image16.png)