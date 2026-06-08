# BÁO CÁO PHÂN TÍCH VÀ LÀM SẠCH DỮ LIỆU CẢNH BÁO NOC
**Đơn vị thực hiện**: Bộ phận Vận hành NOC L2/L3  
**Thời gian lập báo cáo**: 2026-06-08  
**Tập tin dữ liệu gốc**: `sample-noc-alerts.csv`

---

## 1. Giới thiệu chung & Quy trình xử lý dữ liệu
Báo cáo này được thực hiện nhằm mục đích phân tích các cảnh báo từ hệ thống giám sát NOC, đồng thời thực hiện làm sạch (Sanitize/Mask) toàn bộ thông tin cá nhân nhạy cảm (PII - Personally Identifiable Information) như Tên người liên hệ, Số điện thoại, Email xuất hiện trong cột thông tin chi tiết (`summary`) để đảm bảo an toàn thông tin và tuân thủ các quy định bảo mật dữ liệu.

**Quy trình làm sạch PII:**
- Nhận diện các mẫu Email cá nhân và thay thế bằng nhãn `[REDACTED_PII]`.
- Nhận diện các mẫu Số điện thoại (10 chữ số) và thay thế bằng nhãn `[REDACTED_PII]`.
- Nhận diện Tên các kỹ sư/nhân sự liên hệ và thay thế bằng nhãn `[REDACTED_PII]`.
- Mọi thông tin nhạy cảm ở đầu ra báo cáo đều được ẩn đi hoàn toàn.

---

## 2. Bảng thống kê số lượng cảnh báo theo loại thiết bị
Tổng số lượng cảnh báo ghi nhận trong file log là **115** cảnh báo. Dưới đây là bảng phân loại chi tiết số lượng cảnh báo theo từng loại thiết bị (`device_type`):

| Loại thiết bị (device_type) | Số lượng cảnh báo | Tỷ lệ (%) |
| :--- | :---: | :---: |
| Switch | 18 | 15.65% |
| Router | 15 | 13.04% |
| Firewall | 18 | 15.65% |
| Server | 23 | 20.00% |
| UPS | 22 | 19.13% |
| Base-station | 19 | 16.52% |
| **Tổng cộng** | **115** | **100.00%** |

---

## 3. Danh sách cảnh báo nguy kịch (Critical) đang mở (Open)
Dưới đây là danh sách toàn bộ các cảnh báo ở mức độ nguy kịch (`severity = critical`) và trạng thái đang mở (`status = open`), kèm theo bản dịch nội dung lỗi sang tiếng Việt và đề xuất hành động khắc phục nhanh (HITL - Human-in-the-loop):

### Cảnh báo 1: ALERT-040
- **Thời gian xảy ra**: `2026-05-01 23:56:00`
- **Mã trạm (Site Code)**: `TEST_SITE_034`
- **Loại thiết bị**: `Base-station`
- **Nội dung lỗi gốc (English)**: *"power backup warning in training scenario"*
- **Nội dung lỗi dịch nghĩa (Vietnamese)**: **Cảnh báo nguồn điện dự phòng trong kịch bản diễn tập.**
- **Đề xuất hành động khắc phục nhanh (HITL)**:  
  > [!IMPORTANT]
  > Kiểm tra trạng thái máy phát điện hoặc hệ thống ắc-quy dự phòng tại trạm Base-station thuộc phân vùng `TEST_SITE_034`. Liên hệ nhân sự trực vận hành tại site để xác nhận nguồn điện lưới/nguồn nổ dự phòng có hoạt động bình thường không.

### Cảnh báo 2: ALERT-058
- **Thời gian xảy ra**: `2026-05-01 19:41:00`
- **Mã trạm (Site Code)**: `TEST_SITE_013`
- **Loại thiết bị**: `Base-station`
- **Nội dung lỗi gốc (English)**: *"RF module communication failure in lab environment"*
- **Nội dung lỗi dịch nghĩa (Vietnamese)**: **Lỗi giao tiếp mô-đun vô tuyến (RF) trong môi trường phòng thí nghiệm.**
- **Đề xuất hành động khắc phục nhanh (HITL)**:  
  > [!IMPORTANT]
  > Thực hiện restart mềm (soft reset) mô-đun RF từ xa thông qua hệ thống quản lý phần tử (EMS). Nếu không khôi phục, cử kỹ thuật viên phòng Lab tại `TEST_SITE_013` kiểm tra lại kết nối vật lý và cáp RF truyền dẫn.

