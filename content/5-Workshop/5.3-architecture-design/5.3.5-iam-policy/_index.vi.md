---
title: "Khởi tạo và cấu hình IAM policy"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.3.5. </b> "
---

# 5.3.5. Khởi tạo và Cấu hình các IAM Policy chuẩn bảo mật

Trong hệ thống **Automated Securities Financial Data Platform trên AWS**, phân quyền bảo mật IAM được thiết kế chặt chẽ theo nguyên tắc **Quyền tối thiểu (Principle of Least-Privilege)** và nguyên tắc **Tách biệt vai trò (Separation of Duties)**.

Toàn bộ các tài khoản AWS Account ID cá nhân và địa chỉ Email riêng đã được chuẩn hóa thành các **Biến giữ chỗ (Placeholders)** như `<ACCOUNT_ID>`, `<REGION>`, `<VERIFIED_EMAIL>`, `<RAW_BUCKET_NAME>`, `<CURATED_BUCKET_NAME>` để phục vụ việc tham khảo, tái sử dụng và triển khai hạ tầng bằng Code (Terraform/CloudFormation).

---

## 1. Phân loại cấu trúc IAM Policy

Hệ thống phân quyền IAM được chia làm **2 nhóm chính**:
1. **Developer User Policy**: Cấp quyền cho Kỹ sư / Nhà phát triển triển khai và quản trị dự án qua AWS Console, CLI hoặc Terraform.
2. **Service Execution Roles & Inline Policies**: Cấp quyền thực thi tự động cho các dịch vụ AWS Serverless (Lambda, Glue, EventBridge, Cognito).

---

## 2. Nhóm 1: Policy dành cho Người dùng Quản trị (Developer IAM Policy)

