# Lab 02 — Phase 1 & 2: AI Problem Scan & Quick Assessment

Họ và tên: Nguyễn Đức Mạnh  
MSSV: [Điền MSSV của bạn vào đây]  
Lớp: AI Product Lab  
Nhóm: Vin Smart Future (GSM / Xanh SM)

---

# 🔍 Phase 1 — SCAN: Tìm kiếm cơ hội (Cá nhân)

Dưới đây là bảng quét cơ hội sử dụng **4 Lenses** tìm kiếm các bài toán/bottleneck thực tế thuộc hoạt động vận hành của các công ty thành viên Vingroup.

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **Xanh SM** | Lặp lại | So khớp và phân bổ lại cuốc xe khi khách hàng yêu cầu thay đổi điểm đến đột ngột giữa chừng. |
| 2 | **Xanh SM** | Tốn thời gian | Điều phối viên (Dispatcher) xử lý thủ công các phản hồi khẩn cấp từ tài xế về sự cố sạc pin hoặc va chạm thực địa (mất 15-20 phút/lượt). |
| 3 | **VinFast** | Lặp lại | So khớp hóa đơn điện tiêu thụ và đối chiếu số liệu trạm sạc đối tác hằng tuần để thanh toán chi phí. |
| 4 | **Vinhomes** | AI-upgrade | Phân loại và route tự động phản hồi/khiếu nại của cư dân trên ứng dụng Vinhomes Resident đến đúng ban quản lý từng tòa nhà. |
| 5 | **Vinmec** | Pain từ người khác | Bác sĩ mất quá nhiều thời gian viết tóm tắt hồ sơ bệnh án và thủ tục xuất viện (mất 20-30 phút/bệnh nhân), gây quá tải vào khung giờ cao điểm. |
| 6 | **VinFast** | Stakeholder Pain | Khách hàng mua xe VF8 phàn nàn về việc trợ lý ảo ViVi không phản hồi chính xác khi có tiếng ồn lớn hoặc nhạc nền bật to trong khoang xe. |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards (Cá nhân)

Dưới đây là 3 thẻ đánh giá nhanh bài toán tiềm năng nhất từ danh sách SCAN ở trên.

### QUICK PROBLEM CARD #1: Xanh SM Xử lý sự cố sạc pin thực địa (Đã chọn)
```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu): Tài xế Xanh SM báo cáo sự cố sạc pin hoặc │
│ hết pin giữa đường cần điều phối cứu hộ hoặc trạm sạc gần.   │
│ Công ty thành viên: [x] Xanh SM  [ ] VinFast  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác ________________  │
│                                                             │
│ Ai đang đau (Actor)? Tài xế (chờ đợi), Điều phối viên (quá tải)     │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Tài xế gọi tổng đài điều vận báo hết pin               │
│   ──> 2. Điều phối viên tra cứu thủ công vị trí xe trên bản đồ│
│   ──> 3. Tra cứu thủ công các trạm sạc VinFast còn trụ trống │
│   ──> 4. Soạn tin nhắn chỉ dẫn/đường đi gửi qua App tài xế  │
│   ──> 5. Liên hệ đội xe cứu hộ nếu xe đã cạn kiệt pin (< 5%)│
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3-4 (⏱ 12 phút/lượt)  │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3-4              │
│ (Tự động hóa lấy vị trí -> Tra cứu trạm trống -> Draft tin) │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ Giảm thời gian xử lý sự cố từ 15 phút ──> dưới 3 phút.      │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

### QUICK PROBLEM CARD #2: Vinhomes Phân loại và Điều hướng Khiếu nại Cư dân
```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu): Cư dân gửi khiếu nại bằng tiếng Việt trên│
│ App Vinhomes Resident, cần phân loại và gửi đúng bộ phận.  │
│ Công ty thành viên: [ ] Xanh SM  [ ] VinFast  [x] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác ________________  │
│                                                             │
│ Ai đang đau (Actor)? Bộ phận CSKH Vinhomes (tốn 12 tiếng để│
│ phân loại thủ công), cư dân (chờ đợi lâu).                   │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Cư dân điền biểu mẫu khiếu nại (bằng chữ viết tự do)    │
│   ──> 2. Nhân viên CSKH đọc và phân loại thủ công nội dung   │
│   ──> 3. Tạo ticket trên hệ thống ERP nội bộ                 │
│   ──> 4. Gửi ticket sang kỹ thuật hoặc ban quản lý tòa nhà   │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-3 (⏱ 8 phút/lượt)   │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2                 │
│ (Phân loại ngữ nghĩa khiếu nại -> Gán nhãn bộ phận tự động) │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ Giảm thời gian điều phối ticket từ 12 giờ ──> dưới 5 phút.   │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

### QUICK PROBLEM CARD #3: Vinmec Tự động tóm tắt hồ sơ bệnh án và xuất viện
```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán (1 câu): Bác sĩ mất nhiều thời gian soạn thảo tóm │
│ tắt hồ sơ xuất viện bằng tiếng Việt từ lịch sử điều trị.     │
│ Công ty thành viên: [ ] Xanh SM  [ ] VinFast  [ ] Vinhomes  │
│                     [x] Vinmec   [ ] Khác ________________  │
│                                                             │
│ Ai đang đau (Actor)? Bác sĩ (quá tải hành chính), Bệnh nhân │
│ (chờ đợi hoàn tất thủ tục ra viện lâu).                     │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Bác sĩ mở lịch sử điều trị, kết quả khám, đơn thuốc   │
│   ──> 2. Đọc và tổng hợp thủ công thông tin lâm sàng         │
│   ──> 3. Viết tay tóm tắt diễn tiến và hướng dẫn sau xuất viện│
│   ──> 4. In ấn và ký xác nhận gửi cho bệnh nhân              │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-3 (⏱ 25 phút/lượt)   │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3              │
│ (Trích xuất thông tin lâm sàng -> Draft tóm tắt xuất viện)  │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ Giảm thời gian làm thủ tục từ 30 phút ──> dưới 5 phút/lượt. │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```
