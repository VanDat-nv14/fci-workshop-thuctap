---
title: "Worklog Tuần 11"
date: 2026-06-28
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Mục tiêu tuần 11:

* Nghiên cứu công cụ và lập kịch bản kiểm thử tải (Load Testing) cho hệ thống.
* Tiến hành kiểm thử hiệu năng để đánh giá khả năng co giãn thực tế của Auto Scaling Group (ASG) dưới tải cao.
* Theo dõi các hoạt động co giãn (Scaling Activities) và tối ưu hóa thời gian chờ (Cooldown Periods).
* Tổng hợp tài liệu hướng dẫn cấu hình Auto Scaling và báo cáo kết quả kiểm thử tải.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Nghiên cứu các công cụ kiểm thử hiệu năng hệ thống (như Apache JMeter, Locust, Artillery) để giả lập lượng truy cập lớn đồng thời. | 29/06/2026 | 30/06/2026 | <https://000006.awsstudygroup.com/> |
| 3 | - Thiết lập kịch bản kiểm thử tải chi tiết, gửi lượng lớn HTTP requests đồng thời qua Application Load Balancer để đẩy CPU của máy chủ lên cao. | 30/06/2026 | 01/07/2026 | N/A |
| 4 | - Thực thi kịch bản kiểm thử, theo dõi giám sát chỉ số trên CloudWatch để kiểm tra quá trình ASG tự động tạo thêm máy chủ (Scale-out). | 01/07/2026 | 02/07/2026 | N/A |
| 5 | - Theo dõi tiến trình tự động giảm số lượng máy chủ (Scale-in) khi tải giảm, thực hiện điều chỉnh cấu hình Cooldown Period tối ưu nhất. | 02/07/2026 | 03/07/2026 | N/A |
| 6 | - Soạn thảo hướng dẫn kỹ thuật chi tiết về thiết lập Auto Scaling và báo cáo phân tích kết quả chịu tải thực tế. | 03/07/2026 | 03/07/2026 | <https://000002.awsstudygroup.com/> |

### Kết quả đạt được tuần 11:

* Chọn lựa thành thạo công cụ kiểm thử tải phù hợp để giả lập hành vi người dùng truy cập ứng dụng.
* Thiết lập thành công kịch bản kiểm thử tải an toàn và hiệu quả hướng đến cổng API của ứng dụng.
* Kiểm thử tải thực tế thành công: Xác nhận Auto Scaling Group tự động tạo thêm EC2 instances mới khi chỉ số CPU trung bình vượt ngưỡng cấu hình.
* Tối ưu hóa thành công thời gian cooldown và cơ chế scale-in để tránh lãng phí tài nguyên và chi phí khi lưu lượng truy cập giảm xuống.
* Hoàn thành bộ tài liệu kỹ thuật chi tiết hướng dẫn thiết lập và kết quả kiểm thử khả năng co giãn chịu tải của hệ thống.
