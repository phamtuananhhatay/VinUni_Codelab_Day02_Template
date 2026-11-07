# Problem Scan & Quick Assessment — Xanh SM Smart Dispatching

> **Trạng thái:** Bản nháp cho nhóm. Use case chính do nhóm chọn là tối ưu điểm đón từ tin nhắn tài xế và tọa độ GPS. Nguồn công khai xác nhận luồng đặt xe/đổi điểm đón phía khách hàng; nguồn không mô tả quy trình xử lý tin nhắn nội bộ của điều phối viên. Các bước nội bộ bên dưới là giả thuyết cần xác nhận; con số trong Quick Cards là mục tiêu pilot, không phải số liệu baseline đã đo.

## Phase 1 — SCAN

| # | Công ty | Lens | Bài toán vận hành |
|---:|---|---|---|
| 1 | Xanh SM | Tốn thời gian | Điều phối viên đọc tin nhắn tiếng Việt tự nhiên của tài xế, đối chiếu với GPS và chuyến đang chạy để chọn hoặc điều chỉnh điểm đón phù hợp. |
| 2 | VinFast | Lặp lại | Đối chiếu phiên sạc tại trạm đối tác với hóa đơn và dữ liệu giao dịch; chuyển các dòng lệch cho nhân viên tài chính kiểm tra. |
| 3 | Vinhomes | Lặp lại | Phân loại phản ánh cư dân gửi qua ứng dụng và chuyển đến đúng đội vận hành/tòa nhà. |
| 4 | Vinmec | Tốn thời gian | Soạn bản nháp tóm tắt xuất viện từ bệnh án và ghi chú đã được bác sĩ xác nhận; bác sĩ duyệt trước khi phát hành. |
| 5 | Vinpearl | Pain từ người khác | Phát hiện và ưu tiên các đánh giá có phản ánh khẩn cấp về phòng, vệ sinh hoặc trải nghiệm để quản lý xử lý. |
| 6 | Vinpearl | AI-upgrade | Tạo bản nháp trả lời câu hỏi tự do về giờ nhận phòng, tiện ích và thủ tục từ chính sách đã duyệt, giúp nhân viên điều chỉnh phản hồi theo ngữ cảnh. |

Sáu cơ hội trên bao phủ đủ bốn lenses: Lặp lại, Tốn thời gian, AI-upgrade và Pain từ người khác. Đây là các giả thuyết scoping; chưa phải kết luận khảo sát nội bộ tại các công ty.

## Phase 2 — QUICK-ASSESS

### Card #1 — Xanh SM Smart Dispatching (use case nhóm chọn)

