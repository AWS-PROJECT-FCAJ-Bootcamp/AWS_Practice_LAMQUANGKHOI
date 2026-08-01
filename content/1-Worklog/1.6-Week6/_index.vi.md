---
title: "Worklog Tuần 6"
date: 2026-07-27
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

* Pitching ý tưởng đồ án với các mentor để nhận phản hồi và điều chỉnh sơ đồ kiến trúc AWS phù hợp thực tế.
* Nghiên cứu và đánh giá các dịch vụ AWS cốt lõi bao gồm AWS Amplify, S3, Glue Crawler, Glue Data Catalog, Athena, Lambda Worker và EventBridge.
* Thực hành bài lab và triển khai thử nghiệm AWS Amplify hosting, WAF, phân quyền IAM tối thiểu và tự động hóa EventBridge.
* Thiết kế luồng dữ liệu kép cho hệ thống: luồng tự động cập nhật báo cáo hàng ngày qua EventBridge và luồng truy vấn trực tiếp qua Amazon Athena theo request người dùng.
* Áp dụng 6 trụ cột AWS Well-Architected Framework để tối ưu hóa bảo mật, chi phí và hiệu năng hệ thống.
* Phân chia lại vai trò công việc trong nhóm (FE, BE, CI/CD, Xử lý dữ liệu, Giám sát hệ thống) và cập nhật sơ đồ kiến trúc phân lớp.
* Duy trì tiến độ viết báo cáo nghiên cứu khoa học journal và chủ động lên phương án ứng phó với hạn chế credit AWS bị reset đầu tháng.

### Các công việc đã triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Pitching ý tưởng đề tài của nhóm cho các anh mentor để thử sai và nhận phản hồi điều chỉnh.<br>- Tìm hiểu 15 dịch vụ AWS chính có liên quan (Amplify, S3, Glue Crawler, Glue Data Catalog, Athena, Lambda Worker,...) và công cụ Tiny Fish (crawl fetch & search).<br>- Hoàn thành một nửa tiến độ cài đặt AWS Amplify và chuẩn bị môi trường thực thi. | 27/07/2026 | 27/07/2026 | |
| 3 | - Triển khai thành công bài lab Amplify và nhận hướng dẫn từ anh Minh về việc sử dụng Langfuse để kiểm tra dataset.<br>- Tham khảo sơ đồ từ workshop của anh Minh để tinh chỉnh luồng dữ liệu khép kín cho hệ thống production.<br>- Rà soát bản chất và phân bổ lại 6 service AWS chính (Amplify, S3, Glue Crawler, Glue Data Catalog, Athena, Lambda Worker) nhằm tối ưu chi phí.<br>- Lập kế hoạch phân quyền IAM tối thiểu và phân chia lại công việc chi tiết cho nhóm (Vỹ - FE, Phong - BE, Khôi - CI/CD, Dương - Data, Tuấn Anh - Monitoring). | 28/07/2026 | 28/07/2026 | |
| 4 | - Hosting hệ thống qua AWS Amplify, cài đặt WAF phân quyền và hoàn thiện các policy IAM theo từng vai trò công việc.<br>- Trao đổi với anh Quinler về luồng ML prediction training, tối ưu chi phí và áp dụng 6 trụ cột AWS Well-Architected (Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization, Sustainability).<br>- Thực hiện ước lượng chi phí hệ thống trên AWS Pricing Calculator và chuẩn bị đẩy hệ thống lên quản lý IAM. | 29/07/2026 | 29/07/2026 | <https://calculator.aws/> |
| 5 | - Nghiên cứu và thực hành Amazon Athena cùng AWS EventBridge để ứng dụng kéo dữ liệu theo 2 luồng: (1) luồng tự động cập nhật hàng ngày bằng EventBridge lên dashboard, (2) luồng kéo Athena trực tiếp theo request của người dùng.<br>- Hoàn thành bài lab hands-on về AWS EventBridge. | 30/07/2026 | 30/07/2026 | <https://youtu.be/77zSXuFs4GA?si=Us_XR5-ogDYW77hj> |
| 6 | - Gửi sơ đồ kiến trúc lên nhóm SGU để nhận review và điều chỉnh phân chia các Layer để bài trình bày dễ nhìn hơn.<br>- Đẩy mạnh tiến độ nghiên cứu khoa học: tìm hiểu cách FinRL xử lý chỉ báo kỹ thuật trước khi huấn luyện, tham khảo các bài báo RDX và phát triển phương pháp XAI mới. | 31/07/2026 | 31/07/2026 | |
| 7 | - Thử nghiệm các dịch vụ chính trong AWS pipeline và giao Tuấn Anh + Dương vẽ lại sơ đồ kiến trúc theo dạng layer để gửi lại nhóm.<br>- Hoàn thành tổng hợp tài liệu báo cáo nghiên cứu khoa học journal.<br>- Ghi nhận khó khăn khi toàn bộ credit AWS bị reset về 0 ngày 01/08 và lập phương án tạo tài khoản mới để tiếp tục chạy lab. | 01/08/2026 | 01/08/2026 | |

### Kết quả đạt được tuần 6:

* Trình bày thành công ý tưởng đồ án với các mentor (anh Khiêm, anh Lực, anh Minh, anh Quinler) và tiếp thu các góp ý thiết thực để tinh chỉnh kiến trúc.
* Triển khai thành công AWS Amplify hosting, tích hợp bộ lọc bảo mật WAF và kết nối Langfuse phục vụ kiểm thử dữ liệu/dataset.
* Làm chủ tính chất và cách phối hợp giữa các dịch vụ trọng tâm: Amplify, S3, Glue Crawler, Glue Data Catalog, Athena, Lambda Worker và EventBridge.
* Thiết kế hoàn chỉnh luồng xử lý dữ liệu kép kết hợp giữa tự động hóa EventBridge và truy vấn Athena theo nhu cầu.
* Xây dựng khung phân quyền IAM tối thiểu (least privilege) và vận dụng 6 trụ cột AWS Well-Architected Framework vào quản trị chi phí & an toàn hệ thống.
* Tối ưu hóa phân công công việc cho từng thành viên trong team (FE, BE, CI/CD, Xử lý dữ liệu, Giám sát hệ thống) và bắt đầu hoàn thiện sơ đồ phân lớp.
* Đạt tiến triển tích cực trong nghiên cứu khoa học song song về xử lý chỉ báo kỹ thuật FinRL và mô hình giải thích XAI.
* Nhận diện và phản ứng kịp thời với sự cố reset credit AWS đầu tháng bằng kế hoạch khởi tạo tài khoản dự phòng.
