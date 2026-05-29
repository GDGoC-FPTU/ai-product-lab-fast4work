# 📖 Báo Cáo Phân Tích Sâu (Deep-Dive Report) — Nhóm Vin Smart Future

> **⚠️ LƯU Ý QUAN TRỌNG:** 
> Nhóm của bạn bắt buộc phải điền đầy đủ thông tin thành viên dưới đây trước khi nộp bài:
> *   **Tên Nhóm:** [Điền Tên Nhóm của bạn tại đây]
> *   **Thành viên 1:** [Họ và tên] - [MSSV]
> *   **Thành viên 2:** [Họ và tên] - [MSSV]
> *   **Thành viên 3:** [Họ và tên] - [MSSV]

---

## 🗳️ 1. Quyết định lựa chọn của nhóm

Nhóm đã thảo luận và thống nhất lựa chọn bài toán: 
**"Xử lý sự cố sạc pin / hết pin thực địa của tài xế Xanh SM (GSM)"** để thực hiện phân tích sâu (Deep-Dive) và xây dựng prompt prototype.

### Lý do lựa chọn bài toán này:
1.  **Tính khẩn cấp cao:** Đây là sự cố thời gian thực (real-time). Khi tài xế hết pin giữa đường, nó trực tiếp làm gián đoạn hành trình của khách hàng và gây rò rỉ doanh thu ngay lập tức.
2.  **Ranh giới an toàn rõ ràng:** Có các quy tắc cứng về kỹ thuật (mức pin < 5% không được đi xa > 5km) rất phù hợp để thiết lập và thử nghiệm ranh giới an toàn (Operational Boundary) cho LLM.
3.  **Tính khả thi kỹ thuật:** Phù hợp để ứng dụng **LLM Feature** hỗ trợ soạn thảo nháp tin nhắn chỉ dẫn, giúp giảm tải công việc lặp đi lặp lại của điều phối viên mà vẫn giữ được sự kiểm soát của con người (Human-in-the-loop).

---

## 🏗️ 2. Phase 3 — DEEP-DIVE

### 3.1. Current-State Workflow (Quy trình hiện tại)
*(Sơ đồ quy trình hiện tại chi tiết được vẽ tay và nộp trong file `04-workflow-diagram`)*

**Mô tả quy trình thủ công:**
1.  **Bước 1:** Tài xế gọi điện/gửi yêu cầu khẩn cấp lên tổng đài báo xe sắp hết pin hoặc đã chết máy giữa đường. *(⏱ 2 phút)*
2.  **Bước 2:** Điều phối viên (Dispatcher) truy cập hệ thống định vị GPS để xác định tọa độ hiện tại của xe. *(⏱ 2 phút)*
3.  **Bước 3 (Bottleneck 🔴):** Điều phối viên mở bản đồ trạm sạc VinFast, tìm kiếm trạm sạc gần vị trí xe nhất và kiểm tra xem còn cổng sạc trống phù hợp với dòng xe (VF5/VF8/VF9) đó hay không. *(⏱ 5 phút)*
4.  **Bước 4 (Bottleneck 🔴):** Điều phối viên soạn thảo tin nhắn hướng dẫn đường đi chi tiết bằng tiếng Việt, bao gồm địa chỉ trạm sạc, khoảng cách, loại cổng sạc và gửi cho tài xế qua ứng dụng hoặc SMS. *(⏱ 5 phút)*
5.  **Bước 5:** Trong trường hợp xe dưới 5% pin và không có trạm sạc gần, điều phối viên phải liên hệ trực tiếp với đội xe cứu hộ pin di động để điều phối xe đến sạc trực tiếp. *(⏱ 1 phút)*

*   **Tổng thời gian xử lý trung bình hiện tại:** **15 phút/lượt**.

---

