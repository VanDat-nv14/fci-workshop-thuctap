---
title: "Event 1"
date: 2026-04-17
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Bài thu hoạch sự kiện về AI, CloudFront và Multi-Agent System

### Mục Đích Của Sự Kiện

- Cung cấp góc nhìn thực tế về cách ứng dụng AI hiệu quả thông qua việc xây dựng ngữ cảnh phù hợp.
- Giới thiệu các công cụ AI hỗ trợ phân tích dữ liệu, tự động hóa quy trình và cộng tác nhóm.
- Chia sẻ vai trò của Amazon CloudFront trong tối ưu chi phí, hiệu năng, bảo mật và độ tin cậy cho hệ thống.
- Trình bày kinh nghiệm xây dựng sản phẩm trong thời gian ngắn thông qua case study UTMorpho tại LotusHacks.
- Phân tích tính không xác định của LLM trong thực tế và cách giảm thiểu rủi ro khi triển khai.
- Giới thiệu mô hình multi-agent system trong bài toán credit scoring cho startup ở môi trường doanh nghiệp.

### Nội Dung Chương Trình

Sự kiện AWS Community Day 2026 tập trung vào các xu hướng công nghệ mới nhất trong hệ sinh thái AWS, đặc biệt nhấn mạnh vào AI tạo sinh (GenAI), Hệ thống đa tác nhân (Multi-Agent), Bảo mật mạng (CloudFront) và các chiến lược tối ưu hóa vận hành cho doanh nghiệp.

#### 1. Ứng dụng GenAI và Hệ thống Đa tác nhân (Multi-Agent Systems)

**Hệ thống xếp hạng tín dụng doanh nghiệp (Vy Lam - VPBank):**

- **Thực trạng:** Các hệ thống ngân hàng truyền thống thường từ chối startup do thiếu dữ liệu lịch sử hoặc tài sản thế chấp.
- **Giải pháp:** Sử dụng mô hình Multi-Agent (Virtual Credit Committee). Thay vì một Agent duy nhất, hệ thống chia nhỏ thành các Agent chuyên biệt: Phân tích tài chính, Đánh giá thị trường, Đánh giá đội ngũ, Phân tích rủi ro và Tuân thủ.
- **Hiệu quả:** Rút ngắn thời gian xử lý từ 2-3 tuần xuống còn 2-4 giờ (nhanh hơn 95%), giảm chi phí vận hành 95% và tăng tỷ lệ chấp thuận lên gấp đôi.

**Tự động hóa quy trình kinh doanh (Phạm Ng Hải Anh - G-AsiaPacific):**

- Giới thiệu Amazon Quick Suite, giải pháp giúp tích hợp dữ liệu công ty và kiến thức thế giới để tạo ra các Agent AI hỗ trợ các tác vụ lặp đi lặp lại như: tự động tạo biên bản họp (MoM), gửi email cho các bên liên quan và lên lịch họp.

#### 2. Kỹ thuật và Tối ưu hóa trong triển khai AI

**Tầm quan trọng của ngữ cảnh (Tinh Truong - GoTymeX):**

- Nguyên nhân chính khiến AI trả lời sai thường là do thiếu ngữ cảnh (Context) chứ không phải do mô hình yếu.
- **Lời khuyên:** Cần cung cấp bằng chứng có chọn lọc thay vì nhồi nhét quá nhiều dữ liệu (Chất lượng > Số lượng). Chuyển dịch từ "Prompt" sang "Context" và hướng tới "Memory" (Bộ não AI thứ hai).

**Tính không xác định của LLM**

- Ngay cả khi thiết lập Temperature = 0, mô hình LLM vẫn có thể đưa ra kết quả khác nhau do kiến trúc GPU (phép tính dấu phẩy động) và kỹ thuật batching trong hạ tầng cloud.
- **Giải pháp:** Nên sử dụng Temperature = 0.1 để tránh vòng lặp vô tận, thực hiện chạy nhiều lần (Majority voting) và ưu tiên đầu ra có cấu trúc (JSON mode).

#### 3. Hạ tầng và Bảo mật với Amazon CloudFront (Nguyễn Tuấn Thịnh)

