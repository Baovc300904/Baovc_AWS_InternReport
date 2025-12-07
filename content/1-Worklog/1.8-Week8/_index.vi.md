---
title: "Worklog Tuần 8"
date: 2024-01-01T00:00:00+07:00
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---
{{% notice warning %}} 
⚠️ **Lưu ý:** Thông tin dưới đây chỉ để tham khảo. Vui lòng **không sao chép nguyên văn** cho báo cáo của bạn, bao gồm cả cảnh báo này.
{{% /notice %}}


### Mục tiêu tuần 8:

* Tìm hiểu về containers và các khái niệm cơ bản về Docker.
* Hiểu về Amazon ECS và container orchestration.
* Triển khai các ứng dụng được đóng gói trong container trên AWS.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tổng quan về AWS Well-Architected Framework, 5 trụ cột: Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization <br> - Xác định vai trò và tầm quan trọng của từng trụ cột trong thiết kế hệ thống | 27/10/2025   | 27/10/2025      | AWS Journey |
| 3   | - Ôn tập Thiết kế Kiến trúc An toàn <br>  IAM, MFA, SCP, Encryption (KMS, TLS/ACM), Security Groups, NACLs, GuardDuty, Shield, WAF, Secrets Manager | 28/10/2025   | 28/10/2025      | AWS Journey |
| 4   | - Ôn tập Thiết kế Kiến trúc Có khả năng Phục hồi <br>  Multi-AZ, Multi-Region, DR Strategies, Auto Scaling, Route 53, Load Balancing, Backup & Restore | 29/10/2025   | 29/10/2025      | AWS Journey |
| 5   | - Ôn tập Tối ưu Hiệu năng và Chi phí (Kiến trúc Hiệu năng Cao & Tối ưu Chi phí) <br>  EC2 Auto Scaling, Lambda, Fargate, CloudFront, Global Accelerator, Cost Explorer, Budgets, Savings Plans, Storage Tiering | 30/10/2025   | 30/10/2025      | AWS Journey |
| 6   | - Thực hành Tổng hợp: <br> + Xây dựng kiến trúc mẫu kết hợp EC2, S3, RDS, IAM, VPC, CloudFront, Lambda, CloudWatch <br> + Đánh giá theo 5 tiêu chí Well-Architected Framework <br> + Viết báo cáo tổng kết tuần | 31/10/2025   | 31/10/2025      | AWS Journey |


### Kết quả đạt được tuần 8:

* Đã tìm hiểu về containerization:
  * Hiểu được lợi ích của containers so với VMs
  * Học các khái niệm cơ bản và thuật ngữ của Docker
  * Nghiên cứu các use case của container

* Thực hành với Docker trên local:
  * Cài đặt Docker Desktop
  * Pull images từ Docker Hub (nginx, node, python)
  * Tạo Dockerfile đơn giản:
    ```dockerfile
    FROM node:14
    WORKDIR /app
    COPY package*.json ./
    RUN npm install
    COPY . .
    EXPOSE 3000
    CMD ["npm", "start"]
    ```
  * Build và chạy containers trên local

* Hiểu về Amazon ECS:
  * Học về kiến trúc và các thành phần của ECS
  * So sánh Fargate (serverless) vs EC2 launch types
  * Nghiên cứu task definitions và services

* Làm việc với ECR:
  * Tạo ECR repository
  * Tag và push Docker image lên ECR
  * Cấu hình authentication với ECR

* Triển khai trên ECS:
  * Tạo ECS cluster
  * Tạo task definition với container settings
  * Deploy service trên Fargate
  * Cấu hình networking và security groups
  * Truy cập ứng dụng qua public IP
  * Xem container logs trong CloudWatch