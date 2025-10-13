---
title: "Proposal"
date: "`r Sys.Date()`"
weight: 200
chapter: false
pre: " <b> 2. </b> "
---

# AWS Cloud Health Dashboard
**Nền tảng Giám sát Hạ tầng AWS Chuyên nghiệp cho Nhiều Khách hàng**

---

## 1. Tóm tắt Điều hành

**AWS Cloud Health Dashboard** là một **nền tảng SaaS đa khách hàng (multi-tenant)** cho phép doanh nghiệp giám sát và tối ưu hóa hạ tầng AWS cho nhiều khách hàng từ một hệ thống tập trung duy nhất. Nền tảng này thể hiện kiến trúc cấp doanh nghiệp trong khi vẫn duy trì mô hình vận hành tiết kiệm chi phí.

**Điểm nổi bật chính:**
- **Kiến trúc đa khách hàng**: Giám sát 10-50+ tài khoản AWS của khách hàng từ một nền tảng
- **Chi phí nền tảng**: $10-20/tháng (hỗ trợ không giới hạn khách hàng với chi phí tăng thêm tối thiểu)
- **Chi phí mỗi khách hàng**: ~$0.50-1.00/tháng cho lưu trữ dữ liệu
- **Tính năng chuyên nghiệp**: Xác thực email, mã hóa thông tin đăng nhập, cảnh báo tự động, giám sát thời gian thực
- **5 bảng DynamoDB**: Mô hình dữ liệu được tối ưu hóa với cách ly khách hàng
- **Hệ thống worker nền**: Thu thập dữ liệu tự động cho tất cả khách hàng
- **Hệ thống thông báo email**: Tích hợp AWS SES cho cảnh báo quan trọng và xác thực

**Công nghệ sử dụng:** FastAPI (Python) + React + DynamoDB + Redis + AWS SES + EC2 t3.micro

---

## 2. Phát biểu Vấn đề

**Thách thức Hiện tại:**

Các doanh nghiệp quản lý hạ tầng AWS cho nhiều khách hàng đang đối mặt với:
- **Không có giám sát tập trung**: Phải đăng nhập vào từng console AWS của khách hàng riêng biệt
- **Cảnh báo bảo mật phân tán**: Các phát hiện quan trọng từ GuardDuty bị bỏ lỡ
- **Thiếu khả năng hiển thị chi phí**: Khó khăn trong việc theo dõi và tối ưu chi phí giữa các khách hàng
- **Quản lý thông tin đăng nhập thủ công**: Lưu trữ AWS access keys không an toàn
- **Không có thông báo chủ động**: Bỏ lỡ các sự kiện quan trọng theo thời gian thực
- **Giám sát tốn thời gian**: 30+ phút mỗi ngày cho mỗi khách hàng

**Giải pháp của chúng tôi:**

Cloud Health Dashboard cung cấp một **nền tảng duy nhất để giám sát tất cả khách hàng** với:

**Kiến trúc Đa khách hàng**
- Một nền tảng giám sát 10-50+ tài khoản AWS của khách hàng
- Lưu trữ thông tin đăng nhập AWS được mã hóa (mã hóa Fernet)
- Cách ly dữ liệu hoàn toàn giữa các khách hàng
- Quản lý worker tự động cho mỗi khách hàng

**Giám sát Tập trung**
- Dashboard duy nhất cho tất cả khách hàng
- Tình trạng hạ tầng thời gian thực
- Lưu trữ dữ liệu lịch sử (30-365 ngày)
- 15+ dịch vụ AWS được giám sát

**Hệ thống Thông báo Email**
- Xác thực email với token có thời hạn
- Cảnh báo GuardDuty quan trọng qua AWS SES
- Mẫu email HTML đẹp mắt
- Tùy chọn thông báo có thể tùy chỉnh
- Khách hàng có thể cập nhật email trong Cài đặt

