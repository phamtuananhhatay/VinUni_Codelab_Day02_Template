# 🔍 Phase 1 — SCAN

| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 |Vinhomes |CSKH & Vận hành (NLP/LLM) |Tự động phân loại, đánh giá mức độ ưu tiên và điều phối hàng ngàn ticket yêu cầu của cư dân đến đúng nhân sự phụ trách. |
| 2 |Vinhomes |Quản lý Năng lượng (IoT/RL) |Tự động điều chỉnh hệ thống HVAC và chiếu sáng công cộng theo thời gian thực dựa trên thời tiết và mật độ người để chống lãng phí điện năng. |
| 3 |Vinhomes |An ninh & Trật tự (Computer Vision) |Phân tích luồng camera CCTV tự động phát hiện vi phạm nội khu (đỗ xe sai, xả rác, đột nhập) và cảnh báo tức thời cho đội tuần tra. |
| 4 |Vinhomes |Kỹ thuật Tòa nhà (Machine Learning) |Phân tích dữ liệu cảm biến (rung chấn, nhiệt) để dự đoán trước thời điểm hỏng hóc của thang máy, máy bơm, chuyển từ bảo trì định kỳ sang bảo trì dự đoán. |
| 5 |Vinhomes |Hành chính & BQL (OCR/GenAI) |Tự động số hóa, trích xuất và đối chiếu chéo dữ liệu từ hồ sơ bàn giao, giấy tờ tùy thân, bản vẽ thi công để loại bỏ thao tác nhập liệu thủ công. |
---

# 🃏 Phase 2 — QUICK-ASSESS

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #001                                     │
│                                                             │
│ Bài toán (1 câu): Phân loại và điều phối tự động hàng ngàn  │
│ ticket yêu cầu của cư dân đến đúng nhân sự phụ trách.       │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Nhân sự CSKH (Ban Quản Lý)             │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Cư dân gửi Text ──> 2. CSKH đọc/hiểu ──> 3. Phân loại  │
│   & Chọn ưu tiên ──> 4. Gán thủ công cho phòng ban liên quan│
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? 2 & 3 (⏱ 3-5 phút/lượt)    │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3 (NLP/LLM   │
│ tự động đọc hiểu, trích xuất thông tin, gắn tag và gán task)│
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian điều   │
│ phối ticket từ 5 phút ──> dưới 1 giây, tiết kiệm 100h/ngày. │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #002                                     │
│                                                             │
│ Bài toán (1 câu): Tự động hóa điều chỉnh HVAC và chiếu sáng │
│ công cộng theo thời gian thực để chống lãng phí điện năng.  │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Kỹ sư trưởng MEP & Giám đốc Vận hành   │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Xem dự báo thời tiết/Cài đặt theo lịch tĩnh (Timer) ──>│
│   2. Ngồi tại phòng BMS ──> 3. Tăng/giảm công suất Chiller, │
│   bật/tắt đèn thủ công ──> 4. Nhận phàn nàn nóng/lạnh để sửa│
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 1 & 3 (Chạy tĩnh 1    │
│ mức công suất bất chấp thực tế, gây lãng phí năng lượng lớn)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1, 3 & 4 (AI RL  │
│ đọc sensor nhiệt/mật độ người để tự động điều khiển qua BMS)│
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm 15% - 20% tổng   │
│ chi phí hóa đơn điện cho hệ thống HVAC/chiếu sáng mỗi tháng.│
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [x] Agent │
└─────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #003                                     │
│                                                             │
│ Bài toán (1 câu): Phân tích luồng camera tự động phát hiện  │
│ vi phạm nội khu và cảnh báo tức thời cho đội an ninh.       │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Bảo vệ trực CCTV & Trưởng ban An ninh  │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Bảo vệ nhìn chăm chú hàng chục màn hình CCTV ──> 2.    │
│   Dùng mắt tìm điểm bất thường ──> 3. Bộ đàm cho tuần tra   │
│   ──> 4. Tuần tra di chuyển đến hiện trường để xử lý        │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 (⏱ Mất tập trung    │
│ sau 20 phút, tỷ lệ bỏ lọt vi phạm cao, phản ứng chậm trễ)   │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1 & 2 (Computer  │
│ Vision chạy 24/7, tự highlight vi phạm và bắn pop-up alert) │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Tăng tỷ lệ bắt lỗi    │
│ vi phạm >90%; Giảm 30% chi phí nhân sự trực phòng camera.   │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [x] Agent │
└─────────────────────────────────────────────────────────────┘
