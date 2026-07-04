---
title: "Worklog Tuần 10"
date: 2026-01-01
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Mục tiêu tuần 10:

*   Tái cấu trúc kiến trúc hệ thống, chuyển đổi mô hình xử lý tĩnh tại local sang kiến trúc Serverless động trên AWS nhằm cải thiện hiệu năng.
*   Hoàn thiện "Lõi AI" với luồng Retrieval-Augmented Generation và Google Gemini API.
*   Phát triển hệ thống API quản lý dự án và tích hợp Amazon RDS (PostgreSQL) để quản lý siêu dữ liệu.

### Các công việc cần triển khai trong tuần này:
<table class="worklog-table">
<colgroup>
  <col class="col-day" style="width:5%">
  <col class="col-task" style="width:42%">
  <col class="col-start" style="width:13%">
  <col class="col-end" style="width:13%">
  <col class="col-ref" style="width:27%">
</colgroup>
  <thead>
    <tr>
      <th>Ngày</th>
      <th>Công việc</th>
      <th>Ngày bắt đầu</th>
      <th>Ngày hoàn thành</th>
      <th>Nguồn tài liệu</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td class="col-day">1</td>
      <td class="col-task">- Tái cấu trúc hệ thống: Bắt đầu chuyển đổi luồng xử lý nặng từ local lên hạ tầng AWS Cloud để tăng tính mở rộng. <br> - Chuẩn bị môi trường Cloud: Tạo IAM User và cấp quyền truy cập cần thiết cho các dịch vụ S3 và RDS. Kiểm tra quyền và cấu hình AWS CLI phục vụ quá trình triển khai.</td>
      <td class="col-date">22/06/2026</td>
      <td class="col-date">22/06/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">2</td>
      <td class="col-task">- Thiết kế Cơ sở dữ liệu: Xây dựng sơ đồ cơ sở dữ liệu trên Amazon RDS (PostgreSQL) lưu trữ siêu dữ liệu (Metadata) bao gồm: thông tin Người dùng, danh sách Dự án (Git/Zip, thời gian tạo) và Lịch sử sinh mã của AI. <br> - Cấu hình Routing: Thực hiện cấu hình Amazon Route 53, tạo Hosted Zone cho tên miền của hệ thống và tạo DNS Record trỏ đến CloudFront Distribution theo kiến trúc đã thiết kế.</td>
      <td class="col-date">23/06/2026</td>
      <td class="col-date">23/06/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">3</td>
      <td class="col-task">- Phát triển Lambda 1 (Project Import - Git/Zip): Tiếp nhận mã nguồn, giải nén và đồng bộ dữ liệu thô lên Amazon S3, đồng thời ghi nhận siêu dữ liệu dự án vào RDS. Sử dụng VPC Endpoint để tối ưu quyền truy cập S3 và tránh Timeout. <br> - Cấu hình CDN: Triển khai Amazon CloudFront Distribution, cấu hình Origin kết nối đến API Gateway và thiết lập Cache Behaviors cho API & Static Assets.</td>
      <td class="col-date">24/06/2026</td>
      <td class="col-date">24/06/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">4</td>
      <td class="col-task">- Hoàn thiện Core AI: Triển khai thành công luồng RAG thực thi việc đọc dữ liệu ngữ cảnh từ kho lưu trữ Amazon S3 và giao tiếp với Google Gemini API để tự động sinh mã nguồn Unit Test. <br> - Bảo mật luồng truyền tải: Thực hiện cấu hình SSL Certificate, gắn Custom Domain cho CloudFront và kiểm tra khả năng truy cập an toàn qua giao thức HTTPS. <br> - Phát triển Lambda 2 (File Tree Service): Xây dựng hàm đọc cấu trúc thư mục, duyệt qua các file mã nguồn đã lưu trên Amazon S3 và trả về định dạng cây (Tree) cho Frontend hiển thị danh sách file.</td>
      <td class="col-date">25/06/2026</td>
      <td class="col-date">25/06/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">5</td>
      <td class="col-task">- Phát triển Lambda 3 (API Invoke Service): Xây dựng hàm cầu nối giao tiếp trực tiếp với AI. Hàm chịu trách nhiệm truyền dữ liệu Prompt và Context một cách bảo mật. <br> - Phát triển Lambda 4 (Rag Context Lambda): Xây dựng logic gom nhóm và tiền xử lý ngữ cảnh trước khi đưa vào mô hình ngôn ngữ lớn.</td>
      <td class="col-date">26/06/2026</td>
      <td class="col-date">26/06/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">6</td>
      <td class="col-task">- Phát triển Lambda 5 (Result And History Service): Hàm tiếp nhận phản hồi từ AI, lọc bỏ cú pháp Markdown thừa, lưu kết quả Unit Test tinh khiết lên S3 và ghi nhận vào bảng Lịch sử trong RDS để Frontend truy xuất. <br> - Tích hợp Spring Boot Backend: Thiết lập lớp ProjectApiController làm cầu nối giao tiếp giữa giao diện và các hàm AWS Lambda thông qua AWS SDK. <br> - Khắc phục sự cố: Xử lý triệt để các xung đột ánh xạ API và thiết lập cấu hình giải quyết lỗi thiếu Bean trong Spring Boot để đảm bảo luồng nghiệp vụ thông suốt.</td>
      <td class="col-date">27/06/2026</td>
      <td class="col-date">27/06/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">7</td>
      <td class="col-task">- Kiểm thử End-to-End (E2E): Chạy thực nghiệm toàn bộ chu trình sinh Unit Test tự động xuyên suốt từ: Frontend -> Backend -> AWS Lambda -> S3 & RDS. <br> - Đánh giá: Rà soát toàn bộ cấu hình hạ tầng AWS đã thực hiện, đánh giá hiệu năng xử lý của các hàm Lambda và khả năng phản hồi của Google Gemini API. <br> - Ghi nhận kết quả triển khai, hoàn tất chuẩn hóa code và cập nhật tài liệu kỹ thuật.</td>
      <td class="col-date">28/06/2026</td>
      <td class="col-date">28/06/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
  </tbody>
</table>


### Kết quả đạt được tuần 10:

*   Chuyển đổi thành công kiến trúc từ Local sang Serverless với việc đóng gói và triển khai 5 AWS Lambda functions (Import, Result, History, Invoke, FileTree).
*   Triển khai thành công luồng Core AI RAG kết nối ổn định với Google Gemini API, sinh ra mã Unit Test làm sạch markdown tự động.
*   Tích hợp hoàn thiện cơ sở dữ liệu Amazon RDS để quản lý Metadata người dùng và dự án.
*   Kết nối liền mạch toàn bộ hệ thống từ Frontend qua Backend xuống hệ sinh thái Serverless AWS.