- **Bài toán:** Điều phối viên mất thời gian diễn giải tin nhắn tài xế và ghép thông tin đó với GPS/chuyến hiện tại để xác định điểm đón phù hợp.
- **Actor:** Điều phối viên; tài xế chịu ảnh hưởng khi phải chờ xác nhận hoặc nhận điểm đón chưa phù hợp.
- **Workflow hiện tại (giả thuyết cần xác minh):** 1) Nhận tin nhắn và vị trí GPS; 2) làm rõ ý định nếu tin nhắn mơ hồ; 3) đối chiếu GPS, chuyến hiện tại và điểm đón; 4) chọn phương án; 5) cập nhật hệ thống và phản hồi tài xế.
- **Bottleneck:** Diễn giải mô tả không theo cấu trúc và đối chiếu thủ công với dữ liệu vị trí. Thời gian/lượt: **chưa đo**.
- **AI hỗ trợ:** Trích xuất ý định/ràng buộc từ tin nhắn và tạo bản nháp gợi ý; phép tính khoảng cách, kiểm tra vùng phục vụ và cập nhật dispatch vẫn thuộc hệ thống quy tắc/con người.
- **Metric đề xuất cho pilot:** Giảm ít nhất **30%** median thời gian từ lúc nhận tin đến khi điều phối viên duyệt phương án, so với baseline đo trên cùng loại tình huống. Ít nhất **90%** gợi ý được điều phối viên đánh giá là dùng được sau khi kiểm tra; **0** thay đổi được thực hiện nếu chưa có phê duyệt.
- **Quick architecture:** LLM Feature để hiểu ngôn ngữ + Rule/geo checks để xác thực vị trí; Human-in-the-loop trước mọi cập nhật.
- **Căn cứ công khai:** Hướng dẫn Green SM cho biết khách nhập điểm đón/điểm đến hoặc dùng GPS, gửi yêu cầu, hệ thống tìm tài xế gần đó và báo khi tài xế nhận chuyến; khách có thể gọi/nhắn tài xế để xác nhận điểm đón ([Trung tâm giải đáp Green SM](https://greensm.com/vn-vi/helps)). Green SM có thông báo tính năng khách đổi điểm đón sau khi tài xế đã nhận chuyến và đang trên đường đến trong bản cập nhật **ứng dụng Tài xế Green SM Bike** ([cập nhật ứng dụng ngày 08/09/2026](https://www.greensm.com/vn-vi/news/cap-nhat-cac-tinh-nang-moi-tren-ung-dung-tai-xe-green-sm)); không suy rộng tính năng này cho taxi nếu chưa xác minh. Các nguồn không xác nhận hàng đợi tin nhắn tài xế cho điều phối viên hoặc thời gian xử lý nội bộ.

### Card #2 — Vinhomes phân loại phản ánh cư dân

- **Bài toán:** Nhân viên đọc từng phản ánh, nhận diện loại sự cố/tòa nhà và chuyển việc cho đúng đội.
- **Actor:** Nhân viên chăm sóc cư dân và đội kỹ thuật/vận hành; cư dân chờ phản hồi.
- **Workflow hiện tại (giả thuyết):** Nhận phản ánh → đọc nội dung → xác định loại việc và mức ưu tiên → tìm đội phụ trách → chuyển và theo dõi.
- **Bottleneck:** Phản ánh tự do có thể thiếu thông tin về vị trí hoặc loại sự cố; thời gian/lượt cần đo.
- **AI hỗ trợ:** Gợi ý nhãn, mức ưu tiên và câu hỏi cần bổ sung; nhân viên xác nhận trước khi chuyển.
- **Metric đề xuất:** Đạt ít nhất **90%** phân loại đúng trên bộ mẫu được nhân viên gán nhãn; không tự gửi cam kết về thời hạn xử lý.
- **Quick architecture:** Rule-based routing cho danh mục rõ ràng; LLM chỉ trích xuất nội dung không theo mẫu.

### Card #3 — VinFast đối chiếu hóa đơn sạc

- **Bài toán:** Nhân viên tài chính đối chiếu dữ liệu phiên sạc với hóa đơn từ nhiều đối tác.
- **Actor:** Nhân viên kế toán/đối soát.
- **Workflow hiện tại (giả thuyết):** Nhận file → chuẩn hóa mã giao dịch/thời gian/số tiền → ghép phiên sạc với hóa đơn → đánh dấu dòng lệch → nhân viên xử lý ngoại lệ.
- **Bottleneck:** Ghép dòng dữ liệu và xử lý định dạng khác nhau; khối lượng và thời gian cần đo.
- **AI hỗ trợ:** Trích xuất trường từ hóa đơn không đồng nhất; phép tính và đối chiếu số tiền nên dùng quy tắc xác định.
- **Metric đề xuất:** Đạt ít nhất **98%** độ chính xác trích xuất trường trên bộ hóa đơn được gán nhãn trước khi cân nhắc xử lý tự động; mọi sai lệch tài chính vẫn cần nhân viên duyệt.
- **Quick architecture:** Rule-based matching + LLM/OCR chỉ cho tài liệu không cấu trúc.

## Lựa chọn để Deep-Dive

Nhóm chọn **Card #1 — Xanh SM Smart Dispatching**. Phạm vi prototype được thu hẹp thành gợi ý cho điều phối viên dựa trên tin nhắn tài xế và GPS đã xác minh. Nguồn công khai giúp xác nhận bối cảnh khách có thể liên hệ tài xế và đổi điểm đón sau khi tài xế nhận chuyến; nhóm vẫn cần xác nhận quy trình điều phối nội bộ và đo baseline trước khi nộp bản cuối.

- **Ưu tiên Card #1:** Đây là case nhóm đã chọn; phần hiểu tin nhắn có thể thử bằng dữ liệu tổng hợp, với đầu ra nháp và người duyệt làm ranh giới rõ ràng. Chưa khẳng định đã có quyền truy cập GPS hoặc log thật.
- **Chưa chọn Card #2:** Cần danh mục sự cố, ma trận trách nhiệm theo tòa nhà và dữ liệu gán nhãn của ban quản lý; project hiện chưa có các dữ liệu đó để đánh giá chất lượng routing.
- **Chưa chọn Card #3:** Phần ghép mã giao dịch và tính sai lệch số tiền ưu tiên quy tắc xác định. Chỉ cân nhắc LLM/OCR nếu có hóa đơn không đồng nhất và bộ mẫu được phép dùng, nên chưa cần một prototype LLM cho toàn bộ luồng.

## Kiểm chứng kỹ thuật ban đầu

Ngày **06/11/2026**, ba case adversarial của Card #1 được gọi live một lần/case trên **`gemini-3.5-flash-lite`** qua Interactions API. Cả ba giữ `[DRAFT_ONLY]`, trả JSON hợp lệ và `suggested_pickup=null`; hai case về quyền duyệt/GPS yêu cầu người xem xét, case pin 2% tạo yêu cầu nháp `dispatch_mobile_charger`. Kết quả kiểm tra cấu trúc/hành động và đọc nội dung bởi Codex: **3/3 PASS trong mẫu này**.

Model `gemini-3.8-flash` được thử trước nhưng gặp lỗi API 504/503, nên chưa đánh giá được ranh giới trên model đó. Xem [kết quả chi tiết và nguyên output](02-deep-dive-report.md#4-prototype--boundary-tests) cùng [log JSON](test-results/boundary-tests-20261106-231359.json).

Thời gian lời gọi API trung bình 2,765 giây không phải baseline điều phối. Các mục tiêu giảm 30% thời gian và đạt 90% gợi ý dùng được vẫn là đề xuất pilot; chưa có dữ liệu vận hành để kết luận đã đạt.

Ngày **07/11/2026**, cấu hình mặc định trong `.env` được đổi sang 3.5 Flash-Lite. Chạy lại đúng lệnh không có tham số model: cùng ba case tiếp tục đạt, exit code 0; toàn bộ tiến trình mất 6,516 giây. Xem [output lần chạy mặc định](test-results/prototype-default-20261107-070231.log). Đây là lượt lặp lại cùng ba tình huống, chưa mở rộng bộ dữ liệu.
