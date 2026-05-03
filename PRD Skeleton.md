# PRD Skeleton — Final v2 (Validated Mindset)

## Problem
TikTok Shop seller Việt Nam báo cáo khó duy trì đủ video mỗi ngày do workflow thủ công, dẫn đến giảm khả năng duy trì traffic và phải tăng chi phí nhân sự khi scale. Thị trường hiện có nhiều tool lẻ (batch/spam/edit), nhưng seller vẫn phải tự ghép workflow và chất lượng output không nhất quán.

---

## Target User
TikTok Shop seller Việt Nam (≥10 SKU, đăng video thường xuyên)

---

## User Story #1 (Daily bottleneck)
As a TikTok Shop seller đang đăng video mỗi ngày,  
I want có sẵn nhiều video với hook khác nhau để test và đăng,  
so that tôi không bị mất traffic và có thể tìm ra hook hiệu quả nhanh hơn.

---

## User Story #2 (Scaling context)
As a TikTok Shop seller đang mở rộng số SKU,  
I want tăng số video output mà không tăng nhân sự,  
so that tôi có thể scale kinh doanh với chi phí cố định.

---

## AI-Specific

### Model Selection
- Image: Gemini Image (Imagen)
- Video: Seedance 2.0 / Kling
- LLM: Claude (prompt)
> MVP (Wizard of Oz): không phụ thuộc model cố định — ưu tiên output usable. Model sẽ được quyết định sau Phase 1.

---

### Trade-offs
**Chấp nhận:**
- Giao trong 24h thay vì realtime
- Manual backend (Wizard of Oz)
- 3 video/pack thay vì volume lớn — ít nhưng có cấu trúc hook rõ
- Video chưa hoàn hảo 100% nhưng đăng được

**Không chấp nhận:**
- Video không dám đăng
- Sai nội dung sản phẩm
- MVP phụ thuộc vào workflow tạo video có chi phí quá cao khiến không thể lặp lại nhiều request test
- User chỉ dùng vì miễn phí, không có repeat paid usage

---

### Data Source
- Ảnh sản phẩm (user)
- Mô tả sản phẩm
- Prompt template nội bộ (hook angle framework)

---

### Fallback UX
- Nếu output không usable → xử lý thủ công và gửi lại
- **Fallback SLA:** ≤ 60 phút kể từ khi phát hiện lỗi
- Nếu vượt SLA → thông báo user và có thể hủy request

---

### Business / Cost Constraint
- MVP không dùng backend API video unlimited
- Giai đoạn đầu dùng Wizard of Oz + selected AI tools để đo willingness-to-pay và cost/pack trước khi tự động hóa
- Ghi nhận cost/pack thực tế ngay từ request đầu tiên để biết workflow nào nên automate sau
- Trước khi tự động hóa, cần xác nhận cost/pack nằm trong ngưỡng có thể lặp lại bền vững

---

## Success Criteria

**Main Hypothesis (MVP — test ngay):**
- ≥60% pack có ít nhất 1/3 video (≥1 trong 3) được user đăng lên TikTok

**Next Signals (observe nếu có điều kiện — không phải primary goal của MVP):**
- Retention: ≥40% user order pack mới cho sản phẩm khác trong 72h–7 ngày
- Paid intent: ≥20–30% user chấp nhận mức giá cụ thể khi được hỏi sau khi đã dùng pack

---

## Notes
- MVP = Wizard of Oz (manual backend)
- Không optimize model ở giai đoạn này
- Main hypothesis: Quality / Usability (seller có dám đăng không)
- Next signals (observe sau): Retention → Paid intent