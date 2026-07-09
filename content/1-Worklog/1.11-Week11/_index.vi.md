---
title: "Worklog Tuần 11"
date: 2026-06-28
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Mục tiêu tuần 11:

* Kiểm thử hiệu năng hệ thống Rookwork trên môi trường Production để đánh giá khả năng chịu tải thực tế của hạ tầng AWS.
* Tổ chức buổi Demo nội bộ dự án nhóm, tiếp nhận góp ý và thực hiện điều chỉnh trước khi bàn giao.
* Dọn dẹp các tài nguyên AWS thử nghiệm để kiểm soát chi phí vận hành.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tiến hành kiểm thử hiệu năng hệ thống Rookwork trên môi trường Production: đánh giá khả năng chịu tải thực tế của Application Load Balancer và cơ chế tự động mở rộng (Auto Scaling Group) dưới nhiều ngưỡng người dùng đồng thời. | 29/06/2026 | 30/06/2026 | <https://000006.awsstudygroup.com/> |
| 3 | - Phối hợp cùng nhóm tổ chức buổi Demo nội bộ dự án Rookwork, trình bày kiến trúc hạ tầng AWS, quy trình CI/CD và các tính năng hoàn chỉnh của ứng dụng. | 30/06/2026 | 01/07/2026 | N/A |
| 4 | - Tiếp nhận góp ý từ buổi Demo, thực hiện các điều chỉnh và vá lỗi phát sinh (Bug Fix) trên cả Frontend và Backend trước khi bàn giao chính thức. | 01/07/2026 | 02/07/2026 | N/A |
| 5 | - Tiếp tục kiểm tra toàn diện hệ thống sau khi vá lỗi, đảm bảo các tính năng hoạt động ổn định và đúng yêu cầu trên môi trường Production. | 02/07/2026 | 03/07/2026 | N/A |
| 6 | - Dọn dẹp và hủy bỏ các tài nguyên AWS thử nghiệm không còn sử dụng (EC2 test instances, Snapshots cũ) để tránh phát sinh chi phí không cần thiết. | 03/07/2026 | 03/07/2026 | <https://000002.awsstudygroup.com/> |

### Kết quả đạt được tuần 11:

* Thực hiện kiểm thử tải (Load Testing) thành công trên hệ thống Rookwork Production, xác minh Application Load Balancer phân phối đều lưu lượng đến các EC2 instance và Auto Scaling Group tự động khởi tạo instance mới khi CPU vượt ngưỡng cấu hình.
* Tổ chức thành công buổi Demo nội bộ dự án Rookwork, trình bày rõ ràng kiến trúc hạ tầng AWS 3-tier, quy trình CI/CD tự động và toàn bộ tính năng của ứng dụng trước nhóm và cán bộ hướng dẫn.
* Tổng hợp các góp ý từ buổi Demo và thực hiện Bug Fix kịp thời: điều chỉnh giao diện Frontend, xử lý các edge case trên Backend và kiểm tra lại luồng xác thực (Authentication flow).
* Hoàn tất kiểm tra toàn diện hệ thống sau khi vá lỗi, xác nhận tất cả tính năng hoạt động ổn định, đúng yêu cầu và sẵn sàng cho bàn giao chính thức.
* Dọn dẹp và xóa thành công các tài nguyên AWS thử nghiệm không còn cần thiết, giảm thiểu chi phí vận hành trong giai đoạn cuối kỳ thực tập.
