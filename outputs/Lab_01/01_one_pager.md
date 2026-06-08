# PHIẾU MÔ TẢ TRƯỜNG HỢP SỬ DỤNG (ONE-PAGER)
## Tên bài toán: Trợ lý AI Kiểm soát Tiến độ và Chỉ số Dự án cho Quản lý Dự án (PM/Sub-PM)

---

### 1. Thông tin chung & Người dùng chính (Primary User)
* **Người dùng chính:** Quản lý Dự án (Project Manager - PM) và Quản lý Dự án thành phần (Sub-PM).
* **Mục tiêu chính:** Hỗ trợ PM/Sub-PM theo dõi, tra cứu nhanh chóng và chính xác tiến độ công việc, các chỉ số sức khỏe của dự án bằng cách tương tác bằng ngôn ngữ tự nhiên với trợ lý AI, giảm thiểu thời gian tổng hợp báo cáo thủ công.

---

### 2. Mô hình Đầu vào (Input) & Đầu ra (Output) Dự kiến

```mermaid
graph TD
    subgraph Đầu vào (Giả lập)
        A[Tiến độ dự án - Bảng biểu/Task] --> E(Trợ lý AI)
        B[Các chỉ số sức khỏe dự án - KPI, SPI, CPI] --> E
        C[Tài liệu mô tả dự án - Charter/Kế hoạch] --> E
        D[Tin nhắn báo cáo nhanh từ nhân sự] --> E
    end
    subgraph Đầu ra của AI
        E --> F[Tiến độ lũy kế chi tiết theo nhân sự]
        E --> G[Nguồn trích dẫn rõ ràng từ dữ liệu gốc]
        E --> H[Khuyến nghị liên hệ nhân sự khi vượt phạm vi]
    end
    subgraph Kiểm duyệt (HITL)
        F & G & H --> I((PM/Sub-PM rà soát & xác nhận))
    end
```

#### Đầu vào dự kiến (Input giả lập):
1. **Tiến độ các dự án:** Bảng trạng thái task công việc giả lập (ví dụ: Task ID, Tên task, Người phụ trách, Ngày bắt đầu, Hạn hoàn thành, % Hoàn thành thực tế).
2. **Các chỉ số sức khỏe dự án:** Chỉ số hiệu suất chi phí (CPI), Chỉ số hiệu suất tiến độ (SPI), Số ngày chậm trễ dự kiến.
3. **Tài liệu mô tả dự án:** Tài liệu Charter dự án giả lập, Kế hoạch quản lý phạm vi công việc.
4. **Tin nhắn cập nhật:** Đoạn hội thoại chat báo cáo nhanh của các kỹ sư phụ trách gửi PM (ví dụ: *"Nhân sự A báo cáo: Task thiết kế giao diện UI đã hoàn thành 90%, đang đợi feedback"*).

#### Đầu ra mong muốn (Output):
1. **Bảng tổng hợp tiến độ lũy kế:** Báo cáo chi tiết phần trăm công việc đã hoàn thành lũy kế, gom nhóm theo từng nhân sự phụ trách được hỏi.
2. **Nguồn trích dẫn rõ ràng (Data Grounding):** Trích dẫn cụ thể thông tin được lấy từ đâu (Ví dụ: *"Nguồn: Tệp 'Ke_hoach_trien_khai_v1.xlsx', sheet 'TienDo', dòng 14"* hoặc *"Dựa trên tin nhắn cập nhật của Kỹ sư B lúc 09:30 ngày 08/06/2026"*).
3. **Khuyến nghị vượt phạm vi (Fallback):** Đưa ra chỉ dẫn liên hệ nhân sự phụ trách nếu thông tin cần hỏi không có trong cơ sở dữ liệu hoặc có sự mâu thuẫn lớn. (Ví dụ: *"AI Khuyến nghị: Thông tin về lý do chậm trễ của Task #104 không được ghi nhận trong báo cáo hay tin nhắn. Vui lòng liên hệ trực tiếp Kỹ sư [Tên nhân sự giả lập] để làm rõ"*).

---

