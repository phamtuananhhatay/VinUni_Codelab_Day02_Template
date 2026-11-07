# Phase 1: SCAN & Phase 2: QUICK-ASSESS (Vin Smart Future)

## 1. Bảng quét cơ hội (SCAN List)

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **VinFast** | Lặp lại | Đối chiếu, so khớp dữ liệu hóa đơn sạc điện từ các trạm sạc đối tác bên ngoài với hệ thống kế toán nội bộ. |
| 2 | **Xanh SM** | Tốn thời gian | Điều phối viên xử lý thủ công các báo cáo sự cố xe hết pin/lỗi trạm sạc trên đường để tìm trạm sạc gần nhất hoặc điều xe cứu hộ. |
| 3 | **Vinhomes** | AI-upgrade | Phân loại và route tự động các phản ánh, khiếu nại của cư dân gửi qua app Vinhomes Resident về đúng ban quản lý từng phân khu/tòa nhà. |
| 4 | **Vinmec** | Pain từ người khác | Bác sĩ mất nhiều thời gian tổng hợp kết quả lâm sàng và ghi chú để viết tóm tắt hồ sơ bệnh án xuất viện (Discharge Summary). |
| 5 | **Vinpearl** | Tốn thời gian | Nhân viên đặt phòng mất nhiều thời gian xử lý email đặt phòng đoàn (Group Booking) phức tạp từ các công ty lữ hành để kiểm tra quỹ phòng và báo giá. |

---

## 2. Quick Problem Cards

### QUICK PROBLEM CARD #1
- **Bài toán (1 câu):** Phân loại và chuyển tiếp tự động các phản ánh của cư dân Vinhomes đến đúng bộ phận phụ trách.
- **Công ty thành viên:** [x] Vinhomes
- **Ai đang đau (Actor)?** Nhân viên CSKH/Ban quản lý tòa nhà (quá tải sàng lọc tin nhắn) và cư dân (chờ phản hồi lâu).
- **Workflow thủ công hiện tại:**
  1. Cư dân gửi ticket phản ánh qua ứng dụng Vinhomes Resident.
  2. CSKH trung tâm đọc nội dung và xác định loại vấn đề (điện, nước, an ninh, vệ sinh).
  3. CSKH gán nhãn thủ công và chuyển tiếp ticket đến kỹ thuật viên tòa nhà liên quan.
  4. Kỹ thuật viên tiếp nhận và cập nhật trạng thái xử lý.
- **Bước tốn thời gian/lỗi nhất:** Bước 2 & 3 (⏱ 10 - 15 phút/ticket do lượng tin gửi dồn vào giờ cao điểm).
- **AI can thiệp vào bước nào?** Tự động đọc hiểu ngôn ngữ tự nhiên của cư dân ở bước 2 và tự động gán nhãn/route ở bước 3.
- **Metric thành công:** Giảm thời gian định tuyến ticket từ 15 phút xuống dưới 30 giây; độ chính xác phân loại đạt trên 95%.
- **Quick Architecture:** [x] LLM Feature

---

### QUICK PROBLEM CARD #2
- **Bài toán (1 câu):** Điều phối viên Xanh SM hỗ trợ xử lý sự cố tài xế báo cạn pin hoặc lỗi trụ sạc khẩn cấp giữa đường.
- **Công ty thành viên:** [x] Xanh SM (GSM)
- **Ai đang đau (Actor)?** Tài xế taxi (hoang mang, nguy cơ hủy cuốc) và Điều phối viên (Dispatcher) tại trung tâm vận hành.
- **Workflow thủ công hiện tại:**
  1. Tài xế gọi hotline hoặc nhắn tin khẩn cấp báo sắp hết pin (< 10%).
  2. Điều phối viên tra cứu GPS vị trí xe trên bản đồ quản trị.
  3. Mở dashboard trạm sạc VinFast để tìm trụ sạc tương thích còn trống gần nhất.
  4. Soạn tin nhắn hướng dẫn lộ trình kèm tọa độ gửi về app tài xế.
  5. Liên hệ đội xe cứu hộ pin lưu động nếu xe không đủ pin chạy tới trạm.
- **Bước tốn thời gian/lỗi nhất:** Bước 3 & 4 (⏱ 8 - 12 phút/lượt do phải tra chéo nhiều hệ thống và gõ tay chỉ dẫn).
- **AI can thiệp vào bước nào?** Tự động nhận diện tọa độ, lọc trạm sạc trống phù hợp dòng xe và draft sẵn tin nhắn chỉ đường cho Dispatcher duyệt.
- **Metric thành công:** Rút ngắn thời gian xử lý sự cố từ 15 phút xuống dưới 3 phút; giảm 50% số ca xe phải gọi cứu hộ kéo xe.
- **Quick Architecture:** [x] LLM Feature

---

### QUICK PROBLEM CARD #3
- **Bài toán (1 câu):** Tự động trích xuất thông tin bệnh án để soạn thảo bản tóm tắt hồ sơ xuất viện cho bệnh nhân nội trú Vinmec.
- **Công ty thành viên:** [x] Vinmec
- **Ai đang đau (Actor)?** Bác sĩ điều trị và nhân viên điều dưỡng khoa nội trú.
- **Workflow thủ công hiện tại:**
  1. Bệnh nhân có chỉ định xuất viện.
  2. Bác sĩ tra cứu lại lịch sử xét nghiệm, hình ảnh chẩn đoán, diễn biến bệnh và đơn thuốc trong hệ thống HIS.
  3. Bác sĩ gõ tay bản tóm tắt quá trình điều trị và dặn dò sau xuất viện.
  4. Trưởng khoa ký duyệt hồ sơ.
- **Bước tốn thời gian/lỗi nhất:** Bước 2 & 3 (⏱ 20 - 30 phút/bệnh nhân).
- **AI can thiệp vào bước nào?** RAG đọc dữ liệu bệnh án điện tử và draft sẵn mẫu tóm tắt hồ sơ xuất viện kèm lời dặn bằng ngôn ngữ phổ thông.
- **Metric thành công:** Giảm thời gian soạn thảo hồ sơ từ 25 phút xuống dưới 5 phút/bệnh nhân; 100% bản nháp được bác sĩ chuyên khoa soát lỗi trước khi ban hành.
- **Quick Architecture:** [x] LLM Feature (có RAG)