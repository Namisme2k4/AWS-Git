---
title: "Blog 3"
date: "2025-09-12"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# Tối ưu hóa ingestion Metrics với Amazon Managed Service for Prometheus

## Tổng quan

Khi các tổ chức mở rộng hệ thống quan sát (observability), việc xử lý metrics hiệu quả trở thành yếu tố then chốt để đảm bảo độ tin cậy, hiệu năng và tối ưu chi phí. Bài viết này giới thiệu cách **Amazon Managed Service for Prometheus (AMP)** giúp doanh nghiệp tối ưu pipeline ingestion, giám sát quota, và bảo vệ workload thông qua giới hạn active series dựa trên nhãn (label-based).

![Kiến trúc tối ưu hóa Metrics với AMP](/images/Blog3.png)  
_Hình 1:- Hợp nhất nhật ký trên nhiều tài khoản và vùng._

---

## Bối cảnh và thách thức

### Thực trạng hiện tại

- Doanh nghiệp hiện đại vận hành nhiều workload trên nhiều tài khoản và vùng AWS.
- Khối lượng metrics ingestion dễ dàng tăng lên đến hàng triệu time series đang hoạt động.
- Nếu không có kiểm soát, “noisy neighbors” có thể làm suy giảm hiệu năng và tăng chi phí.

### Các thách thức chính

- Tập trung hóa ingestion metrics ở quy mô lớn.
- Giám sát hiệu quả quota ingestion.
- Bảo vệ workload quan trọng khỏi agent cấu hình sai hoặc phát sinh bất thường.
- Cân bằng giữa tính linh hoạt, tính dự đoán và kiểm soát chi phí.

---

## Nguyên tắc cốt lõi của AWS cho ingestion tối ưu

### Nguyên tắc 1: Quan sát tập trung (Centralized Observability)

**Triết lý cốt lõi:**

- Hợp nhất metrics từ nhiều tài khoản để có một mặt phẳng quan sát thống nhất.
- Bộ thu thập (collector) được quản lý và vai trò cross-account bảo mật giúp đơn giản hóa pipeline ingestion.

**Giải pháp:**

- Các workspace AMP đóng vai trò endpoint tập trung để scrape và lưu trữ metrics.
- Ingestion cross-account với IAM roles đảm bảo bảo mật và khả năng mở rộng.

---

### Nguyên tắc 2: Giám sát quota và mức sử dụng

**Nền tảng:**

- AMP định nghĩa quota cho tốc độ ingestion, số lượng active series, và độ đồng thời của truy vấn.
- Metrics của CloudWatch cung cấp thông tin chi tiết về mức sử dụng.

**Các metrics chính:**

- `IngestionRate`: số mẫu/giây.
- `ActiveSeries`: số lượng time series đang hoạt động.
- `DiscardedSamples`: số mẫu bị từ chối.
- Metrics về đánh giá rule và truy vấn TPS giúp tăng khả năng quan sát.

---

### Nguyên tắc 3: Kiểm soát bằng giới hạn active series theo nhãn

**Năng lực cải tiến:**

- Giới thiệu giới hạn dựa trên nhãn để cô lập workload gây ồn (noisy).
- Giới hạn áp dụng theo tập nhãn (ví dụ: `app="payment-service", environment="prod"`).
- Ngăn chặn một dịch vụ đơn lẻ làm quá tải toàn bộ workspace.

**Lợi ích:**

- Bảo vệ workload quan trọng.
- Cải thiện khả năng dự đoán chi phí.
- Khuyến khích thiết kế nhãn tốt hơn và duy trì hygiene cho metrics.

---

### Nguyên tắc 4: Quan sát và quản trị theo tập nhãn

**Cải tiến:**

- CloudWatch giờ đây cung cấp dữ liệu ingestion chi tiết ở mức độ tập nhãn.
- Các metrics quan trọng:
  - `ActiveSeriesPerLabelSet`
  - `IngestionRatePerLabelSet`
  - `DiscardedSamplesPerLabelSet` (kèm lý do từ chối)

**Kết quả:**

- Nhóm vận hành có thể xác định workload nào tạo ra nhiều overhead nhất.
- Cung cấp dữ liệu để điều chỉnh giới hạn nhãn và cải thiện thực hành quan sát.

---

## Tính năng và dịch vụ chính

### Giới hạn dựa trên nhãn

- Kiểm soát chi tiết số lượng active time series.
- Có thể cấu hình qua AWS Console hoặc CLI.
- Giới hạn mặc định áp dụng cho workload ngoài tập nhãn đã định nghĩa.

### Bộ thu thập được quản lý (Managed Collectors)

- Bộ scrape an toàn, có thể mở rộng, do AWS quản lý.
- Hỗ trợ nhãn ngoài (external labels) để đồng bộ ingestion với quản trị tập nhãn.

### Tích hợp CloudWatch

- Có sẵn metrics theo dõi sức khỏe ingestion.
- Cho phép tạo dashboard, cảnh báo và phản ứng tự động.

---

## Kết quả thực tế

### Kết quả từ khách hàng

- **Nhóm quan sát doanh nghiệp**: giảm rủi ro ingestion quá tải.
- **Môi trường đa tài khoản**: đơn giản hóa việc thu thập metrics cross-account.
- **Nhóm vận hành**: cải thiện khả năng nhìn rõ chi phí và sức khỏe workload.

### Ví dụ lợi ích

- Bảo vệ dịch vụ production khỏi workload thử nghiệm gây ồn.
- Giảm chi phí nhờ tinh chỉnh metrics có độ phân mảnh cao (high-cardinality).
- Duy trì hiệu năng ingestion ổn định ở quy mô lớn.

---

## Khuyến nghị hành động

### Lời khuyên chính

- Thiết lập AMP workspace trung tâm cho quan sát ở quy mô lớn.
- Liên tục giám sát quota và metrics ingestion qua CloudWatch.
- Cấu hình giới hạn dựa trên nhãn để cô lập workload và đảm bảo quản trị.

### Chiến lược triển khai

1. Định nghĩa nhãn ngoài (external labels) cho từng workload hoặc tài khoản.
2. Đặt giới hạn nhãn theo mức độ quan trọng kinh doanh.
3. Giám sát `discarded samples` và điều tra metrics gây ồn.
4. Tự động hóa cảnh báo và dashboard để phát hiện sớm.

---

## Kết luận

**Amazon Managed Service for Prometheus** cung cấp các công cụ cần thiết để mở rộng quan sát mà không hy sinh độ tin cậy hoặc hiệu quả chi phí. Với ingestion tập trung, giám sát quota, và quản trị dựa trên nhãn, các tổ chức có thể bảo vệ workload quan trọng, ngăn chặn noisy neighbors, và duy trì hiệu năng ổn định khi nhu cầu quan sát ngày càng tăng.
