---
title: "Dữ liệu mẫu của dự án"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

# 5.4.2. Dữ liệu Mẫu và Cấu trúc Data Contract

Để phục vụ kiểm thử luồng tự động hóa, dự án chuẩn hóa cấu trúc dữ liệu đầu vào dưới định dạng JSON với các thông số giá mở cửa, giá cao nhất, giá thấp nhất, giá đóng cửa và khối lượng giao dịch.

---

### 1. Cấu trúc Payload dữ liệu JSON thô (Sample Raw Payload)

```json
{
  "symbol": "FPT",
  "provider": "YFINANCE_API",
  "collected_at": "2026-08-15T10:00:00.000Z",
  "records": [
    {
      "symbol": "FPT",
      "trading_date": "2026-08-14",
      "open_price": 128500.0,
      "high_price": 130000.0,
      "low_price": 127800.0,
      "close_price": 129500.0,
      "volume": 3450000
    },
    {
      "symbol": "FPT",
      "trading_date": "2026-08-13",
      "open_price": 127000.0,
      "high_price": 129000.0,
      "low_price": 126500.0,
      "close_price": 128200.0,
      "volume": 2890000
    }
  ]
}
```

- Dữ liệu thô được ghi trực tiếp vào đường dẫn S3 Raw Bucket theo phân vùng thời gian `ohlcv/ohlcv/year=YYYY/month=MM/day=DD/batch_FPT_<timestamp>.json`.

![(Hình 5.4.2.1) Cấu trúc thư mục phân vùng lưu trữ dữ liệu JSON thô trên Amazon S3 Raw Bucket](/images/workshop/image3.png)
