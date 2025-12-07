---
title: "Blog 3"
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

# Cách Launchpad từ Pega kích hoạt khả năng mở rộng SaaS an toàn với AWS Lambda

**Tác giả: Anton Aleksandrov, Giridhar Ramadhenu, Rajesh Kumar Maram, và Anubhav Sharma**  
**Ngày đăng: 30 tháng 5, 2024**  
**Danh mục: AWS Lambda, Best Practices, Customer Solutions, Serverless, Technical How-to**

## Giới thiệu

Các tổ chức lớn ngày càng áp dụng các giải pháp phần mềm dưới dạng dịch vụ (SaaS) để tập trung vào các ưu tiên kinh doanh, giảm chi phí quản lý cơ sở hạ tầng và tối ưu hóa chi phí. Những tổ chức này mong đợi các nhà cung cấp SaaS cung cấp khả năng tùy chỉnh để điều chỉnh hành vi giải pháp theo nhu cầu của họ. Mặc dù các phương pháp truyền thống như feature flags và webhooks cung cấp một số tính linh hoạt, chúng thường không đáp ứng được mức độ tùy chỉnh cao. Một pattern mới nổi trong lĩnh vực này là thực thi mã tùy chỉnh do tenant cung cấp, cho phép các tenant đưa mã của riêng họ vào các điểm workflow cụ thể, tạo điều kiện cho việc tùy chỉnh sâu trong khi vẫn bảo tồn tính toàn vẹn và bảo mật của giải pháp SaaS cốt lõi.

Trong bài viết này, chúng tôi chia sẻ cách Pegasystems (Pega) xây dựng Launchpad, nền tảng phát triển SaaS mới của họ, để giải quyết một thách thức cốt lõi trong môi trường multi-tenant: kích hoạt tùy chỉnh khách hàng an toàn. Bằng cách chạy mã tenant trong các môi trường cô lập với AWS Lambda, Launchpad cung cấp cho khách hàng một nền tảng an toàn, có thể mở rộng, loại bỏ nhu cầu tùy chỉnh mã riêng biệt.

## Tổng quan giải pháp

Launchpad, được xây dựng trên AWS, là một nền tảng end-to-end mà các nhà cung cấp phần mềm có thể xây dựng, triển khai và vận hành các ứng dụng SaaS B2B tập trung vào workflow và các giải pháp AI. Nó cung cấp một môi trường cloud được quản lý, an toàn, có thể mở rộng để hosting các ứng dụng và dữ liệu multi-tenant. Nó tăng tốc trải nghiệm xây dựng với các công cụ low code được hỗ trợ bởi generative AI, các khả năng được xây dựng sẵn và cấu hình cấp độ subscriber. Là một nền tảng multi-tenant ở cốt lõi, Launchpad phải duy trì sự cô lập nghiêm ngặt giữa các tenant trong kiến trúc của nó.

Một trong những yêu cầu của Launchpad là cho phép các tenant của họ tăng cường workflows một cách native bằng cách cung cấp mã tùy chỉnh. Một số kịch bản phổ biến bao gồm giao tiếp với các hệ thống bên ngoài có giao thức độc quyền không theo tiêu chuẩn ngành, tái sử dụng logic kinh doanh hiện có, và phát triển mã tùy chỉnh dựa trên SDK.

Giải pháp đòi hỏi khả năng cho các tenant cung cấp mã tùy chỉnh triển khai logic kinh doanh cần thiết, mà Launchpad sẽ thực thi. Điều này đòi hỏi việc thiết kế một môi trường runtime an toàn cho việc thực thi mã tùy chỉnh duy trì mức độ cô lập cross-tenant cao nhất trong kiến trúc multi-tenant, đồng thời cho phép truy cập đầy đủ vào các API và dịch vụ của nền tảng. Việc xây dựng một kiến trúc tách rời môi trường chạy mã tenant khỏi nền tảng SaaS cốt lõi là điều cần thiết, như được minh họa trong sơ đồ sau.

**Kiến trúc tách biệt môi trường thực thi mã tenant khỏi nền tảng SaaS cốt lõi**

## Thiết kế topology giải pháp

Để đạt được mức độ cô lập compute cao cần thiết cho việc chạy mã do các tenant khác nhau cung cấp, Launchpad đã áp dụng các hàm AWS Lambda trong kiến trúc của mình như một môi trường compute ephemeral an toàn. Mỗi đoạn mã không đáng tin cậy được cung cấp bởi tenant được khởi tạo như một hàm Lambda độc lập, với sự cô lập mạnh mẽ dựa trên Firecracker giữa các hàm và môi trường thực thi riêng biệt. Cơ chế này mang lại khả năng phân bổ tài nguyên chuyên dụng, quyền truy cập có thể tùy chỉnh, giám sát và vận hành độc lập, cùng khả năng tự động mở rộng cho từng hàm, đồng thời duy trì sự tách biệt hoàn toàn khỏi các hàm khác và môi trường thực thi của chúng — như được minh họa trong sơ đồ sau.

