---
title: "Worklog Tuần 10"
date: 2026-06-21
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Mục tiêu tuần 10:

* Giám sát và tối ưu hóa hiệu năng của hệ thống Rookwork sau khi đã triển khai lên môi trường Production.
* Thiết lập hệ thống cảnh báo tự động bằng Amazon CloudWatch để phát hiện sớm các sự cố về tài nguyên và trạng thái dịch vụ.
* Rà soát và tăng cường bảo mật hạ tầng theo nguyên tắc Least Privilege và Defense-in-Depth.
* Bắt đầu viết tài liệu kỹ thuật và chuẩn bị nội dung cho báo cáo thực tập tốt nghiệp.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Thiết lập Amazon CloudWatch Dashboard để theo dõi tập trung các chỉ số hiệu năng (CPU, Memory, Network) của EC2 và RDS trong thời gian thực. | 22/06/2026 | 23/06/2026 | <https://000029.awsstudygroup.com/> |
| 3 | - Cấu hình CloudWatch Alarms và Amazon SNS để tự động gửi cảnh báo qua email khi CPU EC2 vượt ngưỡng 80% hoặc kết nối RDS bị gián đoạn. | 23/06/2026 | 24/06/2026 | <https://000036.awsstudygroup.com/> |
| 4 | - Rà soát toàn bộ IAM Policies và Security Groups của hệ thống, loại bỏ các quyền truy cập thừa theo nguyên tắc Least Privilege để giảm thiểu bề mặt tấn công. | 24/06/2026 | 25/06/2026 | <https://000030.awsstudygroup.com/> |
| 5 | - Tối ưu hóa chi phí vận hành hạ tầng thông qua AWS Cost Explorer và AWS Trusted Advisor, xác định các tài nguyên chưa sử dụng và đề xuất điều chỉnh. | 25/06/2026 | 25/06/2026 | <https://000034.awsstudygroup.com/> |
| 6 | - Viết tài liệu kỹ thuật mô tả kiến trúc hệ thống Rookwork, các quyết định thiết kế và hướng dẫn vận hành, bắt đầu soạn nội dung báo cáo thực tập tốt nghiệp. | 26/06/2026 | 26/06/2026 | N/A |

### Kết quả đạt được tuần 10:

* Thiết lập thành công CloudWatch Dashboard hiển thị tập trung và trực quan các chỉ số quan trọng của toàn bộ hạ tầng Rookwork: CPU Utilization, Network In/Out của EC2, số lượng kết nối Database (RDS Connections) và tỷ lệ Cache Hit của CloudFront.
* Cấu hình CloudWatch Alarms kết hợp Amazon SNS để tự động gửi thông báo email ngay khi phát hiện tình trạng bất thường về hiệu năng hoặc trạng thái dịch vụ.
* Hoàn thành rà soát và tinh chỉnh toàn bộ Security Groups theo hướng tối thiểu hóa quyền mở: xóa các inbound rule không cần thiết, đảm bảo RDS chỉ chấp nhận kết nối nội bộ từ EC2.
* Rà soát IAM Roles và IAM Policies sử dụng cho GitHub Actions, thu hẹp quyền hạn chỉ còn vừa đủ cho các tác vụ deploy (S3 sync, CloudFront Invalidation, không có quyền xóa Bucket).
* Phân tích báo cáo từ AWS Cost Explorer xác định cơ hội giảm chi phí và lập kế hoạch dọn dẹp tài nguyên thử nghiệm không còn sử dụng.
* Hoàn thành bản thảo tài liệu kỹ thuật mô tả kiến trúc hạ tầng, quy trình CI/CD, và hướng dẫn vận hành cơ bản cho hệ thống Rookwork; bắt đầu soạn nội dung Worklog và Bản đề xuất cho báo cáo thực tập tốt nghiệp.
