---
title: "End-to-End Testing Matrix"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.5.4. </b> "
---

# 5.5.4. End-to-End System Testing & Verification Matrix

End-to-end system verification tests validate the complete data pipeline lifecycle from initial API fetch down to Web Dashboard chart rendering.

---

### System Testing Verification Matrix

| Test Case ID | Test Scenario | Expected Result | Status |
| :--- | :--- | :--- | :--- |
| **TC-01** | Trigger Lambda Collector for ticker `FPT` | Raw JSON payload written to S3 Raw Bucket | **PASSED** |
| **TC-02** | Execute Glue ETL Job `ohlcv-glue-processor` | Snappy Parquet file exported under `ticker=FPT/` | **PASSED** |
| **TC-03** | Run Glue Crawler `ohlcv-crawler` | Updates table `ohlcv` in Glue Data Catalog | **PASSED** |
| **TC-04** | Athena SQL query execution | Returns result set with `ma20`, `rsi_14` in < 2s | **PASSED** |
| **TC-05** | Cognito login on Web Dashboard | Issues valid JWT token and unlocks dashboard | **PASSED** |
| **TC-06** | Dispatch SES email notification | Styled HTML email delivered to inbox | **PASSED** |
