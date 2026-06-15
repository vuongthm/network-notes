---
title: "Subnetting & Địa Chỉ IP"
description: "Cấu trúc địa chỉ IP, cách hoạt động của subnet và cách tính toán — kèm ví dụ Python."
tags: ["networking", "ip", "subnetting", "cơ bản"]
date: "2024-10-01"
lang: "vi"
---

# Subnetting & Địa Chỉ IP

Một địa chỉ IP là một số 32-bit. Subnetting là kỹ thuật chia một mạng lớn thành các phân đoạn logic nhỏ hơn — kiểm soát host nào có thể kết nối với host nào, và theo cách nào.

## Tại Sao Quan Trọng

Nếu không có subnetting, mọi thiết bị trên mạng đều tranh chấp cùng một broadcast domain. Subnetting cô lập lưu lượng, cải thiện hiệu suất và cho phép kiểm soát truy cập.

## Cấu Trúc Địa Chỉ IPv4

```
192      .  168      .  1        .  10
11000000    10101000    00000001    00001010
```

Một địa chỉ IPv4 gồm bốn octet (mỗi octet 8 bit), viết theo ký hiệu thập phân có dấu chấm.

### Ký Hiệu CIDR

Subnet được biểu diễn dạng `địa_chỉ/prefix`, trong đó prefix là số bit dành cho phần **mạng**.

```
192.168.1.0/24
            ^--- 24 bit cho mạng, 8 bit cho host
```

| CIDR | Subnet Mask     | Số Host Khả Dụng |
|------|-----------------|------------------|
| /8   | 255.0.0.0       | 16.777.214       |
| /16  | 255.255.0.0     | 65.534           |
| /24  | 255.255.255.0   | 254              |
| /30  | 255.255.255.252 | 2                |

> **Lưu ý:** Số host khả dụng = 2^(32−prefix) − 2. Hai địa chỉ bị trừ là địa chỉ mạng và địa chỉ broadcast.

## Tính Toán Subnet Bằng Python

Module `ipaddress` trong thư viện chuẩn xử lý điều này rất gọn gàng.

```python
import ipaddress

net = ipaddress.ip_network("192.168.1.0/24")

print(net.network_address)   # 192.168.1.0
print(net.broadcast_address) # 192.168.1.255
print(net.netmask)           # 255.255.255.0
print(net.num_addresses)     # 256
print(list(net.hosts())[:3]) # [192.168.1.1, 192.168.1.2, 192.168.1.3]
```

### Chia Mạng Thành Các Subnet Nhỏ Hơn

```python
import ipaddress

parent = ipaddress.ip_network("10.0.0.0/8")

# Chia thành các subnet /10
subnets = list(parent.subnets(new_prefix=10))

for subnet in subnets[:4]:
    print(f"{subnet}  →  số host: {subnet.num_addresses - 2}")

# Kết quả:
# 10.0.0.0/10   →  số host: 4194302
# 10.64.0.0/10  →  số host: 4194302
# 10.128.0.0/10 →  số host: 4194302
# 10.192.0.0/10 →  số host: 4194302
```

### Kiểm Tra Host Có Thuộc Subnet Không

```python
import ipaddress

subnet = ipaddress.ip_network("192.168.10.0/24")
host   = ipaddress.ip_address("192.168.10.55")

if host in subnet:
    print(f"{host} nằm trong {subnet}")
else:
    print(f"{host} KHÔNG nằm trong {subnet}")
```

## Dải Địa Chỉ Riêng Tư (RFC 1918)

Các dải này được dành riêng và không được định tuyến trên internet công cộng.

| Dải                | CIDR           | Dùng Phổ Biến       |
|--------------------|----------------|---------------------|
| 10.0.0.0–10.255.255.255 | 10.0.0.0/8 | Doanh nghiệp lớn |
| 172.16.0.0–172.31.255.255 | 172.16.0.0/12 | Mạng vừa |
| 192.168.0.0–192.168.255.255 | 192.168.0.0/16 | Gia đình / văn phòng nhỏ |

```python
import ipaddress

addresses = ["10.5.1.1", "172.20.0.1", "8.8.8.8", "192.168.0.100"]

for addr in addresses:
    ip = ipaddress.ip_address(addr)
    print(f"{addr:20s} riêng_tư={ip.is_private}")

# 10.5.1.1             riêng_tư=True
# 172.20.0.1           riêng_tư=True
# 8.8.8.8              riêng_tư=False
# 192.168.0.100        riêng_tư=True
```

## Supernetting (Tóm Tắt Route)

Ngược lại với subnetting — kết hợp các mạng liền kề thành một prefix lớn hơn để bảng định tuyến gọn hơn.

```python
import ipaddress

nets = [
    ipaddress.ip_network("192.168.0.0/24"),
    ipaddress.ip_network("192.168.1.0/24"),
    ipaddress.ip_network("192.168.2.0/24"),
    ipaddress.ip_network("192.168.3.0/24"),
]

supernet = ipaddress.collapse_addresses(nets)

for net in supernet:
    print(net)  # 192.168.0.0/22
```

## Thực Tế Trong Công Việc

Trong môi trường thực tế, bạn hiếm khi tính toán thủ công. Nhưng hiểu được phép tính nhị phân có ích khi:

- **Debug lỗi định tuyến**: Tại sao lưu lượng lại đi sai gateway?
- **Thiết kế VPC/VLAN**: Tránh các dải địa chỉ chồng chéo giữa các môi trường
- **Đọc firewall rule**: `0.0.0.0/0` nghĩa là tất cả lưu lượng; `/32` là đúng một host

### Kiểm Tra Nhanh

Nếu hai host có cùng địa chỉ mạng sau khi áp dụng subnet mask, chúng thuộc cùng một subnet — không cần router.

```
Host A:  192.168.1.10  &  255.255.255.0  =  192.168.1.0  ✓
Host B:  192.168.1.99  &  255.255.255.0  =  192.168.1.0  ✓  → cùng subnet
Host C:  192.168.2.5   &  255.255.255.0  =  192.168.2.0  ✗  → khác subnet, cần router
```
