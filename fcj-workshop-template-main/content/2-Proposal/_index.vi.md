---
title: "Proposal"
date: "2025-10-13"
weight: 200
chapter: false
pre: "  2.  "
---
# AWS Cloud Health Dashboard

---

## 1. Tóm tắt điều hành

AWS Cloud Health Dashboard là một nền tảng SaaS đa khách hàng cấp production với triển khai DevSecOps tự động hóa, cho phép doanh nghiệp giám sát và tối ưu hóa hạ tầng AWS cho nhiều khách hàng từ một hệ thống tập trung duy nhất.

### Điểm nổi bật chính:

- **Kiến trúc cấp Production:** Application Load Balancer với SSL/TLS termination, Elastic IP, health checks và khả năng triển khai zero-downtime
- **Pipeline DevSecOps:** CI/CD tự động với quét bảo mật và chiến lược triển khai blue-green
- **Kiến trúc đa khách hàng:** Được thiết kế để hỗ trợ 10-50+ tài khoản AWS khách hàng (demo được test với 3-5 khách hàng)
- **High Availability:** ALB với health checks, khả năng failover tự động và SLA 99.99%
- **Chi phí nền tảng:** $39-49/tháng (Năm 1) với hạ tầng cấp production
- **Chi phí mỗi khách hàng:** ~$0.59/tháng (có thể mở rộng và dự đoán được)
- **Thiết kế bảo mật ưu tiên:** AWS Secrets Manager + mã hóa KMS + chứng chỉ ACM
- **Triển khai tự động:** CodePipeline + CodeBuild với triển khai zero-downtime
- **5 bảng DynamoDB:** Mô hình dữ liệu tối ưu với cô lập khách hàng
- **Redis caching:** Khả năng giảm 60-80% chi phí đọc database
- **Hệ thống thông báo email:** Tích hợp AWS SES cho cảnh báo quan trọng
- **Elastic IP:** IP tĩnh MIỄN PHÍ cho sự ổn định production
- **Stack công nghệ:** FastAPI (Python) + React + DynamoDB + Redis + ALB + Elastic IP + ACM + AWS Secrets Manager + KMS + CloudWatch + CloudTrail + CodePipeline + CodeBuild + EC2 t3.micro

---

## 2. Vấn đề cần giải quyết

### Thách thức hiện tại:

Các doanh nghiệp quản lý hạ tầng AWS cho nhiều khách hàng đối mặt với:

- **Không có giám sát tập trung:** Phải đăng nhập vào console AWS của từng khách hàng riêng biệt
- **Cảnh báo bảo mật rải rác:** Các phát hiện GuardDuty quan trọng bị bỏ qua
- **Khoảng trống về chi phí:** Khó theo dõi và tối ưu hóa chi phí trên nhiều khách hàng
- **Quản lý thông tin xác thực không an toàn:** AWS access keys được lưu trong database hoặc config files
- **Không có triển khai tự động:** Triển khai thủ công dẫn đến lỗi và downtime
- **Thiếu quét bảo mật:** Lỗ hổng được triển khai lên production
- **Không có audit logging:** Không thể theo dõi sự kiện bảo mật và API calls
- **Giám sát thủ công:** 30+ phút mỗi ngày cho mỗi khách hàng
- **Downtime trong quá trình triển khai:** Không có chiến lược triển khai zero-downtime
- **IP động:** IP thay đổi khi khởi động lại gây ra vấn đề DNS/whitelist

### Giải pháp của chúng tôi:

Cloud Health Dashboard cung cấp nền tảng cấp production với các phương pháp DevSecOps bao gồm:

#### Hạ tầng cấp Production

- Application Load Balancer cho high availability và phân phối traffic
- Elastic IP cho địa chỉ IP ổn định, bền vững (MIỄN PHÍ khi được gắn)
- SSL/TLS termination với AWS Certificate Manager (ACM)
- Health checks với failover tự động
- Khả năng triển khai zero-downtime
- Sẵn sàng mở rộng theo chiều ngang (hiện tại single EC2, được thiết kế cho multi-instance)
- Đảm bảo SLA 99.99% của ALB

#### Kiến trúc DevSecOps

- Pipeline CI/CD tự động (CodePipeline + CodeBuild)
- Quét bảo mật (SAST với Bandit, quét dependency với Safety)
- Chiến lược triển khai blue-green cho zero downtime
- Giám sát hạ tầng với CloudWatch
- Audit logging với CloudTrail
- Quản lý secrets với AWS Secrets Manager + KMS

#### Kiến trúc đa khách hàng

- Kiến trúc có thể mở rộng để giám sát 10-50+ tài khoản AWS khách hàng (MVP demo với 3-5 khách hàng)
- AWS Secrets Manager cho lưu trữ thông tin xác thực (mã hóa KMS)
- Cô lập dữ liệu hoàn toàn giữa các khách hàng
- Hệ thống lập lịch task để thu thập dữ liệu

#### Giám sát tập trung

- Dashboard duy nhất cho tất cả khách hàng
- Sức khỏe hạ tầng theo thời gian thực
- Lưu giữ dữ liệu lịch sử (30-365 ngày)
- 15+ dịch vụ AWS được giám sát
- Redis caching để tối ưu hiệu suất

