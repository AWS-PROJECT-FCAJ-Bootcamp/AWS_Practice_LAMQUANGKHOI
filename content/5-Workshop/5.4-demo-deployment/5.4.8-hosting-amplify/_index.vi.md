---
title: "Hosting giao diện thông qua Amplify"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 5.4.8. </b> "
---

# 5.4.8. Triển khai Web Dashboard trên AWS Amplify

Giao diện người dùng Web Dashboard (viết bằng React Single Page Application) được kết nối CI/CD và hosting trực tiếp trên **AWS Amplify**, cấp phát chứng chỉ HTTPS tự động.

---

### Quy trình triển khai trên AWS Amplify Console

1. Vào **AWS Amplify Console** -> Click **Host web app**.
2. Chọn nguồn mã nguồn **GitHub** -> Ủy quyền truy cập Repository chứa mã nguồn React Web Dashboard.
3. Cấu hình các biến môi trường Build (Environment Variables):
   - `REACT_APP_API_URL`: URL REST API Gateway `https://<api-id>.execute-api.ap-southeast-1.amazonaws.com/dev`
   - `REACT_APP_COGNITO_USER_POOL_ID`: `<REGION>_<USER_POOL_ID>`
   - `REACT_APP_COGNITO_CLIENT_ID`: `<APP_CLIENT_ID>`
4. Click **Save and deploy**. Hệ thống Amplify tự động build mã nguồn và xuất tên miền ứng dụng.

> [!TIP]
> 🌐 **TRANG WEB DEMO THỰC TẾ & TÀI KHOẢN THỬ NGHIỆM**
> * **Đường dẫn ứng dụng Web (Live URL):** [https://feature-dashboard.dgku51j8dnv70.amplifyapp.com/login](https://feature-dashboard.dgku51j8dnv70.amplifyapp.com/login)
> * **Tài khoản thử nghiệm (Test User):** `thoa@gmail.com`
> * **Mật khẩu (Password):** `1111111`
