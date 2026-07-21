---
title: "Event 1"
date: 2026-05-23
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Bài Thu Hoạch Sự Kiện AWS Community Day (23/5)

## 1. Mục Đích Và Ý Nghĩa Của Sự Kiện

Sự kiện AWS Community Day (23/5) là cơ hội để tiếp cận những xu hướng công nghệ mới nhất trong hệ sinh thái AWS, đặc biệt là các chủ đề nổi bật như Generative AI (GenAI), Multi-Agent System, Amazon CloudFront, cùng các phương pháp phát triển sản phẩm và chia sẻ kinh nghiệm triển khai thực tế từ doanh nghiệp.

Không chỉ dừng lại ở các kiến thức lý thuyết, chương trình còn mang đến nhiều tình huống thực tiễn, các case study và kinh nghiệm giải quyết vấn đề trong môi trường phát triển sản phẩm với thời gian giới hạn. Qua đó, người tham dự có cơ hội hiểu rõ hơn cách các doanh nghiệp ứng dụng công nghệ AWS để xây dựng các hệ thống hiện đại, có khả năng mở rộng và đáp ứng yêu cầu thực tế.

---

## 2. Tổng Hợp Nội Dung Các Phiên Trình Bày

### Phiên 1: Build Second Brain (Diễn giả: Anh Tịnh)

Phiên trình bày tập trung giải thích nguyên nhân khiến AI đôi khi đưa ra câu trả lời chưa chính xác. Theo diễn giả, vấn đề không nằm ở năng lực của mô hình mà chủ yếu xuất phát từ việc thiếu ngữ cảnh (Context).

Thay vì cố gắng đưa thật nhiều dữ liệu vào câu lệnh (Prompt), người dùng nên cung cấp những thông tin có chọn lọc, chính xác và liên quan trực tiếp đến bài toán cần giải quyết. Xu hướng sử dụng AI hiện nay đang dịch chuyển từ việc tối ưu Prompt sang xây dựng Context và Memory, giúp AI có thể ghi nhớ tri thức dài hạn và phản hồi chính xác hơn.

**💡 Bài học:** Chất lượng của ngữ cảnh đóng vai trò quyết định hiệu quả của AI. Một hệ thống AI chỉ thực sự phát huy giá trị khi được cung cấp đầy đủ mục tiêu, dữ liệu nền và các ràng buộc phù hợp.

---

### Phiên 2: Friendly AI Assistant with Amazon Quick (Diễn giả: Hải Anh)

Phiên trình bày giới thiệu giải pháp **Amazon Quick Suite**, cho phép kết hợp dữ liệu nội bộ của doanh nghiệp với nguồn tri thức bên ngoài để xây dựng các trợ lý AI thông minh.

Các AI Agent được tạo ra có thể tự động hóa nhiều tác vụ văn phòng như:
- Ghi biên bản cuộc họp (Meeting Minutes)
- Gửi email thông báo
- Quản lý lịch làm việc
- Hỗ trợ phối hợp giữa các phòng ban

Điều này giúp giảm đáng kể khối lượng công việc thủ công, đồng thời nâng cao năng suất và hiệu quả làm việc của doanh nghiệp.

---

### Phiên 3: From Edge To Origin – CloudFront as Your Foundation (Diễn giả: Tuấn Thịnh)

Phiên trình bày làm rõ vai trò của Amazon CloudFront trong kiến trúc hệ thống hiện đại. CloudFront không chỉ là một dịch vụ CDN giúp phân phối nội dung nhanh hơn mà còn đóng vai trò như lớp bảo vệ đầu tiên của hệ thống.

**Cơ chế Flat-rate Pricing:**

Diễn giả giới thiệu các gói **Free, Pro, Business và Premium** nhằm giúp doanh nghiệp chủ động kiểm soát chi phí, hạn chế rủi ro phát sinh hóa đơn lớn khi xảy ra các cuộc tấn công DDoS hoặc khi lưu lượng truy cập tăng đột biến.

