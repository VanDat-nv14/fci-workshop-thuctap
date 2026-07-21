---
title: "Bản đề xuất"
date : 2026-07-09 
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Rookwork - Phần mềm quản lý làm việc nhóm  

### 1. Tổng quan dự án  
Trong bối cảnh cách mạng công nghiệp 4.0 và xu hướng làm việc từ xa (remote working) cũng như làm việc lai (hybrid) ngày càng phổ biến, việc quản lý làm việc nhóm hiệu quả là một yếu tố sống còn đối với các cá nhân, nhóm dự án và doanh nghiệp. **Rookwork** là phần mềm quản lý làm việc nhóm tích hợp, hiện đại, được thiết kế để hoạt động dưới dạng ứng dụng đa nền tảng (browser và desktop app). Hệ thống sử dụng kiến trúc Client-Server với frontend phát triển bằng **React 19**, có thể đóng gói thành ứng dụng desktop bằng **Electron** để tạo trải nghiệm mượt mà, cùng backend vững chắc xây dựng trên nền tảng **Spring Boot** kết hợp cùng các dịch vụ của AWS.

> **Lưu ý về trạng thái hiện tại:** Ở giai đoạn hiện tại, nền tảng chính đang được người dùng sử dụng là phiên bản **web browser** (chạy trực tiếp trên trình duyệt, không cần cài đặt). Phiên bản **desktop (đóng gói bằng Electron)** đã được phát triển nhưng **chưa được cập nhật đồng bộ** với bản web mới nhất, do đó có thể còn thiếu một số tính năng hoặc bản vá so với phiên bản web hiện hành. Mục tiêu dài hạn của dự án vẫn là hoàn thiện và đồng bộ ứng dụng desktop đa nền tảng như định hướng kiến trúc ban đầu.

### 2. Vấn đề cần giải quyết  
Hiện nay, nhiều đội ngũ vẫn đối mặt với các khó khăn lớn trong việc cộng tác và quản lý dự án:
- **Sử dụng công cụ rời rạc:** Nhiều nhóm vẫn đang quản lý công việc thủ công qua các kênh không đồng bộ như Excel, Google Sheets, Messenger/Zalo và email. Điều này dẫn đến việc khó theo dõi tiến độ, dễ xảy ra nhầm lẫn, thiếu tính minh bạch và tiêu tốn nhiều thời gian trao đổi thông tin không cần thiết.
- **Cách thức sử dụng phức tạp:** Các nền tảng quản lý dự án hiện có (như Trello, Jira, Asana, Monday.com) thường có giao diện và luồng thao tác phức tạp, nhiều tầng cấu hình, đòi hỏi người dùng mất thời gian tìm hiểu và làm quen trước khi có thể sử dụng hiệu quả. Điều này gây khó khăn đặc biệt cho các nhóm nhỏ, sinh viên hoặc người mới bắt đầu, những đối tượng cần một công cụ đơn giản trực quan, dễ tiếp cận ngay từ lần sử dụng đầu tiên.
### 3. Kiến trúc giải pháp (Workflow)
Hệ thống hoạt động dựa trên mô hình phân tán trong môi trường AWS Cloud. Luồng xử lý được chia làm các hướng chính như sau:

![Rookwork Architecture](/images/2-Proposal/workflow.png)

**1. Luồng Mạng và Phân phối Nội dung (Networking & Content Delivery):**
- Mọi yêu cầu từ người dùng (Users) đều được phân giải tên miền qua **Amazon Route 53**.
- Giao diện Frontend tĩnh được lưu trữ hoàn toàn trên bucket **Amazon S3 (FE Static)**.
- Để tăng tốc độ tải trang và bảo mật, nội dung Frontend được phân phối qua **Amazon CloudFront** kết hợp với hệ thống tường lửa **AWS WAF** giúp ngăn chặn các cuộc tấn công web.

**2. Luồng Định tuyến và Tính toán Backend (Routing & Compute):**
- Các API traffic đi qua **Internet Gateway** vào mạng **Amazon VPC**, sau đó được phân tải bởi **Application Load Balancer (ALB)**.
- Backend xây dựng bằng Java Spring Boot được triển khai trên các máy ảo **Amazon EC2**. Các máy chủ này được đặt an toàn trong *Private Subnets*.
- Để EC2 trong mạng nội bộ có thể gọi các dịch vụ bên ngoài, hệ thống định tuyến Outbound Traffic thông qua **NAT Gateway**.

