---
title: "Bản đề xuất"
date: 2026-04-17
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Rookwork – Nền tảng quản lý công việc & cộng tác nhóm trên AWS
## Giải pháp hạ tầng Cloud đầy đủ cho ứng dụng thực tế trên Amazon Web Services

---

### 1. Tóm tắt điều hành

**Rookwork** là nền tảng quản lý công việc và cộng tác nhóm đa nền tảng, hỗ trợ đồng thời trên **Web (Browser)** và **Desktop (Electron)**. Ứng dụng cho phép các nhóm làm việc tổ chức dự án, phân công nhiệm vụ, theo dõi tiến độ và giao tiếp theo thời gian thực qua WebSocket.

Dự án được thực hiện trong khuôn khổ kỳ thực tập tại **Công ty TNHH Amazon Web Services Viet Nam** (17/04/2026 – 12/07/2026), dưới sự hướng dẫn của Mentor **Nguyễn Gia Hưng**. Mục tiêu cốt lõi là xây dựng một hệ thống hoàn chỉnh từ lập trình ứng dụng đến triển khai toàn bộ hạ tầng đám mây trên AWS theo tiêu chuẩn thực tế của ngành, bao gồm: kiến trúc 3-tier, phân vùng mạng VPC, cơ sở dữ liệu quan hệ RDS, phân phối nội dung toàn cầu qua CloudFront, và quy trình CI/CD tự động hóa hoàn toàn bằng GitHub Actions.

---

### 2. Tuyên bố vấn đề

**Vấn đề hiện tại:**
Các công cụ quản lý công việc nhóm phổ biến như Trello, Jira hay Notion đang được sử dụng rộng rãi nhưng chủ yếu là giải pháp SaaS bên thứ ba, thường tốn kém khi mở rộng quy mô và không cho phép kiểm soát hoàn toàn về dữ liệu cũng như hạ tầng bên dưới. Chưa có giải pháp nội địa nào được xây dựng và vận hành hoàn toàn trên hạ tầng AWS Cloud một cách bài bản và minh bạch.

**Giải pháp:**
Rookwork được triển khai trên hạ tầng AWS với kiến trúc 3 tầng: Frontend React/Vite được phân phối toàn cầu qua Amazon S3 + CloudFront, Backend Spring Boot chạy trong Docker container trên Amazon EC2, và cơ sở dữ liệu Amazon RDS PostgreSQL đặt trong Private Subnet được bảo vệ hoàn toàn khỏi internet. Toàn bộ quy trình CI/CD được tự động hóa bằng GitHub Actions, từ khi lập trình viên push code đến khi người dùng nhận được bản cập nhật mới nhất. Bảo mật được áp dụng theo nguyên tắc Defense-in-Depth với Bastion Host, IAM Least Privilege, Spring Security + JWT và AWS Secrets Manager.

**Lợi ích và ROI:**
Dự án cung cấp một sản phẩm phần mềm thực tế có thể sử dụng ngay, đồng thời là nền tảng học tập tổng hợp toàn bộ vòng đời phát triển phần mềm trên AWS (SDLC). Chi phí hạ tầng ước tính thấp nhờ sử dụng các gói AWS Free Tier và tối ưu hóa tài nguyên. Hệ thống có thể mở rộng dễ dàng nhờ kiến trúc ALB + Auto Scaling Group kết hợp cùng quy trình CI/CD tự động, giúp giảm đáng kể thời gian và rủi ro khi phát hành phiên bản mới.

---

### 3. Kiến trúc giải pháp

Rookwork áp dụng kiến trúc **3-tier (3 tầng)** trên nền tảng AWS, triển khai tại vùng `ap-southeast-1` (Singapore):

```
[ Người dùng (Web / Desktop) ]
        │
        ▼
[ Amazon CloudFront (CDN) ] ←── [ Amazon S3 (Frontend Static Files) ]
        │
        ▼ (API requests đến api.rookwork.asia)
[ Amazon Route 53 (DNS) ]
        │
        ▼
[ Application Load Balancer (ALB) ]
        │
        ▼
[ Auto Scaling Group ]
  └── [ Amazon EC2 – Docker Container (Spring Boot Backend, port 8080) ]
        │
        ├── [ AWS Secrets Manager (DB Credentials, JWT Secret) ]
        │
        ▼
[ Amazon RDS – PostgreSQL (Private Subnet) ]
```

**Phân tầng mạng (VPC Architecture):**

