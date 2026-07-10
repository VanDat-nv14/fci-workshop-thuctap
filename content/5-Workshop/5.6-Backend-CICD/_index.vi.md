---
title: "Triển khai CI/CD Backend với GitHub Actions"
date: 2026-04-17
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

Trong bước này, chúng ta sẽ xây dựng đường ống dẫn luồng tự động hóa **CI/CD** sử dụng **GitHub Actions** để tự động đóng gói ứng dụng Spring Boot và triển khai tuần tự lên hai máy chủ EC2 (EC2-1 và EC2-2) chạy Docker thông qua **AWS Systems Manager (SSM)** và **Amazon S3**.

---

## 1. Cấu hình GitHub Repository Secrets

Để đảm bảo tính bảo mật cho thông tin nhạy cảm (như thông tin truy cập AWS, liên kết database, mật khẩu,...), chúng ta lưu trữ các giá trị này trong **GitHub Secrets** của Repository. Các biến này sẽ được gọi trực tiếp trong file pipeline YAML mà không bị lộ mã nguồn.

Đi tới **Settings → Secrets and variables → Actions → New repository secret** trên repo GitHub của bạn và cấu hình các biến sau:

![Cấu hình GitHub Repository Secrets](/images/5-Workshop/5.6/cicd-be-1.png)

### Chi tiết các biến Secrets cần thiết:

* **Biến truy cập AWS & S3:**
  * `AWS_ACCESS_KEY_ID` & `AWS_SECRET_ACCESS_KEY`: Quyền hạn AWS IAM của tài khoản CI/CD để đẩy file JAR lên S3 và gửi lệnh Run Command tới SSM.
  * `S3_BUCKET`: Tên S3 Bucket đóng vai trò lưu trữ trung gian các file build `.jar`.
* **Biến EC2 Instance ID:**
  * `EC2_INSTANCE_ID_1` & `EC2_INSTANCE_ID_2`: ID của 2 EC2 instances chạy ứng dụng backend trong Private Subnet.
* **Biến kết nối Cơ sở dữ liệu (RDS):**
  * `DB_URL`: Địa chỉ kết nối JDBC đến database RDS PostgreSQL.
  * `DB_USERNAME` & `DB_PASSWORD`: Tài khoản và mật khẩu đăng nhập database.
* **Biến cấu hình Ứng dụng (Rookwork):**
  * `JWT_SECRET`: Khóa ký mã hóa cho xác thực bảo mật JSON Web Token.
  * `GOOGLE_CLIENT_ID`: ID xác thực OAuth2 dành cho chức năng đăng nhập qua Google.
  * `AWS_S3_ACCESS_KEY_ID` & `AWS_S3_SECRET_ACCESS_KEY` & `AWS_S3_BUCKET`: Cấu hình S3 riêng phục vụ lưu trữ tệp tin upload của người dùng trong ứng dụng.
  * `APP_EMAIL_FROM`: Địa chỉ email hệ thống sử dụng để gửi thông báo.

---

## 2. Quy trình hoạt động của File cấu hình Pipeline (.github/workflows/deploy.yml)

Quy trình tự động hóa được thiết lập chi tiết qua các bước chính sau trong job `deploy`:

### Bước 1: Chuẩn bị mã nguồn và Build file JAR
- **Checkout code**: Kéo mã nguồn mới nhất từ nhánh GitHub về máy ảo chạy pipeline.
- **Set up JDK 21**: Cài đặt và cấu hình môi trường Java 21 (Temurin distribution).
- **Build JAR**: Thực thi lệnh `./mvnw clean package -DskipTests` để biên dịch mã nguồn và đóng gói thành tệp `.jar` (bỏ qua chạy thử nghiệm test để tối ưu thời gian build).

### Bước 2: Đẩy tệp build lên Amazon S3
- **Configure AWS credentials**: Cấu hình xác thực thông tin quyền truy cập AWS.
- **Set version tag**: Xác định tag phiên bản. Nếu trigger bởi tags của Git (dạng `v*`), hệ thống sẽ lấy tên tag làm phiên bản, ngược lại mặc định là `latest`.
- **Copy JAR to S3**: Đẩy tệp build `.jar` vừa tạo lên S3 Bucket trung gian bằng AWS CLI:
  ```bash
  aws s3 cp target/*.jar s3://${{ secrets.S3_BUCKET }}/rookwork-backend-${{ steps.version.outputs.VERSION }}.jar
  ```