**Phân tích & Tối ưu Chi phí**
- Theo dõi chi phí theo từng khách hàng
- Xu hướng lịch sử và dự báo
- Đề xuất gốc từ AWS
- Cảnh báo ngân sách

**Bảo mật & Tuân thủ**
- Phát hiện mối đe dọa GuardDuty
- Tích hợp Security Hub
- Lọc theo mức độ nghiêm trọng
- Cảnh báo email thời gian thực cho các phát hiện quan trọng

**Đề xuất Dựa trên Quy tắc**
- Đề xuất tối ưu chi phí
- Cải thiện hiệu suất
- Tăng cường bảo mật
- Ưu tiên dựa trên tác động

**ROI & Lợi ích:**

Cho Người vận hành Nền tảng:
- Giám sát 20 khách hàng chỉ với $15-20/tháng tổng cộng
- Thu thập dữ liệu tự động (tiết kiệm 10+ giờ/tuần)
- Dự án portfolio chuyên nghiệp
- Thể hiện kỹ năng kiến trúc doanh nghiệp

Cho Mỗi Khách hàng:
- Tiết kiệm chi phí tiềm năng 15-25% thông qua tối ưu hóa
- Cảnh báo bảo mật quan trọng ngay lập tức qua email
- Hiển thị dữ liệu lịch sử
- Dashboard duy nhất thay vì nhiều console AWS

---

## 3. Kiến trúc Giải pháp

### **Tổng quan Kiến trúc Đa khách hàng**

![diagram-export-10-13-2025-11_02_08-AM.png](/images/2-Proposal/diagram-export-10-13-2025-11_02_08-AM.png)

### **Sơ đồ Mạng**
![cloud_dashboard.drawio.png](/images/2-Proposal/cloud_dashboard.drawio.png)

### **Luồng Dữ liệu**

```
1. ĐĂNG KÝ KHÁCH HÀNG
   Khách hàng → API → Mã hóa AWS Keys → Bảng DynamoDB Clients
   → Gửi Email Xác thực (AWS SES)
   
2. XÁC THỰC EMAIL
   Khách hàng → Click Link → Xác minh Token → Đánh dấu Email đã Xác thực
   
3. WORKER MANAGER
   Mỗi 5 phút → Kiểm tra khách hàng mới → Khởi động worker cho mỗi khách hàng
   
4. THU THẬP DỮ LIỆU (mỗi khách hàng)
   Worker → API AWS của Khách hàng → Thu thập metrics → Lưu vào DynamoDB (với client_id)
   → Nếu phát hiện quan trọng → Gửi cảnh báo email (AWS SES)
   
5. HIỂN THỊ DASHBOARD
   Khách hàng đăng nhập → API (lọc theo client_id) → Trả về chỉ dữ liệu của họ
```

### **Các Thành phần Cốt lõi**

**EC2 t3.micro Instance (Máy chủ Đơn):**
- **Nginx**: Reverse proxy, SSL termination
- **FastAPI**: RESTful API, xác thực, tích hợp AWS
- **React**: Ứng dụng dashboard single-page
- **Redis**: Lớp caching (TTL 5 phút)
- **Worker Manager**: Điều phối công việc nền đa khách hàng
- **CloudWatch Agent**: Giám sát nền tảng

**5 Bảng DynamoDB (Đa khách hàng):**

1. **CloudHealthClients**
   - Lưu trữ tài khoản khách hàng với thông tin đăng nhập AWS được mã hóa
   - Địa chỉ email và trạng thái xác thực
   - Tùy chọn thông báo
   - Metadata khách hàng

2. **CloudHealthMetrics**
   - Metrics chuỗi thời gian từ CloudWatch
   - Phân vùng theo client_id
   - Lưu trữ 30 ngày (TTL)

3. **CloudHealthCosts**
   - Dữ liệu chi phí từ Cost Explorer
   - Theo dõi chi phí theo khách hàng
   - Lưu trữ 365 ngày

