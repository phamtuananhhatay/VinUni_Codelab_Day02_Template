# Lab 02 — Deliverable 01: Problem Scan & Quick Problem Cards
**Học viên:** Phạm Tuấn Anh  
**Đơn vị:** Vin Smart Future (Vingroup)  
**Mảng nghiên cứu chính:** GSM (Xanh SM) & Hệ sinh thái Vingroup  

---

# 🔍 Phase 1 — SCAN: Bảng quét cơ hội ứng dụng AI trong Vingroup

Sử dụng **4 Lenses** (Lặp lại, Tốn thời gian, AI có thể làm tốt hơn, Nỗi đau của các bên liên quan) để rà soát quy trình vận hành thực tế tại các công ty thành viên:

| # | Công ty thành viên | Lăng kính (Lens) | Tên bài toán / Nghiệp vụ thủ công | Mô tả ngắn bài toán & Điểm nghẽn |
|---|-------------------|------------------|------------------------------------|-----------------------------------|
| 1 | **Xanh SM (GSM)** | **Tốn thời gian** | Điều phối cứu hộ xe điện sắp cạn kiệt pin thực địa | Khi tài xế báo pin còn dưới 5% hoặc kẹt xe giữa đường, điều phối viên mất 15-20 phút gọi điện, dò tìm trạm sạc trống và soạn tin nhắn hướng dẫn thủ công. |
| 2 | **VinFast** | **AI-upgrade** | Phân loại và chẩn đoán sơ bộ mã lỗi xe điện qua mô tả của khách | Khách hàng mô tả lỗi bằng ngôn ngữ tự nhiên (VD: "xe qua gờ giảm tốc kêu cụp cụp ở bánh trước"), CSKH mất nhiều thời gian tra cứu cẩm nang kỹ thuật để phân loại mã lỗi. |
| 3 | **Vinhomes** | **Lặp lại** | Phân loại và điều hướng phản ánh sự cố cư dân trên App Vinhomes Resident | Hằng ngày có hàng ngàn phản ánh (hỏng đèn hành lang, mất nước, tiếng ồn). Nhân viên trực tổng đài phải đọc từng tin và gán thủ công cho ban quản trị tòa nhà tương ứng. |
| 4 | **Vinmec** | **Tốn thời gian** | Tự động hóa soạn thảo bản tóm tắt hồ sơ xuất viện (Discharge Summary) | Bác sĩ lâm sàng mất 20–30 phút/bệnh nhân để tổng hợp các kết quả xét nghiệm, chẩn đoán và đơn thuốc thành một tài liệu tóm tắt dễ hiểu cho bệnh nhân. |
| 5 | **Xanh SM (GSM)** | **Nỗi đau bên ngoài** | Phân tích lý do khách hàng hủy chuyến từ ghi âm và ghi chú tài xế | Tài xế và khách hàng phàn nàn về tình trạng hủy cuốc giờ cao điểm. Đội ngũ QA/Vận hành phải nghe lại hàng trăm đoạn ghi âm thủ công để thống kê lý do. |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Chọn lọc **3 bài toán tiềm năng nhất** từ bảng quét trên để phân tích sơ bộ:

---

### 📇 QUICK PROBLEM CARD #1 (Ưu tiên số 1 — Chọn để Deep-Dive)

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                                       │
│                                                                             │
│ Bài toán (1 câu): Hỗ trợ điều phối viên Xanh SM xử lý báo cáo xe điện       │
│ taxi gặp sự cố pin dưới 5% cần điều xe sạc di động hoặc hướng dẫn trạm sạc. │
│ Công ty thành viên: [x] Xanh SM (GSM)                                       │
│                                                                             │
│ Ai đang đau (Actor)?                                                        │
│ - Điều phối viên (Dispatcher): Quá tải tra cứu bản đồ, định vị và trạm sạc. │
│ - Tài xế Xanh SM: Lo sợ xe chết máy giữa đường, ảnh hưởng an toàn & SLA.    │
│                                                                             │
│ Workflow thủ công hiện tại (5 bước):                                        │
│   1. Nhận báo cáo sự cố qua hotline/app tài xế                              │
│   ──> 2. Tra cứu tọa độ GPS của xe trên phần mềm quản lý đội xe             │
│   ──> 3. Mở cổng thông tin trạm sạc VinFast kiểm tra bán kính & trụ trống   │
│   ──> 4. Soạn thảo tin nhắn SMS/App hướng dẫn lộ trình cho tài xế           │
│   ──> 5. Gọi điện kích hoạt xe cứu hộ/sạc lưu động nếu pin < 5%             │
│                                                                             │
│ Bước nào tốn thời gian/lỗi nhất?                                            │
│   Bước 3 & 4 (⏱ 12 - 15 phút/lượt; dễ sai sót khi tra cứu bán kính di chuyển)│
│                                                                             │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                                       │
│   Bước 4 & 5: Tự động trích xuất % pin, vị trí; draft ngay hành động xử lý  │
│   (dispatch mobile charger hoặc trạm sạc) kèm lý do rõ ràng.                │
│                                                                             │
│ Đo thành công bằng gì (Metric có số)?                                        │
│   - Giảm thời gian xử lý sự cố từ 15 phút ──> dưới 3 phút/cuộc gọi.          │
│   - 100% trường hợp pin < 5% đều được kích hoạt quy trình sạc di động.      │
│                                                                             │
│ Quick Architecture: [x] LLM Feature (Drafting + Rule-based Boundary Guard)   │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

