---
title: "Blog 1"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

# Khám phá các tính năng mới nhất của Amazon Q Developer CLI

**Tác giả: Brian Beach vào ngày 20 THÁNG 5 2025**  
**Danh mục: Amazon Q Developer, Announcements**

## Giới thiệu

Đã vài tuần kể từ bài viết cuối cùng của tôi về Amazon Q Developer Command Line Interface (CLI), và tôi rất hào hứng được chia sẻ tất cả các tính năng mới tuyệt vời và các cải tiến mà team đã làm việc. Amazon Q Developer CLI đã phát triển nhanh chóng với trọng tâm là nâng cao trải nghiệm người dùng, cải thiện quản lý context, và thêm các khả năng mạnh mẽ mới. Trong bài viết này, tôi sẽ hướng dẫn bạn qua những thay đổi quan trọng nhất khiến Amazon Q Developer CLI trở nên mạnh mẽ và thân thiện với người dùng hơn.

## Conversation Persistence

Một trong những tính năng được yêu cầu nhiều nhất là khả năng lưu trữ các cuộc hội thoại, và tôi rất vui được chia sẻ rằng điều này hiện đã có sẵn. Với lệnh `q chat --resume` mới, các cuộc hội thoại của bạn giờ đây được tự động lưu theo thư mục làm việc. Điều này có nghĩa là bạn có thể tiếp tục ngay từ nơi bạn đã dừng lại khi quay lại một dự án, mà không cần phải xây dựng lại context hoặc lặp lại thông tin.

Q Developer cũng đã thêm hai lệnh mới để cung cấp cho bạn nhiều quyền kiểm soát hơn đối với trạng thái cuộc hội thoại của mình:

- `/save` cho phép bạn lưu rõ ràng trạng thái cuộc hội thoại hiện tại
- `/load` cho phép bạn khôi phục một cuộc hội thoại đã lưu trước đó

Những lệnh này giúp việc quản lý nhiều luồng hội thoại liên quan đến các khía cạnh khác nhau của dự án của bạn trở nên dễ dàng hơn. Bạn có thể lưu một cuộc hội thoại về một tính năng, chuyển sang làm việc trên một thứ khác, và sau đó tải cuộc hội thoại trước đó khi bạn sẵn sàng tiếp tục.

## MCP và Tool Use Enhancements

Model Context Protocol (MCP) là một phần quan trọng của Amazon Q Developer CLI, cho phép khả năng mở rộng thông qua các công cụ và server bổ sung. Amazon Q Developer đã thực hiện một số cải tiến về cách các MCP server được tải và quản lý:

Đầu tiên, Q Developer đã triển khai background MCP server loading, điều này cải thiện đáng kể thời gian khởi động cho `q chat`. Thay vì đợi tất cả MCP server khởi tạo trước khi bạn có thể bắt đầu tương tác với Q Developer, CLI giờ đây tải các server ở chế độ nền trong khi bạn bắt đầu cuộc hội thoại. Điều này có nghĩa là bạn có thể bắt đầu làm việc ngay lập tức, với các công cụ trở nên khả dụng khi các server của chúng hoàn tất việc tải.

Team cũng đã thêm một subcommand mới, `q mcp`, cung cấp giao diện chuyên dụng để cập nhật và quản lý cấu hình MCP server của bạn. Điều này giúp việc thêm, xóa, hoặc sửa đổi các MCP server mở rộng khả năng của CLI trở nên dễ dàng hơn.

Để kiểm soát chi tiết hơn về những công cụ nào có thể được sử dụng, Q Developer đã thêm lệnh `/tools` trong `q chat`. Điều này cho phép bạn quản lý quyền cho từng công cụ riêng lẻ, cung cấp cho bạn nhiều quyền kiểm soát hơn về những gì Q Developer có thể làm trong môi trường của bạn. Bạn cũng có thể reset quyền cho một công cụ cụ thể nếu bạn thay đổi ý kiến.

## Improved Context Control

Context là yếu tố quan trọng để tận dụng tối đa Q Developer, và team đã thực hiện một số cải tiến về cách bạn có thể quản lý và xem context:

Việc lựa chọn file trong fuzzy finder của `q chat` giờ đây nhận biết git, giúp việc bao gồm các file liên quan từ repository của bạn trở nên dễ dàng hơn. Điều này đặc biệt hữu ích khi làm việc với các codebase lớn, vì nó giúp bạn tập trung vào những file quan trọng cho tác vụ hiện tại của bạn.

Q Developer đã thêm fuzzy search cho các slash command với `Ctrl + s`, cho phép bạn nhanh chóng tìm và thực thi các lệnh mà không cần nhớ cú pháp chính xác của chúng. Điều này làm cho CLI trở nên dễ tiếp cận hơn, đặc biệt đối với người dùng mới hoặc những người không sử dụng các lệnh nhất định thường xuyên.