4. **SecurityFindings**
   - Phát hiện từ GuardDuty và Security Hub
   - Cảnh báo bảo mật theo khách hàng
   - Lưu trữ 90 ngày

5. **Recommendations**
   - Đề xuất về chi phí, hiệu suất, bảo mật
   - Ưu tiên dựa trên tác động
   - Lưu trữ 180 ngày

**Các Dịch vụ AWS Sử dụng:**
- **EC2**: Tính toán (t3.micro, Free Tier)
- **DynamoDB**: Lưu trữ dữ liệu đa khách hàng
- **AWS SES**: Xác thực email và cảnh báo quan trọng
- **CloudWatch**: Thu thập metrics
- **Cost Explorer**: Phân tích chi phí
- **GuardDuty**: Phát hiện mối đe dọa (tùy chọn)
- **Security Hub**: Tổng hợp phát hiện bảo mật (tùy chọn)
- **S3**: Lưu trữ backup

---

## 4. Tính năng Chính

### **Quản lý Đa khách hàng**
- Đăng ký khách hàng tự phục vụ với xác thực AWS key
- Lưu trữ thông tin đăng nhập được mã hóa (mã hóa Fernet)
- Tự động tạo worker cho mỗi khách hàng
- Cách ly dữ liệu hoàn toàn
- Truy cập dashboard theo khách hàng
- Giám sát tình trạng worker

### **Hệ thống Thông báo Email**
- Xác thực email với token bảo mật (hết hạn 24h)
- Cảnh báo GuardDuty quan trọng qua AWS SES
- Tùy chọn thông báo có thể tùy chỉnh:
   - Cảnh báo bảo mật quan trọng
   - Cảnh báo cảnh cáo
   - Mẹo tối ưu chi phí
   - Tóm tắt hạ tầng hàng ngày
- Chỉnh sửa email trong trang Cài đặt
- Tùy chọn gửi lại email xác thực

### **Giám sát Hạ tầng**
- Metrics thời gian thực từ 15+ dịch vụ AWS
- Lưu trữ dữ liệu lịch sử (30+ ngày)
- Giám sát EC2, S3, RDS, Lambda
- Metrics CloudWatch tùy chỉnh
- Dashboard tình trạng dịch vụ

### **Tối ưu Chi phí**
- Theo dõi chi phí hàng ngày theo dịch vụ
- Xu hướng chi phí hàng tháng
- Đề xuất từ AWS Cost Explorer
- Cảnh báo ngân sách
- Phân tích sử dụng tài nguyên

### **Giám sát Bảo mật**
- Phát hiện mối đe dọa GuardDuty
- Tổng hợp phát hiện Security Hub
- Lọc theo mức độ nghiêm trọng
- Cảnh báo email cho phát hiện quan trọng
- Tgiheo dõi trạng thái tuân thủ

### **Công cụ Đề xuất**
- Đề xuất tối ưu chi phí
- Cải thiện hiệu suất
- Tăng cường bảo mật
- Ưu tiên dựa trên tác động
- Theo dõi triển khai

---

## 5. Triển khai Kỹ thuật

### **Công nghệ Sử dụng**

**Backend:**
- Python 3.9+ với FastAPI
- boto3 (AWS SDK)
- cryptography (mã hóa Fernet)
- asyncio cho worker đồng thời
- Redis cho caching

**Frontend:**
- React 18 với Vite
- TanStack Query cho data fetching
- Recharts cho trực quan hóa
- Tailwind CSS cho styling

**Cơ sở dữ liệu:**
- DynamoDB (5 bảng, on-demand pricing)
- Redis (in-memory caching)

**Email:**
- AWS SES cho email giao dịch
- Mẫu email HTML
- Xác thực dựa trên token

### **Triển khai Bảo mật**