### 📋 Policy 1: `DeveloperProjectAccessPolicy`
* **Đối tượng gán**: Gán cho IAM User / IAM Group của Kỹ sư triển khai dự án.
* **Phạm vi tác động**: Cấp quyền thao tác trên các tài nguyên thuộc dự án tại Region chỉ định (`<REGION>`).

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "S3BucketLevelAccess",
      "Effect": "Allow",
      "Action": [
        "s3:CreateBucket",
        "s3:DeleteBucket",
        "s3:GetBucketLocation",
        "s3:GetBucketPolicy",
        "s3:PutBucketPolicy",
        "s3:DeleteBucketPolicy",
        "s3:GetBucketVersioning",
        "s3:PutBucketVersioning",
        "s3:GetBucketLogging",
        "s3:PutBucketLogging",
        "s3:GetBucketNotification",
        "s3:PutBucketNotification",
        "s3:GetEncryptionConfiguration",
        "s3:PutEncryptionConfiguration",
        "s3:GetLifecycleConfiguration",
        "s3:PutLifecycleConfiguration",
        "s3:GetBucketTagging",
        "s3:PutBucketTagging",
        "s3:ListBucket",
        "s3:ListBucketVersions",
        "s3:GetBucketPublicAccessBlock",
        "s3:PutBucketPublicAccessBlock"
      ],
      "Resource": [
        "arn:aws:s3:::<RAW_BUCKET_NAME>",
        "arn:aws:s3:::<CURATED_BUCKET_NAME>",
        "arn:aws:s3:::aws-athena-query-results-<ACCOUNT_ID>-<REGION>"
      ]
    },
    {
      "Sid": "S3ObjectLevelAccess",
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject",
        "s3:DeleteObject",
        "s3:GetObjectVersion",
        "s3:DeleteObjectVersion",
        "s3:GetObjectTagging",
        "s3:PutObjectTagging",
        "s3:GetObjectAcl",
        "s3:PutObjectAcl"
      ],
      "Resource": [
        "arn:aws:s3:::<RAW_BUCKET_NAME>/*",
        "arn:aws:s3:::<CURATED_BUCKET_NAME>/*",
        "arn:aws:s3:::aws-athena-query-results-<ACCOUNT_ID>-<REGION>/*"
      ]
    },
    {
      "Sid": "S3ListAllBucketsForConsole",
      "Effect": "Allow",
      "Action": [
        "s3:ListAllMyBuckets",
        "s3:GetAccountPublicAccessBlock"
      ],
      "Resource": "*"
    },
    {
      "Sid": "LambdaFunctionManagement",
      "Effect": "Allow",
      "Action": [
        "lambda:CreateFunction",
        "lambda:UpdateFunctionCode",
        "lambda:UpdateFunctionConfiguration",
        "lambda:DeleteFunction",
        "lambda:GetFunction",
        "lambda:GetFunctionConfiguration",
        "lambda:ListFunctions",
        "lambda:InvokeFunction",
        "lambda:PublishVersion",
        "lambda:CreateAlias",
        "lambda:UpdateAlias",
        "lambda:DeleteAlias",
        "lambda:GetAlias",
        "lambda:ListAliases",
        "lambda:AddPermission",
        "lambda:RemovePermission",
        "lambda:GetPolicy",
        "lambda:ListTags",
        "lambda:TagResource",
        "lambda:UntagResource",
        "lambda:PutFunctionConcurrency",
        "lambda:DeleteFunctionConcurrency",
        "lambda:GetFunctionConcurrency"
      ],
      "Resource": [
        "arn:aws:lambda:<REGION>:<ACCOUNT_ID>:function:financial-data-collector",
        "arn:aws:lambda:<REGION>:<ACCOUNT_ID>:function:financial-data-email"
      ]
    },
    {
      "Sid": "LambdaIAMPassRole",
      "Effect": "Allow",
      "Action": "iam:PassRole",
      "Resource": [
        "arn:aws:iam::<ACCOUNT_ID>:role/LambdaCollectorExecutionRole-dev",
        "arn:aws:iam::<ACCOUNT_ID>:role/LambdaEmailExecutionRole-dev"
      ],
      "Condition": {
        "StringEquals": {
          "iam:PassedToService": "lambda.amazonaws.com"
        }
      }
    },
    {
      "Sid": "GlueJobAndCrawlerManagement",
      "Effect": "Allow",
      "Action": [
        "glue:CreateJob",
        "glue:UpdateJob",
        "glue:DeleteJob",
        "glue:GetJob",
        "glue:GetJobs",
        "glue:StartJobRun",
        "glue:StopJobRun",
        "glue:GetJobRun",
        "glue:GetJobRuns",
        "glue:BatchStopJobRun",
        "glue:CreateCrawler",
        "glue:UpdateCrawler",
        "glue:DeleteCrawler",
        "glue:GetCrawler",
        "glue:GetCrawlers",
        "glue:StartCrawler",
        "glue:StopCrawler",
        "glue:GetCrawlerMetrics"
      ],
      "Resource": [
        "arn:aws:glue:<REGION>:<ACCOUNT_ID>:job/ohlcv-glue-processor",
        "arn:aws:glue:<REGION>:<ACCOUNT_ID>:crawler/ohlcv-crawler"
      ]
    },
    {
      "Sid": "GlueDataCatalogManagement",
      "Effect": "Allow",
      "Action": [
        "glue:CreateDatabase",
        "glue:UpdateDatabase",
        "glue:DeleteDatabase",
        "glue:GetDatabase",
        "glue:GetDatabases",
        "glue:CreateTable",
        "glue:UpdateTable",
        "glue:DeleteTable",
        "glue:GetTable",
        "glue:GetTables",
        "glue:BatchDeleteTable",
        "glue:CreatePartition",
        "glue:UpdatePartition",
        "glue:DeletePartition",
        "glue:GetPartition",
        "glue:GetPartitions",
        "glue:BatchCreatePartition",
        "glue:BatchDeletePartition"
      ],
      "Resource": [
        "arn:aws:glue:<REGION>:<ACCOUNT_ID>:catalog",
        "arn:aws:glue:<REGION>:<ACCOUNT_ID>:database/financial_data_lake",
        "arn:aws:glue:<REGION>:<ACCOUNT_ID>:table/financial_data_lake/*"
      ]
    },
    {
      "Sid": "GlueIAMPassRole",
      "Effect": "Allow",
      "Action": "iam:PassRole",
      "Resource": "arn:aws:iam::<ACCOUNT_ID>:role/AWSGlueETLProcessorRole-dev",
      "Condition": {
        "StringEquals": {
          "iam:PassedToService": "glue.amazonaws.com"
        }
      }
    },
    {
      "Sid": "AthenaQueryExecution",
      "Effect": "Allow",
      "Action": [
        "athena:StartQueryExecution",
        "athena:StopQueryExecution",
        "athena:GetQueryExecution",
        "athena:GetQueryResults",
        "athena:GetQueryResultsStream",
        "athena:ListQueryExecutions",
        "athena:BatchGetQueryExecution",
        "athena:GetWorkGroup",
        "athena:CreateWorkGroup",
        "athena:UpdateWorkGroup",
        "athena:DeleteWorkGroup",
        "athena:ListWorkGroups",
        "athena:GetDataCatalog"
      ],
      "Resource": [
        "arn:aws:athena:<REGION>:<ACCOUNT_ID>:workgroup/*",
        "arn:aws:athena:<REGION>:<ACCOUNT_ID>:datacatalog/*"
      ]
    },
    {
      "Sid": "DynamoDBTableManagement",
      "Effect": "Allow",
      "Action": [
        "dynamodb:CreateTable",
        "dynamodb:UpdateTable",
        "dynamodb:DeleteTable",
        "dynamodb:DescribeTable",
        "dynamodb:ListTables",
        "dynamodb:PutItem",
        "dynamodb:GetItem",
        "dynamodb:UpdateItem",
        "dynamodb:DeleteItem",
        "dynamodb:Query",
        "dynamodb:Scan"
      ],
      "Resource": [
        "arn:aws:dynamodb:<REGION>:<ACCOUNT_ID>:table/Users",
        "arn:aws:dynamodb:<REGION>:<ACCOUNT_ID>:table/Users/index/*",
        "arn:aws:dynamodb:<REGION>:<ACCOUNT_ID>:table/UserWatchlists",
        "arn:aws:dynamodb:<REGION>:<ACCOUNT_ID>:table/UserWatchlists/index/*"
      ]
    },
    {
      "Sid": "EventBridgeSchedulerManagement",
      "Effect": "Allow",
      "Action": [
        "scheduler:CreateSchedule",
        "scheduler:UpdateSchedule",
        "scheduler:DeleteSchedule",
        "scheduler:GetSchedule",
        "scheduler:ListSchedules"
      ],
      "Resource": [
        "arn:aws:scheduler:<REGION>:<ACCOUNT_ID>:schedule/default/daily-financial-pipeline-schedule"
      ]
    },
    {
      "Sid": "CloudWatchLogsManagement",
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:DeleteLogGroup",
        "logs:DescribeLogGroups",
        "logs:CreateLogStream",
        "logs:GetLogEvents",
        "logs:FilterLogEvents"
      ],
      "Resource": [
        "arn:aws:logs:<REGION>:<ACCOUNT_ID>:log-group:/aws/lambda/financial-data-collector:*",
        "arn:aws:logs:<REGION>:<ACCOUNT_ID>:log-group:/aws/lambda/financial-data-email:*",
        "arn:aws:logs:<REGION>:<ACCOUNT_ID>:log-group:/aws-glue/*:*"
      ]
    },
    {
      "Sid": "IAMReadOnly",
      "Effect": "Allow",
      "Action": [
        "iam:GetRole",
        "iam:GetRolePolicy",
        "iam:ListRoles",
        "iam:ListRolePolicies",
        "iam:ListAttachedRolePolicies"
      ],
      "Resource": "*"
    }
  ]
}
```

---

## 3. Nhóm 2: Service Execution Roles & Inline Policies (Quyền tự động hóa dịch vụ)

### 🔵 Role 1: AWS Lambda Collector Execution Role (`LambdaCollectorExecutionRole-dev`)
Dành cho hàm `financial-data-collector` ghi log CloudWatch và cào dữ liệu thô vào S3 Raw Bucket.

#### 1a. Trust Policy (Mối quan hệ tin cậy)
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "LambdaAssumeRole",
      "Effect": "Allow",
      "Principal": {
        "Service": "lambda.amazonaws.com"
      },
      "Action": "sts:AssumeRole",
      "Condition": {
        "StringEquals": {
          "aws:SourceAccount": "<ACCOUNT_ID>"
        }
      }
    }
  ]
}
```

