# Lab 02 — Phase 1 & Phase 2: Problem Scan & Quick Problem Cards (Vin Smart Future)

---

# 🔍 Phase 1 — SCAN: Tìm kiếm cơ hội (Cá nhân)

Dùng **4 Lenses** quét qua vận hành của 5 công ty thành viên Vingroup:

### 📝 List bài toán của tôi:

| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|---|---|---|
| 1 | **Xanh SM** | Tốn thời gian | Điều phối viên tra cứu thủ công GPS xe, tìm trụ sạc trống và gõ tin nhắn chỉ dẫn khi tài xế báo sự cố pin thực địa (mất 15 phút/lượt, rò rỉ 15% doanh thu ca chạy). |
| 2 | **VinFast** | AI có thể tốt hơn | Cố vấn dịch vụ dịch thủ công mô tả triệu chứng tiếng Việt đời thường của chủ xe sang mã lỗi kỹ thuật để xếp lịch xưởng (mất 15 phút/ca, 22% xếp nhầm khoang sửa chữa). |
| 3 | **Vinhomes** | Lặp lại | Nhân viên BQL đọc và phân loại thủ công ~2.000 phản ánh cư dân/ngày trên App Vinhomes Resident về đúng bộ phận kỹ thuật/an ninh (mất 8 phút/ticket, phản hồi chậm 4–12 giờ). |
| 4 | **Vinmec** | Pain từ người khác | Bác sĩ nội trú phải tổng hợp 4–5 màn hình bệnh án điện tử (EMR) để soạn Tóm tắt hồ sơ xuất viện cho bệnh nhân (mất 25–30 phút/bệnh nhân, gây quá tải giờ xuất viện). |
| 5 | **Vinpearl** | Tốn thời gian | Nhân viên Đặt phòng đoàn đọc email yêu cầu đoàn phức tạp từ đại lý lữ hành để kiểm tra quỹ phòng trống và nhập tay vào PMS (mất 30 phút/đoàn, chậm báo giá làm mất 10% khách). |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards (Cá nhân)

Chọn Top 3 từ danh sách SCAN: **#1 (Xanh SM Sự cố sạc pin)**, **#2 (VinFast Chẩn đoán lỗi xe)**, **#3 (Vinhomes Phản ánh cư dân)**.

### 1. Card #1 — Xanh SM: Xử lý sự cố sạc pin & cứu hộ thực địa

| Trường thông tin | Nội dung ngắn gọn |
|---|---|
| **Bài toán (1 câu)** | Tài xế Xanh SM báo sự cố pin yếu giữa đường cần điều phối viên tra cứu trụ sạc trống gần nhất hoặc điều xe cứu hộ pin. |
| **Công ty thành viên** | **Xanh SM (GSM)** |
| **Ai đang đau (Actor)?** | Tài xế (chờ đợi lâu ngoài đường) & Điều phối viên (quá tải giờ cao điểm). |
| **Workflow thủ công (5 bước)** | `1. Nhận cuộc gọi báo hết pin` → `2. Tra vị trí GPS & % pin xe` → `3. Tìm trụ sạc VinFast còn trống` → `4. Gõ tin nhắn chỉ dẫn gửi App tài xế` → `5. Gọi xe cứu hộ nếu pin < 5%` |
| **Bước tốn thời gian nhất** | Bước 3 & Bước 4 (⏱ **10–12 phút/lượt**). |
| **AI hỗ trợ ở bước nào?** | Bước 3 & 4: Tự động lấy vị trí + trụ trống → Soạn nháp tin nhắn `[DRAFT_ONLY]` hoặc đề xuất `dispatch_mobile_charger` nếu pin $< 5\%$. |
| **Metric thành công (có số)** | Giảm thời gian xử lý sự cố từ **15 phút ──> dưới 3 phút/lượt**. |
| **Quick Architecture** | **[x] LLM Feature** *(Tự động soạn chỉ dẫn nháp + Rule kiểm tra ngưỡng pin)* |

---

### 2. Card #2 — VinFast: Chẩn đoán sơ bộ lỗi xe từ mô tả tiếng Việt