**3. Luồng Cơ sở dữ liệu và Lưu trữ (Database & Storage):**
- Mọi dữ liệu nghiệp vụ lõi được lưu trữ trên **Amazon RDS (PostgreSQL)**, triển khai theo mô hình **Multi-AZ (DB Replication)** trên nhiều Availability Zones để dự phòng thảm họa.
- Quá trình quản lý thay đổi cấu trúc DB được thực hiện bởi Flyway.
- Các file đính kèm được EC2 ghi vào S3 thông qua một đường dẫn nội bộ bảo mật (Internal Routing) bằng **VPC S3 Gateway Endpoint**. Người dùng sau đó có thể tải file thông qua Direct File Access hoặc CloudFront một cách an toàn.

**4. Luồng Thông báo và Quản lý dùng chung (Notifications & Shared Services):**
- Mọi sự kiện kích hoạt email (như mời tham gia nhóm) được EC2 gọi trực tiếp đến **Amazon SES** để gửi cho Users.
- Chứng chỉ SSL/TLS được quản lý bởi **AWS Certificate Manager (ACM)** kết hợp chặt chẽ việc phân quyền hệ thống qua **AWS IAM**.

### 3. Triển khai kỹ thuật
*Yêu cầu kỹ thuật*
- **Backend:** Java Spring Boot, Spring Security (JWT/OAuth2), Spring Data JPA.
- **Database:** PostgreSQL, Flyway (Database Migration).
- **Frontend:** ReactJS / TypeScript.
- **Các dịch vụ AWS sử dụng:** 
  - *Mạng & Bảo mật:* Amazon Route 53, Amazon CloudFront, AWS WAF, Amazon VPC (IGW, NAT Gateway, ALB, S3 Gateway Endpoint), AWS ACM, AWS IAM.
  - *Tính toán & Lưu trữ:* Amazon EC2, Amazon RDS (PostgreSQL Multi-AZ), Amazon S3 (FE Static & File Storage).
  - *Khác:* Amazon SES (Email Notification).

### 5. Lộ trình & Mốc triển khai
- *Giai đoạn 1:* Thiết kế kiến trúc AWS Cloud, cấu hình VPC, Subnets, Route 53 và IAM.
- *Giai đoạn 2:* Xây dựng Backend Spring Boot và database PostgreSQL (RDS Multi-AZ).
- *Giai đoạn 3:* Triển khai Backend lên EC2 với ALB, cấu hình Nat Gateway.
- *Giai đoạn 4:* Phát triển Frontend, upload lên S3 và cấu hình CloudFront + WAF.
- *Giai đoạn 5:* Tích hợp luồng S3 Endpoint để lưu trữ file và Amazon SES để gửi email.
- *Giai đoạn 6:* Kiểm thử End-to-end, rà soát bảo mật và bàn giao hệ thống.

### 6. Chi phí vận hành
Hệ thống Rookwork được triển khai trên nền tảng AWS theo tiêu chuẩn kiến trúc High Availability (Độ sẵn sàng cao) và bảo mật 3 lớp. Nền tảng sử dụng các dịch vụ cốt lõi bao gồm cụm máy chủ EC2 xử lý backend Spring Boot, cơ sở dữ liệu RDS PostgreSQL và S3 để lưu trữ frontend React 19 cùng tài nguyên tĩnh.

Trong giai đoạn hiện tại, do hệ thống chủ yếu phục vụ mục đích kiểm thử và báo cáo nghiệm thu trước hội đồng với lưu lượng truy cập nội bộ, tổng chi phí hạ tầng ước tính duy trì ở mức tối ưu khoảng **136 USD/tháng** (tương đương **3,48 triệu VND**). Mức ngân sách này đã bao gồm:
- Thiết lập dự phòng đa vùng **(Multi-AZ)** cho backend và database.
- Duy trì **NAT Gateway** để đảm bảo an toàn mạng (Private Subnet).
- Tích hợp **S3 Gateway Endpoint** giúp đưa chi phí băng thông nội bộ về 0.

Nếu cần cắt giảm thêm ngân sách trong giai đoạn đánh giá này, dự án hoàn toàn có thể linh hoạt chuyển đổi tạm thời về cấu hình **Single-AZ** hoặc thiết lập kịch bản tự động tắt/mở hệ thống ngoài giờ làm việc.

### 7. Đánh giá rủi ro
*Ma trận rủi ro*
- Lỗi thiết lập định tuyến (Routing) trong VPC khiến EC2 không kết nối được Internet hoặc DB: Ảnh hưởng cao, xác suất trung bình.
- Ngân sách AWS vượt quá mức do thiết lập NAT Gateway hoặc cấu hình Multi-AZ: Ảnh hưởng trung bình, xác suất cao (với tài khoản học tập).

*Chiến lược giảm thiểu*
- Cấu hình hạ tầng dưới dạng Code (IaC) để dễ bề kiểm soát.
- Đặt giới hạn chi phí (AWS Budgets) và cấu hình CloudWatch theo dõi lưu lượng mạng chặt chẽ. Đóng bớt DB Replication (Multi-AZ) trên môi trường Dev để tiết kiệm chi phí.