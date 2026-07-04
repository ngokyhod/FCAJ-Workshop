---
title: "Worklog Tuần 9"
date: 2026-01-01
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Mục tiêu tuần 9:

*   Tối ưu hóa truy vấn cơ sở dữ liệu véc-tơ bằng pgvector.
*   Hoàn thiện luồng điều phối dữ liệu tích hợp giữa Chunking, Embedding và Database.
*   Chuẩn bị môi trường Agentic sử dụng Google Gemini API để tạo Unit Test.
*   Chuẩn bị triển khai kiến trúc hạ tầng mạng bảo mật và phân phối nội dung với Amazon Route 53, CloudFront và AWS WAF.

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
      <td class="col-task">- Rà soát lại toàn bộ kiến trúc triển khai hạ tầng của đồ án, đối chiếu các thành phần Backend hiện tại với sơ đồ hạ tầng AWS dự kiến triển khai. <br> - Nghiên cứu cơ chế Truy vấn tìm kiếm lai trong cơ sở dữ liệu véc-tơ để tối ưu hóa độ chính xác khi tìm kiếm mã nguồn.</td>
      <td class="col-date">15/06/2026</td>
      <td class="col-date">15/06/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">2</td>
      <td class="col-task">- Viết các câu lệnh SQL chuyên sâu kết hợp với thư viện pgvector trong Spring Boot. <br> - Áp dụng phép đo khoảng cách Cosine Similarity của pgvector để truy xuất và xếp hạng các khối mã nguồn có ý nghĩa tương đồng nhất với truy vấn của người dùng.</td>
      <td class="col-date">16/06/2026</td>
      <td class="col-date">16/06/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">3</td>
      <td class="col-task">- Hoàn thiện luồng Điều phối dữ liệu VectorStoreService ở tầng Backend. <br> - Lắp ráp các module Phân mảnh mã nguồn và Nhúng dữ thành một chu trình xử lý khép kín và tự động.</td>
      <td class="col-date">17/06/2026</td>
      <td class="col-date">17/06/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">4</td>
      <td class="col-task">- Nghiên cứu quy trình cấu hình các dịch vụ phục vụ lớp phân phối nội dung và bảo mật hạ tầng mạng trên AWS. <br> - Phân tích mô hình triển khai Amazon Route 53, Amazon CloudFront và các luật bảo mật của AWS WAF để bảo vệ API Gateway cho các luồng xử lý Backend.</td>
      <td class="col-date">18/06/2026</td>
      <td class="col-date">18/06/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">5</td>
      <td class="col-task">- Kiểm thử hệ thống luồng dữ liệu Backend: Xây dựng các API nội bộ để mô phỏng và đánh giá End-to-End luồng nạp mã nguồn. <br> - Chạy thực nghiệm để xác nhận dữ liệu véc-tơ từ Gemini API được tạo ra định dạng chuẩn và lưu trữ chính xác vào cơ sở dữ liệu PostgreSQL.</td>
      <td class="col-date">20/06/2026</td>
      <td class="col-date">20/06/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">6</td>
      <td class="col-task">- Chuẩn bị môi trường Agentic: Lên phương án tích hợp và xây dựng cấu trúc Prompt cho Google Gemini API để hệ thống có thể tiếp nhận dữ liệu ngữ cảnh vừa truy xuất từ Vector Database. Đây là bước đệm quan trọng cho giai đoạn tự động sinh Unit Test. <br> - Tổng hợp tài liệu thiết kế và chuẩn bị môi trường để bắt đầu cấu hình thực tế các dịch vụ hạ tầng AWS vào tuần tiếp theo.</td>
      <td class="col-date">21/06/2026</td>
      <td class="col-date">21/06/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
  </tbody>
</table>


### Kết quả đạt được tuần 9:

*   Lập trình thành công tính năng tìm kiếm ngữ cảnh dựa trên Cosine Similarity của pgvector, đảm bảo AI tìm được đúng đoạn code liên quan.
*   Hoàn thiện chu trình VectorStoreService với khả năng xử lý Chunking, gọi Embedding API và Batch Insert mượt mà.
*   Xây dựng thành công hệ thống API nội bộ để kiểm thử luồng dữ liệu, xác nhận dữ liệu véc-tơ được lưu trữ chính xác.
*   Lên được phương án triển khai bảo mật AWS WAF, Route 53 và CloudFront để bảo vệ Backend.
*   Sẵn sàng luồng dữ liệu Context để đưa vào Google Gemini sinh mã Unit Test tự động.