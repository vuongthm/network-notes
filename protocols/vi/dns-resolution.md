---
title: "DNS Phân Giải Trong 6 Bước"
description: "Làm thế nào một hostname trở thành địa chỉ IP, và cache, recursion, TTL nằm ở đâu trong quá trình đó."
tags: ["dns", "resolution", "protocols"]
date: "2025-03-12"
lang: "vi"
---

# DNS Phân Giải Trong 6 Bước

DNS biến tên thành địa chỉ, nhưng giá trị thực sự là nó làm điều đó có thể dự đoán được, ở quy mô lớn, và có đủ cache để chạy nhanh.

![Sơ đồ luồng lưu lượng](/media/notes/protocols/network-traffic.svg)

## Đường Tra Cứu

1. Trình duyệt kiểm tra cache của chính nó.
2. Hệ điều hành kiểm tra resolver của máy.
3. Recursive resolver kiểm tra cache của nó.
4. Resolver hỏi root server.
5. Resolver hỏi TLD server.
6. Resolver hỏi authoritative server.

### Recursive và iterative

Recursive resolution nghĩa là "hãy làm hộ tôi." Iterative resolution nghĩa là "hãy chỉ cho tôi bước tiếp theo."

#### Lệnh ví dụ
dig +trace example.com
## TTL rất quan trọng

| Trường | Ý nghĩa | Vì sao quan trọng |
|------|---------|----------------|
| A record | Ánh xạ tên sang địa chỉ IPv4 | Đường đi web phổ biến |
| AAAA record | Ánh xạ tên sang địa chỉ IPv6 | Hỗ trợ dual-stack |
| TTL | Thời gian sống | Quyết định độ tươi của cache |

### Các lỗi thường gặp

- TTL quá thấp tạo thêm traffic tra cứu.
- Cache cũ có thể che mất thay đổi DNS.
- Gõ sai trong zone authoritative có thể làm hỏng toàn bộ chuỗi phía sau.

#### Kết luận

DNS trông đơn giản vì phần việc nặng đã được chia ra giữa nhiều cache và server khác nhau.
