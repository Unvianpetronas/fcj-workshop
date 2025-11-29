---
title : "Kiểm tra Gateway Endpoint"
date :  "`r Sys.Date()`" 
weight : 532
chapter : false
pre : " <b> 5.3.2 </b> "
---

#### Tạo S3 bucket

1. Đi đến S3 management console
2. Trong Bucket console, chọn **Create bucket**

![8.png](/images/5-Workshop/5.3-S3-vpc/8.png)

3. Trong Create bucket console
+ Đặt tên bucket: chọn 1 tên mà không bị trùng trong phạm vi toàn cầu (gợi ý: lab\<số-lab\>\<tên-bạn\>)

![bucket-name.png](/images/5-Workshop/5.3-S3-vpc/bucket-name.png)


+ Giữ nguyên giá trị của các fields khác (default)
+ Kéo chuột xuống và chọn **Create bucket**

![10.png](/images/5-Workshop/5.3-S3-vpc/10.png)  

+ Tạo thành công S3 bucket

![11.png](../../../../static/images/5-Workshop/5.3-S3-vpc/11.png)


4. Tạo EC2 trong console 
+ Đi đến EC2 console

![14.png](/images/5-Workshop/5.3-S3-vpc/14.png)

+ Click vào lauch instance tại màn hình chính đặt tên cho instance và giữ nguyên các defaults
![15.png](/images/5-Workshop/5.3-S3-vpc/15.png)

+ Chọn keypair có sẵn hoặc tạo 1 keypair mới 
![16.png](/images/5-Workshop/5.3-S3-vpc/16.png)
+ Trong phần network chọn VPC đã tạo endoint ở phần trước
![17.png](/images/5-Workshop/5.3-S3-vpc/17.png)
+ Click vào launch instance và đợi thông báo thành công
![18.png](/images/5-Workshop/5.3-S3-vpc/18.png)
5. Tạo IAM role cho EC2 instance và attach role
+ Tìm IAM trên thanh tìm kiếm sau đó nhấn vào IAM role
+ Bấm vào create
+ Chọn vào AWS service và usecase là EC2
![19.png](/images/5-Workshop/5.3-S3-vpc/19.png)
+ Bấm next, trong thanh search bar tìm AmazonSSMManagedInstanceCore sau đó chọn và bấm next
![20.png](/images/5-Workshop/5.3-S3-vpc/20.png)
+ Đặt tên và bấm create role
![21.png](/images/5-Workshop/5.3-S3-vpc/21.png)
+ Di chuyển qua EC2 console, tại đây nhấn vào action và chọn security và chọn modify IAM role
![22.png](/images/5-Workshop/5.3-S3-vpc/22.png)
+ Chọn vào IAM role vừa tạo và click vào update IAM role
![23.png](/images/5-Workshop/5.3-S3-vpc/23.png)
#### Kết nối với EC2 bằng session manager


1. Trong AWS Management Console, gõ Systems Manager trong ô tìm kiếm và nhấn Enter:

![12.png](/images/5-Workshop/5.3-S3-vpc/12.png)

2. Từ **Systems Manager** menu, tìm **Node Management** ở thanh bên trái và chọn **Session Manager**:

![13.png](/images/5-Workshop/5.3-S3-vpc/13.png)

3. Click Start Session, và chọn EC2 instance tên **Test-Gateway-Endpoint**. 

![24.png](/images/5-Workshop/5.3-S3-vpc/24.png)
![25.png](/images/5-Workshop/5.3-S3-vpc/25.png)

Session Manager sẽ mở browser tab mới với shell prompt: sh-4.2 $

![26.png](/images/5-Workshop/5.3-S3-vpc/26.png)

Bạn đã bắt đầu phiên kết nối đến EC2 trong VPC Cloud thành công. Trong bước tiếp theo, chúng ta sẽ tạo một  S3 bucket và một tệp trong đó.
#### Create a file and upload to s3 bucket

1. Đổi về ssm-user's thư mục bằng lệnh "cd ~" 

![27.png](/images/5-Workshop/5.3-S3-vpc/27.png)

2. Tạo 1 file để kiểm tra bằng lệnh "fallocate -l 1G testfile.xyz", 1 file tên "testfile.xyz" có kích thước 1GB sẽ được tạo.

![28.png](/images/5-Workshop/5.3-S3-vpc/28.png)

3. Tải file mình vừa tạo lên S3 với lệnh "aws s3 cp testfile.xyz s3://your-bucket-name". Thay your-bucket-name bằng tên S3 bạn đã tạo.

![29.png](/images/5-Workshop/5.3-S3-vpc/29.png)

Bạn đã tải thành công tệp lên bộ chứa S3 của mình. Bây giờ bạn có thể kết thúc session.

#### Kiểm tra object trong S3 bucket

1. Đi đến S3 console.  
2. Click tên s3 bucket của bạn
3. Trong Bucket console, bạn sẽ thấy tệp bạn đã tải lên S3 bucket của mình

![30.png](/images/5-Workshop/5.3-S3-vpc/30.png)

#### Tóm tắt

Chúc mừng bạn đã hoàn thành truy cập S3 từ VPC. Trong phần này, bạn đã tạo gateway endpoint cho Amazon S3 và sử dụng AWS CLI để tải file lên. Quá trình tải lên hoạt động vì gateway endpoint cho phép giao tiếp với S3 mà không cần Internet gateway gắn vào "VPC Cloud". Điều này thể hiện chức năng của gateway endpoint như một đường dẫn an toàn đến S3 mà không cần đi qua public Internet.