---

## 3. Kiến trúc giải pháp

### Tổng quan kiến trúc

![final_architecture.drawio.png](/images/2-Proposal/final_architecture.drawio.png)

### Các thành phần chính:

- **Internet Gateway:** Điểm vào VPC cho traffic đến
- **Application Load Balancer (ALB):**
   - Phân phối traffic qua nhiều availability zones
   - SSL/TLS termination với chứng chỉ ACM
   - Health checks với failover tự động
   - Path-based routing (sẵn sàng mở rộng trong tương lai)
   - Bảo vệ DDoS (AWS Shield Standard)
- **Elastic IP:**
   - Địa chỉ IPv4 public tĩnh cho EC2 instance
   - MIỄN PHÍ khi được gắn vào EC2 instance đang chạy
   - Bền vững qua các lần khởi động lại instance
   - Cho phép truy cập SSH ổn định và IP whitelisting
- **EC2 Instance (t3.micro):** Máy chủ ứng dụng xử lý
- **Triển khai Zero-Downtime:** Chiến lược blue-green sử dụng target groups
- **DynamoDB:** Lưu trữ dữ liệu đa khách hàng
- **Redis:** Lớp caching trong bộ nhớ
- **AWS Secrets Manager:** Lưu trữ thông tin xác thực an toàn
- **CloudWatch & CloudTrail:** Giám sát và audit logging

### Luồng dữ liệu

#### 1. QUY TRÌNH DEVELOPER (DevSecOps với Triển khai Zero-Downtime)

```
Developer → Git push → GitHub → CodePipeline được kích hoạt
→ CodeBuild chạy:
  • Cài đặt dependencies
  • Chạy tests (pytest)
  • Quét bảo mật (Bandit, Safety)
  • Build frontend
  • SSH đến EC2 (qua Elastic IP) → Deploy phiên bản mới
  • ALB health check xác thực triển khai
  • Chuyển đổi traffic (zero downtime)
→ CloudWatch ghi log mọi thứ
```

#### 2. TRAFFIC ĐẾN (Luồng Production)

```
Trình duyệt client → HTTPS (Cloudflare CDN/SSL)
→ Internet Gateway → ALB (chứng chỉ ACM, SSL termination)
→ Target Group (health check: /health endpoint)
→ EC2 Instance (Elastic IP được gắn, ứng dụng FastAPI)
→ Response trả lại qua ALB → Client
```

#### 3. ĐĂNG KÝ KHÁCH HÀNG

```
Khách hàng → Nhập AWS Access key → Click đăng nhập → Tài khoản tự động tạo
→ API (qua ALB) → Xác thực AWS keys
→ Lưu trong Secrets Manager (mã hóa KMS)
→ Lưu metadata trong DynamoDB
```

#### 4. XÁC THỰC EMAIL

```
Khách hàng → Nhập email → Kiểm tra email → Gửi email xác thực 
→ Click link → ALB → API → Xác thực token
→ Đánh dấu email đã xác thực → CloudTrail ghi log sự kiện
```

#### 5. WORKER MANAGER (Đa khách hàng)

```
Mỗi 15 phút → Lấy khách hàng đang hoạt động từ DynamoDB
→ Với mỗi khách hàng:
  • Lấy thông tin xác thực từ Secrets Manager
  • Lập lịch async task để thu thập dữ liệu
  • Thu thập dữ liệu AWS
  • Cache trong Redis (TTL 5 phút)
  • Lưu trong DynamoDB
```

#### 6. THU THẬP DỮ LIỆU (Mỗi khách hàng)

```
Async Task → Secrets Manager (lấy thông tin xác thực)
→ Client AWS API → Thu thập metrics
→ Cache trong Redis → Lưu trong DynamoDB (phân vùng theo aws_account_id)
→ Nếu phát hiện quan trọng → Gửi email cảnh báo (SES)
→ CloudWatch ghi log tất cả các hoạt động
```

#### 7. HIỂN THỊ DASHBOARD

```
Khách hàng đăng nhập → Request qua ALB → FastAPI kiểm tra Redis cache
→ Cache HIT: Trả về dữ liệu cached (< 200ms)
→ Cache MISS: Query DynamoDB (lọc theo aws_account_id)
→ Lưu trong Redis → Trả về dữ liệu qua ALB
```

#### 8. HEALTH CHECKS & FAILOVER

```
ALB → Mỗi 30s → GET /health endpoint
→ Nếu không khỏe mạnh (2 lần thất bại liên tiếp):
  • Đánh dấu target không khỏe mạnh
  • Dừng routing traffic
  • CloudWatch alarm được kích hoạt
  • Systemd cố gắng auto-restart
→ Khi khỏe mạnh: Tiếp tục routing traffic
```

#### 9. TRUY CẬP SSH & QUẢN LÝ

```
DevOps Engineer → SSH đến Elastic IP
→ Truy cập trực tiếp EC2 cho:
  • Xác thực triển khai
  • Kiểm tra logs
  • Khắc phục sự cố khẩn cấp
  • Bảo trì hệ thống
→ IP ổn định (không cần cập nhật DNS)
```

#### 10. BẢO MẬT & AUDIT

