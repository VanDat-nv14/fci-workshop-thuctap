---
title: "Khởi tạo Cơ sở dữ liệu RDS"
date: 2026-07-08
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---


Trong phần này, bạn sẽ thực hiện khởi tạo cơ sở dữ liệu **Amazon RDS PostgreSQL** kết nối trực tiếp với máy chủ EC2 trong mạng nội bộ VPC để lưu trữ dữ liệu cho dự án Rookwork.

---

{{% notice note %}}
Trước khi bắt đầu, hãy đảm bảo rằng bạn đã khởi tạo các Security Group cần thiết. Bạn có thể xem chi tiết hướng dẫn tại [Phần 3 – Thiết lập Security Groups (Bước 1)](../5.3-s3-vpc/#bước-1-tạo-security-group-cho-alb-rookwork-alb-sg) của bài viết 5.3.
{{% /notice %}}

---

### Bước 1: Khởi tạo Amazon RDS PostgreSQL

Sau khi đã chuẩn bị xong mạng và máy chủ, chúng ta bắt đầu tạo cơ sở dữ liệu quan hệ RDS:

1. Tìm kiếm và mở dịch vụ **RDS** trên AWS Management Console.
2. Chọn **Databases** ở cột bên trái và nhấn nút **Create database**.
3. Tại phần **Engine options**, lựa chọn loại cơ sở dữ liệu **PostgreSQL**.
4. Tại mục **Choose a database creation method**, chọn phương thức **Easy create** để hệ thống tự động thiết lập các cài đặt khuyên dùng tốt nhất.

![Khởi tạo RDS - Bước 1](/images/5-Workshop/5.5/create-rds-step1.png)

5. Tại mục cấu hình DB instance size, lựa chọn **Free tier** để sử dụng cấu hình miễn phí (`db.t4g.micro`, 2 vCPUs, 1 GiB RAM, 20 GiB Storage).
6. **DB instance identifier**: Nhập tên định danh cơ sở dữ liệu của bạn, ví dụ: `rookwork-db`.
7. **Master username**: Mặc định là `postgres`.
8. **Credentials management**: Chọn **Self managed** để nhập mật khẩu quản trị tùy chọn và tự quản lý mật khẩu kết nối.

![Khởi tạo RDS - Bước 2](/images/5-Workshop/5.5/create-rds-step2.png)

9. Tại phần **Set up EC2 connection - optional** (Cấu hình kết nối với máy chủ EC2):
   * Lựa chọn **Connect to an EC2 compute resource**.
   * Tại danh sách **EC2 instance**, chọn máy chủ EC2 của bạn (ví dụ: `rookwork-ec2-1` / `i-035c5645906cef43f`).
   * *Lưu ý: Thiết lập này giúp Amazon RDS tự động cấu hình các nhóm bảo mật thích hợp (`rds-ec2-X` cho DB và `ec2-rds-X` cho máy chủ EC2), đảm bảo luồng traffic giữa EC2 và RDS thông suốt một cách an toàn mà không cần cấu hình thủ công cổng Database.*

![Khởi tạo RDS - Bước 3](/images/5-Workshop/5.5/create-rds-step3.png)

10. Nhấp chọn **Create database** để tiến hành khởi tạo DB. Tiến trình này sẽ chạy trong vài phút cho đến khi trạng thái chuyển sang **Available**.

---

### Bước 2: Cấu hình Spring Boot để trỏ tới RDS

Để ứng dụng Spring Boot chạy trên EC2 kết nối tới cơ sở dữ liệu Amazon RDS PostgreSQL, ta tiến hành cập nhật tệp tin cấu hình dự án:

1. Mở tệp tin cấu hình `src/main/resources/application.properties` (hoặc `application.yml`) trong mã nguồn dự án của bạn.
2. Thêm hoặc cập nhật các thuộc tính cấu hình kết nối database như sau:

```properties
spring.datasource.url=jdbc:postgresql://rookwork-db.cxegees2u15z.ap-southeast-1.rds.amazonaws.com:5432/postgres
spring.datasource.username=postgres
spring.datasource.password=your_password_here
spring.datasource.driver-class-name=org.postgresql.Driver

spring.jpa.hibernate.ddl-auto=update
spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect
spring.jpa.show-sql=true
```

{{% notice info %}}
**Lưu ý:** Cấu hình trên chỉ mang tính chất thiết lập tạm thời để kiểm tra kết nối cục bộ. Về sau, chúng ta sẽ tối ưu hóa cấu hình này thông qua việc sử dụng các biến môi trường (Environment Variables) trong quy trình CI/CD để đảm bảo tính bảo mật của thông tin kết nối (credentials).
{{% /notice %}}

---

### Bước 3: Kiểm tra kết nối từ EC2 đến RDS PostgreSQL

Sau khi cơ sở dữ liệu RDS chuyển sang trạng thái **Available**, thực hiện các bước sau từ máy chủ EC2 để kiểm tra kết nối:

1. Thực hiện kết nối vào máy chủ EC2 (sử dụng [**SSM Session Manager**](../5.4-Create-EC2-Private-Env/#bước-5-kết-nối-và-kiểm-tra-môi-trường-trong-ec2-private) tương tự hướng dẫn ở Bước 5 của bài trước).
2. Chạy lệnh cài đặt công cụ máy khách `psql` (PostgreSQL Client) trên máy chủ EC2:
   ```bash
   sudo dnf install -y postgresql15
   ```
3. Tiến hành kết nối tới RDS PostgreSQL bằng lệnh sau:
   ```bash
   psql -h <RDS_ENDPOINT> -U postgres -d postgres
   ```
   *(Thay thế `<RDS_ENDPOINT>` bằng địa chỉ Endpoint thực tế của RDS instance tìm thấy trên trang chi tiết AWS RDS Console)*
4. Nhập mật khẩu quản trị mà bạn đã thiết lập ở Bước 2. Khi dấu nhắc lệnh chuyển thành `postgres=>`, điều này chứng tỏ máy chủ EC2 đã kết nối thành công tới RDS PostgreSQL thông qua mạng nội bộ VPC.

![Kết nối RDS thành công](/images/5-Workshop/5.5/rds-connect-completed.png)

{{% notice note %}}
**Lưu ý:** Ảnh trên minh họa giao diện phiên kết nối terminal thành công. Việc kiểm tra và cài đặt này đảm bảo ứng dụng chạy trên EC2 của bạn có thể giao tiếp và lưu trữ dữ liệu an toàn vào cơ sở dữ liệu RDS PostgreSQL.
{{% /notice %}}