**Các lợi ích kỹ thuật:**
- Giảm tải cho máy chủ gốc bằng cách nén dữ liệu
- Xử lý TLS Handshake ngay tại các Edge Location
- Chặn các cuộc tấn công DDoS trước khi lưu lượng độc hại đến hệ thống backend

Điều này góp phần cải thiện hiệu năng, tăng tính sẵn sàng và tối ưu chi phí vận hành.

![Why CloudFront is better equipped for volumetric attack](/images/4-EventParticipated/4.1-Event1/cloudfront-ddos.png)
_Kiến trúc CloudFront giúp chống lại các cuộc tấn công DDoS_

---

### Phiên 4: 36 Hours with LotusHacks – Building UTMorpho from Idea to Reality (Diễn giả: Team VIB)

Phiên chia sẻ kể lại hành trình xây dựng sản phẩm **UTMorpho** trong cuộc thi Hackathon kéo dài 36 giờ.

UTMorpho là một AI Agent có khả năng chuyển đổi mô tả bằng ngôn ngữ tự nhiên thành giao diện người dùng (UI) và hỗ trợ chỉnh sửa trực tiếp trên giao diện **WYSIWYG**. Giải pháp này giúp giảm đáng kể việc phải liên tục viết lại Prompt khi muốn điều chỉnh thiết kế.

**Bài học từ hành trình phát triển:**
- Những ý tưởng giá trị thường xuất phát từ chính các khó khăn trong quá trình làm việc hằng ngày.
- Trong môi trường áp lực cao, sự phối hợp và thấu hiểu giữa các thành viên quan trọng không kém năng lực chuyên môn.
- Việc xác định đúng vấn đề và tập trung xây dựng **MVP (Minimum Viable Product)** là yếu tố quyết định thành công của dự án.

![Architecture](/images/4-EventParticipated/4.1-Event1/utmorpho-architecture.png)
_Kiến trúc hệ thống của UTMorpho_

---

### Phiên 5: Deep Dive Talk – How LLM Actually Works? (Diễn giả: Đào Đức)

Phiên trình bày đi sâu vào nguyên lý hoạt động của **Large Language Model (LLM)**, đặc biệt là đặc tính không hoàn toàn xác định (Non-deterministic).

Ngay cả khi đặt tham số **Temperature = 0**, mô hình vẫn có khả năng sinh ra các kết quả khác nhau do:
- Ảnh hưởng của quá trình tính toán dấu phẩy động trên GPU
- Kỹ thuật xử lý song song (Batching) trong hạ tầng Cloud

**Khuyến nghị khi triển khai AI trong Production:**
- Sử dụng **Temperature = 0.1**
- Kết hợp kỹ thuật **Majority Voting**
- Ưu tiên đầu ra có cấu trúc (**JSON Mode**)
- Xây dựng các cơ chế **Guardrails** nhằm kiểm soát chất lượng và tính an toàn của hệ thống

![The role of Temperature, Top-P, and Top-K](/images/4-EventParticipated/4.1-Event1/llm-temperature.png)
_Sự ảnh hưởng của tham số Temperature đối với LLM_

---

### Phiên 6: Enterprise-Grade Multi-Agent System – The Case of Startup Credit Scoring (Diễn giả: Cát Vy)

Phiên cuối cùng trình bày mô hình **Multi-Agent System** trong bài toán đánh giá tín dụng dành cho các startup.

Do các doanh nghiệp khởi nghiệp thường thiếu lịch sử tín dụng và tài sản đảm bảo, VPBank đã xây dựng một **Virtual Credit Committee** gồm nhiều AI Agent chuyên trách các nhiệm vụ khác nhau:

| Agent | Nhiệm vụ |
|---|---|
| Financial Analyst | Phân tích tài chính |
| Market Assessor | Đánh giá thị trường |
| Team Evaluator | Phân tích đội ngũ sáng lập |
| Risk Manager | Quản trị rủi ro |
| Compliance Officer | Kiểm tra tuân thủ |

