# Day 17 Submission
**Student:** Dương Khoa Diễm
**Date:** 03/05/2026
**Product idea:** Tạo Video Test Pack bằng AI cho TikTok Shop seller Việt Nam — 3 video / 3 hook khác nhau cho 1 sản phẩm, giao trong 24h, giúp seller test hook hiệu quả và tăng SKU mà không cần tăng nhân sự.

---

## 1. MVP Boundary Sheet

**Riskiest Assumption (Phase 1 — phải validate trước):**
> Seller thấy Video Test Pack đủ usable để đăng ít nhất 1 video lên TikTok Shop thật.

**Critical Assumption (Phase 3 — chỉ test được sau Phase 1 + 2):**
> Nếu pack đủ usable và seller quay lại, họ sẽ chấp nhận trả tiền cho pack tiếp theo thay vì chỉ dùng tool spam rẻ hoặc tự làm bằng nhiều tool lẻ.

**In-Scope** (tối đa 3):
- [x] Nhận input (ảnh sản phẩm + mô tả ngắn) qua Google Form, trả output (Video Test Pack: 3 video dọc 9:16, 15–30 giây, mỗi video 1 hook angle khác nhau, có text overlay) qua Zalo/Telegram trong ≤24h — test giả định: seller có dám đăng video AI tạo không?
- [x] Đo main metric: hỏi user có đăng ít nhất 1 video trong pack lên TikTok Shop thật không — test giả định: ≥60% pack có ít nhất 1 video được đăng là mốc thành công của MVP.
- [x] Observe next signals nếu có điều kiện: retention (order pack mới trong 72h–7 ngày) và paid intent (willingness-to-pay sau khi đã dùng pack) — không phải primary goal của MVP, nhưng ghi nhận để chuẩn bị cho giai đoạn sau.

**Out-of-Scope:**
- Web app / dashboard riêng — lý do bỏ: không cần cho MVP, tốn dev time không test được giả định cốt lõi.
- Automation pipeline hoàn chỉnh / batch processing — lý do bỏ: Wizard of Oz manual đủ để validate; tự động hóa là bước sau khi biết workflow nào tạo value và cost/pack có thể lặp lại.
- Tích hợp TikTok / Shopee API — lý do bỏ: ngoài phạm vi MVP, chưa cần thiết ở giai đoạn này.

**Non-Goals:**
- Không build hệ thống AI hoàn chỉnh hay optimize model/performance trong giai đoạn này.
- Không làm revision theo yêu cầu cá nhân; mỗi request chỉ trả 1 pack duy nhất.
- Không dùng workflow tạo video có chi phí quá cao khiến không thể lặp lại nhiều request test — ghi nhận cost/pack thực tế để biết bước nào nên automate sau.

---

## 2. PRD Skeleton

### Problem Statement
> TikTok Shop seller Việt Nam (≥10 SKU, đăng video thường xuyên) báo cáo khó duy trì đủ video mỗi ngày do workflow thủ công, dẫn đến giảm traffic và phải tăng chi phí nhân sự khi scale. Thị trường hiện có nhiều tool lẻ (batch/spam/edit), nhưng seller vẫn phải tự ghép workflow và chất lượng output không nhất quán.

### Target User
> TikTok Shop seller Việt Nam có từ 10 SKU trở lên, đang đăng video sản phẩm thường xuyên, cần tăng output video nhưng không muốn tăng chi phí nhân sự.

### User Stories

**Story 1:**
> As a TikTok Shop seller đang đăng video mỗi ngày, I want có sẵn nhiều video với hook khác nhau để test và đăng, so that tôi không bị mất traffic và tìm ra hook hiệu quả nhanh hơn.

**Story 2:**
> As a TikTok Shop seller đang mở rộng số SKU, I want tăng số video output mà không tăng nhân sự, so that tôi có thể scale kinh doanh với chi phí cố định.

### AI-Specific

**Model Selection:**
- Model: Gemini Image (Imagen) cho ảnh, Seedance 2.0 / Kling cho video, Claude cho prompt LLM
- Lý do chọn: MVP ưu tiên output usable — không lock vào model cố định; sẽ quyết định model sau Phase 1.
- Trade-offs chấp nhận: Giao trong 24h thay vì realtime; manual backend; video chưa hoàn hảo 100% nhưng đăng được.
- Trade-offs không chấp nhận: Video không dám đăng; sai nội dung sản phẩm; MVP phụ thuộc workflow có chi phí quá cao khiến không thể lặp lại nhiều request test; user chỉ dùng vì miễn phí.

