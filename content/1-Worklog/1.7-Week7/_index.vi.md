---
title: "Worklog Tuần 7"
date: 2026-01-01
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:

*   Nghiên cứu kiến trúc RAG nâng cao chuyên biệt về kiểm thử phần mềm.
*   Xây dựng module xử lý dữ liệu và phân mảnh mã nguồn ở phía Backend.
*   Hoàn thành bản thiết kế mô hình kiến trúc hệ thống tổng thể của đồ án.
*   Tiếp tục nghiên cứu, ôn tập các dịch vụ AWS đã học và lập kế hoạch triển khai, phân chia giai đoạn thực hiện đồ án.

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
      <td class="col-task">- Ôn tập các kiến thức về Amazon EC2, Amazon RDS, Amazon S3 và Amazon CloudFront nhằm đánh giá khả năng sử dụng các dịch vụ AWS trong quá trình triển khai đồ án. <br> - Nghiên cứu kiến trúc RAG nâng cao: Tìm hiểu phương pháp tối ưu hóa dữ liệu đầu vào cho AI Agent chuyên biệt về lĩnh vực kiểm thử phần mềm.</td>
      <td class="col-date">01/06/2026</td>
      <td class="col-date">01/06/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">2</td>
      <td class="col-task">- Phân tích yêu cầu chức năng và yêu cầu phi chức năng của đồ án để xác định các thành phần hệ thống cần triển khai trên nền tảng AWS. <br> - Xây dựng module Tiêu hóa dữ liệu ở phía Backend: Tích hợp thư viện JavaParser để thực hiện phân tích tĩnh mã nguồn.</td>
      <td class="col-date">02/06/2026</td>
      <td class="col-date">02/06/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">3</td>
      <td class="col-task">- Nghiên cứu mô hình triển khai hệ thống trên AWS, xác định vai trò của từng dịch vụ (Amazon EC2 triển khai Backend, Amazon RDS lưu trữ cơ sở dữ liệu, S3 lưu trữ tĩnh, v.v.). <br> - Triển khai chiến lược Phân mảnh mã nguồn: Bắt đầu lập trình xử lý cắt mã nguồn dựa trên Cây cú pháp trừu tượng thay vì cắt theo số lượng ký tự thông thường để đảm bảo tính logic của code.</td>
      <td class="col-date">03/06/2026</td>
      <td class="col-date">03/06/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">4</td>
      <td class="col-task">- Trích xuất siêu dữ liệu : Lập trình trích xuất chi tiết đến cấp độ hàm, thu thập đồng thời các thông tin cấu trúc quan trọng như Annotations, Constructors, Imports và Dependencies để đảm bảo AI không bị mất ngữ cảnh khi phân tích. <br> - Xây dựng phương án triển khai hạ tầng cho đồ án, đánh giá khả năng mở rộng, tính bảo mật và tính sẵn sàng của hệ thống.</td>
      <td class="col-date">04/06/2026</td>
      <td class="col-date">04/06/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">5</td>
      <td class="col-task">- Hoàn thành thiết kế mô hình kiến trúc hệ thống tổng thể dựa trên các luồng xử lý Frontend, Backend, Database và AI Service đã phân tích. <br> - Lập kế hoạch triển khai đồ án, xác định các giai đoạn thực hiện: Thiết kế hệ thống, Xây dựng Backend, Xây dựng Frontend, Triển khai hạ tầng AWS, Kiểm thử và tối ưu hệ thống.</td>
      <td class="col-date">05/06/2026</td>
      <td class="col-date">05/06/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">6</td>
      <td class="col-task">- Phân chia nhiệm vụ giữa các thành viên trong nhóm, xác định tiến độ thực hiện và các mốc hoàn thành của từng giai đoạn. Tập trung chốt các task liên quan đến xử lý dữ liệu Backend và kết nối AI.</td>
      <td class="col-date">06/06/2026</td>
      <td class="col-date">06/06/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">7</td>
      <td class="col-task">- Tổng hợp kết quả nghiên cứu module RAG và Code Chunking. <br> - Hoàn thiện kế hoạch triển khai đồ án và chuẩn bị cho giai đoạn thiết kế kiến trúc hệ thống chi tiết hơn.</td>
      <td class="col-date">07/06/2026</td>
      <td class="col-date">07/06/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
  </tbody>
</table>


### Kết quả đạt được tuần 7:

*   Hiểu và triển khai bước đầu thành công module Data Ingestion sử dụng thư viện JavaParser và chiến lược Code Chunking bằng AST.
*   Xây dựng được cơ chế trích xuất Metadata chuyên phục vụ cho Advanced RAG.
*   Hoàn thành bản vẽ mô hình kiến trúc tổng thể của hệ thống.
*   Hệ thống hóa phương án áp dụng các dịch vụ AWS vào đồ án, xây dựng kế hoạch triển khai và phân chia nhiệm vụ rõ ràng cho nhóm.