**Kết quả đạt được:**
- Thời gian đánh giá hồ sơ giảm từ **2–3 tuần** xuống còn **2–4 giờ**
- Chi phí vận hành giảm khoảng **95%**
- Tỷ lệ phê duyệt hồ sơ tăng **gấp đôi**

Việc phân chia nhiệm vụ thành nhiều Agent độc lập giúp hệ thống xử lý song song, nâng cao độ chính xác và hiệu quả ra quyết định.

![Virtual credit committee architecture](/images/4-EventParticipated/4.1-Event1/virtual-credit-committee.png)
_Mô hình Multi-Agent trong hệ thống Virtual Credit Committee_

![Proposed Deployment Approach](/images/4-EventParticipated/4.1-Event1/deployment-approach.png)
_Cách thức triển khai hệ thống AgentCore trên AWS_

---

## 3. Những Điểm Nổi Bật Và Bài Học Rút Ra

Sau khi tham dự sự kiện, em nhận thấy một số bài học quan trọng có thể áp dụng trong học tập và công việc.

### Về AI

Hiệu quả của mô hình phụ thuộc rất lớn vào chất lượng của ngữ cảnh được cung cấp. Việc xác định rõ mục tiêu, dữ liệu đầu vào, các ràng buộc và kết quả mong muốn sẽ giúp AI đưa ra phản hồi chính xác hơn. Đồng thời, do LLM vẫn tồn tại tính không ổn định, các hệ thống thực tế cần được thiết kế thêm cơ chế kiểm thử, giám sát và xác thực kết quả.

### Về Kiến Trúc Cloud

Amazon CloudFront không chỉ giúp tăng tốc độ truy cập mà còn đóng vai trò quan trọng trong việc tối ưu hiệu năng, kiểm soát chi phí và tăng cường khả năng bảo mật cho hệ thống.

### Về Phát Triển Sản Phẩm

Môi trường Hackathon cho thấy tầm quan trọng của việc nhanh chóng xây dựng sản phẩm khả dụng tối thiểu (MVP), liên tục thử nghiệm và cải tiến dựa trên phản hồi thực tế thay vì theo đuổi một giải pháp hoàn hảo ngay từ đầu.

---

## 4. Kế Hoạch Ứng Dụng Vào Học Tập Và Công Việc

Sau sự kiện, em định hướng áp dụng những kiến thức đã tiếp thu vào quá trình học tập và phát triển bản thân.

1. **Thay đổi cách sử dụng AI:** Tập trung xây dựng ngữ cảnh đầy đủ thay vì chỉ tối ưu câu lệnh, từ đó nâng cao hiệu quả khi học tập, nghiên cứu và lập trình.

2. **Phát triển AI workflow cá nhân:** Xây dựng các workflow phục vụ công việc như tự động ghi chú, tổng hợp tài liệu, hỗ trợ phân tích dữ liệu và quản lý tri thức cá nhân.

3. **Nghiên cứu chuyên sâu về Cloud:** Tiếp tục tìm hiểu về Amazon CloudFront và kiến trúc Multi-Agent System nhằm xây dựng các giải pháp AI có khả năng xử lý những bài toán phức tạp trong doanh nghiệp.

4. **Thiết kế cơ chế kiểm soát LLM:** Khi tích hợp LLM vào các dự án thực tế, chú trọng thiết kế các cơ chế kiểm tra chéo, giám sát và đánh giá kết quả nhằm đảm bảo tính chính xác, ổn định và an toàn của hệ thống.

---

### Một Số Hình Ảnh Khi Tham Gia Sự Kiện

![AWS Community Day 2026](/images/4-EventParticipated/4.1-Event1/event1.jpg)
_Hình ảnh tại sự kiện AWS Community Day 2026_

Tổng thể, sự kiện giúp em hiểu rõ hơn về cách kết hợp AI, cloud infrastructure và tư duy sản phẩm để giải quyết các bài toán thực tế. Đây cũng là động lực để em tiếp tục học sâu hơn về AI engineering, cloud architecture và các hệ thống multi-agent trong tương lai.