| Trường thông tin | Nội dung ngắn gọn |
|---|---|
| **Bài toán (1 câu)** | Cố vấn dịch vụ mất nhiều thời gian dịch mô tả tiếng Việt đời thường của chủ xe sang mã lỗi kỹ thuật khi đặt lịch bảo dưỡng. |
| **Công ty thành viên** | **VinFast** |
| **Ai đang đau (Actor)?** | Cố vấn dịch vụ xưởng & Chủ xe điện VinFast (VF5, VF8, VF9). |
| **Workflow thủ công (4 bước)** | `1. Khách mô tả hiện tượng xe` → `2. Cố vấn tra cẩm nang mã lỗi kỹ thuật` → `3. Kiểm tra phụ tùng & khoang sửa chữa` → `4. Xác nhận lịch hẹn` |
| **Bước tốn thời gian nhất** | Bước 2 (⏱ **10 phút/lượt**). |
| **AI hỗ trợ ở bước nào?** | Bước 2: Phân tích mô tả tiếng Việt tự nhiên và gợi ý Top 3 mã lỗi kỹ thuật sơ bộ cho Cố vấn duyệt. |
| **Metric thành công (có số)** | Giảm thời gian phân loại lỗi ban đầu từ **15 phút ──> dưới 4 phút/lượt**. |
| **Quick Architecture** | **[x] LLM Feature** |

---

### 3. Card #3 — Vinhomes: Phân loại & điều hướng phản ánh cư dân

| Trường thông tin | Nội dung ngắn gọn |
|---|---|
| **Bài toán (1 câu)** | Nhân viên BQL Vinhomes phải đọc và phân loại thủ công hàng nghìn ticket khiếu nại/yêu cầu của cư dân mỗi ngày trên App. |
| **Công ty thành viên** | **Vinhomes** |
| **Ai đang đau (Actor)?** | Nhân viên CSKH Ban Quản lý & Cư dân Vinhomes. |
| **Workflow thủ công (4 bước)** | `1. Cư dân gửi phản ánh qua App` → `2. CSKH đọc & phân loại bộ phận xử lý` → `3. Tạo phiếu việc cho Kỹ thuật/An ninh` → `4. Gõ tin nhắn phản hồi cư dân` |
| **Bước tốn thời gian nhất** | Bước 2 & Bước 4 (⏱ **6 phút/ticket**). |
| **AI hỗ trợ ở bước nào?** | Bước 2 & 4: Tự động gắn nhãn bộ phận phụ trách, mức độ khẩn cấp và soạn nháp tin nhắn phản hồi. |
| **Metric thành công (có số)** | Giảm thời gian xử lý ticket từ **8 phút ──> dưới 1.5 phút/ticket**. |
| **Quick Architecture** | **[x] LLM Feature** |

---

# 🗳️ Quyết định lựa chọn cho Phase 3 (Deep-Dive)

* **Bài toán được chọn:** **Card #2 — VinFast: Chẩn đoán sơ bộ lỗi xe từ mô tả tiếng Việt**.
* **Lý do chọn Card #2 và loại các thẻ còn lại:**
  * **Card #2 (VinFast):** Tận dụng đúng thế mạnh cốt lõi nhất của LLM là xử lý ngôn ngữ tự nhiên tiếng Việt đa dạng/khẩu ngữ của chủ xe (*"kêu lục cục ở bánh trước"*, *"hụt ga"*, *"báo đèn vàng hình con rùa"*), giải quyết trực tiếp điểm nghẽn ~400 ca đặt lịch xưởng/ngày và giảm tỷ lệ 22% xếp nhầm khoang sửa chữa.
  * **Card #1 (Xanh SM):** Phần lớn logic kiểm tra ngưỡng pin ($< 5\%$) và khoảng cách ($< 5\text{km}$) mang tính toán học xác định (Rule-based), ít phức tạp về mặt ngôn ngữ tự nhiên hơn so với chẩn đoán triệu chứng kỹ thuật của VinFast.
  * **Card #3 (Vinhomes):** Tác vụ phân loại ticket hành chính nội khu có rủi ro vướng các khiếu nại pháp lý/hợp đồng, cần chuẩn hóa quy trình SOP của từng ban quản lý trước.
