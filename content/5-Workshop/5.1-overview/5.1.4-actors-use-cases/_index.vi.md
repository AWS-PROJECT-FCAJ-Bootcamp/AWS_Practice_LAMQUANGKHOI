---
title: "Actor và use cases"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.1.4. </b> "
---

# 5.1.4. Phân tích Actor và Use Cases

### 1. Phân biệt Actor và Stakeholder
Trong thiết kế kiến trúc hệ thống (UML và C4 Model), việc phân định rõ giữa **Actor** và **Stakeholder** có ý nghĩa rất quan trọng:
* **Actor**: Là các đối tượng (người dùng, quản trị viên, hoặc hệ thống bên ngoài) trực tiếp gửi request hoặc nhận response qua biên giới hệ thống (System Boundary).
* **Stakeholder**: Là các bên có lợi ích liên quan đến dự án nhưng KHÔNG tương tác trực tiếp với ranh giới Use Case. Ví dụ: **AWS Cloud Provider** là stakeholder cung cấp hạ tầng, nhưng AWS KHÔNG phải là actor trong sơ đồ Use Case hay Context Diagram vì người dùng tương tác với hệ thống, còn hệ thống mới tự gọi các dịch vụ AWS.

---

### 2. Phân loại Actor trong hệ thống

#### Primary Actors (Nhóm Actor tương tác trực tiếp)
1. **Investor (Nhà đầu tư cá nhân / Khách hàng)**:
   * **Mục tiêu**: Tìm kiếm mã chứng khoán, xem báo cáo tài chính đã chuẩn hóa, xem biểu đồ chỉ báo kỹ thuật, quản lý danh mục (portfolio), danh sách theo dõi (watchlist), nhận email cảnh báo và xuất dữ liệu chuẩn hóa.
   * **Pain Point được giải quyết**: Không phải mở nhiều trang web rời rạc, dữ liệu được tự động hợp nhất và tính toán sẵn chỉ số.
2. **Administrator (Quản trị viên hệ thống)**:
   * **Mục tiêu**: Quản lý tài khoản user, kích hoạt đồng bộ dữ liệu thủ công khi cần, giám sát tình trạng pipeline và nhận cảnh báo chi phí AWS Budgets.
   * **Pain Point được giải quyết**: Giám sát tập trung toàn bộ luồng dữ liệu và chi phí hạ tầng theo thời gian thực.

#### Secondary / External Actors (Actor hệ thống bên ngoài)
1. **Financial Data Providers (VNStock, HOSE, HNX, CafeF, Fireant)**:
   * **Vai trò**: Nguồn cung cấp dữ liệu thô (OHLCV, Báo cáo tài chính, Tin tức) cho hàm cào dữ liệu của hệ thống.

---

### 3. Sơ đồ Use Case tổng quan (Use Case Diagram)

```mermaid
graph LR
    subgraph Actors["Các Tác nhân (Actors)"]
        INV["Investor (Nhà đầu tư)"]
        ADM["Administrator (Quản trị viên)"]
        FDP["Financial Data Providers"]
    end

    subgraph System["Ranh giới Hệ thống Financial Data Platform"]
        UC1["UC01: Đăng nhập & Xác thực"]
        UC2["UC02: Tìm kiếm mã chứng khoán"]
        UC3["UC03: Xem dữ liệu & Báo cáo tài chính"]
        UC4["UC04: Xem chỉ báo kỹ thuật (MA20/RSI14)"]
        UC5["UC05: Quản lý Danh mục (Portfolio)"]
        UC6["UC06: Quản lý Watchlist"]
        UC7["UC07: Nhận Email cảnh báo biến động"]
        UC8["UC08: Xem dự báo xu hướng (AI ML Output)"]
        UC9["UC09: Quản lý Người dùng & Phân quyền"]
        UC10["UC10: Đồng bộ dữ liệu tự động"]
    end

    INV --> UC1
    INV --> UC2
    INV --> UC3
    INV --> UC4
    INV --> UC5
    INV --> UC6
    INV --> UC7
    INV --> UC8

    ADM --> UC1
    ADM --> UC9
    ADM --> UC10

    FDP --> UC10
```

---

### 4. Sơ đồ C4 Model Level 1: System Context Diagram

```mermaid
graph TD
    subgraph External Actors
        INV["Investor / Nhà đầu tư"]
        ADM["Administrator / Quản trị viên"]
        FDP["Financial Data Providers (VNStock, HOSE, HNX)"]
    end

    subgraph Central Platform Boundary
        SYS["Hệ thống Quản lý & Phân tích Dữ liệu Tài chính (AWS Serverless System)"]
    end

    INV -->|Thông tin đăng nhập, yêu cầu tìm kiếm, portfolio| SYS
    SYS -->|Dữ liệu Dashboard, chỉ số kỹ thuật, Email cảnh báo| INV

    ADM -->|Cấu hình hệ thống, kích hoạt sync, quản lý user| SYS
    SYS -->|Trạng thái pipeline, log giám sát, cảnh báo chi phí| ADM

    SYS -->|Request API cào dữ liệu theo lịch| FDP
    FDP -->|Dữ liệu thô OHLCV, báo cáo tài chính, tin tức| SYS
```

#### Ma trận tương tác dữ liệu (System Context Data Matrix)

| Actor / Hệ thống | Dữ liệu gửi đến Hệ thống | Dữ liệu nhận từ Hệ thống |
| :--- | :--- | :--- |
| **Investor** | Thông tin đăng nhập, Ticker tìm kiếm, Cập nhật Watchlist & Portfolio | Dữ liệu Dashboard, Báo cáo tài chính, Chỉ số MA20/RSI14, Email cảnh báo |
| **Administrator** | Cấu hình Ingestion, Kích hoạt sync thủ công, Phân quyền user | Log truy vết CloudWatch, Trạng thái chạy Glue Job, Cảnh báo AWS Budgets |
| **Data Providers** | Payload JSON thô, file báo cáo tài chính, tin tức doanh nghiệp | Các HTTPS Request đặt lịch từ AWS Lambda Collector |