- **Cơ chế Flat-rate Pricing mới:** AWS ra mắt các gói giá cố định (Free, Pro, Business, Premium) cho CloudFront giúp doanh nghiệp dự báo chi phí dễ dàng, tránh tình trạng "sốc hóa đơn" khi bị tấn công DDoS hoặc nội dung trở nên viral.
- **Tối ưu hóa hiệu năng & Chi phí:**
  - CloudFront giúp giảm tải CPU cho EC2 bằng cách đảm nhận việc nén dữ liệu (giảm dung lượng đến 82%) và xử lý TLS handshake.
  - Sử dụng mạng lưới Edge toàn cầu của AWS để chặn đứng các cuộc tấn công DDoS ngay tại điểm gần nguồn nhất.

#### 4. Case Study thực tế: UTMorpho (Team VIB)

- **Dự án:** Một Agent AI cho phép tạo giao diện UI từ mô tả và cho phép chỉnh sửa trực tiếp trên canvas (WYSIWYG) thay vì phải re-prompt nhiều lần làm trôi thiết kế.
- **Bài học kinh nghiệm:** Ý tưởng thực tế nhất đến từ chính những khó khăn trong công việc hàng ngày. Sự thấu hiểu giữa các thành viên trong đội ngũ quan trọng hơn kỹ năng cá nhân khi làm việc dưới áp lực thời gian (36 giờ hackathon).

### Các Điểm Mấu Chốt Cho Báo Cáo

- **Về kinh doanh:** AI đa tác nhân (Multi-Agent) giúp tiết kiệm 95% thời gian và chi phí cho các quy trình phức tạp như xét duyệt tín dụng.
- **Về kỹ thuật:** Context là chìa khóa để AI hoạt động hiệu quả; cần lưu ý tính không xác định của LLM ngay cả ở mức thiết lập thấp nhất.
- **Về hạ tầng:** CloudFront không chỉ là CDN mà còn là nền tảng bảo mật giúp kiểm soát chi phí hạ tầng hiệu quả với các gói giá cố định mới.

### Những Gì Học Được

#### Tư Duy Làm Việc Với AI

- AI không chỉ phụ thuộc vào prompt mà còn phụ thuộc rất nhiều vào ngữ cảnh được cung cấp.
- Muốn AI hỗ trợ tốt hơn cần mô tả rõ mục tiêu, dữ liệu đầu vào, ràng buộc, tiêu chí đánh giá và kỳ vọng đầu ra.
- Khái niệm Second AI Brain cho thấy xu hướng AI sẽ ngày càng gắn với memory, workflow cá nhân và tri thức dài hạn.
- Sinh viên nên bắt đầu học AI bằng cách xây dựng các use case nhỏ, có dữ liệu thật và có vòng lặp cải thiện liên tục.

#### Kiến Thức Về Cloud Và Hạ Tầng

- CloudFront có thể được xem là một lớp nền quan trọng giữa người dùng và origin.
- CDN không chỉ phục vụ static content mà còn có vai trò trong bảo mật, reliability và tối ưu chi phí.
- Việc đưa workload ra edge giúp cải thiện trải nghiệm người dùng, đặc biệt với hệ thống có người dùng phân tán ở nhiều khu vực.
- Khi thiết kế hệ thống cloud, cần cân nhắc đồng thời hiệu năng, chi phí, bảo mật và khả năng mở rộng.

#### Kinh Nghiệm Xây Dựng Sản Phẩm

- Hackathon là môi trường tốt để rèn luyện khả năng biến ý tưởng thành sản phẩm trong thời gian ngắn.
- Việc xác định vấn đề rõ ràng quan trọng hơn việc bắt đầu code quá sớm.
- Trong sprint ngắn, nhóm cần ưu tiên tính năng cốt lõi, phân chia công việc hợp lý và liên tục kiểm tra tiến độ.
- Những thất bại hoặc thay đổi giữa chừng có thể trở thành bước ngoặt giúp sản phẩm thực tế hơn.

#### Hiểu Rõ Hơn Về LLM Và Multi-Agent

- LLM có thể không hoàn toàn deterministic dù đã cấu hình các tham số tưởng như cố định.
- Khi đưa AI vào production, cần có chiến lược kiểm thử, giám sát và quản trị rủi ro.
- Multi-agent system phù hợp với các bài toán có nhiều bước đánh giá, nhiều nguồn dữ liệu và cần nhiều vai trò chuyên môn.
- Guardrails, compliance và khả năng giải thích là những yếu tố rất quan trọng khi triển khai AI trong doanh nghiệp.

