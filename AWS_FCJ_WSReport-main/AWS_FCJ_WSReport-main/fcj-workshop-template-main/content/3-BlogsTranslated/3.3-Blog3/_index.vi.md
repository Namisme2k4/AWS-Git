---
title: "Blog 3"
date: "2025-09-29"
weight: 1
chapter: false
pre: " <b> 3.4. </b> "
---

# Đo lường độ chính xác của việc ghép dữ liệu theo luật hoặc ML trong AWS Entity Resolution


Trong nhiều tổ chức, dữ liệu khách hàng, sản phẩm hoặc bệnh nhân thường được lưu trữ trên nhiều hệ thống khác nhau. Để xây dựng cái nhìn thống nhất, cần phải **ghép (matching)** các bản ghi trùng khớp với cùng một thực thể. Thách thức đặt ra là đảm bảo phương pháp ghép — **dựa trên luật (rule-based)** hoặc **dựa trên học máy (ML-based)** — có độ chính xác đủ cao.

Bài viết này giải thích cách đánh giá và đo lường độ chính xác trong **AWS Entity Resolution**, giúp các nhóm xác minh liệu chiến lược đã chọn có đáp ứng yêu cầu kinh doanh và tuân thủ hay không.

---

## Tại sao độ chính xác quan trọng

Độ chính xác trong ghép thực thể ảnh hưởng trực tiếp đến nhiều trường hợp sử dụng:

- **Customer 360**: Xây dựng hồ sơ khách hàng toàn diện
- **Phát hiện gian lận**: Nhận diện các danh tính trùng lặp hoặc giả mạo
- **Y tế**: Liên kết hồ sơ bệnh nhân mà vẫn bảo vệ quyền riêng tư
- **Marketing**: Đảm bảo cá nhân hóa và phân khúc chính xác

Nếu độ chính xác thấp, có thể xảy ra:

- **Dương tính giả**: Các thực thể khác nhau bị ghép sai thành một
- **Âm tính giả**: Các bản ghi cùng thực thể nhưng không được ghép

---

## Ghép dữ liệu theo luật vs. học máy

- **Ghép theo luật (Rule-based)**  
  Sử dụng các quy tắc xác định (ví dụ: khớp chính xác theo email + khớp gần đúng theo tên).

  - Dễ hiểu và dễ giải thích
  - Hiệu quả khi dữ liệu đã chuẩn hóa
  - Hạn chế khi dữ liệu có nhiều biến thể

- **Ghép theo học máy (ML-based)**  
  Sử dụng mô hình học để nhận diện mức độ tương đồng giữa các thuộc tính.
  - Xử lý tốt dữ liệu lộn xộn, không đầy đủ hoặc không đồng nhất
  - Có thể cải thiện độ chính xác theo thời gian
  - Cần dữ liệu gán nhãn để đánh giá

---

## Đo lường độ chính xác trong AWS Entity Resolution

AWS Entity Resolution cung cấp sẵn công cụ để đo lường độ chính xác của cả hai phương pháp:

1. **Precision (Độ chính xác)** – Tỷ lệ các ghép đúng trong tổng số ghép được phát hiện.
2. **Recall (Độ bao phủ)** – Tỷ lệ các ghép đúng được phát hiện trong tất cả các ghép thực tế.
3. **F1 Score** – Trung bình điều hòa giữa Precision và Recall.

### Quy trình đánh giá

1. Xác định **tập dữ liệu kiểm thử** có nhãn “ground truth”.
2. Thực hiện ghép theo cấu hình (luật hoặc ML).
3. So sánh kết quả ghép với ground truth.
4. Đánh giá các chỉ số (Precision, Recall, F1) và điều chỉnh nếu cần.

---

## Thực tiễn tốt nhất

- Bắt đầu với **tập dữ liệu đại diện** cho thực tế.
- Lặp lại và tinh chỉnh quy tắc hoặc mô hình ML dựa trên kết quả đo lường.
- Sử dụng **tính năng giải thích (explainability)** khi áp dụng ML để tăng độ tin cậy.
- Liên tục theo dõi và đánh giá lại khi thêm nguồn dữ liệu mới.

---

## Kết luận

Đo lường độ chính xác là bước cốt lõi để chọn chiến lược ghép dữ liệu phù hợp trong AWS Entity Resolution. Dù chọn **sự đơn giản của luật** hay **sự linh hoạt của ML**, việc theo dõi các chỉ số như Precision, Recall, và F1 sẽ đảm bảo giải pháp mang lại kết quả tin cậy cho các trường hợp sử dụng trong nhiều lĩnh vực như y tế, tài chính và bán lẻ.

---
