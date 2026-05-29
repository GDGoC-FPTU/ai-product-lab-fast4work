# BÁO CÁO PHÂN TÍCH SÂU DỰ ÁN AI (DEEP-DIVE REPORT)
---

## 1. QUYẾT ĐỊNH LỰA CHỌN
Sau quá trình quét (Scan) và đánh giá nhanh (Quick-Assess) các điểm nghẽn vận hành tại các công ty thành viên Vingroup, nhóm quyết định chọn bài toán mang lại ROI (Tỷ suất hoàn vốn) cao nhất về mặt thời gian và nguồn lực y tế:
* **Tên bài toán được chọn:** Hệ thống Tự động hóa Tóm tắt Bệnh án Xuất viện (Smart Discharge Summary - SDS).

---

## 2. PROBLEM STATEMENT (BẢN Tuyên Ngôn Vấn Đề 6-Field)

| STT | Trường thông tin (Field) | Mô tả chi tiết |
|:---:|:---|:---|
| **1** | **Actor / Operator**<br>*(Ai là người thực hiện?)* | Bác sĩ điều trị chính / Bác sĩ nội trú tại bệnh viện đa khoa Vinmec. |
| **2** | **Current Workflow**<br>*(Quy trình hiện tại)* | Tra cứu HIS (Hệ thống thông tin bệnh viện) lội ngược dòng lịch sử điều trị ➡️ Đối chiếu kết quả xét nghiệm qua các ngày ➡️ Gõ phím thủ công bản tóm tắt diễn biến bệnh & hướng điều trị ➡️ In ấn, ký duyệt và bàn giao cho điều dưỡng. |
| **3** | **Bottleneck**<br>*(Nút thắt cổ chai)* | Việc phải đọc hàng chục trang dữ liệu phi cấu trúc (ghi chú hằng ngày của nhiều ca trực) và thao tác gõ máy tính thủ công tốn trung bình **27 phút/hồ sơ**. |
| **4** | **Business Impact**<br>*(Hậu quả kinh doanh)* | 1. **Talent Misallocation:** Lãng phí thời gian quý giá của bác sĩ chuyên môn cao vào việc hành chính giấy tờ.<br>2. **Bed Blocking:** Bệnh nhân phải chờ đợi giấy tờ để xuất viện, làm chậm trễ việc giải phóng giường bệnh, trực tiếp làm giảm tỷ lệ quay vòng giường bệnh (Bed Turnover Rate) và doanh thu nội trú. |
| **5** | **Success Metric**<br>*(Chỉ số thành công)* | - **Thời gian:** Giảm thời gian hoàn thành Tóm tắt bệnh án từ 27 phút xuống **< 5 phút/ca** (bao gồm cả thời gian review của bác sĩ).<br>- **Chất lượng:** Tỷ lệ ảo giác (Hallucination) về chẩn đoán và đơn thuốc bằng **0%**. |
| **6** | **Operational Boundary**<br>*(Ranh giới vận hành AI)* | - AI **CHỈ** được đóng vai trò trích xuất thông tin có sẵn để tạo BẢN NHÁP (Draft).<br>- AI **CẤM TUYỆT ĐỐI** việc tự suy luận bệnh mới, tự kê đơn thuốc hoặc đưa ra chẩn đoán trái với hồ sơ gốc.<br>- Output bắt buộc phải ở định dạng JSON để đổ vào biểu mẫu HIS. |

---

## 3. FUTURE-STATE FLOW & AI FIT

### 3.1. Sơ đồ quy trình tương lai (Text-diagram)
Sự can thiệp của AI sẽ cắt bỏ hoàn toàn việc gõ máy thủ công, chuyển bác sĩ từ "Người soạn thảo" thành "Người phê duyệt".

```text
[LỆNH XUẤT VIỆN TỪ HỆ THỐNG]
       │
       ▼
[BƯỚC 1: AI AGENT QUÉT & TRÍCH XUẤT] 
       ├─ Input: Toàn bộ Text Log điều trị + Kết quả xét nghiệm.
       ├─ Xử lý: LLM tóm tắt, chuẩn hóa ngôn ngữ y khoa chuyên ngành.
       └─ Output: Điền tự động vào biểu mẫu JSON nháp (Draft).
       │
       ▼
[BƯỚC 2: HUMAN-IN-THE-LOOP (BÁC SĨ PHÊ DUYỆT)] 
       ├─ Bác sĩ đối chiếu bản nháp trên màn hình HIS.
       └─ Sửa chữa (nếu cần) và bấm "DUYỆT & KÝ SỐ".
       │
       ▼
[BƯỚC 3: XUẤT VIỆN] ➡️ Bàn giao hồ sơ pháp lý hoàn chỉnh cho bệnh nhân.