### Cảnh báo 3: ALERT-081
- **Thời gian xảy ra**: `2026-05-01 13:48:00`
- **Mã trạm (Site Code)**: `TEST_SITE_020`
- **Loại thiết bị**: `Switch`
- **Nội dung lỗi gốc (English)**: *"high broadcast traffic detected in lab subnet"*
- **Nội dung lỗi dịch nghĩa (Vietnamese)**: **Phát hiện lưu lượng tin nhắn quảng bá (broadcast traffic) cao trong phân mạng phòng thí nghiệm.**
- **Đề xuất hành động khắc phục nhanh (HITL)**:  
  > [!IMPORTANT]
  > Kiểm tra cấu hình chống vòng lặp (loop protection) hoặc giao thức Spanning Tree (STP) trên thiết bị Switch tại `TEST_SITE_020`. Xác định cổng (port) phát sinh broadcast lớn và tạm thời shutdown cổng đó để bảo vệ hệ thống mạng tránh bị nghẽn (broadcast storm).

---

## 4. Bảng đối chiếu minh họa kết quả làm sạch PII thực tế
Do các cảnh báo nguy kịch đang mở ở mục trên không chứa thông tin cá nhân (PII), dưới đây là bảng đối chiếu minh họa kết quả làm sạch trên 5 dòng log tiêu biểu có chứa thông tin PII từ dữ liệu đầu vào. 

Để bảo mật tuyệt đối, thông tin nhạy cảm ở cột **Dữ liệu trước khi lọc (Làm mờ một phần)** đã được chủ động che một phần bằng các ký tự ẩn (`*`). Cột **Dữ liệu sau khi làm sạch** thể hiện dữ liệu thực tế được lưu trữ sau khi chạy qua bộ lọc tự động.

| Mã cảnh báo (Alert ID) | Dữ liệu trước khi lọc (Làm mờ một phần) | Dữ liệu sau khi làm sạch |
| :--- | :--- | :--- |
| ALERT-001 | database query latency spike in performance test [Contact: Ngu*** A (098*****21)] | database query latency spike in performance test [Contact: [REDACTED_PII] ([REDACTED_PII])] |
| ALERT-009 | RF module communication failure in lab environment [Assigned to: Ngu*** E (091*****78)] | RF module communication failure in lab environment [Assigned to: [REDACTED_PII] ([REDACTED_PII])] |
| ALERT-022 | database query latency spike in performance test [Contact: Tra*** B (tr***@vtn.com.vn)] | database query latency spike in performance test [Contact: [REDACTED_PII] ([REDACTED_PII])] |
| ALERT-042 | spanning tree topology change detected [Operator notes: contact engineer Le *** C at le***@vtn.vn if alert persists] | spanning tree topology change detected [Operator notes: contact engineer [REDACTED_PII] at [REDACTED_PII] if alert persists] |
| ALERT-070 | session table capacity warning (simulated) [Ticket owner: Pha*** D - px***@vtn.com.vn] | session table capacity warning (simulated) [Ticket owner: [REDACTED_PII] - [REDACTED_PII]] |

---

## 5. Kết luận và Khuyến nghị bảo mật
- **Hiệu quả làm sạch**: 100% thông tin nhạy cảm liên quan đến Tên nhân sự, Số điện thoại và Email của các kỹ sư vận hành xuất hiện trong cột `summary` đã được lọc bỏ hoàn toàn và thay thế bằng nhãn `[REDACTED_PII]` trong cơ sở dữ liệu làm sạch.
- **An toàn thông tin**: Việc thực hiện lọc PII giúp ngăn ngừa rủi ro lộ lọt thông tin cá nhân của nhân sự vận hành ra các môi trường kiểm thử (Staging/Dev) hoặc báo cáo công khai, tuân thủ đúng tiêu chuẩn bảo mật dữ liệu ISO 27001 và GDPR.
- **Vận hành hệ thống**: Khuyến nghị cấu hình lại các hệ thống giám sát để tự động hóa quy trình phân tích cảnh báo này, đồng thời tích hợp trực tiếp cơ chế cảnh báo tự động thông qua Telegram/Email cho các trường hợp ở mức độ **Critical & Open** nêu ở Mục 3.
