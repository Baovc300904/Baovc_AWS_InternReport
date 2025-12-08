---
title: "Blog 3"
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

# Cách Launchpad từ Pega Cho Phép Mở Rộng SaaS An Toàn với AWS Lambda

**Tác giả:** Anton Aleksandrov, Giridhar Ramadhenu, Rajesh Kumar Maram, và Anubhav Sharma  
**Xuất bản:** 30 THÁNG 5, 2025  
**Danh mục:** AWS Lambda, Best Practices, Giải Pháp Khách Hàng, Serverless, Hướng Dẫn Kỹ Thuật

Các tổ chức lớn ngày càng áp dụng các giải pháp phần mềm dưới dạng dịch vụ (SaaS) để tập trung vào các ưu tiên kinh doanh, giảm chi phí quản lý cơ sở hạ tầng và tối ưu hóa chi phí. Các tổ chức này mong đợi các nhà cung cấp SaaS cung cấp khả năng tùy chỉnh để điều chỉnh hành vi giải pháp theo nhu cầu của họ. Mặc dù các phương pháp truyền thống như feature flags và webhooks cung cấp một số tính linh hoạt, chúng thường không đạt được mức độ tùy chỉnh cao. Một mô hình mới nổi trong lĩnh vực này là **thực thi mã tùy chỉnh do tenant cung cấp**, cho phép các tenant inject mã của riêng họ vào các điểm workflow cụ thể, cho phép tùy chỉnh sâu trong khi vẫn bảo toàn tính toàn vẹn và bảo mật của các giải pháp SaaS cốt lõi.

Trong bài viết này, chúng tôi chia sẻ cách **Pegasystems (Pega)** xây dựng **Launchpad**, nền tảng phát triển SaaS mới của họ, để giải quyết một thách thức cốt lõi trong môi trường multi-tenant: cho phép tùy chỉnh khách hàng an toàn. Bằng cách chạy mã tenant trong các môi trường cô lập với AWS Lambda, Launchpad cung cấp cho khách hàng của mình một nền tảng an toàn, có khả năng mở rộng, loại bỏ nhu cầu tùy chỉnh mã bespoke.

---

## Tổng Quan Giải Pháp

Launchpad, được xây dựng trên AWS, là một nền tảng end-to-end mà trên đó các nhà cung cấp phần mềm có thể xây dựng, khởi chạy và vận hành các ứng dụng SaaS B2B tập trung vào workflow và các giải pháp AI. Nó cung cấp một môi trường cloud được quản lý, an toàn, có khả năng mở rộng để lưu trữ các ứng dụng và dữ liệu multi-tenant. Nó tăng tốc trải nghiệm xây dựng với các công cụ low code được hỗ trợ bởi generative AI, các khả năng được xây dựng sẵn và cấu hình cấp độ subscriber. Là một nền tảng multi-tenant ở cốt lõi, Launchpad phải duy trì sự cô lập nghiêm ngặt giữa các tenant trong kiến trúc của nó.

Một trong những yêu cầu của Launchpad là cho phép các tenant của họ tăng cường workflow một cách tự nhiên bằng cách cung cấp mã tùy chỉnh. Một số kịch bản phổ biến bao gồm:
- Giao tiếp với các hệ thống bên ngoài với các giao thức độc quyền không tuân theo tiêu chuẩn ngành
- Tái sử dụng logic nghiệp vụ hiện có
- Phát triển mã tùy chỉnh dựa trên SDK

Giải pháp đòi hỏi khả năng cho các tenant cung cấp mã tùy chỉnh sẽ triển khai logic nghiệp vụ cần thiết, mà Launchpad sẽ thực thi. Điều này đòi hỏi thiết kế một môi trường runtime an toàn cho việc thực thi mã tùy chỉnh duy trì mức độ cô lập cross-tenant cao nhất trong kiến trúc multi-tenant, đồng thời cho phép truy cập đầy đủ vào các API và dịch vụ của nền tảng. Điều cần thiết là xây dựng một kiến trúc tách biệt môi trường chạy mã tenant khỏi nền tảng SaaS cốt lõi.

