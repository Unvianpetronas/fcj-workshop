---
title: "Blog 2"
date: "`r Sys.Date()`"
weight: 302
chapter: false
pre: " <b> 3.2. </b> "
---

# Di chuyển và hiện đại hóa hệ thống VMware với AWS Transform for VMware

*Tác giả: Kiran Reid, Ramu Akula, Patrick Kremer, và Mark Jaggers vào ngày 08 JUL 2025 trong [AWS for VMware](https://aws.amazon.com/blogs/architecture/category/compute/aws-for-vmware/), [AWS Transform](https://aws.amazon.com/blogs/architecture/category/artificial-intelligence/generative-ai/aws-transform/), [Generative AI](https://aws.amazon.com/blogs/architecture/category/artificial-intelligence/generative-ai/)*

Vào ngày 15 tháng 5 năm 2025, AWS đã công bố một giải pháp đột phá: [AWS Transform for VMware](https://aws.amazon.com/transform/vmware/). Dịch vụ sáng tạo này giải quyết trực tiếp những thách thức lâu dài của việc di chuyển lên đám mây, mở ra một kỷ nguyên mới với các chuyển đổi được tối ưu hóa và hiệu quả lên AWS Cloud. Bằng cách giảm đáng kể công việc thủ công và tăng tốc di chuyển các hệ thống VMware quan trọng, AWS Transform for VMware sẽ cách mạng hóa cách các tổ chức tiếp cận hành trình đám mây của họ.

Kể từ khi [ra mắt chính thức](https://aws.amazon.com/blogs/migration-and-modernization/new-capabilities-in-aws-transform-for-vmware/), AWS Transform for VMware đã khơi dậy sự nhiệt tình trong các ngành công nghiệp, với các tổ chức háo hức tận dụng khả năng của nó để tăng tốc các dự án di chuyển và hiện đại hóa hệ thống VMware. Khi chúng ta đi sâu vào những phức tạp của công nghệ chuyển đổi này, chúng ta sẽ khám phá cách AWS Transform for VMware không chỉ đơn giản hóa việc di chuyển, mà còn định hình lại cảnh quan của việc áp dụng đám mây và chuyển đổi số.

## Thách thức khi di chuyển VMware

Di chuyển các hệ thống doanh nghiệp lên đám mây không chỉ là một thách thức kỹ thuật – đó là một sự chuyển đổi kinh doanh đòi hỏi độ chính xác, tốc độ và ít gián đoạn nhất. Nhiều năm quy trình vận hành đã được thiết lập thường dẫn đến các môi trường phức tạp với cấu hình được ghi chép kém, thực hành bảo mật không nhất quán, và phụ thuộc nặng vào kiến thức của nhân viên. Các nhóm kỹ thuật phải điều hướng các mối phụ thuộc ứng dụng phức tạp, phối hợp với nhiều bên liên quan, và duy trì hoạt động kinh doanh liên tục trong khi thực hiện các dự án chuyển đổi này. Việc thiếu tài liệu toàn diện và hiểu biết rõ ràng về các mối phụ thuộc hệ thống thường dẫn đến việc kéo dài thời gian di chuyển và tăng rủi ro dự án. Ngoài ra, nhu cầu cân bằng các hoạt động đang diễn ra với các hoạt động di chuyển tạo ra những thách thức. Việc chuyển giao kiến thức đúng cách thêm một lớp phức tạp khác vào những sáng kiến quan trọng này.

## Tổng quan giải pháp

Hãy khám phá cách AWS Transform for VMware đơn giản hóa việc khám phá ứng dụng, tự động hóa chuyển đổi mạng và điều phối các việc di chuyển phức tạp thông qua kiến trúc toàn diện của nó trong sơ đồ sau.

![Để hiểu cách các khả năng này hoạt động cùng nhau, hãy xem xét từng thành phần của kiến trúc.](/images/3-blog/2-image/Screenshot-2025-07-06-at-18.42.20.png)

## Khám phá và đánh giá được tối ưu hóa

Hành trình bắt đầu với việc khám phá và đánh giá kỹ lưỡng môi trường VMware của bạn (1). [AWS Transform for VMware](https://aws.amazon.com/transform/vmware/) (4) hỗ trợ nhiều phương pháp khám phá. Một lựa chọn là [RVTools](https://www.robware.net/) để thu thập thông tin kho VMware. Đối với khách hàng đang chạy VMware NSX, có chức năng [nhập/xuất](https://aws.amazon.com/blogs/migration-and-modernization/exporting-network-configuration-data-with-import-export-for-nsx/) tùy chọn. Ngoài ra, [AWS Application Discovery Service](https://aws.amazon.com/application-discovery/) cung cấp cả tùy chọn khám phá có sử dụng phần mềm đại diện và không sử dụng phần mềm đại diện (2) để thu thập dữ liệu và các mối phụ thuộc cho việc di chuyển.

Khả năng Khám phá Kho (5) thu thập dữ liệu quan trọng từ môi trường nguồn của bạn và lưu trữ an toàn trong các thùng chứa [Amazon Simple Storage Service](https://aws.amazon.com/s3/) (Amazon S3) (12) trong Tài khoản Khám phá Di chuyển AWS (7). Dữ liệu này tạo nền tảng cho việc lập kế hoạch di chuyển có thông tin và được xử lý thêm bởi AWS Application Discovery Service (15) trong Tài khoản Lập kế hoạch Di chuyển AWS. AWS Transform hoạt động cùng với các dịch vụ này để cung cấp một nơi duy nhất theo dõi tiến trình di chuyển và thu thập dữ liệu kho máy chủ và phụ thuộc, điều này rất cần thiết cho việc nhóm ứng dụng và lập kế hoạch đợt thành công.

## Chuyển đổi mạng thông minh và lập kế hoạch theo đợt

Với sự hiểu biết toàn diện về môi trường của bạn, AWS Transform for VMware chuyển sang giai đoạn quan trọng tiếp theo. Khả năng Di chuyển Mạng (19) tự động hóa việc tạo các mẫu [AWS CloudFormation](https://aws.amazon.com/cloudformation/) (13, 26) để thiết lập hạ tầng mạng đích. Các mẫu này đảm bảo môi trường đám mây của bạn phản ánh chặt chẽ thiết lập nguồn của bạn, đơn giản hóa việc thiết lập cho di chuyển.

Trong khi đó, khả năng Lập kế hoạch Đợt (6) sử dụng [mạng nơ-ron đồ thị](https://en.wikipedia.org/wiki/Graph_neural_network) tiên tiến để phân tích các mối phụ thuộc ứng dụng và lập kế hoạch các đợt di chuyển tối ưu. Điều này giảm thiểu phân tích phụ thuộc danh mục đầu tư và ứng dụng phức tạp, và cung cấp các kế hoạch đợt sẵn sàng di chuyển, dẫn đến các lần di chuyển mượt mà.

## Bảo mật và tuân thủ nâng cao

Bảo mật vẫn là điều tối quan trọng trong suốt quá trình di chuyển. [AWS Key Management Service](https://aws.amazon.com/kms/) (AWS KMS) (8, 16, 26) cung cấp mã hóa mạnh mẽ cho dữ liệu được lưu trữ, lịch sử trò chuyện và các sản phẩm. Theo mặc định, các khóa được quản lý bởi AWS được sử dụng, với tùy chọn sử dụng [khóa được quản lý bởi khách hàng](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html) (CMK) để kiểm soát bổ sung.

[AWS Organizations](https://aws.amazon.com/organizations/) (9) cho phép quản lý tập trung trên nhiều tài khoản AWS, và [AWS CloudTrail](https://aws.amazon.com/cloudtrail/) (14, 26) ghi lại và lưu nhật ký các hoạt động API để có đường kiểm tra hoàn chỉnh. Kiểm soát truy cập được quản lý thông qua [AWS Identity and Access Management](https://aws.amazon.com/iam/) (IAM) (26), cung cấp quản lý truy cập tập trung trên các tài khoản AWS.

[Amazon CloudWatch](https://aws.amazon.com/cloudwatch/) (10, 26) liên tục giám sát các hoạt động dịch vụ AWS Transform, sử dụng tài nguyên và các chỉ số vận hành trong tài khoản quản lý, cung cấp khả năng hiển thị và kiểm soát đầy đủ trong suốt quá trình di chuyển. [AWS Identity Center](https://aws.amazon.com/iam/identity-center/) (11) tăng cường bảo mật hơn nữa bằng cách cung cấp quản lý truy cập tập trung trên tất cả các tài khoản AWS liên quan đến di chuyển.

## Thực hiện di chuyển được điều phối

Khi đến lúc thực hiện di chuyển, AWS Transform điều phối di chuyển từ đầu đến cuối bằng cách phối hợp giữa các công cụ và dịch vụ AWS khác nhau (20). [AWS Application Migration Service](https://aws.amazon.com/application-migration-service/) (25) sao chép các máy chủ từ môi trường nguồn của bạn đến các phiên bản [Amazon Elastic Compute Cloud](https://aws.amazon.com/ec2/) (Amazon EC2) (21) trong Tài khoản Đích Di chuyển AWS (18), dựa trên các đợt và nhóm được lập kế hoạch cẩn thận.

Phần mềm Sao chép AWS (2) hoạt động song song với AWS Application Migration Service để đảm bảo chuyển dữ liệu hiệu quả và đáng tin cậy. [Amazon Elastic Block Store](https://aws.amazon.com/ebs/) (Amazon EBS) (21) cung cấp lưu trữ cần thiết cho các máy ảo được di chuyển, đảm bảo hiệu suất và khả năng mở rộng tối ưu.

## Cấu hình mạng linh hoạt

AWS Transform for VMware cung cấp hai mô hình mạng để phù hợp với các yêu cầu khác nhau:

- **Mô hình Trung tâm và chấm** – [AWS Transit Gateway](https://aws.amazon.com/transit-gateway/) (23) kết nối các đám mây riêng ảo (VPC) thông qua một VPC trung tâm với các [cổng NAT](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html) được chia sẻ. Mô hình này lý tưởng cho quản lý tập trung và dịch vụ được chia sẻ.
- **Mô hình Độc lập** – Mỗi VPC hoạt động độc lập mà không có kết nối nào được thiết lập. Cách tiếp cận này được thiết kế cho khách hàng có hạ tầng mạng AWS hiện có, cho phép bạn kết nối thủ công các VPC mới với cấu trúc mạng hiện có của bạn.

Các VPC (22) được tạo bởi AWS Transform khớp với các phân đoạn mạng tại chỗ của bạn, cung cấp chuyển đổi liền mạch. Các cổng NAT (24) cung cấp truy cập internet đi ra cho các mạng con riêng tư, duy trì bảo mật trong khi cho phép kết nối cần thiết. Trong kiến trúc trung tâm và chấm, các cổng NAT tập trung trong VPC trung tâm có thể phục vụ nhiều VPC chấm, tối ưu hóa chi phí và đơn giản hóa quản lý. Đối với triển khai VPC độc lập, các cổng NAT chuyên dụng phải được cung cấp trong mỗi VPC yêu cầu truy cập internet. Trong tất cả các trường hợp, bạn phải cấu hình bảng định tuyến để cho phép luồng lưu lượng ra ngoài thông qua các cổng NAT.

Để có hướng dẫn thiết lập và yêu cầu hoàn chỉnh, tham khảo [Hướng dẫn Sử dụng AWS Transform](https://docs.aws.amazon.com/transform/latest/userguide/what-is-service.html).

## Các cân nhắc bổ sung

Các không gian làm việc khám phá AWS Transform for VMware có sẵn trên toàn cầu (3). Để có thông tin cập nhật nhất về các Vùng được hỗ trợ, tham khảo [Dịch vụ AWS theo Vùng](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/) (17).

Trong suốt quá trình di chuyển, các thùng chứa Amazon S3 (12, 26) trong cả Tài khoản Khám phá Di chuyển AWS và Tài khoản Đích Di chuyển AWS lưu trữ các sản phẩm di chuyển chính. Bao gồm dữ liệu kho, ánh xạ phụ thuộc, kế hoạch đợt và nhóm ứng dụng, cũng như các mẫu Hạ tầng dưới dạng Mã (AWS CloudFormation và AWS Cloud Development Kit) và kế hoạch di chuyển theo từng đợt.

## Lợi ích cho khách hàng

AWS Transform for VMware mang lại những lợi thế đáng kể:

- **Giảm công việc thủ công** – Giảm thiểu lỗi con người và giải phóng các tài nguyên IT có giá trị thông qua tự động hóa
- **Tăng độ chính xác** – Sử dụng ánh xạ phụ thuộc và lập kế hoạch đợt được điều khiển bởi AI cho các chiến lược di chuyển tối ưu
- **Cải thiện hợp tác** – Quản lý và theo dõi tập trung thúc đẩy phối hợp tốt hơn giữa các nhóm
- **Tối ưu hóa chi phí** – Điều chỉnh kích thước phiên bản phù hợp và tận dụng các mô hình định giá linh hoạt của AWS để tiết kiệm ngay lập tức và dài hạn
- **Đảm bảo tương lai** – Mở ra cơ hội cho việc hiện đại hóa và đổi mới liên tục trên nền tảng AWS Cloud

Luôn xem xét và tuân theo các yêu cầu bảo mật của tổ chức bạn, nghĩa vụ tuân thủ và [các thực hành tốt nhất bảo mật AWS](https://aws.amazon.com/architecture/security-identity-compliance/?cards-all.sort-by=item.additionalFields.sortDate&cards-all.sort-order=desc&awsf.content-type=*all&awsf.methodology=*all) khi triển khai bất kỳ giải pháp di chuyển nào. Để có hướng dẫn bảo mật chi tiết, tham khảo [Tài liệu Bảo mật AWS](https://docs.aws.amazon.com/security/) và nhóm bảo mật của tổ chức bạn.

## Giá cả

AWS Transform tăng tốc các dự án di chuyển và hiện đại hóa cho hệ thống VMware với khả năng AI đại diện. Hiện tại, chúng tôi cung cấp các tính năng cốt lõi của chúng tôi—bao gồm đánh giá và chuyển đổi—miễn phí* cho khách hàng AWS. Điều này cho phép bạn tăng tốc hành trình di chuyển và hiện đại hóa mà không có chi phí trả trước.

***Miễn phí đề cập đến chính dịch vụ AWS Transform. Các phí tiêu chuẩn áp dụng cho các dịch vụ và tài nguyên AWS được sử dụng trong quá trình di chuyển.**

## Tóm tắt và các bước tiếp theo

AWS Transform for VMware trao quyền cho các tổ chức vượt qua những phức tạp của di chuyển và hiện đại hóa VMware. Bằng cách cung cấp một cách tiếp cận toàn diện, tự động hóa, nó cho phép chuyển đổi nhanh hơn, đáng tin cậy hơn lên AWS Cloud. Dịch vụ mới này cung cấp các công cụ và khả năng cần thiết để điều hướng cảnh quan VMware đang thay đổi một cách tự tin.

Kiến trúc mà chúng ta đã khám phá chứng minh cách AWS Transform for VMware giải quyết các thách thức chính:

- Tối ưu hóa các quy trình khám phá và đánh giá
- Tự động hóa chuyển đổi mạng và lập kế hoạch đợt thông minh
- Điều phối thực hiện di chuyển với ít gián đoạn nhất
- Tăng cường bảo mật và tuân thủ trong suốt quá trình di chuyển
- Cung cấp quản lý và giám sát tập trung
- Cung cấp các tùy chọn mạng linh hoạt để phù hợp với các yêu cầu đa dạng

Sẵn sàng tăng tốc hành trình di chuyển VMware của bạn? Truy cập trang sản phẩm [AWS Transform for VMware](https://aws.amazon.com/transform/vmware/) để tìm hiểu thêm và bắt đầu ngay hôm nay. Xem [bản demo tương tác](https://aws.storylane.io/share/qye0se68an9i?utm_source=pdp) sau đây của AWS Transform for VMware. Nếu bạn đang xuất cấu hình mạng từ môi trường VMware NSX, cũng tham khảo [Xuất dữ liệu cấu hình mạng với Nhập/Xuất cho NSX](https://aws.amazon.com/blogs/migration-and-modernization/exporting-network-configuration-data-with-import-export-for-nsx/). Nhóm chuyên gia của chúng tôi sẵn sàng hướng dẫn bạn qua các sáng kiến di chuyển và hiện đại hóa của bạn, giúp bạn khai thác toàn bộ tiềm năng của AWS Cloud.

---

## Về các tác giả

![Kiran Reid](/images/3-blog/2-image/Screenshot-2025-05-28-at-09.14.50.png)
### Kiran Reid

Kiran Reid là Chuyên gia Cấp cao về AI Tạo sinh và Học máy tại Amazon Web Services (AWS). Với chuyên môn về công nghệ trí tuệ nhân tạo (AI), Kiran làm việc chặt chẽ với các đối tác và khách hàng AWS để hỗ trợ di chuyển và hiện đại hóa hệ thống bằng AI.

![Ramu Akula](/images/3-blog/2-image/akulrama.jpeg)
### Ramu Akula

Ram Akula là Quản lý Danh mục Đầu tư Chính và chuyên gia Chuyển đổi Mạng Viễn thông tại Amazon Web Services (AWS). Với hơn 24 năm kinh nghiệm IT, anh ấy giúp khách hàng di chuyển và hiện đại hóa hệ thống của họ lên AWS.

![Patrick Kremer](/images/3-blog/2-image/Patrick-Kremer.png)
### Patrick Kremer

Patrick Kremer là Kiến trúc sư Giải pháp Chuyên gia Cấp cao tập trung vào Di chuyển và Hiện đại hóa Hạ tầng. Patrick gia nhập AWS vào năm 2022, nơi anh ấy tận dụng 20 năm kinh nghiệm VMware để giúp khách hàng chuyển sang các giải pháp AWS. Anh ấy là Kiến trúc sư Giải pháp AWS Chuyên nghiệp được chứng nhận với niềm đam mê giảng dạy và viết blog.

![Mark Jaggers](/images/3-blog/2-image/markjagg.jpeg)
### Mark Jaggers

Mark Jaggers là Quản lý Quản lý Sản phẩm Đối ngoại tại Amazon Web Services (AWS), nơi anh ấy dẫn dắt các chiến lược đưa ra thị trường cho các giải pháp di chuyển đám mây và khôi phục thảm họa. Mark làm việc trong các nhóm dịch vụ cho cả AWS Elastic Disaster Recovery và AWS Application Migration Service, giúp khách hàng hiện đại hóa hạ tầng IT của họ ở quy mô lớn.