```
Tất cả API calls → CloudTrail logging
Truy cập thông tin xác thực → CloudWatch alarm
Các lần thử xác thực thất bại → Email cảnh báo
Sử dụng KMS key → Audit trail
ALB access logs → S3 (tùy chọn, cho compliance)
```

---

## 4. Các tính năng chính

### Hạ tầng cấp Production

#### Application Load Balancer (ALB):

- **High Availability:** Phân phối traffic qua nhiều availability zones
- **SSL/TLS Termination:** Chứng chỉ ACM cho mã hóa HTTPS
- **Health Checks:** Failover tự động nếu EC2 không khỏe mạnh
- **Path-Based Routing:** Routing Frontend và API (sẵn sàng mở rộng trong tương lai)
- **Bảo vệ DDoS:** AWS Shield Standard (tự động, không tốn phí)
- **SLA 99.99%:** Uptime được AWS đảm bảo
- **Triển khai Zero-Downtime:** Chuyển đổi target group cho triển khai blue-green

#### Elastic IP:

- **Địa chỉ IP tĩnh:** Public IPv4 bền vững không bao giờ thay đổi
- **Chi phí MIỄN PHÍ:** $0/tháng khi được gắn vào EC2 instance đang chạy
- **Truy cập SSH ổn định:** Truy cập quản lý đáng tin cậy với IP cố định
- **IP Whitelisting:** Cho phép tích hợp bên thứ ba với IP nhất quán
- **Không cần cập nhật DNS:** IP tồn tại qua các lần khởi động lại instance
- **Disaster Recovery:** Failover nhanh trong khi duy trì cùng một IP

#### Bảo mật SSL/TLS:

- Chứng chỉ được quản lý bởi ACM (tự động gia hạn)
- Hỗ trợ TLS 1.2/1.3
- Mã hóa end-to-end (Cloudflare → ALB → EC2)
- Perfect Forward Secrecy (PFS)
- Chỉ các cipher suites mạnh

#### Sẵn sàng mở rộng:

- Có khả năng mở rộng theo chiều ngang (hiện tại single EC2)
- Kiến trúc target group hỗ trợ nhiều instances
- Sẵn sàng Auto Scaling Group (triển khai trong tương lai)
- Connection draining cho graceful shutdowns

### Triển khai DevSecOps

#### Pipeline CI/CD tự động:

- Tích hợp GitHub cho source control
- CodePipeline cho orchestration (1 pipeline MIỄN PHÍ)
- CodeBuild cho automated builds (100 phút/tháng MIỄN PHÍ)
- Quét bảo mật tự động trước khi triển khai
- Triển khai zero-downtime qua ALB target groups
- Triển khai SSH đến Elastic IP để đảm bảo độ tin cậy
- Xác thực health check trước khi chuyển đổi traffic

#### Quét bảo mật:

- **Bandit:** Static Application Security Testing (SAST)
- **Safety:** Quét lỗ hổng dependency
- Cổng bảo mật trước khi triển khai
- Build thất bại với các vấn đề mức độ HIGH

#### Chiến lược triển khai:

- Triển khai blue-green sử dụng ALB target groups
- Xác thực health check (/health endpoint)
- Rollback tự động khi thất bại
- Connection draining trong quá trình triển khai
- Không ảnh hưởng đến khách hàng trong quá trình cập nhật

#### Quản lý Secrets:

- AWS Secrets Manager cho lưu trữ thông tin xác thực
- Mã hóa KMS cho tất cả secrets
- Hỗ trợ chính sách rotation (có thể cấu hình trong production)
- Không có thông tin xác thực trong code hoặc biến môi trường
- Chỉ truy cập dựa trên IAM role

#### Giám sát & Logging:

- CloudWatch cho logs và metrics ứng dụng
- CloudTrail cho API audit logging
- ALB access logs (tùy chọn, cho compliance)
- Custom metrics cho KPIs kinh doanh
- Cảnh báo tự động cho bất thường
- Lưu giữ logs 90 ngày

### Bảo mật hạ tầng:

#### Lớp mạng:

- **Security Groups:** HTTPS (443) từ ALB chỉ đến EC2
- **ALB Security Group:** HTTPS (443) từ Internet (0.0.0.0/0)
- **Truy cập SSH (22)** đến Elastic IP chỉ từ các IP quản lý

#### Lớp ứng dụng:

- Xác thực dựa trên JWT với xác thực IP
- Rate limiting (100 req/phút mỗi IP)
- Thực thi chính sách CORS

#### Lớp hạ tầng:

- AWS Shield Standard (bảo vệ DDoS)
- SSL/TLS termination tại ALB
- IAM roles với least privilege
- Mã hóa KMS cho data at rest
- Cô lập VPC

### Quản lý đa khách hàng

- Đăng ký khách hàng tự phục vụ với xác thực AWS key
- Thông tin xác thực được lưu trong Secrets Manager (mã hóa KMS)
- Hệ thống lập lịch task cho thu thập dữ liệu định kỳ
- Cô lập dữ liệu hoàn toàn
- Truy cập dashboard cho từng khách hàng
- Redis caching để tối ưu hiệu suất

### Hệ thống thông báo Email