**Kiến trúc cô lập Lambda functions với Firecracker**

Với Lambda là một dịch vụ serverless compute, việc áp dụng nó cho kiến trúc Launchpad mang lại một số lợi ích đáng kể. Lợi ích kinh doanh chính là các tenant có thể triển khai hàng nghìn tùy chỉnh workflow tự mình chỉ bằng cách cung cấp các đoạn mã, thay vì đội ngũ kỹ thuật Launchpad phải chịu trách nhiệm triển khai chúng trong mã nền tảng cốt lõi. Các lợi ích khác bao gồm:

- **Managed runtimes** – AWS tự động xử lý việc vá lỗi và cập nhật cho cơ sở hạ tầng, hệ điều hành, và runtime, giúp giảm đáng kể bề mặt tấn công tiềm ẩn.
- **Fine-grained permissions** – Mỗi hàm có thể được gán chính sách truy cập (IAM policy) riêng, cho phép kiểm soát chi tiết đối với tài nguyên và hành động mà hàm được phép thực hiện.
- **No pre-provisioning or over-provisioning costs** – Lambda tự động mở rộng lên/xuống theo lưu lượng truy cập, giúp loại bỏ nhu cầu dự trữ trước năng lực compute và tránh lãng phí chi phí.
- **Built-in monitoring** – Các hàm Lambda tự động phát sinh metrics, logs, và traces chi tiết thông qua Amazon CloudWatch và AWS X-Ray, giúp giám sát việc thực thi mã tenant trở nên dễ dàng và minh bạch.

Để giảm thiểu rủi ro bảo mật, Launchpad triển khai các hàm AWS Lambda chứa mã không đáng tin cậy trong một AWS account chuyên dụng, hoàn toàn tách biệt với account nền tảng SaaS cốt lõi. Khi người dùng cuối tạo một hàm mới trong portal authoring của Launchpad, họ tải lên mã nguồn của mình và chỉ định code handler để được thực thi trong quá trình invocation. Người dùng cũng có thể ánh xạ input và output của hàm đến các trường dữ liệu trong Launchpad nhằm xử lý thêm, giúp kích hoạt mức độ tùy chỉnh và tích hợp cao hơn. Dịch vụ authoring multi-tenant này là một thành phần Control Plane, chạy dưới dạng microservice trên cụm Amazon Elastic Kubernetes Service (Amazon EKS) và sử dụng Lambda API để quản lý vòng đời của các hàm Lambda (function lifecycle), như được minh họa trong sơ đồ sau. Sau khi một function resource được tạo, nó có thể được tái sử dụng cho các invocation tiếp theo, giúp đảm bảo hiệu suất và tính cô lập ổn định cho từng tenant.

**Kiến trúc authoring và lifecycle management với Amazon EKS**

## Kiến trúc runtime

Tại runtime, khi Launchpad cần thực thi một hàm, hệ thống sẽ gọi Lambda Invoke API. Trước khi hàm được kích hoạt, dịch vụ runtime multi-tenant thực hiện kiểm tra tenancy để đảm bảo yêu cầu xuất phát từ một tenant được ủy quyền, thông qua quá trình xác thực token. Sau khi xác thực thành công, dịch vụ sẽ invoke hàm Lambda tương ứng. Trong trường hợp các hàm được lưu trữ trong một AWS account khác, dịch vụ runtime multi-tenant sử dụng một AWS Identity and Access Management (IAM) role để assume các quyền cần thiết và thực hiện lời gọi đến AWS Lambda thông qua AWS SDK. Chuỗi tương tác giữa các thành phần trong quá trình này được thể hiện trong sơ đồ kiến trúc sau.

**Kiến trúc runtime và flow invocation với IAM cross-account access**

Workflow bao gồm các bước sau:

1. Yêu cầu user đến sẽ reach dịch vụ application gateway.
2. Application gateway xác thực yêu cầu bằng dịch vụ tenancy security.
3. Sau khi được xác thực, yêu cầu được chuyển tiếp đến dịch vụ runtime multi-tenant.
4. Dịch vụ runtime multi-tenant xác thực token được cung cấp và thực hiện kiểm tra tenancy. Điều này đảm bảo các tenant chỉ có thể invoke các hàm riêng mà họ có quyền (ví dụ: các hàm họ sở hữu).
5. Pod dịch vụ runtime multi-tenant assume IAM role cần thiết để invoke hàm Lambda tenant-specific trong một AWS account khác.
6. Pod dịch vụ runtime multi-tenant invoke hàm Lambda cần thiết.

