---
title: "Tổng quan"
date: "`r Sys.Date()`"
weight: 501
chapter: false
pre: " <b> 5.1. </b> "
---


# Xây dựng AWS VPC Cơ bản với Public & Private Subnets

Trong workshop này, bạn sẽ xây dựng một nền tảng mạng đơn giản trong AWS sử dụng **Amazon VPC (Virtual Private Cloud)**.
Mục tiêu là giúp người mới bắt đầu hiểu cách mạng hoạt động trên cloud, và cách các thành phần khác nhau kết nối
với nhau để tạo thành một môi trường an toàn, được kiểm soát.

Bạn sẽ tạo một VPC với hai subnets:

+ **Public Subnet** – cho phép các resources (như EC2 instances) truy cập Internet trực tiếp.
+ **Private Subnet** – được cách ly khỏi Internet công cộng theo mặc định để bảo mật tốt hơn.

Để cho phép private subnet tải packages và truy cập các dịch vụ online mà không bị lộ ra công khai, bạn sẽ
cấu hình:

+ **Internet Gateway (IGW)** – cho phép traffic của public subnet truyền đến/từ Internet.
+ **NAT Gateway** – cho phép các instances trong private subnet truy cập Internet *một cách gián tiếp*, mà không cần
  gán public IP.

Khi kết thúc, bạn sẽ khởi động hai EC2 instances — một instance trong mỗi subnet — và kiểm tra kết nối giữa chúng trong khi
quan sát vai trò của routing và gateways.

Workshop thực hành này dạy các kiến thức nền tảng về mạng AWS thiết yếu thông qua các bước hướng dẫn và điều hướng console.
Hoàn hảo cho người mới bắt đầu muốn xây dựng sự tự tin khi làm việc với VPCs.