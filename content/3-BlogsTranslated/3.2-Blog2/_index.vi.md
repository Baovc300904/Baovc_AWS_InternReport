---
title: "Blog 2"
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

# Tối ưu hóa hoạt động đội xe sử dụng Amazon SageMaker AI và Amazon Bedrock

**Tác giả: Manny Sidhu và Stephen Mistele vào ngày 28 THÁNG 5 2025**  
**Danh mục: Amazon Bedrock, Amazon SageMaker AI, Architecture, AWS IoT Greengrass, Generative AI, Intermediate (200)**

## Giới Thiệu

Hàng năm tại Hoa Kỳ, lái xe mất tập trung cướp đi hàng nghìn sinh mạng và gây ra thiệt hại tài chính khổng lồ. Hơn 1,6 triệu vụ tai nạn hàng năm là do việc sử dụng điện thoại di động khi lái xe, và 1,5 triệu vụ khác do tài xế buồn ngủ ngủ gật sau tay lái. Những vụ tai nạn tàn khốc—và có thể ngăn ngừa được—này đã thúc đẩy một nỗ lực lớn để tăng cường an toàn cho tài xế trên toàn quốc.

Sáng kiến này đặc biệt quan trọng trong lĩnh vực đội xe thương mại, vì các vụ tai nạn liên quan đến xe tải hạng nặng thường nguy hiểm hơn và có thể gây thiệt hại lên đến hàng trăm nghìn đô la. Bài viết này khám phá một giải pháp sáng tạo tận dụng Amazon SageMaker AI và Amazon Bedrock nhằm cách mạng hóa quy trình huấn luyện tài xế và nâng cao hiệu quả vận hành đội xe. Bằng cách khai thác sức mạnh của machine learning và artificial intelligence, chúng tôi minh họa cách các nhà điều hành đội xe có thể chuyển đổi cảnh quay dashcam thô thành những hiểu biết có thể hành động, từ đó trao quyền giám sát tài xế theo thời gian thực và triển khai các biện pháp an toàn chủ động — giúp giảm đáng kể các vụ tai nạn tốn kém. Cách tiếp cận của chúng tôi kết hợp AWS Artificial Intelligence (AI) và các dịch vụ Internet of Things (IoT) để xây dựng một giải pháp toàn diện, không chỉ phát hiện hành vi lái xe mất tập trung mà còn liên tục cải thiện hiệu suất theo thời gian. Thông qua giải pháp này, chúng tôi hướng đến việc chứng minh rằng các nhà quản lý đội xe có thể giảm thiểu đáng kể sự cố lái xe mất tập trung, nâng cao hiệu quả hoạt động, và tối ưu chi phí trong vận hành đội xe thương mại.

## Thách thức: Quản lý hiệu quả nhiều nguồn cấp dữ liệu dashcam từ đội xe thương mại

Các xe thương mại ngày nay được trang bị hệ thống đa camera cung cấp phạm vi bao quát toàn diện: camera hướng vào trong giám sát hành vi tài xế, camera hướng ra ngoài theo dõi giao thông đối diện, và camera bên/phía sau phát hiện giao thông chéo và khả năng va chạm phía sau. Lượng dữ liệu video khổng lồ được tạo ra hàng ngày bởi hàng nghìn xe tạo ra những thách thức quản lý và phân tích đáng kể. Trong khi các nhà điều hành đội xe truyền thống sử dụng cảnh quay dashcam này cho mục đích phản ứng – như báo cáo thực thi pháp luật, khiếu nại bảo hiểm, và thanh minh tài xế – nhiều tổ chức đang bỏ lỡ một cơ hội đáng kể để tận dụng dữ liệu này. Khi các đội xe thương mại tích lũy nhiều dặm hơn, họ tạo ra các bộ dữ liệu phong phú có thể được sử dụng để huấn luyện các mô hình AI có khả năng tạo điều kiện cho những cải tiến an toàn chủ động.

