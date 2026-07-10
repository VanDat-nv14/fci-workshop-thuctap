---
title: "Tích hợp dịch vụ gửi Email với Amazon SES"
menuTitle: "Tích hợp dịch vụ gửi Email với Amazon SES"
date: 2026-05-01
weight: 5
chapter: false
pre: " <b> 5.7.5. </b> "
---

Amazon SES là dịch vụ đám mây giúp gửi email với hiệu suất cao và chi phí thấp, tối ưu cho việc gửi thông báo giao dịch trong dự án Rookwork.

### Bước 1: Xác thực danh tính trên Amazon SES (Identities)
1. Tìm kiếm dịch vụ **Amazon Simple Email Service (SES)** trên AWS Console.
2. Trong mục **Configuration**, chọn **Identities**, sau đó click **Create identity**.
   ![Truy cập SES và click Create identity](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/SES-1.png)
3. Chọn loại **Domain** (Khuyên dùng khi bạn đã có tên miền riêng):
   * **Domain:** Nhập tên miền của bạn (ví dụ: `rookwork.asia`).
   * **Easy DKIM:** Chọn Kích hoạt và để cài đặt mặc định để xác thực danh tính đáng tin cậy.
4. Click **Create identity**.
   ![Điền tên miền và cấu hình DKIM](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/SES-2.png)
5. Hệ thống sẽ cung cấp các bản ghi CNAME phục vụ cấu hình DKIM. Hãy click vào nút **Create records in Route 53** để Route 53 tự động thêm các bản ghi DNS này vào Hosted Zone.
   ![Click Create records in Route 53](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/SES-3.png)
6. *(Lưu ý)* Khi tài khoản mới tạo, SES sẽ nằm trong môi trường **Sandbox**. Bạn cần tạo thêm một Identity có loại **Email address** (ví dụ: email cá nhân của bạn) để làm địa chỉ nhận thư kiểm thử trong quá trình dev.
   ![Xác thực email nhận kiểm thử trong SES Sandbox](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/SES-4.png)

### Bước 2: Tạo SMTP Credentials cho ứng dụng Spring Boot
1. Trong menu bên trái của SES, chọn **SMTP settings**.
2. Click **Create SMTP credentials**.
3. Đặt tên cho tài khoản IAM và nhấn Create.
4. Sao chép và lưu lại **SMTP Username** và **SMTP Password** hiển thị trên màn hình.

### Bước 3: Cấu hình ứng dụng Spring Boot gửi email
Trong ứng dụng Rookwork backend (`rookwork-backend-sb`), chúng ta nạp cấu hình SMTP bảo mật từ các biến môi trường của hệ thống (file `.env`) và kết nối Spring Mail:

```properties
spring.mail.host=email-smtp.ap-southeast-1.amazonaws.com
spring.mail.port=587
spring.mail.username=${SMTP_USERNAME}
spring.mail.password=${SMTP_PASSWORD}
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true
```

Sử dụng `JavaMailSender` để viết service gửi email OTP xác thực đăng ký tài khoản, khôi phục mật khẩu và gửi link mời tham gia dự án cộng tác.

![Mã nguồn cài đặt JavaMailSender gửi email OTP](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/SES-code.png)

---

Sau khi hoàn tất toàn bộ các dịch vụ AWS Core, bạn đã sẵn sàng chuyển sang phần **[5.8. Triển khai CI/CD Frontend với GitHub Actions](../../5.8-cicd-frontend/)** để hoàn tất chu trình tự động hóa triển khai.
