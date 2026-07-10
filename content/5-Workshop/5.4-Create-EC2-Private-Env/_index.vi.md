---
title: "Tạo EC2 & Kiểm tra môi trường Private"
date: 2026-07-08
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---


### Tổng quan

Trong workshop này, bạn sẽ thực hiện tìm hiểu cách thức triển khai máy chủ EC2 an toàn bên trong các private subnet thuộc Amazon VPC. Chúng ta sẽ tiến hành tạo hai máy chủ EC2 với tên gọi **rookwork-ec2-1** và **rookwork-ec2-2** nằm bên trong các private subnet của VPC. Bạn sẽ cấu hình chi tiết cài đặt mạng (network settings) để đảm bảo các máy chủ được bảo vệ an toàn khỏi internet công cộng và tiến hành kiểm tra cấu hình môi trường bên trong chúng (IP nội bộ, kết nối mạng thông qua NAT Gateway và cài đặt các môi trường chạy cần thiết như Docker, Node.js, Git).

---

### Bước 1: Khởi tạo rookwork-ec2-1

1. Truy cập vào AWS Management Console và mở dịch vụ **EC2 Dashboard**.
2. Nhấn chọn **Launch instances**.
3. Tại mục **Name and tags**, nhập tên `rookwork-ec2-1` vào trường Name.
4. Tại mục **Application and OS Images (Amazon Machine Image)**, lựa chọn hệ điều hành **Amazon Linux 2023 AMI** (hoặc AMI phù hợp với dự án của bạn).

![Khởi tạo máy chủ - Bước 1](/images/5-Workshop/5.4/ec2-1-launch.png)

---

### Bước 2: Cấu hình Cài đặt mạng (Network settings)

1. Cuộn xuống phần **Network settings** và nhấn nút **Edit** (phía trên bên phải của khung cấu hình).
2. **VPC**: Chọn VPC tùy chỉnh của bạn (ví dụ: `Rookwork-VPC`).
3. **Subnet**: Chọn một **Private Subnet** (ví dụ: `rookwork-private-subnet-1`).
4. **Auto-assign public IP**: Chọn **Disable** (điều này đảm bảo máy chủ không nhận IP công cộng và nằm hoàn toàn trong mạng nội bộ).
5. **Firewall (security groups)**: Lựa chọn hoặc tạo mới Security Group cho phép truy cập cổng SSH hoặc cổng dịch vụ từ Bastion host, Application Load Balancer hoặc từ các dải mạng nội bộ thuộc VPC.

![Cấu hình Network Settings](/images/5-Workshop/5.4/ec2-1-step2.png)

![Cấu hình Network Settings](/images/5-Workshop/5.4/ec2-1-step3.png)

---

### Bước 3: Cấu hình Key Pair và Storage

1. Tại mục **Key pair (login)**, chọn Key Pair hiện có hoặc tạo mới để sử dụng cho việc kết nối bảo mật sau này.
2. Tại mục **Configure storage**, kiểm tra dung lượng và loại ổ cứng Root Volume (ví dụ: gp3).
3. Nhấp chọn **Launch instance** ở bảng bên phải để hoàn tất việc khởi tạo máy chủ.

---

### Bước 4: Khởi tạo rookwork-ec2-2

1. Thực hiện các bước tương tự như trên để khởi tạo máy chủ EC2 thứ hai.
2. Tại trường **Name**, nhập tên `rookwork-ec2-2`.
3. Tại phần **Network settings**, chọn cùng VPC tùy chỉnh nhưng chọn **Private Subnet thứ 2** (ví dụ: `rookwork-private-subnet-2`) để đảm bảo tính sẵn sàng cao (High Availability) giữa các Availability Zones khác nhau.
4. Giữ nguyên mục **Auto-assign public IP** là **Disable**.
5. Gán Security Group tương tự hoặc Security Group dành riêng cho private EC2 thứ hai.
6. Nhấp chọn **Launch instance** để tạo máy chủ.

![Khởi tạo rookwork-ec2-2](/images/5-Workshop/5.4/ec2-2-step1.png)

---

### Bước 5: Kết nối và Kiểm tra môi trường trong EC2 Private

Vì cả hai máy chủ EC2 đều nằm trong private subnet, chúng không có IP công cộng và không thể truy cập trực tiếp từ Internet qua giao thức SSH thông thường. Chúng ta sẽ kết nối vào máy chủ sử dụng **AWS Systems Manager (SSM) Session Manager**.

1. Tại danh sách máy chủ EC2, tích chọn máy chủ cần kết nối (ví dụ: `rookwork-ec2-2`) và nhấn nút **Connect** ở góc trên bên phải.

![Kết nối EC2 - Bước 1](/images/5-Workshop/5.4/check-connect-ec2-step1.png)

2. Chuyển sang tab **Session Manager** và nhấp chọn nút **Connect** màu cam ở góc dưới bên phải để bắt đầu phiên kết nối.

![Kết nối EC2 - Bước 2](/images/5-Workshop/5.4/check-connect-ec2-step2.png)

---

### Bước 6: Kiểm tra cấu hình và cài đặt môi trường

Sau khi cửa sổ terminal kết nối thành công, tiến hành thực hiện các kiểm tra sau:

#### 1. Kiểm tra kết nối Internet (qua NAT Gateway)
Để đảm bảo private subnet có cấu hình bảng định tuyến (Route Table) trỏ lưu lượng đi ra ngoài Internet qua NAT Gateway (giúp tải các gói thư viện/cập nhật hệ thống):
```bash
ping -c 3 google.com
```

![Kiểm tra môi trường thành công](/images/5-Workshop/5.4/check-connect-completed.png)
#### 2. Kiểm tra các phần mềm/môi trường đã cài đặt
Chạy các lệnh kiểm tra phiên bản để đảm bảo các công cụ cần thiết phục vụ cho việc triển khai dự án Rookwork đã sẵn sàng:

* **Docker**:
  ```bash
  docker --version
  ```
  *Nếu chưa cài đặt, chạy lệnh sau:*
  ```bash
  sudo dnf update -y
  sudo dnf install -y docker
  sudo systemctl start docker
  sudo systemctl enable docker
  sudo usermod -aG docker $USER
  ```

* **Node.js**:
  ```bash
  node --version
  ```
  *Nếu chưa cài đặt, chạy lệnh sau để cài đặt qua NVM:*
  ```bash
  curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
  source ~/.bashrc
  nvm install 20
  ```

* **Git**:
  ```bash
  git --version
  ```
  *Nếu chưa cài đặt, chạy lệnh sau:*
  ```bash
  sudo dnf install -y git
  ```
