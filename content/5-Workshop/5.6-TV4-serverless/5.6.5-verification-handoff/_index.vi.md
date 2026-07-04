---
title: "Kiểm thử toàn trình (Verifying Handoff)"
date: 2026-07-03
weight: 5
chapter: false
pre: " <b> 5.6.5. </b> "
---

Đây là bước cuối cùng để nghiệm thu toàn bộ kiến trúc hệ thống ZeroBug-Agent. Chúng ta sẽ đóng vai người dùng cuối, đi qua luồng thao tác (User Flow) thực tế trên giao diện Frontend để xác minh sự xuyên suốt của dữ liệu: từ trình duyệt $\rightarrow$ Backend (Spring Boot) $\rightarrow$ AWS Step Functions $\rightarrow$ Amazon Bedrock $\rightarrow$ Trả về kết quả.

#### 1. Xác thực người dùng (Authentication)
Truy cập vào ứng dụng qua cổng Frontend. Tại màn hình đăng nhập, sử dụng tài khoản đã đăng ký (được cấp phép và lưu trữ an toàn trong AWS RDS) để lấy phiên làm việc (Session/Token) hợp lệ. Bước này đảm bảo mọi thao tác gọi AI phía sau đều được định danh và bảo mật nghiêm ngặt.

![](/images/5-Workshop/5.6/28.png)

#### 2. Bảng điều khiển và Khởi tạo dự án (Workspace Dashboard)
Sau khi đăng nhập thành công, hệ thống chuyển hướng người dùng đến giao diện Quản lý dự án. Tại đây, lịch sử các lần sinh Unit Test trước đó được tải về từ database. Nhấn chọn **Tạo dự án mới** để bắt đầu một luồng công việc.

![](/images/5-Workshop/5.6/29.png)

#### 3. Nạp mã nguồn vào hệ thống (Import Source Code)
Hệ thống cung cấp hai phương thức nạp mã nguồn linh hoạt: Tải lên trực tiếp file `.zip` (Local) hoặc kết nối qua URL của kho lưu trữ (GitHub/GitLab). Ngay khi bạn nhấn **Kết nối và tải dự án**, dữ liệu sẽ được Backend đẩy thẳng lên AWS S3 Bucket để làm tài nguyên RAG cho AI.

![](/images/5-Workshop/5.6/30.png)

#### 4. Lựa chọn ngữ cảnh và Ra lệnh (Context Selection & Prompting)
Khi mã nguồn được giải nén và phân tích thành công, không gian làm việc IDE thu nhỏ sẽ xuất hiện:
* **Thao tác 1:** Chọn file hoặc hàm cụ thể cần viết Unit Test ở thanh cây thư mục bên trái. Hệ thống sẽ trích xuất đoạn code này làm ngữ cảnh (Context).
* **Thao tác 2:** Nhập yêu cầu vào hộp thoại chat của ZeroBug-Agent (ví dụ: *"viết cho tôi unit test của hàm này"*). Nhấn gửi để chính thức kích hoạt cỗ máy AWS Step Functions hoạt động ngầm.

![](/images/5-Workshop/5.6/31.png)

#### 5. Nghiệm thu kết quả AI (AI Generation Result)
Chỉ sau vài giây xử lý toàn trình (Nhận diện File $\rightarrow$ Xây dựng RAG Context $\rightarrow$ Gọi Model Bedrock $\rightarrow$ Lưu lịch sử), Agent sẽ trả về mã nguồn Unit Test hoàn chỉnh ngay trên giao diện chat. Mã nguồn được định dạng chuẩn xác, sẵn sàng để sao chép (Copy) và tích hợp ngược lại vào IDE cục bộ của lập trình viên.

![](/images/5-Workshop/5.6/32.png)

**Kết luận:** Quy trình Handoff thành công rực rỡ. Kiến trúc Serverless tích hợp AI đã sẵn sàng đưa vào vận hành thực tế!