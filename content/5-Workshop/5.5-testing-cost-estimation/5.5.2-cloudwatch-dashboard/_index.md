---
title: "CloudWatch Dashboard & Observability"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.5.2. </b> "
---

# 5.5.2. System Observability with CloudWatch Dashboards & Alarms

To ensure system reliability and budget control, **Amazon CloudWatch Dashboards** monitor key operational metrics in real-time.

---

### 1. Key Performance Indicators

- **Lambda Metrics**: Error counts, Duration (ms), Throttles.
- **AWS Glue Metrics**: Job execution elapsed time, S3 write bytes.
- **API Gateway Metrics**: 4XXError, 5XXError, Latency.

---

### 2. Cost Control with AWS Budgets Alarms

Automated email notifications trigger at budget thresholds:
- **50% Budget ($10 USD)**: Informational alert.
- **80% Budget ($16 USD)**: Warning alert.
- **100% Budget ($20 USD)**: Critical budget threshold.
