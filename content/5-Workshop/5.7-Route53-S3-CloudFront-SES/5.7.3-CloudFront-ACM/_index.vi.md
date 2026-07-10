---
title: "Phân phối bảo mật qua Amazon CloudFront & ACM"
menuTitle: "Phân phối bảo mật qua Amazon CloudFront & ACM"
date: 2026-05-01
weight: 3
chapter: false
pre: " <b> 5.7.3. </b> "
---

Để kích hoạt HTTPS bảo mật cho website và tăng tốc tải trang toàn cầu, chúng ta cấu hình AWS Certificate Manager (ACM) kết hợp với CloudFront.

### Bước 1: Yêu cầu chứng chỉ SSL/TLS qua ACM
1. Tìm kiếm dịch vụ **Certificate Manager** trên AWS Console.
2. *(Quan trọng)* Chuyển vùng sang **us-east-1 (N. Virginia)** (CloudFront yêu cầu chứng chỉ SSL/TLS phải nằm ở Region này để hoạt động).
   ![Chuyển Region của ACM sang us-east-1](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/ACM-1.png)
3. Click **Request certificate**, chọn **Request a public certificate** và click Next.
   ![Yêu cầu chứng chỉ Public SSL](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/ACM-2.png)
4. Tại phần cấu hình:
   * **Fully qualified domain name:** Nhập tên miền của bạn (ví dụ: `rookwork.asia` và một tên miền phụ bổ sung `*.rookwork.asia`).
   * **Validation method:** Chọn **DNS validation**.
5. Click **Request**.
   ![Điền tên miền và chọn DNS Validation](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/ACM-3.png)
6. Trạng thái ban đầu sẽ là *Pending validation*. Click vào Certificate ID vừa tạo và nhấn nút **Create records in Route 53** để tự động tạo bản ghi xác thực DNS trên Route 53.
   ![Tạo bản ghi xác thực DNS trên Route 53](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/ACM-4.png)
7. Đợi khoảng vài phút để chứng chỉ được đổi trạng thái thành **Issued**.
   ![Chứng chỉ ACM được cấp thành công](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/ACM-5.png)

### Bước 2: Tạo CloudFront Distribution
1. Tìm kiếm dịch vụ **CloudFront** trên AWS Console.
2. Click **Create distribution**.
   ![Truy cập CloudFront và click Create distribution](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/Cloudfront-1.png)
3. Tại phần cấu hình:
   * **Origin domain:** Chọn tên S3 bucket của bạn.
     ![Chọn S3 Bucket làm Origin](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/Cloudfront-2.png)
   * **Origin access:** Chọn **Origin access control settings (OAC)** (khuyên dùng). Click **Create control setting**, để mặc định cấu hình và lưu lại.
     ![Cấu hình Origin Access Control (OAC)](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/Cloudfront-3.png)
   * **Viewer protocol policy:** Chọn **Redirect HTTP to HTTPS**.
     ![Chọn Redirect HTTP to HTTPS](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/Cloudfront-4.png)
   * **Alternate domain name (CNAME):** Thêm tên miền của bạn (ví dụ: `rookwork.asia` và `www.rookwork.asia`).
   * **Custom SSL certificate:** Chọn chứng chỉ ACM vừa được cấp ở Bước 1.
     ![Thêm CNAME và chứng chỉ ACM SSL](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/Cloudfront-5.png)
   * **Default root object:** Nhập `index.html`.
     ![Cấu hình Default Root Object là index.html](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/Cloudfront-6.png)
4. Click **Create distribution**.
   ![CloudFront Distribution khởi tạo thành công](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/Cloudfront-7.png)
5. Sao chép đoạn **Bucket Policy** gợi ý từ giao diện của CloudFront và cập nhật vào mục **Permissions -> Bucket Policy** trong S3 Bucket của bạn để cấp quyền cho CloudFront truy xuất file.
   ![Mở phần Permissions trên S3 Bucket để sửa Policy](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/S3-policy-1.png)
   ![Nhấp Edit S3 Bucket Policy](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/S3-policy-2.png)
   ![Dán đoạn Policy JSON copy từ CloudFront](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/S3-policy-3.png)
   ![Nhấp Save changes để lưu lại](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/S3-policy-4.png)
   ![Bucket Policy được cập nhật thành công](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/S3-policy-5.png)

### Bước 3: Tạo bản ghi DNS trên Route 53 trỏ về CloudFront
1. Quay lại dịch vụ **Route 53** và mở Hosted Zone của tên miền.
2. Click **Create record**.
   ![Mở Hosted Zone và click Create record](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/Route53-CF-1.png)
3. Cấu hình bản ghi:
   * **Record name:** Để trống (đối với tên miền gốc `rookwork.asia`).
   * **Record type:** Chọn **A - Routes traffic to an IPv4 address and some AWS resources**.
   * **Alias:** Kích hoạt (gạt nút bật sang phải).
   * **Route traffic to:** Chọn **Alias to CloudFront distribution** -> Chọn phân phối CloudFront vừa tạo.
   ![Cấu hình bản ghi A Alias trỏ về CloudFront](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/Route53-CF-2.png)
4. Click **Create records**.
   ![Bản ghi A DNS được Route 53 tạo thành công](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/Route53-CF-3.png)

Bây giờ bạn đã có thể truy cập website của bạn một cách bảo mật bằng giao thức HTTPS thông qua tên miền tùy chỉnh!

---

Sau khi website tĩnh đã được cấu hình hoạt động thành công qua HTTPS, hãy tiếp tục với **[5.7.4. Lưu trữ tệp tin riêng tư với Amazon S3](../5.7.4-s3-storage/)** để chuẩn bị không gian lưu trữ cho phần Backend.