Invoke platform API từ mã tùy chỉnh đơn giản như khi kết nối với bất kỳ external API nào khác. Mã tùy chỉnh có thể xác thực với nền tảng thông qua OAuth2. Để tạo điều kiện cho việc xác thực, developer có thể truyền credentials như các tham số input cho hàm từ nền tảng cốt lõi. Sau đó, developer có thể tạo một record tương ứng (được cô lập bởi tenant) trong nền tảng để lưu trữ credentials cho từng hàm, và truyền các credentials này như tham số input trong quá trình invocation.

## Observability kiến trúc phân tán

Vận hành một kiến trúc phân tán chạy mã không đáng tin cậy qua nhiều AWS accounts đòi hỏi một chiến lược observability toàn diện. Cách tiếp cận của Launchpad kết hợp logging và monitoring tập trung với tổng hợp cross-account để cung cấp một góc nhìn vận hành thống nhất của nền tảng.

Kiến trúc monitoring sử dụng CloudWatch Metrics để quan sát các hàm Lambda, tổng hợp dữ liệu thông qua một lớp observability tập trung. Thiết lập này cho phép các platform operators tương quan metrics của hàm Lambda với các dịch vụ nền tảng cốt lõi chạy trên Amazon EKS. Launchpad cũng thu thập telemetry per-function như số lượng invocations, tỷ lệ lỗi, và thời gian thực thi, cho phép quan sát metrics per-tenant. Những chiều dữ liệu telemetry này cung cấp cả góc nhìn monitoring toàn nền tảng (platform-wide) và góc nhìn cụ thể theo từng tenant (tenant-specific).

Để logging và troubleshooting, Launchpad triển khai một pipeline logging thống nhất tổng hợp logs hàm Lambda với logs application gateway và runtime service. Mỗi yêu cầu chảy qua hệ thống mang một correlation ID, vì vậy các operators có thể theo dõi đường dẫn thực thi qua các dịch vụ SaaS cốt lõi và vào các tenant functions chạy trong AWS account chạy tenant Lambda functions.

Với kiến trúc observability đa tầng (multi-layer) này, Launchpad có thể duy trì hiệu suất vận hành xuất sắc trong khi vẫn đảm bảo an toàn cho việc chạy mã tenant ở quy mô lớn. Các đánh giá vận hành định kỳ thúc đẩy cải tiến liên tục trong phạm vi coverage monitoring và quy trình phản ứng sự cố (incident response). Việc triển khai các hàm Lambda per-tenant cũng cho phép Launchpad sử dụng tags phân bổ chi phí đặc thù cho từng tenant, giúp họ hiểu rõ hơn footprint chi phí của việc chạy mã tùy chỉnh tenant, đồng thời tăng khả năng theo dõi và tối ưu hóa chi phí vận hành trên toàn hệ thống.

## Best practices

Khi xây dựng một giải pháp SaaS, việc duy trì một code base cốt lõi thống nhất là điều cần thiết cho khả năng mở rộng và quản lý. Triển khai các variations per-tenant trong mã nền tảng cốt lõi có thể dẫn đến sự phức tạp trong bảo trì và technical debt. Thay vào đó, hãy thiết kế giải pháp SaaS của bạn có các extension points, cho phép các tenant của bạn đưa mã tùy chỉnh của họ vào các điểm cụ thể trong workflow, kích hoạt tùy chỉnh mà không ảnh hưởng đến khả năng bảo trì của nền tảng. Pattern này đảm bảo nền tảng SaaS cốt lõi vẫn sạch sẽ và được tiêu chuẩn hóa trong khi cung cấp tính linh hoạt mà khách hàng yêu cầu.

Các best practices bổ sung bao gồm:

- Sử dụng các AWS account riêng biệt để chạy các hàm Lambda với mã không đáng tin cậy do tenant cung cấp, nhằm đảm bảo chúng được cô lập hoàn toàn khỏi mã nền tảng SaaS cốt lõi.
- Cấp quyền truy cập tối thiểu cần thiết (principle of least privilege) cho execution role được gán cho hàm. Mã tùy chỉnh chạy trong môi trường thực thi chỉ nên có quyền được định nghĩa trong execution role khi gọi đến AWS API endpoints. Nếu hàm không cần truy cập AWS API, hãy xóa tất cả quyền khỏi execution role và thêm một chính sách AWSDenyAll rõ ràng.
- Sử dụng các hàm Lambda riêng biệt cho từng đoạn mã và từng tenant, đảm bảo mức độ cô lập cross-tenant cao nhất. Không tái sử dụng tài nguyên hoặc môi trường thực thi giữa các hàm khác nhau.
- Sử dụng Lambda layers trong trường hợp cần thêm lớp mã do vendor cung cấp, giúp tách biệt hoàn toàn với mã không đáng tin cậy từ tenant.
- Triển khai các kiểm soát bảo mật bổ sung, chẳng hạn như sử dụng Amazon Virtual Private Cloud (Amazon VPC) để hạn chế truy cập mạng, và VPC Flow Logs để giám sát lưu lượng và hoạt động mạng.

## Kết luận

Việc triển khai một môi trường thực thi mã không đáng tin cậy an toàn trong các nền tảng SaaS giải quyết một nhu cầu quan trọng cho tùy chỉnh tenant trong khi duy trì tính toàn vẹn kiến trúc. AWS Lambda cung cấp một mô hình cô lập tích hợp, kiểm soát bảo mật fine-grained, và khả năng mở rộng serverless, cho phép các nhà cung cấp SaaS như Launchpad giải quyết yêu cầu thực thi mã do tenant cung cấp trong môi trường multi-tenant, đồng thời cung cấp khả năng tùy chỉnh mạnh mẽ trong khi vẫn duy trì ranh giới bảo mật nghiêm ngặt và hiệu quả vận hành.

Mẫu pattern kiến trúc này cho phép các nhà cung cấp SaaS tập trung vào phát triển nền tảng cốt lõi, trong khi vẫn tự tin hỗ trợ các workflows tenant-specific thông qua môi trường thực thi Lambda an toàn và có thể mở rộng.

Để tìm hiểu thêm, hãy tham khảo white paper Security Overview of AWS Lambda. Để biết thêm các mẫu kiến trúc serverless, hãy xem Serverlessland.com.

## Về các tác giả

**Anton Aleksandrov**  
Anton là Principal Solutions Architect chuyên về AWS Serverless và kiến trúc Event-Driven. Với hơn hai thập kỷ kinh nghiệm về kỹ thuật và kiến trúc, anh làm việc với các khách hàng ISV và SaaS lớn để thiết kế các giải pháp đám mây có khả năng mở rộng cao, sáng tạo và an toàn.

**Giridhar Ramadhenu**  
Giridhar là một Software Architect dày dạn kinh nghiệm với hơn hai thập kỷ chuyên môn trong ngành, chuyên về nhiều phong cách kiến trúc như microservices, event-driven và layered architectures. Là Fellow Software Architect cho Launchpad tại Pegasystems và thành viên chủ chốt của Architecture Guild, Giridhar đóng vai trò then chốt trong việc định hình kiến trúc của các sản phẩm khác nhau. Những đóng góp của anh bao trùm toàn bộ technology stack — từ AWS và cơ sở hạ tầng cloud dựa trên Kubernetes đến các lĩnh vực trọng yếu như tích hợp API, dữ liệu, quản lý case và bảo mật.

**Rajesh Kumar Maram**  
Rajesh Kumar Maram là Senior Principal Software Engineer cho Launchpad tại Pegasystems, với hơn một thập kỷ kinh nghiệm trong ngành. Anh dẫn dắt việc giải quyết các thách thức kỹ thuật phức tạp và thúc đẩy đổi mới thông qua các giải pháp sáng tạo. Rajesh đam mê khám phá các công nghệ mới nổi và đánh giá khả năng áp dụng của chúng cho các use case kinh doanh của Pega. Anh đã thử nghiệm và tích hợp nhiều dịch vụ AWS vào các microservices khác nhau trong Launchpad.

**Anubhav Sharma**  
Anubhav Sharma là Principal Solutions Architect tại AWS với hơn hai thập kỷ kinh nghiệm trong việc lập trình và thiết kế các ứng dụng business-critical. Được biết đến với niềm đam mê học hỏi và đổi mới, Anubhav đã dành sáu năm qua tại AWS làm việc chặt chẽ với nhiều independent software vendors (ISVs) và doanh nghiệp. Anh chuyên hướng dẫn các tổ chức này trong hành trình xây dựng, triển khai và vận hành các giải pháp SaaS trên AWS.

**TAGS:** AWS Lambda, Serverless, SaaS Architecture, Multi-tenancy, Security

---

*Bài viết gốc: How Launchpad from Pega enables secure SaaS extensibility with AWS Lambda*
