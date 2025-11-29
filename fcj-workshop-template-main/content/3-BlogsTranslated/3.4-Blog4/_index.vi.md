---
title: "Blog 4"
date: "`r Sys.Date()`"
weight: 304
chapter: false
pre: " <b> 3.4. </b> "
---

# Đơn giản hóa các hoạt động AI với kiến trúc tham khảo Multi-Provider Generative AI Gateway

**Tác giả:** Dan Ferguson, Bobby Lindsey, Nick McCarthy, Chaitra Mathur, và Sreedevi Velagala  
**Ngày:** 21 tháng 11 năm 2025  
**Tags:** Amazon Bedrock, Amazon SageMaker, Intermediate (200), Open Source, Technical How-to

---

Khi các tổ chức ngày càng áp dụng khả năng AI trên các ứng dụng của họ, nhu cầu quản lý tập trung, bảo mật và kiểm soát chi phí truy cập AI model là một bước cần thiết trong việc mở rộng quy mô các giải pháp AI. Hướng dẫn Generative AI Gateway trên AWS giải quyết những thách thức này bằng cách cung cấp hướng dẫn cho một gateway thống nhất hỗ trợ nhiều AI provider trong khi cung cấp khả năng governance và monitoring toàn diện.

Generative AI Gateway là một reference architecture cho các doanh nghiệp muốn triển khai các giải pháp generative AI end-to-end có nhiều model, response được làm giàu dữ liệu, và khả năng agent theo cách tự lưu trữ (self-hosted). Hướng dẫn này kết hợp quyền truy cập model rộng rãi của Amazon Bedrock, trải nghiệm developer thống nhất của Amazon SageMaker AI, và khả năng quản lý mạnh mẽ của LiteLLM, đồng thời hỗ trợ khách hàng truy cập các model từ các model provider bên ngoài một cách an toàn và đáng tin cậy hơn.

LiteLLM là một open source project giải quyết các thách thức phổ biến mà khách hàng gặp phải khi triển khai generative AI workload. LiteLLM đơn giản hóa việc truy cập multi-provider model trong khi chuẩn hóa các yêu cầu vận hành production bao gồm cost tracking, observability, prompt management, và nhiều hơn nữa. Trong bài viết này, chúng tôi sẽ giới thiệu cách Multi-Provider Generative AI Gateway reference architecture cung cấp hướng dẫn để triển khai LiteLLM vào môi trường AWS cho quản lý và governance generative AI workload production.

## Thách thức: Quản lý multi-provider AI infrastructure

Các tổ chức xây dựng với generative AI đối mặt với một số thách thức phức tạp khi họ mở rộng quy mô các sáng kiến AI của mình:

**Provider fragmentation:** Các team thường cần truy cập vào các AI model khác nhau từ nhiều provider—Amazon Bedrock, Amazon SageMaker AI, OpenAI, Anthropic và những provider khác—mỗi provider có API, phương pháp authentication và billing model khác nhau.

**Decentralized governance model:** Không có điểm truy cập thống nhất, các tổ chức gặp khó khăn trong việc triển khai các security policy nhất quán, usage monitoring và cost control trên các AI service khác nhau.

**Operational complexity:** Quản lý nhiều access paradigm từ AWS Identity and Access Management role đến API key, rate limit theo model cụ thể, và failover strategy trên các provider tạo ra overhead hoạt động và tăng rủi ro gián đoạn service.

**Cost management:** Việc hiểu và kiểm soát chi tiêu AI trên nhiều provider và team trở nên ngày càng khó khăn, đặc biệt khi usage tăng quy mô.

**Security và compliance:** Việc tạo điều kiện cho các security policy nhất quán và audit trail trên các AI provider khác nhau đặt ra những thách thức đáng kể cho enterprise governance.

## Multi-Provider Generative AI Gateway reference architecture

Hướng dẫn này giải quyết những thách thức phổ biến của khách hàng bằng cách cung cấp một gateway tập trung trừu tượng hóa sự phức tạp của nhiều AI provider đằng sau một interface duy nhất, được quản lý.
![ML-19752-image-1.png](/images/3-blog/4-image/ML-19752-image-1.png)

Được xây dựng trên các AWS service và sử dụng open source LiteLLM project, các tổ chức có thể sử dụng giải pháp này để tích hợp với các AI provider trong khi duy trì control, security và observability tập trung.
![ML-19752-image-2.png](/images/3-blog/4-image/ML-19752-image-2.png)

## Flexible deployment option trên AWS

Multi-Provider Generative AI Gateway hỗ trợ nhiều deployment pattern để đáp ứng các nhu cầu tổ chức đa dạng:

### Amazon ECS deployment

