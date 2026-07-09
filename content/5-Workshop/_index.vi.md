---
title: "Workshop"
date: 2026-04-17
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Workshop Thực Hành

Phần này ghi lại bài thực hành chuyên sâu được thực hiện trong kỳ thực tập, tập trung vào việc triển khai hạ tầng mạng AWS phục vụ dự án nhóm **Rookwork**.

Bài workshop được tổ chức theo cấu trúc chuẩn gồm: **Mục tiêu → Kiến trúc → Các bước thực hiện → Kết quả và bài học kinh nghiệm**.

---

### Workshop – Triển khai hạ tầng mạng VPC cho Rookwork

Hướng dẫn từng bước xây dựng hệ thống mạng AWS VPC có tính sẵn sàng cao, bao gồm thiết lập Subnets, NAT Gateway, Internet Gateway, VPC Endpoint và Security Groups phân tầng.

1. **[5.1 – Tổng quan Workshop](5.1-Workshop-overview/)** – Kiến trúc hệ thống và các thành phần sẽ được triển khai
2. **[5.2 – Điều kiện tiên quyết](5.2-Prerequisite/)** – Kiểm tra tài khoản IAM và Region làm việc
3. **[5.3 – Khởi tạo VPC và Subnets](5.3-S3-vpc/)** – Tạo toàn bộ hạ tầng mạng bằng "VPC and more"
4. **[5.4 – Cấu hình NAT Gateway](5.4-S3-onprem/)** – Cho phép Private Subnet kết nối internet an toàn
5. **[5.5 – Thiết lập Security Groups](5.5-Policy/)** – Tường lửa ảo phân tầng cho ALB và Backend
6. **[5.6 – Dọn dẹp tài nguyên](5.6-Cleanup/)** – Xóa tài nguyên sau khi hoàn thành lab