- Xác thực email với tokens bảo mật (hết hạn 24h)
- Cảnh báo quan trọng qua AWS SES
- Tùy chọn thông báo có thể tùy chỉnh
- Templates email HTML
- Quản lý email trong trang Settings
- **Lưu ý:** Chế độ sandbox SES yêu cầu xác thực email trước khi gửi

### Giám sát hạ tầng

- Metrics theo thời gian thực từ 15+ dịch vụ AWS
- Lưu giữ dữ liệu lịch sử (30+ ngày)
- Redis caching (tối ưu hiệu suất)
- Giám sát EC2, S3, RDS, Lambda
- Dashboards sức khỏe dịch vụ
- Metrics ALB (số lượng request, latency, healthy targets)

### Tối ưu chi phí

- Theo dõi chi phí hàng ngày cho mỗi dịch vụ
- Xu hướng chi phí hàng tháng
- Khuyến nghị AWS Cost Explorer
- Cảnh báo ngân sách
- Phân tích chi phí cho từng khách hàng

### Giám sát bảo mật

- Hỗ trợ tích hợp GuardDuty (demo sử dụng các phát hiện mô phỏng)
- Lọc dựa trên mức độ nghiêm trọng
- Cảnh báo email cho các phát hiện quan trọng
- CloudTrail audit logging
- Theo dõi trạng thái tuân thủ

---

## 5. Triển khai kỹ thuật

### Stack công nghệ

#### Frontend:

- React 18 với Vite
- TanStack Query cho data fetching
- Recharts cho visualization
- Tailwind CSS cho styling

#### Backend:

- Python 3.12+ với FastAPI
- boto3 (AWS SDK)
- asyncio cho concurrent task execution
- Redis cho caching
- pytest cho testing
- uvicorn ASGI server

#### Hạ tầng:

- **Load Balancer:** Application Load Balancer (ALB)
- **Networking:** Elastic IP (static public IPv4)
- **Compute:** EC2 t3.micro (1 vCPU, 1GB RAM, Ubuntu 22.04 LTS)
- **Database:** DynamoDB (5 bảng, on-demand pricing)
- **Cache:** Redis (in-memory, localhost)
- **Networking:** VPC với public subnet, Internet Gateway

#### DevSecOps:

- GitHub (source control)
- CodePipeline (CI/CD orchestration)
- CodeBuild (automated builds)
- Bandit (SAST security scanner)
- Safety (dependency scanner)

#### Bảo mật:

- AWS Certificate Manager (ACM) - Chứng chỉ SSL/TLS
- AWS Secrets Manager (lưu trữ thông tin xác thực)
- AWS KMS (encryption keys)
- CloudTrail (audit logging)
- CloudWatch (giám sát & cảnh báo)
- Security Groups (network firewall)

#### Email:

- AWS SES (Simple Email Service)
- Templates email HTML
- Xác thực email với tokens
- Thông báo cảnh báo

---

## 6. Phân tích chi phí

**Lưu ý chi phí:** Ước tính dựa trên giá AWS us-east-1 (tháng 12 năm 2025) sử dụng AWS Pricing Calculator. Giả định: 10 khách hàng, khoảng thu thập 15 phút, lưu giữ dữ liệu 30 ngày.

### Chi phí năm 1 (Tối đa Free Tier)

| Dịch vụ AWS | Chi phí/Tháng | Ghi chú |
|------------|-----------|-------|
| Application Load Balancer | $16.20 | $0.0225/giờ + phí LCU |
| Elastic IP | $0 | MIỄN PHÍ khi gắn vào EC2 đang chạy |
| EC2 t3.micro (750h miễn phí) | $0 (12 tháng) | Free tier năm đầu |
| DynamoDB (25GB miễn phí) | $0-3 | Free tier + vượt mức |
| S3 (5GB miễn phí) | $0-1 | Backups + ALB logs (tùy chọn) |
| CloudWatch (10 metrics miễn phí) | $0-2 | Metrics bổ sung |
| Secrets Manager (1 secret) | $0.40 | Per secret/tháng |
| ACM Certificate | $0 | MIỄN PHÍ cho ALB/CloudFront |
| CodePipeline (1 pipeline miễn phí) | $0 | Pipeline đầu tiên miễn phí |
| CodeBuild (100 phút/tháng miễn phí) | $0 | Trong free tier |
| CloudTrail (1 trail miễn phí) | $0 | Management events miễn phí |
| KMS (20,000 requests miễn phí) | $0-1 | Chủ yếu trong free tier |
| SES (3,000 emails miễn phí) | $0 | Xác thực + cảnh báo |
| Data Transfer (1GB miễn phí) | $0-1 | ALB đến EC2 (không tính phí cùng AZ) |
| **TỔNG Năm 1** | **$39-49/tháng** | Hạ tầng cấp production |

### Chi phí năm 2+

