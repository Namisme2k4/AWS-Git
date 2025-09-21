---
title: "Blog 2"
date: "2025-09-12"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# Tối Ưu Hóa Thu Thập Metrics với Amazon Managed Service for Prometheus

## Tổng quan

Khi các tổ chức mở rộng hệ thống quan sát (observability), việc xử lý metrics một cách hiệu quả trở nên then chốt cho độ tin cậy, hiệu suất và tối ưu chi phí. Bài viết này giới thiệu cách **Amazon Managed Service for Prometheus (AMP)** giúp doanh nghiệp tối ưu hóa pipeline ingestion, giám sát quota và bảo vệ workload thông qua giới hạn số chuỗi theo nhãn (label-based active series limits).

![Kiến trúc AMP Metrics Optimization](/images/AMP.png)  
_Hình 1: Quan sát tập trung với Amazon Managed Service for Prometheus và kiểm soát ingestion dựa trên nhãn._

---

## Bối cảnh và Thách thức

### Thực trạng hiện tại

- Các doanh nghiệp hiện đại vận hành nhiều workload trên nhiều tài khoản AWS và khu vực khác nhau.
- Khối lượng metrics ingestion có thể tăng lên hàng triệu chuỗi thời gian đang hoạt động.
- Nếu không kiểm soát, “noisy neighbors” có thể làm giảm hiệu suất và tăng chi phí.

### Thách thức chính

- Tập trung hóa metrics ingestion ở quy mô lớn.
- Giám sát quota ingestion một cách hiệu quả.
- Bảo vệ workload quan trọng khỏi các agent bị cấu hình sai hoặc đột biến dữ liệu.
- Cân bằng giữa tính linh hoạt và khả năng dự đoán chi phí.

---

## Nguyên tắc của AWS để Tối Ưu Hóa Ingestion

### Nguyên tắc 1: Quan sát tập trung

**Triết lý cốt lõi:**

- Việc tập trung hóa metrics từ nhiều tài khoản mang lại mặt phẳng quan sát thống nhất.
- Managed collectors và IAM roles cross-account đơn giản hóa pipeline ingestion.

**Giải pháp:**

- AMP workspaces đóng vai trò là endpoint tập trung cho việc scrape và lưu trữ metrics.
- Cross-account ingestion với IAM roles đảm bảo bảo mật và khả năng mở rộng.

---

### Nguyên tắc 2: Giám sát Quota và Mức sử dụng

**Nền tảng:**

- AMP định nghĩa quota cho tốc độ ingestion, số chuỗi đang hoạt động và độ đồng thời của truy vấn.
- CloudWatch cung cấp metrics chi tiết về mức sử dụng.

**Metrics chính:**

- `IngestionRate`: số mẫu mỗi giây.
- `ActiveSeries`: số chuỗi thời gian đang hoạt động.
- `DiscardedSamples`: số mẫu bị loại bỏ.
- Metrics cho rule evaluation và query TPS để tăng khả năng quan sát.

---

### Nguyên tắc 3: Kiểm soát bằng Giới hạn Chuỗi Theo Nhãn

**Khả năng nâng cao:**

- AMP giới thiệu tính năng **label-based active series limits** để cô lập workload “ồn ào”.
- Giới hạn được áp dụng theo label set (ví dụ: `app="payment-service", environment="prod"`).
- Ngăn một dịch vụ đơn lẻ làm quá tải toàn bộ workspace.

**Lợi ích:**

- Bảo vệ workload quan trọng.
- Nâng cao khả năng dự đoán chi phí.
- Khuyến khích thiết kế nhãn và metrics hợp lý.

---

### Nguyên tắc 4: Quan sát và Quản trị theo Label Set

**Nâng cấp:**

- CloudWatch metrics cung cấp dữ liệu ingestion theo từng label set.
- Metrics quan trọng:
  - `ActiveSeriesPerLabelSet`
  - `IngestionRatePerLabelSet`
  - `DiscardedSamplesPerLabelSet` (kèm lý do bị loại)

**Kết quả:**

- Đội ngũ có thể xác định workload nào tạo ra nhiều chi phí và overhead nhất.
- Cung cấp dữ liệu để tinh chỉnh giới hạn nhãn và cải thiện thực hành quan sát.

---

## Các Tính năng và Dịch vụ Chính

### Giới hạn theo Nhãn (Label-Based Limits)

- Kiểm soát chi tiết số chuỗi thời gian đang hoạt động.
- Cấu hình dễ dàng qua AWS Console hoặc CLI.
- Có giới hạn mặc định cho workload không thuộc label set nào.

### Managed Collectors

- Scraper bảo mật, có khả năng mở rộng, được quản lý bởi AWS.
- Hỗ trợ external labels để phù hợp với quy định label-set.

### Tích hợp CloudWatch

- Metrics dựng sẵn theo dõi tình trạng ingestion.
- Cho phép tạo dashboard, cảnh báo và phản ứng tự động.

---

## Kết quả Thực tế

### Hiệu quả Khách hàng

- **Đội ngũ observability doanh nghiệp**: giảm rủi ro ingestion quá tải.
- **Môi trường đa tài khoản**: đơn giản hóa việc thu thập metrics cross-account.
- **Đội vận hành**: tăng khả năng quan sát chi phí và sức khỏe workload.

### Ví dụ Lợi ích

- Bảo vệ dịch vụ production khỏi workload noisy test.
- Giảm chi phí bằng cách tối ưu metrics có độ phân mảnh nhãn cao (high-cardinality).
- Đạt hiệu suất ingestion ổn định ở quy mô lớn.

---

## Khuyến nghị Hành động

### Lời khuyên chính

- Thiết lập AMP workspaces tập trung để quan sát ở quy mô lớn.
- Liên tục giám sát quota và metrics ingestion qua CloudWatch.
- Cấu hình label-based limits để cô lập workload và đảm bảo quản trị.

### Chiến lược triển khai

1. Xác định external labels cho từng workload hoặc tài khoản.
2. Thiết lập giới hạn theo nhãn dựa trên mức độ quan trọng của nghiệp vụ.
3. Giám sát discarded samples và phân tích nguyên nhân metrics noisy.
4. Tự động hóa cảnh báo và dashboard để phát hiện sớm.

---

## Kết luận

Amazon Managed Service for Prometheus mang đến công cụ cần thiết để mở rộng hệ thống quan sát mà không làm ảnh hưởng đến độ tin cậy hay chi phí. Với ingestion tập trung, giám sát quota và quản trị dựa trên nhãn, doanh nghiệp có thể bảo vệ workload quan trọng, tránh noisy neighbors và duy trì hiệu suất ổn định trong khi nhu cầu quan sát ngày càng tăng.
