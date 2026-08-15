---
title: "Đăng ký người dùng với AWS Cognito"
date: 2024-01-01
weight: 9
chapter: false
pre: " <b> 5.4.9. </b> "
---

# 5.4.9. Quản lý Đăng ký & Xác thực người dùng với AWS Cognito

Dịch vụ **Amazon Cognito User Pool** quản lý đăng ký, đăng nhập và cấp Token JWT an toàn cho người dùng ứng dụng Web Dashboard.

---

### Quy trình khởi tạo trên Amazon Cognito Console

1. Vào **Amazon Cognito Console** -> Click **Create user pool**.
2. **Sign-in options**: Chọn **Email** (Đăng nhập bằng Email).
3. **Password policy**: Chọn chuẩn Cognito (tối thiểu 8 ký tự, chữ hoa, chữ thường, số, ký tự đặc biệt).
4. **User pool name**: `financial-data-user-pool-dev`.
5. **App Client Configuration**:
   - App client type: `Public client` (SPA - Single Page Application).
   - App client name: `financial-data-web-client`.
   - Client secret: Chọn **Don't generate a client secret** (bắt buộc cho React/Amplify frontend).

![(Hình 5.4.9.1) Giao diện tùy chọn thuộc tính đăng nhập khi tạo Cognito User Pool](/images/workshop/image24.png)

![(Hình 5.4.9.2) Amazon Cognito Console màn hình thông tin App Client ID & User List](/images/workshop/image25.png)
