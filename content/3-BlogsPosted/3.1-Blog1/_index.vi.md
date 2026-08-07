---
title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# HỆ THỐNG THU THẬP VÀ PHÂN TÍCH DỮ LIỆU TÀI CHÍNH CHỨNG KHOÁN VIỆT NAM TRÊN AWS SERVERLESS

Trong quá trình thực hiện dự án phân tích dữ liệu tài chính doanh nghiệp niêm yết trên các sàn chứng khoán Việt Nam (HOSE, HNX, UPCOM), nhóm mình nhận thấy việc thu thập và chuẩn hóa dữ liệu Báo cáo Tài chính từ nhiều nguồn rải rác chính là rào cản lớn nhất. Để giải quyết triệt để bài toán này, nhóm mình đã xây dựng giải pháp tự động hóa toàn bộ quy trình thu thập, xử lý và tính toán chỉ số tài chính dựa trên kiến trúc điện toán đám mây AWS Serverless. Dưới đây là những điểm cốt lõi quan trọng nhất mà nhóm mình muốn chia sẻ để người đọc dễ dàng hình dung và ghi nhớ về hệ thống:

* **Tự động hóa 100% luồng thu thập dữ liệu đa nguồn**: Nhóm mình sử dụng Amazon EventBridge để lập lịch định kỳ theo quý/năm, AWS Step Functions điều phối quy trình cào dữ liệu đa luồng có xử lý lỗi và checkpointing, cùng AWS Lambda / ECS Task đóng gói các script `vnstock` để trích xuất trọn bộ Bảng cân đối kế toán, Báo cáo kết quả kinh doanh, Báo cáo lưu chuyển tiền tệ và giá cổ phiếu từ các nguồn VCI, MAS, KBS về lưu trữ tại Amazon S3 Raw Data Lake.
* **Tiến trình ETL làm sạch và chuẩn hóa dữ liệu tài chính**: Nhóm mình thiết lập AWS Glue Job để tự động loại bỏ nhóm ngành tài chính đặc thù (Ngân hàng, Chứng khoán, Bảo hiểm), đồng bộ hóa hàng trăm biến thể tên chỉ tiêu đa nguồn về chuẩn chung, lọc điều kiện tối thiểu 5 năm dữ liệu liên tục và áp dụng kỹ thuật Winsorization (1%-99%) để loại bỏ các giá trị nhiễu cực đoan.
* **Lưu trữ Data Lake tối ưu và truy vấn tốc độ cao**: Dữ liệu sau khi làm sạch được nhóm mình lưu dưới định dạng Apache Parquet nén Snappy phân vùng theo năm và quý tại S3 Curated bucket. Kết hợp với AWS Glue Data Catalog, hệ thống cho phép Amazon Athena thực thi các truy vấn SQL trực tiếp với tốc độ vượt trội và chi phí tối ưu.
* **Engine tính toán chỉ số và gán nhãn rủi ro kiệt quệ tài chính**: Nhóm mình xây dựng bộ engine tự động tính toán các nhóm chỉ số thanh khoản, sinh lời, đòn bẩy và tăng trưởng; đồng thời gán nhãn rủi ro kiệt quệ tài chính (Distress Labeling) tích hợp cả bộ quy tắc thực tế tại Việt Nam (lỗ lũy kế 2 năm, VCSH âm, EBIT < chi phí lãi vay 2 năm) và mô hình Altman Z-Score cho thị trường mới nổi.
* **Giao diện Web Dashboard và hệ thống cảnh báo tự động**: Toàn bộ dịch vụ được nhóm mình đóng gói qua Amazon API Gateway, xác thực người dùng bằng Amazon Cognito, hiển thị trực quan trên Web Dashboard hosted bởi AWS Amplify, và tự động gửi email thông báo qua Amazon SES ngay khi phát hiện cổ phiếu rơi vào vùng rủi ro kiệt quệ tài chính.

Qua bài viết này, nhóm mình hy vọng mang lại cái nhìn rõ ràng về cách ứng dụng kiến trúc serverless trên AWS để giải quyết bài toán xử lý dữ liệu tài chính quy mô lớn một cách tự động, tối ưu chi phí và đạt độ chính xác cao.

![Sơ đồ kiến trúc hệ thống AWS Serverless](/images/3layer_v1.0.drawio.png)

---

### Các đường liên kết tài nguyên & Bài đăng:
* **Bài đăng chính thức trên AWS Study Group**:
![Bài đăng trên AWS Study Group](/images/Post_Blog_1.png)
* **Link trực tiếp hệ thống ứng dụng (Live System)**: [Link hệ thống ứng dụng]()