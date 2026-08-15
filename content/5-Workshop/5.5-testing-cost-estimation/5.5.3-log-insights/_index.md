---
title: "Log Insights & Debugging"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.5.3. </b> "
---

# 5.5.3. Log Management with CloudWatch Log Insights

All serverless execution logs from Lambda Functions (`/aws/lambda/financial-data-collector`) and Glue Jobs (`/aws-glue/jobs/output`) are continuously streamed to **CloudWatch Log Groups**.

---

### Sample Log Insights Queries

1. **Filter Lambda Collector Errors**:
   ```sql
   fields @timestamp, @message
   | filter @message like /Error/ or @message like /Exception/
   | sort @timestamp desc
   | limit 20
   ```

2. **Calculate Lambda Execution Duration Stats**:
   ```sql
   stats avg(@duration), max(@duration), min(@duration) by bin(5m)
   ```