Trong bài viết này, chúng ta sẽ khám phá cách tối đa hóa giá trị của cảnh quay dashcam thông qua các thực hành tốt nhất để triển khai và quản lý hệ thống Computer Vision trong hoạt động đội xe thương mại. Chúng ta sẽ chứng minh cách xây dựng và triển khai các mô hình machine learning dựa trên edge cung cấp cảnh báo thời gian thực cho các hành vi lái xe mất tập trung, trong khi hiệu quả thu thập, xử lý, và phân tích cảnh quay để huấn luyện các mô hình AI này. Cách tiếp cận này chuyển đổi hoạt động đội xe từ quản lý sự cố phản ứng sang nâng cao an toàn chủ động, giúp các tổ chức chuyển đổi dữ liệu video thô thành những hiểu biết có thể hành động nhằm giảm các sự cố an toàn và cải thiện hiệu quả hoạt động đội xe tổng thể và hiệu quả chi phí.

## Tổng quan giải pháp

Một Sự cố Lái xe Mất tập trung có thể xảy ra khi tài xế tham gia vào các hành vi không an toàn như chạy quá tốc độ, dừng lăn, phanh gấp, và tăng tốc mạnh. Các nhà quản lý đội xe cần hiểu không chỉ điều gì đã xảy ra trong các sự cố này, mà còn trạng thái chú ý của tài xế – liệu họ có tập trung vào đường hay bị phân tâm bởi các hoạt động như sử dụng điện thoại di động, ăn uống, hoặc trải qua mệt mỏi phổ biến trong lái xe đường dài.

Giải pháp của chúng tôi tận dụng các dịch vụ AWS để tạo ra một quy trình làm việc end-to-end có khả năng phát hiện và giảm thiểu lái xe mất tập trung. Các bước liên quan bao gồm:

1. Thu thập, tiếp nhận, và gán nhãn sự cố
2. Huấn luyện, tối ưu hóa, và triển khai mô hình
3. Kiểm tra và cải thiện liên tục

## Giải pháp deep dive

Giải pháp này dựa vào sự kết hợp của AWS IoT, AI và các dịch vụ generative AI để xây dựng một giải pháp có thể mở rộng và hiệu quả về chi phí. Hãy bắt đầu bằng cách xem xét kiến trúc giải pháp ở mức cao và xây dựng giải pháp từng bước.

### Thu thập, tiếp nhận, và gán nhãn sự cố

Để bắt đầu quá trình tiếp nhận video từ camera hành trình (dashcam) của tài xế vào cloud, chúng ta thu thập nguồn cấp dữ liệu dashcam bằng IoT Greengrass Kinesis Video Streamer Component. Video được truyền vào AWS Cloud thông qua Kinesis Video Streams và lưu trữ trong Amazon S3 bằng cách tận dụng Kinesis Firehose. Các video sau đó được chuyển đổi thành các khung hình riêng lẻ, được phân tích bởi mô hình Amazon Bedrock Nova Pro để xác định mức độ mất tập trung của tài xế, và được sắp xếp bởi hàm AWS Lambda vào bucket S3 dựa trên kết quả phân tích. Các khung hình đã được sắp xếp này tiếp tục được sử dụng để huấn luyện mô hình AI triển khai ở edge, nhằm phát hiện hành vi lái xe mất tập trung trong thời gian thực.

Từ góc độ bảo mật, việc mã hóa dữ liệu trong các bucket Amazon S3 bằng AWS Key Management Service (KMS) là một thực hành tốt. Bạn có thể thực thi điều này bằng cách thiết lập SSE-KMS làm phương pháp mã hóa mặc định để tự động mã hóa tất cả các đối tượng được tải lên. Đồng thời, nên triển khai các vai trò AWS Identity & Access Management (IAM) chi tiết để cấp quyền truy cập có phạm vi giới hạn cho hình ảnh và video. Đối với dữ liệu đang được truyền giữa edge và cloud, bạn có thể sử dụng chứng chỉ AWS IoT Greengrass để mã hóa luồng dữ liệu và thực thi xác minh danh tính. Những biện pháp này giúp tăng cường bảo mật tổng thể và ngăn chặn truy cập trái phép vào dữ liệu nhạy cảm.

