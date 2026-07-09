---
title: "Cấu hình NAT Gateway"
date: 2026-06-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

# Cấu hình NAT Gateway

## Mục đích

Để các tài nguyên trong **Private Subnet** (ví dụ: Backend EC2 servers) có thể tải các bản cập nhật, thư viện từ Internet mà **không bị lộ địa chỉ IP** ra ngoài, chúng ta sử dụng **NAT Gateway**.

NAT Gateway hoạt động như một cổng trung gian: nhận request ra internet từ các máy chủ nội bộ, thực hiện yêu cầu thay mặt cho chúng, sau đó trả kết quả về — giúp các server backend hoàn toàn không tiếp xúc trực tiếp với internet.

---

## Các bước thực hiện

### Bước 1: Điều hướng đến NAT Gateways

- Trên menu bên trái trong trang VPC, chọn **NAT Gateways**.

---

### Bước 2: Kiểm tra NAT Gateway đã được tạo tự động

Khi tạo VPC bằng chế độ **"VPC and more"**, NAT Gateway tên **`rookwork-nat`** đã được **tự động tạo** với:
- **Connectivity type**: `Public`
- **Subnet**: Gắn vào một trong các Public Subnets (`rookwork-subnet-public1-...`)
- **Elastic IP**: Được tự động cấp phát

---

### Bước 3: Kiểm tra trạng thái

- Đảm bảo cột **State** của `rookwork-nat` hiển thị màu **xanh lá cây (✅ Available)**.
- Nếu trạng thái còn là `Pending`, hãy chờ thêm 1–2 phút và làm mới trang.

---

### Bước 4: Xác nhận Elastic IP đã được gán

Đảm bảo NAT Gateway đã được cấp phát tự động địa chỉ **Elastic IP (EIP)** — đây là địa chỉ IP công khai cố định đại diện cho toàn bộ Private Subnet khi ra internet.

Trong ảnh dưới, chúng ta thấy NAT Gateway đã liên kết thành công (**Status: Succeeded**) với các IP như `54.254.36.209` và `47.131.240.109`.

![NAT Gateway Rookwork đã được tạo thành công với Elastic IP](/images/5-Workshop/5.4-S3-onprem/nat-gateway.png)

---

## Kết quả đạt được

- ✅ NAT Gateway `rookwork-nat` ở trạng thái **Available** (xanh lá).
- ✅ Đã được gán **Elastic IP** cố định.
- ✅ Các Private Subnets đã được định tuyến qua NAT Gateway trong Route Table tương ứng (`rookwork-rtb-private1-...`).

> [!NOTE]
> NAT Gateway tính phí theo **giờ** và theo **lượng dữ liệu truyền**. Nhớ xóa tài nguyên này sau khi hoàn thành lab tại bước [5.6 – Dọn dẹp tài nguyên](../5.6-Cleanup/).
