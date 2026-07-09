---
title: "Blog 1"
date: 2026-06-30
weight: 1
chapter: false
pre: "<b> 3.1. </b>"
---

# Scaling telco automation to millions of devices with Managed Red Hat Ansible Automation Platform (AAP) on AWS and Red Hat OpenShift Service on AWS (ROSA)

Chào mọi người cộng đồng AWS Study Group VN.

Nếu mọi người đang làm trong ngành liên quan đến viễn thông hoặc đang vận hành những hệ thống có số lượng thiết bị đầu cuối khổng lồ lên tới hàng triệu node như router gia đình, modem hay broadband gateway, việc cập nhật cấu hình hay nâng cấp firmware đồng loạt mà không làm nghẽn hệ thống luôn là một bài toán cực kỳ đau đầu.

Hôm nay mình muốn chia sẻ với mọi người một kiến trúc rất hay được lược dịch và bổ sung chi tiết từ AWS Architecture Blog, giải quyết bài toán quy mô siêu khủng này bằng cách kết hợp bộ đôi Managed Red Hat Ansible Automation Platform (AAP) trên AWS và Red Hat OpenShift Service on AWS (ROSA).

### Điểm Mấu Chốt: Tách Biệt Hoàn Toàn Mặt Điều Khiển (Control Plane) Và Mặt Thực Thi (Execution Plane)

Các hệ thống quản lý truyền thống thường gom cả cụm điều khiển (giao diện, API) và cục thực thi (chạy lệnh) vào chung một hạ tầng máy chủ. Khi mọi người ra lệnh cho hàng trăm ngàn thiết bị cùng lúc, lượng tiến trình tăng đột biến sẽ chiếm sạch tài nguyên, gây treo luôn cả giao diện quản lý của kỹ sư.

Với kiến trúc hiện đại này:

*   **Control Plane:** AAP trên AWS đóng vai trò trung tâm (chỉ xử lý giao diện UI, API và điều phối). Cụm điều khiển này sẽ đồng bộ động danh sách thiết bị (Inventory) từ các nguồn bên ngoài như hệ thống CMDB hoặc Cloud Providers, đồng thời đồng bộ kịch bản tự động hóa (Playbooks) từ các kho quản lý mã nguồn tập trung như GitHub.
*   **Execution Plane:** Việc chạy các kịch bản Ansible Playbook nặng nhọc sẽ được đẩy hết sang các Pod container (gọi là Container Groups trong Kubernetes) chạy trên cụm ROSA.

Khi cao điểm nổ ra, mặt thực thi có thể phình to thoải mái qua giao thức IPv4/IPv6 tới các gateway thiết bị, trong khi mặt điều khiển của mọi người vẫn phản hồi mượt mà.

### Hạ Tầng Đỉnh Cao: Thiết Kế Đa Vùng (Multi-Region) & Stateless Với Amazon S3

Để gánh được quy mô hàng triệu endpoint trên toàn quốc, hệ thống không chỉ chạy ở một chỗ. Kiến trúc này triển khai Red Hat OpenShift Service on AWS (ROSA) trên nhiều vùng địa lý của AWS (Multi-Region) như us-east-1, us-west-1, us-west-2. Ở mỗi vùng, các Worker Nodes chịu trách nhiệm thực thi lại tiếp tục được phân bổ đều trên 3 Vùng sẵn sàng độc lập (Availability Zones: AZ-A, AZ-B, và AZ-C) để đảm bảo tính sẵn sàng cao (High Availability), đứt cáp hay sập một zone cũng không sợ chết hệ thống.

Đặc biệt, để giữ cho cụm AAP luôn nhẹ nhàng và phi trạng thái (stateless), toàn bộ dữ liệu quản lý nặng nề được offload hoàn toàn sang Amazon Simple Storage Service (S3) Buckets. S3 sẽ là nơi lưu trữ và phân phối các Ansible Content Collection cũng như các Container Images của Automation Execution Environments.

### Cơ Chế Co Giãn Tự Động Và Tối Ưu Chi Phí Theo Thời Gian Thực

Đây là điểm đáng tiền nhất của giải pháp. Thay vì phải duy trì một dàn máy chủ khủng để phòng hờ cho những ngày nâng cấp hệ thống rồi bỏ không vào ngày bình thường, sự kết hợp giữa ROSA và AWS giúp tối ưu chi phí tuyệt đối theo mô hình dùng bao nhiêu trả bấy nhiêu.

Hãy nhìn vào timeline thực tế của một đợt nâng cấp định kỳ:

