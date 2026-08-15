---
title: "Daily Scheduling with Amazon EventBridge"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4.4. </b> "
---

# 5.4.4. Automated Daily Pipeline Scheduling with EventBridge

To automate the daily ingestion pipeline after the Vietnamese stock market closes, the platform configures an **Amazon EventBridge Scheduler** triggering the Lambda Collector function daily at **16:00 VN (10:00 UTC)**, Monday through Friday.

---

### Console Setup Steps

1. Open **Amazon EventBridge Console** -> Select **Schedules** -> Click **Create schedule**.
2. **Schedule name**: `daily-financial-pipeline-schedule`.
3. **Schedule pattern**:
   - Schedule type: `Recurring schedule` (Cron-based).
   - Cron expression: `cron(0 10 ? * MON-FRI *)` (10:00 UTC = 16:00 ICT).
   - Timezone: `Asia/Ho_Chi_Minh`.
4. **Target detail**: Select target -> **AWS Lambda** -> Select `financial-data-collector`.
5. **Permissions**: Attach IAM Role `EventBridgeSchedulerRole`.

![(Figure 5.4.4.1) Amazon EventBridge Scheduler Cron Expression Configuration](/images/workshop/image21.png)

![(Figure 5.4.4.2) Active EventBridge Schedule Details Screen (Enabled State)](/images/workshop/image22.png)
