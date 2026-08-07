---
title: "Blog 3"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# CHUẨN HÓA DỮ LIỆU TÀI CHÍNH QUY MÔ LỚN VỚI AWS GLUE JOBS VÀ AMAZON S3 DATA LAKE

Một trong những mảnh ghép quan trọng nhất trong quá trình xây dựng hạ tầng dữ liệu của nhóm mình là thiết lập tiến trình tích hợp dữ liệu (ETL) tự động. Dữ liệu thô thu thập từ nhiều nguồn báo cáo tài chính khác nhau thường xuất hiện mâu thuẫn về tên gọi chỉ tiêu, cấu trúc khuyết thiếu và khối lượng lớn bản ghi chưa qua xử lý. Nhóm mình đã chọn ứng dụng AWS Glue Jobs — dịch vụ tích hợp dữ liệu serverless mạnh mẽ của AWS. Dưới đây là những điểm cốt lõi quan trọng nhất về việc chuẩn hóa dữ liệu với AWS Glue Jobs mà nhóm mình đúc kết:

* **Tự động khám phá Schema bằng AWS Glue Crawlers**: Nhóm mình thiết lập Glue Crawlers định kỳ quét các tệp dữ liệu thô (JSON/CSV) từ `S3 Raw Bucket`, tự động trích xuất cấu trúc schema của 3 bảng Báo cáo Tài chính và đăng ký metadata vào AWS Glue Data Catalog làm kho quản lý trung tâm.
* **Xử lý biến đổi dữ liệu phân tán với PySpark ETL Jobs**: Nhóm mình lập trình các script PySpark trên AWS Glue Jobs để đồng bộ hóa hàng trăm biến thể tên chỉ tiêu tài chính đa nguồn về chuẩn tên thống nhất (`total_assets`, `net_revenue`, `ebit`, `ocf`), đồng thời loại bỏ khối ngành tài chính đặc thù (Ngân hàng, Chứng khoán, Bảo hiểm).
* **Lọc điều kiện khuyết thiếu và xử lý nhiễu bằng Winsorization**: Nhóm mình cài đặt điều kiện lọc chỉ giữ lại doanh nghiệp đủ 5 năm dữ liệu liên tục và tích hợp thuật toán Winsorization (1%-99%) để giới hạn các giá trị ngoại lệ bất thường mà không làm méo lệch bản chất dữ liệu.
* **Tối ưu chi phí DPU với tính năng Job Bookmarks**: Nhóm mình tận dụng tính năng Job Bookmarks để Glue Job tự động ghi nhớ trạng thái các tệp đã xử lý, giúp tiến trình ETL chỉ tập trung làm sạch phần BCTC mới bổ sung trong mỗi kỳ cào định kỳ, tránh xử lý trùng lặp và tiết kiệm chi phí DPU đáng kể.
* **Chuyển đổi Parquet nén Snappy và phân vùng Data Lake**: Dữ liệu sạch được nhóm mình chuyển sang định dạng Apache Parquet nén Snappy phân vùng theo `year` và `quarter` tại `S3 Curated Bucket`, giúp giảm 80% dung lượng lưu trữ S3 và tăng tốc độ truy vấn SQL từ Amazon Athena lên gấp nhiều lần.

Qua bài viết này, nhóm mình hy vọng giúp người đọc dễ dàng nắm bắt được các nguyên lý cốt lõi khi vận hành AWS Glue Jobs để xây dựng một pipeline chuẩn hóa dữ liệu tài chính tự động, tối ưu chi phí và mở rộng linh hoạt trên đám mây.

![Sơ đồ kiến trúc xử lý dữ liệu ETL với AWS Glue Jobs và Amazon S3 Data Lake](/images/Blog_3.jpg)

---

### Nguồn tham khảo:
* [Trang chủ dịch vụ AWS Glue](https://aws.amazon.com/glue/)
* [Tài liệu kỹ thuật chính thức AWS Glue Documentation](https://docs.aws.amazon.com/glue/)
* [Hướng dẫn chuẩn AWS Glue ETL Best Practices](https://docs.aws.amazon.com/prescriptive-guidance/latest/aws-glue-etl/welcome.html)