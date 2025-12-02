---
title: "Nhật ký tuần 3"
date: "`r Sys.Date()`"
weight: 103
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3:

* Kết nối và làm quen với các thành viên First Cloud Journey
* Hiểu về các dịch vụ AWS cơ bản, cách sử dụng console & CLI
* Học các khái niệm cơ bản về DevOps và CI/CD

### Các nhiệm vụ thực hiện trong tuần:

| Ngày | Nhiệm vụ                                                                                                                                                                                                          | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| ---- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ------------------ |
| 2    | - Làm quen với các thành viên FCJ <br> - Đọc và ghi nhớ nội quy, quy định của đơn vị thực tập                                                                                                                     | 22/09/2025   | 22/09/2025      |                    |
| 3    | - Tìm hiểu về AWS và các loại dịch vụ <br>&emsp; + Compute <br>&emsp; + Storage <br>&emsp; + Networking <br>&emsp; + Database <br>&emsp; + Tổng quan về các công cụ DevOps                                       | 23/09/2025   | 23/09/2025      |                    |
| 4    | - Tạo tài khoản AWS Free Tier <br> - Tìm hiểu về AWS Console & AWS CLI <br> - **Thực hành:** <br>&emsp; + Tạo tài khoản AWS <br>&emsp; + Cài đặt & cấu hình AWS CLI <br>&emsp; + Cách sử dụng AWS CLI           | 24/09/2025   | 24/09/2025      |                    |
| 5    | - Học cơ bản về EC2: <br>&emsp; + Các loại instance <br>&emsp; + AMI <br>&emsp; + EBS <br>&emsp; + Public vs Private subnets <br> - Các phương pháp kết nối SSH tới EC2 <br> - Tìm hiểu về Elastic IP            | 25/09/2025   | 25/09/2025      |                    |
| 6    | - **Thực hành:** <br>&emsp; + Khởi chạy EC2 instance <br>&emsp; + Kết nối qua SSH <br>&emsp; + Gắn EBS volume <br>&emsp; + Kết nối tới EC2 trong private subnet                                                  | 26/09/2025   | 26/09/2025      |                    |

### Thành tựu tuần 3:

**Kiến thức nền tảng AWS & Thiết lập tài khoản:**
* Hiểu AWS là gì và nắm vững các nhóm dịch vụ cơ bản: Compute, Storage, Networking và Database
* Tạo và cấu hình thành công tài khoản AWS Free Tier
* Làm quen với AWS Management Console và học cách tìm kiếm, truy cập và sử dụng các dịch vụ qua giao diện web
* Cài đặt và cấu hình AWS CLI trên máy tính, bao gồm cấu hình Access Key, Secret Key và Default Region
* Sử dụng AWS CLI để thực hiện các thao tác cơ bản như kiểm tra thông tin tài khoản, lấy danh sách regions, xem dịch vụ EC2, tạo và quản lý key pairs, và kiểm tra các dịch vụ đang chạy
* Có khả năng quản lý tài nguyên AWS song song thông qua cả giao diện web và CLI

**DevOps & CI/CD:**
* Học các khái niệm và nguyên tắc cơ bản về DevOps
* Có kinh nghiệm thực hành với Jenkins cho Continuous Integration (CI)
* Cài đặt và cấu hình thành công Jenkins trên EC2 instance

**EC2 & Networking:**
* Hoàn thành bài lab thực hành triển khai EC2 instance
* Kết nối thành công tới các EC2 instances trong private subnets
* Hiểu được sự khác biệt giữa cấu hình public và private subnet

**Database & Containerization:**
* Triển khai và cấu hình database trên EC2
* Học các khái niệm cơ bản về Docker và nguyên tắc container