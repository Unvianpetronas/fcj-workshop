---
title: "Sự kiện 4"
date: "`r Sys.Date()`"
weight: 404
chapter: false
pre: " <b> 4.4. </b> "
---

# BÁO CÁO SỰ KIỆN: DEVOPS TRÊN AWS


**Mục tiêu Sự kiện:** Cung cấp kiến thức toàn diện về Văn hóa DevOps, các nguyên tắc, chỉ số hiệu suất và bộ dịch vụ AWS DevOps để xây dựng CI/CD pipeline, IaC và Observability.

---

## PHIÊN BUỔI SÁNG (8:30 AM – 12:00 PM): CI/CD & INFRASTRUCTURE AS CODE

### 1. Chào mừng & Tư duy DevOps (8:30 – 9:00 AM)

- Sự kiện bắt đầu với tóm tắt nhanh từ phiên AI/ML trước đó, thiết lập bối cảnh để đưa các mô hình/ứng dụng vào production.
- Trọng tâm là văn hóa và nguyên tắc DevOps, nhấn mạnh sự hợp tác giữa Development (Dev) và Operations (Ops).
- Giới thiệu các chỉ số hiệu suất chính như **DORA** (Deployment Frequency, Lead Time for Changes, MTTR, Change Failure Rate) và **MTTR** (Mean Time To Recovery).

### 2. Dịch vụ AWS DevOps – Xây dựng CI/CD Pipeline (9:00 – 10:30 AM)

Phần này đi sâu vào bộ công cụ AWS CodeFamily để tự động hóa Continuous Integration (CI) và Continuous Deployment (CD):

- **Source Control:** Sử dụng AWS CodeCommit và thảo luận về các chiến lược quản lý source code như GitFlow và Trunk-based.
- **Build & Test:** Cấu hình CodeBuild để tự động hóa biên dịch và chạy các pipeline testing.
- **Deployment:** Phân tích các chiến lược triển khai nâng cao với CodeDeploy, bao gồm Blue/Green, Canary và Rolling updates.
- **Orchestration:** Sử dụng CodePipeline để tự động hóa toàn bộ quá trình từ commit đến production.
- **Demo:** Trình diễn trực quan một CI/CD pipeline hoàn chỉnh trên AWS.

### 3. Infrastructure as Code (IaC) (10:45 AM – 12:00 PM)

IaC là nền tảng của DevOps hiện đại, đảm bảo tính nhất quán và khả năng tái tạo môi trường:

- **AWS CloudFormation:** Học về Templates, Stacks và cách sử dụng Drift Detection để xác định sự khác biệt giữa trạng thái thực tế và source code.
- **AWS CDK (Cloud Development Kit):** Giới thiệu CDK như một cách tiếp cận hiện đại hơn, sử dụng các ngôn ngữ lập trình quen thuộc (TypeScript, Python,...) để định nghĩa hạ tầng thông qua Constructs và các mẫu tái sử dụng.
- **Demo & Thảo luận:** Trình diễn triển khai hạ tầng sử dụng cả CloudFormation và CDK, cùng với thảo luận về tiêu chí lựa chọn công cụ IaC phù hợp.

---

## PHIÊN BUỔI CHIỀU (1:00 PM – 5:00 PM): CONTAINER, OBSERVABILITY & BEST PRACTICES

### 4. Dịch vụ Container trên AWS (1:00 – 2:30 PM)

- **Kiến thức cơ bản Docker:** Ôn tập các khái niệm cơ bản liên quan đến Microservices và Containerization.
- **Amazon ECR (Elastic Container Registry):** Dịch vụ lưu trữ container images, bao gồm các tính năng quét image và lifecycle policies.
- **Amazon ECS & EKS:** So sánh hai dịch vụ điều phối container chính: ECS (đơn giản hơn, tích hợp AWS) và EKS (dựa trên Kubernetes). Phân tích các chiến lược triển khai và mở rộng.
- **AWS App Runner:** Giải pháp đơn giản hóa để triển khai container, tập trung vào code hơn là hạ tầng.
- **Demo & Case Study:** Minh họa triển khai kiến trúc microservices và so sánh các dịch vụ.

### 5. Monitoring & Observability (2:45 – 4:00 PM)

- **CloudWatch:** Công cụ cốt lõi để thu thập metrics, logs, alarms và dashboards trên toàn bộ hệ sinh thái AWS.
- **AWS X-Ray:** Cung cấp Distributed Tracing để theo dõi luồng request qua các microservices, giúp xác định bottlenecks và vấn đề hiệu suất.
- **Demo:** Thiết lập hệ thống observability toàn diện (Full-stack observability).
- **Best Practices:** Thực hành tốt nhất cho Alerting, dashboards và quy trình on-call.

### 6. DevOps Best Practices & Case Studies (4:00 – 4:45 PM)

- **Chiến lược Triển khai Nâng cao:** Thảo luận về Feature flags và A/B testing trong deployment pipeline.
- Automated testing và tích hợp sâu với CI/CD.
- **Incident Management:** Quản lý sự cố và tầm quan trọng của Postmortems để học hỏi và cải tiến.
- **Case Studies:** Bài học kinh nghiệm từ các startup và doanh nghiệp lớn đã trải qua chuyển đổi DevOps thành công.

### 7. Q&A & Kết thúc (4:45 – 5:00 PM)

- Giải đáp các câu hỏi chuyên sâu.
- Cung cấp thông tin về con đường sự nghiệp DevOps và các chứng chỉ AWS liên quan.

---

## PHÂN TÍCH CHI TIẾT CÁC PHIÊN

