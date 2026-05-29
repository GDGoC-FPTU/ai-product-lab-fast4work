# Lab 02 — Phase 3 & 5: AI Product Deep-Dive & Evaluation Report

## 👥 Khai báo thành viên Nhóm:
* **Tên nhóm:** Vin Smart Future (GSM / Xanh SM Team)
* **Thành viên tham gia:**
  1. Nguyễn Đức Mạnh — MSSV: [Điền MSSV của bạn vào đây] (AI Product Engineer)

---

# 🗳️ Quyết định lựa chọn của nhóm:
Nhóm quyết định chọn bài toán **"Card #1 — Xanh SM Xử lý sự cố sạc pin thực địa"** để thực hiện Deep-Dive.

### Lý do lựa chọn và loại bỏ các thẻ khác:
* **Card #2 (Vinhomes CSKH):** Mặc dù tốn thời gian nhưng rủi ro sai sót thông tin liên quan đến phí quản lý, tranh chấp căn hộ có thể dẫn đến khiếu nại pháp lý nặng cho Vinhomes. Cần gom thêm dữ liệu và xử lý bằng Rule-based router trước.
* **Card #3 (Vinmec Tóm tắt xuất viện):** Đây là dữ liệu y tế nhạy cảm cao, rủi ro pháp lý lớn và yêu cầu bảo mật thông tin cực kỳ nghiêm ngặt. Việc áp dụng LLM trực tiếp cần có quy trình thẩm định lâm sàng phức tạp và sự phê duyệt của Hội đồng Y khoa, chưa thích hợp cho prototype nhanh.
* **Bài toán đã chọn (Card #1 - Xanh SM Sự cố sạc pin):** Đây là bài toán có luồng dữ liệu rõ ràng, tần suất xảy ra cao thực tế (~80 vụ/ngày tại Hà Nội), và có ranh giới an toàn rõ ràng (Human-in-the-loop duyệt tin trước khi gửi). Giải pháp giúp giải phóng sức lao động của điều phối viên ngay lập tức và giảm stress cho tài xế.

---

# 🏗️ Phase 3 — DEEP-DIVE (Nhóm)

## 3.1. Current-State Workflow Map
Quy trình xử lý sự cố hết pin thực địa hiện tại của điều phối viên Xanh SM:

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Nhận cuộc    │     │ Tra cứu định │     │ Tra cứu trạm │     │ Soạn văn bản │
│ gọi sự cố    │ ──→ │ vị GPS xe   │ ──→ │ sạc VinFast  │ ──→ │ hướng dẫn    │
│              │     │              │     │ còn trụ trống│     │ gửi tài xế   │
│ Ai: Dispatch │     │ Ai: Dispatch │     │ Ai: Dispatch │     │ Ai: Dispatch │
│ ⏱ 2 phút     │     │ ⏱ 2 phút     │     │ ⏱ 5 phút 🔴  │     │ ⏱ 5 phút 🔴  │
│ In: Điện thoại│     │ In: Biển số  │     │ In: Vị trí GPS│     │ In: Raw data │
│ Out: Log sự cố│     │ Out: Toạ độ  │     │ Out: Địa chỉ  │     │ Out: SMS draft│
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                       │
                                                                       ▼
                                                                ┌──────────────┐
                                                                │ Bước 5       │
                                                                │ Gọi xe cứu   │
                                                                │ hộ (nếu cần) │
                                                                │ Ai: Dispatch │
                                                                │ ⏱ 1 phút     │
                                                                └──────────────┘
🔴 = Bottlenecks (Bước 3 & 4 tốn thời gian nhất do tra cứu thủ công và viết tin nhắn thủ công).
⏱ Tổng thời gian xử lý thủ công trung bình: 15 phút/lượt.
🔄 Handoff points: Giữa Tài xế và Tổng đài (B1), Giữa Tổng đài và Dashboard nội bộ (B2, B3), Giữa Tổng đài và App tài xế (B4).
```

---

## 3.2. Problem Statement (6-field) — Vin Smart Future Standard

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Điều phối viên (Dispatcher) thuộc Trung tâm Điều vận Xanh SM. |
| **2. Current Workflow** | Khi tài xế báo hết pin, điều phối viên nhận cuộc gọi, tra cứu định vị GPS xe trên bản đồ nội bộ, mở Dashboard trạm sạc VinFast để tìm trụ sạc trống gần nhất, viết tin nhắn chỉ dẫn/định vị gửi qua App tài xế, và gọi cứu hộ nếu pin dưới 5%. Quy trình gồm 5 bước, hoàn toàn thủ công, mất 15 phút/lượt. |
| **3. Bottleneck** | Bước 3 & 4 (mất tổng cộng 10 phút): Tra cứu thủ công trụ sạc trống phù hợp với dòng xe (VF5/VFe34/VF8) và soạn thảo tin nhắn hướng dẫn đường đi chi tiết bằng Tiếng Việt thân thiện. |
| **4. Business Impact** | Gây lãng phí ~20 giờ làm việc/ngày của nhóm điều phối viên. Tăng thời gian chết của tài xế và xe điện, làm giảm doanh thu ước tính ~15% và làm giảm mức độ hài lòng của khách hàng do cuốc xe bị trì hoãn hoặc hủy. |
| **5. Success Metric** | 1. Giảm tổng thời gian xử lý sự cố từ 15 phút xuống dưới 3 phút (Tăng năng suất 80%).<br>2. Tỉ lệ hướng dẫn đúng địa điểm và đúng loại trụ sạc phù hợp đạt 98% (Chất lượng). |
| **6. Operational Boundary** | **ĐƯỢC PHÉP:** Tự động hóa lấy vị trí xe, truy vấn API trạm sạc, tự động soạn nháp tin nhắn hướng dẫn dạng [DRAFT_ONLY].<br>**CẤM:** Không được tự động gửi tin nhắn cho tài xế mà chưa qua điều phối viên duyệt (HITL). Không được chỉ đường đến trạm sạc > 5km khi pin xe < 5% (phải chuyển sang điều xe cứu hộ pin di động). |

---

## 3.3. Future-State Flow & AI Fit

* **AI Fit Matrix:** Chọn **LLM Feature** (không chọn Agent tự trị hoàn toàn vì rủi ro an toàn giao thông và chi phí vận hành sai lệch là rất cao nếu xe hết pin giữa đường cao tốc).
* **Quy trình tương lai (Future-State):**

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Nhận cuộc    │     │ 🔵 Auto-pull │     │ 🔵 AI draft  │     │ 🟢 Dispatch  │
│ gọi sự cố    │ ──→ │ vị trí &     │ ──→ │ SMS chỉ dẫn  │ ──→ │ click duyệt  │
│              │     │ trạm sạc trống│    │ & chỉ đường  │     │ & gửi tài xế │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                      │
                                                                      ▼
                                                               ↩️ Fallback:
                                                               Nếu AI draft lỗi hoặc không
                                                               tự tin, Dispatcher tự viết
                                                               tay lại như quy trình cũ.
```

- 🔵 **AI Step (Bước 2 & 3):** Hệ thống tự động kéo vị trí GPS của xe qua API, truy vấn trạm sạc trống phù hợp gần nhất. LLM tiếp nhận dữ liệu này, kiểm tra ranh giới an toàn (mức pin) để tự động soạn tin nhắn hướng dẫn hoặc kích hoạt cứu hộ di động nếu pin < 5%.
- 🟢 **Human Step (HITL - Bước 4):** Điều phối viên bắt buộc phải đọc và nhấn nút "Phê duyệt & Gửi" trên dashboard điều hành. Phản hồi của LLM bắt buộc phải có tiền tố `[DRAFT_ONLY]` để hệ thống nhận diện và không gửi thẳng cho tài xế.
- ↩️ **Fallback:** Nếu mô hình phản hồi lỗi hoặc không có tag `[DRAFT_ONLY]`, hệ thống sẽ chặn và điều phối viên sẽ chuyển sang nhập liệu và soạn tin thủ công như trước.

---

# 🏁 Phase 5 — EVALUATE: Phân tích độ khả thi và Chi phí (Nhóm)

### AI Readiness Checklist:
1. [x] **Dữ liệu mẫu/logs sạch:** Chúng tôi có sẵn dữ liệu định vị xe và trạng thái trạm sạc qua API nội bộ của VinFast và GSM.
2. [x] **Kiểm soát rủi ro:** Đã thiết lập cơ chế Human-in-the-loop (HITL) phê duyệt tin nhắn và Fallback thủ công.
3. [x] **Sự sẵn sàng của Stakeholders:** Nhóm điều phối viên và tài xế rất mong muốn giải pháp này để giảm áp lực công việc.

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[x] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp (thử nghiệm tại Hà Nội).

### Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):
1. **Khả thi kỹ thuật:** Việc tích hợp LLM Feature để soạn thảo tin nhắn dựa trên dữ liệu cấu trúc (vị trí, danh sách trạm sạc) là rất khả thi và có độ chính xác cao. Ranh giới an toàn (pin < 5%) dễ dàng được kiểm soát thông qua System Prompt và logic kiểm tra bổ sung.
2. **Hiệu quả kinh tế & Chi phí:** 
   - *Chi phí vận hành AI:* Ước tính dùng Gemini 2.5 Flash là $0.075 / 1 triệu tokens đầu vào và $0.30 / 1 triệu tokens đầu ra. Với 80 sự cố/ngày, mỗi lượt sử dụng ~2,000 tokens, chi phí API chỉ khoảng **$0.05 / ngày** (cực kỳ rẻ).
   - *Lợi ích:* Tiết kiệm 20 giờ làm việc/ngày của nhân viên điều hành, tương đương tiết kiệm khoảng $150/tháng chi phí nhân sự trực tiếp, giảm tỉ lệ hủy chuyến giúp giữ lại doanh thu ước tính $3,000/tháng cho GSM. Tỷ lệ ROI dự kiến cực kỳ cao.
