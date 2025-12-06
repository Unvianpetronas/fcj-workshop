---
title: "Chia sẻ, đóng góp ý kiến"
date: "6 tháng 12, 2025"
weight: 700
chapter: false
pre: " <b> 7. </b> "
---

> Tại đây em xin chia sẻ những trải nghiệm cá nhân khi tham gia chương trình thực tập tại AWS Vietnam, đồng thời đóng góp ý kiến để giúp cải thiện chương trình cho các thực tập sinh tiếp theo.

---

## Đánh Giá Chung

### 1. Môi Trường Làm Việc

Môi trường làm việc tại AWS Vietnam vô cùng chuyên nghiệp nhưng vẫn rất thân thiện và hỗ trợ. Điều em ấn tượng nhất là văn hóa "learn by doing" - em được tin tưởng để tự thiết kế và triển khai một dự án production thực tế (AWS Cloud Health Dashboard) thay vì chỉ làm các task nhỏ lẻ.

**Điểm mạnh:**
- **Tự do sáng tạo:** Em được quyền tự đưa ra quyết định kiến trúc, lựa chọn công nghệ và triển khai theo cách em cho là tốt nhất, sau đó nhận feedback để cải thiện
- **Công cụ đầy đủ:** Được cung cấp đầy đủ tài nguyên AWS, tài liệu kỹ thuật chính thức và quyền truy cập vào các dịch vụ cloud cần thiết
- **Không gian học hỏi:** Mọi người luôn khuyến khích em thử nghiệm, thậm chí cho phép em "làm hỏng" để học hỏi từ lỗi sai
- **Giao tiếp mở:** Có thể thoải mái thảo luận về các quyết định kỹ thuật, không bị áp đặt giải pháp từ trên xuống


### 2. Sự Hỗ Trợ Của Mentor / Team Admin

Mentor của em thực sự xuất sắc trong việc cân bằng giữa hướng dẫn và để em tự khám phá. Thay vì đưa ra câu trả lời trực tiếp, mentor thường hỏi ngược lại các câu hỏi khiến em phải suy nghĩ sâu hơn về vấn đề.

### 3. Sự Phù Hợp Giữa Công Việc Và Chuyên Ngành Học

Dự án AWS Cloud Health Dashboard hoàn toàn phù hợp và thực sự nâng cao những gì em đã học ở trường.

**Kiến thức từ trường được áp dụng:**
- **Cơ sở dữ liệu (Database):** Áp dụng kiến thức về NoSQL để thiết kế DynamoDB schema tối ưu cho multi-tenant architecture
- **Lập trình Web:** Sử dụng kiến thức về REST API, HTTP methods, authentication để xây dựng backend FastAPI
- **An toàn thông tin:** Áp dụng encryption, hashing, JWT tokens - những khái niệm đã học ở môn Security
- **Kiến trúc phần mềm:** Thiết kế hệ thống phân tán, microservices thinking (mặc dù chưa full microservices)

**Kiến thức mới học được ngoài trường:**
- **Cloud-native architecture:** Cách thiết kế ứng dụng tận dụng đầy đủ sức mạnh của cloud
- **DevSecOps practices:** CI/CD pipeline, infrastructure as code, automated testing
- **Production debugging:** Cách debug issues trong môi trường production thực tế
- **Cost optimization:** Cân bằng giữa performance và cost - điều không được dạy ở trường
- **Multi-tenant security:** Isolation, data privacy, cross-account access - rất thực tế

**Tỷ lệ lý tưởng:**
- 40% kiến thức nền tảng từ trường
- 60% kiến thức mới, thực tế, production-grade

Đây là tỷ lệ lý tưởng giúp em không bị overwhelm nhưng vẫn học được nhiều thứ mới.

### 4. Cơ Hội Học Hỏi & Phát Triển Kỹ Năng

Đây là điểm em đánh giá cao nhất trong chương trình thực tập.

**Kỹ năng kỹ thuật (Technical Skills):**

*AWS Services Deep Dive:*
- **EC2:** Không chỉ start/stop instance mà hiểu về instance types, pricing models, placement strategies
- **DynamoDB:** Partition key design, GSI/LSI, query optimization, capacity planning
- **IAM:** Cross-account roles, trust relationships, least privilege principle
- **GuardDuty:** Security findings interpretation, threat detection
- **Cost Explorer:** API usage optimization, cost allocation tags

*Development Skills:*
- **Python/FastAPI:** Async programming, dependency injection, middleware, error handling
- **React:** Hooks, state management, component architecture, performance optimization
- **Git workflow:** Branch strategies, meaningful commits, code review process

