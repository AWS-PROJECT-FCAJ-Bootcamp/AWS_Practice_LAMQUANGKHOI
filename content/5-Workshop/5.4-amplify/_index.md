---
title: "REST API & User Authentication (Cognito, API Gateway & Lambda API)"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

# 5.4. REST API & USER AUTHENTICATION (COGNITO, API GATEWAY & LAMBDA API)

In this module, our team builds the **API & Auth Layer** to expose secure RESTful API endpoints for the Web Dashboard application to query financial indicator metrics and distress alert records.

---

### Step 1: Initialize Amazon Cognito User Pool
1. Open **Amazon Cognito Console** ➔ Click **Create user pool**.
2. User Pool Name: `vietnam-financial-user-pool`.
3. Sign-in options: **Email** and **Username**.
4. Configure password security policies and issue JWT Tokens (Access Token & ID Token).
5. Create an **App Client**: `vietnam-financial-web-client`.
6. Save the `User Pool ID` and `App Client ID`.

---

### Step 2: Write AWS Lambda Backend API Script (Athena Integration)

AWS Lambda function handling incoming REST requests from API Gateway, executing SQL queries against Amazon Athena, and returning JSON payloads:

```python
import json
import boto3
import time

athena_client = boto3.client('athena')
DATABASE = 'vietnam_financial_db'
S3_OUTPUT = 's3://s3-vietnam-financial-curated-data-prod/athena_query_results/'

def lambda_handler(event, context):
    # Extract query parameters (e.g., /api/financial?symbol=VNM)
    params = event.get('queryStringParameters') or {}
    symbol = params.get('symbol', 'VNM')
    
    query = f"""
        SELECT symbol, year, z_score, distress_zone, roa, roe, dar, cr 
        FROM {DATABASE}.financial_features 
        WHERE symbol = '{symbol}' 
        ORDER BY year DESC;
    """
    
    # Execute Athena Query
    response = athena_client.start_query_execution(
        QueryString=query,
        QueryExecutionContext={'Database': DATABASE},
        ResultConfiguration={'OutputLocation': S3_OUTPUT}
    )
    
    query_execution_id = response['QueryExecutionId']
    
    # Wait for execution completion
    time.sleep(2)
    
    results = athena_client.get_query_results(QueryExecutionId=query_execution_id)
    
    rows = results['ResultSet']['Rows']
    data = []
    headers = [col['VarCharValue'] for col in rows[0]['Data']]
    
    for row in rows[1:]:
        values = [field.get('VarCharValue', '') for field in row['Data']]
        data.append(dict(zip(headers, values)))
        
    return {
        'statusCode': 200,
        'headers': {
            'Content-Type': 'application/json',
            'Access-Control-Allow-Origin': '*'
        },
        'body': json.dumps({'symbol': symbol, 'financial_data': data}, ensure_ascii=False)
    }
```

---

### Step 3: Configure Amazon API Gateway & AWS WAF
1. Open **Amazon API Gateway Console** ➔ Create a new **REST API**: `vietnam-financial-api`.
2. Add Resource `/api/financial` and create a `GET` Method.
3. Attach **Cognito Authorizer**: Connect `vietnam-financial-user-pool` to enforce JWT token authorization.
4. Attach **AWS WAF (Web Application Firewall)** to protect against common web exploits (DDoS, SQL Injection, XSS).
5. Click **Deploy API** to Stage `prod`. Note down the `Invoke URL`.
