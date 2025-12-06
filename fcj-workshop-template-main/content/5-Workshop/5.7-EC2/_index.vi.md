---
title: "7. EC2"
date: "`r Sys.Date()`"
weight: 507
chapter: false
pre: " <b> 5.7. </b> "
---

Bây giờ bạn sẽ triển khai hai EC2 instances:

| Instance    | Subnet         | Quyền truy cập                                    |
|-------------|----------------|---------------------------------------------------|
| EC2-Public  | Public-Subnet  | SSH trực tiếp từ Internet                         |
| EC2-Private | Private-Subnet | Không có public IP — chỉ truy cập được qua Public EC2 |

Cả hai sẽ sử dụng cùng AMI và instance type để đơn giản hóa.

---

### 7.1 Tạo Key Pair

1. Đi tới **EC2 Console**
2. Panel bên trái → **Key Pairs**
3. Click **Create key pair**
4. Cài đặt:
    - **Name:** `Workshop-Key`
    - **Type:** RSA
    - **Format:** `.pem` (Linux/Mac) hoặc `.ppk` (Windows PuTTY)
5. Tải xuống và lưu trữ an toàn

![Key Pair Creation Menu](/images/5-Workshop/7-EC2/ec2-keypair-creation.png)

> _Key này sẽ được sử dụng sau này cho SSH._

---

### 7.2 Khởi chạy Public EC2 Instance

1. Trong EC2 Console → Click **Instances** → Click **Launch instances**
2. Name: `EC2-Public`
3. Chọn AMI:
    - **Amazon Linux**
4. Instance type: **t3.micro** (Đủ điều kiện Free tier)
5. Chọn key pair hiện có: `Workshop-Key`
6. Network settings (Click **Edit** để hiển thị menu):
    - **VPC:** Workshop-VPC
    - **Subnet:** Public-Subnet
    - **Auto-assign Public IP:** Enabled
7. Cấu hình security group:
    - New SG name: `Public-EC2-SG`
    - Inbound rule:
        - Type: **SSH**
        - Source Type: `Custom`
        - Source: `0.0.0.0/0`
8. Để tất cả các cài đặt khác ở mặc định.
9. Launch instance

![EC2 Instance Launch Menu](/images/5-Workshop/7-EC2/ec2-instance-creation.png)

---

### 7.3 Khởi chạy Private EC2 Instance

1. Trong EC2 Console → Click **Instances** → Click **Launch instances** lần nữa
2. Name: `EC2-Private`
3. AMI + instance type = giống như public EC2
4. Sử dụng key pair: `Workshop-Key`
5. Network settings:
    - **VPC:** Workshop-VPC
    - **Subnet:** Private-Subnet
    - **Auto-assign Public IP:** Disabled
6. Security Group:
    - Name: `Private-EC2-SG`
    - Type: **SSH**
    - Source Type: `Custom`
    - Source: `Public-EC2-SG` **(không phải Internet)**

📸 _Screenshot ở đây: Cấu hình Private EC2 với No Public IP_
![EC2 Private Instance Launch Menu](/images/5-Workshop/7-EC2/ec2-instance-private-creation.png)

---

### 7.4 Xác minh Instances

| Instance    | IP              | Kết quả mong đợi              |
|-------------|-----------------|-------------------------------|
| EC2-Public  | Có IPv4 Public  | Có thể SSH từ máy của bạn     |
| EC2-Private | Không có Public IP | Chỉ có thể truy cập qua Public EC2 |

![EC2 Instances List](/images/5-Workshop/7-EC2/ec2-instance-list.png)