**Kiến trúc edge-to-cloud cho giám sát tài xế thời gian thực sử dụng AWS IoT, Kinesis, và các dịch vụ ML**

Với quy trình này, chúng ta liên tục thu thập dữ liệu từ đội xe thương mại của mình (trong khi luôn chú ý đến bảo mật). Dữ liệu này được tự động phân loại và gán nhãn dựa trên phân tích từ mô hình Nova Pro của chúng ta, và được lưu trữ thuận tiện trong S3, cho phép chúng ta huấn luyện một mô hình AI một cách liền mạch – một quy trình mà chúng ta sẽ mô tả tiếp theo.

### Huấn luyện, tối ưu hóa, và triển khai mô hình

Sơ đồ sau minh họa quy trình huấn luyện và triển khai mô hình phát hiện tài xế mất tập trung, được vận hành bên trong Amazon SageMaker Pipelines Workflow, cho phép điều phối liền mạch các dịch vụ Amazon SageMaker AI khác. Quy trình này bắt đầu với các hình ảnh tài xế đã được gán nhãn lưu trữ trong Amazon S3, được tạo ra từ quy trình thu thập và xử lý dữ liệu đã mô tả trước đó. Bộ dữ liệu gán nhãn – bao gồm các hình ảnh được phân loại là "mất tập trung" hoặc "không mất tập trung" – được sử dụng để huấn luyện mô hình ResNet50 thông qua Amazon SageMaker Training Jobs chạy trên instance Trn1 nhằm tối ưu hiệu suất và chi phí. Trong quá trình huấn luyện, mô hình học cách nhận diện hành vi lái xe mất tập trung. Sau khi huấn luyện xong, mô hình được lượng tử hóa thành INT8 bằng SageMaker Processing Jobs và được tối ưu hóa cho phần cứng edge cụ thể bằng SageMaker Neo. Mô hình tối ưu sau đó được lưu trữ trong SageMaker Model Registry để phục vụ quản lý phiên bản và kiểm soát chất lượng – điều này rất hữu ích khi mô hình được cải tiến với dữ liệu huấn luyện mới. Cuối cùng, mô hình được đẩy lên Amazon S3, nơi AWS IoT Greengrass có thể khởi tạo và triển khai mô hình đến các thiết bị edge trong đội xe.

Chạy trên edge, mô hình thực hiện suy luận nhiều lần trong một giây trên các khung hình từ dashcam hướng vào trong. (Tốc độ suy luận được tính toán giả định edge compute có thông số kỹ thuật tương đương với một thiết bị lớp Raspberry-Pi.) Nếu tài xế được phát hiện là mất tập trung, hệ thống cảnh báo tài xế bằng tiếng ồn. (ví dụ: tài xế đang ngủ gật, và cảnh báo đánh thức họ).

**Kiến trúc AWS end-to-end cho phát hiện tài xế mất tập trung: từ huấn luyện mô hình đến triển khai edge**

Với quy trình này, chúng ta đã thành công tận dụng bộ dữ liệu mà chúng ta tạo ra trong sơ đồ đầu tiên để huấn luyện, tối ưu hóa, và triển khai mô hình tùy chỉnh của chúng ta đến 'edge' – trong trường hợp này, đến từng xe trong đội xe của chúng ta. Mô hình của chúng ta giờ đây đang cảnh báo tài xế về hành vi nguy hiểm và giúp ngăn ngừa va chạm một cách chủ động. Nhưng mô hình của chúng ta có thể không hoàn hảo – có thể nó bỏ lỡ một hành vi nguy hiểm không có trong bộ dữ liệu huấn luyện, hoặc cảnh báo không cần thiết. Để xác thực mô hình của chúng ta hoạt động tốt và cải thiện thêm nó để giảm lỗi, chúng ta triển khai các thủ tục kiểm tra và cải thiện liên tục.

### Kiểm tra và cải thiện liên tục

Chúng ta cần tiếp tục tiếp nhận dữ liệu dashcam tài xế và so sánh các dự đoán của mô hình edge với nguồn 'chân lý cơ bản' ban đầu của chúng ta – Nova Pro.

