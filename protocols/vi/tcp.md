---
title: "TCP Ba Bước Bắt Tay Giải Thích"
description: "Hướng dẫn rõ ràng về cách TCP thiết lập kết nối — SYN, SYN-ACK, ACK — và tại sao cơ chế này quan trọng cho độ tin cậy."
tags: ["networking", "tcp", "giao thức"]
date: "2024-10-20"
lang: "vi"
---

# TCP Ba Bước Bắt Tay Giải Thích

TCP (Transmission Control Protocol) là xương sống của giao tiếp internet đáng tin cậy. Trước khi bất kỳ dữ liệu nào được truyền, TCP thực hiện ba bước bắt tay để thiết lập kết nối.

![Sơ đồ luồng lưu lượng](/media/notes/protocols/network-traffic.svg)

## Bước 1: SYN

Client gửi gói SYN (đồng bộ hóa) đến server. Gói này chứa một Initial Sequence Number (ISN) được chọn ngẫu nhiên.
Client → Server: SYN (seq=x)

## Bước 2: SYN-ACK

Server phản hồi bằng gói SYN-ACK. Gói này xác nhận số thứ tự của client và gửi ISN của server.
Server → Client: SYN-ACK (seq=y, ack=x+1)

## Bước 3: ACK

Client hoàn thành quá trình bắt tay bằng cách xác nhận số thứ tự của server.
Client → Server: ACK (ack=y+1)

## Tại Sao Cần Ba Bước?

Hai bước chỉ xác nhận được một kênh một chiều. Ba bước xác nhận cả hai phía đều có thể gửi và nhận — thiết lập kênh song công mà TCP yêu cầu.

## TCP vs UDP

| Tính năng | TCP | UDP |
|----------|-----|-----|
| Kết nối | Có hướng kết nối | Không hướng kết nối |
| Độ tin cậy | Giao hàng đảm bảo | Chỉ cố gắng hết sức |
| Thứ tự | Luôn theo thứ tự | Không đảm bảo thứ tự |
| Tốc độ | Chậm hơn do có overhead | Nhanh hơn do ít overhead |
| Trường hợp dùng | HTTP, email, file | Video streaming, gaming, DNS |

### Khi nào nên chọn TCP

Hãy dùng TCP khi thứ tự và độ tin cậy quan trọng hơn độ trễ thuần túy.

#### Dấu hiệu lỗi quen thuộc

Nếu dịch vụ vẫn truy cập được nhưng dữ liệu bị lỗi hoặc không đầy đủ, vấn đề thường không nằm ở bước bắt tay mà nằm ở cách xử lý dữ liệu sau khi kết nối đã được thiết lập.