Lệnh `/context show --expand` đã được cải thiện để cung cấp thông tin chi tiết hơn về context hiện tại, giúp bạn hiểu Q Developer biết gì về môi trường của bạn. Team cũng đã nâng cao hiển thị context file trong `q chat` để làm cho nó thông tin hơn và dễ đọc hơn.

Một trong những bổ sung thú vị nhất là khả năng mới để thêm context động vào tin nhắn với context hooks. Điều này cho phép CLI tự động bao gồm context liên quan dựa trên cuộc hội thoại của bạn, cải thiện chất lượng phản hồi mà không yêu cầu quản lý context thủ công.

## Context Window Awareness và Optimization

Khi các cuộc hội thoại trở nên dài hơn, việc quản lý context window trở nên ngày càng quan trọng. Q Developer đã thêm hai lệnh mới để giúp với điều này:

- `/usage` hiển thị ước tính sử dụng context window, giúp bạn hiểu bạn đang sử dụng bao nhiều không gian context có sẵn
- `/compact` tóm tắt lịch sử cuộc hội thoại, cho phép bạn giảm kích thước của context trong khi vẫn bảo tồn thông tin quan trọng

Những công cụ này giúp bạn tận dụng tối đa context window có sẵn, đảm bảo rằng Q Developer có quyền truy cập vào thông tin liên quan nhất mà không gặp phải giới hạn token.

## Image Support

Tôi đặc biệt hào hứng thông báo rằng `q chat` giờ đây hỗ trợ hình ảnh! Điều này mở ra một chiều hướng tương tác hoàn toàn mới, cho phép bạn chia sẻ screenshots, sơ đồ, hoặc thông tin hình ảnh khác với Q Developer. Điều này có thể cực kỳ hữu ích cho việc debug các vấn đề UI, thảo luận về các khái niệm thiết kế, hoặc giải thích các ý tưởng phức tạp khó truyền đạt chỉ qua văn bản.

## Editor for Long Prompts

Đối với các truy vấn phức tạp hoặc hướng dẫn chi tiết, bạn có thể muốn nhiều đoạn văn. Q Developer hỗ trợ `Ctrl + j`, cho phép bạn thêm ký tự xuống dòng vào prompt. Ngoài ra, team đã thêm lệnh `/editor`, mở text editor được cấu hình của bạn để soạn thảo prompts. Điều này giúp việc tạo ra các prompt chi tiết, nhiều đoạn hoặc chỉnh sửa và tinh chỉnh câu hỏi của bạn trước khi gửi chúng đến Q Developer trở nên dễ dàng hơn nhiều.

## Expanded Region Support

Tôi vui mừng thông báo rằng Amazon Q Developer đã mở rộng khả năng sẵn có theo khu vực. Người dùng Professional tier giờ đây có thể truy cập Amazon Q Developer trong khu vực Frankfurt (eu-central-1). Việc mở rộng này là một phần của nỗ lực liên tục của Amazon Q Developer nhằm cung cấp độ trễ thấp hơn và dịch vụ tốt hơn cho khách hàng trên toàn cầu. Bằng cách thêm hỗ trợ cho khu vực Frankfurt, Amazon Q Developer trở nên dễ tiếp cận hơn đối với khách hàng châu Âu, cho phép họ hưởng lợi từ độ trễ giảm và hiệu suất được cải thiện.

## Ability to Manage Issues in CLI

Amazon Q Developer đã làm cho việc báo cáo vấn đề trực tiếp từ CLI trở nên dễ dàng hơn với hai tính năng mới:

- Lệnh `/issue` trong `q chat` cho phép bạn tạo các GitHub issue mới
- Công cụ `report_issue` cung cấp một cách có chương trình để Q Developer giúp bạn tạo các báo cáo vấn đề chi tiết

Những tính năng này đơn giản hóa quy trình phản hồi, giúp bạn báo cáo lỗi hoặc yêu cầu tính năng dễ dàng hơn, và giúp team cải thiện CLI dựa trên đầu vào của bạn.

## Keeping Up with Future Changes

Để giúp bạn luôn được thông báo về các tính năng mới và cải tiến, Q Developer đã thêm flag `--changelog` vào lệnh `q version`. Điều này hiển thị change log trực tiếp từ CLI, giúp bạn dễ dàng xem có gì mới mà không cần phải truy cập GitHub repository hoặc đọc các bài viết blog như bài này.

## Kết luận

Amazon Q Developer CLI tiếp tục phát triển nhanh chóng, với các tính năng mới và cải tiến khiến nó trở thành một công cụ thậm chí còn mạnh mẽ hơn cho các nhà phát triển. Từ conversation persistence đến image support, những cập nhật này phản ánh cam kết của Q Developer trong việc xây dựng một CLI giúp bạn làm việc hiệu quả và năng suất hơn trong công việc hàng ngày.

Tôi khuyến khích bạn thử các tính năng mới này bằng cách cài đặt Amazon Q Developer CLI. Cảm ơn bạn vì sự hỗ trợ và phản hồi liên tục, điều này giúp làm cho Amazon Q Developer trở nên tốt hơn mỗi ngày.

**TAGS:** Developer Tools, Development