#### 1b. Permission Policy (`LambdaCollectorPermissionPolicy`)
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "CloudWatchLogsWrite",
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents"
      ],
      "Resource": [
        "arn:aws:logs:<REGION>:<ACCOUNT_ID>:log-group:/aws/lambda/financial-data-collector*"
      ]
    },
    {
      "Sid": "S3RawBucketList",
      "Effect": "Allow",
      "Action": [
        "s3:ListBucket",
        "s3:GetBucketLocation"
      ],
      "Resource": "arn:aws:s3:::<RAW_BUCKET_NAME>",
      "Condition": {
        "Bool": {
          "aws:SecureTransport": "true"
        }
      }
    },
    {
      "Sid": "S3RawBucketPutObject",
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:PutObjectTagging"
      ],
      "Resource": "arn:aws:s3:::<RAW_BUCKET_NAME>/*",
      "Condition": {
        "Bool": {
          "aws:SecureTransport": "true"
        }
      }
    }
  ]
}
```

---

### 🟠 Role 2: AWS Glue ETL Processor Role (`AWSGlueETLProcessorRole-dev`)
Dành cho tiến trình Glue ETL Job PySpark đọc S3 Raw, làm sạch dữ liệu, ghi file Parquet lên S3 Curated, cập nhật Data Catalog và kích hoạt Crawler.

> [!NOTE]
> Gán Managed Policy `arn:aws:iam::aws:policy/service-role/AWSGlueServiceRole` kèm Inline Policy bên dưới.

#### 2a. Trust Policy
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "GlueAssumeRole",
      "Effect": "Allow",
      "Principal": {
        "Service": "glue.amazonaws.com"
      },
      "Action": "sts:AssumeRole",
      "Condition": {
        "StringEquals": {
          "aws:SourceAccount": "<ACCOUNT_ID>"
        }
      }
    }
  ]
}
```

