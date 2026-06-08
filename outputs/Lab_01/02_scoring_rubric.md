# BẢNG CHẤM ĐIỂM SƠ BỘ THEO RUBRIC (SCORING RUBRIC)
## Tên bài toán: Trợ lý AI Kiểm soát Tiến độ và Chỉ số Dự án cho Quản lý Dự án (PM/Sub-PM)

---

### 1. Bảng chấm điểm chi tiết (Thang điểm 100)

| Tiêu chí đánh giá | Trọng số (%) | Điểm số | Điểm quy đổi | Phân tích chi tiết và Lý do cho điểm |
| :--- | :---: | :---: | :---: | :--- |
| **1. Tính khả thi của dữ liệu (Data Feasibility)** | 25% | **80/100** | **20.0/25** | Dữ liệu tiến độ hệ thống (dạng bảng) và tài liệu mô tả dự án có cấu trúc rõ ràng, dễ dàng nạp vào hệ thống RAG. Tuy nhiên, tin nhắn cập nhật nhanh từ nhân sự là dữ liệu phi cấu trúc, có ngôn ngữ tự nhiên tự do, viết tắt và đôi khi thiếu nhất quán, đòi hỏi khâu tiền xử lý (preprocessing) và chuẩn hóa dữ liệu tốt. |
| **2. Mức độ lặp lại (Repeatability)** | 20% | **90/100** | **18.0/20** | PM và Sub-PM thực hiện tra cứu tiến độ dự án hàng ngày và họp giao ban hàng tuần. Nhu cầu trả lời nhanh các câu hỏi như *"Ai đang chậm tiến độ?"* hoặc *"Tỷ lệ hoàn thành dự án X lũy kế là bao nhiêu?"* diễn ra liên tục, tính lặp lại rất cao. |
| **3. Khả năng đo lường hiệu quả (Measurability)** | 15% | **86/100** | **12.9/15** | Dễ dàng đo lường bằng các chỉ số định lượng: thời gian PM chuẩn bị báo cáo tuần (giảm từ giờ xuống phút), tỷ lệ trích dẫn nguồn chính xác của AI (đếm qua số lượt click kiểm tra nguồn), và số lượt đánh giá 👍/👎 từ người dùng. |
| **4. Rủi ro bảo mật & Tuân thủ (Security & Compliance)** | 20% | **70/100** | **14.0/20** | Dữ liệu dự án, chỉ số hiệu suất và kế hoạch nhân sự là thông tin nội bộ có tính bảo mật cao. Nếu dùng API công cộng mà không có cam kết bảo mật, nguy cơ rò rỉ thông tin là rất lớn. Cần thiết lập rào cản an toàn nghiêm ngặt để đạt mức độ an toàn tối đa. |
| **5. Sự tham gia của con người (HITL)** | 20% | **90/100** | **18.0/20** | Quy trình thiết kế đảm bảo con người (PM) luôn kiểm tra nguồn trích dẫn trước khi dùng số liệu báo cáo. Có cơ chế cho người dùng báo cáo lỗi trực tiếp để cải tiến mô hình. |
| **TỔNG ĐIỂM ƯỚC LƯỢNG** | **100%** | | **82.9/100** | **Phân loại: Ý tưởng khả thi cao, ưu tiên triển khai (Quick Win)** |

---

### 2. Biểu đồ radar đánh giá các chiều tiêu chí (Minh họa trực quan)

```mermaid
radar-chart
    title Đánh giá đa chiều Use Case
    labels Tính khả thi dữ liệu, Mức độ lặp lại, Khả năng đo lường, Bảo mật & Tuân thủ, Vai trò HITL
    dataset "Điểm số thực tế (Thang 100)"
        value [80, 90, 86, 70, 90]
```

---

### 3. Nhận xét và khuyến nghị từ Trợ lý AI

#### Ưu điểm nổi bật:
* **Giải quyết điểm nghẽn thực tế (Pain Point):** Giúp PM/Sub-PM thoát khỏi việc lục tìm tin nhắn chat phân tán và các bảng Excel tiến độ khổng lồ.
* **Thời gian hoàn vốn công nghệ nhanh (Time-to-Value):** Sử dụng các công nghệ sẵn có như RAG (Retrieval-Augmented Generation) kết hợp với các mô hình LLM sẵn có là đủ để giải quyết bài toán mà không cần huấn luyện lại mô hình từ đầu (Fine-tuning).

#### Thách thức và Khuyến nghị:
* **Rủi ro ảo tưởng số liệu:** AI có xu hướng nội suy hoặc đoán số liệu phần trăm tiến độ khi tài liệu thiếu thông tin. **Khuyến nghị:** Cài đặt Prompt khắt khe để AI từ chối suy đoán khi thiếu căn cứ và luôn bắt buộc đính kèm trích dẫn nguồn.
* **Bảo mật dữ liệu:** Vì đây là thông tin dự án nội bộ, khuyến nghị triển khai thử nghiệm MVP sử dụng các cổng kết nối API Enterprise bảo mật (dữ liệu không dùng để train lại mô hình) hoặc Private LLM nếu tài nguyên cho phép.