### Bước 3: Triển khai tuần tự lên các EC2 instance qua AWS SSM

Hệ thống CI/CD sẽ triển khai lên **EC2-1 trước**, kiểm tra hoạt động thành công rồi mới tiếp tục triển khai lên **EC2-2**. Quy trình trên mỗi EC2 gồm các bước:

1. **Sinh tham số môi trường:** Sử dụng công cụ `jq` để tự động tạo tệp tham số `ssm-params-ec2.json` chứa các biến môi trường cấu hình lấy từ GitHub Secrets.
2. **Gửi lệnh thực thi qua AWS SSM Run Command:** Gửi yêu cầu chạy lệnh `AWS-RunShellScript` tới máy chủ mục tiêu bằng command:
  ```bash
  aws ssm send-command \
    --instance-ids "${{ secrets.EC2_INSTANCE_ID_1 }}" \
    --document-name "AWS-RunShellScript" \
    --parameters file://ssm-params-ec2-1.json
  ```
3. **Các câu lệnh tự động thực thi trực tiếp trên máy chủ EC2:**
   - Tải tệp `.jar` từ S3 về máy chủ.
   - Build Docker Image mới không dùng cache để cập nhật mã nguồn mới:
     ```bash
     sudo docker build --no-cache -t rookwork-backend:[VERSION] .
     ```
   - Dừng và xóa container backend cũ đang chạy.
   - Khởi chạy container Docker mới trên cổng `8080` kèm theo truyền toàn bộ các tham số cấu hình kết nối database, JWT và lưu trữ.
4. **Giám sát trạng thái triển khai (Polling):** Hệ thống CI/CD sẽ liên tục kiểm tra trạng thái thực thi của lệnh trên EC2 mỗi 15 giây (tối đa 40 lần ~ 10 phút). Nếu trạng thái trả về là `Success`, tiến trình chuyển sang EC2-2. Nếu thất bại (`Failed`, `Cancelled`, `TimedOut`), pipeline sẽ in lỗi ra log và dừng lại lập tức.

---

## 3. Mã cấu hình Pipeline chi tiết

Dưới đây là mã cấu hình hoàn chỉnh cho tệp tin `.github/workflows/deploy.yml`:

