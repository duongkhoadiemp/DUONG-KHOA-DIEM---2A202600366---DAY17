# MVP BOUNDARY SHEET (Final v2)

## Idea (1 câu):
Tạo Video Test Pack cho TikTok seller để họ test nhiều hook cùng lúc và tăng số SKU mà không cần tăng nhân sự.

---

## Riskiest Assumption (Phase 1 — phải validate trước):
Seller thấy Video Test Pack đủ usable để đăng ít nhất 1 video lên TikTok Shop thật.

## Critical Assumption (Phase 3 — chỉ test được sau Phase 1 + 2):
Nếu pack đủ usable và seller quay lại, họ sẽ chấp nhận trả tiền cho pack tiếp theo thay vì chỉ dùng tool spam rẻ hoặc tự làm bằng nhiều tool lẻ.

---

## IN-SCOPE
(Tính năng cốt lõi bắt buộc để test giả thuyết)

- Input:
  - Ảnh sản phẩm
  - Mô tả ngắn: tên sản phẩm + điểm bán hàng chính

- Output:
  - 1 Video Test Pack gồm 3 video dọc 9:16
  - Mỗi video 15–30 giây, dùng 1 hook angle khác nhau
  - Có text overlay: hook + USP
  - Không voiceover
  - Format phù hợp TikTok Shop

- Delivery:
  - Nhận input qua Google Form
  - Trả output qua Zalo / Telegram

- Turnaround:
  - SLA: ≤ 24h / pack
  - Tính từ lúc xác nhận nhận request đến lúc gửi pack
  - Xử lý tuần tự: 1 request tại một thời điểm
  - Nếu có queue: báo ETA cho user

- Measurement:
  - **Main metric (MVP):** user có đăng ít nhất 1 video trong pack lên TikTok Shop thật không
  - Khi user submit lần 2, hỏi rõ:
    > Đây là sản phẩm mới hay bạn muốn làm lại pack trước?
  - Chỉ tính **sản phẩm mới** vào repeat usage.

- Next signals (observe, không phải primary goal của MVP):
  - **Retention:** user order pack mới cho sản phẩm khác trong 72h–7 ngày
  - **Paid intent:** user chấp nhận trả tiền khi được hỏi mức giá cụ thể — chỉ hỏi sau khi đã nhận và dùng pack

---

## OUT-OF-SCOPE
(Tính năng tốt nhưng không cần cho MVP)

- Không build web app / dashboard riêng
- Không automation pipeline hoàn chỉnh
- Không batch processing tự động
- Không tích hợp TikTok / Shopee
- Không nhận video mẫu từ user
- Không nhận yêu cầu custom style phức tạp

---

## NON-GOALS
(Ranh giới đỏ – tuyệt đối không làm trong giai đoạn này)

- Không build hệ thống AI hoàn chỉnh
- Không optimize model / performance
- Không scale infrastructure
- Không làm revision theo yêu cầu cá nhân
- Mỗi request chỉ trả 1 pack duy nhất
- Nếu user muốn làm lại pack cũ → ghi nhận là remake request, không tính là repeat success
- Không dùng workflow tạo video có chi phí quá cao khiến không thể lặp lại nhiều request test — phải ghi nhận cost/pack thực tế để biết bước nào nên automate sau

---

## NOTE QUAN TRỌNG

- Cần recruit khoảng 20–30 TikTok seller để có đủ sample test.
- Scale về sample size, không scale về hạ tầng.
- Main hypothesis của MVP là quality / usability — seller có dám đăng không.
- Retention và paid intent là next signals: quan trọng nhưng chỉ có ý nghĩa sau khi quality pass.
- Không hỏi willingness-to-pay trước khi user đã nhận và dùng pack.