| Dịch vụ AWS | Chi phí/Tháng | Ghi chú |
|------------|-----------|-------|
| Application Load Balancer | $16.20 | $0.0225/giờ + phí LCU |
| Elastic IP | $0 | MIỄN PHÍ khi gắn vào EC2 đang chạy |
| EC2 t3.micro | $8-10 | Giá on-demand |
| DynamoDB | $3-5 | Storage + operations |
| CloudWatch | $2-4 | Logs + metrics |
| Secrets Manager | $0.40 | Per secret/tháng |
| ACM Certificate | $0 | MIỄN PHÍ cho ALB |
| CodePipeline | $0 | Pipeline đầu tiên miễn phí |
| CodeBuild | $0 | Trong giới hạn free tier |
| CloudTrail | $0 | Management events miễn phí |
| KMS | $1-2 | Lưu trữ key + requests |
| SES | $2-5 | Gửi email |
| S3 | $1 | Backups + logs |
| Data Transfer | $1 | Outbound traffic |
| **TỔNG Năm 2+** | **$47-57/tháng** | Production với ALB |

### Chi phí mở rộng

#### Năm 1 (EC2 Free Tier):

- **10 khách hàng:** Nền tảng $39 + (10 × $0.59) = $44.90/tháng
- **20 khách hàng:** Nền tảng $39 + (20 × $0.59) = $50.80/tháng
- **50 khách hàng:** Nền tảng $39 + (50 × $0.59) = $68.50/tháng

#### Năm 2+ (EC2 trả phí):

- **10 khách hàng:** Nền tảng $47 + (10 × $0.59) = $52.90/tháng
- **20 khách hàng:** Nền tảng $47 + (20 × $0.59) = $58.80/tháng
- **50 khách hàng:** Nền tảng $47 + (50 × $0.59) = $76.50/tháng

**Lưu ý mở rộng:** Với EC2 t3.micro, nền tảng có thể hỗ trợ hiệu quả 10-20 khách hàng. Để mở rộng đến 50+ khách hàng, khuyến nghị nâng cấp lên các instances lớn hơn (t3.small/medium). Kiến trúc ALB làm cho việc mở rộng theo chiều ngang trở nên đơn giản bằng cách thêm instances vào target group.

### Tối ưu hóa trong tương lai:

- **Reserved Instances:** Tiết kiệm 30-40% trên EC2 (sau 12 tháng)
- **Savings Plans:** Giảm giá linh hoạt dựa trên cam kết
- **Spot Instances:** Cho các tác vụ nền không quan trọng (tiết kiệm 70-90%)
- **S3 Intelligent-Tiering:** Tối ưu chi phí tự động cho backups
- **Tối ưu CloudWatch Logs:** Điều chỉnh retention (7-90 ngày)

### Tổng đầu tư hạ tầng:

- **Năm 1:** $39-49/tháng (với free tier EC2 + Elastic IP)
- **Năm 2+:** $47-57/tháng (giá đầy đủ, Elastic IP vẫn MIỄN PHÍ)
- **Mỗi khách hàng:** $0.59/tháng (có thể mở rộng và dự đoán được)

Đây là giá trị đặc biệt cho một nền tảng SaaS đa khách hàng cấp production, highly available, an toàn với hạ tầng IP tĩnh.

---

## 7. Đánh giá rủi ro

| Rủi ro | Tác động | Xác suất | Giảm thiểu |
|------|--------|-------------|-----------|
| Vượt ngân sách | Trung bình | Thấp | Cảnh báo ngân sách AWS, Redis caching, DynamoDB on-demand, giám sát chi phí ALB, giữ Elastic IP được gắn (MIỄN PHÍ) |
| Chi phí ALB vượt ước tính | Trung bình | Thấp | Giám sát sử dụng LCU, tối ưu cho single AZ (dev), CloudWatch cost alarms |
| Phí Elastic IP | Thấp | Rất thấp | Giữ EC2 chạy 24/7, giám sát trạng thái gắn, CloudWatch alarm nếu bị tháo |
| EC2 downtime | Cao | Rất thấp | ALB health checks, systemd auto-restart, failover tự động, Elastic IP cho phép phục hồi nhanh, mục tiêu uptime 99.5% |
| Cấu hình sai ALB | Cao | Thấp | Xác thực health check, kiểm tra target group, xác thực triển khai CodeBuild |
| Chứng chỉ SSL hết hạn | Trung bình | Rất thấp | ACM auto-renewal (60 ngày trước khi hết hạn), CloudWatch alarms |
| Địa chỉ IP thay đổi | Thấp | Rất thấp | Elastic IP ngăn thay đổi IP, bền vững qua các lần khởi động lại |
| Mất truy cập SSH | Trung bình | Rất thấp | Elastic IP cung cấp SSH endpoint ổn định, truy cập dự phòng qua ALB |
| Bảo mật dữ liệu khách hàng | Cao | Thấp | Secrets Manager + KMS, IAM least privilege, audit logging, ALB access logs |
| Thất bại pipeline CI/CD | Trung bình | Thấp | Logic retry CodeBuild, rollback triển khai, cổng health check, SSH qua Elastic IP cho sửa thủ công |
| Thất bại Redis cache | Trung bình | Thấp | Giám sát systemd, auto-restart, giảm xuống DynamoDB một cách graceful |
| Hot partitions DynamoDB | Trung bình | Thấp | Thiết kế partition key hợp lý, sharding aws_account_id |
| Chi phí Secrets Manager | Trung bình | Trung bình | Giám sát sử dụng, đánh giá các mẫu secret chung trong tương lai |
| Vấn đề gửi email | Trung bình | Thấp | Giám sát AWS SES, logic retry, thông báo dự phòng |
| Lỗ hổng bảo mật trong deps | Cao | Trung bình | Scanner Safety trong CI/CD, cập nhật tự động, cảnh báo bảo mật |
| Scope creep | Trung bình | Cao | Định nghĩa MVP nghiêm ngặt, đóng băng tính năng tuần 8, kế hoạch Phase 2 |
| Health check false positives | Thấp | Thấp | Điều chỉnh ngưỡng health check (2 lần thất bại trước khi không khỏe mạnh) |

