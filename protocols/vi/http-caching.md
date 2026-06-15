---
title: "HTTP Cache Thực Dụng"
description: "Một hướng dẫn thực tế về cache header, độ tươi dữ liệu, và khi nào nên phục vụ từ edge thay vì origin."
tags: ["http", "caching", "protocols"]
date: "2025-04-08"
lang: "vi"
---

# HTTP Cache Thực Dụng

HTTP caching không chỉ là tốc độ. Nó là cách giảm khối lượng công việc lặp lại đúng ở những chỗ gây tốn kém nhất.

![Sơ đồ luồng lưu lượng](/media/notes/protocols/network-traffic.svg)

## Câu Chuyện Của Cache

Trình duyệt hỏi một lần, edge phục vụ nhiều lần, và origin chỉ xuất hiện khi bản cache đã cũ.

### Cache-Control trong thực tế
Cache-Control: public, max-age=300, stale-while-revalidate=60

#### Ý nghĩa từng phần

- `public` cho phép cache dùng chung lưu response.
- `max-age=300` giữ nội dung tươi trong năm phút.
- `stale-while-revalidate=60` cho phép phục vụ bản hơi cũ trong khi tải bản mới.

## Độ tươi và xác thực

| Chiến lược | Tác dụng | Đánh đổi |
|-----------|----------|----------|
| Freshness | Dùng lại response cho đến khi hết hạn | Rất nhanh, có thể hơi cũ |
| Validation | Hỏi server xem bản cache còn hợp lệ không | Chậm hơn, nhưng an toàn hơn |

### Ví dụ ETag
If-None-Match: "abc123"
Server có thể trả `304 Not Modified` khi nội dung không đổi.

## Khi cache thất bại

Cache thất bại khi nhiều người dùng nhận cùng một response nhưng lẽ ra không nên như vậy.

#### Quy tắc ngắn

Nếu nội dung mang tính cá nhân hóa, hãy đặt cache key thật rõ hoặc bỏ cache.

## Danh sách kiểm tra

| Câu hỏi | Trả lời mặc định |
|---------|------------------|
| Response có công khai không? | Cache nó |
| Có phụ thuộc người dùng không? | Đừng chia sẻ |
| Có thay đổi thường xuyên không? | Dùng validation |
| Có tĩnh không? | Cho sống lâu |

