---
title: "User Auth with AWS Cognito"
date: 2024-01-01
weight: 9
chapter: false
pre: " <b> 5.4.9. </b> "
---

# 5.4.9. User Registration & Authentication with AWS Cognito

**Amazon Cognito User Pools** manage user sign-ups, sign-ins, and issue secure JSON Web Tokens (JWT) for the React Web Dashboard application.

---

### Console Setup Steps

1. Open **Amazon Cognito Console** -> Click **Create user pool**.
2. **Sign-in options**: Select **Email**.
3. **Password policy**: Standard Cognito policy (min 8 chars, uppercase, lowercase, numbers, symbols).
4. **User pool name**: `financial-data-user-pool-dev`.
5. **App Client Setup**:
   - App client type: `Public client` (SPA).
   - App client name: `financial-data-web-client`.
   - Client secret: Select **Don't generate a client secret**.

![(Figure 5.4.9.1) Amazon Cognito User Pool Sign-in Options Setup](/images/workshop/image24.png)

![(Figure 5.4.9.2) Cognito User Pool Console displaying App Client ID & User Accounts List](/images/workshop/image25.png)
