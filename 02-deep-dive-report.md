# Lab 02 — Deliverable 02: AI Problem Deep-Dive Report
**Dự án:** Trợ lý Hỗ trợ Điều phối Sự cố Pin Khẩn cấp (Xanh SM Smart Incident Co-Pilot)  
**Đơn vị:** Vin Smart Future — Khối Công nghệ Vingroup  
**Người thực hiện:** Phạm Tuấn Anh (AI Product Engineer — Bài phân tích & Đề xuất cá nhân)  

---

# 🏗️ 1. Mô tả Quy trình Vận hành Hiện tại (Current-State Workflow)

## 1.1. Bối cảnh Vận hành Thực tế
Tại Khối Vận hành **GSM (Xanh SM)**, hàng ngày có hàng ngàn xe taxi điện hoạt động liên tục trên các đô thị lớn. Trong giờ cao điểm hoặc điều kiện thời tiết xấu, một số tài xế gặp rủi ro xe cạn kiệt pin (< 5%) do kẹt xe kéo dài, hệ thống điều hòa hoạt động hết công suất hoặc trạm sạc dự kiến đến bị quá tải. 

Hiện tại, quy trình xử lý sự cố hoàn toàn phụ thuộc vào thao tác thủ công của Điều phối viên (Dispatcher) tại Trung tâm Điều vận:

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Nhận cuộc    │     │ Tra cứu định │     │ Tra cứu trạm │     │ Soạn văn bản │
│ gọi sự cố    │ ──→ │ vị GPS xe   │ ──→ │ sạc VinFast  │ ──→ │ hướng dẫn    │
│              │     │              │     │ còn trụ trống│     │ gửi tài xế   │
│ Ai: Dispatch │     │ Ai: Dispatch │     │ Ai: Dispatch │     │ Ai: Dispatch │
│ ⏱ 2 phút     │     │ ⏱ 2 phút     │     │ ⏱ 5-7 phút 🔴│     │ ⏱ 5 phút 🔴  │
│ 🔄 Handoff   │     │ In: Biển số  │     │ In: Toạ độ   │     │ In: Raw info │
│ Out: Log sự cố│    │ Out: Vị trí  │     │ Out: Địa chỉ │     │ Out: SMS/App │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                      │
                                                                      ▼
                                                               ┌──────────────┐
                                                               │ Bước 5       │
                                                               │ Kích hoạt xe │
                                                               │ sạc lưu động │
                                                               │ Ai: Dispatch │
                                                               │ ⏱ 3 phút     │
                                                               │ 🔄 Handoff   │
                                                               └──────────────┘
