---
title: "3. Tạo VPC-Subnet"
date: "`r Sys.Date()`"
weight: 503
chapter: false
pre: " <b> 5.3. </b> "
---

Trong phần này, bạn sẽ tạo Virtual Private Cloud (VPC) của riêng mình và định nghĩa hai subnets:

- 1 Public Subnet (có thể truy cập Internet)
- 1 Private Subnet (không có quyền truy cập Internet trực tiếp)

### 3.1 Tạo VPC

1. Mở **AWS Management Console**
2. Điều hướng đến **Services → VPC**
3. Click **Create VPC**
4. Chọn **VPC Only**
5. Nhập các thông tin chi tiết:
    - **Name tag:** `Workshop-VPC`
    - **IPv4 CIDR:** `10.0.0.0/16`
6. Giữ tất cả các cài đặt khác ở mặc định
7. Click **Create VPC**

![VPC Creation Menu](/images/5-Workshop/3-VPC-Subnets/vpc-creation.png)

---

### 3.2 Tạo Public Subnet

1. Trong menu bên trái, click **Subnets**
2. Click **Create subnet**
3. Cấu hình như sau:
    - **VPC:** `Workshop-VPC`
    - **Subnet name:** `Public-Subnet`
    - **Availability Zone:** (bất kỳ, ví dụ: `ap-southeast-1a`)
    - **IPv4 CIDR block:** `10.0.1.0/24`
4. Click **Create subnet**

![Subnet Creation Menu](/images/5-Workshop/3-VPC-Subnets/public-subnet-creation.png)

---

### 3.3 Tạo Private Subnet

1. Click **Create subnet** lần nữa
2. Điền các giá trị:
    - **VPC:** `Workshop-VPC`
    - **Subnet name:** `Private-Subnet`
    - **Availability Zone:** (có thể giống hoặc khác)
    - **IPv4 CIDR block:** `10.0.2.0/24`
3. Click **Create subnet**

> Quy trình này tương tự như phần 3.2 chỉ có một số khác biệt nhỏ. Vui lòng xem phần 3.2 để tham khảo.

---

### 3.4 Bật Auto-Assign Public IP (Chỉ cho Public Subnet)

1. Đi tới **Subnets**
2. Chọn **Public-Subnet**
3. Click **Actions → Edit subnet settings**
4. Bật:
    - ☑ **Auto-assign public IPv4**
5. Click **Save**

![Subnet Setting Menu](/images/5-Workshop/3-VPC-Subnets/public-subnet-IP-assign.png)

---

**Kết quả mong đợi:**
Bây giờ bạn đã có một VPC với hai subnets sẵn sàng sử dụng.

Các thành phần đã tạo cho đến nay:

| Resource           | Tên            | CIDR        |
|--------------------|----------------|-------------|
| VPC                | Workshop-VPC   | 10.0.0.0/16 |
| Subnet 1 (Public)  | Public-Subnet  | 10.0.1.0/24 |
| Subnet 2 (Private) | Private-Subnet | 10.0.2.0/24 |