---
title: "Uploading Sample Data to S3 Storage"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.4.3. </b> "
---

# 5.4.3. Uploading Initial Sample Data to S3 Storage

Seeding initial raw data files into the S3 Raw Bucket allows testing of the Spark ETL transformation engine and schema discovery mechanisms.

---

### Step-by-step Console Upload Procedure

1. Open **Amazon S3 Console** -> Navigate to `my-data-lake-raw-<ACCOUNT_ID>`.
2. Browse to folder path: `ohlcv/ohlcv/year=2026/month=08/day=15/`.
3. Click **Upload** -> Select local file `batch_sample.json`.
4. Click **Upload** to finalize object storage.

![(Figure 5.4.3.1) S3 Raw Bucket Upload & Configuration Console](/images/workshop/image2.png)
