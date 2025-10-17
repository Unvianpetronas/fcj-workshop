---
title: "Proposal"
date: "2025-10-13"
weight: 200
chapter: false
pre: "  2.  "
---
# AWS Cloud Health Dashboard 

---

## 1. Tóm Tắt Điều Hành

**AWS Cloud Health Dashboard** là một **nền tảng SaaS đa khách hàng chuẩn production với triển khai DevSecOps đầy đủ**, cho phép doanh nghiệp giám sát và tối ưu hóa hạ tầng AWS cho nhiều khách hàng từ một hệ thống tập trung duy nhất.

**Điểm Nổi Bật:**

- **Kiến trúc DevSecOps**: Pipeline CI/CD hoàn chỉnh với quét bảo mật tự động
- **Kiến trúc đa khách hàng**: Giám sát 10-50+ tài khoản AWS khách hàng từ một nền tảng
- **Chi phí nền tảng**: $23-33/tháng (Năm 1) với các tính năng DevSecOps
- **Chi phí mỗi khách hàng**: ~$0.19/tháng cho lưu trữ dữ liệu
- **Thiết kế bảo mật tối ưu**: Mã hóa AWS Secrets Manager + KMS
- **Triển khai tự động**: CodePipeline + CodeBuild với triển khai SSH
- **5 bảng DynamoDB**: Mô hình dữ liệu tối ưu với cô lập khách hàng
- **Redis caching**: Giảm 80% chi phí đọc database
- **Hệ thống email thông báo**: Tích hợp AWS SES cho cảnh báo quan trọng

**Công Nghệ:** FastAPI (Python) + React + DynamoDB + Redis + AWS Secrets Manager + KMS + CloudWatch + CloudTrail + CodePipeline + CodeBuild + EC2 t3.micro

---

## 2. Bài Toán Đặt Ra

**Thách Thức Hiện Tại:**

Các doanh nghiệp quản lý hạ tầng AWS cho nhiều khách hàng gặp phải:

- **Không có giám sát tập trung**: Phải đăng nhập vào từng AWS console của mỗi khách hàng
- **Cảnh báo bảo mật phân tán**: Các phát hiện quan trọng từ GuardDuty bị bỏ qua
- **Thiếu khả năng hiển thị chi phí**: Khó theo dõi và tối ưu chi phí giữa các khách hàng
- **Quản lý thông tin xác thực không an toàn**: AWS access keys được lưu trong database hoặc config files
- **Không có triển khai tự động**: Triển khai thủ công dẫn đến lỗi và downtime
- **Thiếu quét bảo mật**: Các lỗ hổng được triển khai lên production
- **Không có audit logging**: Không thể theo dõi các sự kiện bảo mật và API calls
- **Giám sát thủ công**: Mất 30+ phút mỗi ngày cho mỗi khách hàng

**Giải Pháp Của Chúng Tôi:**

Cloud Health Dashboard cung cấp **một nền tảng hỗ trợ DevSecOps** với:

**Kiến Trúc DevSecOps**
- Pipeline CI/CD tự động (CodePipeline + CodeBuild)
- Quét bảo mật (SAST với Bandit, quét dependencies với Safety)
- Triển khai tự động qua SSH
- Giám sát hạ tầng với CloudWatch
- Audit logging với CloudTrail
- Quản lý secrets với AWS Secrets Manager + KMS

**Kiến Trúc Đa Khách Hàng**
- Một nền tảng giám sát 10-50+ tài khoản AWS khách hàng
- AWS Secrets Manager để lưu trữ credentials (mã hóa KMS)
- Cô lập dữ liệu hoàn toàn giữa các khách hàng
- Quản lý worker tự động cho mỗi khách hàng

**Giám Sát Tập Trung**
- Dashboard duy nhất cho tất cả khách hàng
- Tình trạng hạ tầng thời gian thực
- Lưu giữ dữ liệu lịch sử (30-365 ngày)
- Giám sát 15+ dịch vụ AWS
- Redis caching để tối ưu hiệu suất

---

## 3. Kiến Trúc Giải Pháp

