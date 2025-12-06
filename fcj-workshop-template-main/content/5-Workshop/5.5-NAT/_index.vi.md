---
title: "5. NAT Gateway"
date: "`r Sys.Date()`"
weight: 505
chapter: false
pre: " <b> 5.5. </b> "
---

NAT Gateway cho phép các instances trong **Private Subnet** truy cập Internet
_mà không bị lộ ra công khai_. Đây là cách EC2 private có thể `yum install`, tải xuống updates, v.v.

### 5.1 Cấp phát Elastic IP

1. Trong menu VPC bên trái, chọn **Elastic IPs**
2. Click **Allocate Elastic IP address**
3. Chọn các cài đặt mặc định
4. Click **Allocate**

![Allocate Elastic IP window](/images/5-Workshop/5-NAT/elastic-IP-allocate.png)

---

### 5.2 Tạo NAT Gateway

1. Trong panel VPC bên trái, đi tới **NAT gateways**
2. Click **Create NAT gateway**
3. Cấu hình:

| Trường                | Giá trị                          |
|-----------------------|----------------------------------|
| Name                  | `Workshop-NAT`                   |
| Availability mode     | Zonal                            |
| Subnet                | `Public-Subnet`                  |
| Connectivity type     | Public                           |
| Elastic IP allocation | Chọn EIP bạn vừa tạo             |

4. Click **Create NAT gateway**

![Create NAT Gateway screen with selected subnet + EIP](/images/5-Workshop/5-NAT/NAT-creation.png)

---

### 5.3 Đợi NAT Gateway chuyển sang trạng thái "Available"

- Trạng thái bắt đầu là **Pending**
- Đợi cho đến khi nó chuyển sang **Available**

![NAT Gateway state](/images/5-Workshop/5-NAT/NAT-state.png)

---

### Kiểm tra Kết quả

| Resource                     | Trạng thái |
|------------------------------|------------|
| NAT Gateway (`Workshop-NAT`) | Available  |
| Elastic IP                   | Associated |

Bây giờ bạn đã sẵn sàng để định tuyến traffic của private subnet thông qua NAT.