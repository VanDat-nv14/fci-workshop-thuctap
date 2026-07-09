---
title: "Thiết lập Security Groups"
date: 2026-06-01
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

# Thiết lập Security Groups

## Mục đích

**Security Group** hoạt động như một **tường lửa ảo (Virtual Firewall)** cấp độ instance, kiểm soát luồng traffic **Inbound** (đến) và **Outbound** (đi). Trong hệ thống Rookwork, chúng ta áp dụng mô hình **bảo mật phân tầng (Layered Security)**:

- **`rookwork-alb-sg`**: Chỉ mở Port 80/443 cho toàn bộ internet.
- **`backend-sg`**: Chỉ nhận traffic từ **`rookwork-alb-sg`** — backend hoàn toàn không tiếp xúc trực tiếp với internet.

Cả 2 Security Group đều nằm trong VPC **`vpc-027222b4e12adb751`** (VPC rookwork).

---

## Các bước thực hiện

### Bước 1: Điều hướng đến Security Groups

- Trên menu bên trái, chọn **Security Groups** dưới mục **Security**.
- Nhấn **Create security group**.

---

### Bước 2: Tạo SG cho Load Balancer (`rookwork-alb-sg`)

Điền các thông tin sau:

| Trường | Giá trị |
| :--- | :--- |
| **Security group name** | `rookwork-alb-sg` |
| **Description** | Security group for Application Load Balancer |
| **VPC** | Chọn VPC `rookwork` |

**Inbound rules:**

| Type | Protocol | Port Range | Source |
| :--- | :--- | :--- | :--- |
| HTTP | TCP | 80 | `0.0.0.0/0` (Mọi nơi) |
| HTTPS | TCP | 443 | `0.0.0.0/0` (Mọi nơi) |

**Outbound rules:** Giữ mặc định (All traffic – `0.0.0.0/0`)

Nhấn **Create security group** để lưu.

---

### Bước 3: Tạo SG cho Backend (`backend-sg`)

Điền các thông tin sau:

| Trường | Giá trị |
| :--- | :--- |
| **Security group name** | `backend-sg` |
| **Description** | Security group for Backend EC2 instances |
| **VPC** | Chọn VPC `rookwork` |

**Inbound rules:**

| Type | Protocol | Port Range | Source |
| :--- | :--- | :--- | :--- |
| Custom TCP | TCP | 8080 | `rookwork-alb-sg` (Security Group ID) |

> [!IMPORTANT]
> Tại trường **Source**, thay vì nhập CIDR IP, hãy chọn **Security Group** và gõ tên `rookwork-alb-sg`. Điều này đảm bảo backend **chỉ nhận** request từ Load Balancer, không nhận từ bất kỳ nguồn internet nào khác.

**Outbound rules:** Giữ mặc định (All traffic – `0.0.0.0/0`)

Nhấn **Create security group** để lưu.

---

## Kết quả đạt được

![Các Security Group của Rookwork đã được tạo thành công](/images/5-Workshop/5.5-Policy/security-groups.png)

Sau khi hoàn thành, bạn sẽ thấy 2 Security Groups mới trong danh sách:

- ✅ `rookwork-alb-sg` – Cho phép HTTP/HTTPS từ mọi nơi.
- ✅ `backend-sg` – Chỉ cho phép Port 8080 từ `rookwork-alb-sg`.

> [!TIP]
> Mô hình này gọi là **Defense-in-Depth (Bảo mật theo chiều sâu)**: ngay cả khi kẻ tấn công vượt qua được Load Balancer, backend vẫn không thể bị truy cập trực tiếp từ internet.
