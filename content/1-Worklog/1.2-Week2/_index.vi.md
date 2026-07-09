---
title: "Worklog Tuần 2"
date: 2026-06-30
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2:

* Làm quen với các thành viên, xác định vai trò và định hướng đề tài của nhóm.
* Thiết lập môi trường làm việc chung trên AWS, GitHub và các công cụ cộng tác.
* Xây dựng bản nháp PRD, phạm vi MVP và kiến trúc ban đầu cho ứng dụng tài chính.
* Tìm hiểu AWS Organizations, Service Control Policies và phương án quản lý tài nguyên chung.
* Thực hành S3, CloudFront và Lambda.
* Chuẩn bị PoC, backlog và kế hoạch phân chia công việc cho giai đoạn tiếp theo.

### Các công việc đã triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 3 | - Tổ chức cuộc họp đầu tiên để các thành viên làm quen và trao đổi thông tin liên lạc.<br>- Thảo luận hai ý tưởng ban đầu liên quan đến Terraform, quản lý dịch vụ AWS và ứng dụng tài chính.<br>- Ghi nhận định hướng vai trò của từng thành viên: BA, Backend, DevOps và DevOps/Backend.<br>- Lên kế hoạch mở biểu mẫu để chốt mong muốn đề tài của cả nhóm. | 30/06/2026 | 30/06/2026 | |
| 4 | - Thiết lập môi trường dùng chung cho nhóm trên AWS và GitHub.<br>- Hoàn thành tutorial AWS Organizations và thử nghiệm SCP ngăn hành động terminate EC2 instance.<br>- Xác định nhu cầu phân chia Organizational Unit và IAM theo nhiệm vụ.<br>- Tạo GitHub Organization, phân quyền thành viên và chuẩn bị repository cho từng người.<br>- Khảo sát Kiro và các vấn đề liên quan đến gói thanh toán.<br>- Học quy tắc vẽ kiến trúc AWS chuyên nghiệp bằng draw.io.<br>- Chuẩn bị thiết kế kiến trúc phiên bản đầu tiên cho ứng dụng tài chính. | 01/07/2026 | 01/07/2026 | <https://youtu.be/l8isyDe-GwY?si=4i3f4a_yzHV49BpJ> |
| 5 | - Kiểm tra lại chi phí tài khoản AWS sau khi gói trải nghiệm cá nhân kết thúc.<br>- Xác định nhu cầu giới hạn thời gian chạy tài nguyên và phân công người trực hệ thống.<br>- Xây dựng các mục cơ bản của PRD và điền thông tin proposal ban đầu.<br>- Tìm hiểu thêm về Lambda, Athena, S3, RDS và DynamoDB. | 02/07/2026 | 02/07/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 6 | - Không có báo cáo công việc được ghi nhận trong tài liệu nguồn. | 03/07/2026 | 03/07/2026 | |
| 7 | - Hoàn thành bài lab Amazon S3.<br>- Sử dụng CloudFront để phân phối nội dung website và tìm hiểu phương thức truy cập nội dung có bảo mật.<br>- Tổng hợp tài liệu AWS cơ bản cho các thành viên trong nhóm.<br>- Xác định các chủ đề cần học: S3, Glue, Athena, Lambda, IAM, VPC và EC2.<br>- Tạo workspace cộng tác cho nhóm.<br>- Chuẩn bị nội dung cuộc họp tiếp theo về PoC, PRD, roadmap, vai trò và deadline. | 04/07/2026 | 04/07/2026 | <https://youtu.be/HxYZAK1coOI?si=ebw8q1duETpwx8jp><br><https://youtu.be/pjr5a-HYAjI?si=IMDdKh2EmoGCnPRs><br><https://youtu.be/2PQYqH_HkXw?si=d2Lo19XCkeqi4jLV><br><https://youtu.be/IY61YlmXQe8?si=BQab3AHWW1GtDhj1> |
| CN | - Hoàn thành trang tài liệu artifact cho nhóm.<br>- Tạo repository sử dụng Hugo theme để đăng tải worklog.<br>- Lập kế hoạch họp phân chia task và bắt đầu PoC trên GitHub trước khi triển khai lên AWS.<br>- Kiểm tra lại trạng thái AWS Organization và nguồn credit của nhóm.<br>- Xây dựng backlog tuần đầu, gồm chốt scope MVP, tạo cấu trúc repository, vẽ kiến trúc, khảo sát nguồn dữ liệu, thử collector local, thiết kế data lake path, mapping dữ liệu, xác định feature/label, dựng Terraform skeleton và checklist bảo mật/chi phí. | 05/07/2026 | 05/07/2026 | |
| 2 | - Tổ chức cuộc họp phân chia công việc và phân tích yêu cầu đồ án.<br>- Thống nhất các công cụ dùng chung cho nhóm.<br>- Hoàn thành nội dung học "Serverless with Lambda".<br>- Xác định các trường dữ liệu tài chính cần chú ý như OHLCV và chỉ báo kỹ thuật.<br>- Chốt công nghệ lập trình tuần đầu, chuẩn bị tài khoản thu thập dữ liệu và tiếp tục đánh giá phương án dùng Kiro chung. | 06/07/2026 | 06/07/2026 | <https://cloudjourney.awsstudygroup.com/> |

### Kết quả đạt được tuần 2:

* Tổ chức thành công cuộc họp khởi động và xác định sơ bộ vai trò của các thành viên trong nhóm.
* Thiết lập GitHub Organization, phân quyền thành viên và định hướng repository cho tài liệu, mã nguồn, hạ tầng và worklog.
* Thử nghiệm AWS Organizations và Service Control Policy để hạn chế thao tác nguy hiểm trên tài nguyên EC2.
* Xác định yêu cầu tiếp tục nghiên cứu Organizational Unit, IAM và Resource Explorer để quản lý tài nguyên nhóm tốt hơn.
* Hoàn thiện bản nháp ban đầu của PRD, proposal, phạm vi MVP và kiến trúc luồng dữ liệu cho ứng dụng tài chính.
* Nắm được các quy tắc cơ bản khi vẽ sơ đồ kiến trúc AWS bằng draw.io, gồm AWS Cloud, Region, VPC, Availability Zone, subnet và quy chuẩn icon.
* Hoàn thành bài lab S3, kết nối S3 với CloudFront và hiểu thêm về phân phối nội dung website có kiểm soát truy cập.
* Hoàn thành nội dung cơ bản về mô hình serverless với AWS Lambda.
* Tổng hợp tài liệu học chung và xác định các dịch vụ trọng tâm: S3, Glue, Athena, Lambda, IAM, VPC, EC2, RDS và DynamoDB.
* Tạo trang artifact và repository Hugo để công bố worklog của nhóm.
* Xây dựng backlog PoC tuần đầu với output, người phụ trách và các hạng mục rõ ràng.
* Nhận diện rủi ro chi phí AWS và thống nhất cần giới hạn thời gian chạy, theo dõi ngân sách và phân công người quản lý tài nguyên.