Đối với các team ưa thích containerized application với managed infrastructure, ECS deployment cung cấp serverless container orchestration với automatic scaling và integrated load balancing.

### Amazon EKS deployment

Các tổ chức có chuyên môn Kubernetes hiện có có thể sử dụng tùy chọn EKS deployment, cung cấp full control về container orchestration trong khi hưởng lợi từ managed Kubernetes control plane. Khách hàng có thể triển khai cluster mới hoặc tận dụng các cluster hiện có cho deployment.

Reference architecture được cung cấp cho các deployment option này phải tuân theo các security testing bổ sung dựa trên các yêu cầu security cụ thể của tổ chức bạn. Thực hiện security testing và review bổ sung khi cần thiết trước khi triển khai bất cứ thứ gì vào production.

## Network architecture option

Multi-Provider Generative AI Gateway hỗ trợ nhiều network architecture option:

### Global Public-Facing Deployment

Đối với các AI service có user base toàn cầu, kết hợp gateway với Amazon CloudFront và Amazon Route 53. Configuration này cung cấp:

- Enhanced security với AWS Shield DDoS protection
- Simplified HTTPS management với Amazon CloudFront default certificate
- Global edge caching để cải thiện latency
- Intelligent traffic routing trên các region

### Regional direct access

Đối với single-Region deployment ưu tiên low latency và cost optimization, direct access đến Application Load Balancer (ALB) loại bỏ CloudFront layer trong khi duy trì security thông qua security group và network ACL được cấu hình đúng cách.

### Private internal access

Các tổ chức yêu cầu cô lập hoàn toàn có thể triển khai gateway trong private VPC mà không có internet exposure. Configuration này đảm bảo rằng AI model access vẫn trong network perimeter an toàn của bạn, với ALB security group hạn chế traffic chỉ đến authorized private subnet CIDR.

## Comprehensive AI governance và management

Multi-Provider Generative AI Gateway được xây dựng để cho phép các governance standard AI mạnh mẽ từ một administrative interface đơn giản. Ngoài policy-based configuration và access management, người dùng có thể cấu hình các capability nâng cao như load-balancing và prompt caching.

### Centralized administration interface

Generative AI Gateway bao gồm một web-based administrative interface trong LiteLLM hỗ trợ quản lý toàn diện việc sử dụng LLM trên tổ chức của bạn.

Các capability chính bao gồm:

**User và team management:** Cấu hình access control ở mức granular, từ individual user đến toàn bộ team, với role-based permission phù hợp với cấu trúc tổ chức của bạn.

**API key management:** Quản lý và rotate API key tập trung cho các connected AI provider trong khi duy trì audit trail của key usage và access pattern.

**Budget control và alerting:** Đặt spending limit trên các provider, team và individual user với automated alert khi threshold được tiếp cận hoặc vượt quá.

**Comprehensive cost control:** Chi phí bị ảnh hưởng bởi AWS infrastructure và LLM provider. Mặc dù khách hàng có trách nhiệm cấu hình giải pháp này để đáp ứng yêu cầu chi phí của họ, khách hàng có thể xem lại các cost setting hiện có để được hướng dẫn thêm.

**Hỗ trợ nhiều model provider:** Tương thích với Boto3, OpenAI và LangGraph SDK, cho phép khách hàng sử dụng model tốt nhất cho workload bất kể provider nào.

**Hỗ trợ Amazon Bedrock Guardrails:** Khách hàng có thể tận dụng guardrail được tạo trên Amazon Bedrock Guardrails cho generative AI workload của họ, bất kể model provider nào.

## Intelligent routing và resilience

Các cân nhắc phổ biến xung quanh model deployment bao gồm model và prompt resiliency. Những yếu tố này quan trọng để xem xét cách xử lý failure khi phản hồi prompt hoặc truy cập data store.

**Load balancing và failover:** Gateway triển khai routing logic tinh vi phân phối request trên nhiều model deployment và tự động fail over sang backup provider khi phát hiện vấn đề.

**Retry logic:** Cơ chế retry tích hợp với exponential back-off tạo điều kiện cho service delivery đáng tin cậy ngay cả khi các provider riêng lẻ gặp phải transient issue.

**Prompt caching:** Intelligent caching giúp giảm chi phí bằng cách tránh duplicate request đến các expensive AI model trong khi duy trì response accuracy.

## Advanced policy management

Model deployment architecture có thể dao động từ đơn giản đến rất phức tạp. Multi-Provider Generative AI Gateway có các advanced policy management tool cần thiết để duy trì governance posture mạnh mẽ.

**Rate limiting:** Cấu hình sophisticated rate limiting policy có thể thay đổi theo user, API key, model type hoặc time of day để tạo điều kiện cho resource allocation công bằng và giúp ngăn chặn abuse.

