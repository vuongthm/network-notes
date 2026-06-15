---
title: "Nguyên Tắc Định Tuyến"
description: "Cách chính sách định tuyến quyết định gói tin đi đâu khi có nhiều hơn một đường đi."
tags: ["routing", "policy", "fundamentals"]
date: "2025-02-14"
lang: "vi"
---

# Nguyên Tắc Định Tuyến

Chính sách định tuyến giữ cho mạng không trở thành một đống đường đi ngẫu nhiên. Nó quyết định đường nào thắng, đường nào chờ, và đường nào không bao giờ nên dùng.

![Ngăn xếp mạng nhiều lớp](/media/notes/fundamentals/layered-stack.svg)

## Cây Quyết Định

Khi có nhiều route, router sẽ so sánh theo mức ưu tiên, độ cụ thể, và chi phí.

### Longest prefix match

Route cụ thể hơn thường sẽ thắng.

#### Ví dụ
10.10.0.0/16
10.10.20.0/24

## Bảng Chính Sách

| Tín hiệu | Tác động đến | Kết quả thường thấy |
|-------|--------------|---------------------|
| Độ dài prefix | Mức cụ thể | Mạng nhỏ hơn thắng |
| Local preference | Chọn đường đi ra | Giá trị cao hơn được ưu tiên |
| MED | Gợi ý cho neighbor | Giá trị thấp hơn có thể được ưu tiên |
| AS path | Chất lượng đường Internet | Đường ngắn hơn thường thắng |

### Quy tắc vận hành

Chính sách nên làm cho đường đi phổ biến trở nên nhàm chán và đường đi đặc biệt trở nên rõ ràng.

#### Mẹo gỡ lỗi

Nếu lưu lượng đi sai đường, hãy xem chính sách trước khi xem gói tin.