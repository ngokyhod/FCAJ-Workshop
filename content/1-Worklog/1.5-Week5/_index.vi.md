---
title: "Worklog Tuần 5"
date: 2026-01-01
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5:

*   Thiết lập và chuẩn bị môi trường phát triển toàn diện cho dự án trên máy cá nhân.
*   Khởi tạo cấu trúc dự án, bao gồm nền tảng giao diện và các dịch vụ Backend tương ứng.
*   Tìm hiểu dịch vụ lưu trữ Amazon S3 trên nền tảng AWS.
*   Tìm hiểu cơ chế phân phối nội dung thông qua Amazon CloudFront để chuẩn bị hạ tầng triển khai.
*   Chuẩn bị sẵn sàng các điều kiện để tích hợp hệ thống Front-end và Backend với hạ tầng AWS Cloud.

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
      <td class="col-task">- Tiếp tục trau dồi kỹ năng chuyên sâu: Duy trì việc nghiên cứu và thực hành các bài Lab quản trị hạ tầng và dịch vụ thông qua chuỗi bài giảng trên nền tảng YouTube. <br> - Tổng hợp, củng cố kiến thức kiến trúc AWS từ các tuần trước nhằm chuẩn bị tốt nhất cho việc triển khai dự án.</td>
      <td class="col-date">18/05/2026</td>
      <td class="col-date">18/05/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">2</td>
      <td class="col-task">- Thiết lập và chuẩn bị môi trường phát triển: Tiến hành cài đặt các công cụ nền tảng và cấu hình môi trường lập trình trên máy cá nhân. <br> - Định cấu hình các công cụ lập trình chuyên sâu cho mảng Backend (Java, Spring Boot, cơ sở dữ liệu) để sẵn sàng cho giai đoạn xây dựng ứng dụng cốt lõi.</td>
      <td class="col-date">19/05/2026</td>
      <td class="col-date">19/05/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">3</td>
      <td class="col-task">- Khởi tạo cấu trúc dự án: Bắt đầu xây dựng nền tảng cho hệ thống giao diện người dùng. <br> - Khởi tạo bộ khung mã nguồn Backend, thiết lập các file cấu hình ban đầu (như application.yml, pom.xml) và kết nối cơ sở dữ liệu môi trường dev.</td>
      <td class="col-date">20/05/2026</td>
      <td class="col-date">20/05/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">4</td>
      <td class="col-task">- Tối ưu hóa quy trình: Cấu hình cấu trúc thư mục mã nguồn một cách khoa học cho cả hai phía ứng dụng. <br> - Chuẩn bị sẵn sàng các điều kiện, thư viện cần thiết để tích hợp Front-end với các dịch vụ Backend và hạ tầng AWS Cloud trong các giai đoạn tiếp theo của dự án.</td>
      <td class="col-date">21/05/2026</td>
      <td class="col-date">21/05/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">5</td>
      <td class="col-task">- Tìm hiểu tổng quan về dịch vụ Amazon S3, nghiên cứu khái niệm S3 Bucket, Object Storage và vai trò của S3 trong hệ sinh thái AWS. <br> - Lên phương án lưu trữ tài nguyên tĩnh và mã nguồn (Static Website Hosting) cho dự án thông qua Amazon S3.</td>
      <td class="col-date">22/05/2026</td>
      <td class="col-date">22/05/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">6</td>
      <td class="col-task">- Nghiên cứu cơ chế Public Access Control và Object Ownership để thiết lập quyền truy cập an toàn trên S3. <br> - Thực hành các thiết lập bảo mật mặc định, chuẩn bị kịch bản tự động tải mã nguồn hoặc các file cấu hình lên Amazon S3.</td>
      <td class="col-date">23/05/2026</td>
      <td class="col-date">23/05/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">7</td>
      <td class="col-task">- Tìm hiểu cơ chế phân phối nội dung (CDN) thông qua Amazon CloudFront. <br> - Rà soát lại toàn bộ môi trường phát triển Local và đánh giá phương án dùng CloudFront để tăng tốc truy cập cho hệ thống khi kết hợp cùng API Backend và S3.</td>
      <td class="col-date">24/05/2026</td>
      <td class="col-date">24/05/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
  </tbody>
</table>


### Kết quả đạt được tuần 5:

*   Cài đặt và định cấu hình thành công môi trường lập trình trên máy cá nhân cho cả Frontend và Backend.
*   Khởi tạo xong cấu trúc dự án và quy hoạch thư mục mã nguồn Backend đồng bộ, tối ưu.
*   Chuẩn bị đầy đủ các điều kiện cần thiết để tích hợp mạch lạc giữa Front-end, các dịch vụ Backend và hạ tầng AWS Cloud.
*   Nắm vững nguyên lý hoạt động của dịch vụ lưu trữ Amazon S3 và Amazon CloudFront.