**Mã hóa Thông tin Đăng nhập:**
```python
# Mã hóa đối xứng Fernet
from cryptography.fernet import Fernet

# AWS keys của khách hàng được mã hóa trước khi lưu trữ
encrypted_key = cipher.encrypt(client_aws_key.encode())
# Lưu trong DynamoDB, chỉ giải mã khi cần thiết
```

**Xác thực Email:**
```python
# Tạo token với thời hạn
token = secrets.token_urlsafe(32)
expires = datetime.now() + timedelta(hours=24)
# Lưu với bản ghi khách hàng, xác minh khi click
```

**Worker Manager:**
```python
# Tự động phát hiện khách hàng và tạo worker
for client in active_clients:
    if client_id not in workers:
        worker = CloudHealthWorker(client_provider, client_id)
        workers[client_id] = asyncio.create_task(worker.start())
```

**Cách ly Dữ liệu:**
- Tất cả truy vấn được lọc theo `client_id`
- Khóa phân vùng DynamoDB bao gồm định danh khách hàng
- Endpoint API yêu cầu xác thực khách hàng
- Không có rò rỉ dữ liệu giữa khách hàng

---

## 6. Lộ trình Phát triển (3 Tháng)

### **Tháng 1: Nền tảng & Tính năng Cốt lõi**

**Tuần 1-2: Hạ tầng & Thiết lập Đa khách hàng**
- Thiết lập EC2 instance với tất cả dịch vụ
- Tạo schema 5 bảng DynamoDB
- API đăng ký khách hàng với mã hóa
- Khung worker manager
- Xác thực cơ bản

**Tuần 3-4: Hệ thống Thu thập Dữ liệu**
- Thu thập metrics CloudWatch
- Triển khai worker theo khách hàng
- Lưu trữ DynamoDB cho tất cả 5 bảng
- Lớp caching Redis
- Dashboard cơ bản với metrics

**Sản phẩm Bàn giao:** Thu thập metrics đa khách hàng hoạt động

---

### **Tháng 2: Tính năng & Hệ thống Email**

**Tuần 5-6: Tích hợp Chi phí & Bảo mật**
- Tích hợp API Cost Explorer
- Thu thập phát hiện GuardDuty
- Dashboard bảo mật
- Biểu đồ phân tích chi phí

**Tuần 7-8: Hệ thống Thông báo Email**
- Thiết lập và xác minh AWS SES
- Triển khai luồng xác thực email
- Mẫu email HTML (xác thực + cảnh báo)
- Gửi email cảnh báo quan trọng
- Trang cài đặt với quản lý email
- Tùy chọn thông báo

**Sản phẩm Bàn giao:** Giám sát đầy đủ + thông báo email

---

### **Tháng 3: Hoàn thiện & Sản xuất**

**Tuần 9-10: Đề xuất & Kiểm thử**
- Công cụ đề xuất
- Ưu tiên dựa trên tác động
- Kiểm thử end-to-end
- Sửa lỗi
- Tối ưu hiệu suất

**Tuần 11-12: Triển khai Sản xuất**
- Thiết lập SSL/TLS
- Cấu hình Nginx
- Giám sát và logging
- Tài liệu
- Chuẩn bị demo

**Sản phẩm Bàn giao:** Nền tảng SaaS đa khách hàng sẵn sàng sản xuất

---

## 7. Ước tính Ngân sách

### **Chi phí Nền tảng Hàng tháng**

| Dịch vụ | Mô tả | Tháng 1 | Tháng 2 | Tháng 3 |
|---------|-------|---------|---------|---------|
| EC2 t3.micro | 750h Free Tier | $0 | $0 | $0 |
| DynamoDB (5 bảng) | On-demand, đa khách hàng | $3-4 | $5-8 | $8-12 |
| AWS SES | Gửi email | $0 | $0-1 | $1-2 |
| CloudWatch | Metrics + Logs | $0-1 | $1-2 | $2-3 |
| Data Transfer | 15GB Free Tier | $0 | $0-1 | $1 |
| S3 Backup | Lưu trữ ~5GB | $0 | $0-1 | $1 |
| **TỔNG CỘNG** | | **$3-5** | **$6-13** | **$13-20** |

