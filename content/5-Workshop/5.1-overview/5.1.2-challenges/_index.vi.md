---
title: "Thách thức và khó khăn"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.1.2. </b> "
---

# 5.1.2. Thách thức và khó khăn

### 1. Các thách thức kỹ thuật & vận hành
Quá trình thiết kế và triển khai hệ thống Thu thập & Phân tích Dữ liệu Tài chính trên AWS gặp phải các thách thức cốt lõi sau:

1. **Sự không đồng nhất của nguồn dữ liệu & Rào cản truy xuất**:
   * Mỗi nguồn dữ liệu tài chính (VNStock, CafeF, SSI iBoard, Fireant, HOSE, HNX) có cấu trúc định dạng hoàn toàn khác nhau (JSON API, HTML scraping, file Excel/PDF).
   * Giới hạn tần suất gọi API (Rate limit), nguy cơ lỗi kết nối, thay đổi cấu trúc API đột ngột từ nhà cung cấp nguồn.
   * Rủi ro pháp lý và tuân thủ điều khoản sử dụng (ToS) khi cào dữ liệu quy mô lớn.
2. **Thách thức tương thích thư viện Cào dữ liệu trên AWS & Chuyển đổi sang `yfinance`**:
   * **Không hỗ trợ `VNStock` trên môi trường AWS**: Theo kế hoạch ban đầu, nhóm dự định sử dụng thư viện `VNStock` trên AWS Lambda. Tuy nhiên, khi triển khai thực tế trên môi trường Serverless của AWS, `VNStock` gặp lỗi không tương thích môi trường runtime (thiếu các phụ thuộc native, bị chặn IP Datacenter/Cloudflare từ nhà cung cấp nguồn khi gọi từ AWS Lambda).
   * **Chuyển đổi sang thư viện `yfinance`**: Nhóm đã chủ động điều chỉnh kiến trúc, chuyển đổi sang sử dụng thư viện `yfinance` (Yahoo Finance API) để thu thập dữ liệu giá OHLCV chứng khoán Việt Nam (các mã dạng `VIC.VN`, `VNM.VN`, `FPT.VN`...).
   * **Hạn chế độ sâu dữ liệu lịch sử**: Do chuyển sang `yfinance`, dữ liệu lịch sử không thể trích xuất sâu về các năm quá cũ hơn so với dự kiến ban đầu, buộc hệ thống phải điều chỉnh khung thời gian thu thập dữ liệu lịch sử phù hợp với giới hạn của API mới.
3. **Môi trường làm việc chung & Quản trị bảo mật**:
   * Cấu hình môi trường AWS dùng chung cho các thành viên trong nhóm mà không vi phạm nguyên tắc bảo mật chuẩn doanh nghiệp.
   * Áp dụng nghiêm ngặt quyền tối thiểu IAM Least-Privilege, phân tách môi trường Dev/Demo và tuyệt đối không hard-code API Key/Secrets.
4. **Quản lý chi phí & Kiểm soát ngân sách đám mây**:
   * Vừa phải vận hành tự động pipeline hàng ngày, vừa phục vụ kiểm thử liên tục nhưng phải giữ chi phí đám mây ở mức tối thiểu.
   * Yêu cầu thiết lập cơ chế AWS Budgets, CloudWatch Alarms và giới hạn dung lượng truy vấn Athena Workgroup.

---

### 2. Bảng phân tích chi tiết các nguồn cung cấp dữ liệu (Data Providers)

| Nguồn dữ liệu | Phạm vi dữ liệu | Định dạng & Cách truy xuất | Ưu điểm | Nhược điểm & Rủi ro | Đánh giá & Khuyến nghị |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **yfinance** *(Thư viện mới)* | Dữ liệu giá OHLCV lịch sử (mã dạng `XXX.VN`) | Python package gọi Yahoo Finance API | Chạy ổn định trên AWS Lambda, không bị block IP Datacenter | Giới hạn độ sâu dữ liệu lịch sử cũ hơn dự kiến | **Nguồn cào chính (Primary)** thay thế VNStock trên AWS |
| **VNStock** *(Thư viện cũ)* | Giá lịch sử OHLCV, Báo cáo tài chính | Python package (dạng DataFrame / JSON) | Miễn phí, dữ liệu phong phú | Không tương thích môi trường AWS Lambda, rủi ro block IP | **Hạn chế dùng** (dự kiến dùng ở local hoặc server cố định) |
| **SSI iBoard** | Giá real-time / near real-time, dữ liệu khớp lệnh | Web/App, API nội bộ không chính thức | Dữ liệu real-time chất lượng cao từ CTCK hàng đầu | Không có API công khai; rủi ro bị chặn IP cao khi cào tự động | Tránh cào trực tiếp ở MVP; chỉ dùng khi có thỏa thuận API chính thức |
| **Fireant** | Tin tức tài chính, chỉ báo kỹ thuật, diễn đàn | Web API (giới hạn/trả phí) | Tích hợp sẵn tin tức & chỉ báo kỹ thuật | Giới hạn request với tài khoản miễn phí; tin tức cần lọc làm sạch | Nguồn phụ dùng cho tính năng tin tức thị trường |
| **CafeF** | Tin tức tài chính, doanh nghiệp, giá cổ phiếu | Web scraping (HTML parsing) | Cập nhật tin tức nhanh, dữ liệu doanh nghiệp phong phú | Không có API chính thức; cấu trúc HTML thay đổi làm gãy script cào | Cần có cơ chế Retry & Rate-limit để tránh vi phạm điều khoản |
| **HOSE / HNX** | Thông báo doanh nghiệp niêm yết, công bố thông tin | File PDF / Excel trên Portal sàn | Nguồn chính thức sàn giao dịch, độ chuẩn xác 100% | Không có API tự động; dữ liệu dạng file khó tự động hóa | Dùng làm **Nguồn đối chiếu (Ground-Truth)** kiểm tra độ chính xác |

> [!IMPORTANT]
> **Chiến lược chọn nguồn dữ liệu thực tế trên AWS**:
> * **Hạ tầng cào tự động chính**: Nhóm chuyển sang sử dụng thư viện `yfinance` đóng gói trong hàm AWS Lambda Collector cào dữ liệu chứng khoán hàng ngày thay thế cho `VNStock` do rào cản tương thích môi trường AWS Serverless.
> * **Giới hạn dữ liệu**: Khung thời gian thu thập dữ liệu lịch sử được điều chỉnh tối ưu theo giới hạn truy xuất của `yfinance`.
> * **Kiểm tra đối chiếu**: Lấy mẫu kiểm tra định kỳ với thông báo chính thức trên portal `HOSE/HNX`.
