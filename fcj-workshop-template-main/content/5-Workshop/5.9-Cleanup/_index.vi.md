---
title: "9. Dọn dẹp"
date: "`r Sys.Date()`"
weight: 509
chapter: false
pre: " <b> 5.9. </b> "
---

Để tránh các khoản phí không cần thiết — đặc biệt là từ **NAT Gateway** — bạn nên xóa tất cả các resources đã tạo sau khi
hoàn thành workshop.

Thực hiện các bước dưới đây theo thứ tự:

---

### 9.1 Terminate EC2 Instances

1. Mở **EC2 Console**
2. Chọn cả `EC2-Public` và `EC2-Private`
3. Click **Instance state → Terminate** (Quá trình này có thể mất một chút thời gian)
4. Xác nhận termination

![EC2 instances termination](/images/5-Workshop/9-Cleanup/ec2-instance-termination.png)

---

### 9.2 Xóa NAT Gateway

1. Đi tới **VPC Console → NAT Gateways**
2. Chọn `Workshop-NAT`
3. Click **Actions → Delete NAT Gateway** (Quá trình này có thể mất một chút thời gian)
4. Xác nhận hành động

> Đợi cho đến khi NAT Gateway hiển thị trạng thái **Deleted** trước khi tiếp tục.

![NAT Gateway termination](/images/5-Workshop/9-Cleanup/NAT-termination.png)

---

### 9.3 Release Elastic IP

Sau khi NAT Gateway đã bị xóa:

1. Đi tới **Elastic IPs**
2. Chọn địa chỉ đã được cấp phát
3. Click **Actions** → **Release Elastic IP address**
4. Xác nhận release (danh sách Elastic IP addresses trống)

---

### 9.4 Detach và Delete Internet Gateway

1. Trong **Internet Gateways**
2. Chọn `Workshop-IGW`
3. Click **Actions → Detach from VPC**
4. Sau đó **Actions → Delete Internet gateway**
5. Xác nhận deletion (danh sách Internet gateways trống)

---

### 9.5 Xóa Subnets

1. Đi tới **Subnets**
2. Xóa `Public-Subnet`
3. Xóa `Private-Subnet`
4. Xác nhận deletion (danh sách Subnets trống)

---

### 9.6 Xóa VPC

1. Trong **VPC Console**
2. Chọn `Workshop-VPC`
3. Click **Actions → Delete VPC**
4. Xác nhận deletion (danh sách VPCs trống)

---

### 9.7 Xóa Route Tables

1. Mở **Route Tables**
2. Chọn `Public-RT` → **Delete**
3. Chọn `Private-RT` → **Delete**
4. Xác nhận deletion (các route tables đã tạo không còn tồn tại)

---

### Hoàn thành Dọn dẹp