# 🏗️ Phase 3 — DEEP-DIVE
## 3.2. Problem Statement (6-field) & Metrics (15 min)
Điền đầy đủ 6 trường thông tin của bài toán:

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Nhân viên CSKH của Ban quản lý Vinhomes.|
| **2. Current Workflow** | Thủ công hoàn toàn. CSKH trực màn hình hệ thống, đọc từng khiếu nại/sự cố, đánh giá mức độ nghiêm trọng và click chọn đội ngũ kỹ thuật tương ứng để chuyển tiếp. |
| **3. Bottleneck** | Đọc hiểu văn bản mô tả của cư dân (đôi khi viết tắt, viết không rõ ràng) và phân tích ảnh chụp sự cố. |
| **4. Business Impact** | Giảm sự hài lòng (CSAT) của cư dân vì xử lý chậm. Tốn chi phí nhân sự trực màn hình 24/7 để làm một việc mang tính lặp lại. |
| **5. Success Metric** | Tự động gắn tag (ví dụ: DIEN_NHE, NUOC_SINH_HOAT, AN_NINH) cho 100% ticket dưới 10 giây/ticket. |
| **6. Operational Boundary** | - CẤM: AI không được tự động hứa hẹn thời gian hoàn thành cụ thể hoặc hứa bồi thường tài chính.- CẤM: Với ticket có từ khóa an toàn tính mạng (cháy, nổ, cấp cứu), AI phải lập tức dán nhãn [KHAN_CAP] và kích hoạt còi báo động, không xử lý theo luồng bình thường. |

## 3.3. Future-State Flow & AI Fit (25 min)
* **Xác định mức AI Fit (AI-Fit Matrix):** Chọn LLM Feature (Không chọn Agent vì quy trình phân loại ticket là tác vụ đơn lẻ, có đầu vào/đầu ra cố định. Việc giao tiếp và hành động tiếp theo vẫn cần sự kiểm soát của con người để tránh sai sót điều phối nhầm thợ kỹ thuật).
* **Vẽ Future-State Flow:** 
```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Cư dân gửi   │     │ 🔵 AI đọc    │     │ 🟢 CSKH duyệt│     │ Kỹ thuật viên│
│ báo cáo sự   │ ──→ │ nội dung &   │ ──→ │ 1 chạm (HITL)│ ──→ │ nhận việc &  │
│ cố qua App   │     │ tự động gán  │     │ và điều phối │     │ đi xử lý     │
│ Vinhomes     │     │ tag phân loại│     │ tự động      │     │              │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                            │                    │
                            ▼                    ▼
                  🚨 LUỒNG KHẨN CẤP:        ↩️ Fallback:
                  Nếu AI gán tag            Nếu AI không tự tin
                  [KHẨN_CẤP], bỏ            phân loại, trả ticket
                  qua Bước 3, kích          về luồng đọc thủ công 
                  hoạt còi báo động.        như cũ.
```
---
# 🏁 Phase 5 — EVALUATE 

### AI Readiness Checklist:
[x] Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test? (Hệ thống App Vinhomes Resident và CRM nội bộ đã lưu trữ hàng ngàn ticket lịch sử bao gồm mô tả của cư dân và tag phân loại thủ công để làm tập đánh giá).

[x] Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)? (Có, thông qua cơ chế duyệt 1 chạm (HITL) của nhân viên CSKH và luồng Fallback trả về phân loại thủ công nếu AI không tự tin).

[x] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ? (Có, đội ngũ CSKH đang quá tải và liên tục phản ánh về áp lực trực màn hình phân loại ticket).

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[x] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp.

[ ] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline):** Trì hoãn để chuẩn bị thêm.

[ ] **NO-GO (Không khả thi / Rule-based tốt hơn):** Hủy bỏ dự án AI này.

**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):**
> 
Quyết định **GO** được đưa ra dựa trên các yếu tố sau:

- Bằng chứng kỹ thuật: Tác vụ phân loại văn bản/hình ảnh (Text/Image Classification) là thế mạnh cốt lõi của các mô hình LLM hiện nay như Gemini Flash. Mô hình không cần thiết kế Agent phức tạp, chỉ cần thiết lập kiến trúc LLM Feature gọn nhẹ gọi qua API. Ranh giới an toàn cho các trường hợp khẩn cấp (cháy, nổ) có thể được bảo vệ kép bằng System Prompt kết hợp với Rule-based từ khóa truyền thống để đảm bảo độ trễ bằng 0.

- Hiệu quả chi phí (ROI): Việc giảm thời gian xử lý từ 15-30 phút xuống dưới 1 phút giúp giải phóng hàng trăm giờ công mỗi tháng cho bộ phận CSKH Vinhomes, cho phép họ tập trung vào các nghiệp vụ chăm sóc khách hàng chuyên sâu hơn thay vì làm công việc điều phối (dispatcher) lặp đi lặp lại.

- Kiểm soát rủi ro: Quy trình mới vẫn giữ lại Human-in-the-loop (HITL) ở bước duyệt 1 chạm. AI đóng vai trò như một người "nháp" sẵn tag và đội ngũ, nhân viên chỉ cần lướt qua và bấm "Duyệt", loại bỏ hoàn toàn rủi ro AI tự động điều phối sai thợ kỹ thuật đến nhà cư dân gây phiền toái.

- Tác động kinh doanh: Cải thiện trực tiếp chỉ số hài lòng khách hàng (CSAT) do tốc độ tiếp nhận và xử lý sự cố tại các khu đô thị được đẩy nhanh đáng kể.

---