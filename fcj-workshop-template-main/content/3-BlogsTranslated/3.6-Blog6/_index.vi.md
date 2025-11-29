---
title: "Blog 6"
date: "`r Sys.Date()`"
weight: 306
chapter: false
pre: " <b> 3.6. </b> "
---

# Sử dụng generative AI trên AWS để phân tích tài liệu lâm sàng hiệu quả

**Tác giả:** Alex Boudreau và John O'Donnell  
**Ngày:** 05 tháng 2 năm 2025  
**Tags:** Amazon Bedrock, Amazon Comprehend Medical, Amazon Machine Learning, Amazon SageMaker, Best Practices, Customer Solutions, Generative AI

---

Clinical trial liên quan đến việc thu thập và xử lý một lượng lớn dữ liệu được quản lý chặt chẽ, bao gồm các complex protocol document mô tả cách trial sẽ được thực hiện. Quản lý khối lượng thông tin này có thể rất choáng ngợp, nhưng generative AI cung cấp một giải pháp bằng cách giúp tự động hóa quy trình và cho phép clinical researcher nhanh chóng tập trung vào thông tin có liên quan nhất. Hiện tại, drug approval process trung bình mất 10–12 năm, với clinical trial study startup time chiếm 1 năm trong khung thời gian đó. Phần lớn thách thức với study startup nằm ở tính chất complex và non-standard của protocol document. Những document này thường yêu cầu nhiều tuần hoặc tháng để review và assess. Review time này làm tăng thêm chu kỳ đã dài để đưa một loại thuốc mới ra thị trường.

Trong bài viết này, chúng tôi cho thấy cách Clario sử dụng AWS platform để tăng tốc clinical document analysis.

## Về Clario

Clario là leading provider của endpoint data solution cho clinical trials industry cung cấp regulatory-grade clinical evidence cho pharmaceutical, biotech và medical device partner. Kể từ khi Clario được thành lập hơn 50 năm trước, endpoint data solution của họ đã hỗ trợ clinical trial hơn 26,000 lần với hơn 700 regulatory approval trên hơn 100 quốc gia. Một trong những critical challenge mà Clario đối mặt là time-consuming process của việc tạo documentation cho clinical trial, có thể mất nhiều tuần hoặc tháng.

## Thách thức kinh doanh

Clinical trial là điều cần thiết cho việc phê duyệt các health innovation mới, bao gồm treatment, procedure và medical device. Chúng yêu cầu thu thập một lượng lớn complex data từ các dispersed clinical trial site để hỗ trợ đánh giá medical benefit và risk, tất cả trong khi duy trì privacy và regulatory compliance. Để làm cho vấn đề trở nên thách thức hơn nữa, việc capturing data trong clinical trial xảy ra không chỉ trong healthcare center mà còn thông qua remote capture thông qua nhiều khía cạnh của daily activity của trial participant.

Các partner như Clario hiểu những thách thức mà life sciences company phải đối mặt khi phân tích large volume của complex clinical document, chẳng hạn như study protocol. Những document này thường chứa hỗn hợp structured và unstructured data, bao gồm table, image và diagram, khiến việc chính xác interpret và extract key information ở quy mô lớn trở nên khó khăn. Trong bài viết này, chúng tôi khám phá cách Clario đã sử dụng sức mạnh của generative AI trên AWS để phân tích clinical document một cách hiệu quả và thúc đẩy outcome tốt hơn cho client của mình.

## Khai thác sức mạnh của large language model

Sự tiến bộ nhanh chóng trong large language model (LLM) đã mở rộng các potential application của natural language processing vượt ra ngoài simple conversational AI assistant. Clario đã thử nghiệm với nhiều technique khác nhau, chẳng hạn như zero-shot learning, few-shot learning, classification, entity extraction và summarization, để sử dụng hiệu quả LLM trong specialized use case. Bằng cách sử dụng prompt engineering, AI orchestration và content retrieval, Clario có thể hướng dẫn các model để chính xác tạo ra insight và extract relevant information từ key clinical research document, bao gồm complex clinical trial protocol.

## Bốn trụ cột của effective document analysis trên AWS

Thông qua research và development effort của mình, Clario đã xác định bốn core pillar cho phép effective document analysis sử dụng generative AI trên AWS:

**Parsing** – Clario sử dụng AWS service như Amazon Textract và Amazon Comprehend để extract text, image và table từ clinical document, duy trì cả data privacy và security.

**Retrieval** – Bằng cách sử dụng embedding model và vector database như Amazon OpenSearch Service, Clario efficiently store và retrieve relevant information từ large document collection dựa trên similarity search. Team đã thử nghiệm với nhiều chunking và retrieval strategy khác nhau để tối ưu hóa accuracy và performance.

**Prompting** – Sử dụng technique như zero-shot và few-shot learning, Clario đã tăng cường accuracy của LLM để classify và extract information. AWS service như Amazon Bedrock đơn giản hóa việc thử nghiệm với các prompting strategy khác nhau và đánh giá model performance.

**Generation** – Clario cẩn thận xem xét các yếu tố như context size, reasoning capability và latency khi chọn LLM thích hợp để tạo structured output. AWS cung cấp một loạt pre-trained model và framework tích hợp liền mạch vào Clario's pipeline.

## Tổng quan giải pháp