---

## Thiết Kế Topology Giải Pháp

Để đạt được mức độ cô lập compute cao cần thiết để chạy mã được cung cấp bởi các tenant khác nhau, Launchpad đã áp dụng **Lambda functions** trong kiến trúc của nó như một môi trường compute ephemeral an toàn. Mỗi đoạn mã không đáng tin cậy được cung cấp bởi các tenant được bootstrap như một Lambda function độc lập, với **sự cô lập dựa trên Firecracker** mạnh mẽ giữa các function khác nhau và các môi trường thực thi đáp ứng các yêu cầu của Launchpad. Sự cô lập này cung cấp:
- Tài nguyên chuyên dụng
- Quyền truy cập có thể tùy chỉnh
- Giám sát và vận hành độc lập
- Tự động scaling cho mỗi function
- Tách biệt hoàn toàn khỏi các function khác và môi trường thực thi của chúng

Với Lambda là một dịch vụ compute serverless, việc áp dụng nó cho kiến trúc Launchpad mang lại một số lợi ích đáng kể:

**Lợi Ích Kinh Doanh Chính:** Các tenant có thể triển khai hàng nghìn tùy chỉnh workflow của riêng họ chỉ bằng cách cung cấp các đoạn mã, thay vì đội ngũ kỹ thuật Launchpad phải chịu trách nhiệm triển khai chúng trong mã nền tảng cốt lõi.

**Lợi Ích Kỹ Thuật:**
- **Managed runtimes** – AWS xử lý patching và cập nhật cơ sở hạ tầng cơ bản, hệ điều hành và runtime cho khách hàng, giảm bề mặt tấn công tiềm năng
- **Fine-grained permissions** – Mỗi function có thể có bộ access policies riêng để kiểm soát chặt chẽ những tài nguyên và hành động mà nó có thể truy cập
- **Không cần pre-provision và trả tiền cho overprovisioned capacity** – Lambda functions tự động scale up và down dựa trên các mô hình traffic
- **Built-in monitoring** – Lambda functions phát ra các metric, log và trace chi tiết thông qua Amazon CloudWatch và AWS X-Ray ngay từ đầu, giúp dễ dàng giám sát việc thực thi mã tenant

Để giảm thêm rủi ro, Launchpad chạy các Lambda functions này với mã không đáng tin cậy trong một **AWS account chuyên dụng**, tách biệt khỏi account nền tảng SaaS cốt lõi. Khi người dùng cuối tạo một function mới trong cổng authoring Launchpad, họ tải lên mã của mình và chỉ định code handler sẽ được thực thi trong quá trình invocation. Người dùng cũng có thể ánh xạ input và output của function sang các trường Launchpad để xử lý thêm nhằm cho phép mức độ tùy chỉnh và tích hợp cao hơn.

Dịch vụ authoring multi-tenant là một component Control Plane chạy như một microservice trên cluster **Amazon Elastic Kubernetes Service (Amazon EKS)** và sử dụng Lambda API để quản lý vòng đời function. Sau khi một tài nguyên function được tạo, nó có thể được sử dụng cho các invocation tiếp theo.

---

## Kiến Trúc Runtime

Tại runtime, khi Launchpad cần gọi một function, nó gọi Lambda Invoke API. Trước khi function được gọi, dịch vụ runtime multi-tenant thực hiện **kiểm tra tenancy** để đảm bảo request đến từ một tenant được ủy quyền bằng cách thực hiện xác thực token. Sau khi xác thực thành công, dịch vụ gọi Lambda function cần thiết. Để gọi các function được lưu trữ trong một AWS account khác, dịch vụ runtime multi-tenant sử dụng một **AWS Identity and Access Management (IAM) role** để assume các quyền cần thiết và gọi dịch vụ Lambda bằng AWS SDK.

**Workflow bao gồm các bước sau:**

