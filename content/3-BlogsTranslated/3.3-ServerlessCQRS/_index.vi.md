---
title: "Blog 3"
date: 2026-07-04
weight: 3
chapter: false
pre: "<b> 3.3. </b>"
---

# Đừng mãi chọn Single-Purpose hay Lambda-lith: Lối đi giữa Read-Write Separation (CQRS Prelude) có gì hay?

Chào anh em. Hôm nay mình vừa đọc được một bài phân tích rất sâu trên AWS Compute Blog của hai anh Principal Solutions Architect về chiến lược thiết kế Serverless Microservices. Thấy bài viết giải quyết đúng pain point mà anh em làm Serverless hay gặp phải khi phân vân cấu trúc AWS Lambda, mình tóm tắt và mổ xẻ thêm góc nhìn technical ở đây để anh em cùng bàn luận.

Khi thiết kế RESTful API bằng AWS Lambda và API Gateway, câu hỏi kinh điển nhất luôn là: **Nên chia nhỏ Lambda đến mức nào?**

Thực tế, anh em thường bị kẹt giữa 2 thái cực:

### 1. Single Responsibility Lambda (Mỗi Endpoint 1 Function)

*   **Cách làm:** Chia nhỏ đến tận cùng (Fine-grained). Mỗi HTTP route + method (`GET /users`, `POST /users`, `DELETE /users/{id}`) map với một Lambda function riêng biệt.
*   **Điểm cộng:**
    *   Tách biệt source code, bundle size cực nhỏ giúp tối ưu thời gian Init Phase (giảm Cold Start).
    *   Áp dụng triệt để nguyên tắc đặc quyền tối thiểu (Least Privilege IAM). Hàm GET chỉ có quyền `dynamodb:GetItem`, hàm POST mới có quyền `dynamodb:PutItem`.
    *   Cấu hình Memory/Timeout độc lập (hàm tính toán nặng cấp 1024MB RAM, hàm nhẹ chỉ cần 128MB).
*   **Điểm trừ:** Quản lý cơ sở hạ tầng (IaC) cực kỳ vất vả. Nếu dùng AWS CloudFormation, bạn rất dễ chạm ngưỡng hard-limit (500 resources/stack). Các hàm ít được gọi (như DELETE) sẽ liên tục bị thu hồi Execution Context, dẫn đến tỷ lệ dính Cold Start cực cao cho end-user. Code lặp lại (DRY violation) ở các khâu database connection pooling hay middleware.

### 2. The Lambda-lith (Gộp tất cả vào một)

*   **Cách làm:** Đẩy toàn bộ API routes của một service vào một function duy nhất (Monolithic Lambda). Thường sử dụng các adapter như `aws-serverless-express` để chạy Express/NestJS (Node.js) hoặc Spring Boot (Java) bên trong Lambda. API Gateway lúc này chỉ đóng vai trò `{proxy+}`.
*   **Điểm cộng:** Code tập trung, dễ dàng chia sẻ DTO, Entity, Database Connection. Tỷ lệ Warm Start tiệm cận 100% vì function liên tục nhận traffic, giữ cho môi trường (container) luôn sống.
*   **Điểm trừ:**
    *   **Deployment Package phình to:** Rất dễ chạm mốc 250MB unzipped. Kéo theo Cold Start cực kỳ ám ảnh, đặc biệt khi phải khởi tạo framework context nặng nề (như DI container của NestJS hay khởi động JVM của Spring Boot).
    *   **Lãng phí GB-seconds:** Bạn đang dùng compute time của Lambda (trả tiền theo ms) chỉ để làm nhiệm vụ routing – việc mà API Gateway có thể làm miễn phí/rẻ hơn rất nhiều.
    *   **IAM Policy phình to:** Function này buộc phải có quyền Read/Write lên tất cả các Table/S3 Bucket mà service đó động tới, tăng rủi ro bảo mật (Blast radius lớn).

---

### Lối đi giữa: Tách biệt luồng Đọc và Ghi (The Pragmatic Middle Ground)

Hai tác giả AWS đã đưa ra một thiết kế thứ 3, trung hòa hoàn hảo hai thái cực trên bằng cách chia theo hành vi (Behavior). Thay vì chia quá nhỏ hay gộp quá lớn, chúng ta chia một Bounded Context thành đúng 2 Lambda Functions:

