# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Hãy sử dụng **4 Lenses** dưới đây để quét qua hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại **ít nhất 5 bài toán/bottleneck** thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:
1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày. (Ví dụ: So khớp hóa đơn sạc điện tại VinFast, route lại chuyến taxi tại Xanh SM).
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên. (Ví dụ: Soạn thảo phản hồi đánh giá 1-star của cư dân Vinhomes).
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn. (Ví dụ: Chatbot CSKH Vinpearl hỗ trợ đặt vé vui chơi).
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn. (Ví dụ: Tài xế Xanh SM phàn nàn về việc hệ thống gợi ý điểm đón khách không chính xác).

> [!TIP]
> **🤖 AI Prompts — Partner brainstorm:**
> Hãy sử dụng prompt sau để brainstorm các bài toán thực tế nếu bạn chưa có ý tưởng:
> *"Tôi là AI Engineer tại Vin Smart Future (Vingroup). Tôi đang tìm kiếm các pain point vận hành cụ thể có thể tối ưu bằng AI cho mảng [Chọn một: VinFast / Xanh SM / Vinhomes / Vinmec]. Hãy gợi ý cho tôi 5 quy trình nghiệp vụ thủ công, tốn nhiều thời gian và gây rò rỉ hiệu suất kèm con số thống kê ước tính về tổn thất."*

### 📝 List bài toán của tôi:
| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 | VinFast | AI có thể tốt hơn | Tự động đề xuất lịch trình sạc tối ưu và trạm sạc trống phù hợp với loại cổng sạc (CCS2/GBT) của từng dòng xe điện (VF5, VF8, VF9) |
| 2 | Vinhomes | Lặp lại | Phân loại và tự động điều hướng các ý kiến phản ánh bằng tiếng Việt của cư dân gửi qua App Vinhomes Resident đến đúng ban quản lý/phòng ban xử lý |
| 3 | Xanh SM | Stakeholder Pain | Phân loại lý do hủy chuyến của tài xế và khách hàng thông qua transcript ghi âm cuộc gọi hoặc ghi chú phản hồi ngắn để tối ưu chất lượng dịch vụ |
| 4 | Vinmec | AI có thể tốt hơn | Trợ lý phân loại triệu chứng và gợi ý chuyên khoa phù hợp để hỗ trợ bệnh nhân đặt lịch khám trực tuyến chính xác |
| 5 | Vinpearl | Tốn thời gian | Tự động đọc email yêu cầu đặt phòng số lượng lớn (Group Booking) phức tạp từ đối tác lữ hành để trích xuất thông tin và soạn thảo booking nháp |

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu): Đề xuất trạm sạc trống và loại cổng sạc   │
│ phù hợp cho tài xế xe điện VinFast nhằm tối ưu lịch trình.  │
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Tài xế xe điện VinFast (VF5, VF8, VF9) │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Xe báo pin yếu ──> 2. Tài xế dừng xe tìm trạm trên map │
│   ──> 3. Lọc thủ công loại cổng sạc phù hợp với dòng xe     │
│   ──> 4. Di chuyển tới trạm và xếp hàng nếu trạm đang đầy.  │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 & 4 (⏱ 15 phút/lượt) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3            │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   Giảm thời gian tìm kiếm và xếp hàng sạc từ 20 min ──> <3 min│
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [x] Agent │
└─────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu): Tự động phân loại và chuyển tiếp phản ánh │
│ của cư dân qua App Vinhomes Resident đến đúng ban quản lý.  │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên Helpdesk trực ban quản lý    │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Nhận phản ánh từ cư dân qua app ──> 2. Nhân viên đọc   │
│   nội dung tiếng Việt ──> 3. Phân tích phân loại bộ phận    │
│   (Kỹ thuật/Vệ sinh/An ninh) ──> 4. Chuyển ticket thủ công. │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 5 phút/lượt) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3            │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   Giảm thời gian điều phối phản ánh từ 10 min ──> under 1 min│
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán (1 câu): Tự động phân tích transcript cuộc gọi hủy │
│ chuyến và ghi chú của tài xế để phân loại lý do hủy.        │
│ Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên kiểm soát chất lượng (QC)    │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Chuyến xe bị hủy ──> 2. Nhân viên QC nghe ghi âm cuộc  │
│   gọi hủy chuyến ──> 3. Đọc ghi chú viết tắt của tài xế     │
│   ──> 4. Phân loại lý do hủy chuyến vào bảng báo cáo tuần.  │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 4 phút/lượt) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2, 3 & 4         │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   Tăng tỷ lệ cuốc hủy được phân loại từ 5% ──> 100% cuốc    │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

> [!TIP]
> **🤖 AI Prompts — Stress-Test thẻ bài toán:**
> Hãy dán nội dung thẻ bài toán của bạn vào LLM để nhận phản biện:
> *"Đây là một thẻ bài toán vận hành tôi đề xuất cho Vin Smart Future: [Dán nội dung]. Hãy đóng vai trò là một CFO và Trưởng phòng Vận hành cực kỳ khắt khe, chỉ ra cho tôi 3 điểm yếu về logic, metric, và giải thích vì sao rule-based code thông thường có thể giải quyết bài toán này tốt hơn là dùng AI."*

---
