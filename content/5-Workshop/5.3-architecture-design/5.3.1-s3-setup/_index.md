---
title: "S3 Initialization and Configuration"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

# 5.3.1. Amazon S3 Storage Layer Initialization and Configuration

In the **Financial Data Platform on AWS**, Amazon S3 serves as the central Data Lake Storage Layer, partitioned into 02 distinct S3 Buckets representing the `Raw` and `Curated` zones.

---

### 1. Provisioning S3 Raw Bucket (`my-data-lake-raw-<ACCOUNT_ID>`)

- **Purpose**: Stores raw JSON market payloads collected daily from market APIs (`yfinance`/`VNStock`). Raw data adheres to *Immutability* principles (never overwritten or deleted) to enable ETL pipeline replay.
- **Console Steps**:
  1. Navigate to AWS S3 Console in Region `<REGION>` (e.g., `ap-southeast-1`).
  2. Click **Create bucket**.
  3. Enter **Bucket name**: `my-data-lake-raw-<ACCOUNT_ID>` (replace `<ACCOUNT_ID>` with your AWS Account ID to ensure global uniqueness).
  4. Keep default settings (Block all public access = ON, SSE-S3 Encryption).
  5. Click **Create bucket**.

![(Figure 5.3.1.1) Amazon S3 Bucket Creation Interface on AWS Console](/images/workshop/image1.png)

![(Figure 5.3.1.2) General Configuration for S3 Raw Bucket](/images/workshop/image2.png)

- **Raw Partition Structure**:
  - Path: `ohlcv/ohlcv/year=YYYY/month=MM/day=DD/batch_<timestamp>.json`

![(Figure 5.3.1.3) Raw JSON Partitioned Folder Structure on Amazon S3 Raw Bucket](/images/workshop/image3.png)

---

### 2. Provisioning S3 Curated Bucket (`my-data-lake-curated-<ACCOUNT_ID>`)

- **Purpose**: Stores normalized, high-performance **Snappy-compressed Parquet** datasets, Hive-partitioned by stock ticker (`ticker=XXX/`) for high-speed Amazon Athena queries.
- **Console Steps**:
  1. On S3 Buckets page, click **Create bucket**.
  2. Enter **Bucket name**: `my-data-lake-curated-<ACCOUNT_ID>`.
  3. Click **Create bucket**.

![(Figure 5.3.1.4) S3 Curated Bucket Management Console](/images/workshop/image4.png)

- **Curated Parquet Folder Structure**:
  - Path: `ohlcv/ohlcv/ticker=ACB/part-000.parquet`, `ohlcv/ohlcv/ticker=FPT/part-000.parquet`

![(Figure 5.3.1.5) Curated Folder ohlcv/ohlcv/ displaying ticker=ACB/, ticker=FPT/ partitions](/images/workshop/image5.png)

---

### 3. S3 Lifecycle Policy Configuration (Optional)

- **Purpose**: Automatically transition raw JSON objects older than 30 days to S3 Standard-IA or Glacier to optimize storage costs.
- **Console Steps**: S3 Raw Bucket -> *Management* tab -> *Create lifecycle rule*.

*(Screenshot of S3 Lifecycle Policy Configuration: To be updated)*