1. Một request người dùng đến service application gateway
2. Application gateway xác thực request bằng dịch vụ bảo mật tenancy
3. Sau khi được xác thực, request được chuyển tiếp đến dịch vụ runtime multi-tenant
4. Dịch vụ runtime multi-tenant xác thực token được cung cấp và thực hiện kiểm tra tenancy (đảm bảo các tenant chỉ có thể gọi các function riêng của họ mà họ có quyền)
5. Pod dịch vụ runtime multi-tenant assume IAM role cần thiết để gọi Lambda function cụ thể của tenant trong một AWS account khác
6. Pod dịch vụ runtime multi-tenant gọi Lambda function cần thiết

Việc gọi platform API từ mã tùy chỉnh đơn giản như kết nối với bất kỳ external API nào. Mã tùy chỉnh có thể xác thực với nền tảng bằng **OAuth2**. Để tạo điều kiện thuận lợi cho việc xác thực, developer có thể truyền credentials như các tham số input cho function từ nền tảng cốt lõi. Sau đó, developer có thể tạo một bản ghi tương ứng (được cô lập bởi tenant) trong nền tảng lưu trữ credentials cho mỗi function, và truyền credentials như các tham số input trong quá trình invocation.

---

## Khả Năng Quan Sát Kiến Trúc Phân Tán

Vận hành một kiến trúc phân tán chạy mã không đáng tin cậy trên nhiều AWS account đòi hỏi một chiến lược observability toàn diện. Cách tiếp cận của Launchpad kết hợp logging và monitoring tập trung với aggregation cross-account để cung cấp một cái nhìn vận hành thống nhất về nền tảng.

Kiến trúc monitoring sử dụng **CloudWatch Metrics** để quan sát các Lambda functions, tổng hợp chúng thông qua một lớp observability tập trung. Thiết lập này cho phép các nhà vận hành nền tảng tương quan các metric của Lambda function với các dịch vụ nền tảng cốt lõi chạy trên Amazon EKS. Launchpad cũng thu thập telemetry theo function như:
- Function invocations
- Error rates
- Execution time

Các dimension telemetry này cho phép cả góc nhìn monitoring toàn nền tảng và cụ thể theo tenant.

Đối với logging và troubleshooting, Launchpad triển khai một pipeline logging thống nhất tổng hợp các log Lambda function với các log application gateway và runtime service. Mỗi request chảy qua hệ thống mang một **correlation ID**, vì vậy các nhà vận hành có thể trace các đường dẫn thực thi qua các dịch vụ SaaS cốt lõi và vào các tenant function chạy trong AWS account chạy các tenant Lambda functions.

Với kiến trúc observability đa lớp này, Launchpad có thể duy trì sự xuất sắc trong vận hành trong khi chạy mã tenant một cách an toàn ở quy mô lớn. Các đánh giá vận hành thường xuyên thúc đẩy cải tiến liên tục trong coverage monitoring và các quy trình phản hồi sự cố. Việc có các Lambda functions theo tenant cho phép Launchpad sử dụng **tenant-specific cost allocation tags**, hỗ trợ thêm việc hiểu chi phí vận hành mã tùy chỉnh của tenant.

---

## Best Practices

Khi xây dựng một giải pháp SaaS, việc duy trì một codebase cốt lõi thống nhất là điều cần thiết cho khả năng mở rộng và quản lý. Việc triển khai các biến thể theo tenant trong mã nền tảng cốt lõi có thể dẫn đến độ phức tạp bảo trì và technical debt. Thay vào đó, hãy thiết kế giải pháp SaaS của bạn để có **extension points**, cho phép các tenant của bạn inject mã tùy chỉnh của họ tại các điểm cụ thể trong workflow, cho phép tùy chỉnh mà không làm tổn hại đến khả năng bảo trì của nền tảng. Mô hình này đảm bảo nền tảng SaaS cốt lõi vẫn sạch và được tiêu chuẩn hóa trong khi cung cấp tính linh hoạt mà khách hàng yêu cầu.

**Các best practice bổ sung bao gồm:**

