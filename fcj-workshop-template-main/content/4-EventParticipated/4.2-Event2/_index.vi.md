---
title: "Sự kiện 2"
date: "`r Sys.Date()`"
weight: 402
chapter: false
pre: " <b> 4.2. </b> "
---


# Báo cáo Tóm tắt: "Amazon Q Developer cho SAP ABAP"

### Mục tiêu Sự kiện

- Giới thiệu về SAP ABAP
- Cách Amazon Q tác động với SAP ABAP
- Cách prompt Amazon Q
- Thực hành Lab

### Diễn giả

- **Jignesh Shah** – Director, Open Source Databases
- **Erica Liu** – Sr. GTM Specialist, AppMod
- **Fabrianne Effendi** – Assc. Specialist SA, Serverless Amazon Web Services

### Điểm nổi bật chính

#### Xác định những hạn chế của kiến trúc ứng dụng legacy

- Chu kỳ phát hành sản phẩm dài → Mất doanh thu/bỏ lỡ cơ hội
- Vận hành không hiệu quả → Giảm năng suất, chi phí cao hơn
- Không tuân thủ các quy định bảo mật → Vi phạm bảo mật, mất uy tín

#### Chuyển đổi sang kiến trúc ứng dụng hiện đại – Microservices

Di chuyển sang hệ thống modular — mỗi chức năng là một **dịch vụ độc lập** giao tiếp qua **sự kiện**, được xây dựng trên ba trụ cột chính:

- **Quản lý Queue**: Xử lý các tác vụ bất đồng bộ
- **Chiến lược Caching**: Tối ưu hóa hiệu suất
- **Xử lý Message**: Giao tiếp linh hoạt giữa các dịch vụ

#### Domain-Driven Design (DDD)

- **Phương pháp bốn bước**: Xác định domain events → sắp xếp timeline → xác định actors → định nghĩa bounded contexts
- **Case study nhà sách**: Minh họa ứng dụng DDD trong thực tế
- **Context mapping**: 7 mẫu để tích hợp bounded contexts

#### Kiến trúc Event-Driven

- **3 mẫu tích hợp**: Publish/Subscribe, Point-to-point, Streaming
- **Lợi ích**: Loose coupling, khả năng mở rộng, khả năng phục hồi
- **So sánh Sync vs async**: Hiểu về các đánh đổi

#### Sự phát triển của Compute

- **Mô hình Trách nhiệm Chia sẻ**: EC2 → ECS → Fargate → Lambda
- **Lợi ích Serverless**: Không quản lý server, auto-scaling, trả tiền theo giá trị
- **Functions vs Containers**: Tiêu chí để lựa chọn phù hợp

#### Amazon Q Developer

- **Tự động hóa SDLC**: Từ lập kế hoạch đến bảo trì
- **Chuyển đổi Code**: Nâng cấp Java, hiện đại hóa .NET
- **AWS Transform agents**: Di chuyển VMware, Mainframe, .NET

### Những điểm chính rút ra

#### Tư duy Thiết kế

- **Cách tiếp cận Business-first**: Luôn bắt đầu từ domain kinh doanh, không phải công nghệ
- **Ngôn ngữ chung**: Tầm quan trọng của từ vựng chung giữa nhóm kinh doanh và kỹ thuật
- **Bounded contexts**: Xác định và quản lý độ phức tạp trong hệ thống lớn

#### Kiến trúc Kỹ thuật

- **Kỹ thuật Event storming**: Phương pháp thực tế để mô hình hóa quy trình kinh doanh
- Sử dụng **giao tiếp event-driven** thay vì các cuộc gọi đồng bộ
- **Mẫu tích hợp**: Khi nào sử dụng sync, async, pub/sub, streaming
- **Phổ Compute**: Tiêu chí để lựa chọn giữa VM, containers và serverless

#### Chiến lược Hiện đại hóa

- **Cách tiếp cận theo giai đoạn**: Không vội vàng — tuân theo lộ trình rõ ràng
- **Framework 7Rs**: Nhiều con đường hiện đại hóa tùy thuộc vào ứng dụng
- **Đo lường ROI**: Giảm chi phí + tính linh hoạt kinh doanh

### Áp dụng vào Công việc

- **Áp dụng DDD** vào các dự án hiện tại: Các phiên event storming với nhóm kinh doanh
- **Tái cấu trúc microservices**: Sử dụng bounded contexts để định nghĩa ranh giới dịch vụ
- **Triển khai mẫu event-driven**: Thay thế một số cuộc gọi sync bằng async messaging
- **Áp dụng serverless**: Thử nghiệm AWS Lambda cho các trường hợp sử dụng phù hợp
- **Thử Amazon Q Developer**: Tích hợp vào quy trình phát triển để tăng năng suất

### Trải nghiệm Sự kiện

Tham dự workshop **"GenAI-powered App-DB Modernization"** vô cùng có giá trị, cho tôi cái nhìn toàn diện về việc hiện đại hóa ứng dụng và cơ sở dữ liệu bằng các phương pháp và công cụ tiên tiến. Những trải nghiệm chính bao gồm:

#### Học hỏi từ các diễn giả có kỹ năng cao
- Các chuyên gia từ AWS và các tổ chức công nghệ lớn đã chia sẻ **thực hành tốt nhất** trong thiết kế ứng dụng hiện đại.
- Thông qua các case study thực tế, tôi hiểu sâu hơn về việc áp dụng **DDD** và **Kiến trúc Event-Driven** vào các dự án lớn.

#### Tiếp xúc kỹ thuật thực hành
- Tham gia các phiên **event storming** giúp tôi hình dung cách **mô hình hóa quy trình kinh doanh** thành các domain events.
- Học cách **tách microservices** và định nghĩa **bounded contexts** để quản lý độ phức tạp của hệ thống lớn.
- Hiểu về đánh đổi giữa **giao tiếp đồng bộ và bất đồng bộ** và các mẫu tích hợp như **pub/sub, point-to-point, streaming**.

#### Tận dụng công cụ hiện đại
- Khám phá **Amazon Q Developer**, công cụ AI hỗ trợ SDLC từ lập kế hoạch đến bảo trì.
- Học cách **tự động hóa chuyển đổi code** và thử nghiệm serverless với **AWS Lambda** để cải thiện năng suất.

#### Kết nối và thảo luận
- Workshop tạo cơ hội trao đổi ý tưởng với các chuyên gia, đồng nghiệp và nhóm kinh doanh, nâng cao **ngôn ngữ chung** giữa kinh doanh và kỹ thuật.
- Các ví dụ thực tế củng cố tầm quan trọng của **cách tiếp cận business-first** thay vì chỉ tập trung vào công nghệ.

#### Bài học rút ra
- Áp dụng DDD và mẫu event-driven giảm **coupling** đồng thời cải thiện **khả năng mở rộng** và **khả năng phục hồi**.
- Hiện đại hóa đòi hỏi **cách tiếp cận theo giai đoạn** với **đo lường ROI**; vội vàng có thể gây rủi ro.
- Các công cụ AI như Amazon Q Developer có thể **tăng năng suất** đáng kể khi được tích hợp vào quy trình làm việc hiện tại.

#### Một số hình ảnh sự kiện
![Event2.1 (1).jpg](/images/4-event/Event2.1%20%281%29.jpg)
![Event2.1 (2).jpg](/images/4-event/Event2.1%20%282%29.jpg)