**Data Requirements:**
- Nguồn: Ảnh sản phẩm (do user cung cấp), mô tả sản phẩm (tên + điểm bán hàng chính), prompt template nội bộ (hook angle framework).
- Owner: User cung cấp data đầu vào; team nắm prompt template.
- Update frequency: Theo từng request (on-demand).

**Fallback UX**
- Chiến lược: Graceful Handover (xử lý thủ công khi AI không đủ tự tin)
- Trigger: Khi output video không đạt ngưỡng usable (không dám đăng lên TikTok thật).
- Hành động: Gửi lại request xử lý thủ công; Fallback SLA ≤60 phút kể từ khi phát hiện lỗi. Nếu vượt SLA → thông báo user và có thể hủy request.
- User options: User chờ xử lý thủ công, hoặc hủy request và submit lại với ảnh/mô tả rõ hơn.

**Business / Cost Constraint:**
- MVP không dùng backend API video unlimited.
- Dùng Wizard of Oz + selected AI tools để đo willingness-to-pay và cost/pack trước khi tự động hóa.
- Ghi nhận cost/pack thực tế ngay từ request đầu tiên để biết workflow nào nên automate sau.
- Trước khi tự động hóa, cần xác nhận cost/pack nằm trong ngưỡng có thể lặp lại bền vững.

### Success Metrics
- Primary metric: Tỷ lệ pack có ít nhất 1 video được đăng lên TikTok Shop thật (Quality rate)
- Ngưỡng thành công MVP: ≥60% pack có ít nhất 1 video được đăng
- Timeframe đo lường: Trong vòng 24 giờ kể từ khi user nhận pack

Next signals (observe nếu có điều kiện — không enforce ở MVP):
- Retention: ≥40% user order pack mới trong 72h–7 ngày
- Paid intent: ≥20–30% user chấp nhận mức giá cụ thể khi được hỏi sau khi đã dùng pack

### Dependencies & Constraints
- Cần recruit 20–30 TikTok seller để đủ sample size test.
- Xử lý tuần tự: 1 request tại một thời điểm; nếu có queue thì báo ETA cho user.
- MVP = Wizard of Oz (manual backend) — không optimize model hay scale infrastructure.

---

## 3. Hypothesis Table

### Main Hypothesis (MVP — test ngay)
> "Chúng tôi tin rằng việc cung cấp Video Test Pack usable (3 video / 3 hook) sẽ giúp TikTok Shop seller có đủ content để duy trì tần suất đăng và test hook hiệu quả.
> Chúng tôi sẽ biết mình đúng khi thấy ≥60% pack có ít nhất 1 video được user đăng lên TikTok trong vòng 24 giờ."

Riskiest assumption: Seller thấy Video Test Pack đủ usable để tự tin đăng lên TikTok Shop thật.

Cách test cheapest: Wizard of Oz — nhận input qua Google Form, tạo pack thủ công có AI-assist, giao qua Zalo/Telegram; hỏi user có đăng không.

### Next Signals (observe nếu có điều kiện — không phải hypothesis chính của MVP)

**Retention signal:**
> "Nếu pack lần đầu đủ tốt, seller sẽ quay lại order pack mới cho sản phẩm khác trong 72h–7 ngày."
> Ngưỡng tham khảo: ≥40% user order pack mới.

**Paid intent signal:**
> "Nếu seller thấy value, họ sẽ chấp nhận trả tiền cho pack tiếp theo thay vì chỉ dùng tool spam rẻ."
> Ngưỡng tham khảo: ≥20–30% user chọn mức giá cụ thể khi được hỏi sau khi đã dùng pack.
> Chỉ có ý nghĩa sau khi quality pass — nếu seller còn chưa dám đăng video thì hỏi trả tiền không có nhiều ý nghĩa.

---

## 4. PMF Scorecard

**Aha Moment:**
> User nhận Video Test Pack → đăng ít nhất 1 video trong pack lên TikTok Shop thật (hành vi cụ thể, quan sát được — không phải cảm giác).