*   **09:00 AM (Trạng thái bình thường):** Hệ thống chỉ chạy đúng 3 máy chủ EC2 (Worker Nodes) để giám sát nhẹ nhàng, chi phí giữ ở mức tối thiểu.
*   **09:01 AM (Vào giờ cao điểm):** Lịch nâng cấp "Firmware Tuesday" được kích hoạt đồng loạt cho 1,000 thiết bị home router.
*   **09:02 AM (Bùng nổ tiến trình):** Để xử lý lượng lớn kết nối song song (dựa trên cấu hình fork), AAP yêu cầu sinh ra ngay lập tức 200 Pod thực thi trên Kubernetes.
*   **09:05 AM (Tự động mở rộng ngầm):** Vì 3 máy chủ ban đầu đã full tài nguyên, các Pod mới rơi vào trạng thái Pending. Ngay lập tức, tính năng ROSA Cluster Autoscaler phát hiện ra và gọi AWS bật thêm các máy chủ EC2 mới ở nền để gánh tải. Công việc được xử lý mượt mà, không lo nghẽn mạch.
*   **09:30 AM (Nhiệm vụ hoàn thành):** Chiến dịch nâng cấp firmware thành công, 200 Pod thực thi tự động bị hủy bỏ.
*   **09:45 AM (Tự động thu nhỏ):** Hệ thống nhận thấy các máy chủ EC2 mới bật thêm giờ đã trống rỗng. ROSA Metrics Server sẽ tự động hạ tải và tắt chúng đi, đưa hạ tầng về lại trạng thái 3 máy chủ ban đầu.

Kết quả là doanh nghiệp chỉ phải trả tiền cho lượng máy chủ phát sinh đúng trong vòng mấy chục phút cao điểm đó chứ không cần phải nuôi hệ thống chạy hết công suất 24/7.

### Đẩy Logs Sang Hệ Thống OLAP Để Huấn Luyện AI / Operations LLM

Chạy xong hàng triệu thiết bị thì lượng log sinh ra là khổng lồ. Kiến trúc này không lưu log theo kiểu truyền thống dễ gây nghẽn I/O. Toàn bộ dữ liệu thực thi, log và metrics từ AAP sẽ được đẩy liên tục vào một cơ sở dữ liệu OLAP (Online Analytical Processing) phân tán, chuyên dụng cho phân tích thời gian thực và log aggregation như Grafana / Grafana Loki.

Bên cạnh việc giúp các kỹ sư có Dashboard tường minh để debug, điểm "đắt giá" nhất ở đây là nguồn dữ liệu log sạch và khổng lồ này sẽ được dùng làm nguyên liệu đầu vào để huấn luyện các mô hình AI/LLM chuyên dụng cho vận hành (Operations LLM Mode). Trong tương lai, chính AI sẽ tự học từ các log này để tự động phát hiện lỗi và đưa ra giải pháp tự sửa lỗi (Self-healing) cho mạng lưới viễn thông.

### Những Lợi Thế Vượt Trội Khác So Với Cách Làm Cũ

Bên cạnh việc co giãn linh hoạt, kiến trúc này giải quyết triệt để bài toán xung đột thư viện (dependencies) giữa các đời thiết bị khác nhau. Mỗi kịch bản tự động hóa được đóng gói gọn gàng thành một Container Image riêng (Execution Environment), chạy xong tự xóa sạch, không để lại rác hay làm ảnh hưởng đến các tiến trình khác.

Đồng thời, vì cả AAP và ROSA đều là các dịch vụ được quản lý hoàn toàn (Managed Services) phối hợp bởi Red Hat và AWS, toàn bộ gánh nặng về vá lỗi hệ điều hành, cấu hình mạng network layer hay duy trì hạ tầng đều được giảm bớt cho đội ngũ vận hành. Kỹ sư chỉ cần tập trung vào việc viết Playbook để cấu hình thiết bị.

Để bảo vệ túi tiền của doanh nghiệp, mọi người hoàn toàn có thể đặt các hàng rào kiểm soát (Fences) như:

*   **Kubernetes Resource Quotas:** Giới hạn cứng số lượng CPU tối đa mà AAP được phép chiếm dụng trong namespace.
*   **AWS Service Quotas:** Giới hạn trần số lượng instance EC2 (ví dụ dòng m5.xlarge) được phép bật lên trong một Region, tránh trường hợp kịch bản tự động bị lỗi vòng lặp vô hạn làm tăng hóa đơn ngoài ý muốn.

---

**Nguồn bài viết gốc:** [AWS IBM-RedHat Blog](https://aws.amazon.com/vi/blogs/ibm-redhat/scaling-telco-automation-to-millions-of-devices-with-managed-red-hat-ansible-automation-platform-aap-on-aws-and-red-hat-openshift-service-on-aws-rosa/)

**Bài viết đã dịch:** [AWS Study Group VN | Facebook](https://www.facebook.com/share/p/1Uaej3AE3d/)

**Hình ảnh minh họa:**

![Sơ đồ kiến trúc tự động hóa viễn thông](/images/3-BlogsTranslated/blog1.0.png)
![Sơ đồ co giãn tự động](/images/3-BlogsTranslated/blog1.1.png)