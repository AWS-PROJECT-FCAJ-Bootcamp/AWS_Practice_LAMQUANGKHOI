---
title: "Stream Testing from Data Source to S3 Curated"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 5.4.7. </b> "
---

# 5.4.7. End-to-End Stream Testing from Data Source to S3 Curated

To verify pipeline integrity, an end-to-end verification test is performed from initial API ingestion down to S3 Curated Parquet partitioning.

---

### Verification Flow Steps

1. Manually trigger Lambda Collector `financial-data-collector` with payload `{"symbol": "FPT"}`.
2. Verify CloudWatch Logs confirming raw JSON write to `s3://my-data-lake-raw-<ACCOUNT_ID>/ohlcv/ohlcv/year=2026/...`.
3. Run Glue ETL Job `ohlcv-glue-processor` and Glue Crawler `ohlcv-crawler`.
4. Verify S3 Curated Bucket displaying the new Hive partition `ticker=FPT/` containing Parquet files.

![(Figure 5.4.7.1) Curated Folder ohlcv/ohlcv/ displaying ticker=FPT/ Parquet Partitions](/images/workshop/image5.png)