### 📇 QUICK PROBLEM CARD #2

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                                       │
│                                                                             │
│ Bài toán (1 câu): Trợ lý AI phân loại và điều hướng tự động phản ánh của    │
│ cư dân Vinhomes tới đúng bộ phận kỹ thuật / ban quản trị tòa nhà.           │
│ Công ty thành viên: [x] Vinhomes                                            │
│                                                                             │
│ Ai đang đau (Actor)?                                                        │
│ - Nhân viên trực tổng đài Vinhomes Resident: Quá tải hàng ngàn ticket/ngày. │
│ - Cư dân: Chờ đợi phản hồi chậm (trung bình 6-12 tiếng mới có phản hồi).    │
│                                                                             │
│ Workflow thủ công hiện tại (4 bước):                                        │
│   1. Cư dân gửi phản ánh bằng văn bản tự do qua App Vinhomes Resident      │
│   ──> 2. Nhân viên đọc nội dung và phân loại hạng mục (Điện, Nước, An ninh) │
│   ──> 3. Chọn bộ phận tiếp nhận và forward ticket thủ công                  │
│   ──> 4. Soạn tin nhắn xác nhận tiếp nhận gửi cư dân                        │
│                                                                             │
│ Bước nào tốn thời gian/lỗi nhất?                                            │
│   Bước 2 & 3 (⏱ 8 phút/ticket; dễ phân loại sai bộ phận kỹ thuật).          │
│                                                                             │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                                       │
│   Bước 2 & 3: LLM đọc hiểu ngữ cảnh tiếng Việt, tự động tag category và     │
│   gợi ý phòng ban xử lý chính xác.                                          │
│                                                                             │
│ Đo thành công bằng gì (Metric có số)?                                        │
│   - Giảm thời gian định tuyến ticket từ 45 phút ──> dưới 2 phút.            │
│   - Tỉ lệ phân loại chính xác đạt trên 92%.                                 │
│                                                                             │
│ Quick Architecture: [x] LLM Classifier + Router                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

### 📇 QUICK PROBLEM CARD #3

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                                       │
│                                                                             │
│ Bài toán (1 câu): Chẩn đoán sơ bộ mã lỗi kỹ thuật xe điện VinFast từ mô tả  │
│ hiện tượng bất thường bằng ngôn ngữ tự nhiên của chủ xe.                    │
│ Công ty thành viên: [x] VinFast                                             │
│                                                                             │
│ Ai đang đau (Actor)?                                                        │
│ - Khách hàng lái xe điện: Hoang mang khi xe hiện đèn cảnh báo kèm âm thanh lạ│
│ - Kỹ thuật viên tiếp nhận xưởng dịch vụ: Tốn thời gian hỏi han, ghi chép lại│
│                                                                             │
│ Workflow thủ công hiện tại (4 bước):                                        │
│   1. Khách gọi hotline kỹ thuật mô tả âm thanh/triệu chứng xe              │
│   ──> 2. Chuyên viên dịch vụ hỏi đi hỏi lại các điều kiện xảy ra lỗi        │
│   ──> 3. Tra cứu sổ tay lỗi kỹ thuật xe điện VinFast (VF5, VF8, VF9)        │
│   ──> 4. Hẹn giờ tiếp nhận xe và thông báo cho kỹ thuật viên xưởng          │
│                                                                             │
│ Bước nào tốn thời gian/lỗi nhất?                                            │
│   Bước 2 & 3 (⏱ 15 phút/lần trao đổi; khách không rành thuật ngữ kỹ thuật)   │
│                                                                             │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                                       │
│   Bước 3: LLM phân tích mô tả của khách, trích xuất triệu chứng và map với   │
│   kho mã lỗi OBD-II / DTC để đưa ra khuyến cáo ban đầu.                     │
│                                                                             │
│ Đo thành công bằng gì (Metric có số)?                                        │
│   - Rút ngắn thời gian tư vấn ban đầu từ 20 phút ──> dưới 5 phút.           │
│   - Giảm 40% số trường hợp khách đưa xe đến xưởng vì lỗi thao tác đơn giản. │
│                                                                             │
│ Quick Architecture: [x] LLM RAG (Retrieval-Augmented Generation)            │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

# 🗳️ Quyết định lựa chọn bài toán Deep-Dive của nhóm:

Nhóm thống nhất chọn **Quick Problem Card #1: Hỗ trợ điều phối viên Xanh SM xử lý sự cố xe cạn kiệt pin thực địa** làm bài toán trọng tâm.

**Lý do lựa chọn:**
1. **Tính cấp thiết và an toàn:** Sự cố hết pin giữa đường đe dọa trực tiếp đến an toàn giao thông và hình ảnh dịch vụ của Xanh SM.
2. **Operational Boundary rõ ràng:** Bài toán yêu cầu ranh giới an toàn cực kỳ khắt khe: Khi pin < 5%, AI **tuyệt đối không được chỉ đường tới trạm sạc xa** mà phải yêu cầu điều phối xe sạc di động và bắt buộc có con người phê duyệt (`requires_human_approval: true`).
3. **Tính khả thi kỹ thuật:** Hoàn toàn đồng nhất và đã được hiện thực hóa mẫu kiểm thử trong file code `prompt_prototype.py`.

