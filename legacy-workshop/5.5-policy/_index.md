---
title: "Email Alerts & Web Dashboard Deployment (SES, Lambda & Amplify)"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

# 5.5. EMAIL ALERTS & WEB DASHBOARD DEPLOYMENT (SES, LAMBDA EMAIL & AMPLIFY)

In this module, we build the **Notification Layer & UI Layer** leveraging **Amazon SES**, **AWS Lambda Email**, and **AWS Amplify** to automate pipeline execution reporting emails and host the frontend Web Dashboard.

---

### PART 5: NOTIFICATION LAYER (LAMBDA EMAIL & AMAZON SES)

#### 5.1 Configure Amazon SES (Simple Email Service)

* **Description**: Verifies administrator email address (`duyphong242004@gmail.com`) on Amazon SES as the authorized sender/receiver for system reports.
* **Detailed AWS Console Steps**:
  1. Access **Amazon SES Console** ➔ **Verified identities** ➔ Click **Create identity**.
  2. **Identity type**: Select **Email address**.
  3. **Email address**: Enter `duyphong242004@gmail.com` ➔ Click **Create identity**.
  4. Log in to Gmail inbox `duyphong242004@gmail.com`, open the verification email sent by AWS, and click the confirmation link.

![(Figure 5.1) Email Address Verified Identity Creation Page on Amazon SES](/images/workshop/image17.png)

![(Figure 5.2 - Fig 5.1 in doc) Verified Email Identities List on Amazon SES (Status: Verified)](/images/workshop/image18.png)

---

#### 5.2 Lambda Email Notification

* **Description**: Serverless Lambda function `financial-data-email` receives execution metrics from the pipeline, generates an HTML email formatted table detailing processed ticker statistics, and dispatches via SES.
* **Detailed AWS Console Steps**:
  1. Open **AWS Lambda Console** ➔ Click **Create function**.
  2. **Function name**: `financial-data-email`.
  3. **Runtime**: `Python 3.10` / `Python 3.12`.
  4. Go to **Configuration** ➔ **Environment variables** ➔ Set: `ADMIN_EMAIL` = `duyphong242004@gmail.com`.

![(Figure 5.3) Lambda Function Source Code Configuration Interface for financial-data-email](/images/workshop/image19.png)

![(Figure 5.4 - Fig 5.2 in doc) Gmail Inbox opening automatically generated Pipeline HTML Report Email](/images/workshop/image20.png)

* **Complete Source Code for Lambda Email Notification (`lambda_function.py`)**:

```python
import boto3
import json
import os

ses_client = boto3.client('ses', region_name=os.environ.get('AWS_DEFAULT_REGION', 'ap-southeast-1'))
ADMIN_EMAIL = os.environ.get('ADMIN_EMAIL', 'duyphong242004@gmail.com')

def lambda_handler(event, context):
    pipeline_status = event.get('status', 'SUCCESS')
    processed_tickers = event.get('processed_tickers', 119)
    failed_tickers = event.get('failed_tickers', 0)
    
    subject = f"📊 [AWS Financial Data Platform] Report: Pipeline Execution Status ({pipeline_status})"
    
    html_body = f"""
    <html>
    <head>
        <style>
            body {{ font-family: Arial, sans-serif; line-height: 1.6; color: #333; }}
            .container {{ max-width: 600px; margin: 0 auto; padding: 20px; border: 1px solid #e0e0e0; border-radius: 8px; }}
            .header {{ background-color: #232f3e; color: #ffffff; padding: 15px; text-align: center; border-radius: 6px 6px 0 0; }}
            .badge-success {{ background-color: #28a745; color: white; padding: 4px 8px; border-radius: 4px; font-weight: bold; }}
            .table {{ width: 100%; border-collapse: collapse; margin-top: 15px; }}
            .table th, .table td {{ border: 1px solid #ddd; padding: 10px; text-align: left; }}
            .table th {{ background-color: #f4f4f4; }}
        </style>
    </head>
    <body>
        <div class="container">
            <div class="header">
                <h2>Automated AWS Financial Data Pipeline Report</h2>
            </div>
            <div class="content">
                <p>Dear Administrator,</p>
                <p>The market data crawler, PySpark Glue ETL process, and Glue Data Catalog updates completed successfully.</p>
                <table class="table">
                    <tr><th>Pipeline Status</th><td><span class="badge-success">{pipeline_status}</span></td></tr>
                    <tr><th>Processed Tickers Count</th><td><b>{processed_tickers}</b></td></tr>
                    <tr><th>Failed Tickers Count</th><td><b>{failed_tickers}</b></td></tr>
                    <tr><th>S3 Raw Bucket</th><td><code>my-data-lake-raw-699061130094-dev</code></td></tr>
                    <tr><th>S3 Curated Bucket</th><td><code>my-data-lake-curated-699061130094-dev</code></td></tr>
                </table>
                <p>You can query the updated dataset table <code>financial_data_lake.ohlcv</code> via Amazon Athena Editor.</p>
            </div>
        </div>
    </body>
    </html>
    """
    
    response = ses_client.send_email(
        Source=ADMIN_EMAIL,
        Destination={'ToAddresses': [ADMIN_EMAIL]},
        Message={
            'Subject': {'Data': subject, 'Charset': 'UTF-8'},
            'Body': {'Html': {'Data': html_body, 'Charset': 'UTF-8'}}
        }
    )
    
    return {
        'statusCode': 200,
        'body': json.dumps({'message': 'Report email dispatched successfully', 'MessageId': response['MessageId']})
    }
```

---

### PART 6: DEPLOY WEB DASHBOARD ON AWS AMPLIFY

* **Description**: Deploys the Web Dashboard application (React/Next.js) on **AWS Amplify**, connecting it to REST API Gateway endpoints and Cognito User Pool credentials.
* **Detailed AWS Console Steps**:
  1. Open **AWS Amplify Console** ➔ Click **Host web app**.
  2. Connect to the GitHub repository hosting the Web Dashboard source code.
  3. Configure build environment variables:
     * `REACT_APP_API_URL`: URL of REST API Gateway `financial-data-platform-api` (`dev`).
     * `REACT_APP_COGNITO_USER_POOL_ID`: `financial-data-user-pool-dev` User Pool ID.
     * `REACT_APP_COGNITO_CLIENT_ID`: `financial-data-web-client` App Client ID.
  4. Click **Save and deploy**. AWS Amplify executes the automated CI/CD build pipeline and provisions HTTPS domain routes.

> [!TIP]
> 🌐 **LIVE DEMO WEB DASHBOARD & TEST ACCOUNT CREDENTIALS**
> * **Live Application URL:** [https://feature-dashboard.dgku51j8dnv70.amplifyapp.com/login](https://feature-dashboard.dgku51j8dnv70.amplifyapp.com/login)
> * **Test Email Account:** `thoa@gmail.com`
> * **Test Password:** `1111111`
