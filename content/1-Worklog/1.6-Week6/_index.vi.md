---
title: "Worklog Tuần 6"
date: 2026-01-01
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

*   Tìm hiểu chuyên sâu dịch vụ Amazon Route 53 và các giải pháp quản lý DNS trên đám mây.
*   Ứng dụng giải pháp phân giải tên miền nội bộ để tối ưu hóa và bảo mật kết nối cho hệ thống Backend và Database.
*   Thực hành triển khai hạ tầng bằng CloudFormation và cấu hình Hybrid DNS.
*   Tổng hợp và hệ thống hóa toàn bộ các kiến thức AWS đã học trong 05 tuần trước (IAM, VPC, EC2, RDS, S3, CloudFront).
*   Xây dựng phương án áp dụng các hạ tầng AWS vào kiến trúc triển khai thực tế của đồ án.

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
      <td class="col-task">- Rà soát và tổng hợp kiến thức về các dịch vụ AWS đã học, ôn tập các khái niệm nền tảng về điện toán đám mây và kiến trúc AWS Global Infrastructure. <br> - Tìm hiểu vai trò của Amazon Route 53 – dịch vụ DNS quản lý đám mây có khả năng mở rộng cao và độ sẵn sàng tuyệt đối.</td>
      <td class="col-date">25/05/2026</td>
      <td class="col-date">25/05/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">2</td>
      <td class="col-task">- Ôn tập các nội dung về quản lý danh tính và phân quyền (IAM User, IAM Group, IAM Role) để thiết lập quyền hạn an toàn cho các dịch vụ Backend. <br> - Phân tích sự khác biệt giữa các loại DNS Query: Recursive queries (truy vấn đệ quy) và Iterative queries (truy vấn lặp) trong hệ thống phân giải tên miền.</td>
      <td class="col-date">26/05/2026</td>
      <td class="col-date">26/05/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">3</td>
      <td class="col-task">- Ôn lại kiến thức về hạ tầng mạng AWS, rà soát lại quy trình xây dựng VPC, Public/Private Subnet, Internet Gateway, NAT Gateway và Security Group. <br> - Phân tích lợi ích của việc tập trung hóa quản trị DNS: Giảm thiểu sự phức tạp khi duy trì nhiều máy chủ DNS rời rạc ở các môi trường khác nhau, giúp dễ dàng kiểm soát luồng mạng cho các API Backend.</td>
      <td class="col-date">27/05/2026</td>
      <td class="col-date">27/05/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">4</td>
      <td class="col-task">- Ôn tập kiến thức về Amazon EC2 và Amazon RDS, tổng hợp các phương pháp tối ưu tài nguyên, sao lưu (Backup) và khôi phục (Restore) dữ liệu. <br> - Ứng dụng lý thuyết DNS vào hệ thống: Đảm bảo tính nhất quán (Consistency) trong việc gọi tên các dịch vụ Backend/Database bằng tên miền nội bộ (.internal) thay vì địa chỉ IP tĩnh.</td>
      <td class="col-date">28/05/2026</td>
      <td class="col-date">28/05/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">5</td>
      <td class="col-task">- Tăng cường bảo mật mạng thông qua việc truyền tải dữ liệu DNS qua các kết nối riêng (Private Connections) thay vì Internet công cộng nhằm ẩn giấu hoàn toàn hệ thống Backend. <br> - Nghiên cứu và thực hành theo các bài Lab: Tạo Key Pair, khởi tạo mẫu hạ tầng tự động (Initialize CloudFormation Template) và cấu hình Security Group.</td>
      <td class="col-date">29/05/2026</td>
      <td class="col-date">29/05/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">6</td>
      <td class="col-task">- Ôn tập các kiến thức về Amazon S3 và Amazon CloudFront, đánh giá vai trò của các dịch vụ này trong việc lưu trữ và phân phối nội dung tĩnh cho Frontend. <br> - Nghiên cứu bài Lab thiết lập hệ thống phân giải tên miền lai (Set up Hybrid DNS with Route 53 Resolver) và thực hành kết nối an toàn vào hạ tầng (Connect to RDGW).</td>
      <td class="col-date">30/05/2026</td>
      <td class="col-date">30/05/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">7</td>
      <td class="col-task">- Tổng hợp toàn bộ kiến thức đã học trong 06 tuần. <br> - Đối chiếu các dịch vụ AWS đã học với yêu cầu xử lý nghiệp vụ Backend và quản trị cơ sở dữ liệu của đồ án nhóm. <br> - Xây dựng phương án áp dụng các hạ tầng AWS vào kiến trúc triển khai của hệ thống và lập kế hoạch nghiên cứu các công nghệ tích hợp AI còn thiếu.</td>
      <td class="col-date">31/05/2026</td>
      <td class="col-date">31/05/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
  </tbody>
</table>


### Kết quả đạt được tuần 6:

*   Hệ thống hóa toàn bộ kiến thức đã học về các dịch vụ AWS lõi (IAM, VPC, EC2, RDS, S3, CloudFront).
*   Hiểu sâu về Amazon Route 53, các loại truy vấn DNS và phương pháp bảo mật kết nối bằng DNS nội bộ (.internal) cho Backend.
*   Nắm bắt được cách tự động hóa khởi tạo hạ tầng bằng CloudFormation và cấu hình hệ thống Hybrid DNS.
*   Xây dựng thành công phương án tổng thể để áp dụng hạ tầng AWS vào kiến trúc hệ thống của đồ án nhóm.