---

## 8. Kết quả mong đợi

### Sản phẩm kỹ thuật

#### Hạ tầng cấp Production:

- Application Load Balancer với SSL/TLS termination
- Elastic IP cho truy cập SSH ổn định và IP whitelisting (MIỄN PHÍ)
- Chứng chỉ được quản lý bởi ACM với gia hạn tự động
- Health checks với khả năng failover tự động
- Triển khai zero-downtime sử dụng target groups
- Sẵn sàng mở rộng theo chiều ngang (có khả năng multi-instance)
- SLA uptime 99.99% của ALB

#### Nền tảng DevSecOps:

- Pipeline CI/CD tự động (GitHub → CodePipeline → CodeBuild → EC2 qua Elastic IP)
- Quét bảo mật tự động (Bandit SAST, Safety dependency scan)
- Chiến lược triển khai blue-green qua ALB
- Giám sát CloudWatch và CloudTrail audit logging
- AWS Secrets Manager cho lưu trữ thông tin xác thực
- Mã hóa KMS cho tất cả dữ liệu nhạy cảm
- Lớp caching Redis trên EC2

#### SaaS đa khách hàng:

- Đăng ký khách hàng với xác thực email
- 5 bảng DynamoDB với cô lập dữ liệu hợp lý
- Hệ thống lập lịch task để thu thập dữ liệu (demo với 3-5 khách hàng)
- Dashboard giám sát theo thời gian thực cho mỗi khách hàng
- Hệ thống thông báo email
- Trang Settings với quản lý email/thông báo

#### Mục tiêu hiệu suất:

- **Thời gian phản hồi API:** Mục tiêu <200ms (Redis cache HIT), <2s (cache MISS)
- **Thời gian phản hồi ALB:** Thêm <50ms latency (SSL termination + routing)
- **Truy cập SSH:** Truy cập trực tiếp qua Elastic IP cho triển khai và khắc phục sự cố
- **Gửi email:** <30 giây (chế độ sandbox SES, yêu cầu xác thực email)
- **Thu thập dữ liệu:** Mỗi 15 phút cho mỗi khách hàng
- **Uptime nền tảng:** Mục tiêu 99.5%+ (SLA 99.99% ALB + EC2 với health checks)
- **Khoảng health check:** 30 giây (2 lần thất bại liên tiếp = không khỏe mạnh)
- **Thời gian triển khai:** <5 phút (zero downtime với blue-green)
- **Thời gian build:** Mục tiêu <5 phút (trong giới hạn free tier)

#### Bảo mật & Tuân thủ:

- Zero thông tin xác thực trong code hoặc biến môi trường
- Tất cả secrets được mã hóa với KMS
- Mã hóa SSL/TLS end-to-end (Cloudflare → ALB → EC2)
- Elastic IP ổn định cho truy cập SSH (không có thay đổi IP động)
- Xác thực email trước các cảnh báo quan trọng
- Audit trail API hoàn chỉnh (CloudTrail)
- ALB access logs có sẵn (tùy chọn, cho compliance)
- Cô lập dữ liệu giữa các khách hàng
- IAM roles least privilege
- Quét bảo mật tự động trong CI/CD

### Kết quả học tập

#### Thành thạo hạ tầng Production:

- Cấu hình và quản lý Application Load Balancer
- Phân bổ và quản lý Elastic IP (tối ưu chi phí)
- Quản lý chứng chỉ SSL/TLS với ACM
- Cấu hình health checks và target group
- Chiến lược triển khai zero-downtime
- Các mẫu kiến trúc high availability
- Bảo mật mạng với Security Groups
- Quản lý IP tĩnh cho sự ổn định production

#### Thành thạo DevSecOps:

- Thiết kế và triển khai pipeline CI/CD
- Quét bảo mật tự động (SAST, quét dependency)
- Triển khai blue-green
- Các phương pháp hay nhất về quản lý secrets
- Audit logging và tuân thủ
- Giám sát và cảnh báo
- Chiến lược triển khai tự động
- Triển khai dựa trên SSH qua IP ổn định

#### Thành thạo các dịch vụ AWS:

- DynamoDB nâng cao (multi-tenant, GSI, TTL)
- Cấu hình Application Load Balancer (ALB)
- Phân bổ Elastic IP và các phương pháp hay nhất
- Tích hợp AWS Certificate Manager (ACM)
- Tích hợp AWS Secrets Manager + KMS
- CloudWatch + CloudTrail
- CodePipeline + CodeBuild
- Tích hợp AWS SES
- IAM roles và policies
- VPC networking và Security Groups

#### Kiến trúc SaaS:

- Mô hình hóa dữ liệu đa khách hàng
- Điều phối tác vụ nền
- Quản lý thông tin xác thực an toàn
- Chiến lược caching (Redis)
- Hệ thống thông báo email
- Thiết kế hạ tầng có thể mở rộng
- Các mẫu hạ tầng IP tĩnh

