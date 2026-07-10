---
title : "Các bước chuẩn bị"
date : 2024-01-01 
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

Trước khi bắt đầu triển khai hạ tầng AWS cho dự án **Rookwork**, bạn cần đảm bảo các điều kiện tiên quyết sau đây đã được thiết lập đầy đủ.

---

#### 1. Tài khoản AWS

- Có tài khoản **AWS** đang hoạt động (có thể dùng AWS Free Tier hoặc tài khoản học tập).
- Tài khoản cần có quyền tạo các tài nguyên: **VPC, EC2, RDS, S3, SES, CloudFront, WAF, Route 53, IAM, ACM**.

{{% notice tip %}}
💡 Nên sử dụng **IAM User** thay vì Root Account để triển khai. Tạo IAM User với quyền **AdministratorAccess** hoặc gán policy tùy chỉnh theo tài nguyên cần dùng.
{{% /notice %}}

---

#### 2. Công cụ cần thiết

Cài đặt các công cụ sau trên máy tính của bạn:

| Công cụ | Phiên bản | Mục đích |
|---------|-----------|---------|
| **Java JDK** | 17+ | Build và chạy Spring Boot backend |
| **Maven** | 3.8+ | Quản lý dependency và build backend |
| **Node.js** | 18+ | Build React 19 frontend |
| **Yarn** | 1.22+ | Quản lý package frontend |
| **Docker** | 24+ | Đóng gói ứng dụng thành container |
| **AWS CLI** | v2 | Tương tác với AWS từ command line |
| **Git** | 2.x | Quản lý source code |

##### Kiểm tra cài đặt

Chạy các lệnh sau để xác nhận các công cụ đã được cài đúng:

```bash
java --version        # Java 17+
mvn --version         # Maven 3.8+
node --version        # Node.js 18+
yarn --version        # Yarn 1.22+
docker --version      # Docker 24+
aws --version         # AWS CLI 2.x
git --version         # Git 2.x
```

---

#### 3. Cấu hình AWS CLI

Cấu hình AWS CLI với thông tin credentials của IAM User:

```bash
aws configure
```

Nhập lần lượt:
- **AWS Access Key ID** — lấy từ IAM Console → Security credentials
- **AWS Secret Access Key** — lấy từ IAM Console → Security credentials
- **Default region name** — ví dụ: `ap-southeast-1` (Singapore)
- **Default output format** — `json`

##### Kiểm tra kết nối

```bash
aws sts get-caller-identity
```

---

#### 4. IAM Permissions

Gắn **IAM policy** sau vào IAM User của bạn để có đủ quyền triển khai hạ tầng Rookwork:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:*",
                "rds:*",
                "s3:*",
                "cloudfront:*",
                "wafv2:*",
                "route53:*",
                "ses:*",
                "acm:*",
                "iam:CreateRole",
                "iam:AttachRolePolicy",
                "iam:PassRole",
                "iam:GetRole",
                "iam:CreateInstanceProfile",
                "iam:AddRoleToInstanceProfile",
                "elasticloadbalancing:*",
                "autoscaling:*",
                "ecr:*",
                "logs:*",
                "cloudwatch:*"
            ],
            "Resource": "*"
        }
    ]
}
```

---

#### 5. Chuẩn bị Source Code

Clone source code dự án Rookwork về máy:

```bash
# Backend (Spring Boot)
git clone https://github.com/ChauThanhDat720/rookwork-backend.git

# Frontend (React 19)
git clone https://github.com/ChauThanhDat720/rookwork-frontend.git
```

##### Build thử Backend

```bash
cd rookwork-backend
mvn clean package -DskipTests
```

##### Chạy thử Frontend

```bash
cd rookwork-frontend
yarn install
yarn build:browser
yarn start:browser
```

---

#### 6. Kiểm tra Docker

Đảm bảo Docker đang chạy và có thể build image:

```bash
docker info
docker pull amazoncorretto:17
```


---

Sau khi hoàn thành tất cả các bước chuẩn bị, bạn đã sẵn sàng để bắt đầu triển khai hạ tầng AWS cho Rookwork từ phần [5.3 — Khởi tạo VPC, NAT Gateway và Security Groups](../5.3-S3-vpc/).