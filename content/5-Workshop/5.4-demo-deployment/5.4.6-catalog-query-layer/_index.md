---
title: "CATALOG & QUERY LAYER (AWS GLUE CRAWLER & ATHENA)"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.4.6. </b> "
---

# 5.4.6. CATALOG & QUERY LAYER — AWS GLUE CRAWLER & ATHENA

The **Catalog & Query Layer** combines **AWS Glue Crawler** for automated Schema discovery and **Amazon Athena** for high-speed ad-hoc SQL querying.

---

### 1. Automated Schema Discovery with AWS Glue Crawler

Glue Crawler `ohlcv-crawler` scans the S3 Curated Bucket, automatically inferring data schemas and cataloging table `ohlcv` inside database `financial_data_lake`.

![(Figure 5.4.6.1) Adding S3 Data Store to AWS Glue Crawler](/images/workshop/image13.png)

![(Figure 5.4.6.2) Schema Update Rules & Crawler Settings](/images/workshop/image14.png)

---

### 2. Analytical Queries on Amazon Athena

Data analysts and Web Dashboards execute ad-hoc SQL queries directly on the Parquet tables:

```sql
SELECT ticker, date, close_price, ma20, rsi_14, return_pct 
FROM "financial_data_lake"."ohlcv" 
ORDER BY date DESC LIMIT 100;
```

![(Figure 5.4.6.3) Amazon Athena Query Editor Interface](/images/workshop/image15.png)

![(Figure 5.4.6.4) Athena SQL Query Output displaying financial indicators](/images/workshop/image16.png)
