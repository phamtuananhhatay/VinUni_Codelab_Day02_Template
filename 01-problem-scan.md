# Lab 02 — Quét bài toán và đánh giá nhanh

## Phase 1 — SCAN: Tìm kiếm bài toán

| # | Công ty thành viên | Góc nhìn (Lens) | Mô tả bài toán |
|---|---|---|---|
| 1 | Xanh SM | Tốn thời gian | Điều phối viên phải đọc yêu cầu hỗ trợ sạc, xác nhận tình trạng pin và soạn phản hồi cho tài xế. |
| 2 | Vinhomes | Lặp lại | Nhân viên phải đọc, phân loại và chuyển phản ánh bảo trì của cư dân đến đúng bộ phận. |
| 3 | VinFast | Tốn thời gian | Cố vấn dịch vụ phải tóm tắt mô tả dài của khách hàng thành phiếu tiếp nhận tình trạng xe. |
| 4 | Vinpearl | AI có thể tốt hơn | Nhân viên phải trả lời nhiều câu hỏi tương tự về đặt phòng và chính sách, dựa trên tài liệu được phê duyệt. |
| 5 | Xanh SM | Pain từ người khác | Tài xế và khách phải trao đổi nhiều lần khi mô tả điểm đón không rõ ràng. |

## Phase 2 — QUICK-ASSESS: Ba thẻ bài toán

### Thẻ #1 — Soạn phản hồi hỗ trợ sạc cho tài xế

| Trường | Nội dung |
|---|---|
| **Bài toán (1 câu)** | Giảm thời gian soạn phản hồi hỗ trợ sạc, đồng thời giữ đúng quy định và bước duyệt của điều phối viên. |
| **Công ty thành viên** | Xanh SM. |
| **Ai đang đau (Actor)?** | Điều phối viên mất thời gian xử lý; tài xế phải chờ phản hồi. |
| **Quy trình thủ công hiện tại (3–5 bước)** | 1. Nhận yêu cầu → 2. Xác nhận pin và vị trí → 3. Kiểm tra phương án hỗ trợ → 4. Soạn phản hồi → 5. Duyệt, gửi hoặc chuyển bộ phận hỗ trợ. |
| **Bước tốn thời gian/lỗi nhất** | Bước 3–4, giả định 4 phút/lượt; riêng soạn phản hồi là 2 phút. Tổng quy trình giả định 8 phút/lượt. |
| **AI hỗ trợ ở đâu?** | Bước 4: hiểu nội dung yêu cầu, soạn bản nháp hoặc câu hỏi làm rõ; điều phối viên kiểm tra và duyệt. |
| **Metric có số** | Mục tiêu giảm thời gian soạn từ 2 xuống tối đa 1 phút/lượt; cả 3 ca kiểm thử tấn công phải giữ đúng ranh giới. Cần đo lại thời gian gốc để kiểm chứng. |
| **Kiến trúc sơ bộ** | [ ] Không AI · [ ] Chỉ luật · [x] LLM kết hợp luật và người duyệt · [ ] Agent. |

### Thẻ #2 — Phân loại phản ánh bảo trì của cư dân

| Trường | Nội dung |
|---|---|
| **Bài toán (1 câu)** | Hỗ trợ nhân viên xác định loại sự cố và bộ phận tiếp nhận phản ánh bảo trì. |
| **Công ty thành viên** | Vinhomes. |
| **Ai đang đau (Actor)?** | Nhân viên chăm sóc cư dân và cư dân đang chờ xử lý. |
| **Quy trình thủ công hiện tại (3–5 bước)** | 1. Đọc phản ánh → 2. Xác định sự cố → 3. Kiểm tra mức khẩn cấp → 4. Chọn bộ phận → 5. Tạo và chuyển phiếu. |
| **Bước tốn thời gian/lỗi nhất** | Bước 2–3: hiểu phản ánh mơ hồ hoặc có nhiều sự cố; giả định 3 phút/phiếu trên tổng 6 phút. |
| **AI hỗ trợ ở đâu?** | Đề xuất nhãn phân loại, tóm tắt sự cố và đánh dấu thông tin còn thiếu; nhân viên duyệt trước khi chuyển. |
| **Metric có số** | Đúng ít nhất 90% nhãn trên 30 phản ánh giả lập được người chấm gán nhãn; giảm thời gian phân loại từ 3 xuống tối đa 1,5 phút. |
| **Kiến trúc sơ bộ** | [ ] Không AI · [ ] Chỉ luật · [x] LLM kết hợp luật chuyển tuyến và người duyệt · [ ] Agent. |

### Thẻ #3 — Tóm tắt thông tin tiếp nhận dịch vụ xe

| Trường | Nội dung |
|---|---|
| **Bài toán (1 câu)** | Chuyển mô tả dài của khách thành phiếu tóm tắt triệu chứng xe, không tự suy diễn chẩn đoán. |
| **Công ty thành viên** | VinFast. |
| **Ai đang đau (Actor)?** | Cố vấn dịch vụ chuẩn bị thông tin bàn giao cho kỹ thuật viên. |
| **Quy trình thủ công hiện tại (3–5 bước)** | 1. Đọc mô tả → 2. Trích thông tin xe và triệu chứng → 3. Xác định dữ kiện thiếu → 4. Viết tóm tắt → 5. Bàn giao kỹ thuật viên. |
| **Bước tốn thời gian/lỗi nhất** | Bước 2–4: giữ đủ dữ kiện khi tóm tắt; giả định 4 phút/lượt trên tổng 7 phút. |
| **AI hỗ trợ ở đâu?** | Soạn bản tóm tắt theo biểu mẫu và câu hỏi bổ sung; cố vấn kiểm tra trước khi bàn giao. |
| **Metric có số** | Giữ ít nhất 95% dữ kiện quan trọng trên 20 mô tả giả lập, không thêm dữ kiện bịa; giảm thời gian soạn từ 4 xuống tối đa 2 phút. |
| **Kiến trúc sơ bộ** | [ ] Không AI · [ ] Chỉ luật · [x] LLM có cố vấn duyệt · [ ] Agent. |

## Đề xuất lựa chọn cá nhân

Chọn **thẻ #1: Soạn phản hồi hỗ trợ sạc cho tài xế Xanh SM** để phân tích sâu. Bài toán có phạm vi nhỏ, đầu ra dễ kiểm tra và phù hợp với mã nguồn mẫu của lab. Hai phương án còn lại cần thêm bộ nhãn phân loại hoặc người có chuyên môn kiểm tra dữ kiện. Đây là đề xuất cá nhân để nhóm xem xét.

Cần so sánh với mẫu phản hồi cố định: nếu mẫu có sẵn đáp ứng tốt với chi phí thấp hơn thì không cần dùng LLM. Các chỉ tiêu thời gian ở trên là mục tiêu, chưa phải kết quả đạt được.
