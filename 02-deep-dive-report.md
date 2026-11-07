# Lab 02 — Báo cáo phân tích sâu
## Phase 3 — DEEP-DIVE
### 3.1 Current-State Workflow Mapping
![Quy trình hiện tại](04-workflow-diagram.jpg)
### 3.2. Problem Statement (6-field) & Metrics
| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Nhân viên, Kỹ thuật viên và Tài xế |
| **2. Current Workflow** |Tài xế đặt lịch qua hệ thống,nhân viên xác nhận. Tài xế đến xưởng, làm thủ tục và chờ lượt. Kỹ thuật viên kiểm tra xe yêu cầu phụ tùng nếu cần.Kỹ thuật viên bảo dưỡng, kiểm tra và nhân viên bàn giao bàn giao.|
| **3. Bottleneck** | Chờ tiếp nhận: Lịch hẹn chưa khớp với thời lượng công việc và năng lực thực tế của xưởng, Chờ phụ tùng: Phụ tùng cần dùng chưa được kiểm tra hoặc chuẩn bị trước, một số nhu cầu chỉ phát hiện sau khi kiểm tra xe. |
| **4. Business Impact** | Thời gian xe nằm xưởng kéo dài, tài xế không thể chạy xe, nhân viên phải giải thích, đổi lịch và xử lý tồn đọng.|
| **5. Success Metric** |giảm thời gian chờ tiếp nhận trung bình của xe đến đúng hẹn từ 60 xuống 30 phút, tính từ lúc check-in đến lúc kỹ thuật viên bắt đầu kiểm tra. |
| **6. Operational Boundary** | **Được phép**: Dự báo thời lượng, gợi ý phụ tùng có khả năng cần dùng, kiểm tra dữ liệu tồn kho và đề xuất lịch hoặc xưởng phù hợp. **Không được phép**: Tự kết luận hư hỏng, quyết định thay phụ tùng, đặt mua phụ tùng hoặc tự hủy/đổi lịch đã xác nhận. **Cần duyệt**: Nhân viên duyệt phương án lịch; tài xế đồng ý khi đổi lịch/xưởng, kỹ thuật viên xác nhận hạng mục và phụ tùng sau kiểm tra. |

### 3.3. Future-State Flow & AI Fit 
![Quy trình tương lai](04-workflow-diagram_3.3.jpg)

##  Phase 5 — EVALUATE 

### AI Readiness Checklist:
1. [ ] Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test?
2. [x] Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)?
3. [ ] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ?

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[ ] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp.
[x] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline):** Trì hoãn để chuẩn bị thêm.
[ ] **NO-GO (Không khả thi / Rule-based tốt hơn):** Hủy bỏ dự án AI này.

**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):**
> *Viết lý giải chi tiết tại đây*
- Kết quả 3 bài kiểm thử cho thấy mô hình bước đầu giữ đúng định dạng và ranh giới: không tự xác nhận lịch, chẩn đoán hoặc bịa dữ liệu nguồn lực. Tuy nhiên, đây mới là kiểm thử prompt, chưa chứng minh giải pháp giảm thời gian chờ từ 60 xuống 30 phút.

- Nhóm chưa có dữ liệu thực tế để xác lập thời gian chờ, nguyên nhân chậm và khả năng đáp ứng của xưởng. Chi phí API, thời gian nhân viên duyệt và chi phí tích hợp cũng chưa được đo, nên chưa đủ cơ sở đánh giá hiệu quả đầu tư.

- Bước tiếp theo là thu thập dữ liệu tại một xưởng, xây dựng baseline kiểm tra lịch và phụ tùng bằng rule-based, rồi so sánh với phương án có LLM. Chỉ chuyển sang GO khi chứng minh phần AI giảm công sức tiếp nhận mà vẫn bảo đảm độ chính xác và chi phí phù hợp.
---