Hệ thống thu thập các khung hình xác thực mô hình trong hai trường hợp: khi telemetry của xe phát hiện sự cố (chẳng hạn như phanh gấp hoặc va chạm), hoặc khi mô hình edge xác định tài xế có dấu hiệu mất tập trung. Những khung hình này được gửi đến Amazon Bedrock để "kiểm tra sự thật", nhằm đánh giá xem mô hình edge có hoạt động tối ưu hay không. Kết quả so sánh giữa Amazon Bedrock và mô hình edge được lưu trữ trong bucket Amazon S3 chuyên dụng cho việc đánh giá mô hình. Khi đã thu thập đủ dữ liệu xác thực mới, hoặc khi độ đồng thuận giữa mô hình edge và Amazon Bedrock giảm xuống dưới ngưỡng cho phép, Amazon EventBridge sẽ kích hoạt SageMaker Pipelines Workflow đã mô tả trước đó để fine-tune, tối ưu hóa và triển khai lại mô hình được cải thiện đến các thiết bị edge, giờ đây được hỗ trợ bởi "dữ liệu bất đồng" mới được thu thập.

**Vòng lặp phản hồi edge-to-cloud cho xác thực mô hình ML sử dụng AWS IoT, Bedrock, và các dịch vụ SageMaker**

Chúng ta cũng nên thực hiện phân tích so sánh mô hình mới của chúng ta với các mô hình lịch sử được lưu trữ trong Amazon SageMaker Model Registry để xác thực rằng mô hình mới nhất của chúng ta thực sự hoạt động tốt hơn các mô hình lịch sử, xác minh chúng ta không thấy sự thoái lui. Nếu mô hình mới nhất của chúng ta không vượt trội hơn các mô hình lịch sử, chúng ta không nên triển khai nó, và thay vào đó điều tra xem chúng ta có đang gặp phải overfitting hoặc dữ liệu huấn luyện xấu không. Tóm lại, giờ đây chúng ta có một mô hình chạy bên trong các xe đội xe có khả năng cảnh báo tài xế về hành vi không an toàn. Điều này có thể hiệu quả giảm các vụ tai nạn lái xe buồn ngủ bằng cách giữ tài xế tỉnh táo và cảnh giác, đồng thời cảnh báo tài xế về các quyết định không an toàn như ăn uống hoặc sử dụng điện thoại di động khi lái xe. Hệ thống này cũng tự huấn luyện và tự cải thiện, vì vậy nó sẽ tiếp tục trở nên tốt hơn theo thời gian. Ngoài ra, các công ty quản lý đội xe có thể tổng hợp dữ liệu an toàn và thưởng cho các tài xế hàng đầu để khuyến khích thêm thói quen lái xe an toàn.

## Kết luận

Trong bài viết này, chúng ta đã khám phá một giải pháp sáng tạo tận dụng các dịch vụ AWS để cách mạng hóa quy trình huấn luyện tài xế và vận hành đội xe thương mại. Bằng cách kết hợp sức mạnh của Amazon SageMaker và Amazon Bedrock với khả năng của AWS IoT và edge computing, chúng ta đã chứng minh cách xây dựng một giải pháp toàn diện, có thể mở rộng, giúp giám sát và cải thiện hành vi tài xế theo thời gian thực. Giải pháp này giải quyết hiệu quả các thách thức trong việc quản lý lượng lớn dữ liệu video từ camera hành trình của đội xe thương mại, chuyển đổi dữ liệu thô thành những hiểu biết có thể hành động. Thông qua việc triển khai quy trình làm việc end-to-end bao gồm thu thập dữ liệu, phân loại, huấn luyện mô hình, triển khai và cải thiện liên tục, các nhà điều hành đội xe có thể chuyển đổi từ mô hình quản lý phản ứng sang mô hình an toàn chủ động, góp phần giảm thiểu tai nạn, tối ưu chi phí và nâng cao hiệu quả vận hành.

Lợi ích của cách tiếp cận này bao gồm:

