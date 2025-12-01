---
title: "Blog 3"
date: "2025-09-29"
weight: 1
chapter: false
pre: " <b> 3.4. </b> "
---

# Đo lường độ chính xác khi so khớp theo quy tắc hoặc máy học trong AWS Entity Resolution

Khi xây dựng hệ thống làm sạch dữ liệu hoặc hợp nhất bản ghi (entity matching), câu hỏi quan trọng là:  
**Làm sao biết quy tắc (rule-based) hay mô hình máy học (ML-based) đủ chính xác để triển khai thực tế?**

Việc thiếu tiêu chí đánh giá rõ ràng thường làm các dự án:
- tốn nhiều thời gian,
- phải lặp lại quy trình,
- hoặc phải thay đổi phương pháp với chi phí cao.

Bài viết này hướng dẫn cách thiết lập khung đo lường khách quan bằng **AWS Entity Resolution**, giúp đánh giá độ chính xác của cả hai phương pháp: rule-based và machine learning.

---

## ⭐ Lợi ích chính

- **Xác định rõ mục tiêu chính xác cần đạt** trước khi triển khai sản xuất.  
- **Đánh giá khách quan** thông qua ground truth set, tính precision, recall và F1-score.  
- **Áp dụng được cho cả rule-based và ML-based**, giúp chọn giải pháp tối ưu.  
- **Có dataset mở (BPID)** để thử nghiệm nếu thiếu dữ liệu thật.

---

## 🧱 Hướng dẫn kiến trúc & thành phần chính

### **Bước 1. Hiểu về “ground truth set”**

Ground truth set là một tập nhỏ các cặp bản ghi đã được con người đánh nhãn:  
- **match** (trùng nhau)  
- **no-match** (không trùng)

Yêu cầu:
- Không cần lớn nhưng phải **đại diện** cho nhiều trường hợp: thiếu dữ liệu, lỗi nhập, định dạng sai, trùng lặp một phần…  
- Phải tuân thủ an toàn dữ liệu và bảo mật PII khi sử dụng.

---

### **Bước 2. Các thách thức**

- Dữ liệu thực tế thường **không sạch**, chứa thiếu sót và lỗi.  
- Cần chọn **ngưỡng độ chính xác phù hợp** — quá cao sẽ không thực tế, quá thấp thì rủi ro sai lệch.  
- Tùy ngành nghề mà ưu tiên khác nhau:
  - **Precision cao** trong tài chính & bảo hiểm.  
  - **Recall cao** trong marketing & CRM.

---

### **Bước 3. Các chỉ số đánh giá cơ bản**

- **Precision**  
  Tỷ lệ cặp mà hệ thống đánh là "match" và thực sự đúng.  
- **Recall**  
  Tỷ lệ cặp thực sự trùng được hệ thống tìm ra.  
- **F1-score**  
  Trung bình điều hòa giữa precision và recall.

---

### **Bước 4. Ví dụ thực hành với dataset mở BPID**

AWS cung cấp dataset **BPID** (~20.000 bản ghi tổng hợp), gồm:
- tên  
- email  
- số điện thoại  
- địa chỉ  
- ngày sinh  
- trường hợp dữ liệu phức tạp mô phỏng thực tế

#### **Quy trình thực hiện:**
1. Tải dữ liệu BPID.  
2. Tiền xử lý dữ liệu (chuẩn hóa email, số điện thoại, địa chỉ...).  
3. Chạy workflow matching với AWS Entity Resolution:
   - rule-based  
   - ML-based  
4. So sánh kết quả với ground truth để tính:
   - True Positive  
   - False Positive  
   - True Negative  
   - False Negative  
   - Precision, Recall, F1-score  

---

## ✨ Các tính năng mới trong giải pháp

- Dataset BPID phản ánh dữ liệu phức tạp hơn các dataset sạch thông thường → đánh giá sát thực tế hơn.  
- AWS Entity Resolution hỗ trợ **cả rule-based và ML-based matching**.  
- Có hướng dẫn chi tiết từ:
  - tiền xử lý  
  - chạy matching  
  - tính F1-score  

---

## 🚀 Chiến lược triển khai & di trú

### **1. Xây ground truth set nội bộ**
- Nếu thiếu dữ liệu thực có nhãn, hãy tạo một ground truth nhỏ từ dữ liệu sản xuất.  
- Nên đưa vào nhiều **trường hợp khó (edge cases)**.

### **2. Thử nghiệm song song**
- Chạy rule-based và ML-based để so sánh:
  - độ chính xác  
  - chi phí  
  - tốc độ  
  - bảo mật  
  - mức độ dễ bảo trì  

### **3. Định mức ngưỡng chấp nhận**
- Xác định trước mức precision/recall tối thiểu phù hợp ngành.  
- Ví dụ:
  - Tài chính: ưu tiên precision cao để tránh nhận nhầm.  
  - Marketing: ưu tiên recall cao để mở rộng tập khách hàng.

### **4. Sử dụng dữ liệu tổng hợp khi thiếu dữ liệu thực**
- Khi vấn đề bảo mật hoặc dữ liệu chưa sẵn sàng → dùng BPID để thử nghiệm.

---

## ✅ Kết luận

Việc đo lường độ chính xác khi so khớp bản ghi là bắt buộc để:

- đảm bảo hệ thống xử lý được các trường hợp khó,  
- tránh lãng phí thời gian do phải chỉnh sửa sau triển khai,  
- có tiêu chuẩn rõ ràng khi so sánh nhiều giải pháp hoặc nhà cung cấp.

**AWS Entity Resolution** cung cấp:
- công cụ mạnh mẽ,  
- dataset mẫu BPID,  
- quy trình đánh giá chuẩn,  
→ giúp bạn kiểm chứng và lựa chọn phương pháp tối ưu cho dữ liệu của mình.

---

## 👤 About the Authors

<table style="width: 100%; border-collapse: collapse;">
<tr>
<td style="width: 200px; vertical-align: top; padding-right: 30px;">
<img src="/images/Blog2.jpeg" alt="Travis Barnes" style="width: 180px; height: 180px; object-fit: cover; border-radius: 8px;">
</td>
<td style="vertical-align: top;">
<h3 style="margin: 0 0 10px 0;"><strong>Travis Barnes</strong></h3>
<p style="margin: 0;">Senior Product Manager, Technical tại AWS Entity Resolution.<br/>Có hơn 10 năm kinh nghiệm trong identity và adtech, tập trung vào giải pháp data onboarding và identity resolution.</p>
</td>
</tr>
<tr style="height: 40px;">
<td colspan="2"></td>
</tr>
<tr>
<td style="width: 200px; vertical-align: top; padding-right: 30px;">
<img src="/images/Blog3.2.jpeg" alt="Yefan Tao" style="width: 180px; height: 180px; object-fit: cover; border-radius: 8px;">
</td>
<td style="vertical-align: top;">
<h3 style="margin: 0 0 10px 0;"><strong>Yefan Tao</strong></h3>
<p style="margin: 0;">Senior Applied Scientist, chuyên về hệ thống entity resolution và information retrieval.<br/>Nghiên cứu và triển khai thuật toán ML quy mô lớn, tập trung vào governance và xử lý dữ liệu phức tạp.</p>
</td>
</tr>
</table>

