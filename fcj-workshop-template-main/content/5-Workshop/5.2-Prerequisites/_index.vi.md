---
title: "2. Yêu cầu Trước khi Bắt đầu"
date: "`r Sys.Date()`"
weight: 502
chapter: false
pre: " <b> 5.2. </b> "
---


Trước khi bắt đầu workshop, hãy đảm bảo bạn có quyền truy cập vào những thứ sau:

### Tài khoản & Quyền Truy cập

- Một **Tài khoản AWS**
- **IAM User** với quyền đối với:
    - Amazon VPC
    - Amazon EC2
    - Elastic IP
    - NAT Gateway
    - Internet Gateway
- Policy được đề xuất: **AdministratorAccess** (chỉ cho mục đích lab)

> Nếu bạn đang sử dụng IAM user, hãy đảm bảo bạn có access keys hoặc thông tin đăng nhập console.

### Thông báo về Chi phí

Workshop này bao gồm việc tạo **NAT Gateway**, đây là một resource tính phí.
Để giảm thiểu chi phí, bạn nên xóa tất cả các resources đã tạo trong bước dọn dẹp.

_chi phí dự kiến cho workshop: ~ vài USD nếu resources được dọn dẹp trong cùng ngày_

### Công cụ Yêu cầu

| Công cụ                       | Mục đích sử dụng                                               |
|-------------------------------|----------------------------------------------------------------|
| AWS Management Console        | Giao diện chính để xây dựng VPC, Subnets, Route Tables         |
| SSH client (PuTTY / Terminal) | Sử dụng sau này để kết nối tới EC2 instances                   |
| Key Pair                      | Cần thiết để đăng nhập EC2 (bạn sẽ tạo trong quá trình workshop) |

### Lựa chọn Region

Chúng tôi khuyến nghị sử dụng region gần bạn nhất để có độ trễ thấp hơn.  
Ví dụ: **ap-southeast-1 (Singapore)** cho người dùng Việt Nam.

> Bạn phải giữ nguyên region cho tất cả các bước trong workshop này.