*DevOps/Cloud Skills:*
- **CI/CD:** AWS CodePipeline, CodeBuild, automated deployment
- **Monitoring:** CloudWatch metrics, alarms, log analysis
- **Security:** Encryption (Fernet, KMS), secrets management, vulnerability scanning

**Kỹ năng mềm (Soft Skills):**

*Problem Solving:*
Em học được cách approach một vấn đề phức tạp:
1. Hiểu rõ vấn đề (ví dụ: cost quá cao)
2. Thu thập data (kiểm tra API calls, tính toán cost per call)
3. Phân tích root cause (gọi API quá thường xuyên)
4. Đề xuất multiple solutions (caching, alternative data sources, longer intervals)
5. Evaluate trade-offs (freshness vs cost)
6. Implement & monitor

*Technical Communication:*
- Viết documentation rõ ràng (README, architecture docs, API specs)
- Giải thích technical decisions (tại sao chọn DynamoDB thay vì RDS?)
- Present findings (cost analysis report, security audit results)

*Time Management:*
- Balance giữa nhiều tasks (bug fixes, new features, documentation)
- Prioritize dựa trên impact (security bugs > new features)
- Set realistic timelines


## Một Số Câu Hỏi Khác

### Điều em nghĩ công ty **cần cải thiện** cho các thực tập sinh sau?
**Em nghĩ là mình cần có những use case thực tế để mọi người có thể hình dung được 1 dự án thực tế nó như thế nào, cũng như có thể áp dụng vào project, security và optimize performance cũng như cost 

### Nếu giới thiệu cho bạn bè, em có **khuyên họ thực tập ở đây không**? Vì sao?

**Có, em sẽ strongly recommend - nhưng với một số caveats.**

**Ai em sẽ recommend:**

**1. Students muốn học THẬT SỰ, không chỉ "làm cho có CV"**

Nếu bạn chỉ muốn:
- Có tên FCJ trong CV
- Làm việc nhẹ nhàng
- 9-to-5 rồi về

→ Đây KHÔNG phải chỗ cho bạn.

Nhưng nếu bạn:
- Muốn build something real và meaningful
- Sẵn sàng đối mặt với challenges
- Muốn learn by doing, không ngại "làm hỏng"
- Passionate về cloud/AWS/technology

→ Đây là nơi PERFECT cho bạn.

**2. Students có foundation tốt hoặc learn nhanh**

Để thành công ở đây, bạn cần:
- **Programming fundamentals:** Python hoặc ít nhất 1 ngôn ngữ backend
- **Basic web development:** Hiểu HTTP, REST APIs, databases
- **Willingness to learn:** AWS có learning curve, phải sẵn sàng đọc nhiều docs
- **Problem-solving mindset:** Không ngại Google, debug, trial-and-error

Em không giỏi programming khi bắt đầu, nhưng em willing to learn → đó mới là quan trọng nhất.

**Lý do recommend:**

**Về Technical Growth:**
- 📈 Learn production-grade AWS architecture
- 📈 Hands-on với 17+ AWS services
- 📈 Real DevSecOps experience
- 📈 Portfolio project có thể show employers

**Về Soft Skills:**
- 💪 Decision-making under uncertainty
- 💪 Technical communication
- 💪 Problem-solving methodology
- 💪 Ownership mindset

**Về Career:**
- 🚀 AWS experience trên CV (rất valuable!)
- 🚀 Real production project để discuss in interviews
- 🚀 Understanding of cloud economics (cost optimization)
- 🚀 Network với AWS professionals

**Về Culture:**
- ❤️ Supportive mentorship
- ❤️ Safe environment to fail and learn
- ❤️ Meritocracy (good work được recognize)
- ❤️ Innovation encouraged


---

## Đề Xuất & Mong Muốn

### Bạn có đề xuất gì để cải thiện trải nghiệm trong kỳ thực tập?

*Đề xuất: Internal Knowledge Base*
- **Content cần có:**
    - Common architectural patterns used in team
    - Best practices cho AWS services team hay dùng
    - Troubleshooting guides (đã gặp vấn đề gì, fix thế nào)
    - Code examples và templates
- **Format:** Internal wiki hoặc Git repo
- **Maintain:** Mỗi intern contribute khi họ giải quyết được vấn đề


### Em có muốn tiếp tục chương trình này trong tương lai?

**Có, em rất muốn đòng hành cùng FCJ tới hết chương trình**