```yaml
name: Deploy to EC2

on:
  push:
    branches: [main]
    tags:
      - "v*"

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Set up JDK 21
        uses: actions/setup-java@v3
        with:
          java-version: "21"
          distribution: "temurin"

      - name: Build JAR
        run: ./mvnw clean package -DskipTests

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ap-southeast-1

      - name: Set version tag
        id: version
        run: |
          if [[ "${{ github.ref }}" == refs/tags/* ]]; then
            echo "VERSION=${{ github.ref_name }}" >> $GITHUB_OUTPUT
          else
            echo "VERSION=latest" >> $GITHUB_OUTPUT
          fi

      - name: Copy JAR to S3
        run: |
          aws s3 cp target/*.jar s3://${{ secrets.S3_BUCKET }}/rookwork-backend-${{ steps.version.outputs.VERSION }}.jar

      # ── Deploy EC2-1 trước, chờ hoàn thành ────────────────────────────────────
      - name: Deploy on EC2-1 via SSM
        run: |
          set -euo pipefail

          jq -n \
            --arg version "${{ steps.version.outputs.VERSION }}" \
            --arg s3_bucket "${{ secrets.S3_BUCKET }}" \
            --arg db_url "${{ secrets.DB_URL }}" \
            --arg db_user "${{ secrets.DB_USERNAME }}" \
            --arg db_pass "${{ secrets.DB_PASSWORD }}" \
            --arg jwt_secret "${{ secrets.JWT_SECRET }}" \
            --arg google_id "${{ secrets.GOOGLE_CLIENT_ID }}" \
            --arg aws_key "${{ secrets.AWS_S3_ACCESS_KEY_ID }}" \
            --arg aws_secret "${{ secrets.AWS_S3_SECRET_ACCESS_KEY }}" \
            --arg aws_bucket "${{ secrets.AWS_S3_BUCKET }}" \
            --arg email_from "${{ secrets.APP_EMAIL_FROM }}" \
            '
            [
              if $db_url != "" then "-e DB_URL=" + ($db_url | @sh) else empty end,
              if $db_user != "" then "-e DB_USERNAME=" + ($db_user | @sh) else empty end,
              if $db_pass != "" then "-e DB_PASSWORD=" + ($db_pass | @sh) else empty end,
              if $jwt_secret != "" then "-e JWT_SECRET=" + ($jwt_secret | @sh) else empty end,
              if $google_id != "" then "-e GOOGLE_CLIENT_ID=" + ($google_id | @sh) else empty end,
              if $aws_key != "" then "-e AWS_S3_ACCESS_KEY_ID=" + ($aws_key | @sh) else empty end,
              if $aws_secret != "" then "-e AWS_S3_SECRET_ACCESS_KEY=" + ($aws_secret | @sh) else empty end,
              if $aws_bucket != "" then "-e AWS_S3_BUCKET=" + ($aws_bucket | @sh) else empty end,
              if $email_from != "" then "-e APP_EMAIL_FROM=" + ($email_from | @sh) else empty end
            ] | join(" ") as $env_flags |
            ("aws s3 cp s3://" + $s3_bucket + "/rookwork-backend-" + $version + ".jar /home/ec2-user/rookwork-backend-sb/rookwork-backend.jar --no-progress && cd /home/ec2-user/rookwork-backend-sb && sudo docker build --no-cache -t rookwork-backend:" + $version + " . && { sudo docker rm -f rookwork-backend 2>/dev/null; sudo docker run -d --restart always --name rookwork-backend -p 8080:8080 " + $env_flags + " -e JWT_EXPIRATION=604800000 -e JWT_REFRESH_EXPIRATION=2592000000 -e AWS_REGION=ap-southeast-1 rookwork-backend:" + $version + "; }") as $cmd |
            { "commands": [$cmd] }
            ' > ssm-params-ec2-1.json

          COMMAND_ID=$(aws ssm send-command \
            --instance-ids "${{ secrets.EC2_INSTANCE_ID_1 }}" \
            --document-name "AWS-RunShellScript" \
            --region ap-southeast-1 \
            --parameters file://ssm-params-ec2-1.json \
            --query "Command.CommandId" \
            --output text)

          echo "EC2-1 SSM Command ID: $COMMAND_ID"

          # Polling tối đa 40 lần x 15s = 10 phút (đủ cho docker build)
          for i in $(seq 1 40); do
            STATUS=$(aws ssm get-command-invocation \
              --command-id "$COMMAND_ID" \
              --instance-id "${{ secrets.EC2_INSTANCE_ID_1 }}" \
              --query "Status" --output text 2>/dev/null || echo "Pending")
            echo "[EC2-1] [$i/40] Status: $STATUS"
            if [ "$STATUS" = "Success" ]; then
              echo "EC2-1 deploy SUCCESS"
              break
            elif [ "$STATUS" = "Failed" ] || [ "$STATUS" = "Cancelled" ] || [ "$STATUS" = "TimedOut" ]; then
              echo "EC2-1 deploy FAILED với status: $STATUS"
              echo "--- EC2-1 Error Output ---"
              aws ssm get-command-invocation \
                --command-id "$COMMAND_ID" \
                --instance-id "${{ secrets.EC2_INSTANCE_ID_1 }}" \
                --query "{stdout:StandardOutputContent,stderr:StandardErrorContent}" \
                --output json || true
              exit 1
            fi
            sleep 15
          done
          if [ "$STATUS" != "Success" ]; then
            echo "EC2-1 deploy timeout sau 10 phút"
            exit 1
          fi

      # ── Deploy EC2-2 sau khi EC2-1 thành công ─────────────────────────────────
      - name: Deploy on EC2-2 via SSM
        run: |
          set -euo pipefail

          jq -n \
            --arg version "${{ steps.version.outputs.VERSION }}" \
            --arg s3_bucket "${{ secrets.S3_BUCKET }}" \
            --arg db_url "${{ secrets.DB_URL }}" \
            --arg db_user "${{ secrets.DB_USERNAME }}" \
            --arg db_pass "${{ secrets.DB_PASSWORD }}" \
            --arg jwt_secret "${{ secrets.JWT_SECRET }}" \
            --arg google_id "${{ secrets.GOOGLE_CLIENT_ID }}" \
            --arg aws_key "${{ secrets.AWS_S3_ACCESS_KEY_ID }}" \
            --arg aws_secret "${{ secrets.AWS_S3_SECRET_ACCESS_KEY }}" \
            --arg aws_bucket "${{ secrets.AWS_S3_BUCKET }}" \
            --arg email_from "${{ secrets.APP_EMAIL_FROM }}" \
            '
            [
              if $db_url != "" then "-e DB_URL=" + ($db_url | @sh) else empty end,
              if $db_user != "" then "-e DB_USERNAME=" + ($db_user | @sh) else empty end,
              if $db_pass != "" then "-e DB_PASSWORD=" + ($db_pass | @sh) else empty end,
              if $jwt_secret != "" then "-e JWT_SECRET=" + ($jwt_secret | @sh) else empty end,
              if $google_id != "" then "-e GOOGLE_CLIENT_ID=" + ($google_id | @sh) else empty end,
              if $aws_key != "" then "-e AWS_S3_ACCESS_KEY_ID=" + ($aws_key | @sh) else empty end,
              if $aws_secret != "" then "-e AWS_S3_SECRET_ACCESS_KEY=" + ($aws_secret | @sh) else empty end,
              if $aws_bucket != "" then "-e AWS_S3_BUCKET=" + ($aws_bucket | @sh) else empty end,
              if $email_from != "" then "-e APP_EMAIL_FROM=" + ($email_from | @sh) else empty end
            ] | join(" ") as $env_flags |
            ("aws s3 cp s3://" + $s3_bucket + "/rookwork-backend-" + $version + ".jar /home/ec2-user/rookwork-backend-sb/rookwork-backend.jar --no-progress && cd /home/ec2-user/rookwork-backend-sb && sudo docker build --no-cache -t rookwork-backend:" + $version + " . && { sudo docker rm -f rookwork-backend 2>/dev/null; sudo docker run -d --restart always --name rookwork-backend -p 8080:8080 " + $env_flags + " -e JWT_EXPIRATION=604800000 -e JWT_REFRESH_EXPIRATION=2592000000 -e AWS_REGION=ap-southeast-1 rookwork-backend:" + $version + "; }") as $cmd |
            { "commands": [$cmd] }
            ' > ssm-params-ec2-2.json

          COMMAND_ID=$(aws ssm send-command \
            --instance-ids "${{ secrets.EC2_INSTANCE_ID_2 }}" \
            --document-name "AWS-RunShellScript" \
            --region ap-southeast-1 \
            --parameters file://ssm-params-ec2-2.json \
            --query "Command.CommandId" \
            --output text)

          echo "EC2-2 SSM Command ID: $COMMAND_ID"

          for i in $(seq 1 40); do
            STATUS=$(aws ssm get-command-invocation \
              --command-id "$COMMAND_ID" \
              --instance-id "${{ secrets.EC2_INSTANCE_ID_2 }}" \
              --query "Status" --output text 2>/dev/null || echo "Pending")
            echo "[EC2-2] [$i/40] Status: $STATUS"
            if [ "$STATUS" = "Success" ]; then
              echo "EC2-2 deploy SUCCESS"
              break
            elif [ "$STATUS" = "Failed" ] || [ "$STATUS" = "Cancelled" ] || [ "$STATUS" = "TimedOut" ]; then
              echo "EC2-2 deploy FAILED với status: $STATUS"
              echo "--- EC2-2 Error Output ---"
              aws ssm get-command-invocation \
                --command-id "$COMMAND_ID" \
                --instance-id "${{ secrets.EC2_INSTANCE_ID_2 }}" \
                --query "{stdout:StandardOutputContent,stderr:StandardErrorContent}" \
                --output json || true
              exit 1
            fi
            sleep 15
          done
          if [ "$STATUS" != "Success" ]; then
            echo "EC2-2 deploy timeout sau 10 phút"
            exit 1
          fi
```

> [!TIP]
> Tiến trình deploy tuần tự (Rolling Update) đảm bảo hệ thống luôn hoạt động liên tục (Zero Downtime) khi cập nhật phiên bản mới của backend.
