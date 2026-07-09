---
title: "Worklog Tuần 1"
date: 2026-06-23
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Mục tiêu tuần 1:

* Làm quen với các dịch vụ AWS nền tảng: IAM, Budget, VPC, EC2 và CloudWatch.
* Hiểu các thành phần mạng cơ bản trong AWS, bao gồm subnet, Internet Gateway, NAT Gateway, route table, CIDR và Elastic IP.
* Thực hành khởi tạo, kết nối và giám sát EC2 instance.
* Theo dõi chi phí sử dụng AWS và thiết lập cảnh báo ngân sách.
* Khảo sát các dịch vụ cần thiết cho ý tưởng đồ án tài chính trên AWS.

### Các công việc đã triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 3 | - Kích hoạt tài khoản AWS có credit thử nghiệm.<br>- Tạo và thử nghiệm các IAM user với quyền quản trị và vận hành.<br>- Bắt đầu tìm hiểu VPC, subnet, NAT Gateway, Internet Gateway, route table và CIDR. | 23/06/2026 | 23/06/2026 | <https://cloudjourney.awsstudygroup.com/> <https://000002.awsstudygroup.com/vi/1-introduction/> |
| 4 | - Tiếp tục học VPC và EC2.<br>- Thực hành cấu hình VPC, Internet Gateway, NAT Gateway và Elastic IP để EC2 có thể truy cập Internet.<br>- Kết nối từ xa thành công đến EC2.<br>- Thử nghiệm chuyển IAM role cho tài khoản vận hành. | 24/06/2026 | 24/06/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 5 | - Kiểm tra các khoản chi phí phát sinh khi làm lab.<br>- Tạo cảnh báo ngân sách không chi tiêu và ngân sách giới hạn cho giai đoạn học lab.<br>- Thử nghiệm VPC endpoint, NAT Gateway, Elastic IP và đường đi mạng của instance.<br>- Sử dụng Session Manager để truy cập instance không cần SSH.<br>- Làm quen với CloudWatch Monitoring. | 25/06/2026 | 25/06/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 6 | - Hoàn thiện thêm cấu hình EC2.<br>- Phân biệt vai trò của Internet Gateway, NAT Gateway và route table.<br>- Thử nghiệm mô hình Site-to-Site VPN giữa một EC2 mô phỏng phía khách hàng và private network.<br>- Ghi nhận lỗi kết nối VPN mới chỉ hoạt động một chiều và khảo sát Libreswan thay cho thư viện cũ. | 26/06/2026 | 26/06/2026 | <https://docs.aws.amazon.com/vpn/> |
| 7 | - Không làm việc, học tập liên quan đến AWS | 27/06/2026 | 27/06/2026 | |
| CN | - Không làm việc, học tập liên quan đến AWS | 28/06/2026 | 28/06/2026 | |
| 2 | - Tiếp tục bài học EC2 và chuẩn bị tìm hiểu RDS.<br>- Khởi chạy Windows EC2 từ Microsoft AMI.<br>- Ghi nhận sự cố không thể tạo lại mật khẩu để truy cập instance.<br>- Đánh giá tiến độ học EC2 đạt khoảng 40%.<br>- Chuẩn bị xác định đề tài nghiên cứu và tài liệu trình bày cho nhóm. | 29/06/2026 | 29/06/2026 | <https://cloudjourney.awsstudygroup.com/> |

### Kết quả đạt được tuần 1:

* Hiểu được vai trò cơ bản của IAM user, IAM role và nguyên tắc phân quyền khi sử dụng tài khoản AWS.
* Thực hành thành công việc xây dựng kết nối Internet cho EC2 thông qua VPC, public subnet, Internet Gateway, route table và Elastic IP.
* Hiểu rõ hơn sự khác nhau giữa các thành phần mạng:
  * **Internet Gateway:** Cho phép tài nguyên trong public subnet giao tiếp hai chiều với Internet khi được cấu hình route và địa chỉ public phù hợp.
  * **NAT Gateway:** Cho phép tài nguyên trong private subnet truy cập ra Internet mà không mở kết nối chủ động từ Internet vào.
  * **Route table:** Quy định tuyến đường mà lưu lượng mạng trong subnet sẽ sử dụng.
* Truy cập được EC2 bằng phương thức remote thông thường và AWS Systems Manager Session Manager.
* Thử nghiệm CloudWatch để theo dõi trạng thái và chỉ số hoạt động của EC2.
* Thiết lập các cảnh báo ngân sách để kiểm soát chi phí khi thực hành lab.
* Thử nghiệm Site-to-Site VPN và xác định được vấn đề kết nối một chiều cần tiếp tục xử lý.
* Ghi nhận sự cố với Windows EC2 AMI và đưa vào danh sách vấn đề cần nghiên cứu thêm.
* Xây dựng danh sách dịch vụ cần tìm hiểu cho đồ án, gồm S3, RDS, DynamoDB, Lambda, CloudFront, ECS, SQS, SNS, Cognito, CDK, WAF, KMS, Macie và Amplify.