| Tầng | Tài nguyên | Vị trí mạng |
| :--- | :--- | :--- |
| **Tầng trình diện (Presentation)** | Amazon S3 + CloudFront, tên miền `rookwork.asia` | Public – AWS Edge Locations |
| **Tầng ứng dụng (Application)** | ALB + Auto Scaling Group + EC2 (Spring Boot trong Docker) | Public Subnet (Bastion Host bảo vệ SSH) |
| **Tầng dữ liệu (Data)** | Amazon RDS PostgreSQL | Private Subnet (không tiếp xúc internet) |

**Dịch vụ AWS sử dụng:**

| Dịch vụ AWS | Vai trò trong hệ thống |
| :--- | :--- |
| **Amazon EC2** | Chạy Docker container cho Spring Boot Backend |
| **Amazon RDS (PostgreSQL)** | Cơ sở dữ liệu quan hệ trong Private Subnet |
| **Amazon S3** | Lưu trữ file build tĩnh của Frontend (React/Vite) |
| **Amazon CloudFront** | CDN phân phối Frontend toàn cầu, tối ưu cache |
| **Amazon Route 53** | Quản lý DNS, phân giải tên miền `rookwork.asia` |
| **AWS VPC** | Phân vùng mạng bảo mật (Public/Private Subnet) |
| **Application Load Balancer** | Cân bằng tải và định tuyến traffic vào Backend |
| **Auto Scaling Group** | Tự động co giãn số lượng EC2 theo tải |
| **AWS IAM** | Quản lý quyền truy cập cho GitHub Actions và tài nguyên AWS |
| **AWS Secrets Manager** | Lưu trữ bảo mật biến nhạy cảm (DB credentials, JWT Secret) |
| **AWS Bastion Host** | Cổng quản trị SSH bảo mật vào các máy chủ nội bộ |

---

### 4. Triển khai kỹ thuật

**Công nghệ sử dụng:**

*Backend:*

| Thành phần | Công nghệ | Phiên bản |
| :--- | :--- | :--- |
| Ngôn ngữ | Java | 21 (LTS) |
| Framework | Spring Boot | 4.0.2 |
| Bảo mật | Spring Security + JWT (JJWT 0.12.6) | – |
| Xác thực bên thứ 3 | Google OAuth2 | – |
| ORM | Spring Data JPA + Hibernate | – |
| Database Migration | Flyway (PostgreSQL) | – |
| Giao tiếp thời gian thực | Spring WebSocket + STOMP | – |
| Quản lý bí mật | AWS Secrets Manager (spring-cloud-aws 4.0.2) | – |
| Đóng gói | Docker (eclipse-temurin:21-jdk) | – |
| Build tool | Apache Maven | – |

*Frontend:*

| Thành phần | Công nghệ | Phiên bản |
| :--- | :--- | :--- |
| Framework UI | React | 19 |
| Build tool | Vite | – |
| CSS Framework | Tailwind CSS | v4 |
| Desktop App | Electron | 28 |
| Routing | React Router DOM | v7.13.0 |
| Realtime | STOMP.js + SockJS | – |
| Animation | Motion (Framer Motion) | v12 |
| Drag & Drop | React DnD | v16 |
| Quản lý Package | Yarn Workspaces (Monorepo) | – |

**Quy trình CI/CD (GitHub Actions):**

Hệ thống sử dụng **GitHub Actions** để tự động hóa hoàn toàn quy trình build và deploy khi có code mới được đẩy lên nhánh `main`:

*Pipeline Backend:*
```
Push to main → Checkout code → Setup JDK 21 → Maven Build JAR
→ SCP copy JAR to EC2 → SSH into EC2 → Docker stop/rm old container
→ Docker build new image → Docker run new container (port 8080)
```

*Pipeline Frontend:*
```
Push to main → Checkout code → Setup Node.js 22 → Yarn install
→ Vite Build (VITE_API_URL=https://api.rookwork.asia)
→ Configure AWS Credentials → S3 sync assets (cache: 1 year)
→ S3 upload index.html (no-cache) → CloudFront Invalidation
```

---

### 5. Lộ trình & Mốc triển khai

