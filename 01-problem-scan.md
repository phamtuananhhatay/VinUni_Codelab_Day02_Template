# Lab 02 — Quét bài toán và đánh giá nhanh

## Phase 1 — SCAN: Tìm kiếm bài toán
| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 | **Vinmec** | AI-upgrade | Khách hàng phải hỏi nhiều lượt để biết thủ tục khám, giấy tờ cần mang và cách đặt lịch. Trợ lý AI có thể trả lời theo hướng dẫn được bệnh viện phê duyệt, hỏi rõ nhu cầu và chuyển nhân viên khi cần.|
| 2 | **Vinhomes** | Tốn thời gian | Ban quản lý đọc phản ánh của cư dân, tra lịch sử xử lý và soạn phản hồi cho từng trường hợp. AI có thể tóm tắt hồ sơ, phân loại yêu cầu và dự thảo phản hồi theo thông tin thực tế. |
| 3 | **Xanh SM** |Pain từ người khác| Bảo dưỡng xe | Tài xế đã đặt lịch bảo dưỡng nhưng khi đến xưởng vẫn phải xếp hàng hoặc chờ phụ tùng thay thế, khiến xe ngừng hoạt động lâu hơn dự kiến và giảm thời gian chạy xe kiếm thu nhập. |  
| 4 | **Vinhome** |Lặp lại (Repetitive)| Nhân viên ban quản lý phải đọc từng yêu cầu của cư dân, phân loại như điện nước, vệ sinh, an ninh rồi chuyển đến bộ phận phụ trách. Thao tác này lặp lại hằng ngày trên nhiều yêu cầu, AI có thể tự động phân loại và đề xuất bộ phận tiếp nhận. |
| 5 |**VinFast**| Pain từ người khác | Khách hàng không biết hoặc hiểu nhầm ngày hết ưu đãi, bất ngờ khi phát sinh tiền sạc và phải liên hệ nhiều lần để được giải thích. |
## Phase 2 — QUICK-ASSESS: Ba thẻ bài toán

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│Bài toán (1 câu):  Khách hàng phải hỏi nhiều lượt để biết thủ|
| tục khám, giấy tờ cần mang và cách đặt lịch, khiến việc     |
| chuẩn bị đi khám mất thời gian và nhân viên phải giải đáp   |
| lại những câu hỏi tương tự.                                 |
│ Công ty thành viên: [x] VinMec                              │
│                                                             │
│                                                             │
│ Ai đang đau (Actor)? Khách hàng (phải tìm hiểu, chuẩn bị    |
| giấy tờ), Nhân viên (hỏi lại thông tin, giải thích)         │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Khách hàng hỏi về thủ tục và đặt lịch khám.            |
|   ->2. Nhân viên hỏi thêm cơ sở muốn khám, dịch vụ và       |
|        thông tin liên quan.                                 |
|   ->3. Nhân viên tra cứu hướng dẫn, giấy tờ và quy trình    |
|        phù hợp.                                            |
|   ->4. Nhân viên trả lời, khách hàng hỏi bổ sung thông      |
|       tin chưa đầy đủ.                                      |
|   ->5. Nhân viên hướng dẫn đặt lịch hoặc chuyển bộ          |
|        phận tiếp nhận.                                      │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-4(⏱ 30 phút/lượt)      │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-4(Đọc nội dung 
| khách hàng gửi, truy xuất hướng dẫn được bệnh viện phê duyệt|
| và tạo danh sách giấy tờ, bước chuẩn bị phù hợp.)     |
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian phân loại|
| và tiếp nhận từ 30 phút xuống 5 phút                          │
│   VD: "Giảm thời gian soạn phản hồi từ 10 min ──> under 2 min"│
│                                                             │
│ Quick Architecture: [x] LLM(Tự động soạn hướng dẫn và danh | 
|                 sách chuẩn bị)                                │
└─────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│Bài toán (1 câu):  Tài xế đã đặt lịch bảo dưỡng nhưng khi    |
| đến xưởng vẫn phải xếp hàng hoặc chờ phụ tùng thay thế,     |
| khiến xe ngừng hoạt động lâu hơn dự kiến và giảm thời       |
| gian chạy xe kiếm thu nhập.                                 |
│ Công ty thành viên: [x] Xanh SM                             │
│                                                             │
│                                                             │
│ Ai đang đau (Actor)? Tài xế (chờ đợi), Nhân viên (giải thích|
|                         đổi lịch, xử lý tồn đọng)           │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Tài xế đặt lịch bảo dưỡng trên hệ thống                |
|   ─> 2. Hệ thống xác nhận lịch bảo dưỡng                    |
|   ─> 3. Tài xế mang xe đến xưởng bảo dưỡng để kiểm tra, xe| 
|         xếp hàng chờ đến lượt                             |   
|   ─> 4. phát hiện cần thay phụ tùng nhưng xưởng không có    |
|         sẵn   
|   ─> 5. Xưởng hoàn tất dịch vụ, bàn giao xe               │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 - 4(⏱ 60 phút - vài| 
|                                             ngày/lượt)      │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1–2(Dự báo thời |
|lượng bảo dưỡng và phụ tùng có khả năng cần dùng từ số km, |
|lịch sử bảo dưỡng và triệu chứng tài xế cung cấp. Kết hợp dữ |
|liệu năng lực xưởng và tồn kho để đề xuất giờ hẹn hoặc xưởng |
|phù hợp), Bước 3–4(Cập nhật thời gian hoàn thành dự kiến khi |
|phát sinh chậm trễ.)                                         |
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian chờ tiếp|
|nhận từ 60 xuống 30 phút                                      │
│   VD: "Giảm thời gian soạn phản hồi từ 10 min ──> under 2 min"│
│                                                             │
│ Quick Architecture: [ ] No AI  [x]Rule                      │
└─────────────────────────────────────────────────────────────┘
```
```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #4                                       │
│                                                             │
│Bài toán (1 câu):  Nhân viên ban quản lý phải đọc từng yêu   |
|cầu của cư dân, phân loại như điện nước, vệ sinh, an ninh rồi| 
|chuyển đến bộ phận phụ trách.                                 |
│ Công ty thành viên: [x] Vinhomes                            │
│                                                             │
│                                                             │
│ Ai đang đau (Actor)? Cư dân (chờ đợi), Nhân viên (lọc, phân |
|                         loại, chuyển yêu cầu)               │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Cư dân gửi yêu cầu đến ban quản lý                     |
|   ─> 2. Nhân viên đọc và kiểm tra thông tin, vị trí xảy ra|
|         vấn đề                                              |
|   ─> 3. Nhân viên phân loại yêu cầu và chuyển cho bộ phận |
|         phụ trách                                           |   
|   ─> 4. Bộ phận phụ trách tiếp nhận và kiểm tra             |
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 - 4(⏱ 15 phút/      |
|                                                   lượt)      │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-4(Đọc, phân loại|
| và chuyển yêu cầu đến bộ phận phụ trách vấn đề)              |
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian chờ tiếp|
|nhận từ 15 xuống 5 phút                                      │
│   VD: "Giảm thời gian soạn phản hồi từ 10 min ──> under 2 min"│
│                                                             │
│ Quick Architecture: [x]Agent(Tự động phân loại |
|và gửi yêu cầu cho bộ phận phụ trách )                      │
└─────────────────────────────────────────────────────────────┘
```

## Đề xuất lựa chọn cá nhân
Chọn bài toán **"Card #3 - Giảm thời gian chờ bảo dưỡng xe đã đặt lịch"** để thực hiện Deep-Dive.

## Lý do lựa chọn và loại bỏ các thẻ khác:
* **Card #1 — Vinmec**: Nhóm cần có tài liệu hướng dẫn được bệnh viện phê duyệt và cập nhật để bảo đảm câu trả lời chính xác. Các câu hỏi thủ tục thông thường cũng có thể được xử lý bằng biểu mẫu và checklist.

* **Card #4 — Vinhomes**: Bài toán dễ thử nghiệm nhưng việc cho cư dân chọn nhóm yêu cầu và định tuyến bằng quy tắc có thể đã giải quyết phần lớn thao tác thủ công.
---