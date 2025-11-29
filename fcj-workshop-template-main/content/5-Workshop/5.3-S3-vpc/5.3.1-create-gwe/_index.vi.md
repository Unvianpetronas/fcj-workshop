---
title : "Tạo một Gateway Endpoint"
date :  "`r Sys.Date()`" 
weight : 531
chapter : false
pre : " <b> 5.3.1 </b> "
---

1. Mở [Amazon VPC console](https://us-east-1.console.aws.amazon.com/vpc/home?region=us-east-1#Home:)

2. Trong thanh điều hướng, chọn **Endpoints**, click **Create Endpoint**:
![1.png](/images/5-Workshop/5.3-S3-vpc/1.png)

3. Trong Create endpoint console:
+ Đặt tên cho endpoint: s3-gwe
+ Trong service category, chọn **aws services**
![2.png](/images/5-Workshop/5.3-S3-vpc/2.png)


+ Trong **Services**, gõ "s3" trong hộp tìm kiếm và chọn dịch vụ với loại **gateway**
+ 
![3.png](/images/5-Workshop/5.3-S3-vpc/3.png)


+ Đối với VPC, chọn **VPC Cloud** từ drop-down menu.
+ Đối với Route tables, chọn bảng định tuyến mà đã liên kết với 2 subnets (lưu ý: đây không phải là bảng định tuyến chính cho VPC mà là bảng định tuyến thứ hai do CloudFormation tạo).

![4.png](/images/5-Workshop/5.3-S3-vpc/4.png)

+ Đối với Policy, để tùy chọn mặc định là Full access để cho phép toàn quyền truy cập vào dịch vụ. Bạn sẽ triển khai VPC endpoint policy trong phần sau để chứng minh việc hạn chế quyền truy cập vào S3 bucket dựa trên các policies.
![5.png](/images/5-Workshop/5.3-S3-vpc/5.png)


+ Không thêm tag vào VPC endpoint. Click Create endpoint.
+ 
![6.png](/images/5-Workshop/5.3-S3-vpc/6.png)

+  click x sau khi nhận được thông báo tạo thành công.

![7.png](/images/5-Workshop/5.3-S3-vpc/7.png)
