---
title: "Worklog Tuần 8"
date: 2026-01-01
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8:

*   Thiết lập hạ tầng Vector Database (PostgreSQL với pgvector).
*   Cấu hình ORM Hibernate để hỗ trợ định dạng dữ liệu véc-tơ.
*   Tích hợp bảo mật và kết nối ứng dụng Spring Boot với hạ tầng AWS.
*   Triển khai quy trình nhúng dữ liệu sử dụng Gemini API.

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
      <td class="col-task">- Thiết lập hạ tầng Vector Database: Tiến hành cài đặt và khởi chạy máy chủ PostgreSQL tích hợp extension pgvector. <br> - Cấu hình môi trường để Backend Spring Boot kết nối được với container cơ sở dữ liệu.</td>
      <td class="col-date">08/06/2026</td>
      <td class="col-date">08/06/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">2</td>
      <td class="col-task">- Cấu hình ORM Hibernate trong Spring Boot: Xây dựng các Entity lớp @Entity hỗ trợ kiểu dữ liệu mảng. <br> - Định nghĩa cấu hình cho cột embedding với định dạng vector(1024) để đảm bảo tính tương thích khi lưu trữ các ma trận véc-tơ sau này.</td>
      <td class="col-date">09/06/2026</td>
      <td class="col-date">09/06/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">3</td>
      <td class="col-task">- Kiểm tra tính sẵn sàng của các client dịch vụ (S3, RDS) để đảm bảo Backend có đầy đủ quyền truy cập tài nguyên cloud.</td>
      <td class="col-date">10/06/2026</td>
      <td class="col-date">10/06/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">4</td>
      <td class="col-task">- Chuyển đổi chiến lược nhúng dữ liệu: Sửa đổi logic để thay thế mô hình Titan Embedding cũ bằng việc gọi trực tiếp Google Gemini API. <br> - Viết Service xử lý kết nối, gửi dữ liệu văn bản từ Document Chunks qua API Gemini để lấy về các ma trận véc-tơ tương ứng.</td>
      <td class="col-date">11/06/2026</td>
      <td class="col-date">11/06/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">5</td>
      <td class="col-task">- Phát triển logic lưu trữ véc-tơ: Lập trình hàm lưu trữ kết quả Embedding từ Gemini API vào cột vector(1024) trong PostgreSQL. <br> - Kiểm thử đơn vị (Unit Test) cho luồng dữ liệu từ lúc đọc file đến khi lưu trữ thành công vào Database.</td>
      <td class="col-date">12/06/2026</td>
      <td class="col-date">12/06/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">6</td>
      <td class="col-task">- Tối ưu hóa dữ liệu: Xử lý các trường hợp văn bản vượt quá giới hạn token của API Gemini bằng cách cắt nhỏ trước khi gửi đi. <br> - Đảm bảo định dạng đầu ra của véc-tơ khớp hoàn toàn với cấu trúc yêu cầu của pgvector ([0.1, 0.2, ...]).</td>
      <td class="col-date">13/06/2026</td>
      <td class="col-date">13/06/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">7</td>
      <td class="col-task">- Tổng hợp và kiểm tra luồng nhúng dữ liệu: Thực hiện chạy thử nghiệm quy trình nạp dữ liệu từ máy cá nhân lên cơ sở dữ liệu véc-tơ. <br> - Xác nhận dữ liệu đã được nhúng và lưu trữ chính xác, sẵn sàng cho giai đoạn thực hiện truy vấn Cosine Similarity ở tuần tiếp theo.</td>
      <td class="col-date">14/06/2026</td>
      <td class="col-date">14/06/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
  </tbody>
</table>


### Kết quả đạt được tuần 8:

*   Triển khai thành công PostgreSQL với extension pgvector, tối ưu chi phí và tài nguyên phát triển cục bộ.
*   Cấu hình thành công Hibernate để Spring Boot tương tác được với kiểu dữ liệu vector(1024).
*   Chuyển đổi thành công sang sử dụng API của Google Gemini để thực hiện nhúng dữ liệu cho các khối mã nguồn.
*   Hệ thống hóa quy trình nhúng và lưu trữ véc-tơ, đảm bảo dữ liệu mã nguồn đã sẵn sàng để chuyển sang bước truy vấn tương đồng ở tuần tới.