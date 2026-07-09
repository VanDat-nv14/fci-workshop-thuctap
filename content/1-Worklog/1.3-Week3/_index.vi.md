---
title: "Worklog Tuần 3"
date: 2026-05-03
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3:

* Thiết kế và cấu hình VPC theo mô hình Public Subnet và Private Subnet.
* Thực hành truy cập an toàn vào EC2 nội bộ thông qua Bastion Host.
* Tìm hiểu cách troubleshooting, giám sát network và kiểm soát chi phí trên AWS.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Thiết lập VPC với mô hình Public/Private Subnet, cấu hình NAT Gateway và Internet Gateway. | 04/05/2026 | 06/05/2026 | <https://000003.awsstudygroup.com/> |
| 3 | - Triển khai Bastion Host để quản trị các instance nội bộ, cấu hình Security Groups và SSH Key Pairs. | 05/05/2026 | 06/05/2026 | <https://000092.awsstudygroup.com/> |
| 4 | - Khởi tạo và cấu hình Amazon EC2 (Linux), sử dụng VS Code Remote - SSH để truy cập server. | 06/05/2026 | 07/05/2026 | <https://000087.awsstudygroup.com/> |
| 5 | - Kích hoạt VPC Flow Logs để phân tích lưu lượng mạng và xử lý lỗi kết nối (SSH troubleshooting). | 07/05/2026 | 08/05/2026 | <https://000074.awsstudygroup.com/> |
| 6 | - Kiểm soát chi phí qua AWS Billing/Cost Explorer, thực hành phân quyền và bảo mật tài nguyên. | 08/05/2026 | 08/05/2026 | <https://000064.awsstudygroup.com/> |

### Kết quả đạt được tuần 3:

* Xây dựng được kiến trúc VPC cơ bản gồm Public Subnet và Private Subnet.
* Hiểu sự khác nhau giữa tài nguyên đặt trong public network và private network trên AWS.
* Cấu hình route table và đường truy cập internet phù hợp cho từng loại subnet.
* Sử dụng NAT Gateway để instance trong Private Subnet có thể truy cập internet mà không bị expose trực tiếp.
* Triển khai Bastion Host làm điểm truy cập trung gian để quản trị EC2 nội bộ.
* Thực hành quản lý inbound và outbound traffic bằng Security Groups.
* Kết nối vào EC2 Linux bằng SSH Key Pairs.
* Sử dụng VS Code Remote - SSH để chỉnh sửa file và thao tác trực tiếp trên server.
* Kích hoạt VPC Flow Logs để quan sát traffic và hỗ trợ troubleshooting.
* Biết cách kiểm tra AWS Billing và Cost Explorer để theo dõi chi phí.
* Cải thiện kỹ năng xử lý lỗi kết nối SSH qua nhiều tầng network.
