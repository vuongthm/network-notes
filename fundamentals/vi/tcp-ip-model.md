---
title: "Mô Hình TCP/IP Giải Thích"
description: "Một cách nhìn thực tế về ngăn xếp TCP/IP và cách từng lớp ánh xạ vào hành vi mạng."
tags: ["networking", "tcp-ip", "fundamentals"]
date: "2025-01-14"
lang: "vi"
locked: true
---

# Mô Hình TCP/IP Giải Thích

Mô hình TCP/IP là ngăn xếp thực tế mà đa số kỹ sư dùng hằng ngày. Nó ngắn hơn OSI, nhưng bám sát cách lưu lượng thực sự di chuyển hơn.

![Ngăn xếp TCP/IP](/media/notes/fundamentals/layered-stack.svg)

## Bốn Lớp

| Lớp | Nhiệm vụ | Ví dụ phổ biến |
|-----|----------|----------------|
| Application | Giao thức hướng người dùng và nội dung | HTTP, DNS, SMTP |
| Transport | Truyền giữa các tiến trình đầu cuối | TCP, UDP |
| Internet | Định địa chỉ và định tuyến logic | IP, ICMP |
| Network access | Khung dữ liệu, bit và môi trường vật lý | Ethernet, Wi-Fi |

### Gói tin đi như thế nào

Ứng dụng yêu cầu dữ liệu, transport chọn cổng, internet thêm địa chỉ nguồn và đích, rồi lớp truy cập mạng đưa frame lên đường truyền.

#### Ví dụ
curl https://example.com
## Vì Sao Quan Trọng

TCP/IP giúp bạn đi theo con đường gỡ lỗi rõ ràng hơn so với việc nói về mọi chi tiết triển khai cùng lúc.

### Mẹo hữu ích

Nếu thay đổi port hoặc protocol mà lỗi biến mất, rất có thể vấn đề nằm ở lớp transport.

## So Sánh Lớp

| Câu hỏi | Câu trả lời trong TCP/IP |
|---------|--------------------------|
| Ai sở hữu định dạng thông điệp? | Lớp application |
| Ai sở hữu việc truyền giữa hai host? | Lớp transport |
| Ai sở hữu định tuyến? | Lớp internet |
| Ai sở hữu môi trường truyền vật lý? | Lớp network access |

#### Kết luận

OSI giỏi giải thích. TCP/IP giỏi vận hành.