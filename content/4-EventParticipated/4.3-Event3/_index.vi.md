---
title: "Event 3"
date: 2024-01-01T00:00:00+07:00
weight: 1
chapter: false
pre: " <b> 4.3. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

# Báo cáo Tổng kết: "Workshop AWS Security & Compliance"

**Ngày & Giờ:** Thứ Bảy, 26/10/2025, 2:00 PM – 5:00 PM  
**Địa điểm:** Sự kiện Online (Zoom)  
**Vai trò:** Người tham gia Workshop  
**Đơn vị tổ chức:** First Cloud Journey, AWS User Group Vietnam

### Mục Đích Sự Kiện

Workshop này tập trung vào các phương pháp tốt nhất về bảo mật AWS và yêu cầu tuân thủ. Sự kiện nhằm giúp người tham gia hiểu cách xây dựng hạ tầng cloud an toàn, triển khai giám sát bảo mật và duy trì tuân thủ các tiêu chuẩn ngành sử dụng các dịch vụ bảo mật AWS.

### Tổng quan Chương trình

**2:00 – 2:15 PM | Đăng ký & Giới thiệu**
- Tổng quan workshop và nền tảng bảo mật
- Giới thiệu AWS Shared Responsibility Model
- Tổng quan về bối cảnh bảo mật trong cloud computing
- Thảo luận các thách thức bảo mật phổ biến

**2:15 – 3:30 PM | Dịch vụ Bảo mật AWS**

**AWS IAM (Identity and Access Management)**
- IAM best practices và policies
- Multi-Factor Authentication (MFA)
- IAM roles và service accounts
- Permission boundaries và SCPs

**Giám sát Bảo mật AWS**
- Amazon GuardDuty cho phát hiện mối đe dọa
- AWS Security Hub cho cái nhìn bảo mật tập trung
- AWS CloudTrail cho audit logging
- Amazon Macie cho bảo vệ và khám phá dữ liệu

**Bảo mật Mạng**
- VPC security best practices
- Security Groups và Network ACLs
- AWS WAF (Web Application Firewall)
- AWS Shield cho bảo vệ DDoS

**3:30 – 3:45 PM | Giải lao**
- Phiên Q&A và networking

**3:45 – 5:00 PM | Tuân thủ & Bảo vệ Dữ liệu**

**Dịch vụ Mã hóa AWS**
- AWS KMS (Key Management Service)
- Mã hóa at rest và in transit
- AWS Certificate Manager
- AWS Secrets Manager

**Compliance Frameworks**
- Tổng quan AWS compliance programs
- AWS Artifact cho compliance reports
- Cân nhắc GDPR, HIPAA, PCI DSS
- Tự động hóa compliance với AWS Config

**Hands-on Lab: Bảo mật Ứng dụng Web**
- Thiết lập security monitoring với GuardDuty
- Triển khai encryption với KMS
- Cấu hình WAF rules
- Kiểm tra security controls

### Điểm Nổi bật

- **Defense in Depth:** Nhiều lớp security controls
- **Giám sát Tự động:** GuardDuty và Security Hub cung cấp phát hiện mối đe dọa liên tục
- **Encryption Everywhere:** Triển khai mã hóa dễ dàng sử dụng AWS services
- **Compliance Dễ dàng hơn:** AWS services giúp đáp ứng yêu cầu compliance
- **Audit Trail:** CloudTrail cung cấp activity logging hoàn chỉnh

### Những Gì Học Được

- Hiểu AWS Shared Responsibility Model là nền tảng
- IAM policies nên tuân theo nguyên tắc least privilege
- Encryption nên được enable mặc định cho dữ liệu nhạy cảm
- Security monitoring phải liên tục, không phải định kỳ
- Compliance là trách nhiệm chung giữa AWS và khách hàng
- Security automation giảm lỗi con người

### Ứng Dụng vào Thực Tập

**Ứng dụng Ngay lập tức:**
- Xem xét và cải thiện IAM policies trong các dự án thực hành
- Enable GuardDuty cho threat detection
- Triển khai encryption cho S3 buckets và RDS databases
- Thiết lập CloudTrail cho audit logging

**Security Best Practices:**
- Luôn sử dụng MFA cho privileged accounts
- Thường xuyên rotate access keys và credentials
- Triển khai least privilege access control
- Enable encryption at rest và in transit
- Giám sát security events liên tục

**Cân nhắc Compliance:**
- Hiểu compliance frameworks nào áp dụng
- Sử dụng AWS Config cho compliance automation
- Tài liệu hóa security controls và processes
- Security audits và reviews định kỳ

### Trải nghiệm Cá nhân

**Workshop AWS Security & Compliance** cung cấp insights giá trị về cloud security.

**Hiểu Nền tảng Bảo mật:**
- Shared Responsibility Model làm rõ trách nhiệm AWS vs khách hàng
- Học được rằng security phải được xây dựng vào kiến trúc từ đầu
- Hiểu về cách tiếp cận defense-in-depth

**Công cụ Bảo mật Thực hành:**
- Demo GuardDuty cho thấy threat detection hoạt động tự động
- Security Hub cung cấp security dashboard tập trung
- Triển khai KMS encryption khá đơn giản
- Cấu hình WAF minh họa web application protection

**Nhận thức Compliance:**
- Học về các compliance frameworks chính
- Hiểu cách AWS giúp với compliance
- AWS Artifact cung cấp quyền truy cập compliance reports
- Config rules có thể tự động hóa compliance checks

**Kỹ năng Thực tế:**
- Hiểu cơ bản về security service architecture
- Khả năng triển khai common security controls
- Biết nơi tìm compliance documentation
- Hiểu các công cụ security monitoring

**Thách thức:**
- Các khái niệm security có thể phức tạp và overwhelming
- Cân bằng security với usability và cost
- Hiểu security services nào dùng khi nào
- Theo kịp các mối đe dọa bảo mật đang phát triển

#### Một số hình ảnh khi tham gia sự kiện
*Screenshot workshop sẽ được thêm vào đây*

> Tổng thể, workshop này nhấn mạnh rằng security không phải tùy chọn mà là nền tảng của cloud computing. Các demo thực tế và hands-on labs giúp làm cho các khái niệm security trở nên cụ thể hơn và cung cấp nền tảng để triển khai hạ tầng AWS an toàn.
