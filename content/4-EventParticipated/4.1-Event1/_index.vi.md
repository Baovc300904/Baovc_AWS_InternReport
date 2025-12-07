---
title: "Event 1"
date: 2024-01-01T00:00:00+07:00
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

# Bài thu hoạch "AWS Community Day Vietnam 2025"

**Địa điểm:** Trung tâm Hội chợ và Triển lãm Sài Gòn (SECC), TP. Hồ Chí Minh  
**Thời gian:** 09:00 – 17:00, Thứ Bảy, 20/09/2025  
**Đơn vị tổ chức:** AWS User Group Vietnam, First Cloud Journey  
**Điều phối viên:** Phương Nguyễn, Minh Trần, Khánh Lê

### Mục Đích Của Sự Kiện

- Kết nối các thành viên cộng đồng AWS trên toàn Việt Nam
- Chia sẻ kinh nghiệm thực tế với các dịch vụ AWS cloud
- Giới thiệu các tính năng AWS mới và best practices
- Cung cấp workshop thực hành cho người mới bắt đầu và người dùng trung cấp
- Thúc đẩy việc chia sẻ kiến thức giữa các developer, architect và những người đam mê cloud

### Danh Sách Diễn Giả

- **Nguyễn Văn Thành** – Cloud Solutions Architect, AWS Vietnam
- **Trần Thị Lan** – Senior DevOps Engineer, VNG Corporation
- **Lê Quốc Hùng** – CTO, CloudTech Solutions
- **Phạm Minh Đức** – AWS Community Hero

### Nội Dung Nổi Bật

#### Phát biểu khai mạc – Hành trình Cloud tại Việt Nam

Sự kiện mở đầu với bài nói truyền cảm hứng về sự phát triển của việc sử dụng cloud tại Việt Nam. Diễn giả nhấn mạnh cách các startup và doanh nghiệp Việt Nam đang tận dụng AWS để mở rộng quy mô kinh doanh, giảm chi phí hạ tầng, và tăng tốc chuyển đổi số.

Các thống kê được chia sẻ:
- Tăng 60% trong việc sử dụng AWS của các công ty Việt Nam trong 2024-2025
- Serverless và containerization đang trở thành xu hướng chính
- Nhu cầu ngày càng cao về bảo mật cloud và giải pháp tuân thủ

#### Phiên 1: Xây dựng Ứng dụng Web Mở rộng trên AWS

Phiên này đề cập các mẫu kiến trúc thực tế cho việc xây dựng ứng dụng web có khả năng mở rộng:

- **Kiến trúc 3 tầng**: Tầng web (CloudFront + S3), Tầng app (EC2/ECS), Tầng dữ liệu (RDS)
- **Chiến lược Auto Scaling**: Target tracking, step scaling, scheduled scaling
- **Cân bằng tải**: ALB vs NLB, health check, sticky session
- **Tầng caching**: ElastiCache (Redis/Memcached) để tối ưu hiệu suất

Diễn giả minh họa một ứng dụng thương mại điện tử thực tế xử lý 10,000+ người dùng đồng thời sử dụng Auto Scaling Group và RDS read replica.

#### Phiên 2: Best Practices về Kiến trúc Serverless

Đây là một trong những phiên hấp dẫn nhất, tập trung vào các mẫu serverless:

- **Thiết kế Lambda function**: Tối ưu cold start, cấu hình memory, thiết lập timeout
- **Tích hợp API Gateway**: REST vs HTTP API, validation request, throttling
- **Mẫu event-driven**: S3 trigger, DynamoDB Streams, EventBridge
- **Tối ưu chi phí**: Right-sizing Lambda memory, sử dụng Reserved Concurrency hợp lý

Demo trực tiếp cho thấy việc xây dựng một pipeline xử lý ảnh đơn giản sử dụng Lambda, S3 và Rekognition.

#### Phiên 3: Best Practices về Bảo mật trên AWS

Một phiên quan trọng về các nguyên tắc bảo mật cloud cơ bản:

- **Chính sách IAM**: Nguyên tắc đặc quyền tối thiểu, điều kiện policy, service control policy
- **Bảo mật mạng**: Security group, NACL, cô lập VPC
- **Mã hóa dữ liệu**: Mã hóa khi lưu trữ (KMS), mã hóa khi truyền tải (TLS)
- **Giám sát và tuân thủ**: CloudTrail, GuardDuty, Security Hub

Diễn giả nhấn mạnh rằng "bảo mật là trách nhiệm của mọi người" và chia sẻ các sai lầm bảo mật phổ biến cần tránh.

#### Phiên 4: CI/CD với AWS DevOps Tools

Tập trung vào việc tự động hóa phân phối phần mềm:

- **CodePipeline**: Xây dựng pipeline phân phối end-to-end
- **CodeBuild**: Môi trường build container hóa, build specification
- **CodeDeploy**: Blue/green deployment, canary release
- **Tích hợp với GitHub**: GitHub Actions + AWS services

Demo minh họa việc triển khai ứng dụng Node.js từ Git commit đến production với kiểm thử tự động và khả năng rollback.

#### Workshop Thực hành: Xây dựng Dự án AWS Đầu tiên

Workshop buổi chiều nơi người tham gia xây dựng một ứng dụng web đơn giản:

1. Tạo VPC với public và private subnet
2. Khởi chạy EC2 instance với Auto Scaling
3. Thiết lập RDS MySQL database
4. Cấu hình Application Load Balancer
5. Triển khai một web app mẫu và kiểm tra scaling

Người hướng dẫn workshop cung cấp chỉ dẫn từng bước và hỗ trợ khắc phục sự cố.

### Những Gì Học Được

#### Tư Duy Thiết Kế

- **Business-first approach**: Luôn bắt đầu từ business domain, không phải technology
- **Ubiquitous language**: Importance của common vocabulary giữa business và tech teams
- **Bounded contexts**: Cách identify và manage complexity trong large systems

#### Kiến Trúc Kỹ Thuật

- **Event storming technique**: Phương pháp thực tế để mô hình hóa quy trình kinh doanh
- Sử dụng **Event-driven communication** thay vì synchronous calls
- **Integration patterns**: Hiểu khi nào dùng sync, async, pub/sub, streaming
- **Compute spectrum**: Criteria chọn từ VM → containers → serverless

#### Chiến Lược Hiện Đại Hóa

- **Phased approach**: Không rush, phải có roadmap rõ ràng
- **7Rs framework**: Nhiều con đường khác nhau tùy thuộc vào đặc điểm của mỗi ứng dụng
- **ROI measurement**: Cost reduction + business agility

### Ứng Dụng Vào Công Việc

- **Áp dụng DDD** cho project hiện tại: Event storming sessions với business team
- **Refactor microservices**: Sử dụng bounded contexts để identify service boundaries
- **Implement event-driven patterns**: Thay thế một số sync calls bằng async messaging
- **Serverless adoption**: Pilot AWS Lambda cho một số use cases phù hợp
- **Try Amazon Q Developer**: Integrate vào development workflow để boost productivity

### Trải nghiệm trong event

Tham gia workshop **“GenAI-powered App-DB Modernization”** là một trải nghiệm rất bổ ích, giúp tôi có cái nhìn toàn diện về cách hiện đại hóa ứng dụng và cơ sở dữ liệu bằng các phương pháp và công cụ hiện đại. Một số trải nghiệm nổi bật:

#### Học hỏi từ Các Chuyên gia Giàu Kinh nghiệm
- Các diễn giả chia sẻ những kiến thức thực tế từ môi trường production, không chỉ lý thuyết
- Việc nghe về các thách thức thực tế và giải pháp từ VNG Corporation và các công ty khác rất có giá trị
- AWS Community Hero cung cấp chỉ dẫn và tư vấn nghề nghiệp cho các cloud engineer mới

#### Trải Nghiệm Workshop Thực Hành
- Xây dựng ứng dụng web hoàn chỉnh từ đầu giúp củng cố hiểu biết về các dịch vụ AWS
- Khắc phục sự cố trong workshop giúp học các kỹ thuật debug
- Thấy cách các dịch vụ AWS khác nhau tích hợp (VPC, EC2, RDS, ALB) mang lại cái nhìn toàn diện
- Tài liệu workshop được tổ chức tốt và dễ theo dõi

#### Hiểu Về Kiến Trúc Thực Tế
- Demo ứng dụng thương mại điện tử với 10,000+ người dùng đồng thời cho thấy khả năng mở rộng thực tế
- Tìm hiểu về các chính sách Auto Scaling giúp hiểu cách ứng dụng xử lý traffic tăng đột biến
- Phiên bảo mật làm nổi bật các sai lầm cần tránh trong dự án của bản thân
- Demo CI/CD minh họa các thực hành phân phối phần mềm hiện đại

#### Kết Nối với Cộng Đồng
- Gặp gỡ các sinh viên và developer mới đam mê cloud computing
- Trao đổi liên lạc với các kỹ sư có kinh nghiệm sẵn sàng giúp đỡ với câu hỏi học tập
- Khám phá các buổi meetup của AWS User Group địa phương để học tập liên tục
- Kết nối với các thành viên FCJ và thảo luận về kinh nghiệm thực tập

#### Truyền Cảm Hứng và Động Lực
- Thấy các công ty Việt Nam sử dụng AWS thành công rất truyền cảm hứng
- Nhận ra rằng cloud engineering là con đường sự nghiệp khả thi với cơ hội ngày càng nhiều
- Hiểu rằng học tập liên tục là thiết yếu trong công nghệ cloud
- Động lực theo đuổi chứng chỉ AWS sau khi tích lũy thêm kinh nghiệm thực tế

#### Kỹ Năng Thực Tế Đạt Được
- Cấu hình mạng VPC bao gồm subnet, route table và internet gateway
- Khởi chạy EC2 instance với cấu hình security group phù hợp
- Thiết lập RDS database với backup và maintenance window phù hợp
- Triển khai ứng dụng đằng sau Application Load Balancer
- Triển khai các chính sách Auto Scaling cơ bản

#### Một số hình ảnh khi tham gia sự kiện
*Hình ảnh sự kiện sẽ được thêm vào đây*  

> Tóm lại, AWS Community Day Vietnam 2025 không chỉ là sự kiện học tập – đó là cơ hội để hòa mình vào hệ sinh thái AWS, kết nối với các thành viên cộng đồng đầy đam mê, và có được kiến thức thực tế áp dụng trực tiếp vào thực tập và sự nghiệp tương lai.
