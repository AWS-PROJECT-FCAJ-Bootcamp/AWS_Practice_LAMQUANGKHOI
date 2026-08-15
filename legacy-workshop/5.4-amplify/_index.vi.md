---
title: "REST API, Cơ sở dữ liệu & Xác thực (DynamoDB, Cognito & API Gateway)"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

# 5.4. REST API, CƠ SỞ DỮ LIỆU & XÁC THỰC (DYNAMODB, COGNITO & API GATEWAY)

Trong bài học này, nhóm mình tiến hành xây dựng **API, Auth & Security Layer** sử dụng **Amazon DynamoDB**, **Amazon Cognito**, **Amazon API Gateway** và **AWS WAF** để quản lý thông tin người dùng, xác thực token JWT và cung cấp giao diện RESTful API an toàn.

---

### PHẦN 7: API, AUTH & SECURITY LAYER (DYNAMODB, COGNITO, API GATEWAY, WAF)

#### 7.1 Cơ sở dữ liệu NoSQL DynamoDB

* **Nội dung mô tả**: Lưu trữ thông tin người dùng và danh sách cổ phiếu theo dõi (Watchlist) với độ trễ milisecond.
* **Thao tác AWS Console Chi tiết**:
  1. Truy cập **DynamoDB Console** ➔ **Tables** ➔ Bấm **Create table**.
  2. Tạo Bảng 1 - `Users`:
     * **Table name**: `Users`
     * **Partition key (PK)**: `email` (String)
  3. Tạo Bảng 2 - `UserWatchlists`:
     * **Table name**: `UserWatchlists`
     * **Partition key (PK)**: `user_id` (String)
     * **Sort key (SK)**: `symbol` (String)
  4. Bấm **Create table**. Sau khi hoàn tất, 2 bảng sẽ hiển thị trạng thái `Active`.

![(Hình 4.1 - Hình 7.1 trong tài liệu) Danh sách các bảng NoSQL lưu trữ thông tin ứng dụng trên Amazon DynamoDB](/images/workshop/image23.png)

---

#### 7.2 Xác thực người dùng với Amazon Cognito

* **Nội dung mô tả**: Quản lý đăng ký, đăng nhập và cấp Token JWT an toàn cho người dùng ứng dụng Web Dashboard.
* **Thao tác AWS Console Chi tiết**:
  1. **Tạo User Pool**:
     * Vào **Amazon Cognito Console** ➔ Click **Create user pool**.
     * **Sign-in options**: Chọn **Email** (Người dùng đăng nhập bằng Email).
     * **Password policy**: Chọn chuẩn Cognito (tối thiểu 8 ký tự, chữ hoa, chữ thường, số, ký tự đặc biệt).
     * **Multi-factor authentication (MFA)**: Chọn **No MFA** (để thử nghiệm PoC).
     * **User pool name**: Đặt tên `financial-data-user-pool-dev`.
  2. **Tạo App Client (Dành cho Frontend React)**:
     * Chọn **App type**: `Public client` (SPA - Single Page Application).
     * **App client name**: `financial-data-web-client`.
     * **Client secret**: Chọn **Don't generate a client secret** (Bắt buộc đối với Single Page App như React/Amplify).
  3. **Lấy thông tin kết nối**:
     * Sao chép `User Pool ID` (ví dụ: `ap-southeast-1_xxxxxxxxx`) và `App Client ID` (ví dụ: `7xxxxxxxxxxxxxxxxxxxxxxxxx`).

![(Hình 4.2) Giao diện tùy chọn thuộc tính đăng nhập khi tạo Cognito User Pool](/images/workshop/image24.png)

![(Hình 4.3) Amazon Cognito Console màn hình thông tin App Client ID & User List](/images/workshop/image25.png)

---

#### 7.3 Amazon API Gateway & AWS WAF

* **Nội dung mô tả**: Cung cấp RESTful API kết nối Lambda Backend (`financial-data-platform-api`), được bảo vệ bởi AWS WAF ngăn chặn các tấn công web (SQL Injection, Rate Limiting).
* **Quy trình gắn Cognito Authorizer vào API Gateway**:
  1. Vào **API Gateway Console** ➔ Chọn API `financial-data-platform-api`.
  2. Chọn mục **Authorizers** ở menu bên trái ➔ Click **Create authorizer**:
     * **Authorizer name**: `CognitoAuthorizer`.
     * **Authorizer type**: Chọn **Cognito**.
     * **Cognito user pool**: Chọn `financial-data-user-pool-dev`.
     * **Token source**: Đặt là `Authorization` (Header mà Frontend truyền lên).
  3. **Gán Authorizer vào tài nguyên API**:
     * Vào mục **Resources** ➔ Chọn method `ANY` tại tài nguyên `/` hoặc `/{proxy+}`.
     * Tại mục **Method Request**, chỉnh sửa phần **Authorization**: Chọn `CognitoAuthorizer`.
  4. Bấm nút **Deploy API** sang Stage `dev`.

![(Hình 4.4) Vị trí API Gateway Console REST API Resources & Stages (/dev)](/images/workshop/image26.png)

![(Hình 4.5) Cấu hình các điểm cuối API (Endpoints) và gán Cognito Authorizer trên Amazon API Gateway](/images/workshop/image27.png)

