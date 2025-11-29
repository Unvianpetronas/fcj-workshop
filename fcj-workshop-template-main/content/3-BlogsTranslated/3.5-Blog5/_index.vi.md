---
title: "Blog 5"
date: "`r Sys.Date()`"
weight: 305
chapter: false
pre: " <b> 3.6. </b> "
---

# Cách Smartsheet tăng năng suất developer với Amazon Bedrock và Roo Code

**Tác giả:** Rony Blum và JB Brown  
**Ngày:** 19 tháng 5 năm 2025  
**Tags:** Amazon Bedrock, Best Practices, Customer Solutions, Generative AI, Technical How-to

---

Bài viết này được đồng viết với JB Brown từ Smartsheet.

Các tổ chức thường tìm kiếm các cách để tăng cường năng suất developer trong khi duy trì hiệu quả chi phí. AI-powered coding assistant là các giải pháp hiệu quả để tăng tốc development cycle, nhưng việc triển khai các giải pháp này ở quy mô lớn trong khi quản lý chi phí đặt ra những thách thức độc đáo. Roo Code, một AI-powered autonomous coding agent nằm trực tiếp trong editor của developer, đại diện cho sự phát triển mới nhất trong AI-assisted development. Nó giúp developer code nhanh hơn và thông minh hơn bằng cách cung cấp các capability từ code generation và refactoring đến documentation writing và code base analysis thông qua natural language interaction. Bài viết này khám phá cách Smartsheet triển khai thành công Roo Code với Amazon Bedrock và Anthropic's Claude, đạt được những cải thiện đáng kể về hiệu quả developer trong khi tối ưu hóa chi phí thông qua các caching strategy sáng tạo.

Amazon Bedrock prompt caching capability, đã có sẵn rộng rãi vào tháng 4 năm 2025, đại diện cho một bước tiến đáng kể trong việc tối ưu hóa AI model interaction. Với tính năng này, các tổ chức có thể cache các prompt thường được sử dụng trên nhiều API call, giảm chi phí lên đến 90% và giảm latency lên đến 85%. Công nghệ này đặc biệt có giá trị cho các application sử dụng context tương tự nhiều lần, chẳng hạn như coding assistant cần duy trì context về code file. Prompt caching hỗ trợ nhiều foundation model (FM), bao gồm Anthropic's Claude 3.5 Haiku và Anthropic's Claude 3.7 Sonnet, cũng như Amazon Nova family of model. Cached context vẫn có sẵn trong tối đa 5 phút sau mỗi lần truy cập, với mỗi cache hit reset countdown này. Đối với các tổ chức triển khai AI-assisted development workflow, họ có thể sử dụng capability này để cân bằng performance với cost-efficiency.

## Lợi ích của Amazon Bedrock prompt caching

Smartsheet là một leading cloud-based enterprise work management service, cho phép hàng triệu người dùng trên toàn thế giới lập kế hoạch, quản lý, theo dõi, tự động hóa và báo cáo công việc ở quy mô lớn. Engineering team của họ đã xác định được cơ hội để tăng cường năng suất developer bằng cách triển khai AI-assisted coding capability trên toàn tổ chức. Giải pháp cần mở rộng quy mô hiệu quả trong khi duy trì chi phí hợp lý, dẫn đến việc họ chọn Roo Code với Amazon Bedrock với Anthropic's Claude làm FM provider của họ.

> "Engineering team của chúng tôi đã đạt được giảm 60% operational cost và cải thiện 20% response latency khi sử dụng Roo Code với Amazon Bedrock prompt caching. Những cải thiện này trực tiếp chuyển thành năng suất developer được nâng cao trên toàn tổ chức của chúng tôi," JB Brown, VP of Engineering tại Smartsheet nói.

Thực tế này được nhấn mạnh bởi khả năng của team để nhanh chóng tạo ra comprehensive code documentation và architecture diagram chỉ trong 4 giờ, một nhiệm vụ trước đây tiêu tốn 2 tuần manual effort. Hiệu quả mới này mở rộng đến các lĩnh vực quan trọng như AWS cost management, nơi một vital analysis tool được xây dựng chỉ trong 30 phút và dự kiến sẽ tiết kiệm cho Smartsheet $450,000 hàng năm. Hơn nữa, tốc độ và khả năng tiếp cận thông tin đã được khuếch đại đáng kể, như được minh chứng bởi 6–8 giờ senior engineer time được tiết kiệm bằng cách nhanh chóng giải thích complex Terraform/Terragrunt infrastructure. Những ví dụ này nêu bật cách Amazon Bedrock prompt caching không chỉ là về incremental gain, mà là về việc mở khóa substantial time và resource saving, trao quyền cho developer của chúng tôi tập trung vào innovation thay vì các time-consuming manual process.

## Tổng quan giải pháp

