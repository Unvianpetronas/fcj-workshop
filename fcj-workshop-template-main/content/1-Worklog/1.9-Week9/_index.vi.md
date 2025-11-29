---
title: "Nhật ký tuần 9"
date: "`r Sys.Date()`"
weight: 109
chapter: false
pre: " <b> 1.9. </b> "
---


# Báo cáo tuần 9

### Mục tiêu
* Nghiên cứu toàn diện Well-Architected Framework để áp dụng vào dự án
* Viết tài liệu về API references để chuẩn bị thực hiện dự án frontend
* Sửa lỗi liên quan đến định dạng JSON datetime trong cache
* Logic tự động hóa để thu thập dữ liệu khách hàng mỗi 10 phút
* Hotfix lỗi với client provider

### Các nhiệm vụ thực hiện trong tuần:
| Ngày | Nhiệm vụ                                                                                                                                                             | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| :--- |:---------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------|:----------------| :--- |
| 2 | - **Đánh giá kiến trúc:** <br> - Xem xét dự án theo AWS Well-Architected Framework <br> - Xác định các khoảng trống về bảo mật & độ tin cậy                         | 03/11/2025   | 03/11/2025      | AWS Well-Architected Tool |
| 3 | - **Tài liệu hóa:** <br> - Viết tài liệu tham khảo API <br> - Định nghĩa request/response schemas để chuẩn bị cho Frontend                                          | 04/11/2025   | 04/11/2025      | Internal Wiki / Swagger |
| 4 | - **Triển khai tính năng:** <br> - Logic tự động hóa để thu thập dữ liệu khách hàng <br> - Cấu hình scheduler (Cron/Lambda) cho khoảng thời gian 10 phút             | 05/11/2025   | 05/11/2025      | Python Boto3 Docs |
| 5 | - **Sửa lỗi (Cache):** <br> - Điều tra lỗi JSON serialization <br> - Sửa lỗi định dạng JSON datetime trong Redis cache                                              | 06/11/2025   | 06/11/2025      | StackOverflow / Library Docs |
| 6 | - **Bảo trì:** <br> - Chẩn đoán vấn đề kết nối Client Provider <br> - Triển khai Hotfix lên production                                                              | 07/11/2025   | 07/11/2025      | System Logs / CloudWatch |

### Thành tựu tuần 9:

* **Tối ưu hóa hạ tầng:**
  * Thực hiện đánh giá toàn diện dự án sử dụng **AWS Well-Architected Framework**.
  * Lập bản đồ các cải tiến cho khả năng phục hồi dữ liệu và tuân thủ bảo mật.

* **Tài liệu hóa & Chuyển giao Frontend:**
  * Hoàn thành thành công việc viết và hoàn thiện **Tài liệu tham khảo API**.
  * Cung cấp các endpoints rõ ràng và cấu trúc dữ liệu để hỗ trợ giai đoạn tích hợp Frontend.

* **Tự động hóa Backend:**
  * Phát triển và triển khai logic thu thập dữ liệu.
  * Thiết lập chu kỳ tự động hóa đáng tin cậy để lấy dữ liệu khách hàng mỗi **10 phút**.

* **Giải quyết các lỗi nghiêm trọng:**
  * **Cache Layer:** Giải quyết lỗi định dạng `JSON datetime`, đảm bảo quá trình serialization dữ liệu trong cache diễn ra suôn sẻ.
  * **Client Provider:** Xác định và áp dụng **hotfix** cho vấn đề kết nối client provider, khôi phục lại sự ổn định của dịch vụ.