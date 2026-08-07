---
title: "Xây dựng REST API & Xác thực người dùng (Cognito, API Gateway & Lambda API)"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

# 5.4. XÂY DỰNG REST API & XÁC THỰC NGƯỜI DÙNG (COGNITO, API GATEWAY & LAMBDA API)

Trong phần này, nhóm mình xây dựng **API & Auth Layer** để cung cấp giao diện RESTful API an toàn cho ứng dụng Web Dashboard truy vấn thông tin báo cáo tài chính và danh sách cảnh báo rủi ro.

---

### Bước 1: Khởi tạo Amazon Cognito User Pool
1. Truy cập **Amazon Cognito Console** ➔ Nhấp **Create user pool**.
2. Đặt tên User Pool: `vietnam-financial-user-pool`.
3. Cấu hình tùy chọn đăng nhập: **Email** và **Username**.
4. Thiết lập độ phức tạp mật khẩu và cấp Token JWT (Access Token & ID Token).
5. Tạo mới **App Client**: `vietnam-financial-web-client`.
6. Lưu lại `User Pool ID` và `App Client ID`.

---

### Bước 2: Viết mã nguồn AWS Lambda Backend API (Athena Integration)

Mã nguồn AWS Lambda nhận REST API request từ API Gateway, khởi tạo truy vấn SQL trên Amazon Athena và trả dữ liệu JSON cho Client:

```python
import json
import boto3
import time

athena_client = boto3.client('athena')
DATABASE = 'vietnam_financial_db'
S3_OUTPUT = 's3://s3-vietnam-financial-curated-data-prod/athena_query_results/'

def lambda_handler(event, context):
    # Lấy tham số mã chứng khoán từ Query String (ví dụ: /api/financial?symbol=VNM)
    params = event.get('queryStringParameters') or {}
    symbol = params.get('symbol', 'VNM')
    
    query = f"""
        SELECT symbol, year, z_score, distress_zone, roa, roe, dar, cr 
        FROM {DATABASE}.financial_features 
        WHERE symbol = '{symbol}' 
        ORDER BY year DESC;
    """
    
    # Thực thi Athena SQL Query
    response = athena_client.start_query_execution(
        QueryString=query,
        QueryExecutionContext={'Database': DATABASE},
        ResultConfiguration={'OutputLocation': S3_OUTPUT}
    )
    
    query_execution_id = response['QueryExecutionId']
    
    # Chờ kết quả truy vấn Athena
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

### Bước 3: Cấu hình Amazon API Gateway & AWS WAF
1. Vào **Amazon API Gateway Console** ➔ Tạo mới **REST API**: `vietnam-financial-api`.
2. Tạo Resource `/api/financial` và tạo Method `GET`.
3. Tạo **Cognito Authorizer**: Thêm `vietnam-financial-user-pool` làm Authorizer kiểm tra JWT Token ở mỗi request.
4. Gắn **AWS WAF (Web Application Firewall)** để chống các cuộc tấn công Web phổ biến (DDoS, SQL Injection, Cross-Site Scripting).
5. Nhấp **Deploy API** sang Stage `prod`. Lưu lại `Invoke URL`.
