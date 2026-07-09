---
title: "Worklog Tuần 7"
date: 2026-05-31
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:

* Phân tích yêu cầu hạ tầng và thiết kế kiến trúc mạng VPC thực tế phục vụ riêng cho dự án nhóm "rookwork".
* Xây dựng phân vùng mạng an toàn (Public/Private Subnets), triển khai NAT Gateway và cấu hình Bastion Host cho dự án.
* Thiết lập môi trường máy chủ EC2 cho ứng dụng backend nằm trong mạng nội bộ và cấu hình kết nối bảo mật đến database RDS.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Phân tích yêu cầu hạ tầng và thiết kế kiến trúc hệ thống VPC chi tiết cho dự án nhóm "rookwork". | 01/06/2026 | 02/06/2026 | <https://000003.awsstudygroup.com/> |
| 3 | - Thiết lập các subnet mạng (Public/Private) và NAT Gateway để đảm bảo kết nối internet an toàn cho các máy chủ nội bộ. | 02/06/2026 | 03/06/2026 | <https://000003.awsstudygroup.com/> |
| 4 | - Cài đặt và cấu hình Bastion Host tại Public Subnet làm cổng quản trị duy nhất cho các máy chủ EC2 nội bộ của nhóm. | 03/06/2026 | 04/06/2026 | <https://000092.awsstudygroup.com/> |
| 5 | - Thiết kế và phân quyền mạng chi tiết (Security Groups) liên tầng giữa Bastion, Backend EC2 và RDS Database của dự án. | 04/06/2026 | 05/06/2026 | <https://000003.awsstudygroup.com/> |
| 6 | - Cấu hình môi trường chạy ứng dụng backend trên EC2 trong Private Subnet và thiết lập kết nối bảo mật đến RDS. | 05/06/2026 | 05/06/2026 | <https://000005.awsstudygroup.com/> |

### Kết quả đạt được tuần 7:

* Thiết kế và triển khai thành công kiến trúc VPC chuyên biệt, tối ưu cho sơ đồ vận hành của dự án nhóm "rookwork".
* Phân bổ hợp lý các khối IP cho Public Subnet và Private Subnet, cấu hình Route Table và NAT Gateway giúp các máy chủ ứng dụng kết nối internet an toàn.
* Triển khai thành công Bastion Host đặt tại Public Subnet làm điểm nhảy (jump server) để toàn bộ nhóm quản trị EC2 nội bộ qua SSH một cách an toàn.
* Hoàn thiện thiết lập Security Groups liên tầng chặt chẽ cho dự án nhóm (Bastion -> Backend EC2 -> RDS Database), giảm thiểu tối đa bề mặt tấn công.
* Giải quyết triệt để sự cố mất kết nối mạng (SSH timeout) thông qua việc rà soát và cấu hình lại Security Groups, Route Table nội bộ.
* Cài đặt thành công môi trường runtime cho ứng dụng backend trên máy chủ EC2 nằm hoàn toàn trong phân vùng Private Subnet của dự án.
* Thiết lập kết nối bảo mật từ Backend EC2 tới Amazon RDS thông qua mạng nội bộ VPC, đảm bảo dữ liệu của cơ sở dữ liệu không bị lộ ra ngoài internet.
