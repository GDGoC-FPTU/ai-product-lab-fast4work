# Vin Smart Future (Vinmec Use Case)
---

Là AI Product Engineer tại Vin Smart Future, phụ trách chuyển đổi số cho hệ thống bệnh viện đa khoa quốc tế Vinmec.

Thông qua quá trình quan sát thực tế các bác sĩ nội trú và bác sĩ điều trị chính, tôi phát hiện một "điểm mù" nghiêm trọng trong vận hành: Các bác sĩ có chuyên môn cao đang phải tiêu tốn quá nhiều thời gian cho các công việc hành chính và giấy tờ, đặc biệt là việc tổng hợp hồ sơ bệnh án khi bệnh nhân xuất viện. Điều này không chỉ gây lãng phí nguồn lực y tế đắt giá mà còn làm chậm trễ quá trình giải phóng giường bệnh.

## Phase 1 - SCAN: Tìm kiếmm cơ hội cá nhân
---

| # | Subsidiary | Lens | Mô tả bài toán |
| :-: | :----: | :--- | :----- |
| # | Vinmec | Tốn thời gian | Bác sĩ mất quá nhiều thời gian đọc lại hàng chục trang bệnh án/xét nghiệm để viết Tóm tắt hồ sơ xuất viện. |
| # | Vinhomes | Lặp lại |. Ban quản lý liên tục phải trả lời các câu hỏi lặp lại của cư dân về giờ mở cửa bể bơi, phí gửi xe. | 
| # | VinFast | AI-upgrade | Trợ lý ảo (ViVi) trên xe nhận diện sai lệnh giọng nói khi người dùng sử dụng tiếng lóng hoặc từ ngữ địa phương. |
| # | Vinpearl | Pain từ người khác |. Lễ tân mất quá nhiều thời gian nhập liệu thủ công thông tin hộ chiếu của các đoàn khách du lịch lớn. |
| # | XanhSM | Tốn thời gian | CSKH phải nghe lại toàn bộ file ghi âm cuộc gọi dài 10 phút chỉ để xác định xem lỗi hủy chuyến thuộc về tài xế hay khách hàng. |


## Phase 2 - Quick Assess (Quyết định lựa chọn)
---

Từ danh sách trên, tôi đưa ra quyết định Phản biện trực diện để thu hẹp bài toán:

- Loại bỏ #4 (Vinpearl): Bài toán nhập liệu Passport không cần đến LLM. Một hệ thống Rule-based kết hợp OCR (Nhận dạng ký tự quang học) truyền thống sẽ giải quyết nhanh, rẻ và chính xác hơn AI tạo sinh.

- Loại bỏ #2 (Vinhomes): Việc trả lời câu hỏi lặp lại có thể dùng Chatbot kịch bản sẵn (Decision Tree), dùng LLM ở giai đoạn này dễ sinh rủi ro "ảo giác" (Hallucination) đưa sai biểu phí.

- Quyết định CHỌN bài toán #1 (Vinmec): Đây là bài toán Tóm tắt văn bản phi cấu trúc (Unstructured data summarization) – thế mạnh tuyệt đối của LLM, đồng thời mang lại giá trị kinh tế khổng lồ khi giải phóng thời gian cho bác sĩ.

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Bác sĩ điều trị mất quá nhiều thời gian viết      │
│ Tóm tắt hồ sơ xuất viện từ hàng loạt giấy tờ rời rạc.       │
│ Công ty thành viên: [x] Vinmec                              │
│                                                             │
│ Ai đang đau? Bác sĩ (Quá tải), Bệnh viện (Kẹt giường bệnh)  │
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│   1. Mở hệ thống HIS, đọc lại toàn bộ log điều trị          │
│   → 2. Xem lại các chỉ số xét nghiệm (Lab results)          │
│   → 3. Gõ tay bản tóm tắt diễn biến bệnh & hướng điều trị   │
│   → 4. Ký duyệt và in cho bệnh nhân                         │
│                                                             │
│ Bước nào tốn nhất? Bước 1-2-3 (⏱ 25 - 30 phút/bệnh nhân)    │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1-2-3            │
│ (AI tự động trích xuất thông tin lõi và sinh bản nháp)      │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm thời gian viết tóm tắt từ 25 phút ──> dưới 5 phút.     │
│                                                             │
│ Quick Architecture: [x] LLM Feature (Summarization + HITL)  │
└─────────────────────────────────────────────────────────────┘