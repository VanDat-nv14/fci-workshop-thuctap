---
title: "Lưu trữ Web tĩnh với Amazon S3"
menuTitle: "Lưu trữ Web tĩnh với Amazon S3"
date: 2026-05-01
weight: 2
chapter: false
pre: " <b> 5.7.2. </b> "
---

Amazon S3 là giải pháp lưu trữ lý tưởng cho các tệp tĩnh của Frontend React App nhờ độ tin cậy cao và chi phí cực thấp.

### Bước 1: Tạo Amazon S3 Bucket
1. Mở dịch vụ **Amazon S3** trên AWS Console.
   ![Tìm kiếm và truy cập S3](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/S3-1.png)
2. Click **Create bucket**.
   ![Nhấn nút Create bucket](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/S3-2.png)
3. Cấu hình chi tiết:
   * **Bucket name:** Nhập tên miền của bạn (ví dụ: `rookwork.asia` hoặc `www.rookwork.asia`) để dễ quản lý.
     ![Cấu hình tên Bucket](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/S3-3.png)
   * **AWS Region:** Chọn Region gần người dùng nhất (ví dụ: `ap-southeast-1` Singapore).
     ![Chọn Region](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/S3-4.png)
   * **Block Public Access settings:** Chọn **Block all public access** (Chúng ta sẽ giữ bucket ở chế độ riêng tư và chỉ cho phép CloudFront OAC truy cập để tăng bảo mật).
     ![Bật Block All Public Access](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/S3-5.png)
     ![Cấu hình mã hóa dữ liệu tĩnh](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/S3-6.png)
4. Click **Create bucket**.
   ![Xác nhận cấu hình tạo Bucket](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/S3-7.png)
   ![S3 Bucket được tạo thành công](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/S3-8.png)

### Bước 2: Upload mã nguồn Frontend lên S3
1. Mở bucket vừa tạo.
2. Click **Upload**, sau đó click **Add folder** hoặc **Add files**.
3. Chọn toàn bộ các thư mục và file nằm bên trong thư mục `dist/` (sau khi build React dự án ở local) và tải lên. (Đảm bảo file `index.html` nằm ở thư mục gốc của bucket).
4. Click **Upload** để hoàn tất quá trình tải tệp tin lên S3.

---

Sau khi đã tải mã nguồn Frontend lên S3 thành công, hãy tiếp tục với **[5.7.3. Phân phối bảo mật qua Amazon CloudFront & ACM](../5.7.3-cloudfront-acm/)** để cấu hình phân phối HTTPS và CDN.
