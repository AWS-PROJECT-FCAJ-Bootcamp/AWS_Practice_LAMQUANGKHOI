---
title: "API, Auth & Security Layer (DYNAMODB, COGNITO, API GATEWAY, WAF)"
date: 2024-01-01
weight: 11
chapter: false
pre: " <b> 5.4.11. </b> "
---

# 5.4.11. API, AUTH & SECURITY LAYER (DYNAMODB, COGNITO, API GATEWAY, WAF)

The **API, Auth & Security Layer** integrates **Amazon DynamoDB**, **Amazon Cognito**, **Amazon API Gateway**, and **AWS WAF** to expose low-latency, highly secure RESTful API endpoints.

---

### 1. Amazon DynamoDB NoSQL Tables

- Table 1: `Users` (Partition key: `email` - String).
- Table 2: `UserWatchlists` (Partition key: `user_id` - String, Sort key: `symbol` - String).

![(Figure 5.4.11.1) Amazon DynamoDB Active Tables List](/images/workshop/image23.png)

---

### 2. API Gateway Security with Cognito Authorizer & AWS WAF

![(Figure 5.4.11.2) API Gateway Console REST API Resources & Stages](/images/workshop/image26.png)

![(Figure 5.4.11.3) API Endpoints Configuration & Cognito Authorizer Integration](/images/workshop/image27.png)