### **Tổng Quan Kiến Trúc**

![new.png](/images/2-Proposal/new.png)

```

### **Luồng Dữ Liệu**

```
1. QUY TRÌNH LÀM VIỆC CỦA DEVELOPER (DevSecOps)
   Developer → Git push → GitHub → CodePipeline được kích hoạt
   → CodeBuild chạy:
     • Cài đặt dependencies
     • Chạy tests (pytest)
     • Quét bảo mật (Bandit, Safety)
     • Build frontend
     • SSH đến EC2 → Triển khai
   → CloudWatch ghi log mọi thứ

2. ĐĂNG KÝ KHÁCH HÀNG
   Khách hàng → Nhập vào email → Nhấn vào xác thực → email xác thực được gửi tới → Nhấn vào link → API → Xác thực AWS keys → Lưu vào Secrets Manager (mã hóa KMS)
   → Lưu metadata vào DynamoDB → Gửi email xác thực (SES)

3. XÁC THỰC EMAIL
   Khách hàng → Click link → Xác thực token → Đánh dấu email đã xác thực 
   → CloudTrail ghi log sự kiện

4. WORKER MANAGER (Đa Khách Hàng)
   Mỗi 15 phút → Lấy khách hàng đang hoạt động từ DynamoDB
   → Cho mỗi khách hàng:
     • Lấy credentials từ Secrets Manager
     • Khởi động worker nếu chưa chạy
     • Thu thập dữ liệu AWS
     • Cache trong Redis (TTL 5 phút)
     • Lưu vào DynamoDB

5. THU THẬP DỮ LIỆU (Mỗi Khách Hàng)
   Worker → Secrets Manager (lấy credentials)
   → AWS API của khách hàng → Thu thập metrics
   → Cache trong Redis → Lưu vào DynamoDB (phân vùng theo aws_account_id)
   → Nếu phát hiện nghiêm trọng → Gửi email cảnh báo (SES)
   → CloudWatch ghi log tất cả operations

6. HIỂN THỊ DASHBOARD
   Khách hàng đăng nhập → FastAPI kiểm tra Redis cache
   → Cache HIT: Trả về dữ liệu cached (< 100ms)
   → Cache MISS: Query DynamoDB (lọc theo aws_account_id)
   → Lưu vào Redis → Trả về dữ liệu

7. BẢO MẬT & KIỂM TOÁN
   Tất cả API calls → CloudTrail logging
   Truy cập credentials → CloudWatch alarm
   Thử đăng nhập thất bại → Email cảnh báo
   Sử dụng KMS key → Audit trail

## 4. Tính Năng Chính

### **Triển Khai DevSecOps**

**Pipeline CI/CD Tự Động:**
- Tích hợp GitHub cho quản lý mã nguồn
- CodePipeline để điều phối (1 pipeline MIỄN PHÍ)
- CodeBuild để build tự động (100 phút/tháng MIỄN PHÍ)
- Quét bảo mật tự động trước khi triển khai
- Triển khai tự động qua SSH
- Chiến lược triển khai không downtime

**Quét Bảo Mật:**
- **Bandit**: Static Application Security Testing (SAST)
- **Safety**: Quét lỗ hổng dependencies
- Security gates trước triển khai
- Build thất bại nếu có lỗi HIGH severity

**Quản Lý Secrets:**
- AWS Secrets Manager để lưu trữ credentials
- Mã hóa KMS cho tất cả secrets
- Hỗ trợ rotation tự động
- Không có credentials trong code hoặc biến môi trường
- Chỉ truy cập qua IAM role

**Giám Sát & Logging:**
- CloudWatch cho logs và metrics ứng dụng
- CloudTrail cho API audit logging
- Custom metrics cho business KPIs
- Cảnh báo tự động cho bất thường
- Lưu giữ log 90 ngày

**Bảo Mật Hạ Tầng:**
- Security Groups (tường lửa mạng)
- IAM roles với quyền tối thiểu
- AWS Shield Standard (bảo vệ DDoS)
- Mã hóa dữ liệu lưu trữ (KMS)
- Mã hóa dữ liệu truyền tải (TLS/HTTPS)