**Actionable Metric**
> Quality rate: % pack có ít nhất 1 video được user đăng lên TikTok Shop thật / tổng pack giao — đo bằng cách hỏi trực tiếp user sau 24h nhận pack.

**PMF Method:**
> Main hypothesis: Aha Moment Tracking (quality / usability)
> Ngưỡng thành công MVP: ≥60% pack có video được đăng
>
> Next signals (observe, không enforce ở MVP):
> - Retention: ≥40% user order pack mới trong 72h–7 ngày
> - Paid intent: ≥20–30% user chấp nhận mức giá cụ thể khi được hỏi sau khi đã dùng pack

**Vanity Metrics tôi sẽ không dùng:**
- Số lượt view / like trên video (không phản ánh ý định dùng lại).
- Số người đăng ký / quan tâm (chưa chứng minh họ dùng thật).
- Số video đã tạo ra (không cho biết video có được đăng không).
- "Quan tâm trả tiền" — chỉ tính paid intent khi user chọn mức giá cụ thể.

---

## 5. AI Critique Log

**Điểm AI chỉ ra:**
1. Riskiest Assumption ban đầu chỉ đo quality, chưa có paid signal — Action: Accept — Lý do: Thị trường có nhiều tool spam rẻ; cần validate paid intent. Tuy nhiên paid intent là next signal, không phải main hypothesis của MVP — seller còn chưa dám đăng thì hỏi trả tiền không có nhiều ý nghĩa.
2. Success metric cần tách rõ main hypothesis vs next signals — Action: Accept — Lý do: Main hypothesis (quality/usability) là thứ MVP test ngay. Retention và paid intent là next signals — quan trọng nhưng chỉ observe nếu có điều kiện, không enforce ở giai đoạn này.
3. Repeat usage metric cần làm rõ "sản phẩm mới" vs "remake video cũ" — Action: Accept — Lý do: Đã thêm câu hỏi phân loại khi user submit lần 2; chỉ đếm sản phẩm mới vào repeat success.

**Thay đổi lớn nhất giữa Version A và Version B:**
> Version A định nghĩa MVP là "1 video trong ≤30 phút" với 3 phase song song. Version B chuyển sang "Video Test Pack (3 video / 3 hook, giao trong 24h)" với cấu trúc rõ hơn: 1 main hypothesis (quality/usability) và 2 next signals (retention + paid intent) — phù hợp hơn với product thinking: test thứ rủi ro nhất trước, observe thứ còn lại sau.

---

## 6. Self-assessment

Mắt xích nào trong [MVP Boundary / PRD / Hypothesis / PMF] bạn đang yếu nhất?
> Monetization signal — tôi chưa có kinh nghiệm thiết kế willingness-to-pay test đủ chính xác. Cụ thể: chưa biết nên đưa ra mức giá nào, và làm sao tránh bias khi hỏi ("bạn có trả X không?" vs "bạn chọn A, B hay C?").

**Plan:**
> Phỏng vấn 3–5 TikTok seller trước khi chạy MVP để xác nhận pain có tồn tại, đo workflow hiện tại mất bao nhiêu thời gian/video, và thử hỏi willingness-to-pay bằng cách đưa ra 2–3 mức giá cụ thể.

---

**Learning after Market / Code Exploration:**
> Sau khi khảo sát thị trường, tôi nhận ra nhiều competitor đang bán tool lẻ: prompt tool, batch video tool, edit tool, hoặc automation chạy trên account của user. Nếu build end-to-end full automation quá sớm, API cost và latency sẽ đẩy chi phí lên cao — trong khi chưa biết seller có thực sự dùng output hay không.
>
> Vì vậy tôi điều chỉnh MVP sang Video Test Pack / Wizard of Oz để validate quality và usability trước: seller có dám đăng video AI tạo lên TikTok Shop thật không. Thay vì cạnh tranh bằng volume (tool spam rẻ), tôi test xem seller có cần output usable + có cấu trúc hook rõ không. Retention và paid intent là next signals — sẽ observe sau nếu quality pass.
>
> **Đây không phải là đổi bỏ ý tưởng ban đầu, mà là thu hẹp scope MVP.** Vision dài hạn vẫn là AI Content Factory, nhưng MVP hiện tại được thu hẹp thành Video Test Pack để validate quality/usability trước khi build automation hoàn chỉnh.