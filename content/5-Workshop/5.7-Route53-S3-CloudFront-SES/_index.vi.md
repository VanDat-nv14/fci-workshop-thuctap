---
title: "Tích hợp dịch vụ AWS"
menuTitle: "Tích hợp dịch vụ AWS"
date: 2026-05-01
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

Trong chương này, chúng ta tiến hành cấu hình chi tiết các dịch vụ hạ tầng AWS cốt lõi phục vụ vận hành dự án, bao gồm phân giải tên miền (Route 53), lưu trữ và phân phối frontend tĩnh (S3 + CloudFront), lưu trữ tệp tin đính kèm riêng tư (S3 Storage) và dịch vụ email (SES).

### Nội dung chính

* **[5.7.1. Cấu hình Tên miền & AWS Route 53](5.7.1-Route53/)** - Hướng dẫn chuyển quyền phân giải tên miền từ Tenten.vn sang AWS Route 53.
* **[5.7.2. Lưu trữ Web tĩnh với Amazon S3](5.7.2-S3-Static-Hosting/)** - Hướng dẫn tạo S3 Bucket và upload mã nguồn Frontend React App.
* **[5.7.3. Phân phối bảo mật qua Amazon CloudFront & ACM](5.7.3-CloudFront-ACM/)** - Hướng dẫn cấu hình SSL qua ACM, phân phối qua CDN CloudFront và trỏ tên miền Route 53 về CDN.
* **[5.7.4. Lưu trữ tệp tin riêng tư với Amazon S3](5.7.4-S3-Storage/)** - Hướng dẫn cấu hình Private S3 Bucket, Spring Boot Backend và S3 Service trong Java để lưu tài liệu dự án an toàn.
* **[5.7.5. Tích hợp dịch vụ gửi Email với Amazon SES](5.7.5-SES/)** - Hướng dẫn cấu hình AWS SES xác thực tên miền, tạo SMTP credentials và tích hợp gửi mail thông báo.