### **Quản Lý Đa Khách Hàng**

- Đăng ký khách hàng tự phục vụ với xác thực AWS key
- Credentials lưu trong Secrets Manager (mã hóa KMS)
- Khởi động worker tự động cho mỗi khách hàng
- Cô lập dữ liệu hoàn toàn
- Truy cập dashboard theo từng khách hàng
- Redis caching để tối ưu hiệu suất

### **Hệ Thống Email Thông Báo**

- Xác thực email với token bảo mật (hết hạn 24h)
- Cảnh báo GuardDuty quan trọng qua AWS SES
- Tùy chỉnh preferences thông báo
- HTML email templates
- Chỉnh sửa email trong trang Settings

### **Giám Sát Hạ Tầng**

- Metrics thời gian thực từ 15+ dịch vụ AWS
- Lưu giữ dữ liệu lịch sử (30+ ngày)
- Redis caching (giảm 80% chi phí)
- Giám sát EC2, S3, RDS, Lambda
- Dashboard tình trạng dịch vụ

### **Tối Ưu Chi Phí**

- Theo dõi chi phí hàng ngày theo dịch vụ
- Xu hướng chi phí hàng tháng
- Đề xuất từ AWS Cost Explorer
- Cảnh báo ngân sách
- Phân tích chi phí theo khách hàng

### **Giám Sát Bảo Mật**

- Phát hiện mối đe dọa GuardDuty (tùy chọn)
- Lọc theo mức độ nghiêm trọng
- Email cảnh báo cho phát hiện quan trọng
- CloudTrail audit logging
- Theo dõi trạng thái tuân thủ

---

## 5. Triển Khai Kỹ Thuật

### **Ngăn Xếp Công Nghệ**

**Backend:**
- Python 3.12+ với FastAPI
- boto3 (AWS SDK)
- asyncio cho concurrent workers
- Redis cho caching
- pytest cho testing

**Frontend:**
- React 18 với Vite
- TanStack Query cho data fetching
- Recharts cho visualization
- Tailwind CSS cho styling

**Database:**
- DynamoDB (5 bảng, on-demand pricing)
- Redis (in-memory caching, localhost)

**DevSecOps:**
- GitHub (quản lý mã nguồn)
- CodePipeline (điều phối CI/CD)
- CodeBuild (build tự động)
- Bandit (SAST security scanner)
- Safety (dependency scanner)

**Bảo Mật:**
- AWS Secrets Manager (lưu trữ credentials)
- AWS KMS (khóa mã hóa)
- CloudTrail (audit logging)
- CloudWatch (giám sát & cảnh báo)

**Email:**
- AWS SES cho transactional emails
- HTML email templates
- Xác thực dựa trên token

**Cô Lập Dữ Liệu:**

- Tất cả queries được lọc theo `aws_account_id`
- DynamoDB partition key bao gồm client identifier
- API endpoints yêu cầu client authentication
- Credentials được cô lập trong Secrets Manager
- Không có rò rỉ dữ liệu giữa các khách hàng

---

## 6. Ước Tính Ngân Sách

### **Chi Phí Nền Tảng Hàng Tháng**

| Dịch Vụ                    | Mô Tả                          | Tháng 1 | Tháng 2 | Tháng 3 |
|----------------------------|--------------------------------|---------|---------|---------|
| **EC2 t3.micro**           | 750h Miễn Phí                  | $0      | $0      | $0      |
| **DynamoDB (5 bảng)**      | On-demand, đa khách hàng       | $3-4    | $5-8    | $8-12   |
| **AWS Secrets Manager**    | 20 secrets @ $0.40/secret      | $4-6    | $6-8    | $8      |
| **AWS KMS**                | 2-3 keys @ $1/key              | $2      | $2-3    | $2-3    |
| **AWS SES**                | Gửi email                      | $0      | $0-1    | $1-2    |
| **CloudWatch**             | Logs + Metrics                 | $0-1    | $1-2    | $2-3    |
| **CloudTrail**             | 1 trail (MIỄN PHÍ)             | $0      | $0      | $0      |
| **CodePipeline**           | 1 pipeline (MIỄN PHÍ)          | $0      | $0      | $0      |
| **CodeBuild**              | 100 phút/tháng (MIỄN PHÍ)      | $0      | $0      | $0      |
| **Truyền Dữ Liệu**         | 15GB Miễn Phí                  | $0      | $0-1    | $1      |
| **S3 Backup**              | ~5GB storage                   | $0      | $0-1    | $1      |
| **Redis**                  | Trên EC2 (localhost - MIỄN PHÍ)| $0      | $0      | $0      |
| **TỔNG**                   |                                | **$9-13** | **$14-24** | **$23-33** |

