---
title: "Frontend Hosting via AWS Amplify"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 5.4.8. </b> "
---

# 5.4.8. Frontend Web Dashboard Hosting via AWS Amplify

The user interface Web Dashboard (built with React SPA) is integrated via CI/CD pipelines and hosted on **AWS Amplify**, featuring automated SSL/TLS certificates and global CDN distribution.

---

### AWS Amplify Deployment Steps

1. Open **AWS Amplify Console** -> Click **Host web app**.
2. Connect to **GitHub Repository** hosting the React Dashboard source code.
3. Configure Build Environment Variables:
   - `REACT_APP_API_URL`: REST API Gateway Endpoint URL
   - `REACT_APP_COGNITO_USER_POOL_ID`: Cognito User Pool ID
   - `REACT_APP_COGNITO_CLIENT_ID`: App Client ID
4. Click **Save and deploy**. AWS Amplify builds the React bundle and generates a live HTTPS domain.

> [!TIP]
> 🌐 **LIVE DEMO WEB DASHBOARD & TEST CREDENTIALS**
> * **Application Live URL:** [https://feature-dashboard.dgku51j8dnv70.amplifyapp.com/login](https://feature-dashboard.dgku51j8dnv70.amplifyapp.com/login)
> * **Test Account Email:** `thoa@gmail.com`
> * **Password:** `1111111`
