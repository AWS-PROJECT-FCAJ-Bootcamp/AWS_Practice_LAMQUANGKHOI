---
title: "Blog 2"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# TỰ ĐỘNG HÓA VÒNG ĐỜI ML VỚI AMAZON SAGEMAKER AI TRONG PHÂN TÍCH RỦI RO TÀI CHÍNH

Sau khi hoàn thiện Data Lake lưu trữ dữ liệu tài chính, bước tiếp theo trong hành trình nghiên cứu của nhóm mình là tìm kiếm giải pháp ứng dụng trí tuệ nhân tạo để dự báo sớm nguy cơ kiệt quệ tài chính doanh nghiệp. Nhóm mình đã chọn nghiên cứu và triển khai Amazon SageMaker AI — dịch vụ Machine Learning được quản lý hoàn toàn bởi AWS giúp tự động hóa toàn bộ vòng đời mô hình. Dưới đây là những điểm cốt lõi quan trọng nhất về việc ứng dụng SageMaker AI mà nhóm mình đúc kết để người đọc dễ dàng nắm bắt:

* **Quản lý tính năng tập trung với SageMaker Feature Store**: Nhóm mình lưu trữ và quản lý thống nhất tập các chỉ số tài chính đã làm sạch (CR, ROA, ROE, DAR, WCTA, ...) và nhãn rủi ro kiệt quệ tài chính từ S3 Data Lake vào Feature Store, giúp tái sử dụng tính năng giữa các thử nghiệm mô hình một cách nhất quán.
* **Thử nghiệm và lựa chọn mô hình trong SageMaker Studio**: Thông qua môi trường SageMaker Studio và Autopilot, nhóm mình đã tiến hành thử nghiệm các thuật toán phân loại phổ biến như XGBoost, Random Forest, Logistic Regression và LightGBM để đánh giá khả năng dự đoán trên tập dữ liệu tài chính Việt Nam.
* **Huấn luyện theo chuỗi thời gian và tối ưu chỉ số Recall**: Nhóm mình áp dụng chiến lược phân chia dữ liệu Time-Series Split (2018-2022 Train, 2023-2025 Test) nhằm chống rò rỉ dữ liệu tương lai. Trong bài toán rủi ro tài chính, nhóm mình đặt ưu tiên hàng đầu vào việc tối ưu metric Recall đối với nhóm doanh nghiệp rủi ro (`distress = 1`) nhằm tránh bỏ sót các doanh nghiệp có nguy cơ phá sản.
* **Triển khai Serverless Endpoints tối ưu chi phí**: Nhóm mình đóng gói mô hình tối ưu dưới dạng SageMaker Serverless Endpoints, cho phép API tự động mở rộng khi nhận request từ Web Dashboard và tự động hạ tài nguyên về 0 khi không có lưu lượng truy cập, giúp tiết kiệm tới 70% chi phí hạ tầng.
* **Quản trị MLOps chuẩn mực với Model Registry và Model Monitor**: Nhóm mình quản lý các phiên bản mô hình đã duyệt qua SageMaker Model Registry, đồng thời thiết lập SageMaker Model Monitor để tự động phát hiện Data Drift khi doanh nghiệp cập nhật BCTC quý mới và đưa ra cảnh báo cần tái huấn luyện mô hình.

Qua bài viết này, nhóm mình hy vọng đã cung cấp những điểm cốt lõi nhất về cách ứng dụng Amazon SageMaker AI để xây dựng quy trình MLOps tự động, chuẩn hóa và tối ưu cho các bài toán phân tích rủi ro tài chính doanh nghiệp.

![Sơ đồ quy trình huấn luyện và triển khai mô hình với Amazon SageMaker AI](/images/Blog%202.drawio.png)

---

### Nguồn tham khảo:
* [Trang chủ sản phẩm Amazon SageMaker AI](https://aws.amazon.com/sagemaker/)
* [Tài liệu kỹ thuật chính thức AWS SageMaker Documentation](https://docs.aws.amazon.com/sagemaker/)
* [Bài viết chuyên sâu AWS Machine Learning Blog](https://aws.amazon.com/blogs/machine-learning/)