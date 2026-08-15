---
title: "Resource Clean Up Procedures"
date: 2024-01-01
weight: 12
chapter: false
pre: " <b> 5.4.12. </b> "
---

# 5.4.12. Resource Clean Up & Infrastructure Teardown

After completing workshop demonstrations and testing, sweeping all created AWS resources is essential to prevent unintended cloud charges.

---

### Teardown Checklist

1. **Amazon S3**: Empty and delete S3 Buckets `my-data-lake-raw-<ACCOUNT_ID>` and `my-data-lake-curated-<ACCOUNT_ID>`.
2. **AWS Glue**: Delete Glue Job `ohlcv-glue-processor`, Crawler `ohlcv-crawler`, and Database `financial_data_lake`.
3. **AWS Lambda**: Delete Lambda Functions `financial-data-collector` and `financial-data-email`.
4. **Amazon EventBridge**: Delete Schedule `daily-financial-pipeline-schedule`.
5. **Amazon DynamoDB & Cognito**: Delete Tables `Users`, `UserWatchlists`, and User Pool `financial-data-user-pool-dev`.
6. **AWS Amplify & API Gateway**: Teardown Amplify App hosting and API Gateway `financial-data-platform-api`.
