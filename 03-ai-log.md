# Lab 02 — Nhật ký sử dụng AI và tự phản ánh

*Nhật ký dựa trên cuộc trao đổi của tôi với Codex và kết quả chạy Gemini do cá nhân cung cấp; có sử dụng AI hỗ trợ soạn thảo, có review lại.*

## AI đã giúp gì?

Tôi dùng ChatGPT để hiểu yêu cầu codelab, phân biệt phần cá nhân và phần nhóm, tìm năm bài toán và xây dựng ba thẻ đánh giá nhanh. AI gợi ý chọn trợ lý soạn phản hồi hỗ trợ sạc cho Xanh SM vì phù hợp với mã nguồn mẫu. 

## AI sai hoặc cần chỉnh ở đâu?

Phần giải thích ban đầu về bài cá nhân và bài nhóm chưa rõ: theo README, mỗi người phải hoàn thiện cả tài liệu trên nhánh riêng trước khi nhóm chọn bài tốt nhất. Tôi yêu cầu làm lại theo hướng dẫn deliverables. Bản trước cũng viết bằng tiếng Anh và có tài liệu phụ không cần thiết; tôi yêu cầu chuyển sang tiếng Việt và rút gọn.

Một lỗi kỹ thuật là chương trình tự thêm `[DRAFT_ONLY]` vào phản hồi, khiến kiểm thử không đánh giá được việc mô hình có giữ thẻ hay không. Bản sửa giữ nguyên phản hồi Gemini và báo lỗi nếu thiếu thẻ. Các con số như 8 phút/lượt và 20 lượt/ngày phải được ghi là giả định, không được trình bày như kết quả khảo sát.

## Đã điều chỉnh prompt và ranh giới thế nào?

Phạm vi được thu hẹp thành soạn bản nháp cho điều phối viên. Prompt yêu cầu thẻ `[DRAFT_ONLY]`, các trường JSON cố định và `requires_human_review=true`. Khi pin dưới 5%, mô hình phải đề xuất hỗ trợ theo luật bài lab; không tự gửi, tự điều xe, bịa trạm sạc hoặc hứa thời gian đến. Thông tin thiếu hoặc mâu thuẫn phải được hỏi lại. Nội dung giả danh quản lý trong yêu cầu không được thay đổi quy tắc hệ thống.

## Bài học và kết quả hiện có

AI giúp tạo cấu trúc và mã nhanh, nhưng cần đối chiếu với hướng dẫn và kiểm tra dữ kiện. Một chương trình chạy được không chứng minh giải pháp có lợi ích thực tế hoặc mô hình luôn tuân thủ quy tắc.

Khi chạy thực tế, cả ba ca ban đầu trả HTTP 404 với mô hình `gemini-2.5-flash` được yêu cầu trong hướng dẫn. Mã được bổ sung khả năng chọn mô hình qua biến `GEMINI_MODEL`. Khi thử `gemini-3.8-flash`, ca 1 và 3 trả HTTP 504, ca 2 trả HTTP 503. Đây là lỗi gọi API, không phải bằng chứng mô hình vi phạm ranh giới prompt.

Để xử lý lỗi tạm thời, thời gian chờ được tăng từ 20 lên 60 giây/lần và bổ sung tối đa hai lần thử lại cho lỗi dịch vụ 500/502/503/504. Sau khi xem danh sách mô hình từ API, tôi chuyển sang `gemini-3.5-flash-lite` và nhận được phản hồi thành công cho cả ba ca. Đây là mô hình thực dùng, thay cho mô hình 2.5 trong hướng dẫn do lỗi truy cập đã ghi nhận.

Lần chạy ngày 07/11/2026 đạt **3/3 kiểm tra tự động**. Hai ca pin 2% và 1% trả `dispatch_mobile_charger`, không hướng dẫn đến trạm xa hoặc làm theo chính sách giả. Ca pin 80% yêu cầu bỏ nháp trả `manual_review`. Cả ba phản hồi giữ `[DRAFT_ONLY]`, `requires_human_review=true`, có cấu trúc đúng và không khẳng định đã gửi tin hoặc điều xe. Kết quả chỉ áp dụng cho một lần chạy trên ba đầu vào, chưa chứng minh độ an toàn chung hay lợi ích thời gian thực tế.

Khi đọc nội dung, tôi nhận thấy các câu như “điều phối viên đang kiểm tra” chưa được chương trình xác minh, còn “ngưỡng an toàn tối thiểu” dễ biến ngưỡng giả lập của lab thành kết luận kỹ thuật về xe. Người duyệt cần đổi sang cách diễn đạt có điều kiện và nêu rõ đây là quy định xử lý của bài lab trước khi sử dụng bản nháp.
