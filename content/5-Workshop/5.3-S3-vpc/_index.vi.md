---
title: "Khởi tạo VPC và Subnets"
date: 2026-06-01
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

# Khởi tạo VPC và Subnets

Trong bước này, chúng ta sẽ tạo toàn bộ hạ tầng mạng VPC của Rookwork chỉ bằng một thao tác duy nhất nhờ tính năng **"VPC and more"** của AWS, giúp tự động tạo đồng bộ VPC, Subnets, Route Tables, Internet Gateway, NAT Gateway và VPC Endpoint.

---

## Các bước thực hiện

### Bước 1: Truy cập dịch vụ VPC

- Đăng nhập vào **AWS Management Console**.
- Tìm kiếm dịch vụ **VPC** trên thanh tìm kiếm và chọn **Create VPC**.

---

### Bước 2: Chọn tùy chọn "VPC and more"

- Chọn tùy chọn **`VPC and more`** để tạo nhanh toàn bộ hệ thống mạng trong một lần.
- Thay vì tạo từng tài nguyên riêng lẻ (Subnet, Route Table, IGW,...), AWS sẽ tự động sinh ra tất cả dựa trên cấu hình bạn nhập.

---

### Bước 3: Cấu hình các thông số

Nhập các thông số sau vào form cấu hình:

| Tham số | Giá trị |
| :--- | :--- |
| **Name tag auto-generation** | `rookwork` |
| **IPv4 CIDR block** | `10.0.0.0/16` (mặc định) |
| **Number of Availability Zones** | `2` (`ap-southeast-1a` và `ap-southeast-1b`) |
| **Number of public subnets** | `2` (tự động tạo `rookwork-subnet-public1-...` và `rookwork-subnet-public2-...`) |
| **Number of private subnets** | `2` (tự động tạo `rookwork-subnet-private1-...` và `rookwork-subnet-private2-...`) |
| **NAT gateways** | `1 per AZ` |
| **VPC endpoints** | `S3 Gateway` (tự động tạo `rookwork-vpce-s3`) |

---

### Bước 4: Xem trước và tạo VPC

- Nhấn **Preview** để xem trước bản đồ tài nguyên sẽ được tạo.
- Nhấn **Create VPC** và chờ hệ thống triển khai (khoảng 1–2 phút).

Dưới đây là kết quả **Bản đồ tài nguyên (Resource Map)** cho thấy các Route Tables (`rookwork-rtb-public`, `rookwork-rtb-private1-ap-southeast-1a`, v.v.) đã định tuyến chính xác.

![Bản đồ tài nguyên VPC Rookwork sau khi khởi tạo thành công](/images/5-Workshop/5.1-Workshop-overview/vpc-architecture.png)

---

## Kết quả đạt được

Sau khi hoàn thành bước này, hệ thống sẽ tự động tạo ra các tài nguyên sau:

- ✅ **1 VPC**: `rookwork-vpc` với CIDR `10.0.0.0/16`
- ✅ **4 Subnets**: 2 Public + 2 Private trải dài trên 2 AZs
- ✅ **1 Internet Gateway**: `rookwork-igw`
- ✅ **1 NAT Gateway**: `rookwork-nat` (với Elastic IP tự động gán)
- ✅ **3 Route Tables**: 1 Public + 2 Private
- ✅ **1 VPC Endpoint**: `rookwork-vpce-s3` (S3 Gateway)

> [!TIP]
> Sau khi tạo xong, hãy điều hướng đến mục **Resource Map** (trong trang chi tiết VPC) để kiểm tra trực quan toàn bộ sơ đồ kết nối mạng đã được thiết lập đúng chưa.
