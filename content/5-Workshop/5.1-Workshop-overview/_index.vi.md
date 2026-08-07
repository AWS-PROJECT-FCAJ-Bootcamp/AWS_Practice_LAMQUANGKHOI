---
title : "Giới thiệu"
date : 2024-01-01 
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

# GIỚI THIỆU VỀ AWS VPC ENDPOINT & ARCHITECTURE

#### Giới thiệu về VPC Endpoint trong Hệ thống Tài chính
* **VPC Endpoints** là các thiết bị ảo được hạ tầng AWS quản lý hoàn toàn, có khả năng mở rộng theo chiều ngang, tự động dự phòng và đạt độ sẵn sàng cao (High Availability). VPC Endpoints cho phép tài nguyên điện toán trong hệ thống của nhóm mình giao tiếp riêng tư với các dịch vụ AWS như **Amazon S3** mà không làm lộ lưu lượng ra ngoài Internet công cộng.
* **Tài nguyên điện toán trong VPC** (EC2, Lambda, Glue ETL) truy cập Amazon S3 Data Lake an toàn thông qua **Gateway VPC Endpoint**.
* **Tài nguyên từ trung tâm dữ liệu On-premises** hoặc môi trường lai (Hybrid Cloud) kết nối tới S3 Data Lake thông qua **Interface VPC Endpoint (AWS PrivateLink)**.

#### Mô hình kiến trúc kiểm thử của Workshop
Trong bài workshop này, nhóm mình thiết lập và vận hành cấu trúc mô phỏng gồm hai VPC chính:
* **"VPC Cloud"**: Đại diện cho hạ tầng điện toán đám mây chứa Amazon S3 Data Lake (`S3 Raw` & `S3 Curated Bucket`), Gateway Endpoint và các EC2 instance kiểm thử tính toán chỉ số tài chính.
* **"VPC On-Prem"**: Mô phỏng trung tâm dữ liệu truyền thống của doanh nghiệp. Một EC2 Instance chạy phần mềm strongSwan VPN được khởi tạo trong "VPC On-prem" và tự động thiết lập đường hầm **Site-to-Site VPN** với AWS Transit Gateway để kết nối riêng tư vào VPC Cloud.

![Sơ đồ tổng quan kiến trúc bài lab VPC Endpoint](/images/5-Workshop/5.1-Workshop-overview/diagram1.png)