1.  **Lambda Ghi (Command/Write Operations):** Gom các request thay đổi trạng thái hệ thống (POST, PUT, DELETE, PATCH).
    *   *Technical insight:* Các thao tác ghi thường share chung các thư viện validation phức tạp, ORM mapping, và logic transaction. Việc gom lại giúp bundle size được tối ưu hợp lý, đồng thời các hàm ít dùng (như DELETE) được "ké" Execution Context đang warm từ các hàm POST có traffic cao hơn.
2.  **Lambda Đọc (Query/Read Operations):** Xử lý thuần túy luồng GET.
    *   *Technical insight:* Logic đọc thường rất nhẹ, chủ yếu là query DB và map response. Không cần nạp các thư viện validate body (như Joi, Zod hay class-validator). Bundle size nhỏ, Init phase nhanh, giúp latency của API trả về cho end-user đạt mức tối ưu nhất.

---

### Khả năng tiến hóa (Evolutionary Architecture) lên CQRS

Điều giá trị nhất của pattern này là nó tạo ra một bước đệm hoàn hảo để hệ thống tiến hóa lên **CQRS (Command Query Responsibility Segregation)** và kiến trúc hướng sự kiện (Event-Driven) khi scale:

*   **Decouple luồng Write:** Thay vì API Gateway gọi thẳng Lambda Ghi (Synchronous), bạn có thể dễ dàng chuyển sang Asynchronous bằng cách kẹp Amazon SQS vào giữa. API Gateway trả ngay HTTP 202 Accepted. Lambda Ghi sẽ pull message từ SQS thông qua Event Source Mapping và xử lý dạng Batching, kết hợp với Dead Letter Queue (DLQ) để retry. Điều này giúp database (như RDS hay MongoDB) không bị over-connection khi traffic spike.
*   **Scale luồng Read:** Ở phía Đọc, vì logic đã hoàn toàn tách biệt, bạn có thể tự do cắm thêm ElastiCache (Redis) theo pattern Cache-Aside, hoặc trỏ thẳng Lambda Đọc vào một Read Replica Database mà không lo sợ rủi ro side-effect làm gãy luồng Ghi.

### Góc nhìn thực tế từ Domain Quản lý học vụ (EdTech System)

Anh em tưởng tượng khi build một hệ thống quản lý trường học (School Management System), bản chất traffic đọc và ghi hoàn toàn bất đối xứng. Lượng request vào để Đọc (sinh viên xem thời khóa biểu, check điểm, tải thông báo) có thể sinh ra hàng ngàn RPS. Trong khi đó, luồng Ghi (giảng viên điểm danh, phòng đào tạo nhập điểm) lại ít hơn rất nhiều nhưng yêu cầu tính toàn vẹn dữ liệu (ACID) cực cao.

Áp dụng pattern Read-Write Separation này giúp chúng ta scale thẳng tay memory và concurrency cho luồng Đọc trong các kỳ thi cao điểm, mà vẫn giữ luồng Ghi hoạt động ổn định, cô lập rủi ro và tối ưu chi phí hạ tầng AWS.

### Kết luận

Trong Serverless, không có kiến trúc nào là Silver Bullet. Việc chọn pattern nào phụ thuộc vào business context để bạn trade-off giữa: Tốc độ phát triển (DevX), Chi phí (Cost), Bảo mật (IAM), và Hiệu năng (Cold Start).

---

**Nguồn bài viết gốc:** [Comparing design approaches for building serverless microservices | AWS Compute Blog](https://aws.amazon.com/blogs/compute/comparing-design-approaches-for-building-serverless-microservices/)

**Bài viết đã dịch:** [AWS Study Group VN | Facebook](https://www.facebook.com/groups/awsstudygroupfcj/permalink/2203819477049679/?rdid=3sEML8B3PXziCwcs#)

**Hình ảnh minh họa:**

![Sơ đồ so sánh Single Responsibility vs Monolithic Lambda](/images/3-BlogsTranslated/blog3.1.png)
![Sơ đồ mô hình Read/Write Separation](/images/3-BlogsTranslated/blog3.2.png)