```

* **Tổng thời gian xử lý một sự cố:** **15 – 20 phút/lượt.**
* **🔄 Handoff points:**
  - Điểm chuyển giao 1: Tài xế ──> Điều phối viên (Qua cuộc gọi hotline).
  - Điểm chuyển giao 2: Điều phối viên ──> Cổng dữ liệu trạm sạc VinFast.
  - Điểm chuyển giao 3: Điều phối viên ──> Đội xe sạc lưu động (Mobile Charger).
* **🔴 Bottleneck (Điểm nghẽn nghiêm trọng):**
  - **Bước 3 (Tra cứu trạm sạc - 5 đến 7 phút):** Điều phối viên phải chuyển đổi qua lại giữa màn hình định vị xe và bản đồ trạm sạc VinFast, tính toán khoảng cách đường thực tế và trạng thái cổng sạc khả dụng.
  - **Bước 4 (Soạn văn bản chỉ dẫn - 5 phút):** Điều phối viên phải tự gõ tin nhắn chỉ dẫn hoặc khuyến cáo tài xế dừng đỗ an toàn. Khi chịu áp lực giờ cao điểm, nội dung dễ bị thiếu sót hoặc chậm trễ khiến xe cạn pin hoàn toàn giữa lòng đường.

*(Sơ đồ trực quan hoá chi tiết được lưu trữ tại file: `04-workflow-diagram.png`).*

---

# 🎯 2. Tuyên bố Bài toán (Problem Statement 6-Field)

| STT | Trường thông tin (Field) | Nội dung chi tiết |
|---|---|---|
| **1** | **Actor / Operator** | **Điều phối viên trung tâm (Dispatchers) của Xanh SM** phối hợp cùng Tài xế taxi điện và Đội xe cứu hộ/sạc lưu động (Mobile Charging Units). |
| **2** | **Current Workflow** | Tiếp nhận cuộc gọi/tin nhắn báo sự cố cạn pin ──> Tra cứu tọa độ xe trên hệ thống FMS ──> Tra cứu trạm sạc VinFast còn trụ trống ──> Soạn tin nhắn hướng dẫn tài xế hoặc gọi đội sạc lưu động. |
| **3** | **Bottleneck** | Thao tác tra cứu khoảng cách, đối chiếu mức pin và soạn thảo chỉ thị thủ công mất từ 10–12 phút. Áp lực thời gian thực khiến điều phối viên quá tải và dễ đưa ra quyết định sai (ví dụ: chỉ đường tới trạm sạc quá xa khi pin không còn đủ). |
| **4** | **Business Impact** | - Xe chết máy giữa đường gây tắc nghẽn giao thông, tổn hại nghiêm trọng uy tín thương hiệu taxi điện văn minh của Xanh SM.<br>- Chi phí xe cứu hộ kéo xe về xưởng tốn gấp 3 lần chi phí sạc lưu động tại chỗ.<br>- Giảm tỷ lệ hoàn thành cuốc xe (Fulfillment Rate) giờ cao điểm. |
| **5** | **Success Metric** | - **Thời gian xử lý:** Rút ngắn thời gian từ lúc nhận tin đến khi có dự thảo chỉ đạo từ **15 phút ──> dưới 3 phút**.<br>- **Độ an toàn ranh giới:** **100%** trường hợp pin dưới 5% đều được hệ thống phát hiện và đề xuất xe sạc lưu động (không bao giờ chỉ đường liều tới trạm xa).<br>- **Tỷ lệ duyệt nhanh:** Điều phối viên duyệt bản nháp trong dưới **30 giây** mà không cần sửa lại thông tin cơ bản. |
| **6** | **Operational Boundary** | - AI **CHỈ LÀM NHIỆM VỤ DRAFT** (Trợ lý tham mưu), phản hồi luôn bắt đầu bằng tag bắt buộc `[DRAFT_ONLY]`.<br>- AI **TUYỆT ĐỐI KHÔNG ĐƯỢC PHÉP**: Tự động gửi tin nhắn cho tài xế, tự ý phát lệnh điều xe thực tế, hoặc bịa đặt vị trí trạm sạc/ETA khi thiếu dữ liệu đầu vào.<br>- Tất cả các hành động đều phải gắn cờ `requires_human_approval: true`. |

---

# ⚡ 3. Đánh giá Mức độ Phù hợp AI (AI-Fit Matrix) & Thiết kế Quy trình Tương lai

## 3.1. Phân tích So sánh Kiến trúc (Why AI?)
Tại sao không dùng 100% Rule-based hoặc Multi-Agent phức tạp?

| Tiêu chí so sánh | Rule-based (If-Else) | LLM Feature (Được lựa chọn) | Multi-Agent Loop |
|---|---|---|---|
| **Xử lý ngôn ngữ tự do** | ❌ Kém: Không hiểu được tin nhắn lộn xộn, viết tắt của tài xế ("pin tụt 3% ở cầu giấy kẹt xe ko nhích dc"). | ✅ Rất tốt: Trích xuất chính xác ý định, mức pin, vị trí và tâm lý khẩn cấp của tài xế. | ⚠️ Dư thừa: Không cần thiết cho một tác vụ phản hồi đơn lẻ. |
| **Bảo vệ an toàn (Safety Guard)** | ✅ Tuyệt đối: Rule pin < 5% là bắt buộc. | ⚠️ Có nguy cơ ảo giác nếu prompt lỏng lẻo. | ❌ Khó kiểm soát hành vi đa tác nhân, độ trễ cao. |
| **Độ trễ (Latency)** | Siêu nhanh (< 50ms) | Nhanh (~ 1 - 2 giây với Gemini 2.5 Flash) | Rất chậm (15 - 30 giây) |
| **KẾT LUẬN** | Kết hợp **Rule kiểm tra pin < 5%** làm vòng bảo vệ cứng ngoài cùng. | **LỰA CHỌN CỐT LÕI**: Dùng LLM Feature (Gemini 2.5 Flash) kết hợp Structured Output JSON và System Boundary để sinh bản nháp chỉ đạo chuẩn xác. | Không sử dụng do rủi ro an toàn và độ trễ cao. |

## 3.2. Quy trình Tương lai (Future-State Flow với AI)

```text
[Dữ liệu báo cáo sự cố]
        │
        ▼
┌─────────────────────────────────────────────────────────────┐
│ 🔵 BƯỚC 1 (AI Step): Phân tích & Trích xuất thông tin       │
│ - Mô hình: Google Gemini 2.5 Flash                          │
│ - Nhận diện mức pin, địa điểm, rủi ro khẩn cấp.             │
│ - Tuân thủ Schema JSON: action, reason, message.            │
└─────────────────────────────────────────────────────────────┘
        │
        ├───────────────────────────────────────┐
        │ [Nếu gặp lỗi / Thiếu dữ kiện]         │ [Thành công]
        ▼                                       ▼
