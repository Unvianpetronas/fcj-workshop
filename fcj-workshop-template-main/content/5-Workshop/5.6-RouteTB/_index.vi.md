---
title: "6. Route Table"
date: "`r Sys.Date()`"
weight: 506
chapter: false
pre: " <b> 5.6. </b> "
---

Route Tables quyết định cách traffic di chuyển bên trong VPC của bạn.  
Trong phần này, bạn sẽ:

- Tạo các route tables riêng biệt cho public và private subnets
- Định tuyến traffic Internet từ public subnet → Internet Gateway
- Định tuyến traffic private subnet → NAT Gateway cho quyền truy cập outbound

---

### 6.1 Tạo Route Table cho Public Subnet

1. Trong VPC console, mở **Route Tables**
2. Click **Create route table**
3. Nhập:
    - **Name:** `Public-RT`
    - **VPC:** `Workshop-VPC`
4. Click **Create route table**

![Route Table Creation Menu](/images/5-Workshop/6-RouteTB/RT-creation.png)

---

### 6.2 Thêm Route tới Internet Gateway

1. Chọn route table **Public-RT**
2. Đi tới tab **Routes**
3. Click **Edit routes → Add route**
4. Nhập:
    - **Destination:** `0.0.0.0/0`
    - **Target:** `Internet Gateway (Workshop-IGW)`
5. Click **Save changes**

![Route Table add new route](/images/5-Workshop/6-RouteTB/RT-add-route.png)

---

### 6.3 Liên kết Public Subnet với Public Route Table

1. Vẫn ở trong trang Public-RT
2. Click **Subnet associations**
3. Trong tab **Explicit subnet associations** → **Edit subnet associations**
4. Chọn **Public-Subnet**
5. **Save**

![Route Table add Public Subnet](/images/5-Workshop/6-RouteTB/RT-add-subnet.png)

---

### 6.4 Tạo Route Table cho Private Subnet

1. Click **Create route table**
2. Nhập:
    - **Name:** `Private-RT`
    - **VPC:** `Workshop-VPC`
3. Click **Create route table**

> Quy trình này tương tự như phần 6.1 chỉ có một số khác biệt nhỏ. Vui lòng xem phần 6.1 để tham khảo.

---

### 6.5 Thêm Route tới NAT Gateway (Quyền Truy cập Outbound cho Private Subnet)

1. Chọn **Private-RT**
2. Đi tới **Routes → Edit routes**
3. Thêm:
    - **Destination:** `0.0.0.0/0`
    - **Target:** `NAT Gateway (Workshop-NAT)`
4. Save

![Route Table add new route to NAT Gateway](/images/5-Workshop/6-RouteTB/RT-add-route-NAT.png)

---

### 6.6 Liên kết Private Subnet

1. Đi tới **Subnet associations** (của route table **Private-RT**)
2. Trong tab **Explicit subnet associations** → **Edit subnet associations**
3. Chọn **Private-Subnet**
4. Save

![Route Table add Private Subnet](/images/5-Workshop/6-RouteTB/RT-add-subnet-private.png)

---

### Kiểm tra Kết quả

Bây giờ bạn sẽ thấy:

| Route Table | Subnet         | Đường dẫn Internet |
|-------------|----------------|--------------------|
| Public-RT   | Public-Subnet  | Internet Gateway   |
| Private-RT  | Private-Subnet | NAT Gateway        |

Tại thời điểm này, cơ sở hạ tầng mạng đã **hoạt động hoàn toàn**.  
Tiếp theo — chúng ta triển khai EC2 instances để kiểm tra các biện pháp bảo mật.