### **Chi phí Tăng thêm Mỗi Khách hàng**

**Lưu trữ & Vận hành DynamoDB (mỗi khách hàng/tháng):**
- Lưu trữ: ~500MB = $0.125
- Ghi: 43,200/tháng = $0.054
- Đọc: 30,000/tháng = $0.0075
- **Tổng mỗi khách hàng: ~$0.19/tháng**

**Ví dụ về Mở rộng:**
- 10 khách hàng: $5 + ($0.19 × 10) = **$6.90/tháng**
- 20 khách hàng: $5 + ($0.19 × 20) = **$8.80/tháng**
- 50 khách hàng: $5 + ($0.19 × 50) = **$14.50/tháng**
- 100 khách hàng: $5 + ($0.19 × 100) = **$24/tháng**

**Chi phí Email (AWS SES):**
- 62,000 email đầu tiên/tháng: Miễn phí (nếu gửi từ EC2)
- Thêm: $0.10 mỗi 1,000 email
- Sử dụng điển hình: 2-5 email mỗi khách hàng/tháng = **Miễn phí**

### **Chiến lược Tối ưu Chi phí**
TTL DynamoDB cho dọn dẹp dữ liệu tự động
Caching Redis giảm chi phí đọc 80%
Ghi hàng loạt để hiệu quả
Định giá on-demand (không lãng phí công suất dự phòng)
Tối đa hóa Free Tier (12 tháng)
Gửi email hàng loạt để tiết kiệm chi phí

---

## 8. Đánh giá Rủi ro

| Rủi ro | Tác động | Xác suất | Giảm thiểu |
|--------|----------|----------|------------|
| Vượt ngân sách | Trung bình | Thấp | Cảnh báo AWS Budget, giám sát hàng ngày, DynamoDB on-demand |
| Ngừng hoạt động EC2 | Cao | Thấp | Cảnh báo CloudWatch, tự động khởi động lại systemd, mục tiêu uptime 98% |
| Bảo mật dữ liệu khách hàng | Cao | Thấp | Mã hóa Fernet, đặc quyền tối thiểu IAM, ghi nhật ký kiểm toán |
| Phân vùng nóng DynamoDB | Trung bình | Thấp | Thiết kế khóa phân vùng đúng, phân mảnh client_id |
| Vấn đề gửi email | Trung bình | Thấp | Giám sát AWS SES, logic thử lại, thông báo dự phòng |
| Lỗi worker manager | Trung bình | Thấp | Kiểm tra sức khỏe, tự động khởi động lại, ghi nhật ký lỗi |
| Mở rộng phạm vi | Trung bình | Cao | Định nghĩa MVP nghiêm ngặt, đóng băng tính năng tuần 8, kế hoạch Giai đoạn 2 |

---

## 9. Kết quả Mong đợi

### **Sản phẩm Bàn giao Kỹ thuật**

**Nền tảng Đa khách hàng Hoạt động:**
- Đăng ký khách hàng với xác thực email
- 5 bảng DynamoDB với cách ly dữ liệu đúng
- Quản lý worker tự động (10-50 khách hàng)
- Dashboard giám sát thời gian thực cho mỗi khách hàng
- Hệ thống thông báo email (xác thực + cảnh báo)
- Tính năng chi phí, bảo mật và đề xuất
- Trang cài đặt với quản lý email/thông báo

**Mục tiêu Hiệu suất:**
- Thời gian phản hồi API: <300ms (cached), <2s (uncached)
- Gửi email: <30 giây
- Thu thập dữ liệu: Mỗi 5 phút cho mỗi khách hàng
- Thời gian hoạt động nền tảng: 98-99%
- Khởi động worker: <10 giây mỗi khách hàng

