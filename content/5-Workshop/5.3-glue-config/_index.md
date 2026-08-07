---
title: "Data Processing & Data Lake (AWS Glue ETL & Athena Query)"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

# 5.3. DATA PROCESSING & DATA LAKE (AWS GLUE ETL & ATHENA QUERY)

In this module, our team builds the **Process & Query Layer** to transform raw JSON payloads into a normalized, feature-engineered Curated Parquet Dataset for fast Serverless SQL queries and downstream Machine Learning pipelines.

---

### Step 1: Create Amazon S3 Curated Bucket
1. Navigate to **Amazon S3** ➔ **Create bucket**.
2. Bucket Name: `s3-vietnam-financial-curated-data-prod`.
3. AWS Region: `ap-southeast-1`.
4. Target Prefix: `curated/parquet/financial_features/`.

---

### Step 2: Write AWS Glue PySpark ETL Job

PySpark ETL script running on AWS Glue to perform feature engineering, Winsorization outlier handling, and **Altman Z-Score** calculation:

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

# 1. Read raw JSON payloads from S3 Raw Bucket
df_raw = spark.read.option("multiline", "true").json(RAW_PATH)

# 2. Map line-items and compute financial ratios (Features)
df_features = df_raw.select(
    col("symbol"),
    col("year"),
    # Current Ratio (CR) = Current Assets / Short-term Debt
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

# 3. Calculate Altman Z-Score (Emerging Market Variant)
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

# 4. Write output in Parquet format partitioned by year into S3 Curated Bucket
df_zscore.write \
    .mode("overwrite") \
    .partitionBy("year") \
    .parquet(OUTPUT_PATH)

print("AWS Glue Job successfully processed financial features and saved Parquet files!")
```

![AWS Step Functions State Machine Orchestrating AWS Glue ETL Job](/images/StepFunction.png)

*AWS Step Functions State Machine executing AWS Glue Job (`StartJobRun`), monitoring job completion (`GetJobRun`), and triggering Glue Crawlers.*

---

### Step 3: Configure AWS Glue Crawler & Data Catalog
1. Open **AWS Glue Console** ➔ **Crawlers** ➔ Click **Add crawler**.
2. Crawler Name: `crawler-vietnam-financial-curated`.
3. S3 Data Path: `s3://s3-vietnam-financial-curated-data-prod/curated/parquet/`.
4. Target Database: `vietnam_financial_db`.
5. Run Crawler. Upon completion, the `financial_features` table schema will be populated in the **AWS Glue Data Catalog**.

---

### Step 4: Serverless SQL Querying via Amazon Athena
Open **Amazon Athena**, select database `vietnam_financial_db`, and execute SQL queries to retrieve distress-risk listed companies:

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