### **Chi Phí Sau Miễn Phí (Năm 2+)**

| Dịch Vụ                    | Chi Phí Hàng Tháng |
|----------------------------|--------------------|
| EC2 t3.micro               | $8-10              |
| DynamoDB                   | $8-12              |
| Secrets Manager            | $8                 |
| KMS                        | $2-3               |
| SES                        | $1-2               |
| CloudWatch                 | $2-3               |
| S3                         | $1                 |
| Truyền Dữ Liệu             | $1                 |
| **TỔNG Năm 2+**            | **$31-40/tháng**   |

### **Chi Phí Gia Tăng Mỗi Khách Hàng**

**DynamoDB Lưu Trữ & Operations (mỗi khách hàng/tháng):**

- **Lưu trữ:** ~500MB = $0.125
- **Writes:** 43,200/tháng = $0.054
- **Reads (với Redis):** 6,000/tháng = $0.0015 (giảm 80%)
- **Tổng mỗi khách hàng: ~$0.19/tháng**

**Secrets Manager (mỗi khách hàng):**
- **Mỗi secret:** $0.40/tháng
- **Tổng với Secrets Manager: ~$0.59/tháng mỗi khách hàng**

**Ví Dụ Mở Rộng:**

- **10 khách hàng:** Nền tảng $23 + (10 × $0.59) = **$28.90/tháng**
- **20 khách hàng:** Nền tảng $23 + (20 × $0.59) = **$34.80/tháng**
- **50 khách hàng:** Nền tảng $23 + (50 × $0.59) = **$52.50/tháng**

### **Chiến Lược Tối Ưu Chi Phí**

- **Redis caching**: Giảm DynamoDB reads 80%
- **DynamoDB TTL**: Tự động dọn dẹp dữ liệu (không tốn phí)
- **Batch writes**: Ít API calls hơn
- **On-demand pricing**: Chỉ trả tiền cho usage
- **Tối đa hóa Free Tier**: 12 tháng miễn phí cho EC2, S3, CloudWatch
- **Tối ưu CodeBuild**: Giữ builds dưới 100 phút/tháng (MIỄN PHÍ)
- **Email batching**: Tối đa hóa SES free tier

---

## 7. Đánh Giá Rủi Ro

| Rủi Ro                        | Tác Động | Xác Suất | Giảm Thiểu                                                      |
|-------------------------------|----------|----------|----------------------------------------------------------------|
| Vượt ngân sách                | Trung bình | Thấp   | AWS Budget alerts, Redis caching, DynamoDB on-demand           |
| EC2 downtime                  | Cao      | Thấp     | CloudWatch alarms, systemd auto-restart, mục tiêu 98% uptime   |
| Bảo mật dữ liệu khách hàng    | Cao      | Thấp     | Secrets Manager + KMS, IAM tối thiểu, audit logging            |
| Lỗi CI/CD pipeline            | Trung bình | Thấp   | CodeBuild retry logic, rollback triển khai, health checks      |
| Redis cache failure           | Trung bình | Thấp   | Systemd monitor, auto-restart, graceful degradation            |
| DynamoDB hot partitions       | Trung bình | Thấp   | Thiết kế partition key đúng, sharding aws_account_id           |
| Chi phí Secrets Manager       | Trung bình | Trung bình | Giám sát usage, tối ưu số lượng secret, xem xét alternatives |
| Vấn đề gửi email              | Trung bình | Thấp   | Giám sát AWS SES, retry logic, thông báo dự phòng             |
| Lỗ hổng bảo mật trong deps    | Cao      | Trung bình | Safety scanner trong CI/CD, cập nhật tự động, cảnh báo bảo mật|
| Scope creep                   | Trung bình | Cao    | Định nghĩa MVP nghiêm ngặt, đóng băng tính năng tuần 8        |

