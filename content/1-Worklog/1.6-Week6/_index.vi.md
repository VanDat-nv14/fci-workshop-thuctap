---
title: "Worklog Tuần 6"
date: 2026-05-24
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

* Cấu hình kết nối ứng dụng với cơ sở dữ liệu Amazon RDS.
* Triển khai frontend React lên S3 kết hợp CloudFront.
* Theo dõi log ứng dụng và xử lý các lỗi phổ biến khi deploy hệ thống lên AWS.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Cấu hình Security Group cho RDS và thiết lập quy tắc kết nối an toàn từ máy chủ EC2. | 25/05/2026 | 26/05/2026 | <https://000005.awsstudygroup.com/> |
| 3 | - Cấu hình và kết nối ứng dụng backend với database RDS. | 26/05/2026 | 27/05/2026 | <https://000005.awsstudygroup.com/> |
| 4 | - Tìm hiểu cách deploy frontend React lên S3 + CloudFront. | 27/05/2026 | 28/05/2026 | <https://000094.awsstudygroup.com/> |
| 5 | - Cấu hình domain hoặc endpoint để người dùng truy cập hệ thống. | 28/05/2026 | 28/05/2026 | <https://000010.awsstudygroup.com/> |
| 6 | - Theo dõi log ứng dụng (terminal/CloudWatch) và kiểm tra/xử lý các lỗi phổ biến khi deploy. | 29/05/2026 | 29/05/2026 | <https://000008.awsstudygroup.com/> |

### Kết quả đạt được tuần 6:

* Cấu hình Security Group cho RDS để giới hạn quyền truy cập cơ sở dữ liệu, chỉ cho phép kết nối nội bộ từ EC2.
* Kết nối thành công backend với database Amazon RDS, xử lý được lỗi security group và endpoint connection string.
* Triển khai được frontend React lên S3 Static Website Hosting kết hợp CloudFront để phân phối nội dung toàn cầu.
* Cấu hình endpoint để người dùng có thể truy cập hệ thống qua CloudFront URL hoặc domain tùy chỉnh.
* Biết cách xem log ứng dụng qua terminal, systemd service logs và CloudWatch Logs cơ bản.
* Tích lũy kinh nghiệm xử lý các lỗi thường gặp khi deploy: sai port, lỗi environment variable, lỗi kết nối database, thiếu quyền IAM/S3.
* Hoàn thiện kỹ năng kết hợp nhiều dịch vụ AWS để xây dựng một hệ thống hoàn chỉnh hơn.
