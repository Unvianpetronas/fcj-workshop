---
title: "Nhật ký tuần 7"
date: "`r Sys.Date()`"
weight: 107
chapter: false
pre: " <b> 1.7. </b> "
---


### Mục tiêu tuần 7:

* Sửa chữa CI/CD pipelines, tạo tất cả các service cần thiết
* Script để cài đặt trên máy ảo
* Ôn tập các lab trước đó trong module 3, 4 và kiến thức liên quan
* Coding route

### Các nhiệm vụ thực hiện trong tuần:

| Ngày | Nhiệm vụ                                                                                                                                                      | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| ---- |---------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|-----------------|-------------------|
| 2    | - Coding route EC2, GuardDuty, CloudWatch,...                                                                                                                 | 20/10/2025   | 20/10/2025      |                   |
| 3    | - Phân tích các lỗi hiện tại của CI/CD pipeline<br>- Xác định các service cần thiết (build, test, deploy)<br>- Tạo file cấu hình cho các service             | 21/10/2025   | 21/10/2025      |                   |
| 4    | - Thiết lập các CI/CD services trong pipeline<br>- Cấu hình các giai đoạn build và deployment<br>- Kiểm tra quá trình thực thi pipeline                      | 22/10/2025   | 22/10/2025      |                   |
| 5    | - Viết shell script để cài đặt trên VM<br>- Bao gồm dependencies, thiết lập môi trường và cài đặt service<br>- Kiểm tra script trên VM sạch                  | 23/10/2025   | 23/10/2025      |                   |
| 6    | - Ôn tập các lab Module 3 và ôn tập các lab Module 4                                                                                                         | 24/10/2025   | 24/10/2025      |                   |

### Thành tựu tuần 7:

* **Sửa chữa thành công CI/CD pipelines** bao gồm:
  * Xác định và giải quyết các điểm lỗi trong pipeline
  * Tạo các service cần thiết: build service, test service, deployment service
  * Cấu hình tự động triggers và notifications
  * Thiết lập quản lý biến môi trường phù hợp

* **Phát triển script cài đặt VM** với các tính năng:
  * Tự động cài đặt dependencies
  * Thiết lập cấu hình môi trường
  * Cài đặt và khởi động service
  * Cơ chế xử lý lỗi và logging
  * Kiểm tra thành công trên máy ảo sạch

* **Triển khai routing cho ứng dụng** bằng cách:
  * Coding các RESTful API routes (GET, POST, PUT, DELETE)
  * Thêm validation và xử lý lỗi cho routes
  * Tích hợp routes với database layer
  * Kiểm tra thành công tất cả các endpoints

* **Coding integration với AWS services:**
  * Tạo routes cho EC2 service để quản lý instances
  * Tích hợp GuardDuty để giám sát bảo mật
  * Kết nối CloudWatch để thu thập metrics và logs

* **Ôn tập và củng cố kiến thức:**
  * Ôn lại các lab thực hành Module 3 về networking và security
  * Ôn tập Module 4 về storage và database services
  * Củng cố kiến thức liên quan để áp dụng vào dự án