### Buổi sáng: CI/CD & Infrastructure as Code (8:30 AM – 12:00 PM)

| Thời gian | Trọng tâm Phiên | Kiến thức Chính & Dịch vụ AWS được Đề cập |
|-----------|-----------------|-------------------------------------------|
| 8:30 – 9:00 AM | Văn hóa & Tư duy DevOps | Thiết lập bối cảnh từ phiên AI/ML trước đó. Nhấn mạnh sự hợp tác (Dev/Ops). Giới thiệu DORA (Deployment Frequency, Lead Time, MTTR, CFR) và các chỉ số hiệu suất MTTR. |
| 9:00 – 10:30 AM | Xây dựng CI/CD Pipeline với AWS CodeFamily | Tự động hóa Continuous Integration/Deployment. Thảo luận về Source Control (AWS CodeCommit, GitFlow/Trunk-based). Cấu hình CodeBuild cho biên dịch/testing. Chiến lược triển khai nâng cao (Blue/Green, Canary, Rolling updates) với CodeDeploy. Điều phối toàn bộ quy trình thông qua CodePipeline. Phiên bao gồm demo thực tế. |
| 10:45 AM – 12:00 PM | Infrastructure as Code (IaC) | Tầm quan trọng của IaC cho tính nhất quán và khả năng tái tạo. Khám phá chi tiết AWS CloudFormation (Templates, Stacks, Drift Detection). Giới thiệu cách tiếp cận hiện đại với AWS CDK (Cloud Development Kit) sử dụng ngôn ngữ lập trình, Constructs và các mẫu tái sử dụng. Phiên bao gồm demo và thảo luận về tiêu chí lựa chọn công cụ. |

### Buổi chiều: Container, Observability & Best Practices (1:00 PM – 5:00 PM)

| Thời gian | Trọng tâm Phiên | Kiến thức Chính & Dịch vụ AWS được Đề cập |
|-----------|-----------------|-------------------------------------------|
| 1:00 – 2:30 PM | Dịch vụ Container trên AWS | Ôn tập Microservices và kiến thức cơ bản Docker. Lưu trữ và quản lý sử dụng Amazon ECR (quét image, lifecycle policies). So sánh các dịch vụ điều phối: Amazon ECS (tích hợp) vs. EKS (Kubernetes). Tóm tắt về AWS App Runner cho triển khai đơn giản hóa. Phiên bao gồm demo và case study. |
| 2:45 – 4:00 PM | Monitoring & Observability | Nguyên tắc cốt lõi của full-stack observability. CloudWatch để thu thập metrics, logs, alarms và dashboards. AWS X-Ray cho Distributed Tracing để xác định vấn đề hiệu suất và bottlenecks giữa các dịch vụ. Thực hành tốt nhất cho alerting và quy trình on-call. Phiên bao gồm demo toàn diện. |
| 4:00 – 4:45 PM | DevOps Best Practices & Case Studies | Thảo luận về các kỹ thuật triển khai nâng cao (Feature flags, A/B testing). Tầm quan trọng của tích hợp Automated Testing. Chiến lược cho Incident Management và học hỏi tổ chức thông qua Postmortems. Xem xét các chuyển đổi DevOps thành công trong doanh nghiệp. |
| 4:45 – 5:00 PM | Q&A và Kết thúc | Giải đáp câu hỏi của người tham dự. Cung cấp hướng dẫn về con đường sự nghiệp DevOps và các chứng chỉ AWS được đề xuất. |

---

## ĐÁNH GIÁ CHUNG VÀ ỨNG DỤNG

### Đánh giá

Sự kiện bao gồm toàn bộ vòng đời DevOps, từ văn hóa, quy trình CI/CD, quản lý hạ tầng dưới dạng code, đến vận hành container và observability. Kiến thức được truyền đạt rõ ràng, kết hợp lý thuyết (Nguyên tắc DevOps, chỉ số DORA) và công cụ thực tế (CodePipeline, CDK, X-Ray).

### Kiến thức Chính Thu được

- Thành thạo các công cụ AWS CodeFamily để thiết lập các CI/CD pipeline tự động.
- Hiểu rõ ưu nhược điểm của CloudFormation và CDK cho IaC.
- Khả năng phân biệt và áp dụng các dịch vụ container (ECS, EKS).
- Thiết lập hệ thống monitoring chủ động sử dụng CloudWatch và X-Ray.
- Thành thạo trong việc thiết lập hệ thống distributed tracing cho kiến trúc microservices.

### Các Bước Tiếp theo Đề xuất

1. **Thực hành CI/CD:** Sử dụng CodeCommit, CodeBuild và CodePipeline để tự động hóa triển khai cho dự án hiện tại (nếu có) hoặc một ứng dụng mẫu.

2. **Khám phá CDK:** Bắt đầu sử dụng AWS CDK để định nghĩa hạ tầng dự án thay vì Console, tập trung vào việc tạo các Constructs đơn giản. Bắt đầu di chuyển định nghĩa hạ tầng từ sử dụng console thủ công sang code.

3. **Triển khai Observability:** Tích hợp CloudWatch Logs và thử nghiệm với AWS X-Ray để theo dõi các function call (API calls) trong dự án để áp dụng các nguyên tắc monitoring ngay từ đầu. Thiết lập monitoring chủ động và distributed tracing cho microservices hoặc API calls trong môi trường phát triển hiện tại.

## Hình ảnh Sự kiện
![Event4.1.jpg](/images/4-event/Event4.1.jpg)
![Event4.2.jpg](/images/4-event/Event4.2.jpg)
![Event4.3.jpg](/images/4-event/Event4.3.jpg)