**Model access control:** Hạn chế truy cập vào các AI model cụ thể dựa trên user role, đảm bảo rằng các sensitive hoặc expensive model chỉ có thể truy cập bởi authorized personnel.

**Custom routing rule:** Triển khai business logic route request đến các provider cụ thể dựa trên tiêu chí như request type, user location, hoặc cost optimization requirement.

## Monitoring và observability

Khi AI workload phát triển để bao gồm nhiều component hơn, nhu cầu observability cũng vậy. Multi-Provider Generative AI Gateway architecture tích hợp với Amazon CloudWatch. Tích hợp này cho phép người dùng cấu hình vô số monitoring và observability solution, bao gồm các open-source tool như Langfuse.

### Comprehensive logging và analytics

Các gateway interaction được tự động log vào CloudWatch, cung cấp insight chi tiết về:

- Request pattern và usage trend trên các provider và team
- Performance metric bao gồm latency, error rate và throughput
- Cost allocation và spending pattern theo user, team và model type
- Security event và access pattern cho compliance reporting

### Built-in troubleshooting

Administrative interface cung cấp real-time log viewing capability để administrator có thể nhanh chóng chẩn đoán và giải quyết usage issue mà không cần truy cập CloudWatch trực tiếp.
![ML-19752-image-3.png](/images/3-blog/4-image/ML-19752-image-3.png)

## Amazon SageMaker integration để mở rộng model access

Amazon SageMaker giúp tăng cường Multi-Provider Generative AI Gateway guidance bằng cách cung cấp comprehensive machine learning system tích hợp liền mạch với gateway architecture. Bằng cách sử dụng Amazon SageMaker managed infrastructure cho model training, deployment và hosting, các tổ chức có thể phát triển custom foundation model hoặc fine-tune các model hiện có có thể được truy cập thông qua gateway cùng với các model từ các provider khác. Tích hợp này loại bỏ nhu cầu về separate infrastructure management trong khi tạo điều kiện cho governance nhất quán trên cả custom và third-party model. SageMaker AI model hosting capability mở rộng gateway model access để bao gồm self-hosted model, cũng như những model có sẵn trên Amazon Bedrock, OpenAI và các provider khác.

## Open source contribution của chúng tôi

Reference architecture này xây dựng dựa trên các contribution của chúng tôi cho LiteLLM open source project, tăng cường khả năng của nó cho enterprise deployment trên AWS. Các enhancement của chúng tôi bao gồm improved error handling, enhanced security feature và optimized performance cho cloud-native deployment.

## Bắt đầu

Multi-Provider Generative AI Gateway reference architecture có sẵn ngay hôm nay thông qua GitHub repository của chúng tôi, đầy đủ với:

- **Infrastructure-as-Code:** Amazon CloudFormation và AWS Cloud Development Kit (CDK) template cho automated deployment vào Amazon ECS cluster
- **Comprehensive documentation:** Step-by-step deployment guide và configuration example
- **Interactive workshop:** Trải nghiệm học tập hands-on để khám phá gateway capability
- **Detailed deployment guide:** Deployment blog trên AWS Builder Center

Code repository mô tả một số flexible deployment option để bắt đầu.

### Public gateway với global CloudFront distribution

Sử dụng CloudFront để cung cấp globally distributed, low-latency access point cho generative AI service của bạn. CloudFront edge location cung cấp content nhanh chóng cho người dùng trên toàn thế giới, trong khi AWS Shield Standard giúp bảo vệ chống lại DDoS attack. Đây là configuration được khuyến nghị cho public-facing AI service có global user base.

### Custom domain với CloudFront

Để có trải nghiệm branded hơn, bạn có thể cấu hình gateway để sử dụng custom domain name của riêng bạn, trong khi vẫn hưởng lợi từ performance và security feature của CloudFront. Option này lý tưởng nếu bạn muốn duy trì tính nhất quán với online presence của công ty bạn.

### Direct access qua public Application Load Balancer

Khách hàng ưu tiên low-latency hơn global distribution có thể chọn direct-to-ALB deployment, không có CloudFront layer. Architecture đơn giản hóa này có thể cung cấp cost saving, mặc dù nó yêu cầu cân nhắc thêm cho web application firewall protection.

### Private VPC-only access

Để có mức độ security cao, bạn có thể triển khai gateway hoàn toàn trong private VPC, cô lập khỏi public internet. Configuration này phù hợp để xử lý sensitive data hoặc triển khai internal-facing generative AI service. Access bị hạn chế đối với trusted network như VPN, Direct Connect, VPC peering hoặc AWS Transit Gateway.