| Giai đoạn | Tuần | Nội dung triển khai |
| :---: | :---: | :--- |
| **Nền tảng Cloud** | Tuần 1–2 | Học AWS cơ bản: IAM, EC2, S3, MFA, Cost Management |
| **Mạng & Lưu trữ** | Tuần 3–4 | VPC, Bastion Host, NAT Gateway, S3 Static Web, CloudFront |
| **Triển khai ứng dụng** | Tuần 5–6 | Deploy app lên EC2, kết nối RDS, DNS Route 53 |
| **Hạ tầng dự án thực tế** | Tuần 7–8 | Xây dựng VPC Rookwork thực tế, ALB, Auto Scaling, CI/CD Pipeline |
| **Production & Giám sát** | Tuần 9–10 | Deploy Production, HTTPS, CloudWatch, tối ưu bảo mật & chi phí |
| **Hoàn thiện & Bàn giao** | Tuần 11–12 | Load Testing, Demo nội bộ, Bug Fix, viết báo cáo, bàn giao |

---

### 6. Ước tính ngân sách

**Chi phí hạ tầng AWS (ước tính hàng tháng):**

| Dịch vụ | Cấu hình | Chi phí ước tính |
| :--- | :--- | :--- |
| Amazon EC2 (t3.micro) | 1 instance, Free Tier 12 tháng | ~0,00 USD/tháng |
| Amazon RDS (db.t3.micro) | PostgreSQL, Single-AZ, 20 GB SSD | ~0,00 USD/tháng (Free Tier) |
| Amazon S3 | 5 GB lưu trữ, ~10.000 request | ~0,12 USD/tháng |
| Amazon CloudFront | 10 GB data transfer out | ~0,00 USD/tháng (Free Tier) |
| Amazon Route 53 | 1 hosted zone | ~0,50 USD/tháng |
| AWS Secrets Manager | 2 secret | ~0,80 USD/tháng |
| **Tổng ước tính** | | **~1,42 USD/tháng** |

> *Lưu ý: Chi phí thực tế có thể thay đổi tùy theo lưu lượng truy cập và thời gian sử dụng ngoài AWS Free Tier.*

---

### 7. Đánh giá rủi ro

**Ma trận rủi ro:**

| Rủi ro | Mức ảnh hưởng | Xác suất | Chiến lược giảm thiểu |
| :--- | :---: | :---: | :--- |
| EC2 instance gặp sự cố / bị quá tải | Cao | Thấp | Auto Scaling Group + ALB tự động phân tải và khởi động instance mới |
| Mất dữ liệu RDS | Rất cao | Rất thấp | Kích hoạt automated backup hàng ngày + Manual Snapshot định kỳ |
| Vượt ngân sách AWS | Trung bình | Trung bình | Thiết lập AWS Budget Alerts, thường xuyên kiểm tra Cost Explorer |
| Lộ thông tin nhạy cảm (credentials) | Cao | Thấp | Sử dụng AWS Secrets Manager + IAM Least Privilege, không hardcode credential |
| Sự cố khi deploy (rollback) | Trung bình | Trung bình | Giữ Docker image cũ, có thể rollback bằng cách chạy lại container phiên bản trước |

**Kế hoạch dự phòng:**
- Tắt Auto Scaling và chạy thủ công trên EC2 đơn nếu CI/CD pipeline gặp sự cố.
- Sử dụng RDS Snapshot để khôi phục dữ liệu trong vòng 5 phút nếu xảy ra lỗi nghiêm trọng.
- Có thể hạ cấp xuống mô hình deploy đơn giản (không ALB) để tiết kiệm chi phí khi cần thiết.

---

### 8. Kết quả kỳ vọng

- Hệ thống **Rookwork** được triển khai hoàn chỉnh và ổn định trên môi trường Production AWS với tên miền chính thức `rookwork.asia`.
- Quy trình **CI/CD tự động** hoạt động đầu cuối: từ khi lập trình viên push code đến khi người dùng cuối nhận được bản cập nhật mới nhất mà không cần can thiệp thủ công.
- Hạ tầng được thiết kế đúng nguyên tắc **Defense-in-Depth** (bảo mật theo chiều sâu) và **Least Privilege** (phân quyền tối thiểu).
- Sinh viên tích lũy được kinh nghiệm thực tế vận hành một hệ thống Cloud hoàn chỉnh từ đầu đến cuối, bao gồm cả thiết kế kiến trúc, triển khai, giám sát và tối ưu hóa, sẵn sàng áp dụng vào môi trường làm việc chuyên nghiệp.
- Đội nhóm Rookwork bàn giao được một sản phẩm phần mềm **sử dụng được thực tế**, không chỉ dừng lại ở demo hay môi trường dev.
