---
title: "Worklog Tuần 8"
date: 2026-06-07
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---


### Mục tiêu tuần 8:

* Cấu hình Application Load Balancer và Auto Scaling Group để đảm bảo hệ thống có khả năng mở rộng tự động.
* Thiết lập cơ chế sao lưu tự động cho RDS và EBS.
* Tìm hiểu và thực hành quy trình CI/CD trên AWS.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tìm hiểu và cấu hình Application Load Balancer (ALB) để điều phối lưu lượng truy cập. | 08/06/2026 | 09/06/2026 | <https://000006.awsstudygroup.com/> |
| 3 | - Cấu hình Auto Scaling Group (ASG) cho EC2 giúp tự động tăng/giảm số lượng máy chủ để tối ưu chi phí. | 09/06/2026 | 10/06/2026 | <https://000006.awsstudygroup.com/> |
| 4 | - Thiết lập chế độ tự động sao lưu dữ liệu (Snapshot/Backup) cho RDS và EBS, kiểm tra khả năng chịu lỗi (Fault Tolerance). | 10/06/2026 | 11/06/2026 | <https://000013.awsstudygroup.com/> |
| 5 | - Tìm hiểu tổng quan về quy trình CI/CD trên nền tảng đám mây AWS. | 11/06/2026 | 11/06/2026 | <https://000152.awsstudygroup.com/> |
| 6 | - Cấu hình AWS CodePipeline và AWS CodeDeploy (hoặc GitHub Actions) để tự động cập nhật code lên EC2. | 12/06/2026 | 12/06/2026 | <https://000017.awsstudygroup.com/> |

### Kết quả đạt được tuần 8:

* Hiểu được vai trò của Application Load Balancer (ALB) trong việc phân phối lưu lượng đến nhiều EC2 instance.
* Cấu hình ALB với Target Group và Health Check để đảm bảo chỉ route traffic đến instance đang hoạt động.
* Thiết lập Auto Scaling Group với Launch Template, cấu hình min/max/desired capacity phù hợp.
* Cấu hình scaling policy dựa trên chỉ số CPU để hệ thống tự động scale-out/scale-in theo tải thực tế.
* Bật tính năng tự động backup (Automated Backup) và tạo Snapshot thủ công cho Amazon RDS.
* Tạo Snapshot cho EBS volume để có thể phục hồi dữ liệu khi cần thiết.
* Kiểm tra Fault Tolerance bằng cách dừng một EC2 instance và quan sát ALB tự động điều hướng traffic sang instance còn lại.
* Nắm được tổng quan quy trình CI/CD và lợi ích của việc tự động hoá deploy lên AWS.
* Thực hành cấu hình AWS CodePipeline hoặc GitHub Actions để tự động deploy code mới lên EC2 khi có commit lên GitHub.
