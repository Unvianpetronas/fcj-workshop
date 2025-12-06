---
title: "4. Internet Gateway"
date: "`r Sys.Date()`"
weight: 504
chapter: false
pre: " <b> 5.4. </b> "
---

Internet Gateway (IGW) cho phép các resources bên trong **Public Subnet** của bạn gửi và nhận traffic từ Internet.
Không có nó, ngay cả một subnet với public IP vẫn sẽ bị cách ly.

### 4.1 Tạo Internet Gateway

1. Trong AWS Console, mở dịch vụ **VPC**
2. Ở panel bên trái, chọn **Internet Gateways**
3. Click **Create internet gateway**
4. Điền vào:
    - **Name tag:** `Workshop-IGW`
5. Click **Create internet gateway**

![Internet Gateway Creation Menu](/images/5-Workshop/4-IG/IG-creation.png)

---

### 4.2 Gắn Internet Gateway vào VPC

1. Sau khi IGW được tạo, click **Actions → Attach to VPC**
2. Chọn **Workshop-VPC**
3. Click **Attach internet gateway**

![Internet Gateway Attach Menu](/images/5-Workshop/4-IG/IG-attach-vpc.png)

---

### Kiểm tra Kết quả

Bây giờ bạn sẽ thấy:

| Resource     | Trạng thái               |
|--------------|--------------------------|
| Workshop-IGW | Attached to Workshop-VPC |

![Internet Gateway List](/images/5-Workshop/4-IG/IG-list.png)