#### 2b. Inline Policy (`GlueETLCustomInlinePolicy`)
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "S3RawBucketRead",
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:GetObjectVersion",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::<RAW_BUCKET_NAME>",
        "arn:aws:s3:::<RAW_BUCKET_NAME>/*"
      ],
      "Condition": {
        "Bool": {
          "aws:SecureTransport": "true"
        }
      }
    },
    {
      "Sid": "S3CuratedBucketReadWrite",
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject",
        "s3:DeleteObject",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::<CURATED_BUCKET_NAME>",
        "arn:aws:s3:::<CURATED_BUCKET_NAME>/*"
      ],
      "Condition": {
        "Bool": {
          "aws:SecureTransport": "true"
        }
      }
    },
    {
      "Sid": "GlueDataCatalogReadWrite",
      "Effect": "Allow",
      "Action": [
        "glue:GetDatabase",
        "glue:GetDatabases",
        "glue:GetTable",
        "glue:GetTables",
        "glue:GetPartition",
        "glue:GetPartitions",
        "glue:BatchGetPartition",
        "glue:CreateTable",
        "glue:UpdateTable",
        "glue:DeleteTable",
        "glue:CreatePartition",
        "glue:UpdatePartition",
        "glue:DeletePartition",
        "glue:BatchCreatePartition",
        "glue:BatchDeletePartition"
      ],
      "Resource": [
        "arn:aws:glue:<REGION>:<ACCOUNT_ID>:catalog",
        "arn:aws:glue:<REGION>:<ACCOUNT_ID>:database/financial_data_lake",
        "arn:aws:glue:<REGION>:<ACCOUNT_ID>:table/financial_data_lake/*"
      ]
    },
    {
      "Sid": "GlueStartCrawler",
      "Effect": "Allow",
      "Action": [
        "glue:StartCrawler",
        "glue:GetCrawler",
        "glue:GetCrawlerMetrics"
      ],
      "Resource": "arn:aws:glue:<REGION>:<ACCOUNT_ID>:crawler/ohlcv-crawler"
    },
    {
      "Sid": "CloudWatchLogsWrite",
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents"
      ],
      "Resource": [
        "arn:aws:logs:<REGION>:<ACCOUNT_ID>:log-group:/aws-glue/*"
      ]
    }
  ]
}
```

---

### 🟢 Role 3: EventBridge Scheduler Role (`EventBridgeSchedulerRole-dev`)
Cho phép trình lập lịch EventBridge Scheduler tự động gọi hàm Lambda Collector theo lịch Cron.

#### 3a. Trust Policy
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "EventBridgeSchedulerAssumeRole",
      "Effect": "Allow",
      "Principal": {
        "Service": "scheduler.amazonaws.com"
      },
      "Action": "sts:AssumeRole",
      "Condition": {
        "StringEquals": {
          "aws:SourceAccount": "<ACCOUNT_ID>"
        },
        "ArnLike": {
          "aws:SourceArn": "arn:aws:scheduler:<REGION>:<ACCOUNT_ID>:schedule/default/daily-financial-pipeline-schedule"
        }
      }
    }
  ]
}
```