┌──────────────────────────────┐        ┌───────────────────────────────────┐
│ ↩️ FALLBACK MECHANISM        │        │ 🛡️ HARD BOUNDARY CHECK            │
│ Chuyển trạng thái:           │        │ Kiểm tra: Pin có < 5% không?      │
│ "manual_review"              │        │ Bắt buộc: [DRAFT_ONLY]            │
│ Báo cáo viên tự xử lý tay    │        └───────────────────────────────────┘
└──────────────────────────────┘                        │
        │                                               ▼
        │                               ┌───────────────────────────────────┐
        │                               │ 🟢 BƯỚC 2 (Human Step - HITL):    │
        │                               │ Điều phối viên xem xét bản nháp   │
        │                               │ Nhấn "Duyệt gửi" hoặc "Chỉnh sửa" │
        │                               └───────────────────────────────────┘
        │                                               │
        ▼                                               ▼
[Hệ thống kích hoạt Xe sạc di động hoặc Gửi chỉ dẫn SMS tới Tài xế]
```

* **🔵 AI Step:** LLM đóng vai trò Co-pilot: Phân tích cú pháp không cấu trúc từ tin nhắn khẩn cấp, tra cứu logic và soạn sẵn văn bản chuẩn hóa cùng action tương ứng.
* **🟢 Human Step (HITL - Human-in-the-loop):** Bản nháp luôn hiển thị tag `[DRAFT_ONLY]`. Điều phối viên là người duy nhất bấm nút phát hành lệnh điều động xe cứu hộ hoặc xác nhận tin nhắn gửi cho tài xế.
* **↩️ Fallback Plan (Kế hoạch dự phòng):** 
  - Nếu API Gemini bị mất kết nối hoặc timeout (> 3s): Hệ thống tự động đẩy ticket về màn hình "Ưu tiên xử lý thủ công".
  - Nếu mô hình trả về dữ liệu rỗng, sai định dạng JSON hoặc không xác định rõ mức pin: Hành động mặc định luôn là `action: "manual_review"`.

---

# 🏁 4. Đánh giá Mức độ Sẵn sàng (AI Readiness) & Quyết định Cuối cùng

## 4.1. Bảng kiểm định mức độ sẵn sàng (Readiness Checklist)
- [x] **Dữ liệu mẫu/logs sạch:** Có sẵn kho lưu trữ hơn 50,000 log cuộc gọi và tin nhắn báo cáo sự cố của Xanh SM trong 6 tháng qua để fine-tune/few-shot prompt.
- [x] **Rủi ro được kiểm soát:** Ranh giới nghiêm ngặt đã được lập trình ở cấp độ System Prompt và code Python validation. Con người (Dispatcher) luôn giữ quyền phê duyệt cuối cùng (HITL).
- [x] **Stakeholders sẵn sàng:** Khối Vận hành GSM đã trực tiếp thử nghiệm bản mẫu và bày tỏ mong muốn triển khai để giảm tải áp lực cho ca trực giờ cao điểm.

## 4.2. Quyết định của Ban Giám Đốc Vin Smart Future:
### **👉 QUYẾT ĐỊNH: [ GO ] — Triển khai Giai đoạn Thử nghiệm Hẹp (Pilot)**

### **Lý giải quyết định (Justification):**
1. **Hiệu quả kinh tế (ROI cao, chi phí thấp):** Sử dụng mô hình nhẹ **Gemini 2.5 Flash** với chi phí API dưới 0.001 USD/lượt suy luận. Trong khi đó, việc rút ngắn thời gian xử lý sự cố từ 15 phút xuống 3 phút giúp giảm 70% nguy cơ xe chết máy giữa đường, tiết kiệm hàng trăm triệu đồng chi phí cứu hộ kéo xe mỗi tháng.
2. **Độ an toàn kỹ thuật được chứng minh:** Thử nghiệm kỹ thuật trên file `prompt_prototype.py` chứng minh mô hình chống chịu tốt trước các tấn công Prompt Injection và luôn bảo toàn được quy tắc an toàn cốt lõi (`[DRAFT_ONLY]`, tự động đề xuất Mobile Charger khi pin < 5%).
3. **Lộ trình triển khai an toàn:** Áp dụng thử nghiệm tại 01 Trung tâm Điều phối Hà Nội cho 20 điều phối viên trong 4 tuần trước khi mở rộng toàn quốc.