Khi triển khai AI-assisted coding ở quy mô lớn, Smartsheet đối mặt với một số thách thức chính xung quanh việc quản lý chi phí liên quan đến large language model (LLM) API call, giảm latency cho developer interaction và xử lý repetitive element trong prompt một cách hiệu quả. Development team nhận ra rằng một phần đáng kể của các prompt được gửi đến Amazon Bedrock là các repeated element: system prompt và continuously building message history, đưa ra cơ hội cho optimization.

Để giải quyết những thách thức này, Smartsheet đã triển khai một comprehensive solution tập trung xung quanh Roo Code integration với Amazon Bedrock và prompt caching. Smartsheet đã triển khai Roo Code, powered by Amazon Bedrock và Anthropic's Claude 3.7, trên toàn tổ chức và tích hợp liền mạch vào existing development workflow. Điều này cung cấp cho developer quyền truy cập ngay lập tức vào AI assistance cho code writing, reviewing và debugging task.

Team sau đó đã đóng góp một sophisticated Amazon Bedrock prompt caching system cho Roo Code mà xác định và lưu trữ common prompt element, giảm đáng kể redundant data transmission đến Bedrock. Caching layer này tỏ ra quan trọng trong việc tối ưu hóa cả cost và performance metric. Caching decision flow kết hợp hai key check: đầu tiên, nó xác minh nếu selected model hỗ trợ prompt caching, sau đó nó đảm bảo system prompt đáp ứng minimum token threshold để làm cho caching có giá trị.

Giải pháp tích hợp Roo Code trực tiếp vào integrated development environment (IDE) của developer, sử dụng Amazon Bedrock prompt caching capability. Architecture phân tách content thành static element (như system instruction và code context) và dynamic element (như user query). Khi một developer thực hiện request, static content được tự động đánh dấu với cache checkpoint và lưu trữ trong 5 phút, với mỗi lần truy cập làm mới window này.

Đối với subsequent query, system kiểm tra cached content khớp trước khi xử lý. Khi tìm thấy match, nó bỏ qua recomputation của các section này, giảm đáng kể cả response time và cost.

Sơ đồ sau đây cho thấy các integration point giữa Roo Code và Amazon Bedrock.

![roocache-arch.jpg](/images/3-blog/5-image/roocache-arch.jpg)

Screenshot sau đây nêu bật các model provider setting và connection parameter cần thiết cho implementation.
![Roo-UI.png](/images/3-blog/5-image/Roo-UI.png)

## Tối ưu hóa performance với prompt caching

Khi phân tích code file, phần lớn context—chẳng hạn như file content và environment detail—vẫn nhất quán trên nhiều query. Bằng cách cache content này, subsequent request có thể tái sử dụng nó từ cache, giảm đáng kể cost và latency.

Trong ví dụ của chúng tôi, đầu tiên chúng tôi yêu cầu Roo Code phân tích một Python file. Sau đó chúng tôi đã gửi nhiều follow-up query về các khía cạnh khác nhau của code. Query đầu tiên giải thích overall structure, tiếp theo là các câu hỏi về specific class, unused module và potential improvement.

Usage section của các response cho thấy significant improvement thông qua caching. Trong query đầu tiên, model xử lý input và ghi cached content. Đối với subsequent query, 90% của input được phục vụ từ cache. Bởi vì cache read rẻ hơn 90% so với xử lý new input token, điều này dẫn đến giảm 83% cost cho follow-up query.

Đây là cách caching hoạt động trên các query:

- **Query đầu tiên** – Model xử lý entire file content và environment detail, ghi chúng vào cache
- **Subsequent query** – Model load cached content, chỉ xử lý new question text

Để minh họa điều này, hãy xem usage statistic từ một loạt query:

**Query 1: "Explain my @/src/models/product.py code"**
```json
{
    "inputTokens": 1051,
    "outputTokens": 902,
    "totalTokens": 19374,
    "cacheReadInputTokenCount": 0,
    "cacheWriteInputTokenCount": 12421
}
```

**Query 2: "Describe what is ProductUpdate class used for"**
```json
{
    "inputTokens": 1078,
    "outputTokens": 694,
    "totalTokens": 21664,
    "cacheReadInputTokenCount": 19892,
    "cacheWriteInputTokenCount": 0
}
```

**Query 3: "Which module is not in use?"**
```json
{
    "inputTokens": 4,
    "outputTokens": 774,
    "totalTokens": 22753,
    "cacheReadInputTokenCount": 19892,
    "cacheWriteInputTokenCount": 2083
}
```

**Query 4: "Any improvement to implement to my code making it more efficient?"**
```json
{
    "inputTokens": 1097,
    "outputTokens": 1270,
    "totalTokens": 24342,
    "cacheReadInputTokenCount": 21975,
    "cacheWriteInputTokenCount": 0
}
```

Chúng ta có thể thấy từ kết quả rằng query đầu tiên không trả về `cacheReadInputTokenCount` nhưng đã ghi vào cache toàn bộ input. Đối với subsequent query, hầu hết input được đọc từ cache, ngoại trừ specific question.

Kết quả rõ ràng chứng minh lợi ích của prompt caching:

- 70% của total input được phục vụ từ cache trên các query
- 90% của input cho follow-up query đến từ cache
- Overall input cost giảm 60%
- Follow-up query cost giảm 83%

Cách tiếp cận này đặc biệt hiệu quả cho development workflow nơi developer thường xuyên query cùng code base với các câu hỏi khác nhau. Cached content cung cấp necessary context cho mỗi query trong khi giảm đáng kể processing overhead.

Screenshot sau đây hiển thị Roo Code's interface, cho thấy detailed metric bao gồm token usage, cache hit rate và associated API cost. Interface trình bày cost saving đạt được thông qua caching và cung cấp comprehensive usage analytics.

![RooBlog3.png](/images/3-blog/5-image/RooBlog3.png)

## Kết quả và innovation tương lai

Line graph sau đây cho thấy cost reduction trend qua nhiều query run, có query đầu tiên uncached và subsequent input sử dụng significant query response từ cache. Trục y cho thấy cost tính bằng dollar và trục x cho thấy cost per query. Graph cho thấy clear downward trend, với cost giảm 60% từ baseline.

![graph-1024x566.jpg](/images/3-blog/5-image/graph-1024x566.jpg)

Implementation mang lại remarkable improvement trên các key metric. Prompt caching system đạt được giảm 60% operational cost trong khi đồng thời cải thiện response latency 20%.

Lựa chọn Amazon Bedrock prompt caching tỏ ra đặc biệt hiệu quả cho coding assistance workflow của Smartsheet. 5-minute cache Time to Live (TTL) phù hợp hoàn hảo với natural rhythm của developer interaction với Roo Code, nơi nhiều related query thường xảy ra trong short timeframe. Trong active coding session, developer tham gia vào rapid back-and-forth exchange với AI assistant—reviewing code, hỏi follow-up question và yêu cầu modification—trong khi duy trì context thông qua cache. Agentic workflow này, nơi mỗi interaction xây dựng dựa trên previous context, tận dụng tối đa prompt caching mechanism—mỗi query làm mới cache timer trong khi bảo tồn valuable context về code đang được thảo luận.

## Best practice và bài học kinh nghiệm

Thông qua implementation journey của họ, Smartsheet đã phát triển một số key best practice cho các tổ chức muốn triển khai similar solution. Early implementation của prompt caching tỏ ra quan trọng, cho phép team phân tích prompt pattern và thiết kế efficient caching strategy ngay từ đầu. Team phát hiện rằng việc tập trung vào developer experience thông qua response time optimization và seamless tool integration là điều cần thiết cho successful adoption.

Continuous monitoring và analysis của usage pattern trở thành cornerstone của optimization strategy của họ. Bằng cách tracking key metric như response time và cost, team có thể xác định cơ hội cho further optimization và thường xuyên điều chỉnh caching strategy của họ để duy trì optimal performance.

## Nhìn về phía trước

Smartsheet tiếp tục innovate với Amazon Bedrock và Roo Code, khám phá các cách mới để tăng cường năng suất developer. Engineering team của họ đang điều tra advanced caching strategy và đánh giá new FM khi chúng có sẵn trên Amazon Bedrock. Thành công của implementation này đã thiết lập một nền tảng vững chắc cho các AI-assisted development initiative trong tương lai.

## Kết luận

Implementation của Smartsheet về Roo Code với Amazon Bedrock và prompt caching cho thấy cách các tổ chức có thể triển khai thành công AI-assisted coding solution trong khi duy trì cost-efficiency. Cách tiếp cận của họ cung cấp một blueprint cho những người khác muốn tăng cường năng suất developer thông qua AI trong khi tối ưu hóa operational cost. Sự kết hợp của strategic implementation, innovative caching solution và continuous optimization đã cho phép Smartsheet đạt được significant improvement trong performance và cost metric.

Để tìm hiểu thêm về việc triển khai AI solution ở quy mô lớn, tham khảo Amazon Bedrock documentation và khám phá AWS Machine Learning Blog. Các framework và strategy được nêu trong bài viết này có thể giúp các tổ chức có quy mô khác nhau triển khai efficient, cost-effective AI-assisted development workflow.

## Về các tác giả

![026A9027.jpg](/images/3-blog/5-image/026A9027.jpg)### Rony Blum

Rony Blum là Senior Solutions Architect tại AWS có trụ sở tại Seattle, làm việc với ISV customer để thiết kế và triển khai advanced cloud architecture, chuyên về SaaS solution, multi-tenant system và Generative AI application.

![JB_headshot.jpg](/images/3-blog/5-image/JB_headshot.jpg)### JB Brown

JB Brown là Vice President of Engineering tại Smartsheet, một AI-powered, enterprise-grade modern work management platform. Ông là một accomplished engineering executive với kinh nghiệm scaling global engineering team, thúc đẩy AI innovation và cung cấp scalable cloud technology. Trước Smartsheet, ông đã dành gần một thập kỷ tại Nordstrom, gần đây nhất là Senior Director of Mobile and Personalization Technology, nơi ông chịu trách nhiệm cung cấp customer-centric corporate strategy.

---