#### 3b. Permission Policy (`EventBridgeSchedulerPermissionPolicy`)
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "InvokeLambdaCollector",
      "Effect": "Allow",
      "Action": "lambda:InvokeFunction",
      "Resource": [
        "arn:aws:lambda:<REGION>:<ACCOUNT_ID>:function:financial-data-collector",
        "arn:aws:lambda:<REGION>:<ACCOUNT_ID>:function:financial-data-collector:*"
      ]
    }
  ]
}
```

---

### 🟣 Role 4: AWS Lambda Email Execution Role (`LambdaEmailExecutionRole-dev`)
Dành cho hàm `financial-data-email` tự động định dạng và gửi email báo cáo kết quả qua Amazon SES.

#### 4a. Trust Policy
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "LambdaEmailAssumeRole",
      "Effect": "Allow",
      "Principal": {
        "Service": "lambda.amazonaws.com"
      },
      "Action": "sts:AssumeRole",
      "Condition": {
        "StringEquals": {
          "aws:SourceAccount": "<ACCOUNT_ID>"
        }
      }
    }
  ]
}
```

#### 4b. Permission Policy (`LambdaEmailPermissionPolicy`)
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "CloudWatchLogsWrite",
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents"
      ],
      "Resource": [
        "arn:aws:logs:<REGION>:<ACCOUNT_ID>:log-group:/aws/lambda/financial-data-email*"
      ]
    },
    {
      "Sid": "SESSendEmailFromVerifiedIdentity",
      "Effect": "Allow",
      "Action": [
        "ses:SendEmail",
        "ses:SendRawEmail"
      ],
      "Resource": "arn:aws:ses:<REGION>:<ACCOUNT_ID>:identity/<VERIFIED_EMAIL>",
      "Condition": {
        "Bool": {
          "aws:SecureTransport": "true"
        },
        "StringLike": {
          "ses:FromAddress": "<VERIFIED_EMAIL>"
        }
      }
    }
  ]
}
```

---

### 🔴 Role 5: Cognito Authenticated User Role (`CognitoAuthUserRole-dev`)
Bảo mật cấp hàng (**Row-level Security** - `${cognito-identity.amazonaws.com:sub}`) cho phép người dùng sau khi đăng nhập qua AWS Cognito truy vấn bảng DynamoDB cá nhân.

#### 5a. Trust Policy
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "CognitoAuthenticatedAssumeRole",
      "Effect": "Allow",
      "Principal": {
        "Federated": "cognito-identity.amazonaws.com"
      },
      "Action": "sts:AssumeRoleWithWebIdentity",
      "Condition": {
        "StringEquals": {
          "cognito-identity.amazonaws.com:aud": "<REGION>:<IDENTITY_POOL_ID>"
        },
        "ForAnyValue:StringLike": {
          "cognito-identity.amazonaws.com:amr": "authenticated"
        }
      }
    }
  ]
}
```

#### 5b. Permission Policy (`CognitoAuthUserDynamoDBPolicy`)
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DynamoDBUsersTableAccess",
      "Effect": "Allow",
      "Action": [
        "dynamodb:GetItem",
        "dynamodb:PutItem",
        "dynamodb:UpdateItem",
        "dynamodb:Query"
      ],
      "Resource": [
        "arn:aws:dynamodb:<REGION>:<ACCOUNT_ID>:table/Users",
        "arn:aws:dynamodb:<REGION>:<ACCOUNT_ID>:table/Users/index/*"
      ],
      "Condition": {
        "ForAllValues:StringEquals": {
          "dynamodb:LeadingKeys": [
            "${cognito-identity.amazonaws.com:sub}"
          ]
        },
        "Bool": {
          "aws:SecureTransport": "true"
        }
      }
    },
    {
      "Sid": "DynamoDBUserWatchlistsTableAccess",
      "Effect": "Allow",
      "Action": [
        "dynamodb:GetItem",
        "dynamodb:PutItem",
        "dynamodb:UpdateItem",
        "dynamodb:Query"
      ],
      "Resource": [
        "arn:aws:dynamodb:<REGION>:<ACCOUNT_ID>:table/UserWatchlists",
        "arn:aws:dynamodb:<REGION>:<ACCOUNT_ID>:table/UserWatchlists/index/*"
      ],
      "Condition": {
        "ForAllValues:StringEquals": {
          "dynamodb:LeadingKeys": [
            "${cognito-identity.amazonaws.com:sub}"
          ]
        },
        "Bool": {
          "aws:SecureTransport": "true"
        }
      }
    }
  ]
}
```
