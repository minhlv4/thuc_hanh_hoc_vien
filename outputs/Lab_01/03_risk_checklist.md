# DANH SÁCH KIỂM TRA RỦI RO (RISK CHECKLIST)
## Tên bài toán: Trợ lý AI Kiểm soát Tiến độ và Chỉ số Dự án cho Quản lý Dự án (PM/Sub-PM)

---

### 1. Phân tích rủi ro & Rào cản an toàn (Guardrails)

Bản rà soát dưới đây tập trung vào việc nhận diện các rủi ro bảo mật thông tin, tuân thủ dữ liệu nhạy cảm (PII) và các rủi ro kỹ thuật vận hành khi ứng dụng AI tổng hợp tiến độ dự án, kèm theo các rào cản an toàn tương ứng.

---

### 2. Chi tiết các rủi ro và giải pháp kiểm soát

#### Rủi ro 01: Rò rỉ thông tin cá nhân nhạy cảm (PII) của nhân sự dự án
* **Mô tả rủi ro:** Tài liệu kế hoạch chi tiết hoặc tin nhắn cập nhật tiến độ có chứa thông tin cá nhân của nhân sự phụ trách (Họ tên thật, số điện thoại cá nhân, địa chỉ email, mức lương, đánh giá hiệu quả công việc chi tiết). Khi đưa dữ liệu này vào mô hình AI, có nguy cơ thông tin cá nhân bị lộ cho người dùng khác không có thẩm quyền hoặc lưu trữ trên máy chủ bên ngoài.
* **Mào cản an toàn (Guardrails) đề xuất:**
  * **Bộ lọc khử nhận dạng (De-identification Filter):** Triển khai một lớp tiền xử lý dữ liệu trước khi gửi tới LLM. Tự động quét và che giấu/mã hóa các thông tin nhạy cảm. Ví dụ: thay thế *"Nguyễn Văn A - 0912345678"* thành *"Nhân sự phụ trách 01"* hoặc *"[HỌ TÊN ĐÃ ẨN]"*.
  * **Phân quyền truy cập dữ liệu (RBAC - Role-Based Access Control):** Giới hạn quyền của tài khoản hỏi AI. PM chỉ được truy cập dữ liệu của dự án mình quản lý. Nhân sự thuộc dự án nào chỉ được hỏi về tiến độ của dự án đó, không được phép hỏi chéo dữ liệu nhân sự dự án khác.

#### Rủi ro 02: Ảo tưởng dữ liệu (Hallucination) về số liệu tiến độ và chỉ số tài chính
* **Mô tả rủi ro:** AI tự động nội suy, bịa đặt hoặc làm tròn số liệu phần trăm hoàn thành lũy kế khi tài liệu đầu vào bị thiếu thông tin hoặc tin nhắn chưa rõ ràng. Ví dụ: Tiến độ thực tế mới đạt 60% nhưng AI suy đoán đạt 80% dựa trên ngữ cảnh mơ hồ của tin nhắn chat, dẫn đến PM báo cáo sai thông tin cho Ban Giám đốc.
* **Rào cản an toàn (Guardrails) đề xuất:**
  * **Ràng buộc sinh dữ liệu nghiêm ngặt (Strict Grounding Prompt):** Thiết lập Prompt hệ thống bắt buộc AI: *"Chỉ trả lời dựa trên thông tin chính xác có trong tài liệu được cung cấp. Tuyệt đối không tự suy đoán số liệu. Nếu không tìm thấy số liệu cụ thể, hãy trả lời: 'Không tìm thấy dữ liệu về tiến độ này trong tài liệu hiện tại, vui lòng liên hệ nhân sự phụ trách để xác nhận'"*.
  * **Cấu hình tham số mô hình tối ưu:** Thiết lập nhiệt độ sáng tạo (Temperature) của mô hình về mức **0** (hoặc tối đa là **0.1**) để đảm bảo tính nhất quán và chính xác của câu trả lời định lượng, triệt tiêu tính sáng tạo của LLM.
  * **Liên kết nguồn gốc (Citation Linkage):** Bắt buộc đầu ra của AI phải trả về đoạn trích dẫn và đường dẫn trực tiếp tới tệp nguồn dữ liệu gốc để PM kiểm tra đối chiếu nhanh.

#### Rủi ro 03: Rò rỉ thông tin bảo mật doanh nghiệp ra môi trường ngoài qua API LLM công cộng
* **Mô tả rủi ro:** Gửi toàn bộ tài liệu Charter dự án, kế hoạch kinh doanh và các chỉ số tài chính mật của tổ chức lên các mô hình LLM công cộng miễn phí trên Cloud mà không có thỏa thuận bảo mật dữ liệu doanh nghiệp, khiến dữ liệu bị sử dụng để huấn luyện lại mô hình công cộng.
* **Rào cản an toàn (Guardrails) đề xuất:**
  * **Sử dụng API LLM phiên bản doanh nghiệp (Enterprise Tier):** Ký kết điều khoản sử dụng API Enterprise có cam kết bảo mật dữ liệu (Data Privacy Agreements), đảm bảo dữ liệu đầu vào của doanh nghiệp không bị ghi nhật ký (no-logging) hoặc sử dụng để huấn luyện/tối ưu mô hình của nhà cung cấp dịch vụ AI.
  * **Ưu tiên giải pháp Hybrid / On-Premise:** Với các dự án mang tính chiến lược hoặc bảo mật cao của tổ chức, cần triển khai các mô hình ngôn ngữ mã nguồn mở chạy local trong hạ tầng mạng nội bộ của đơn vị (Private Cloud/On-Premise LLM).

#### Rủi ro 04: Mâu thuẫn thông tin giữa các nguồn dữ liệu đầu vào khác nhau
* **Mô tả rủi ro:** AI tổng hợp dữ liệu từ hai nguồn mâu thuẫn nhau: Tài liệu Excel chính thức ghi nhận Task hoàn thành 50%, nhưng tin nhắn chat nhanh của kỹ sư báo cáo đã xong 90%. AI tự động tổng hợp không nhất quán hoặc đưa ra con số trung bình không có thực tế.
* **Rào cản an toàn (Guardrails) đề xuất:**
  * **Phân cấp nguồn dữ liệu (Data Hierarchy Rules):** Thiết lập thứ tự ưu tiên cho AI khi xử lý thông tin. Ví dụ: Dữ liệu hệ thống quản lý dự án chính thức > Báo cáo tuần của PM > Tin nhắn chat nhanh của nhân viên.
  * **Cơ chế gắn cờ mâu thuẫn (Conflict Flagging):** Khi phát hiện độ lệch số liệu giữa các nguồn lớn hơn một ngưỡng quy định (ví dụ: >15%), AI được yêu cầu không tự ý đưa ra kết luận trung bình, mà phải hiển thị rõ cả hai nguồn tin và gắn cờ cảnh báo đỏ: *"Lưu ý: Có sự mâu thuẫn tiến độ giữa Kế hoạch chính thức (50%) và Tin nhắn báo cáo nhanh (90%). Khuyến nghị PM xác nhận lại chéo"*.

---

### 3. Tuyên bố tuân thủ (Compliance Statement)

> [!IMPORTANT]
> Toàn bộ dữ liệu sử dụng để huấn luyện thử nghiệm, chạy thử nghiệm MVP và tài liệu hướng dẫn này đều sử dụng **dữ liệu mô phỏng hoàn toàn**.
> Tuyệt đối không sử dụng tên thật của nhân sự, tên thật của khách hàng, số điện thoại thật hoặc bất kỳ thông tin hạ tầng thực tế nào của VTN trong mọi hoạt động thử nghiệm AI.
