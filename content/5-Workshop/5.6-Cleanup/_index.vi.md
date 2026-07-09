---
title: "Dọn dẹp tài nguyên"
date: 2026-06-01
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

# Dọn dẹp tài nguyên

> [!CAUTION]
> **Quan trọng:** Các tài nguyên như **NAT Gateway** và **Elastic IP** sẽ **liên tục tính phí theo giờ** ngay cả khi không có traffic. Hãy thực hiện đầy đủ các bước dọn dẹp sau khi hoàn thành bài lab để tránh phát sinh chi phí không mong muốn.

---

## Thứ tự dọn dẹp

Hãy thực hiện theo đúng thứ tự dưới đây vì một số tài nguyên phụ thuộc vào tài nguyên khác:

### Bước 1: Xóa NAT Gateway

1. Điều hướng đến **VPC → NAT Gateways**.
2. Chọn NAT Gateway **`rookwork-nat`**.
3. Nhấn **Actions → Delete NAT gateway**.
4. Xác nhận và đợi trạng thái chuyển sang `Deleted` (quá trình này mất khoảng **2–3 phút**).

> [!WARNING]
> Phải xóa NAT Gateway **trước** khi giải phóng Elastic IP. Nếu làm ngược lại, AWS sẽ báo lỗi tài nguyên đang được sử dụng.

---

### Bước 2: Giải phóng Elastic IPs

1. Điều hướng đến **VPC → Elastic IPs**.
2. Chọn các địa chỉ IP đã được cấp phát (ví dụ: `54.254.36.209`, `47.131.240.109`).
3. Nhấn **Actions → Release Elastic IP addresses**.
4. Xác nhận để giải phóng hoàn toàn.

---

### Bước 3: Xóa VPC

1. Điều hướng đến **VPC → Your VPCs**.
2. Chọn VPC **`vpc-027222b4e12adb751`** (VPC rookwork).
3. Nhấn **Actions → Delete VPC**.
4. Xác nhận bằng cách nhập `delete`.

> [!NOTE]
> Khi xóa VPC, AWS sẽ **tự động xóa** toàn bộ các tài nguyên phụ thuộc bao gồm:
> - ✅ 4 Subnets (`rookwork-subnet-public1/2-...`, `rookwork-subnet-private1/2-...`)
> - ✅ 3 Route Tables (`rookwork-rtb-public`, `rookwork-rtb-private1/2-...`)
> - ✅ Internet Gateway (`rookwork-igw`)
> - ✅ 2 Security Groups (`rookwork-alb-sg`, `backend-sg`)
> - ✅ VPC Endpoint (`rookwork-vpce-s3`)

---

## Kiểm tra sau khi dọn dẹp

Sau khi hoàn thành tất cả các bước, hãy kiểm tra lại:

- [ ] Danh sách **NAT Gateways** không còn `rookwork-nat`.
- [ ] Danh sách **Elastic IPs** trống hoặc không còn các IP đã dùng.
- [ ] Danh sách **Your VPCs** không còn VPC rookwork.
- [ ] Danh sách **Security Groups** không còn `rookwork-alb-sg` và `backend-sg`.

---

## Tổng kết Workshop

Chúc mừng! Bạn đã hoàn thành toàn bộ bài Workshop **Triển khai hạ tầng mạng VPC cho Rookwork**. Qua bài thực hành này, bạn đã học được:

- 🌐 Cách thiết kế và triển khai kiến trúc mạng AWS VPC có tính sẵn sàng cao với 2 AZs.
- 🔒 Cách áp dụng mô hình bảo mật phân tầng (Layered Security) bằng Security Groups.
- 🔀 Cách NAT Gateway cho phép Private Subnet ra internet an toàn.
- 💰 Tầm quan trọng của việc dọn dẹp tài nguyên để kiểm soát chi phí AWS.
