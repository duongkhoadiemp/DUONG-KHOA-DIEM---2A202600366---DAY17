# PRD Skeleton — Final (Validated Mindset)
## Problem
TikTok Shop seller Việt Nam báo cáo khó duy trì đủ video mỗi ngày do workflow thủ công, dẫn đến giảm khả năng duy trì traffic và phải tăng chi phí nhân sự khi scale.
---
## Target User
TikTok Shop seller Việt Nam (≥10 SKU, đăng video thường xuyên)
---
## User Story #1 (Daily bottleneck)
As a TikTok Shop seller đang đăng video mỗi ngày,  
I want đảm bảo luôn có đủ video để đăng,  
so that tôi không bị mất traffic và giảm doanh thu.
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
> MVP (Wizard of Oz): không phụ thuộc model cố định — ưu tiên output usable.
---
### Trade-offs
**Chấp nhận:**
- Video chưa hoàn hảo nhưng đăng được
- Latency vài phút
**Không chấp nhận:**
- Video không dám đăng
- Sai nội dung sản phẩm
---
### Data Source
- Ảnh sản phẩm (user)
- Mô tả sản phẩm
- Prompt template nội bộ
---
### Fallback UX
- Nếu output không usable → xử lý thủ công và gửi lại
- **Fallback SLA:** ≤ 60 phút  
- Nếu vượt SLA → thông báo user và có thể hủy request
---
## Success Criteria
**Phase 1 (Quality gate):**
- ≥60% video được user đăng lên TikTok  
**Phase 2 (Retention — chỉ đo nếu Phase 1 pass):**
- ≥40% user gửi sản phẩm mới trong 24–72h  
---
## Notes
- MVP = Wizard of Oz (manual backend)
- Không optimize model ở giai đoạn này
- Validate theo thứ tự:
  1. Quality (dám đăng)
  2. Retention (quay lại dùng)