1. **Sử dụng các account riêng biệt** để chạy các Lambda functions với mã không đáng tin cậy do tenant cung cấp để đảm bảo nó được cô lập khỏi mã nền tảng SaaS cốt lõi của bạn
2. **Cấp quyền truy cập tối thiểu tuyệt đối cần thiết** cho execution role được gán cho function. Mã tùy chỉnh chạy trong môi trường thực thi nhận được các quyền được định nghĩa trong execution role khi thực hiện request đến các AWS API endpoint. Nếu function không cần reach out đến AWS API endpoint, hãy xóa tất cả các quyền khỏi execution role và thêm một policy AWSDenyAll rõ ràng
3. **Sử dụng các Lambda functions riêng biệt** cho mỗi đoạn mã và mỗi tenant. Điều này sẽ cung cấp mức độ cô lập cross-tenant cao nhất. Tài nguyên không được tái sử dụng giữa các function khác nhau và môi trường thực thi
4. **Sử dụng Lambda layers** trong trường hợp bạn cần thêm một lớp mã do vendor cung cấp để giữ nó tách biệt khỏi mã không đáng tin cậy do tenant cung cấp
5. **Triển khai các security control bổ sung**, chẳng hạn như sử dụng các cấu trúc Amazon Virtual Private Cloud (Amazon VPC) để hạn chế truy cập mạng và VPC Flow Logs để giám sát hoạt động mạng

---

## Kết Luận

Việc triển khai một môi trường thực thi mã không đáng tin cậy an toàn trong các nền tảng SaaS giải quyết một nhu cầu quan trọng về tùy chỉnh tenant trong khi duy trì tính toàn vẹn kiến trúc. Lambda cung cấp một mô hình cô lập tích hợp sẵn, các security control chi tiết và khả năng mở rộng serverless, vì vậy các nhà cung cấp SaaS như Launchpad có thể giải quyết các yêu cầu thực thi mã do tenant cung cấp trong môi trường multi-tenant và cung cấp khả năng tùy chỉnh mạnh mẽ trong khi duy trì ranh giới bảo mật nghiêm ngặt và hiệu quả vận hành.

Mô hình kiến trúc này cho phép các nhà cung cấp tập trung vào phát triển nền tảng cốt lõi trong khi tự tin hỗ trợ các workflow cụ thể của tenant thông qua môi trường thực thi Lambda an toàn và có khả năng mở rộng.

**Để tìm hiểu thêm:**
- Security Overview of AWS Lambda white paper
- Các mẫu kiến trúc serverless tại Serverlessland.com

---

## Về Các Tác Giả

**Anton Aleksandrov** - Principal Solutions Architect cho AWS Serverless và Event-Driven architectures. Với hơn hai thập kỷ kinh nghiệm kỹ thuật và kiến trúc thực tế, Anton làm việc với các khách hàng ISV và SaaS lớn để thiết kế các giải pháp cloud có khả năng mở rộng cao, sáng tạo và an toàn.

**Giridhar Ramadhenu** - Software architect dày dạn kinh nghiệm với hơn 2 thập kỷ chuyên môn, chuyên về microservices, event-driven và layered architectures. Là Fellow Software Architect cho Launchpad tại Pegasystems và một thành viên có ảnh hưởng của Architecture Guild, Giridhar đóng vai trò then chốt trong việc định hình kiến trúc của nhiều sản phẩm khác nhau.

**Rajesh Kumar Maram** - Senior Principal Software Engineer cho Launchpad tại Pegasystems với hơn một thập kỷ kinh nghiệm. Anh ấy dẫn dắt với sự đổi mới trong việc giải quyết các vấn đề thách thức và khám phá các công nghệ AWS mới nhất cho các use case kinh doanh Pega.

**Anubhav Sharma** - Principal Solutions Architect tại AWS với hơn 2 thập kỷ kinh nghiệm trong việc coding và thiết kế các ứng dụng quan trọng cho kinh doanh. Anh ấy chuyên hướng dẫn các ISV và doanh nghiệp trong hành trình xây dựng, triển khai và vận hành các giải pháp SaaS trên AWS.
