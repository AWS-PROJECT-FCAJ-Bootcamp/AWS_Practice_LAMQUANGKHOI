---
title: "REST API, Database & Authentication (DynamoDB, Cognito & API Gateway)"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

# 5.4. REST API, DATABASE & AUTHENTICATION (DYNAMODB, COGNITO & API GATEWAY)

In this module, we construct the **API, Auth & Security Layer** utilizing **Amazon DynamoDB**, **Amazon Cognito**, **Amazon API Gateway**, and **AWS WAF** to store application state, manage JWT authentication, and expose protected REST APIs.

---

### PART 7: API, AUTH & SECURITY LAYER (DYNAMODB, COGNITO, API GATEWAY, WAF)

#### 7.1 NoSQL Database Amazon DynamoDB

* **Description**: High-throughput, millisecond-latency storage for user profiles and watchlist tickers.
* **AWS Console Steps**:
  1. Access **DynamoDB Console** ➔ **Tables** ➔ Click **Create table**.
  2. Create Table 1 - `Users`:
     * **Table name**: `Users`
     * **Partition key (PK)**: `email` (String)
  3. Create Table 2 - `UserWatchlists`:
     * **Table name**: `UserWatchlists`
     * **Partition key (PK)**: `user_id` (String)
     * **Sort key (SK)**: `symbol` (String)
  4. Click **Create table**. Both tables will show `Active` status once provisioned.

![(Figure 4.1 - Fig 7.1 in doc) NoSQL DynamoDB Tables List in Management Console](/images/workshop/image23.png)

---

#### 7.2 User Authentication with Amazon Cognito

* **Description**: Manages user registration, login workflows, and secure JWT Token (Access Token & ID Token) issuance for the Web Dashboard.
* **AWS Console Steps**:
  1. **Create User Pool**:
     * Open **Amazon Cognito Console** ➔ Click **Create user pool**.
     * **Sign-in options**: Select **Email** (Users authenticate via Email).
     * **Password policy**: Select default Cognito policy (min 8 characters, uppercase, lowercase, numbers, special symbols).
     * **Multi-factor authentication (MFA)**: Select **No MFA** (for PoC testing).
     * **User pool name**: Set name to `financial-data-user-pool-dev`.
  2. **Create App Client (For React Single Page Application)**:
     * Select **App type**: `Public client` (SPA - Single Page Application).
     * **App client name**: `financial-data-web-client`.
     * **Client secret**: Choose **Don't generate a client secret** (Mandatory for client-side SPAs).
  3. **Retrieve Credentials**:
     * Copy `User Pool ID` (e.g., `ap-southeast-1_xxxxxxxxx`) and `App Client ID` (e.g., `7xxxxxxxxxxxxxxxxxxxxxxxxx`).

![(Figure 4.2) Sign-in Option Setup Page for Amazon Cognito User Pool](/images/workshop/image24.png)

![(Figure 4.3) Cognito User Pool Overview displaying App Client ID & User List](/images/workshop/image25.png)

---

#### 7.3 Amazon API Gateway & AWS WAF

* **Description**: Exposes RESTful APIs routing traffic to Lambda Backend functions (`financial-data-platform-api`), protected by AWS WAF rules against web exploits (SQL Injection, Rate Limiting).
* **Attaching Cognito Authorizer to API Gateway**:
  1. Navigate to **API Gateway Console** ➔ Select API `financial-data-platform-api`.
  2. Select **Authorizers** in the left menu ➔ Click **Create authorizer**:
     * **Authorizer name**: `CognitoAuthorizer`.
     * **Authorizer type**: Select **Cognito**.
     * **Cognito user pool**: Choose `financial-data-user-pool-dev`.
     * **Token source**: Set to `Authorization` (Header submitted by Frontend).
  3. **Bind Authorizer to API Methods**:
     * Navigate to **Resources** tab ➔ Select method `ANY` on `/` or `/{proxy+}` route.
     * Edit **Method Request** ➔ Set **Authorization** to `CognitoAuthorizer`.
  4. Click **Deploy API** to Stage `dev`.

![(Figure 4.4) API Gateway Console REST API Resources & Stages (/dev)](/images/workshop/image26.png)

![(Figure 4.5) API Endpoints & Cognito Authorizer Configuration on Amazon API Gateway](/images/workshop/image27.png)
