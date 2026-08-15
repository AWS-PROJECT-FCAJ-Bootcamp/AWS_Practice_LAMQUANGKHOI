---
title: "Project Sample Datasets"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

# 5.4.2. Project Sample Datasets & Schema Contracts

To support automated pipeline testing, the system standardizes input JSON schemas containing Open, High, Low, Close (OHLCV) prices and daily trading volume.

---

### 1. Sample Raw JSON Payload Contract

```json
{
  "symbol": "FPT",
  "provider": "YFINANCE_API",
  "collected_at": "2026-08-15T10:00:00.000Z",
  "records": [
    {
      "symbol": "FPT",
      "trading_date": "2026-08-14",
      "open_price": 128500.0,
      "high_price": 130000.0,
      "low_price": 127800.0,
      "close_price": 129500.0,
      "volume": 3450000
    }
  ]
}
```

- Raw JSON payloads are uploaded directly to the S3 Raw Bucket path partitioned by date `ohlcv/ohlcv/year=YYYY/month=MM/day=DD/batch_FPT_<timestamp>.json`.

![(Figure 5.4.2.1) Raw JSON Partitioned Folder Structure on Amazon S3 Raw Bucket](/images/workshop/image3.png)