#### Phát triển Full-Stack:

- Lập trình async FastAPI
- Quản lý state React
- Tích hợp Redis
- Phát triển ưu tiên bảo mật
- Thiết kế RESTful API
- Triển khai health check endpoint

### Giá trị Portfolio

Dự án này chứng minh:

#### Hạ tầng cấp Production

- Application Load Balancer cho high availability
- Elastic IP cho hạ tầng ổn định (tối ưu MIỄN PHÍ)
- SSL/TLS termination với chứng chỉ ACM
- Khả năng triển khai zero-downtime
- Health checks với failover tự động
- Sẵn sàng mở rộng theo chiều ngang

#### Triển khai DevSecOps

- Pipeline CI/CD tự động với quét bảo mật
- Chiến lược triển khai blue-green
- Quản lý secrets với AWS Secrets Manager + KMS
- Giám sát và logging toàn diện
- Triển khai tự động với xác thực health
- Triển khai SSH qua Elastic IP tĩnh

#### Kiến trúc doanh nghiệp

- Thiết kế SaaS đa khách hàng có thể mở rộng
- Hạ tầng có thể mở rộng (MVP đã test với 3-5, được thiết kế cho 10-50+ khách hàng)
- Lớp caching Redis
- Hệ thống lập lịch tác vụ nền
- Các mẫu bảo mật sẵn sàng production
- Hạ tầng tối ưu chi phí (Elastic IP MIỄN PHÍ)

#### Chuyên môn bảo mật

- Zero secrets trong code
- Mã hóa end-to-end (Cloudflare → ALB → EC2)
- Mã hóa KMS
- CloudTrail audit logging
- Quét lỗ hổng tự động
- Bảo mật đa lớp (network + application + infrastructure)
- IP ổn định cho truy cập SSH an toàn

#### Thành thạo AWS

- 17+ dịch vụ AWS được tích hợp (thêm ALB + Elastic IP + ACM)
- Thiết kế tối ưu chi phí (Elastic IP MIỄN PHÍ khi được gắn)
- Tối đa hóa free tier
- Các phương pháp hay nhất về IAM
- Kiến trúc high availability
- Quản lý IP tĩnh

#### Hiệu quả chi phí

- $39-49/tháng cho nền tảng (Năm 1)
- $0.59/khách hàng/tháng tăng thêm
- Redis caching để tối ưu chi phí đọc
- Elastic IP MIỄN PHÍ (tiết kiệm $3.65/tháng cho mỗi IP)
- Chi phí ALB hợp lý cho các tính năng cấp production

### Các yếu tố khác biệt chính:

- Hạ tầng cấp production với ALB, Elastic IP và ACM (không chỉ EC2 cơ bản)
- Khả năng triển khai zero-downtime (chiến lược blue-green)
- SLA uptime 99.99% qua ALB
- Hạ tầng IP ổn định với Elastic IP MIỄN PHÍ
- Tối ưu chi phí (Elastic IP được gắn = $0)
- Pipeline DevSecOps tự động với cổng bảo mật
- Bảo mật cấp production (Secrets Manager + KMS + ACM)
- CI/CD tự động với testing tự động
- Kiến trúc đa khách hàng với cô lập dữ liệu
- Giám sát và audit logging toàn diện
- Sẵn sàng mở rộng theo chiều ngang (chứng minh hiểu biết về tăng trưởng)

---

## 9. Kết luận

AWS Cloud Health Dashboard là một nền tảng SaaS đa khách hàng cấp production với các phương pháp DevSecOps tự động chứng minh:

### Hạ tầng Production

- Application Load Balancer cho high availability và quản lý traffic
- Elastic IP cho địa chỉ IP ổn định, bền vững (MIỄN PHÍ khi được gắn)
- SSL/TLS termination với AWS Certificate Manager
- Health checks với failover tự động
- Khả năng triển khai zero-downtime
- Sẵn sàng mở rộng theo chiều ngang
- SLA uptime 99.99% của ALB

### Triển khai DevSecOps

- Pipeline CI/CD tự động (CodePipeline + CodeBuild)
- Quét bảo mật trước mỗi triển khai
- Chiến lược triển khai blue-green
- Triển khai SSH qua Elastic IP ổn định
- Quản lý secrets với AWS Secrets Manager + KMS
- Giám sát toàn diện (CloudWatch + CloudTrail)

### Kiến trúc doanh nghiệp

- Thiết kế đa khách hàng có thể mở rộng (MVP đã test với 3-5, được thiết kế cho 10-50+ khách hàng)
- Hệ thống lập lịch task có thể mở rộng
- Redis caching cho hiệu suất
- Cô lập dữ liệu hoàn toàn
- Các mẫu hạ tầng sẵn sàng production
- Thiết kế tối ưu chi phí (Elastic IP MIỄN PHÍ)

### Thiết kế ưu tiên bảo mật

- Zero secrets trong code hoặc config files
- Mã hóa end-to-end (Cloudflare → ALB → EC2)
- Mã hóa KMS cho tất cả dữ liệu nhạy cảm
- CloudTrail audit logging
- Quét lỗ hổng tự động
- Kiến trúc bảo mật đa lớp
- Truy cập SSH ổn định qua Elastic IP