Để giải quyết các unique challenge liên quan đến việc phân tích clinical document, Clario đã xây dựng một custom generative AI platform trên AWS. Platform này kết hợp một orchestration engine kết hợp nhiều LLM và deep learning model, cho phép nó extract key information một cách chính xác và ở quy mô lớn. Bằng cách sử dụng AWS service như Amazon Elastic Compute Cloud (Amazon EC2), Amazon Elastic Kubernetes Service (Amazon EKS), Amazon Simple Storage Service (Amazon S3), SageMaker và AWS Lambda, Clario có thể efficiently process hàng ngàn document trong vài giây.

Sơ đồ sau đây minh họa solution architecture.

![ARCHBLOG-1078-clario3-imaging-and-alexb-alex-edit-v3-Page-3.drawio-Copy.png](/images/3-blog/6-image/ARCHBLOG-1078-clario3-imaging-and-alexb-alex-edit-v3-Page-3.drawio-Copy.png)

Workflow bao gồm các bước sau:

1. Document được thu thập on premises (1) và upload bằng AWS Direct Connect (2) với encryption in transit đến Amazon S3 (3). Tất cả uploaded document sau đó được tự động và an toàn lưu trữ với server-side object-level encryption.

2. Sau khi các document được upload và user đã review chúng, Clario AI Orchestration Engine (4) xác định best document parsing strategy dựa trên file type và extract text bằng Amazon Textract (5). Sau khi extract, text được vectorize và lưu trữ trong Amazon OpenSearch Service vector engine (6) để semantic retrieval sau này.

3. Sau vectorization, Clario AI Orchestration Engine (4), chạy như một distributed service trong Amazon EKS, khởi chạy một document classification async task bằng Amazon MQ. Amazon EC2 và Lambda được sử dụng cho additional processing nếu cần. Điều này trigger Document Classification Agent, sử dụng Amazon Bedrock LLM (8), để tự động xác định document type.

4. Sau khi các document được classify, Clario AI Orchestration Engine (4) khởi chạy appropriate document analysis agent cho further background processing. Trong trường hợp study protocol, engine khởi chạy Protocol Analysis agent, sử dụng predefined analysis graph configuration được lưu trữ trong Amazon Relational Database Service (Amazon RDS) (7), cũng như sự kết hợp của retrieval strategy và AI model, bao gồm custom deep learning model trên SageMaker (9) và pre-trained LLM trên Amazon Bedrock (8). Orchestration này cung cấp advanced document analysis, chuyển đổi massive amount của unstructured multi-modal data thành structured data và insight.

5. Sau analysis, tất cả structured data sau đó được persist vào Amazon RDS (7) để visualization, review và querying sau này.

## Recommendation và best practice

Dựa trên kinh nghiệm của họ trong việc phát triển và triển khai generative AI solution trên AWS, Clario đã học được các best practice sau:

- Áp dụng incremental và iterative development approach để dần dần xây dựng và tinh chỉnh model của bạn
- Tuân theo standard machine learning approach để đánh giá và validate model performance bằng representative test set
- Tối ưu hóa bốn pillar của document analysis trước khi đầu tư vào fine-tuning và continuous pre-training của LLM
- Điều chỉnh approach của bạn cho specific use case, bởi vì không phải tất cả các vấn đề đều yêu cầu cùng model hoặc technique

## Kết luận

Bằng cách sử dụng sức mạnh của generative AI trên AWS, Clario đã có thể efficiently phân tích complex clinical trial document và extract valuable insight cho client của mình trong life sciences industry. Thông qua sự kết hợp của careful model selection, iterative development và tuân thủ best practice, Clario đã xây dựng một scalable và accurate document analysis pipeline bằng AWS. Mở khóa full potential của clinical trial data của bạn bằng cách áp dụng các best practice này với AWS generative AI solution ngay hôm nay.

## Về các tác giả

![1624904528953.jpg](/images/3-blog/6-image/1624904528953.jpg)### Alex Boudreau

Alex Boudreau là Director of AI tại Clario. Ông dẫn dắt Generative AI department đổi mới của công ty và giám sát việc phát triển advanced multi-modal GenAI Platform của công ty, bao gồm cutting-edge cloud engineering, AI engineering và foundational AI research. Với distinguished career trong AI và machine learning, Alex trước đây đã pioneering Deep Learning speech analysis system cho automotive application, dẫn đầu cloud-based enterprise fraud detection solution, advanced conversational AI technology và groundbreaking project trong medical image analysis. Chuyên môn của ông trong việc leading high-impact initiative đặt ông ở vị trí độc đáo để thúc đẩy ranh giới của AI technology trong business world.

![1675893031202-1-1.jpg](/images/3-blog/6-image/1675893031202-1-1.jpg)### John O'Donnell

John O'Donnell là Principal Solutions Architect tại Amazon Web Services (AWS) nơi ông cung cấp CIO-level engagement và design cho complex cloud-based solution trong healthcare và life sciences (HCLS) industry. Với hơn 20 năm hands-on experience, ông có proven track record trong việc cung cấp value và innovation cho HCLS customer trên toàn cầu. Là một trusted technical leader, ông đã hợp tác với AWS team để dive deep vào customer challenge, đề xuất outcome và đảm bảo high-value, predictable và successful cloud transformation. John đam mê giúp HCLS customer đạt được mục tiêu của họ và tăng tốc cloud native modernization effort của họ.

---