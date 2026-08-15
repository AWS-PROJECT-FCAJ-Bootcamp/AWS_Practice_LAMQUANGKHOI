---
title: "Lập lịch hàng ngày với Amazon EventBridge"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4.4. </b> "
---

# 5.4.4. Lập lịch Hàng ngày với Amazon EventBridge Scheduler

Để tự động hóa hoàn toàn tiến trình cào dữ liệu sau khi thị trường chứng khoán Việt Nam đóng cửa, hệ thống khởi tạo **Amazon EventBridge Scheduler** gửi tín hiệu kích hoạt hàm Lambda Collector định kỳ lúc **16:00 VN (10:00 UTC)** từ thứ Hai đến thứ Sáu.

---

### Quy trình cấu hình trên EventBridge Console

1. Mở **Amazon EventBridge Console** -> Chọn **Schedules** -> Click **Create schedule**.
2. **Schedule name**: `daily-financial-pipeline-schedule`.
3. **Schedule pattern**:
   - Schedule type: `Recurring schedule` (Cron-based schedule).
   - Cron expression: `cron(0 10 ? * MON-FRI *)` (Chạy vào 10:00 UTC = 16:00 giờ Việt Nam các ngày làm việc).
   - Timezone: `Asia/Ho_Chi_Minh`.
4. **Target detail**: Select target -> **AWS Lambda** -> Choose function `financial-data-collector`.
5. **Permissions**: Assign execution role `EventBridgeSchedulerRole`.

![(Hình 5.4.4.1) Cấu hình Cron Expression lập lịch chạy tự động hàng ngày trên Amazon EventBridge Console](/images/workshop/image21.png)

![(Hình 5.4.4.2) Giao diện quản lý chi tiết Schedule daily-financial-pipeline-schedule ở trạng thái Enabled](/images/workshop/image22.png)
