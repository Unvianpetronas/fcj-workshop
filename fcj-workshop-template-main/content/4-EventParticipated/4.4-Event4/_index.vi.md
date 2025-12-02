---
title: "Sự kiện 4"
date: "`r Sys.Date()`"
weight: 404
chapter: false
pre: " <b> 4.4. </b> "
---


# TÓM TẮT SỰ KIỆN: AWS BEDROCK AGENT VÀ AGENTIC WORKFLOW

**Mục tiêu:** Sự kiện này đã thành công trong việc cung cấp một khám phá chuyên sâu về xây dựng và vận hành các quy trình làm việc tự động (Agentic Workflows) sử dụng AWS Bedrock Agent, được bổ sung bởi các chiến lược tối ưu hóa từ đối tác CloudThinker.

---

## TỔNG QUAN CHƯƠNG TRÌNH

Chương trình cấu trúc nội dung một cách logic, chuyển từ các khái niệm nền tảng đến các ứng dụng thực tế nâng cao:

| Thời gian | Chủ đề | Diễn giả | Trọng tâm |
|-----------|--------|----------|-----------|
| 9:00 - 9:10 | Khai mạc | Nguyễn Gia Hùng | Mục tiêu sự kiện và thiết lập bối cảnh. |
| 9:10 - 9:40 | AWS Bedrock Agent Core | Kiên Nguyễn | Phân tích kiến trúc, tính năng cốt lõi và cơ chế hoạt động. |
| 9:40 - 10:00 | [Use Case] Xây dựng Agentic Workflow trên AWS | Việt Phạm | Ví dụ thực tế về triển khai quy trình làm việc tự động. |
| 10:00 - 10:10 | Giới thiệu CloudThinker | Thắng Tôn | Giới thiệu công ty và giải pháp tối ưu hóa GenAI. |
| 10:10 - 10:40 | CloudThinker Optimization (L300) | Henry Bùi | Đi sâu vào Agentic Orchestration và Context Optimization trên Bedrock. |
| 10:40 - 11:00 | Giờ nghỉ trà & Kết nối | | |
| 11:00 - 12:00 | CloudThinker Hack: Workshop Thực hành | Khả Vân | Phiên thực hành xây dựng giải pháp Agentic. |
| 12:00 | Kết nối & Ăn trưa Buffet | | |

---

## KIẾN THỨC VÀ INSIGHTS CHÍNH

Các phiên đã cung cấp kiến thức toàn diện trên bốn lĩnh vực chính:

### 1. Kiến trúc Cốt lõi của AWS Bedrock Agent

- **Chức năng:** Bedrock Agent hoạt động như một công cụ quan trọng để tự động hóa các tác vụ phức tạp bằng cách kết nối thông minh Foundation Models (FM) với các API và hệ thống kinh doanh.
- **Cơ chế:** Agent sử dụng khả năng suy luận của FM để phân tích yêu cầu người dùng, xác định các bước cần thiết và thực hiện các bước đó bằng cách gọi Tools (Action Groups).
- **Giá trị:** Nó giảm đáng kể nỗ lực phát triển cho các quy trình làm việc nhiều bước trong khi đảm bảo truy cập an toàn vào các hệ thống nội bộ thông qua các guardrails bảo mật tích hợp sẵn.

### 2. Ứng dụng Agentic Workflow Thực tế

- Use Case của anh Việt Phạm đã minh họa sự chuyển đổi từ các quy trình tuần tự, thủ công sang các quy trình làm việc tự động, được điều khiển bởi Agent.
- Điều kiện tiên quyết quan trọng cho sự thành công là định nghĩa chính xác các khả năng hệ thống và API để Agent có thể hiểu và sử dụng một cách chính xác.

### 3. Tối ưu hóa Workflows với CloudThinker

CloudThinker đã giải quyết các thách thức vận hành nâng cao (L300) thông qua hai khái niệm chính:

- **Agentic Orchestration:** Chiến lược quản lý nhiều Agent hoặc các tác vụ phức tạp, nhiều bước để đảm bảo hiệu suất và tính nhất quán.
- **Context Optimization:** Các kỹ thuật nâng cao (ví dụ: tối ưu hóa RAG, quản lý context window) để tối ưu hóa ngữ cảnh được cung cấp cho FM, từ đó giảm chi phí token và nâng cao độ chính xác suy luận của Agent.

Những tối ưu hóa này rất cần thiết để triển khai các ứng dụng Agentic đáng tin cậy và hiệu quả về chi phí ở cấp độ doanh nghiệp.

### 4. Trải nghiệm Thực hành Chuyên sâu

- Workshop **CloudThinker Hack**, do anh Khả Vân hướng dẫn, cho phép người tham dự trực tiếp cấu hình và triển khai một Agent cơ bản trên Amazon Bedrock.
- Người tham gia có cơ hội quý báu để áp dụng các kỹ thuật tối ưu hóa của CloudThinker về orchestration và context vào quy trình làm việc họ xây dựng, củng cố kiến thức lý thuyết và làm nổi bật các thách thức kỹ thuật trong production.

---

## KẾT LUẬN VÀ CÁC BƯỚC TIẾP THEO ĐỀ XUẤT

### Đánh giá

Sự kiện rất thành công, cung cấp một đợt khám phá sâu về Bedrock Agent, một tính năng mạnh mẽ của AWS GenAI, từ các nguyên tắc nền tảng đến các tiến bộ cấp độ L300. Sự kết hợp giữa nội dung cốt lõi của AWS và các giải pháp tối ưu hóa thực tế từ CloudThinker, đặc biệt là thông qua phiên thực hành, đã mang lại giá trị thực tế đáng kể.

### Các Bước Tiếp theo Đề xuất

1. **Thử nghiệm Thực tế:** Xây dựng một Agent thử nghiệm để tự động hóa một chức năng kinh doanh đơn giản (ví dụ: gọi API nội bộ hoặc truy vấn DynamoDB) để củng cố hiểu biết về Action Groups.

2. **Nghiên cứu Nâng cao:** Đi sâu hơn vào các kỹ thuật tối ưu hóa context và prompt engineering nâng cao (như được trình bày bởi CloudThinker) để có khả năng tích hợp vào các dự án GenAI hiện tại.

3. **Khám phá MLOps:** Điều tra các thực hành tốt nhất để giám sát và quản lý vòng đời của Bedrock Agents (AgentOps), tập trung vào quy trình cập nhật Foundation Models và Tools.