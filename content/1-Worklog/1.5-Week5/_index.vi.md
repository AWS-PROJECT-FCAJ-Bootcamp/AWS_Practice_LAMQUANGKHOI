---
title: "Worklog Tuần 5"
date: 2026-07-20
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5:

* Hoàn thiện portal AWS Organization, Organizational Unit và IAM role để nhóm sử dụng tài nguyên chung.
* Đánh giá lại phương án subscription Kiro và lựa chọn quy trình làm việc phù hợp cho nhóm.
* Làm rõ định hướng đồ án thông qua việc phân biệt database, data warehouse, data lake và data lakehouse.
* Ghép mã nguồn PoC, chuẩn hóa môi trường phát triển và mở rộng hoạt động kiểm thử tự động.
* Review frontend và backend, xác định rủi ro bảo mật và hoàn thiện bản nháp kiến trúc AWS để gửi mentor.

### Các công việc đã triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Thiết lập portal AWS Organization của nhóm với các Organizational Unit thử nghiệm và role theo kiến trúc hiện tại.<br>- Hoàn thành quyền truy cập cho tài khoản leader administrator và role chỉ xem workload.<br>- Xem xét lại cấu hình Kiro sau khi đã hình thành đầy đủ cấu trúc group cần thiết.<br>- Tham khảo kiến trúc data lake chuẩn và các giao diện thường gặp. | 20/07/2026 | 20/07/2026 | <https://www.projectpro.io/article/how-to-build-a-data-lake/1071> |
| 3 | - Hoàn thành cấu hình IAM chính để nhóm chia sẻ tài nguyên trong AWS Organization.<br>- Gửi yêu cầu đến Support về lỗi Kiro subscription và xác nhận tài khoản không đủ điều kiện tạo organization subscription.<br>- Loại Kiro khỏi phương án công cụ dùng chung và bắt đầu cân nhắc một quy trình phù hợp hơn.<br>- Phân biệt database, data warehouse và data lake theo OLTP, OLAP, ETL, schema, phương thức lưu trữ và use case phân tích.<br>- Lên kế hoạch vẽ lại kiến trúc AWS dựa trên nghiên cứu data lake mới và yêu cầu của lĩnh vực tài chính. | 21/07/2026 | 21/07/2026 | <https://youtu.be/-bSkREem8dM?si=R5y_AqhNa39WvKPK> |
| 4 | - Phân tích framework data lake mẫu của AWS để tham khảo cách trình bày kiến trúc mục tiêu.<br>- Ghép mã nguồn frontend và backend hiện có thành một PoC có thể chạy nhưng chưa hoàn chỉnh.<br>- Thống nhất môi trường phát triển thông qua `uv.lock`, `.gitignore` và các yêu cầu thư viện.<br>- Xác nhận PoC hiện tại đáp ứng quy mô ban đầu khoảng 100 mã cổ phiếu, trong khi mức độ hoàn thiện của demo đạt khoảng 50%.<br>- Ghi nhận các lỗi frontend, phần backend chưa được kiểm thử đầy đủ và yêu cầu module hóa giao diện.<br>- Khảo sát thêm các nhà cung cấp dữ liệu tài chính để tăng độ đa dạng và độ tin cậy của nguồn dữ liệu. | 22/07/2026 | 22/07/2026 | <https://github.com/aws-solutions-library-samples/data-lakes-on-aws><br><https://dichvu.vietstock.vn/du-lieu-tai-chinh/datafeed---du-lieu-tai-chinh-tich-hop-chuyen-nghiep><br><https://youtu.be/CxQJUbdoxt4?si=esU0_gkQo9Tw4RE3><br><https://www.dnse.com.vn/><br><https://www.ssi.com.vn/khach-hang-ca-nhan/fast-connect-api><br><https://fiingroup.vn/vi/giai-phap-du-lieu-chung-khoan-api-datafeed-microsite.html> |
| 5 | - Tiếp tục chỉnh sửa PoC gần nhất và chuẩn bị phương án kiểm thử cho pipeline dữ liệu chính.<br>- Xác định yêu cầu kiểm thử theo từng module frontend và backend, bao gồm luồng chức năng, use case, độ trễ và thực thi CI.<br>- Viết các bài test ban đầu cho pipeline chính từ nguồn dữ liệu vào hệ thống.<br>- Ghi nhận frontend vẫn còn lỗi và một số phần backend chưa được bao phủ kiểm thử toàn diện.<br>- Chuẩn bị danh sách task mới để trình bày đồ án với các mentor. | 23/07/2026 | 23/07/2026 | <https://github.com/aws-solutions-library-samples/data-lakes-on-aws> |
| 6 | - Review giao diện mới và các thay đổi mã nguồn do một thành viên trong nhóm thực hiện.<br>- Phân chia lại công việc theo vai trò và ghi nhận các vấn đề backend, đặc biệt là rủi ro data injection và search injection.<br>- Hoàn thành khoảng 80% sơ đồ kiến trúc AWS.<br>- Xác định yêu cầu mở rộng nguồn dữ liệu theo API và các kiểu dữ liệu đầu vào.<br>- Điều chỉnh định hướng mục tiêu sang kiến trúc data lakehouse và sắp xếp lại các heading của tài liệu AWS để nhóm cùng thực hiện.<br>- Chốt deadline gửi kiến trúc vào ngày 25/07. | 24/07/2026 | 24/07/2026 | <https://github.com/aws-solutions-library-samples/data-lakes-on-aws> |

### Kết quả đạt được tuần 5:

* Hoàn thiện cấu hình chính của portal AWS Organization, Organizational Unit và IAM để nhóm dùng chung tài nguyên.
* Xác nhận tài khoản hiện tại không thể tạo Kiro organization subscription và loại Kiro khỏi toolchain dự kiến.
* Làm rõ sự khác nhau giữa database, data warehouse và data lake, từ đó điều chỉnh định hướng kiến trúc đồ án.
* Ghép mã nguồn PoC và chuẩn hóa môi trường phát triển bằng `uv.lock`, `.gitignore` cùng các khai báo dependency.
* Đạt được PoC có thể chạy với quy mô ban đầu khoảng 100 mã cổ phiếu, đồng thời ghi nhận rõ các phần chưa hoàn thiện.
* Viết các bài test ban đầu cho pipeline dữ liệu chính và xác định yêu cầu mở rộng độ bao phủ cho frontend và backend.
* Khảo sát thêm các nhà cung cấp dữ liệu tài chính và ghi nhận sự đánh đổi giữa độ bao phủ, độ tin cậy và chi phí dịch vụ.
* Review giao diện và backend, ghi nhận các rủi ro liên quan đến injection và phân chia lại công việc theo vai trò.
* Hoàn thành khoảng 80% sơ đồ AWS và điều chỉnh kiến trúc mục tiêu theo hướng data lakehouse.