### Chuyên môn AWS

- Tích hợp sâu với 17+ dịch vụ AWS
- Hạ tầng tối ưu chi phí ($39-49/tháng Năm 1)
- Tối ưu chi phí Elastic IP (MIỄN PHÍ vs $3.65/tháng)
- Các phương pháp hay nhất về bảo mật
- Hệ thống thông báo email (SES)
- Kiến trúc high availability

### Nhạy bén kinh doanh

- Hoạt động hiệu quả chi phí với các tính năng cấp production
- Mô hình giá có thể mở rộng ($0.59/khách hàng/tháng)
- Các tính năng SaaS chuyên nghiệp
- ROI rõ ràng cho khách hàng
- Đầu tư hạ tầng hợp lý

### Lý do đầu tư:

Chi phí ~$16/tháng cho ALB + $0/tháng cho Elastic IP (MIỄN PHÍ) được lý do hóa một cách chiến lược:

- Chuyển đổi dự án từ demo thành portfolio piece cấp production
- Chứng minh các mẫu kiến trúc doanh nghiệp
- Cho phép triển khai zero-downtime
- Cung cấp SLA uptime 99.99%
- Thể hiện khả năng sẵn sàng mở rộng (quan trọng cho các vai trò DevOps/SRE)
- Chứng minh hiểu biết về chiến lược high availability và failover
- Elastic IP thêm sự ổn định production với chi phí bằng không
- Thể hiện kỹ năng tối ưu chi phí (giữ Elastic IP được gắn = MIỄN PHÍ)

**Thời gian:** 3 tháng | **Nhóm:** 4 người | **Ngân sách:** $39-49/tháng (Năm 1), $47-57/tháng (Năm 2+)

Dự án này chứng minh triển khai hạ tầng cấp production và các phương pháp DevSecOps, làm cho nó trở thành một portfolio piece hấp dẫn cho các vai trò cloud engineering, DevOps và SRE.

---

## Phụ lục

### A. Các dịch vụ sử dụng (17 dịch vụ AWS - Cấp Production):

1. **Application Load Balancer (ALB)** - Phân phối traffic & high availability
2. **Elastic IP** - Địa chỉ IPv4 public tĩnh (MIỄN PHÍ khi được gắn vào EC2 đang chạy)
3. **AWS Certificate Manager (ACM)** - Quản lý chứng chỉ SSL/TLS (MIỄN PHÍ cho ALB)
4. **EC2** (Compute)
5. **DynamoDB** (Database)
6. **AWS Secrets Manager** (Lưu trữ thông tin xác thực)
7. **AWS KMS** (Mã hóa)
8. **CloudWatch** (Giám sát)
9. **CloudTrail** (Audit logging)
10. **CodePipeline** (CI/CD)
11. **CodeBuild** (Automated builds)
12. **SES** (Email)
13. **S3** (Backup & ALB logs)
14. **VPC** (Networking)
15. **Internet Gateway** (Kết nối)
16. **Security Groups** (Firewall)
17. **IAM** (Quản lý truy cập)
18. **Shield Standard** (Bảo vệ DDoS - tự động)

### B. Cải tiến kiến trúc từ ALB + Elastic IP:

- **High Availability:** SLA uptime 99.99%
- **Triển khai Zero-Downtime:** Chiến lược blue-green với target groups
- **SSL/TLS Termination:** Giảm tải mã hóa từ EC2
- **Health Checks:** Khả năng failover tự động
- **Mở rộng theo chiều ngang:** Sẵn sàng thêm nhiều EC2 instances
- **Bảo vệ DDoS:** Tích hợp AWS Shield Standard
- **Hạ tầng IP ổn định:** Elastic IP (MIỄN PHÍ khi được gắn)
- **Quản lý SSH:** Truy cập đáng tin cậy qua Elastic IP cố định
- **IP Whitelisting:** Cho phép tích hợp bên thứ ba
- **Tối ưu chi phí:** Elastic IP MIỄN PHÍ (tiết kiệm $3.65/tháng)
- **Cấp Production:** Mẫu hạ tầng sẵn sàng cho doanh nghiệp

### C. Chiến thắng tối ưu chi phí:

- **Elastic IP:** $0/tháng (MIỄN PHÍ khi được gắn) vs $3.65/tháng (idle)
- **ACM Certificate:** $0/tháng (MIỄN PHÍ cho ALB) vs $0.75/tháng (chứng chỉ public)
- **Redis Caching:** Giảm 60-80% chi phí đọc DynamoDB
- **Free Tier:** EC2, CodePipeline, CodeBuild, S3, CloudWatch (Năm 1)
- **Kiến trúc hiệu quả:** Single ALB + Elastic IP phục vụ tất cả khách hàng

### D. GitHub Repository

https://github.com/Unvianpetronas/Cloud_health_dashboard

### E. Domain

cloudhealthdashboard.xyz (Cloudflare DNS + CDN)

### F. Thông tin liên hệ

- **Trưởng dự án:** Trương Quốc Tuấn
- **Email:** unviantruong26@gmail.com
- **WhatsApp:** +84 798806545

---

*Kết thúc tài liệu đề xuất*