---

## 8. Kết Quả Mong Đợi

### **Sản Phẩm Kỹ Thuật**

**Nền Tảng DevSecOps:**

- Pipeline CI/CD hoàn chỉnh (GitHub → CodePipeline → CodeBuild → EC2)
- Quét bảo mật tự động (Bandit SAST, Safety dependency scan)
- Triển khai tự động qua SSH
- Giám sát CloudWatch và audit logging CloudTrail
- AWS Secrets Manager để lưu trữ credentials
- Mã hóa KMS cho tất cả dữ liệu nhạy cảm
- Lớp Redis caching trên EC2

**SaaS Đa Khách Hàng:**

- Đăng ký khách hàng với xác thực email
- 5 bảng DynamoDB với cô lập dữ liệu đúng
- Quản lý worker tự động (10-50 khách hàng)
- Dashboard giám sát thời gian thực cho mỗi khách hàng
- Hệ thống email thông báo
- Trang Settings với quản lý email/thông báo

**Mục Tiêu Hiệu Suất:**

- **Thời gian phản hồi API:** <100ms (Redis cache HIT), <2s (cache MISS)
- **Gửi email:** <30 giây
- **Thu thập dữ liệu:** Mỗi 15 phút cho mỗi khách hàng
- **Uptime nền tảng:** 98-99%
- **Khởi động worker:** <10 giây cho mỗi khách hàng
- **Thời gian build:** <5 phút (dưới giới hạn free tier)
- **Thời gian triển khai:** <3 phút

**Bảo Mật & Tuân Thủ:**

- Không có credentials trong code hoặc biến môi trường
- Tất cả secrets được mã hóa với KMS
- Xác thực email trước cảnh báo quan trọng
- Audit trail API hoàn chỉnh (CloudTrail)
- Cô lập dữ liệu giữa các khách hàng
- IAM roles với quyền tối thiểu
- Quét bảo mật tự động trong CI/CD

### **Kết Quả Học Tập**

**Thành Thạo DevSecOps:**

- Thiết kế và triển khai CI/CD pipeline
- Quét bảo mật tự động (SAST, dependency scanning)
- Khái niệm Infrastructure as Code
- Best practices quản lý secrets
- Audit logging và tuân thủ
- Giám sát và cảnh báo

**Thành Thạo Dịch Vụ AWS:**

- DynamoDB nâng cao (đa khách hàng, GSI, TTL)
- Tích hợp AWS Secrets Manager + KMS
- CloudWatch + CloudTrail
- CodePipeline + CodeBuild
- Tích hợp AWS SES
- IAM roles và policies

**Kiến Trúc SaaS:**

- Mô hình dữ liệu đa khách hàng
- Điều phối background worker
- Quản lý credentials an toàn
- Chiến lược caching (Redis)
- Hệ thống email thông báo

**Phát Triển Full-Stack:**

- Lập trình async FastAPI
- Quản lý state React
- Tích hợp Redis
- Phát triển bảo mật đầu tiên
- Thiết kế RESTful API

### **Giá Trị Portfolio**

Dự án này thể hiện:

1. **DevSecOps Chuẩn Production**
    - Pipeline CI/CD hoàn chỉnh
    - Quét bảo mật tự động
    - Quản lý secrets với AWS Secrets Manager + KMS
    - Giám sát và logging toàn diện

2. **Kiến Trúc Enterprise**
    - Thiết kế SaaS đa khách hàng
    - Hạ tầng có thể mở rộng (10-100+ khách hàng)
    - Lớp Redis caching
    - Hệ thống background worker

