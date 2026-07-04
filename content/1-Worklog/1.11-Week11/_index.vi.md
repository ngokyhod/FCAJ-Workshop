---
title: "Worklog Tuần 11"
date: 2026-01-01
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Mục tiêu tuần 11:

*   Tích hợp phần hạ tầng AWS đã triển khai vào hệ thống chung của toàn nhóm.
*   Kiểm thử toàn bộ hệ thống Spring Boot Backend, AI Agent và luồng dữ liệu trên máy chủ.
*   Điều chỉnh cấu hình Cloud, xử lý lỗi giao tiếp giữa các service để đảm bảo hệ thống hoạt động ổn định.

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
      <td class="col-task">- Tiến hành ghép phần hạ tầng AWS (các hàm Lambda, S3, RDS) đã triển khai vào luồng mã nguồn Spring Boot chính của nhóm. <br> - Kết nối với máy chủ chính và kiểm tra khả năng tương thích giữa các thành phần Backend qua AWS SDK, đảm bảo Spring Boot gọi thành công các dịch vụ Serverless.</td>
      <td class="col-date">29/06/2026</td>
      <td class="col-date">29/06/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">2</td>
      <td class="col-task">- Kiểm tra kết nối và luồng mạng phân phối: Theo dõi luồng request từ Frontend đi qua Route 53, CloudFront, AWS WAF và đi thẳng vào API Gateway / Backend Controller. <br> - Kiểm tra khả năng định tuyến và phân phối nội dung, đảm bảo cơ chế Caching của CloudFront không làm ảnh hưởng đến tính thời gian thực của các API sinh Unit Test.</td>
      <td class="col-date">30/06/2026</td>
      <td class="col-date">30/06/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">3</td>
      <td class="col-task">- Kiểm thử luồng truy cập của hệ thống: Chạy thử luồng nghiệp vụ RAG hoàn chỉnh (Tải mã nguồn -> Chunking -> Vector DB -> Gemini AI -> Trả kết quả Unit Test). <br> - Phối hợp với các thành viên phụ trách Frontend kiểm tra các chức năng giao diện, đảm bảo lịch sử và kết quả test được hiển thị chính xác sau khi tích hợp.</td>
      <td class="col-date">01/07/2026</td>
      <td class="col-date">01/07/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">4</td>
      <td class="col-task">- Khắc phục các lỗi Backend và Cloud phát sinh trong quá trình tích hợp, ví dụ như lỗi Timeout khi Lambda gọi API Gemini, hoặc lỗi CORS giữa Frontend và Spring Boot.</td>
      <td class="col-date">02/07/2026</td>
      <td class="col-date">02/07/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">5</td>
      <td class="col-task">- Thực hiện kiểm thử lại toàn bộ hệ thống sau khi đã fix bug. <br> - Đánh giá hiệu năng truy vấn Hybrid Search của pgvector và khả năng chịu tải, đánh giá khả năng bảo mật của kiến trúc triển khai trước các request mô phỏng.</td>
      <td class="col-date">03/07/2026</td>
      <td class="col-date">03/07/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">6</td>
      <td class="col-task">- Hoàn thiện tài liệu cấu hình hạ tầng AWS, ghi chép lại các biến môi trường cần thiết cho Spring Boot và Lambda. <br> - Cập nhật sơ đồ kiến trúc theo phiên bản triển khai thực tế (bao gồm chi tiết luồng tích hợp Google Gemini API, Vector DB và AWS Serverless).</td>
      <td class="col-date">04/07/2026</td>
      <td class="col-date">04/07/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">7</td>
      <td class="col-task">- Tổng hợp kết quả triển khai toàn bộ lớp Backend và Cloud. <br> - Chuẩn bị mã nguồn, API documentation và bàn giao phần hạ tầng/Backend ổn định để phục vụ giai đoạn hoàn thiện toàn bộ đồ án chung của nhóm.</td>
      <td class="col-date">05/07/2026</td>
      <td class="col-date">05/07/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
  </tbody>
</table>


### Kết quả đạt được tuần 11:

*   Tích hợp thành công phần hạ tầng AWS (Serverless, Storage, Database, Networking) vào hệ thống mã nguồn chung của nhóm.
*   Kiểm thử thành công luồng truy cập và các luồng chức năng cốt lõi của hệ thống trên máy chủ.
*   Hoàn thiện cấu hình, khắc phục dứt điểm các lỗi phát sinh trong quá trình tích hợp.
*   Đảm bảo hệ thống hoạt động trơn tru, ổn định và bảo mật trước khi bước vào tuần hoàn thiện và nghiệm thu đồ án.