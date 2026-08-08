---
title: "Worklog Tuần 7"
date: 2026-08-03
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

# NHẬT KÝ CÔNG VIỆC TUẦN 7: HOÀN THIỆN PIPELINE TỰ ĐỘNG, AWS STEP FUNCTIONS, AMPLIFY HOSTING VÀ BÁO CÁO TỔNG KẾT

### Mục tiêu tuần 7:
* Họp nhóm phân chia nhiệm vụ nhỏ, siết chặt deadline deploy và kiểm soát chi phí thực tế qua AWS Cost Explorer.
* Phân quyền IAM tài khoản cho thành viên, lắng nghe bài trình bày từ Swimburne Team để rút ra bài học về xác định người dùng, bảo mật và cân đối chi phí nền tảng.
* Cấu hình AWS Systems Manager (SSM) Parameter Store, quy định trường dữ liệu S3 Raw cho thông tin giá thị trường (Market Price) và chuẩn hóa dữ liệu trước khi chạy Glue Job.
* Thiết lập và kiểm thử quy trình điều phối dữ liệu (Data Pipeline Orchestration) bằng **AWS Step Functions (State Machine)** kết hợp AWS Lambda, AWS Glue Job và Glue Crawler.
* Tự động hóa luồng Ingestion bằng **AWS EventBridge** (kích hoạt định kỳ hàng ngày sau giờ đóng cửa giao dịch hoặc theo nhu cầu on-demand).
* Phối hợp triển khai trang Web Dashboard lên **AWS Amplify Hosting** với domain riêng: `https://feature-dashboard.dgku51j8dnv70.amplifyapp.com/login/`.
* Hoàn thiện toàn bộ báo cáo cá nhân (Blog Report, Worklog, Các sự kiện tham gia, Bài tự đánh giá và Ý kiến phản hồi FCAJ).

---

### Các công việc đã triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| :---: | :--- | :---: | :---: | :---: |
| 2 | - Họp nhóm phân chia chi tiết các đầu việc nhỏ và đẩy nhanh deadline deploy lên hệ thống AWS.<br>- Chấp nhận chi phí thử nghiệm thực tế khi deploy và chủ động theo dõi, phân tích chi phí qua công cụ **AWS Cost Explorer / Cost Analyze**. | 03/08/2026 | 03/08/2026 | <https://aws.amazon.com/aws-cost-management/aws-cost-explorer/> |
| 3 | - Phân quyền tài khoản IAM với đầy đủ chính sách phù hợp cho từng thành viên trong nhóm.<br>- Nghiên cứu cơ chế dịch vụ SSI FastConnectAPI (chỉ hỗ trợ truy cập khi kết nối tại trụ sở SSI).<br>- Tham dự buổi thuyết trình của **Swimburne Team** để học hỏi kinh nghiệm: xác định đối tượng người dùng từ đầu giúp tối ưu kế hoạch scaling, nhận thức sâu sắc về chi phí & bảo mật giữa các nền tảng (Web triển khai nhanh, Mobile chú trọng UX).<br>- Đánh giá tiến độ MVP và khả năng mở rộng quy mô đồ án theo ứng dụng thực tế. | 04/08/2026 | 04/08/2026 | |
| 4 | - Áp dụng thành công luồng thu thập dữ liệu (Ingestion) và chạy thử nghiệm dữ liệu trên AWS trước khi đẩy luồng CI/CD lên GitHub.<br>- Cấu hình **AWS SSM Parameter Store** để quản lý an toàn biến môi trường.<br>- Quy định lại các trường dữ liệu trên S3 Raw: tập trung tối đa vào dữ liệu Giá thị trường (Market Price), tạm thời giản lược Financial Reports để đảm bảo tiến độ. | 05/08/2026 | 05/08/2026 | <https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-parameter-store.html> |
| 5 | - Đẩy mạnh hoàn thiện luồng Pipeline Service trên AWS.<br>- Thiết lập và vận hành sơ đồ điều phối dữ liệu bằng **AWS Step Functions**: `Start` ➔ `Lambda: Ingest_Financial_Data` ➔ `Glue: StartJobRun` ➔ `Glue: StartCrawler` ➔ `Wait for Crawler` ➔ `Check Crawler Status` ➔ `Choice: Is Crawler Ready?` ➔ `Succeeded / Failed`.<br>- Chuẩn hóa định dạng dữ liệu đẩy vào S3 Lake và chuẩn bị các bước đẩy Frontend/Backend lên AWS. | 06/08/2026 | 06/08/2026 | <https://aws.amazon.com/step-functions/> |
| 6 | - Tự động hóa hoàn toàn Pipeline bằng **AWS EventBridge** (kích hoạt hàng ngày sau giờ đóng cửa giao dịch hoặc theo demand).<br>- Điều phối triển khai trang web lên **AWS Amplify Hosting** thành công với domain riêng (`https://feature-dashboard.dgku51j8dnv70.amplifyapp.com/login/`).<br>- Tổng hợp và hoàn thiện toàn bộ báo cáo cá nhân (Workshop, Sự kiện đã tham gia, Tự đánh giá, Feedback FCAJ) và nộp sản phẩm hoàn chỉnh lên hệ thống. | 07/08/2026 | 07/08/2026 | `https://feature-dashboard.dgku51j8dnv70.amplifyapp.com/login/` |

---

### Kết quả đạt được tuần 7:

* **Tự động hóa luồng Pipeline dữ liệu**: Xây dựng thành công quy trình thu thập và xử lý dữ liệu tài chính khép kín bằng AWS Step Functions, Lambda, Glue và EventBridge.
* **Triển khai Web Dashboard Production**: Đưa ứng dụng Web lên AWS Amplify với domain riêng hoạt động ổn định và sẵn sàng cho người dùng.
* **Quản trị hệ thống & Cấu hình an toàn**: Phân quyền IAM hợp lý cho thành viên và bảo mật biến môi trường bằng AWS SSM Parameter Store.
* **Tối ưu hóa quy trình & Chi phí**: Học hỏi kinh nghiệm thiết kế từ Swimburne Team, rà soát chi phí thực tế qua AWS Cost Explorer và điều chỉnh phạm vi dữ liệu tập trung đúng trọng tâm.
* **Hoàn thiện tài liệu báo cáo**: Đầy đủ nhật ký làm việc 7 tuần, báo cáo workshop, bài thu hoạch sự kiện, tự đánh giá và ý kiến đóng góp cho chương trình FCAJ.
