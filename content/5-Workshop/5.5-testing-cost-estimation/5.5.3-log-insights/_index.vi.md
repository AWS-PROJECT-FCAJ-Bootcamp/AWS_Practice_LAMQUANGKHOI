---
title: "Log và Log Insights"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.5.3. </b> "
---

# 5.5.3. Quản lý Nhật ký với CloudWatch Log Insights

Toàn bộ log thực thi của Lambda Functions (`/aws/lambda/financial-data-collector`) và Glue Jobs (`/aws-glue/jobs/output`) được ghi lại tự động trên **CloudWatch Log Groups**.

---

### Mẫu câu lệnh truy vấn CloudWatch Log Insights

1. **Lọc tất cả các lỗi xảy ra trong Lambda Collector**:
   ```sql
   fields @timestamp, @message
   | filter @message like /Error/ or @message like /Exception/
   | sort @timestamp desc
   | limit 20
   ```

2. **Thống kê thời gian thực thi trung bình và tối đa của Lambda**:
   ```sql
   stats avg(@duration), max(@duration), min(@duration) by bin(5m)
   ```
