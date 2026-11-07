# Nhật ký tương tác AI (AI Collaboration Log)

- **Người thực hiện:** Nguyễn Khắc Khánh
- **Vai trò giả định:** AI Product Engineer - Vin Smart Future
- **Công cụ sử dụng:** Gemini / Claude

---

## 1. AI đã giúp gì trong quá trình Scoping?
- **Brainstorm các pain-point thực tế:** Hỗ trợ quét nhanh các điểm nghẽn vận hành đặc thù trong hệ sinh thái Vingroup (VinFast, Xanh SM, Vinhomes, Vinmec) theo đúng 4 Lenses (Lặp lại, Tốn thời gian, AI-upgrade, Pain từ người khác).
- **Đóng vai trò phản biện (Thought-Partner):** Giả lập góc nhìn của Trưởng phòng Vận hành và CFO khắt khe để stress-test các thẻ bài toán, chỉ ra những điểm số liệu metric chưa thực tế và cảnh báo rủi ro vận hành.
- **Chuẩn hóa cấu trúc Problem Statement:** Hỗ trợ tinh chỉnh Problem Statement 6 trường thông tin, đặc biệt là tách bạch rõ ràng giữa phần logic rule-based và phần tác vụ ngôn ngữ tự nhiên dành cho LLM.

---

## 2. Các điểm AI trả lời sai, ảo giác (Hallucination) hoặc chưa thực tế
- **Đề xuất giải pháp quá phức tạp (Over-engineering):** Ban đầu AI gợi ý xây dựng hệ thống "Autonomous Multi-Agent" tự động đưa ra quyết định điều xe và gửi tin nhắn tức thì cho tài xế mà không cần con người can thiệp. Đây là rủi ro cực lớn trong nghiệp vụ vận tải thực tế vì nếu AI định vị sai trạm sạc, xe có thể chết máy giữa đường gây tắc nghẽn giao thông và tai nạn.
- **Số liệu giả định thiếu căn cứ:** AI tự đưa ra các con số tiết kiệm hàng chục tỷ đồng mà không có căn cứ về số lượng cuốc xe hay đơn giá cứu hộ thực tế của Xanh SM.
- **Nhầm lẫn chuẩn sạc kỹ thuật:** Trong phương án đề xuất ban đầu, AI gợi ý điều xe VF8 vào các điểm sạc chậm không phù hợp với công suất cần nạp nhanh của xe dịch vụ.

---

## 3. Cách điều chỉnh Prompt và thiết lập ranh giới (Operational Boundary)
- **Hạ cấp kiến trúc (Downgrade to LLM Feature):** Đã prompt yêu cầu AI giới hạn vai trò: *"Hãy đóng vai trò kỹ sư hệ thống thực tế. Giải pháp không được dùng Multi-Agent tự trị. Hãy tái cấu trúc lại dưới dạng LLM Feature có con người kiểm duyệt (Human-in-the-loop)."*
- **Thiết lập Operational Boundary nghiêm ngặt:** Bổ sung ràng buộc cứng trong system prompt:
  - Bắt buộc mọi output tạo ra phải có tiền tố `[DRAFT_ONLY]` để không thể tự động kích hoạt API gửi tin nhắn nếu chưa có click duyệt của Dispatcher.
  - Đặt điều kiện biên: Nếu pin xe dưới 5%, AI không được phép đề xuất trạm sạc cách xa quá 3km mà phải tự động chuyển hướng sang đề xuất điều xe cứu hộ pin lưu động.
- **Rút ra bài học:** AI rất mạnh trong việc tạo khung ý tưởng và draft nội dung, nhưng người kỹ sư phải nắm chắc quy trình thực tế và luôn đặt ra các rào cản an toàn (Guardrails / Fallback) để kiểm soát kết quả đầu ra.