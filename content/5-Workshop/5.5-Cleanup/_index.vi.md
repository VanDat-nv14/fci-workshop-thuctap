---
title: "Dọn dẹp tài nguyên"
date: 2026-04-17
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

# Dọn dẹp tài nguyên

Sau khi hoàn tất bài workshop thực hành, để tránh phát sinh chi phí không mong muốn trên tài khoản AWS của bạn (đặc biệt là đối với các tài nguyên tính phí theo giờ như NAT Gateway hay Elastic IP), hãy thực hiện dọn dẹp theo các bước sau.

---

## Quy trình dọn dẹp tuần tự

### Bước 1: Xóa NAT Gateways

1. Điều hướng đến **VPC Dashboard → NAT Gateways**.
2. Chọn NAT Gateway **`rookwork-nat`** đã tạo ở bước 5.3.
3. Nhấn **Actions → Delete NAT gateway**.
4. Nhập `delete` để xác nhận và nhấn **Delete**.
5. Chờ trạng thái của NAT Gateway chuyển sang **Deleted** hoàn toàn.

---

### Bước 2: Giải phóng Elastic IPs

1. Điều hướng đến **VPC Dashboard → Elastic IPs**.
2. Chọn các địa chỉ Elastic IP đã liên kết với NAT Gateway vừa xóa.
3. Nhấn **Actions → Release Elastic IP addresses**.
4. Xác nhận để giải phóng IP khỏi tài khoản AWS của bạn (nếu Elastic IP không gắn vào tài nguyên hoạt động nào thì sẽ bị tính phí nếu không giải phóng).

---

### Bước 3: Xóa VPC và các tài nguyên phụ thuộc

1. Điều hướng đến **VPC Dashboard → Your VPCs**.
2. Chọn VPC **`rookwork-vpc`**.
3. Nhấn **Actions → Delete VPC**.
4. Xác nhận bằng cách nhập `delete` và nhấn **Delete**.

> [!NOTE]
> Khi bạn xóa VPC, AWS sẽ tự động xóa sạch tất cả các tài nguyên mạng phụ thuộc gắn liền với nó, bao gồm:
> - 4 Subnets (`rookwork-subnet-public1/2-...`, `rookwork-subnet-private1/2-...`)
> - 3 Route Tables (`rookwork-rtb-public`, `rookwork-rtb-private1/2-...`)
> - Internet Gateway (`rookwork-igw`)
> - VPC Endpoint (`rookwork-vpce-s3`)

---

### Bước 4: Xóa các Security Groups thủ công

Nếu các Security Groups của bạn (`rookwork-alb-sg`, `backend-sg`, `rds-ec2-1`) không tự động xóa cùng VPC do còn liên kết phụ thuộc chéo:
1. Điều hướng đến **VPC Dashboard → Security Groups**.
2. Chọn các Security Groups trên.
3. Nhấn **Actions → Delete security groups** và xác nhận xóa.

---

## Kiểm tra sau khi dọn dẹp

Hãy đảm bảo các dịch vụ sau đã trống:
- NAT Gateways: Không còn cổng nào hoạt động.
- Elastic IPs: Không còn địa chỉ nào hiển thị trong danh sách.
- VPCs: Không còn `rookwork-vpc` trong danh sách Your VPCs.
- Security Groups: Đã xóa toàn bộ các nhóm bảo mật của Rookwork.
