# MVP BOUNDARY SHEET (Final)

## Idea (1 câu):
Tạo video sản phẩm nhanh cho TikTok seller để họ có thể tăng số SKU mà không cần tăng nhân sự.

---

## Riskiest Assumption:
Video AI tạo ra có chất lượng đủ để seller tự tin đăng lên TikTok Shop thật mà không cần chỉnh tay thêm.

---

## IN-SCOPE
(Tính năng cốt lõi bắt buộc để test giả thuyết)

- Input:
  - Ảnh sản phẩm
  - Mô tả ngắn: tên sản phẩm + điểm bán hàng chính

- Output:
  - 1 video dọc 9:16
  - Độ dài 15–30 giây
  - Có text overlay: hook + USP
  - Không voiceover
  - Format phù hợp TikTok Shop

- Delivery:
  - Nhận input qua Google Form
  - Trả output qua Zalo / Telegram

- Turnaround:
  - SLA: ≤ 30 phút / video
  - Tính từ lúc xác nhận nhận request đến lúc gửi video
  - Xử lý tuần tự: 1 request tại một thời điểm
  - Nếu có queue: báo ETA cho user

- Measurement:
  - **Quality metric:** user có đăng video lên TikTok Shop thật không
  - **Repeat usage metric:** user gửi thêm **sản phẩm mới** trong 24–72h
  - Khi user submit lần 2, hỏi rõ:
    > Đây là sản phẩm mới hay bạn muốn làm lại video trước?

  - Chỉ tính **sản phẩm mới** vào repeat usage.

---

## OUT-OF-SCOPE
(Tính năng tốt nhưng không cần cho MVP)

- Không build web app / dashboard riêng
- Không automation pipeline hoàn chỉnh
- Không batch processing
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
- Mỗi request chỉ trả 1 output duy nhất
- Nếu user muốn làm lại video cũ → ghi nhận là remake request, không tính là repeat success

---

## NOTE QUAN TRỌNG

- Cần recruit khoảng 20–30 TikTok seller để có đủ sample test.
- Scale về sample size, không scale về hạ tầng.
- Quality là điều kiện tiên quyết trước retention.
- Repeat usage chỉ có ý nghĩa nếu video lần đầu đủ tốt để seller dám dùng.