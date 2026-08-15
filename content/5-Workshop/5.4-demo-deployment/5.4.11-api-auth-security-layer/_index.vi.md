---
title: "API, Auth & Security Layer (DYNAMODB, COGNITO, API GATEWAY, WAF)"
date: 2024-01-01
weight: 11
chapter: false
pre: " <b> 5.4.11. </b> "
---

# 5.4.11. API, AUTH & SECURITY LAYER (DYNAMODB, COGNITO, API GATEWAY, WAF)

Tầng dịch vụ **API, Auth & Security Layer** kết hợp **Amazon DynamoDB**, **Amazon Cognito**, **Amazon API Gateway** và **AWS WAF** để cung cấp các điểm cuối RESTful API độ trễ thấp và bảo mật cao.

---

### 1. Khởi tạo Cơ sở dữ liệu NoSQL Amazon DynamoDB

- Tạo Bảng 1: `Users` (Partition key: `email` - String).
- Tạo Bảng 2: `UserWatchlists` (Partition key: `user_id` - String, Sort key: `symbol` - String).

![(Hình 5.4.11.1) Danh sách các bảng NoSQL lưu trữ thông tin ứng dụng trên Amazon DynamoDB Console](/images/workshop/image23.png)

---

### 2. Bảo mật API Gateway với Cognito Authorizer & AWS WAF

1. Mở **API Gateway Console** -> Chọn API `financial-data-platform-api`.
2. Tạo **Authorizer**: Type = `Cognito`, User Pool = `financial-data-user-pool-dev`, Token Source = `Authorization`.
3. Gán Authorizer vào các Resources/Methods -> Deploy API lên Stage `dev`.

![(Hình 5.4.11.2) Vị trí API Gateway Console REST API Resources & Stages (/dev)](/images/workshop/image26.png)

![(Hình 5.4.11.3) Cấu hình các điểm cuối API (Endpoints) và gán Cognito Authorizer trên Amazon API Gateway](/images/workshop/image27.png)