- **Nâng cao an toàn:** Phát hiện thời gian thực các hành vi lái xe mất tập trung cho phép can thiệp và huấn luyện ngay lập tức.
- **Cải thiện hiệu quả:** Phân tích tự động cảnh quay dashcam giảm thời gian xem xét thủ công và chi phí.
- **Khả năng mở rộng:** Giải pháp có thể xử lý các đội xe lớn và bộ dữ liệu ngày càng tăng một cách dễ dàng.
- **Cải thiện liên tục:** Hệ thống học hỏi và thích ứng theo thời gian, trở nên chính xác và hiệu quả hơn.
- **Hiệu quả chi phí:** Bằng cách tận dụng edge computing và các mô hình được tối ưu hóa, giải pháp giảm thiểu chi phí tính toán.

Khi ngành vận tải tiếp tục phát triển, các giải pháp như thế này sẽ đóng vai trò quan trọng trong việc cải thiện an toàn đường bộ, giảm chi phí hoạt động, và nâng cao hiệu suất đội xe tổng thể. Bằng cách khai thác sức mạnh của AI và cloud computing, các nhà điều hành đội xe có thể tạo ra môi trường lái xe an toàn hơn, hiệu quả hơn mang lại lợi ích không chỉ cho doanh nghiệp của họ mà còn cho toàn xã hội. Tương lai của hoạt động đội xe đã ở đây, và nó được điều khiển bởi các hệ thống thông minh, dựa trên dữ liệu biến mỗi dặm lái xe thành cơ hội cải thiện và đổi mới.

Tìm hiểu thêm bằng cách khám phá các mẫu mã AWS để xây dựng chuyên môn thực hành với Amazon SageMaker. Trải nghiệm dịch vụ thông qua các ví dụ thực tế minh họa cách tối ưu hóa quy trình huấn luyện và triển khai mô hình cho nhiều trường hợp sử dụng khác nhau. Đồng thời, hãy khám phá lợi thế tài chính bằng cách thực hiện phân tích TCO kinh tế đám mây, so sánh cơ sở hạ tầng truyền thống với các dịch vụ được quản lý của SageMaker. Bài phân tích này cho thấy cách SageMaker giúp giảm thiểu chi phí ẩn trong khi tăng tốc chu kỳ phát triển machine learning (ML) của bạn.

**Sẵn sàng thực hiện bước tiếp theo?** Kết nối với AWS Solutions Architect của bạn để sắp xếp một buổi SageMaker AI Immersion Day được tùy chỉnh theo những thách thức cụ thể của đội ngũ bạn. Những phiên do các chuyên gia AWS dẫn dắt này cung cấp hướng dẫn cá nhân hóa, giúp bạn triển khai Amazon SageMaker một cách hiệu quả và phù hợp với bối cảnh tổ chức của mình.

Để tìm hiểu sâu hơn về các dịch vụ AWS liên quan, bạn có thể khám phá:
- **Amazon Kinesis Video Streams** – thu thập và xử lý luồng video thời gian thực.
- **AWS IoT Greengrass** – triển khai và quản lý mô hình AI trên thiết bị edge.
- **Amazon Bedrock** – xây dựng và mở rộng các ứng dụng AI tạo sinh.

## Về các tác giả

**Manny Sidhu**  
Manny Sidhu là Enterprise Solutions Architect tại Amazon Web Services ở khu vực San Francisco Bay, California. Manny chuyên về IoT, Generative/AI & Supply Chain. Anh ấy thích geek out, các hoạt động ngoài trời và du lịch cùng gia đình.

**Stephen Mistele**  
Stephen Mistele là AI Infrastructure Engineer tại Amazon Web Services ở Seattle. Là thành viên của team SageMaker AI Training, anh ấy chuyên về xây dựng cơ sở hạ tầng để hỗ trợ các khối lượng công việc ML quy mô lớn. Khi không làm việc, bạn có thể bắt gặp anh ấy trên các sườn dốc trượt tuyết.

**TAGS:** Amazon Bedrock, Amazon SageMaker AI, AWS IoT Greengrass, Machine Learning, Computer Vision
