---
title: "Triển khai CI/CD Frontend với GitHub Actions"
menuTitle: "Triển khai CI/CD Frontend với GitHub Actions"
date: 2026-05-01
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---
---

Để tối ưu hóa quy trình bàn giao sản phẩm (delivery pipeline), chúng ta thiết lập quy trình tích hợp và triển khai liên tục (CI/CD) bằng **GitHub Actions**. 

Mỗi khi lập trình viên đẩy mã nguồn mới lên nhánh chính (`main`), hệ thống sẽ tự động thực hiện cài đặt thư viện phụ thuộc bằng **Yarn**, build ứng dụng tĩnh React Frontend với phiên bản **Node.js 22**, sau đó đồng bộ hóa các tệp tin tài nguyên tĩnh lên S3 Bucket và dọn dẹp bộ nhớ đệm (cache invalidation) của CloudFront CDN.

---

### Luồng Hoạt động CI/CD

```text
Code Push (main) ──► GitHub Actions Runner ──► Build Frontend (Yarn)
                                                       │
                                                       ▼
Invalidate CDN Cache ◄── CloudFront ◄── Sync dist/ to S3 Bucket
```

---

### Bước 1: Khởi tạo IAM User có quyền CI/CD trên AWS

Để bảo mật, chúng ta không nên dùng tài khoản Root. Hãy tạo một **IAM User** chuyên dụng cho CI/CD (ví dụ: `github-actions-deployer`) và gán các quyền tối thiểu sau:
1.  **AmazonS3FullAccess** (hoặc giới hạn quyền đọc/ghi trên riêng S3 Bucket lưu Frontend).
2.  **CloudFrontFullAccess** (hoặc giới hạn quyền tạo invalidation cho riêng Distribution ID).

Sau khi tạo, lấy thông tin **Access Key ID** và **Secret Access Key** để cấu hình vào GitHub.

---

### Bước 2: Cấu hình GitHub Secrets

1.  Truy cập vào Repository chứa mã nguồn Frontend của bạn trên GitHub.
2.  Đi tới phần **Settings -> Secrets and variables -> Actions**.
    ![Truy cập Settings và Secrets Actions trên GitHub](/images/5-Workshop/5.8-CICD-frontend/1.png)
3.  Click **New repository secret** và thêm vào các biến môi trường bảo mật sau:
    *   `AWS_ACCESS_KEY_ID`: ID khóa truy cập của IAM User vừa tạo.
    *   `AWS_SECRET_ACCESS_KEY`: Khóa bảo mật tương ứng.
    *   `S3_BUCKET`: Tên của S3 Bucket chứa tĩnh (ví dụ: `rookwork.asia`).
    *   `CF_DISTRIBUTION_ID`: ID phân phối của CloudFront để kích hoạt làm mới cache.
    *   `VITE_GOOGLE_CLIENT_ID`: ID Client xác thực của Google (để nạp vào ứng dụng lúc build).
    ![Khai báo đầy đủ các biến Repository Secrets](/images/5-Workshop/5.8-CICD-frontend/2.png)

---

### Bước 3: Tạo File Workflow trong Mã nguồn

Trong thư mục gốc của dự án Frontend, tạo cấu trúc thư mục `.github/workflows/` và tạo file cấu hình triển khai `.github/workflows/deploy.yml` với nội dung sau:

```yaml
name: Deploy Frontend to S3

on:
  push:
    branches: [main]    # ← CHỈ nhánh main mới trigger

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 22
          cache: yarn

      - name: Install dependencies
        run: yarn install --frozen-lockfile

      - name: Build browser app
        run: yarn build:browser
        env:
          VITE_API_URL: https://api.rookwork.asia
          VITE_GOOGLE_CLIENT_ID: ${{ secrets.VITE_GOOGLE_CLIENT_ID }}

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ap-southeast-1

      - name: Sync assets to S3 (cache forever)
        run: |
          aws s3 sync ./browser-app/dist s3://${{ secrets.S3_BUCKET }} \
            --delete \
            --exclude "index.html" \
            --cache-control "public,max-age=31536000,immutable"

      - name: Upload index.html (no cache)
        run: |
          aws s3 cp ./browser-app/dist/index.html s3://${{ secrets.S3_BUCKET }}/index.html \
            --cache-control "no-cache,no-store,must-revalidate"

      - name: Invalidate CloudFront cache
        run: |
          aws cloudfront create-invalidation \
            --distribution-id ${{ secrets.CF_DISTRIBUTION_ID }} \
            --paths "/*"
```

---

### Bước 4: Kiểm thử Quy trình

1.  Thực hiện thay đổi nhỏ trên giao diện frontend dưới máy local (ví dụ: sửa đổi chữ hoặc màu sắc trên trang Home).
2.  Chạy các lệnh Git để đẩy code lên GitHub:
    ```bash
    git add .
    git commit -m "feat: update home layout & trigger cicd"
    git push origin main
    ```
3.  Mở tab **Actions** trên repo GitHub của bạn, bạn sẽ thấy tiến trình pipeline đang tự động chạy.
    ![Theo dõi tiến trình pipeline chạy tự động](/images/5-Workshop/5.8-CICD-frontend/3.png)
4.  Khi tất cả các bước hiển thị màu xanh lá (thành công), truy cập vào tên miền tùy chỉnh của bạn (ví dụ: `https://rookwork.asia`), giao diện mới đã được cập nhật hoàn tất mà không cần bất cứ thao tác deploy thủ công nào!
    ![Quy trình deploy hoàn tất thành công và hiển thị các tick xanh](/images/5-Workshop/5.8-CICD-frontend/4.png)


