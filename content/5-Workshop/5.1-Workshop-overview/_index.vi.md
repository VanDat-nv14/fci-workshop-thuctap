---
title: "Tổng quan"
date: 2026-06-01
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

# Tổng quan Workshop – Triển khai hạ tầng mạng VPC cho Rookwork

## Mục tiêu

Trong bài thực hành này, chúng ta sẽ triển khai một hệ thống mạng AWS VPC cơ bản có **tính sẵn sàng cao (High Availability)**. Đây là nền tảng hạ tầng mạng đã được áp dụng thực tế cho dự án nhóm **Rookwork** trong kỳ thực tập.

---

## Kiến trúc hệ thống

Kiến trúc hệ thống được đặt tại Region **Asia Pacific (Singapore) `ap-southeast-1`** và bao gồm các thành phần chính sau:

| Thành phần | Chi tiết |
| :--- | :--- |
| **VPC** | Khởi tạo một Virtual Private Cloud để cô lập tài nguyên mạng |
| **Availability Zones (AZs)** | Phân bổ tài nguyên trải dài trên 2 AZs (`ap-southeast-1a` và `ap-southeast-1b`) để đảm bảo High Availability |
| **Subnets** | 4 subnets: **2 Public Subnets** (chứa Load Balancer, NAT Gateway) và **2 Private Subnets** (dành cho backend an toàn) |
| **Internet Gateway** | `rookwork-igw` – Kết nối mạng public ra internet |
| **NAT Gateway** | `rookwork-nat` – Cho phép mạng private ra internet một chiều |
| **VPC Endpoint** | `rookwork-vpce-s3` – Kết nối nội bộ an toàn đến S3 (không qua internet) |
| **Security Groups** | `rookwork-alb-sg` (cho ALB) và `backend-sg` (cho Backend EC2) |

---

## Sơ đồ kiến trúc

![Sơ đồ kiến trúc VPC Rookwork](/images/5-Workshop/5.1-Workshop-overview/vpc-architecture.png)

---

## Các bước thực hành trong Workshop này

1. **[5.2 – Điều kiện tiên quyết](../5.2-Prerequisite/):** Kiểm tra tài khoản và region làm việc.
2. **[5.3 – Khởi tạo VPC và Subnets](../5.3-S3-vpc/):** Tạo toàn bộ hạ tầng mạng bằng chế độ "VPC and more".
3. **[5.4 – Cấu hình NAT Gateway](../5.4-S3-onprem/):** Kiểm tra và xác nhận NAT Gateway hoạt động đúng.
4. **[5.5 – Thiết lập Security Groups](../5.5-Policy/):** Tạo tường lửa ảo phân tầng cho ALB và Backend.
5. **[5.6 – Dọn dẹp tài nguyên](../5.6-Cleanup/):** Xóa tài nguyên để tránh phát sinh chi phí.
