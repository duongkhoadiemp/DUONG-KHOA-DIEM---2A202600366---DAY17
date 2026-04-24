
# Day 17 Submission
**Student:** Dương Khoa Diễm
**Date:** 24/04/2026
**Product idea:** Tạo video sản phẩm nhanh bằng AI cho TikTok Shop seller Việt Nam, giúp họ tăng số SKU mà không cần tăng nhân sự.

## 1. MVP Boundary Sheet

**Riskiest Assumption:**
> Video AI tạo ra có chất lượng đủ để seller tự tin đăng lên TikTok Shop thật mà không cần chỉnh tay thêm.

**In-Scope** (tối đa 3):
- [x] Nhận input (ảnh sản phẩm + mô tả ngắn) qua Google Form, trả output (video dọc 9:16, 15–30 giây, có text overlay hook + USP) qua Zalo/Telegram trong ≤30 phút — test giả định: seller có dám đăng video AI tạo không?
- [x] Đo quality metric: hỏi user có đăng video lên TikTok Shop thật không — test giả định: ≥60% video được đăng là mốc thành công.
- [x] Đo repeat usage metric: user gửi thêm sản phẩm mới trong 24–72h (chỉ đếm sản phẩm mới, không phải remake) — test giả định: quality đủ tốt tạo ra hành vi quay lại.

**Out-of-Scope:**
- Web app / dashboard riêng — lý do bỏ: không cần cho MVP, tốn dev time không test được giả định cốt lõi.
- Automation pipeline hoàn chỉnh / batch processing — lý do bỏ: Wizard of Oz manual đủ để validate, tự động hóa là bước sau.
- Tích hợp TikTok / Shopee API — lý do bỏ: ngoài phạm vi MVP, chưa cần thiết ở giai đoạn này.

**Non-Goals:**
- Không build hệ thống AI hoàn chỉnh hay optimize model/performance trong giai đoạn này.
- Không làm revision theo yêu cầu cá nhân; mỗi request chỉ trả 1 output duy nhất.

## 2. PRD Skeleton

### Problem Statement
> TikTok Shop seller Việt Nam (≥10 SKU, đăng video thường xuyên) báo cáo khó duy trì đủ video mỗi ngày do workflow thủ công, dẫn đến giảm traffic và phải tăng chi phí nhân sự khi scale.

### Target User
> TikTok Shop seller Việt Nam có từ 10 SKU trở lên, đang đăng video sản phẩm thường xuyên, cần tăng output video nhưng không muốn tăng chi phí nhân sự.

### User Stories

**Story 1:**
> As a TikTok Shop seller đang đăng video mỗi ngày, I want đảm bảo luôn có đủ video để đăng, so that tôi không bị mất traffic và giảm doanh thu.

**Story 2:**
> As a TikTok Shop seller đang mở rộng số SKU, I want tăng số video output mà không tăng nhân sự, so that tôi có thể scale kinh doanh với chi phí cố định.

### AI-Specific

**Model Selection:**
- Model: Gemini Image (Imagen) cho ảnh, Seedance 2.0 / Kling cho video, Claude cho prompt LLM
- Lý do chọn: MVP ưu tiên output usable — không lock vào model cố định; linh hoạt chọn model cho từng bước.
- Trade-offs chấp nhận: Video chưa hoàn hảo 100% nhưng đăng được; latency vài phút.
- Trade-offs không chấp nhận: Video không dám đăng; sai nội dung sản phẩm.

**Data Requirements:**
- Nguồn: Ảnh sản phẩm (do user cung cấp), mô tả sản phẩm (tên + điểm bán hàng chính), prompt template nội bộ.
- Owner: User cung cấp data đầu vào; team nắm prompt template.
- Update frequency: Theo từng request (on-demand).

**Fallback UX**
- Chiến lược: Graceful Handover (xử lý thủ công khi AI không đủ tự tin)
- Trigger: Khi output video không đạt ngưỡng usable (không dám đăng lên TikTok thật).
- Hành động: Gửi lại request xử lý thủ công; Fallback SLA ≤60 phút. Nếu vượt SLA → thông báo user và có thể hủy request.
- User options: User chờ xử lý thủ công, hoặc hủy request và submit lại với ảnh/mô tả rõ hơn.

### Success Metrics
- Primary metric: Tỷ lệ video được user đăng lên TikTok Shop thật (Quality rate)
- Ngưỡng thành công: Phase 1 — ≥60% video được đăng; Phase 2 — ≥40% user gửi sản phẩm mới trong 24–72h (chỉ đo nếu Phase 1 pass)
- Timeframe đo lường: Trong vòng 24 giờ kể từ khi user nhận video