## Tìm hiểu thêm và triển khai ngay hôm nay

Sẵn sàng đơn giản hóa multi-provider AI infrastructure của bạn? Truy cập complete solution package để khám phá interactive learning experience với step-by-step guidance mô tả từng bước của deployment và management process.

## Kết luận

Multi-Provider Generative AI Gateway là solution guidance nhằm giúp khách hàng bắt đầu làm việc trên các generative AI solution theo cách well-architected, trong khi tận dụng AWS environment của các service và complementary open-source package. Khách hàng có thể làm việc với các model từ Amazon Bedrock, Amazon SageMaker JumpStart hoặc third-party model provider. Operation và management của workload được thực hiện thông qua LiteLLM management interface, và khách hàng có thể chọn host trên ECS hoặc EKS dựa trên preference của họ.

Ngoài ra, chúng tôi đã công bố một sample tích hợp gateway vào agentic customer service application. Agentic system được orchestrate bằng LangGraph và deploy trên Amazon Bedrock AgentCore. LLM call được route thông qua gateway, cung cấp flexibility để test agent với các model khác nhau—dù được host trên AWS hay provider khác.

Guidance này chỉ là một phần của mature generative AI foundation trên AWS. Để đọc sâu hơn về các component của generative AI system trên AWS, xem "Architect a mature generative AI foundation on AWS", mô tả các component bổ sung của generative AI system.

## Về các tác giả

![Screenshot_2023-02-08_at_12-47-20_Dan_Ferguson_1_100x132.png](/images/3-blog/4-image/Screenshot_2023-02-08_at_12-47-20_Dan_Ferguson_1_100x132.png)**Dan Ferguson** là Sr. Solutions Architect tại AWS, có trụ sở tại New York, USA. Với tư cách là machine learning services expert, Dan làm việc để hỗ trợ khách hàng trên hành trình tích hợp ML workflow một cách hiệu quả, effective và bền vững.

![Bobby-LIndsey.jpg](/images/3-blog/4-image/Bobby-LIndsey.jpg)**Bobby Lindsey** là Machine Learning Specialist tại Amazon Web Services. Anh ấy đã làm việc trong công nghệ hơn một thập kỷ, trải qua nhiều công nghệ và nhiều vai trò khác nhau. Hiện tại anh ấy tập trung vào việc kết hợp background của mình trong software engineering, DevOps và machine learning để giúp khách hàng deliver machine learning workflow ở quy mô lớn. Trong thời gian rảnh rỗi, anh ấy thích đọc sách, nghiên cứu, đi bộ đường dài, đạp xe và chạy đường mòn.

![mccartni-archive-photo-1-1.jpeg](/images/3-blog/4-image/mccartni-archive-photo-1-1.jpeg)**Nick McCarthy** là Generative AI Specialist tại AWS. Anh ấy đã làm việc với các AWS client trên nhiều ngành công nghiệp khác nhau bao gồm healthcare, finance, thể thao, viễn thông và năng lượng để tăng tốc business outcome của họ thông qua việc sử dụng AI/ML. Ngoài công việc, anh ấy thích dành thời gian đi du lịch, thử các món ăn mới và đọc về khoa học và công nghệ. Nick có bằng Cử nhân về Astrophysics và bằng Thạc sĩ về Machine Learning.

![Chaitra-Headshot.jpeg](/images/3-blog/4-image/Chaitra-Headshot.jpeg)**Chaitra Mathur** là GenAI Specialist Solutions Architect tại AWS. Cô ấy làm việc với khách hàng trên các ngành công nghiệp trong việc xây dựng scalable generative AI platform và operationalize chúng. Trong suốt sự nghiệp của mình, cô ấy đã chia sẻ chuyên môn của mình tại nhiều conference và đã viết một số blog trong lĩnh vực Machine Learning và Generative AI.

![Sree-Pic-100x95.png](/images/3-blog/4-image/Sree-Pic-100x95.png)**Sreedevi Velagala** là Solution Architect trong World-Wide Specialist Organization Technology Solutions team tại Amazon Web Services, có trụ sở tại New Jersey. Cô ấy đã tập trung vào việc cung cấp tailored solution và guidance phù hợp với nhu cầu độc đáo của clientele đa dạng trên các lĩnh vực AI/ML, Compute, Storage, Networking và Analytics. Cô ấy đã đóng vai trò quan trọng trong việc giúp khách hàng tìm hiểu cách AWS có thể giảm compute cost cho machine learning workload bằng cách sử dụng Graviton, Inferentia và Trainium. Cô ấy tận dụng deep technical knowledge và industry expertise của mình để cung cấp tailored solution phù hợp với business need và requirement độc đáo của mỗi client.

---
