---
title: "Worklog Tuần 1"
date: "`r Sys.Date()`"
weight: 101
chapter: false
pre: " <b> 1.1. </b> "
---



### Mục tiêu tuần 1:

* Làm quen với mọi người tại FCJ, đăng ký lên văn phòng để gặp gỡ cũng như chia sẽ kiến thức với các bạn.
* Hiểu dịch vụ điện toán đám mây cơ bản, cách dùng linux.
* Tìm hiểu về VPC, cũng như các dịch vụ có trong VPC.
* tìm hiểu các cách triển khai VPC trong doanh nghiệp cũng như thực tế,

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                                                                                                     | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                             |
|-----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|-----------------|----------------------------------------------------------------------------|
| 2   | - Làm quen với các thành viên, đọc và lưu ý các nội quy, quy định tại đơn vị thực tập, cũng như xem qua các hướng dẫn như đăng ký lên văn phòng,hướng dẫn viết blog, worklog,...                                                                                              | 7/09/2025    | 7/09/2025       |
| 2   | - Tìm hiểu AWS cung cấp những dịch vụ như thế nào <br> - Route 53  <br> - CloudInformation <br> - S3 <br> - EC2 <br> - Database <br>...                                                                                                                                       | 7/09/2025    | 7/08/2025       | https://aws.amazon.com/vi/                                                 |
| 3   | - Tạo AWS Free Tier account <br> - Xem youtube về First Cloud Journry <br> <br> - Bắt đầu hành trình lên mây cùng với AWS <br> - **Thực hành:** <br>&emsp; + Tạo AWS account <br>&emsp; + Cài usage buget dành cho AWS <br> &emsp; + Các gói hỗ trợ và manage support request | 8/09/2025    | 8/09/2025       | <https://000001.awsstudygroup.com/vi/> |
| 4   | - Tìm hiểu VPC cơ bản: <br>&emsp; + subnet <br>&emsp; + route table <br>&emsp; + scurity group & Net ACLs <br>&emsp; + ... <br> - Peering & Transit gateway <br> - Tìm hiểu Elastic IP   <br>                                                                                 | 9/08/2025    | 9/08/2025       | <https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i/>                                  |
| 5   | - Tìm hiểu EC2 cơ bản: <br>&emsp; + Instance  <br>&emsp; + AMI <br>&emsp; + keypair  <br>&emsp; + ... <br> - cách truy cập vào máy ảo EC2 trong public subnet <br> - Tìm hiểu về saving plan   <br>                                                                           | 10/08/2025   | 10/08/2025      | <https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i>                                  |
| 6   | - **Thực hành:** <br>&emsp; + Tạo EC2 instance <br>&emsp; + Kết nối SSH <br>&emsp;  + cài đặt thử jenkin trên EC2 và config Scurity group để truy cập <br>&emsp;                                                                                                              | 11/08/2025   | 12/08/2025      | <https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i>                                  |
| 7   | - **Thực hành:** <br>&emsp; + Tạo VPC <br>&emsp; + Tạo NAT gate way  <br>&emsp;  + kết nối thử các VPC ở các region khác nhay bằng peering và Transit gateway                                                                                                                 | 13/08/2025   | 14/08/2025      | <https://000003.awsstudygroup.com/>                                  |


### Kết quả đạt được tuần 1:

## 1. Định hướng và làm quen 
- Làm quen với team, nội quy đơn vị thực tập
- Nắm quy trình đăng ký văn phòng, viết blog, worklog
- Được định hướng lộ trình học AWS

## 2. Tổng quan AWS 
- **Hiểu AWS:** Nền tảng cloud computing với các dịch vụ Compute, Storage, Networking, Database
- **Nắm các service chính:** EC2, S3, VPC, RDS, CloudFormation
- Hiểu mô hình "pay-as-you-use"

## 3. Thiết lập AWS Account 
- Tạo thành công AWS Free Tier account
- Cấu hình MFA và IAM users
- Thiết lập Usage Budget và cảnh báo chi phí


## 4. VPC và Networking 
- **Hiểu VPC:** Virtual Private Cloud và vai trò trong AWS
- **Các thành phần:** Subnet, Route Table, Security Group, Network ACLs
- **Kết nối:** VPC Peering, Transit Gateway, Elastic IP
- **Tạo custom VPC:** CIDR, public/private subnet, route table
- **NAT Gateway:** Cho private subnet truy cập internet
- **VPC Peering:** Kết nối 2 VPC cùng region
- **Transit Gateway:** Quản lý kết nối nhiều VPC

## 5. EC2 Fundamentals 
- **EC2 basics:** Instance types, AMI, Key Pair
- **Storage:** EBS volumes
- **Cost optimization:** Savings Plans

## 8. AWS CLI
- Cài đặt và cấu hình AWS CLI với Access Key/Secret Key

## 9. Tổng kết
- **Lý thuyết:** Nắm vững AWS concepts cơ bản
- **Thực hành:** Tạo và quản lý tài nguyên AWS
- **Tools:** Sử dụng thành thạo AWS Console và CLI
- **Network:** Hiểu kiến trúc mạng AWS và security