**Bảo mật & Tuân thủ:**
- Lưu trữ thông tin đăng nhập được mã hóa
- Xác thực email trước cảnh báo quan trọng
- Cách ly dữ liệu giữa khách hàng
- Vai trò IAM đặc quyền tối thiểu
- Ghi nhật ký kiểm toán cho tất cả hoạt động

### **Kết quả Học tập**

**Thành thạo Dịch vụ AWS:**
- DynamoDB nâng cao (đa khách hàng, GSI, TTL)
- Tích hợp AWS SES (email giao dịch)
- CloudWatch, Cost Explorer, GuardDuty
- Vai trò IAM và mã hóa (Fernet)

**Kiến trúc SaaS:**
- Mô hình hóa dữ liệu đa khách hàng
- Điều phối worker nền
- Quản lý thông tin đăng nhập được mã hóa
- Hệ thống thông báo email
- Luồng đăng ký tự phục vụ

**Phát triển Full-Stack:**
- Lập trình bất đồng bộ FastAPI
- Quản lý trạng thái React
- Mẫu truy cập DynamoDB
- Thiết kế mẫu email
- Thiết kế RESTful API

### **Giá trị Portfolio**

Dự án này thể hiện:
**Kiến trúc cấp doanh nghiệp** (SaaS đa khách hàng)
**Thiết kế ưu tiên bảo mật** (mã hóa, xác thực email)
**Chuyên môn AWS** (tích hợp 10+ dịch vụ)
**Kỹ năng full-stack** (Python, React, DynamoDB, SES)
**Tối ưu chi phí** ($0.19/khách hàng/tháng ở quy mô)
**Sẵn sàng sản xuất** (giám sát, logging, xử lý lỗi)
**Tính năng chuyên nghiệp** (hệ thống email, cảnh báo tự động)

**Điểm khác biệt chính:**
- Không phải công cụ giám sát đơn giản - một nền tảng SaaS hoàn chỉnh
- Thể hiện khả năng thiết kế cho nhiều khách hàng
- Cho thấy hiểu biết về mã hóa và bảo mật
- Bao gồm tính năng SaaS hiện đại (xác thực email, thông báo)
- Mở rộng tiết kiệm chi phí ($20/tháng cho 50+ khách hàng)

---

## 10. Kết luận

**AWS Cloud Health Dashboard** là một **nền tảng SaaS đa khách hàng cấp sản xuất** thể hiện:

1. **Kỹ năng Kiến trúc Doanh nghiệp**
   - Mô hình hóa dữ liệu đa khách hàng
   - Quản lý thông tin đăng nhập được mã hóa
   - Điều phối worker nền
   - Thiết kế có thể mở rộng (10-100+ khách hàng)

2. **Chuyên môn AWS**
   - Tích hợp sâu với 10+ dịch vụ AWS
   - Thiết kế hạ tầng tối ưu chi phí
   - Thực hành tốt nhất về bảo mật
   - Hệ thống thông báo email (SES)

3. **Phát triển Full-Stack**
   - Công nghệ hiện đại (FastAPI + React)
   - UI/UX chuyên nghiệp
   - Thiết kế mẫu email
   - Thiết kế RESTful API

4. **Hiểu biết Kinh doanh**
   - Vận hành tiết kiệm chi phí ($10-20/tháng)
   - Mô hình định giá có thể mở rộng ($0.19/khách hàng)
   - ROI rõ ràng cho khách hàng (tiết kiệm chi phí 15-25%)
   - Tính năng SaaS chuyên nghiệp

**Thời gian:** 3 tháng | **Nhóm:** 4 người | **Ngân sách:** $10-20/tháng

## Phụ lục

**A. GitHub Repository: https://github.com/Unvianpetronas/Cloud_health_dashboard**

**B. Thông tin Liên hệ:**
- **Trưởng dự án:** Trương Quốc Tuấn
- **Email:** unviantruong26@gmail.com
- **WhatsApp:** 0798806545

---

