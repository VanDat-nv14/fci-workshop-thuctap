---
title: "Cấu hình Tên miền & AWS Route 53"
menuTitle: "Cấu hình Tên miền & AWS Route 53"
date: 2026-05-01
weight: 1
chapter: false
pre: " <b> 5.7.1. </b> "
---

Để AWS Route 53 quản lý các bản ghi DNS của tên miền tùy chỉnh mua từ Tenten.vn, chúng ta cần cấu hình chuyển quyền phân giải tên miền (DNS Delegation).

### Bước 1: Tạo Hosted Zone trên AWS Route 53
1. Đăng nhập vào **AWS Management Console** và tìm kiếm dịch vụ **Route 53**.
2. Trong thanh điều hướng bên trái, chọn **Hosted zones**, sau đó click **Create hosted zone**.
   ![Tìm kiếm và truy cập Hosted Zones](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/Route53-1.png)
   ![Nhấn nút Create hosted zone](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/Route53-2.png)
3. Tại phần cấu hình:
   * **Domain name:** Nhập tên miền chính xác đã mua (ví dụ: `rookwork.asia`).
   * **Type:** Chọn **Public hosted zone**.
   ![Cấu hình tên miền cho Hosted Zone](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/Route53-3.png)
4. Click **Create hosted zone**. Hệ thống sẽ tự động tạo ra 2 bản ghi: **NS** (Name Server) và **SOA** (Start of Authority).
   ![Hosted Zone được khởi tạo thành công](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/Route53-4.png)

### Bước 2: Lấy thông tin Name Servers của Route 53
1. Click vào Hosted Zone vừa tạo.
2. Tìm bản ghi có loại (**Type**) là **NS**.
3. Sao chép **4 địa chỉ máy chủ tên miền** trong cột Value (ví dụ: `ns-xxxx.awsdns-xx.com.`, lưu ý bỏ dấu chấm cuối cùng khi cấu hình trên các DNS manager).

### Bước 3: Cấu hình máy chủ tên miền trên Tenten.vn
1. Đăng nhập vào trang quản lý dịch vụ khách hàng của **Tenten.vn**.
2. Truy cập vào danh sách tên miền của bạn và chọn mục **Cấu hình Name Server (NS)** cho tên miền tương ứng.
   ![Danh sách tên miền trên Tenten](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/TenTen-1.png)
   ![Chọn mục cấu hình Name Server](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/TenTen-2.png)
3. Thay thế các máy chủ tên miền mặc định của Tenten bằng **4 địa chỉ NS** vừa sao chép từ Route 53 ở Bước 2.
   ![Điền 4 địa chỉ NS từ Route 53](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/TenTen-3.png)
4. Lưu lại cấu hình. 
   ![Lưu cấu hình NS thành công](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/TenTen-4.png)

*(Lưu ý: Quá trình đồng bộ DNS trên toàn cầu có thể mất từ vài phút đến tối đa 24 giờ để cập nhật hoàn toàn).*

---

Sau khi đã cấu hình chuyển giao DNS thành công, hãy tiếp tục với **[5.7.2. Lưu trữ Web tĩnh với Amazon S3](../5.7.2-s3-static-hosting/)** để chuẩn bị lưu trữ mã nguồn Frontend.
