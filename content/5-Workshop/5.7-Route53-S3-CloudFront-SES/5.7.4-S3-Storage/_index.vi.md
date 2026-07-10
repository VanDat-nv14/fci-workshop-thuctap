---
title: "Lưu trữ tệp tin riêng tư với Amazon S3"
menuTitle: "Lưu trữ tệp tin riêng tư với Amazon S3"
date: 2026-05-01
weight: 4
chapter: false
pre: " <b> 5.7.4. </b> "
---

Ngoài việc lưu trữ mã nguồn tĩnh của Frontend, dự án Rookwork cần một không gian lưu trữ riêng tư, bảo mật trên đám mây để lưu tài liệu đính kèm của dự án, các file tải lên và ảnh đại diện (avatar) của thành viên. 

Chúng ta sử dụng **Amazon S3** kết hợp cơ chế **Presigned URL** để đảm bảo dữ liệu luôn được lưu trữ an toàn nhất.

### Bước 1: Tạo Amazon S3 Bucket cho Storage
1. Truy cập dịch vụ **Amazon S3** trên AWS Management Console.
2. Click **Create bucket**.
3. Cấu hình chi tiết:
   * **Bucket name:** Nhập tên duy nhất toàn cầu (ví dụ: `rookwork-attachments-bucket`).
   * **AWS Region:** Chọn cùng Region với máy chủ EC2 và RDS (ví dụ: `ap-southeast-1` Singapore).
   * **Block Public Access settings:** Chọn **Block all public access** (Bắt buộc phải chặn toàn bộ truy cập công khai để đảm bảo tệp đính kèm và ảnh đại diện không bị rò rỉ ra bên ngoài).
   ![Cấu hình tạo S3 Bucket lưu trữ riêng tư](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/S3-Storage-1.png)
4. Để mặc định các cấu hình còn lại và click **Create bucket**.
   ![S3 Bucket lưu trữ được tạo thành công](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/S3-Storage-2.png)

### Bước 2: Cấu hình Spring Boot Backend
Trong mã nguồn backend (`rookwork-backend-sb`), chúng ta nạp cấu hình Spring Cloud AWS và S3 thông qua các biến môi trường của hệ thống (file `.env` hoặc cấu hình biến môi trường trên máy chủ EC2 production):

```properties
# AWS S3 Production Configuration
spring.cloud.aws.s3.enabled=true
spring.cloud.aws.region.static=${AWS_REGION:ap-southeast-1}
spring.cloud.aws.credentials.access-key=${AWS_S3_ACCESS_KEY_ID}
spring.cloud.aws.credentials.secret-key=${AWS_S3_SECRET_ACCESS_KEY}
aws.s3.bucket-name=${AWS_S3_BUCKET:rookwork-attachments-bucket}
```

### Bước 3: Cài đặt S3 Service trong Java sử dụng AWS SDK v2
Trong ứng dụng Rookwork backend (`rookwork-backend-sb`), chúng ta sử dụng `S3Template` (từ thư viện Spring Cloud AWS) để hỗ trợ tải lên và xóa tệp tin, cùng với `S3Presigner` để sinh mã link tạm thời (Presigned URL) có hiệu lực 15 phút phục vụ việc xem và tải xuống an toàn:

```java
package com.example.rookwork_backend_sb.services;

import io.awspring.cloud.s3.S3Template;
import lombok.RequiredArgsConstructor;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Service;
import org.springframework.web.multipart.MultipartFile;
import software.amazon.awssdk.services.s3.model.GetObjectRequest;
import software.amazon.awssdk.services.s3.presigner.S3Presigner;
import software.amazon.awssdk.services.s3.presigner.model.GetObjectPresignRequest;
import software.amazon.awssdk.services.s3.presigner.model.PresignedGetObjectRequest;

import java.io.IOException;
import java.io.InputStream;
import java.time.Duration;
import java.util.UUID;

@Service
@RequiredArgsConstructor
public class S3Service {

    private final S3Template s3Template;
    private final S3Presigner s3Presigner;

    @Value("${aws.s3.bucket-name}")
    private String bucketName;

    /**
     * Uploads a file to AWS S3 and returns the generated stored name.
     */
    public String uploadFile(MultipartFile file, UUID projectId, UUID issueId) throws IOException {
        String fileExtension = getFileExtension(file.getOriginalFilename());

        // Generate random unique name
        String fileUuid = UUID.randomUUID().toString();
        // Construct directory structure key: projects/{projectId}/issues/{issueId}/{uuid}{ext}
        String storedName = String.format("projects/%s/issues/%s/%s%s", 
                projectId, 
                issueId, 
                fileUuid, 
                fileExtension);

        try (InputStream inputStream = file.getInputStream()) {
            s3Template.upload(bucketName, storedName, inputStream);
        }
        return storedName;
    }

    /**
     * Deletes a file from AWS S3.
     */
    public void deleteFile(String storedName) {
        s3Template.deleteObject(bucketName, storedName);
    }

    /**
     * Generates a temporary Presigned URL for viewing/downloading files.
     * Valid for 15 minutes.
     */
    public String generatePresignedUrl(String storedName) {
        if (storedName == null || storedName.isEmpty()) {
            return null;
        }

        GetObjectRequest getObjectRequest = GetObjectRequest.builder()
                .bucket(bucketName)
                .key(storedName)
                .build();

        GetObjectPresignRequest getObjectPresignRequest = GetObjectPresignRequest.builder()
                .signatureDuration(Duration.ofMinutes(15))
                .getObjectRequest(getObjectRequest)
                .build();

        PresignedGetObjectRequest presignedGetObjectRequest = s3Presigner.presignGetObject(getObjectPresignRequest);
        return presignedGetObjectRequest.url().toString();
    }

    /**
     * Uploads a user avatar to S3 under the "avatar" directory.
     */
    public String uploadAvatar(MultipartFile file, UUID userId) throws IOException {
        String fileExtension = getFileExtension(file.getOriginalFilename());

        // Key format: avatar/{userId}/{fileUuid}{ext}
        String fileUuid = UUID.randomUUID().toString();
        String storedName = String.format("avatar/%s/%s%s", userId.toString(), fileUuid, fileExtension);

        try (InputStream inputStream = file.getInputStream()) {
            s3Template.upload(bucketName, storedName, inputStream);
        }
        return storedName;
    }

    /**
     * Helper to get presigned URL for avatar if it is stored on S3.
     * If not stored on S3 (e.g. Google URL or null), returns the original value.
     */
    public String getAvatarUrl(String picture) {
        if (picture != null && picture.startsWith("avatar/")) {
            return generatePresignedUrl(picture);
        }
        return picture;
    }

    private String getFileExtension(String filename) {
        if (filename != null && filename.contains(".")) {
            return filename.substring(filename.lastIndexOf("."));
        }
        return "";
    }
}
```

---

Sau khi đã cấu hình lưu trữ tệp đính kèm thành công, hãy tiếp tục với **[5.7.5. Tích hợp dịch vụ gửi Email với Amazon SES](../5.7.5-ses/)** để cấu hình dịch vụ gửi email OTP/thông báo.