### 3.2. Problem Statement (6-field) — Vin Smart Future Standard

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Điều phối viên (Dispatcher) thuộc Trung tâm Điều vận Xanh SM. |
| **2. Current Workflow** | Khi tài xế báo hết pin, điều phối viên phải tra cứu định vị xe, mở Dashboard trạm sạc VinFast để lọc thủ công trạm sạc trống phù hợp, soạn thảo tin nhắn chỉ dẫn gửi tài xế và gọi cứu hộ nếu pin quá thấp. Quy trình 5 bước thủ công hoàn toàn. |
| **3. Bottleneck** | Bước 3 & 4 (mất 10 phút): Tra cứu thủ công các trụ sạc trống tương thích dòng xe và soạn thảo tin nhắn hướng dẫn đường đi chi tiết bằng Tiếng Việt. |
| **4. Business Impact** | Mỗi ngày có trung bình ~80 sự cố pin thực địa tại Hà Nội, gây lãng phí 20 giờ làm việc/ngày của team điều vận. Thời gian chờ đợi lâu làm tăng tỷ lệ hủy chuyến của khách hàng, giảm tuổi thọ pin xe và gây ức chế cho tài xế (rò rỉ doanh thu ước tính ~15%). |
| **5. Success Metric** | 1. Giảm tổng thời gian điều phối xử lý sự cố từ **15 phút xuống dưới 3 phút**.<br>2. Tỷ lệ đề xuất đúng địa điểm trạm sạc tương thích đạt **> 98%**.<br>3. 100% tin nhắn hướng dẫn phải được định dạng chuẩn xác trước khi gửi đi. |
| **6. Operational Boundary** | **AI ĐƯỢC PHÉP:** Truy xuất dữ liệu vị trí xe, danh sách trạm sạc còn trống từ API và tự động soạn thảo tin nhắn hướng dẫn dạng nháp (Draft).<br>**AI TUYỆT ĐỐI CẤM:** Không được tự ý gửi tin nhắn trực tiếp cho tài xế mà không có sự kiểm duyệt của Dispatcher (Bắt buộc Human-in-the-loop); không được đề xuất trạm sạc xa hơn 5km nếu pin xe dưới 5% (phải chuyển sang điều phối xe cứu hộ pin). |

---

### 3.3. Future-State Flow & AI Fit

*   **Mức độ AI Fit:** **LLM Feature** kết hợp **Human-in-the-loop (HITL)**. 
    *(Không dùng Agent tự trị hoàn toàn để tránh rủi ro AI chỉ đường sai trạm khiến xe chết máy giữa đường gây cản trở giao thông).*

#### Sơ đồ Future-State Flow:
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
                                                                Nếu AI draft lỗi,
                                                                Dispatcher tự viết
                                                                tay lại như cũ.
```

**Mô tả các bước có sự tham gia của AI và con người:**
*   🔵 **Bước 2 (AI/Rule Step):** Hệ thống tự động lấy tọa độ xe từ thiết bị giám sát hành trình và truy vấn danh sách trạm sạc VinFast trống gần nhất qua API.
*   🔵 **Bước 3 (AI Step):** LLM phân tích dung lượng pin và khoảng cách để tự động tạo bản nháp tin nhắn chỉ đường kèm tag kiểm duyệt `[DRAFT_ONLY]` hoặc đề xuất điều xe cứu hộ pin.
*   🟢 **Bước 4 (Human-in-the-loop):** Điều phối viên kiểm tra lại nội dung bản nháp của AI trên màn hình điều vận, bấm nút "Phê duyệt & Gửi" để chuyển tin nhắn đến tài xế.
*   ↩️ **Fallback (Dự phòng):** Nếu LLM không thể tạo bản nháp hoặc gặp lỗi kết nối API, hệ thống sẽ hiển thị giao diện nhập tay truyền thống để điều phối viên tự xử lý như cũ.

---

## 🏁 3. Phase 5 — EVALUATE

### AI Readiness Checklist:
*   [x] **Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test?** *(Có dữ liệu tọa độ xe, danh sách trạm sạc VinFast và lịch sử chat của điều phối viên).*
*   [x] **Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)?** *(Có, điều phối viên luôn duyệt lại tin nhắn trước khi gửi, nếu AI sai thì sửa tay hoặc dùng fallback).*
*   [x] **Stakeholders sẵn sàng thay đổi quy trình làm việc cũ?** *(Tài xế và đội điều phối cực kỳ mong muốn giảm thời gian chờ đợi này).*

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
*   [x] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển bản mẫu prompt prototype để chạy thử nghiệm thực tế.

### Justification (Lý giải quyết định & Chi phí):
1.  **Về mặt kỹ thuật:** Bài toán có cấu trúc rõ ràng, dữ liệu đầu vào ổn định từ hệ thống GPS và API trạm sạc. Việc triển khai dưới dạng **LLM Feature** có HITL kiểm duyệt giúp giảm thiểu 100% rủi ro an toàn vận hành.
2.  **Ước lượng chi phí:**
    *   *Chi phí token LLM:* Khoảng 0.005 USD/lần điều phối. Với 80 sự cố/ngày, chi phí API chỉ khoảng 0.4 USD/ngày (~12 USD/tháng) — cực kỳ rẻ so với lợi ích mang lại.
    *   *Chi phí phát triển:* Khoảng 2 tuần phát triển và tích hợp giao diện điều vận cho 1 AI Engineer và 1 Frontend Engineer.
    *   *Lợi ích kinh tế:* Tiết kiệm 20 giờ làm việc/ngày của ban điều vận, tăng tỷ lệ hoàn thành chuyến của Xanh SM, giảm rò rỉ doanh thu từ xe nằm chờ sạc. Quyết định đầu tư là **hoàn toàn khả thi (GO)**.
