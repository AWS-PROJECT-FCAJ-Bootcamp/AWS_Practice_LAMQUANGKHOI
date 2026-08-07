---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# TRUY CẬP AN TOÀN S3 DATA LAKE TÀI CHÍNH BẰNG AWS VPC ENDPOINT & PRIVATELINK

### Tổng quan bài thực hành Workshop
Trong kiến trúc **Hệ thống Thu thập & Phân tích Rủi ro Tài chính Doanh nghiệp Việt Nam** do nhóm mình phát triển, **Amazon S3 Data Lake** đóng vai trò là kho lưu trữ trung tâm chứa toàn bộ dữ liệu báo cáo tài chính thô (`S3 Raw Bucket`) và các tập chỉ số tài chính đã qua làm sạch (`S3 Curated Bucket`). Để bảo mật tuyệt đối các dữ liệu tài chính nhạy cảm và ngăn chặn hoàn toàn nguy cơ rò rỉ dữ liệu qua môi trường mạng ngoài, nhóm mình đã triển khai giải pháp kết nối riêng tư **AWS VPC Endpoints & AWS PrivateLink**.

Giải pháp này cho phép các tác vụ tính toán (EC2, AWS Lambda, AWS Glue Job) nằm trong VPC hoặc các hệ thống từ trung tâm dữ liệu (On-premises) có thể truy cập trực tiếp vào Amazon S3 Data Lake thông qua hạ tầng mạng nội bộ của AWS mà **không cần đi qua Internet công cộng**.

Trong bài workshop này, nhóm mình tổng hợp và hướng dẫn chi tiết các bước khởi tạo, cấu hình và kiểm thử 2 loại VPC Endpoints chính:
* **Gateway VPC Endpoint**: Định tuyến trực tiếp lưu lượng từ các Subnet trong VPC tới Amazon S3 thông qua Bảng định tuyến (Route Table). Giải pháp này giúp tối ưu tốc độ truyền tải dữ liệu tài chính, đảm bảo an toàn tuyệt đối và không phát sinh thêm chi phí dịch vụ.
* **Interface VPC Endpoint (AWS PrivateLink)**: Cung cấp các điểm cuối mạng ảo (ENI) sở hữu địa chỉ IP riêng trong VPC. Giải pháp này cho phép các hệ thống từ môi trường truyền thống (On-premises) hoặc mạng đối tác kết nối an toàn tới S3 Data Lake thông qua tên miền DNS riêng tư.

---

### Danh mục bài thực hành

1. **[5.1. Giới thiệu & Tổng quan Workshop](5.1-workshop-overview/)**
   * Giới thiệu khái niệm VPC Endpoint, so sánh Gateway Endpoint vs Interface Endpoint và kiến trúc tổng quan bài lab.
2. **[5.2. Các bước chuẩn bị môi trường](5.2-prerequiste/)**
   * Khởi tạo hạ tầng VPC, các Subnet, máy chủ EC2 kiểm thử và S3 Bucket lưu trữ dữ liệu.
3. **[5.3. Truy cập Amazon S3 từ mạng VPC](5.3-s3-vpc/)**
   * Tạo Gateway VPC Endpoint, cập nhật Route Table và kiểm tra truy vấn S3 an toàn từ EC2 trong Private Subnet.
4. **[5.4. Truy cập Amazon S3 từ môi trường truyền thống (On-premises)](5.4-s3-onprem/)**
   * Tạo S3 Interface Endpoint, cấu hình mô phỏng DNS On-premises và kiểm tra kết nối PrivateLink.
5. **[5.5. Kiểm soát quyền chi tiết với VPC Endpoint Policies](5.5-policy/)**
   * Cấu hình Endpoint Policy nhằm giới hạn quyền truy cập chỉ cho phép đọc/ghi vào đúng S3 Bucket của dự án.
6. **[5.6. Dọn dẹp tài nguyên](5.6-cleanup/)**
   * Hướng dẫn dọn dẹp các tài nguyên AWS đã tạo sau khi hoàn tất bài lab để tránh phát sinh chi phí.