3. **Chuyên Môn Bảo Mật**
    - Không có secrets trong code
    - Mã hóa KMS
    - CloudTrail audit logging
    - Quét lỗ hổng tự động

4. **Thành Thạo AWS**
    - Tích hợp 14+ dịch vụ AWS
    - Thiết kế tối ưu chi phí
    - Tối đa hóa free tier
    - Best practices IAM

5. **Hiệu Quả Chi Phí**
    - $23-33/tháng cho nền tảng (Năm 1)
    - $0.59/khách hàng/tháng gia tăng
    - Giảm 80% chi phí đọc (Redis)

**Điểm Khác Biệt Chính:**

- Triển khai DevSecOps đầy đủ (không chỉ là app đơn giản)
- Bảo mật chuẩn production (Secrets Manager + KMS)
- CI/CD tự động với security gates
- Kiến trúc đa khách hàng quy mô lớn
- Giám sát toàn diện và audit logging
- Redis caching hiệu quả chi phí

---

## 9. Kết Luận

**AWS Cloud Health Dashboard** là một **nền tảng SaaS đa khách hàng chuẩn production, hỗ trợ DevSecOps** thể hiện:

1. **Xuất Sắc DevSecOps**
    - Pipeline CI/CD tự động (CodePipeline + CodeBuild)
    - Quét bảo mật trước mỗi triển khai
    - Quản lý secrets với AWS Secrets Manager + KMS
    - Giám sát toàn diện (CloudWatch + CloudTrail)
    - Nguyên tắc Infrastructure as Code

2. **Kiến Trúc Enterprise**
    - Thiết kế đa khách hàng hỗ trợ 10-100+ clients
    - Hệ thống worker có thể mở rộng
    - Redis caching cho hiệu suất
    - Cô lập dữ liệu hoàn toàn

3. **Thiết Kế Bảo Mật Đầu Tiên**
    - Không có secrets trong code hoặc config files
    - Mã hóa KMS cho tất cả dữ liệu nhạy cảm
    - CloudTrail audit logging
    - Quét lỗ hổng tự động

4. **Chuyên Môn AWS**
    - Tích hợp sâu với 14+ dịch vụ AWS
    - Hạ tầng tối ưu chi phí
    - Best practices bảo mật
    - Hệ thống email thông báo (SES)

5. **Hiểu Biết Kinh Doanh**
    - Vận hành hiệu quả chi phí ($23-33/tháng Năm 1)
    - Mô hình giá có thể mở rộng ($0.59/khách hàng/tháng)
    - Tính năng SaaS chuyên nghiệp
    - ROI rõ ràng cho khách hàng

**Thời Gian:** 3 tháng | **Nhóm:** 4 người | **Ngân Sách:** $23-33/tháng (Năm 1), $31-40/tháng (Năm 2+)

**Dự án này thể hiện triển khai DevSecOps sẵn sàng production, làm cho nó trở thành một portfolio piece xuất sắc cho các vai trò cloud engineering và SRE.**

---

## Phụ Lục

**A. Các Dịch Vụ Sử Dụng (14 Dịch Vụ AWS):**

1. EC2 (Tính toán)
2. DynamoDB (Cơ sở dữ liệu)
3. AWS Secrets Manager (Lưu trữ credentials)
4. AWS KMS (Mã hóa)
5. CloudWatch (Giám sát)
6. CloudTrail (Audit logging)
7. CodePipeline (CI/CD)
8. CodeBuild (Build tự động)
9. SES (Email)
10. S3 (Backup)
11. VPC (Mạng)
12. Internet Gateway (Kết nối)
13. Security Groups (Tường lửa)
14. IAM (Quản lý truy cập)
15. Shield Standard (Bảo vệ DDoS - tự động)

**B. GitHub Repository:** [https://github.com/Unvianpetronas/Cloud_health_dashboard](https://github.com/Unvianpetronas/Cloud_health_dashboard)

**C. Thông Tin Liên Hệ:**
- **Trưởng Dự Án:** Trương Quốc Tuấn
- **Email:** `unviantruong26@gmail.com`
- **WhatsApp:** `+84 798806545`

---