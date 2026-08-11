---
title: "Web Dashboard & Email Alerts (Amplify, Lambda & SES)"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

# 5.5. WEB DASHBOARD & EMAIL ALERTS (AMPLIFY, LAMBDA & SES)

In this module, our team completes the **UI & Alert Layer** by deploying an interactive Web Dashboard for data visualization and building an automated Email Alert system for high-risk corporate distress warnings.

---

### Step 1: Deploy Web Dashboard on AWS Amplify
1. Navigate to **AWS Amplify Console** ➔ Click **Host web app**.
2. Connect your GitHub Repository containing the Web Dashboard frontend source code (React/Next.js).
3. Configure Build Environment Variables:
   - `REACT_APP_API_GATEWAY_URL`: REST API Gateway `prod` URL.
   - `REACT_APP_COGNITO_USER_POOL_ID`: User Pool ID.
   - `REACT_APP_COGNITO_CLIENT_ID`: App Client ID.
4. Click **Save and deploy**. AWS Amplify automatically runs CI/CD build pipelines and provisions HTTPS domains.

> [!TIP]
> 🌐 **LIVE DEMO WEB APPLICATION & TEST ACCOUNT**
> * **Live Web Application URL:** [https://feature-dashboard.dgku51j8dnv70.amplifyapp.com/login](https://feature-dashboard.dgku51j8dnv70.amplifyapp.com/login)
> * **Test Account Email:** `thoa@gmail.com`
> * **Test Account Password:** `1111111`

---

### Step 2: Configure Amazon SES (Simple Email Service)
1. Open **Amazon SES Console** ➔ **Verified identities**.
2. Click **Create identity** ➔ Select **Email address**.
3. Enter the alert recipient email address (e.g., `alert-financial@domain.com`).
4. Access the email inbox and click the AWS verification link to complete setup.

---

### Step 3: Write AWS Lambda Alert Notification Script (SES Integration)

AWS Lambda function scanning distress-labeled records ($Z\text{-score} \le 1.23$) and triggering Amazon SES email delivery:

```python
import boto3
import json

ses_client = boto3.client('ses', region_name='ap-southeast-1')
SENDER_EMAIL = "alert-financial@domain.com"

def lambda_handler(event, context):
    # Extract distress records triggered by Glue ETL or Athena Callbacks
    records = event.get('distress_records', [])
    
    for record in records:
        symbol = record.get('symbol')
        z_score = record.get('z_score')
        year = record.get('year')
        
        subject = f"⚠️ HIGH FINANCIAL DISTRESS RISK ALERT: Ticker {symbol} ({year})"
        body_text = f"""
        VIETNAMESE SECURITIES FINANCIAL RISK ANALYTICS SYSTEM
        -------------------------------------------------------------
        Company Ticker: {symbol}
        Fiscal Year: {year}
        Altman Z-Score: {z_score} (Entered Red Flag - Distress Zone <= 1.23)
        
        Recommendation: High risk of financial distress or corporate bankruptcy. Investors should carefully evaluate portfolio holdings.
        """
        
        response = ses_client.send_email(
            Source=SENDER_EMAIL,
            Destination={'ToAddresses': [SENDER_EMAIL]},
            Message={
                'Subject': {'Data': subject, 'Charset': 'UTF-8'},
                'Body': {'Text': {'Data': body_text, 'Charset': 'UTF-8'}}
            }
        )
        print(f"Successfully dispatched alert email for {symbol}, MessageId: {response['MessageId']}")
        
    return {
        'statusCode': 200,
        'body': json.dumps("Automated Email Alert Dispatching Completed Successfully!")
    }
```
