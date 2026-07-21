---
title: "Khởi tạo VPC, NAT Gateway và Security Groups"
date : 2026-07-09 
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---
Trong bước này, chúng ta sẽ thiết lập toàn bộ hạ tầng mạng cho dự án Rookwork gồm 3 phần chính:
1. **Tạo VPC** với tính năng "VPC and more"
2. **Tạo NAT Gateway** cho Private Subnet kết nối internet
3. **Tạo Security Groups** phân tầng cho ALB và Backend

---

## Phần 1 – Khởi tạo VPC

### Bước 1: Truy cập dịch vụ VPC

- Đăng nhập vào **AWS Management Console**, đảm bảo đang ở Region **Asia Pacific (Singapore) `ap-southeast-1`**.
- Tìm kiếm **VPC** trên thanh tìm kiếm → chọn **Your VPCs** → nhấn **Create VPC**.

---

### Bước 2: Chọn "VPC and more" và điền cấu hình

- Tại **Resources to create**, chọn **`VPC and more`** (không phải "VPC only").
- Điền theo bảng sau:

| Tham số | Giá trị |
| :--- | :--- |
| **Name tag auto-generation** | `rookwork` |
| **IPv4 CIDR block** | `10.0.0.0/16` |
| **IPv6 CIDR block** | No IPv6 CIDR block |
| **Tenancy** | Default |
| **Number of Availability Zones** | `2` (`ap-southeast-1a` và `ap-southeast-1b`) |
| **Number of public subnets** | `2` |
| **Number of private subnets** | `2` |
| **NAT gateways** | `None` *(tạo riêng ở Phần 2)* |
| **VPC endpoints** | `S3 Gateway` |
| **DNS hostnames** | Enable |
| **DNS resolution** | Enable |

Phía phải màn hình sẽ hiển thị **Preview** sơ đồ VPC với 4 Subnets trên 2 AZs.

![Cấu hình VPC and more – Name, CIDR, số lượng Subnets và Preview](/images/5-Workshop/5.3/vpc-1.png)

- **NAT gateways**: Chọn `None` (sẽ tạo riêng ở Phần 2 bên dưới).
- **VPC endpoints**: Chọn **`S3 Gateway`** để kết nối nội bộ đến S3 không qua internet.
- Giữ nguyên **DNS options**:  Enable DNS hostnames và  Enable DNS resolution.

![Cấu hình NAT Gateway None, VPC Endpoint S3 Gateway và DNS options](/images/5-Workshop/5.3/vpc-2.png)

Nhấn **Create VPC** để triển khai.

---

### Kết quả Phần 1

Hệ thống sẽ tự động tạo:

- **1 VPC**: `rookwork-vpc` — CIDR `10.0.0.0/16`
- **4 Subnets**: 2 Public + 2 Private trên 2 AZs
- **1 Internet Gateway**: `rookwork-igw`
- **3 Route Tables**: 1 Public + 2 Private
- **1 VPC Endpoint**: `rookwork-vpce-s3` (S3 Gateway)

> [!TIP]
> Sau khi tạo xong, vào **Your VPCs → rookwork-vpc → tab Resource Map** để kiểm tra trực quan toàn bộ sơ đồ kết nối mạng.

---

## Phần 2 – Tạo NAT Gateway

NAT Gateway cho phép các tài nguyên trong **Private Subnet** (EC2 Backend) tải thư viện, bản cập nhật từ internet mà **không bị lộ địa chỉ IP** ra ngoài.

### Bước 1: Tạo NAT Gateway

- Trên menu bên trái VPC, chọn **NAT Gateways** → **Create NAT gateway**.
- Điền thông tin:

| Tham số | Giá trị |
| :--- | :--- |
| **Name** | `rookwork-nat` |
| **Availability mode** | `Regional - new` |
| **VPC** | `rookwork-vpc` |
| **Connectivity type** | `Public` |
| **Elastic IP allocation** | `Automatic` |

![Cấu hình NAT Gateway – Name, Availability mode, VPC](/images/5-Workshop/5.3/vpc-nat-1.png)

---

### Bước 2: Xác nhận Tags và tạo

- Kiểm tra tag `Name = rookwork-nat` đã được điền đúng.
- Nhấn **Create NAT gateway**.

![Xác nhận Tags và nhấn Create NAT gateway](/images/5-Workshop/5.3/vpc-nat-2.png)

---

### Bước 3: Kiểm tra trạng thái

- Đảm bảo cột **State** hiển thị ** Available** (màu xanh lá).
- Nếu còn `Pending`, chờ 1–2 phút rồi làm mới trang.

> [!NOTE]
> NAT Gateway tính phí theo **giờ** và **lượng dữ liệu**. Nhớ xóa sau khi hoàn thành lab để tránh phát sinh chi phí.

---

## Phần 3 – Thiết lập Security Groups

Security Group hoạt động như **tường lửa ảo** cấp instance. Chúng ta tạo 3 Security Groups theo mô hình **bảo mật phân tầng**:

