---
title: "Notification Layer (LAMBDA EMAIL & AMAZON SES)"
date: 2024-01-01
weight: 10
chapter: false
pre: " <b> 5.4.10. </b> "
---

# 5.4.10. NOTIFICATION LAYER — LAMBDA EMAIL & AMAZON SES

The **Notification Layer** automates status aggregations and dispatches styled HTML report emails to system administrators via **Amazon SES (Simple Email Service)**.

---

### 1. Amazon SES Identity Verification

1. Open **Amazon SES Console** -> Select **Verified identities** -> Click **Create identity**.
2. Select **Email address** -> Enter `<VERIFIED_EMAIL>` -> Click **Create identity**.
3. Click the verification link sent by Amazon Web Services to the inbox.

![(Figure 5.4.10.1) Amazon SES Create Verified Identity Console](/images/workshop/image17.png)

![(Figure 5.4.10.2) Amazon SES Verified Email Identities List](/images/workshop/image18.png)

---

### 2. Lambda Email Notification Function

![(Figure 5.4.10.3) Source Code Interface of Lambda Email Function](/images/workshop/image19.png)

![(Figure 5.4.10.4) Gmail Inbox displaying HTML Formatted Automated Pipeline Report Email](/images/workshop/image20.png)
