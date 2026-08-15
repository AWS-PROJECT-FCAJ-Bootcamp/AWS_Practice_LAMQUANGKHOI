---
title: "Dọn dẹp tài nguyên và clean up data"
date: 2024-01-01
weight: 12
chapter: false
pre: " <b> 5.4.12. </b> "
---

# 5.4.12. Quy trình Dọn dẹp Tài nguyên và Clean up Data

Sau khi hoàn tất quá trình kiểm thử và demo workshop, việc xóa tài nguyên là bắt buộc để tránh phát sinh chi phí duy trì tài nguyên trên đám mây AWS.

---

### Danh mục thu hồi tài nguyên (Teardown Checklist)

1. **Amazon S3**: Xóa toàn bộ dữ liệu trong S3 Raw & Curated Buckets -> Delete Buckets `my-data-lake-raw-<ACCOUNT_ID>` và `my-data-lake-curated-<ACCOUNT_ID>`.
2. **AWS Glue**: Xóa Glue Job `ohlcv-glue-processor`, Crawler `ohlcv-crawler` và Database `financial_data_lake`.
3. **AWS Lambda**: Xóa Lambda Functions `financial-data-collector` và `financial-data-email`.
4. **Amazon EventBridge**: Xóa Schedule `daily-financial-pipeline-schedule`.
5. **Amazon DynamoDB & Cognito**: Delete Tables `Users`, `UserWatchlists` và User Pool `financial-data-user-pool-dev`.
6. **AWS Amplify & API Gateway**: Delete Amplify App và REST API `financial-data-platform-api`.
