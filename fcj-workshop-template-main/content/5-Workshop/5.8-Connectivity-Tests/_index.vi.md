---
title: "8. Kiểm tra Kết nối"
date: "`r Sys.Date()`"
weight: 508
chapter: false
pre: " <b> 5.8. </b> "
---

Bây giờ cả hai EC2 instances đã chạy, đã đến lúc xác thực hành vi mạng và xác nhận VPC đang hoạt động
đúng cách.  
Phần này xác minh ba điều:

1. Public EC2 có thể truy cập được từ máy local của bạn
2. Private EC2 chỉ có thể truy cập được từ Public EC2 (không trực tiếp từ internet)
3. Private EC2 có thể truy cập internet thông qua NAT Gateway

---

### 8.1 Kết nối tới Public EC2 Instance

1. Mở **EC2 Console** → **Instances**
2. Sao chép **Public IPv4** của **EC2-Public**
3. SSH vào instance

> Bước này phụ thuộc vào loại máy bạn đang sử dụng và có thể khác nhau giữa các công cụ khác nhau. Chúng tôi đang sử dụng
> Powershell trên PC Windows 11 cho Workshop này. Bạn có thể sử dụng các phương án thay thế khác nếu muốn.
```bash
ssh -i "Workshop-Key.pem" ec2-user@<PUBLIC_IP_HERE>
```

> Ví dụ: ssh -i "Workshop-Key.pem" ec2-user@54.218.10.102

4. Chạy lệnh và bạn sẽ thấy một cái gì đó như thế này:

![SSH into public EC2 instance](/images/5-Workshop/8-Connectivity-Tests/ssh-into-public-from-powershell.png)

> Terminal đã kết nối tới EC2-Public. Vì lý do bảo mật, chúng tôi đã làm mờ một số chi tiết.

### 8.2 SSH từ Public EC2 sang Private EC2

1. Trước khi bạn có thể thực hiện bước này, bạn phải sao chép EC2 instance key/pair của mình vào public EC2 instance **EC2-Public**

> Đây không phải là hành động được AWS khuyến nghị. Vì đây là một rủi ro bảo mật lớn khi đặt access keys lên EC2 instances.
> Tuy nhiên, vì mục đích đơn giản hóa trong việc kiểm tra kết nối của chúng ta. Bạn có thể làm điều này.

2. Từ terminal trên máy của bạn (đã đăng xuất khỏi bất kỳ EC2 instances nào). Chạy lệnh này:
```bash
scp -i "Workshop-Key.pem" Workshop-Key.pem ec2-user@<PUBLIC_IP_HERE>:/home/ec2-user/
```

> Lệnh này sao chép access key của bạn vào public EC2 instance **EC2-Public** để sử dụng sau này trong việc truy cập private EC2
> instance **EC2-Private**

3. Sau đó SSH vào **EC2-Public** như bạn đã làm trước đó ở phần 8.1 và chạy:
```bash
chmod 400 Workshop-Key.pem
```

> Lệnh này giới hạn quyền của access key đã sao chép, bảo mật nó. Cho phép bạn sử dụng nó sau này để SSH vào private
> EC2 instance **EC2-Private**

4. Sau đó chạy lệnh này:
```bash
ssh -i "Workshop-Key.pem" ec2-user@<PRIVATE_IP_HERE>
```

![SSH into private instance from public instance](/images/5-Workshop/8-Connectivity-Tests/ssh-into-private-from-public.png)

> Thành công, bạn đã SSH thành công vào private EC2 instance **EC2-Private** từ nguồn kết nối duy nhất được phép:
> Public EC2 instance **EC2-Public**

### 8.3 Kiểm tra Kết nối Internet từ Private Instance

1. Chạy lệnh này:
```bash
ping -c 3 google.com
sudo yum update -y
```

2. Hành vi mong đợi:

| Hành động       | Kết quả                         |
|-----------------|---------------------------------|
| ping google.com | Nhận được phản hồi              |
| yum update      | Packages tải xuống thành công   |

→ Nếu hoạt động, routing NAT Gateway đã được xác nhận.

![Test internet connection of private instance](/images/5-Workshop/8-Connectivity-Tests/ssh-private-test-internet.png)

> Kết nối internet thành công từ private EC2 instance **EC2-Private**

### 8.4 Xác thực Hành vi Bảo mật (Kiểm tra Quan trọng)

| Test                              | Kết quả Mong đợi |
|-----------------------------------|------------------|
| PC của bạn → Private instance SSH | Bị chặn          |
| Public EC2 → Private EC2 SSH      | Được phép        |
| Private EC2 → Internet            | Hoạt động qua NAT|

→ Điều này xác nhận public exposure = được kiểm soát, và private access = an toàn + hoạt động.

![SSH into private instance from local machine](/images/5-Workshop/8-Connectivity-Tests/ssh-into-private-from-pc.png)

> Như mong đợi. Kết nối từ máy local vào private EC2 instance **EC2-Private** không được phép. Các
> biện pháp bảo mật hoạt động như dự định.

### Kết thúc các Bài kiểm tra Kết nối