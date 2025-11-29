---
title: "Nhật ký tuần 11"
date: "`r Sys.Date()`"
weight: 111
chapter: false
pre: " <b> 1.11. </b> "
---



### Mục tiêu tuần 11:

* Tham gia sự kiện với chủ đề DevOps on AWS
* Nắm vững kiến thức cơ bản để thiết lập IaC (Infrastructure as Code) với Terraform
* Frontend React với trang cài đặt để gửi email hàng ngày
* Tài liệu về cách chấm điểm kiến trúc dựa trên metadata lấy từ AWS
* Cải thiện chức năng email và API, thiết lập production trên SES

### Các nhiệm vụ thực hiện trong tuần:
| Ngày | Nhiệm vụ                                                                                                                                                                  | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| :--- |:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------|:----------------| :--- |
| 2 | - **Phát triển chuyên môn:** <br> - Tham dự sự kiện "DevOps on AWS" <br> - Giao lưu với các đồng nghiệp trong ngành và ghi chú các best practices                        | 17/11/2025   | 17/11/2025      | Event Agenda / Notes |
| 3 | - **Infrastructure as Code (IaC):** <br> - Cài đặt Terraform và cấu hình AWS Provider <br> - Viết các file `.tf` cơ bản để định nghĩa các tài nguyên đơn giản (EC2/S3)   | 18/11/2025   | 18/11/2025      | Terraform Registry / HashiCorp Docs |
| 4 | - **Phát triển Frontend:** <br> - Xây dựng trang "Email Settings" trong React <br> - Triển khai UI forms để lên lịch gửi email hàng ngày                                  | 19/11/2025   | 19/11/2025      | React Docs / Material UI |
| 5 | - **Backend & Email Service:** <br> - Tái cấu trúc Email API để cải thiện hiệu suất <br> - Yêu cầu quyền truy cập production cho AWS SES và cấu hình xác minh domain      | 20/11/2025   | 20/11/2025      | AWS SES Developer Guide |
| 6 | - **Tài liệu hóa:** <br> - Viết tài liệu logic cho Architecture Scoring <br> - Định nghĩa cách AWS metadata ánh xạ tới các tiêu chí chấm điểm                             | 21/11/2025   | 21/11/2025      | Internal Wiki / AWS CloudWatch Docs |


### Thành tựu tuần 11:

* **Kiến thức DevOps:**
  * Tham gia sự kiện **DevOps on AWS**, thu được những hiểu biết sâu sắc về các pipeline CI/CD hiện đại và tự động hóa hạ tầng.

* **Infrastructure as Code (Terraform):**
  * Thiết lập thành công môi trường Terraform.
  * Hiểu các khái niệm cốt lõi bao gồm Providers, Resources, State files và quy trình `plan`/`apply`.

* **Triển khai Frontend:**
  * Hoàn thành **trang Email Settings** sử dụng React.
  * Người dùng giờ đây có thể cấu hình lịch trình gửi email hàng ngày thông qua giao diện.

* **Thiết lập AWS SES Production:**
  * Tối ưu hóa logic API gửi email.
  * Chuyển thành công AWS SES từ chế độ Sandbox sang **Production**, cho phép gửi email đến các địa chỉ chưa được xác minh.

* **Tài liệu logic chấm điểm:**
  * Hoàn thiện tài liệu chi tiết về cách hệ thống chấm điểm chất lượng kiến trúc dựa trên AWS metadata đã thu thập.