### Dependencies & Constraints
- Cần recruit 20–30 TikTok seller để đủ sample size test.
- Xử lý tuần tự: 1 request tại một thời điểm; nếu có queue thì báo ETA cho user.
- MVP = Wizard of Oz (manual backend) — không optimize model hay scale infrastructure.

## 3. Hypothesis Table

### Hypothesis 1 (cho tính năng In-Scope #1 — Quality)
> "Chúng tôi tin rằng việc cung cấp video sản phẩm usable (đăng được) sẽ giúp TikTok Shop seller duy trì tần suất đăng video và không bị mất traffic.
> Chúng tôi sẽ biết mình đúng khi thấy ≥60% video được user đăng lên TikTok trong vòng 24 giờ."

Riskiest assumption: Video AI tạo ra có chất lượng đủ để seller tự tin đăng lên TikTok Shop thật mà không cần chỉnh tay thêm.

Cách test cheapest: Wizard of Oz — nhận input qua Google Form, tạo video thủ công có AI-assist, giao qua Zalo/Telegram; hỏi user có đăng không.

### Hypothesis 2 (Retention)
> "Chúng tôi tin rằng nếu video lần đầu đủ tốt để seller dám dùng, họ sẽ quay lại gửi thêm sản phẩm mới.
> Chúng tôi sẽ biết mình đúng khi thấy ≥40% user gửi sản phẩm mới trong 24–72h sau khi nhận video đầu tiên."

## 4. PMF Scorecard

**Aha Moment:**
> User nhận video đầu tiên và đăng video đó lên TikTok Shop thật (hành vi cụ thể, quan sát được — không phải cảm giác).

**Actionable Metric**
> Quality rate: % video được user đăng lên TikTok Shop thật / tổng video giao — đo bằng cách hỏi trực tiếp user sau 24h nhận video.

**PMF Method:**
> Aha Moment Tracking + Retention Check
> Ngưỡng thành công: Phase 1 — ≥60% video được đăng (Quality gate); Phase 2 — ≥40% user gửi sản phẩm mới trong 24–72h (Retention signal).

**Vanity Metrics tôi sẽ không dùng:**
- Số lượt view / like trên video (không phản ánh ý định dùng lại).
- Số người đăng ký / quan tâm (chưa chứng minh họ dùng thật).
- Số video đã tạo ra (không cho biết video có được đăng không).

## 5. AI Critique Log

**Điểm AI chỉ ra:**
1. Riskiest Assumption ban đầu còn mơ hồ ("chất lượng tốt") — Action: Accept — Lý do: Đã làm rõ thành tiêu chí hành vi cụ thể: "seller tự tin đăng lên TikTok Shop thật mà không cần chỉnh tay thêm."
2. Success metric cần tách 2 phase rõ ràng thay vì gộp chung — Action: Accept — Lý do: Phase 1 (Quality) là điều kiện tiên quyết; Phase 2 (Retention) chỉ có ý nghĩa nếu Phase 1 pass — tránh đo retention trên quality kém.
3. Repeat usage metric cần làm rõ "sản phẩm mới" vs "remake video cũ" — Action: Accept — Lý do: Đã thêm câu hỏi phân loại khi user submit lần 2; chỉ đếm sản phẩm mới vào repeat success.

**Thay đổi lớn nhất giữa Version A và Version B:**
> Version A gộp quality và retention thành một metric chung, chưa rõ cách phân biệt repeat success. Version B tách rõ 2 phase, định nghĩa chính xác "sản phẩm mới" vs remake, và thêm Fallback SLA cụ thể (≤60 phút) để xử lý trường hợp AI không đủ tự tin.

## 6. Self-assessment

Mắt xích nào trong [MVP Boundary / PRD / Hypothesis / PMF] bạn đang yếu nhất?
> PMF — cụ thể là phần Retention Curve. Tôi biết Aha Moment và quality metric, nhưng chưa có đủ framework để phân tích liệu retention signal (40% repeat trong 24–72h) có thực sự phản ánh PMF hay chỉ là novelty effect.

Open questions bạn muốn giải đáp tiếp:
1. Làm sao phân biệt được seller quay lại vì video tốt thật sự vs vì tò mò dùng thử lần 2? (Cần thêm câu hỏi qualitative?)
2. Nếu Phase 1 pass (≥60%) nhưng Phase 2 thất bại (<40%), lý do nào có khả năng cao nhất — chất lượng video không đủ nhất quán, hay seller không có nhu cầu đủ thường xuyên?