### 3. Giá trị kỳ vọng & Chỉ số đo lường hiệu quả (KPIs)
* **Thời gian tiết kiệm dự kiến:** 
  * Giảm từ **3 - 4 giờ/tuần** xuống còn **15 - 20 phút/tuần** cho mỗi PM/Sub-PM trong việc tổng hợp dữ liệu, chuẩn bị báo cáo tuần và rà soát tiến độ chéo.
* **Chỉ số đo lường hiệu quả chính:**
  * **Tỷ lệ trích dẫn chính xác nguồn dữ liệu (Grounding Accuracy):** Đạt trên **98%** (AI không tự ý bịa ra số liệu tiến độ mà không chỉ ra được dòng/tài liệu tham chiếu).
  * **Tỷ lệ câu trả lời hữu ích (Helpfulness Rate):** Trên **90%** số câu hỏi của PM được AI trả lời đầy đủ thông tin hoặc định hướng đúng nhân sự cần liên hệ.
  * **Thời gian phản hồi truy vấn (Latency):** Dưới **3 giây** cho mỗi câu hỏi tra cứu tiến độ.

---

### 4. Phạm vi sản phẩm khả dụng tối thiểu (MVP) - Những gì CHƯA xử lý

> [!WARNING]
> Để đảm bảo an toàn tuyệt đối cho hệ thống dữ liệu gốc và tránh các quyết định sai lầm do AI ảo tưởng, các tính năng sau **không nằm trong phạm vi MVP**:

* **CHƯA tự động ghi nhận/cập nhật dữ liệu:** AI chỉ có quyền **Đọc (Read-only)** dữ liệu tiến độ và tin nhắn, tuyệt đối **không tự động ghi đè hoặc cập nhật** phần trăm hoàn thành vào hệ thống quản lý dự án (Jira/Trello) khi PM hỏi hoặc ra lệnh qua chatbot.
* **CHƯA tự động đánh giá hiệu suất/chấm điểm KPI nhân sự:** AI không được phép đưa ra các kết luận định tính về năng lực làm việc của nhân sự (ví dụ: đánh giá nhân sự làm việc chậm, kém hiệu quả) để tránh định kiến sai lệch.
* **CHƯA tự động ra quyết định điều phối tài nguyên:** AI không tự động phân công lại công việc từ nhân sự chậm tiến độ sang nhân sự khác.
* **CHƯA dự báo tài chính/dòng tiền sâu:** Không thực hiện các phân tích tài chính phức tạp ngoài việc đọc các chỉ số tài chính cơ bản đã được tính toán sẵn (như CPI, dự chi ngân sách đã nhập).

---

### 5. Điểm dừng kiểm duyệt của con người trong vòng lặp (Human-in-the-loop - HITL)

> [!IMPORTANT]
> AI chỉ đóng vai trò trợ lý tổng hợp thông tin. Con người luôn là chốt chặn quyết định cuối cùng.

1. **Xác nhận số liệu trước khi báo cáo:** PM/Sub-PM bắt buộc phải thực hiện đối chiếu chéo (click vào link nguồn trích dẫn do AI cung cấp) để kiểm tra tính xác thực của số liệu trước khi copy vào báo cáo gửi cấp quản lý cao hơn.
2. **Cơ chế phản hồi trực tiếp (Feedback Loop):** Cung cấp nút bấm 👍 (Hữu ích/Đúng) và 👎 (Sai lệch/Không rõ ràng) bên cạnh mỗi câu trả lời của AI. Nếu bấm 👎, hệ thống sẽ yêu cầu người dùng chỉ ra điểm sai để gửi thông tin này về đội ngũ quản trị hệ thống (PMO/QA) tinh chỉnh prompt và dữ liệu.
3. **Quy trình kiểm toán dữ liệu định kỳ (Data Auditing):** Bộ phận PMO/QA sẽ thực hiện kiểm tra ngẫu nhiên (sampling) 5% - 10% các lịch sử chat hàng tuần để phát hiện lỗi ảo tưởng tiềm ẩn của AI và cập nhật các rào cản an toàn (guardrails).
