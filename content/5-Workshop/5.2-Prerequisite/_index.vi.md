---
title: "Điều kiện tiên quyết"
date: 2026-06-01
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

# Điều kiện tiên quyết

Trước khi bắt đầu bài thực hành, hãy đảm bảo bạn đã chuẩn bị đầy đủ các điều kiện sau để tránh gặp lỗi trong quá trình thực hiện.

---

## Yêu cầu cần thiết

### 1. Tài khoản AWS
- Có một **tài khoản AWS đang hoạt động** (Active).
- Nếu chưa có, hãy đăng ký tại [aws.amazon.com](https://aws.amazon.com/).

### 2. Quyền IAM
- Tài khoản IAM User cần có quyền **`AmazonVPCFullAccess`** hoặc **`AdministratorAccess`**.
- Không nên sử dụng tài khoản **root** cho các tác vụ hàng ngày.

> [!WARNING]
> Đảm bảo tài khoản IAM của bạn **không bị giới hạn** quyền tạo VPC, Subnet, NAT Gateway, Security Group và VPC Endpoint. Nếu không đủ quyền, bạn sẽ gặp lỗi `UnauthorizedOperation` trong các bước tiếp theo.

### 3. Chọn đúng Region
- Lựa chọn đúng **Region làm việc: Asia Pacific (Singapore) (`ap-southeast-1`)** trước khi bắt đầu.
- Kiểm tra bằng cách nhìn vào góc trên bên phải của AWS Management Console.

---

## Lưu ý về chi phí

> [!NOTE]
> Một số tài nguyên trong bài lab này có thể **phát sinh chi phí** nếu chạy lâu dài, đặc biệt là **NAT Gateway** và **Elastic IP**. Hãy thực hiện đầy đủ bước [Dọn dẹp tài nguyên (5.6)](../5.6-Cleanup/) sau khi hoàn thành bài lab.