- **`rookwork-alb-sg`**: Mở HTTP/HTTPS từ internet → nhận request người dùng.
- **`backend-sg`**: Chỉ nhận traffic từ `rookwork-alb-sg` → backend không tiếp xúc internet trực tiếp.
- **`rds-ec2-1`**: Chỉ nhận kết nối PostgreSQL (port 5432) từ `ec2-rds-1` → RDS hoàn toàn cô lập khỏi internet.

### Bước 1: Tạo Security Group cho ALB (`rookwork-alb-sg`)

- Menu bên trái VPC → **Security Groups** → **Create security group**.

| Trường | Giá trị |
| :--- | :--- |
| **Security group name** | `rookwork-alb-sg` |
| **Description** | Security group for Application Load Balancer |
| **VPC** | `rookwork-vpc` |

**Inbound rules:**

| Type | Protocol | Port | Source |
| :--- | :--- | :--- | :--- |
| HTTPS | TCP | 443 | `0.0.0.0/0` |
| HTTP | TCP | 80 | `0.0.0.0/0` |

**Outbound rules**: Giữ mặc định (All traffic). Nhấn **Create security group**.

![Tạo rookwork-alb-sg với Inbound HTTP/HTTPS từ mọi nơi](/images/5-Workshop/5.3/sg-alb-1.png)

---

### Bước 2: Tạo Security Group cho Backend (`backend-sg`)

- Nhấn **Create security group** thêm lần nữa.

| Trường | Giá trị |
| :--- | :--- |
| **Security group name** | `backend-sg` |
| **Description** | Security group for Backend EC2 instances |
| **VPC** | `rookwork-vpc` |

**Inbound rules**: Để trống (chưa thêm — sẽ thêm ở Bước 3).  
**Outbound rules**: Giữ mặc định. Nhấn **Create security group**.

![Tạo backend-sg – Inbound để trống, Outbound mặc định](/images/5-Workshop/5.3/sg-be-1.png)

---

### Bước 3: Thêm Inbound rule cho `backend-sg`

Sau khi `rookwork-alb-sg` đã được tạo và có Security Group ID, quay lại chỉnh sửa **`backend-sg`** → **Inbound rules** → **Edit inbound rules**:

| Type | Protocol | Port | Source |
| :--- | :--- | :--- | :--- |
| Custom TCP | TCP | 8080 | `rookwork-alb-sg` (chọn theo SG ID) |
| HTTP | TCP | 80 | `rookwork-alb-sg` (chọn theo SG ID) |

> [!IMPORTANT]
> Tại **Source**, chọn **Custom** và gõ tên hoặc ID của `rookwork-alb-sg` thay vì nhập CIDR. Điều này đảm bảo Backend **chỉ nhận traffic từ Load Balancer**, không từ internet.

Nhấn **Save rules**.

![Inbound rules của backend-sg: Port 8080 và 80 chỉ nhận từ rookwork-alb-sg](/images/5-Workshop/5.3/sg-be-2.png)

---

### Bước 4: Tạo Security Group cho RDS (`rds-ec2-1`)

Để bảo vệ database, cần tạo một Security Group riêng chỉ cho phép EC2 Backend kết nối vào RDS qua port PostgreSQL.

- Nhấn **Create security group** thêm lần nữa.

| Trường | Giá trị |
| :--- | :--- |
| **Security group name** | `rds-ec2-1` |
| **Description** | Security group for RDS PostgreSQL |
| **VPC** | `rookwork-vpc` |

**Inbound rules** — Thêm 2 rule:

| Type | Protocol | Port | Source |
| :--- | :--- | :--- | :--- |
| PostgreSQL | TCP | 5432 | `ec2-rds-1` (Security Group ID của EC2) |
| PostgreSQL | TCP | 5432 | `backend-sg` (Security Group ID) |

> [!IMPORTANT]
> Tại **Source**, chọn **Custom** và gõ Security Group ID của `ec2-rds-1` hoặc `backend-sg`. Điều này đảm bảo RDS **chỉ nhận kết nối từ EC2 Backend**, không bao giờ tiếp xúc trực tiếp với internet.

Nhấn **Save rules**.

![Edit inbound rules của rds-ec2-1: PostgreSQL port 5432 chỉ từ ec2-rds-1 và backend-sg](/images/5-Workshop/5.3/sg-rds-1.png)

---

## Tổng kết

Sau khi hoàn tất cả 3 phần, hạ tầng mạng Rookwork đã sẵn sàng:

| Tài nguyên | Tên | Trạng thái |
| :--- | :--- | :--- |
| VPC | `rookwork-vpc` | Available |
| Public Subnets | `rookwork-subnet-public1/2-...` | Available |
| Private Subnets | `rookwork-subnet-private1/2-...` | Available |
| Internet Gateway | `rookwork-igw` | Attached |
| NAT Gateway | `rookwork-nat` | Available |
| VPC Endpoint | `rookwork-vpce-s3` | Available |
| Security Group ALB | `rookwork-alb-sg` | Created |
| Security Group Backend | `backend-sg` | Created |
| Security Group RDS | `rds-ec2-1` | Created |

> [!TIP]
> Mô hình bảo mật **Defense-in-Depth**: ALB → Backend → RDS theo tầng. Mỗi lớp chỉ nhận traffic từ lớp phía trước, đảm bảo database không bao giờ tiếp xúc trực tiếp với internet.