### Ứng Dụng Vào Học Tập Và Công Việc

- Áp dụng cách viết prompt có ngữ cảnh rõ ràng hơn khi sử dụng AI để học tập, nghiên cứu và lập trình.
- Thử xây dựng một workflow AI nhỏ phục vụ việc ghi chú, tổng hợp tài liệu hoặc phân tích dữ liệu cá nhân.
- Tìm hiểu sâu hơn về Amazon CloudFront và cách tích hợp CDN vào kiến trúc web application.
- Rèn luyện kỹ năng xây dựng MVP thông qua các project ngắn hoặc hackathon.
- Khi dùng LLM trong project, cần thiết kế cơ chế kiểm tra output thay vì mặc định tin rằng kết quả luôn ổn định.
- Nghiên cứu thêm về multi-agent system để áp dụng cho các bài toán phức tạp như phân tích tài chính, đánh giá rủi ro hoặc hỗ trợ ra quyết định.

### Trải Nghiệm Trong Event

Tham gia sự kiện là một trải nghiệm bổ ích vì nội dung không chỉ tập trung vào lý thuyết mà còn có nhiều ví dụ thực tế về AI, cloud infrastructure và quá trình xây dựng sản phẩm. Các phiên chia sẻ giúp tôi nhìn rõ hơn cách AI đang được áp dụng trong nhiều ngữ cảnh khác nhau, từ trợ lý phân tích dữ liệu, workflow tự động, đến hệ thống multi-agent cấp doanh nghiệp.

Điểm tôi ấn tượng nhất là thông điệp "context is everything". Trước đây, tôi thường nghĩ việc sử dụng AI chủ yếu phụ thuộc vào cách viết prompt. Sau sự kiện, tôi hiểu rằng prompt chỉ là một phần, còn chất lượng ngữ cảnh, dữ liệu nền và mục tiêu cụ thể mới là yếu tố quyết định AI có thể hỗ trợ hiệu quả hay không.

Phiên về CloudFront cũng giúp tôi mở rộng góc nhìn về hạ tầng cloud. Tôi nhận ra rằng một dịch vụ CDN có thể đóng vai trò quan trọng trong toàn bộ kiến trúc, từ cải thiện tốc độ tải trang, giảm chi phí origin, đến tăng cường bảo mật và độ tin cậy.

Ngoài ra, case study UTMorpho từ LotusHacks mang lại nhiều cảm hứng về tinh thần xây dựng sản phẩm. Việc một nhóm có thể đi từ ý tưởng đến demo trong 36 giờ cho thấy tầm quan trọng của khả năng làm việc nhóm, xác định vấn đề đúng và ưu tiên tính năng phù hợp.

#### Một Số Hình Ảnh Khi Tham Gia Sự Kiện

![AWS Community Day 2026](/images/4-EventParticipated/4.1-Event1/event1.jpg)
_Hình ảnh tại sự kiện AWS Community Day 2026_

![Virtual credit committee architecture](/images/4-EventParticipated/4.1-Event1/virtual-credit-committee.png)
_Mô hình Multi-Agent trong hệ thống Virtual Credit Committee_

![Proposed Deployment Approach](/images/4-EventParticipated/4.1-Event1/deployment-approach.png)
_Cách thức triển khai hệ thống AgentCore trên AWS_

![Why CloudFront is better equipped for volumetric attack](/images/4-EventParticipated/4.1-Event1/cloudfront-ddos.png)
_Kiến trúc CloudFront giúp chống lại các cuộc tấn công DDoS_

![Architecture](/images/4-EventParticipated/4.1-Event1/utmorpho-architecture.png)
_Kiến trúc hệ thống của UTMorpho_

![The role of Temperature, Top-P, and Top-K](/images/4-EventParticipated/4.1-Event1/llm-temperature.png)
_Sự ảnh hưởng của tham số Temperature đối với LLM_

> Tổng thể, sự kiện giúp tôi hiểu rõ hơn về cách kết hợp AI, cloud infrastructure và tư duy sản phẩm để giải quyết các bài toán thực tế. Đây cũng là động lực để tôi tiếp tục học sâu hơn về AI engineering, cloud architecture và các hệ thống multi-agent trong tương lai.
