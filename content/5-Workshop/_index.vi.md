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

### Workshop – Triển khai hạ tầng mạng VPC và CI/CD cho Rookwork

Hướng dẫn từng bước xây dựng hệ thống mạng AWS VPC có tính sẵn sàng cao và thiết lập quy trình tự động hóa CI/CD triển khai ứng dụng Spring Boot.

1. **[5.1 – Tổng quan Workshop](5.1-Workshop-overview/)** – Kiến trúc hệ thống và các thành phần sẽ được triển khai
2. **[5.2 – Điều kiện tiên quyết](5.2-Prerequisite/)** – Kiểm tra tài khoản IAM và Region làm việc
3. **[5.3 – Thiết lập hạ tầng mạng VPC](5.3-S3-vpc/)** – Khởi tạo VPC, NAT Gateway và Security Groups phân tầng
4. **[5.4 – Triển khai CI/CD Backend với GitHub Actions](5.4-Backend-CICD/)** – Cấu hình GitHub Secrets và luồng tự động hóa
5. **[5.5 – Dọn dẹp tài nguyên](5.5-Cleanup/)** – Xóa tài nguyên sau khi hoàn thành lab
