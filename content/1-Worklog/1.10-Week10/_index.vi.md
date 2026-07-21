---
title: "Worklog Tuần 10"
date: 2026-06-21
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Mục tiêu tuần 10:

* Tìm hiểu lý thuyết và cơ chế hoạt động của Auto Scaling trên nền tảng AWS Cloud.
* Thiết lập cấu hình Launch Template cho máy chủ Backend EC2.
* Khởi tạo Auto Scaling Group (ASG) để tự động quản lý số lượng EC2 instances hoạt động.
* Thiết lập các chính sách tự động mở rộng (Scaling Policies) dựa trên hiệu năng sử dụng CPU.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Nghiên cứu lý thuyết về Auto Scaling (Horizontal Scaling vs Vertical Scaling) và các thành phần cốt lõi (Launch Templates, Target Groups). | 22/06/2026 | 23/06/2026 | <https://000006.awsstudygroup.com/> |
| 3 | - Thiết lập Launch Template chi tiết cho máy chủ EC2 Backend, bao gồm lựa chọn AMI, Instance Type, Key Pair và Security Group tương ứng. | 23/06/2026 | 24/06/2026 | <https://000029.awsstudygroup.com/> |
| 4 | - Khởi tạo Auto Scaling Group (ASG) tích hợp trong VPC, cấu hình phạm vi triển khai trên 2 Availability Zones thuộc Private Subnets. | 24/06/2026 | 25/06/2026 | <https://000030.awsstudygroup.com/> |
| 5 | - Cấu hình Target Tracking Scaling Policy với chỉ số giám sát trung bình CPU vượt quá 70% để kích hoạt quá trình tự động mở rộng. | 25/06/2026 | 25/06/2026 | <https://000036.awsstudygroup.com/> |
| 6 | - Kiểm tra sơ bộ hoạt động khởi tạo máy chủ tự động của ASG và liên kết với Application Load Balancer (ALB). | 26/06/2026 | 26/06/2026 |  |

### Kết quả đạt được tuần 10:

* Nắm vững nguyên lý hoạt động của Auto Scaling và các cơ chế co giãn tài nguyên trên AWS Cloud.
* Tạo thành công Launch Template tiêu chuẩn chứa đầy đủ các thông số cấu hình và Docker setup cho Backend.
* Khởi tạo thành công Auto Scaling Group (ASG) hoạt động đa vùng sẵn sàng (Multi-AZ) trong phân vùng mạng nội bộ (Private Subnet).
* Thiết lập chính sách giám sát hiệu năng CPU thành công, giúp hệ thống sẵn sàng kích hoạt thêm máy chủ khi lưu lượng tăng cao.
* Xác minh tính năng tự động liên kết các EC2 instance mới được tạo vào Target Group của Application Load Balancer hoạt động ổn định.
