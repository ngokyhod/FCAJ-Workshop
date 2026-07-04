---
title: "Worklog Tuần 4"
date: 2026-01-01
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4:

*   Thực hành tự thiết lập hạ tầng mạng AWS Virtual Private Cloud (VPC) tùy chỉnh thay vì sử dụng mạng mặc định.
*   Thiết kế và cấu hình kiến trúc mạng phân tầng bảo mật (Public và Private Subnet).
*   Triển khai, gắn kết các luồng định tuyến mạng nâng cao thông qua Internet Gateway (IGW) và NAT Gateway.
*   Thiết lập tường lửa bảo mật tầng máy chủ (Security Groups) và kiểm thử khả năng kết nối của hệ thống.

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
      <td class="col-task">- Nghiên cứu và thực hành Module 02-Lab03-03.1 - Create VPC. <br> - Tiến hành khởi tạo thành công VPC tùy chỉnh với dải địa chỉ IP tổng là 10.0.0.0/16.</td>
      <td class="col-date">11/05/2026</td>
      <td class="col-date">11/05/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">2</td>
      <td class="col-task">- Thực hành Module 02-Lab03-03.2 - Create Subnet. <br> - Thực hiện phân chia kiến trúc mạng phân tầng bảo mật bao gồm các phân khu: Public Subnet và Private Subnet.</td>
      <td class="col-date">12/05/2026</td>
      <td class="col-date">12/05/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">3</td>
      <td class="col-task">- Thực hành Module 02-Lab03-03.3 - Create an Internet Gateway. <br> - Thực hành Module 02-Lab03-03.4 - Create Route Table for Outbound Internet Routing via Internet Gateway. <br> - Triển khai và gắn kết Internet Gateway (IGW) vào VPC để mở đường ra Internet cho vùng Public thông qua bảng định tuyến Public Route Table (0.0.0.0/0).</td>
      <td class="col-date">13/05/2026</td>
      <td class="col-date">13/05/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">4</td>
      <td class="col-task">- Thực hành Module 02-Lab03-04.3 - Create NAT Gateway. <br> - Nghiên cứu bản chất và các trường hợp sử dụng thực tế để thực hành cấu hình NAT Gateway. <br> - Tiến hành cấu hình luồng định tuyến một chiều cho Private Route Table hướng ra NAT Gateway.</td>
      <td class="col-date">14/05/2026</td>
      <td class="col-date">14/05/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">5</td>
      <td class="col-task">- Thực hành Module 02-Lab03-03.5 - Create security groups. <br> - Nghiên cứu các quy tắc Inbound/Outbound và tiến hành thiết lập chính sách bảo mật tầng Instance (Security Groups).</td>
      <td class="col-date">15/05/2026</td>
      <td class="col-date">15/05/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">6</td>
      <td class="col-task">- Thực hành Module 02-Lab03-04.1 - Create EC2 Instances in Subnets. <br> - Khởi tạo các máy chủ ảo Amazon EC2 và phân bổ vào đúng các Subnet (Public/Private).</td>
      <td class="col-date">16/05/2026</td>
      <td class="col-date">16/05/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">7</td>
      <td class="col-task">- Thực hành Module 02-Lab03-04.2 - Test connection. <br> - Thực hành Module 02-Lab03-04.5 - EC2 Instance Connect Endpoint. <br> - Thực hiện kiểm tra Ping, SSH vào các EC2 Instance để xác thực khả năng kết nối.</td>
      <td class="col-date">17/05/2026</td>
      <td class="col-date">17/05/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
  </tbody>
</table>


### Kết quả đạt được tuần 4:

*   Khởi tạo thành công VPC tùy chỉnh (lab-vpc-01) với dải địa chỉ tổng là 10.0.0.0/16.
*   Phân chia thành công kiến trúc mạng phân tầng bảo mật bao gồm Public Subnet và Private Subnet.
*   Triển khai và gắn kết chính xác Internet Gateway (IGW) vào VPC cho vùng Public.
*   Cấu hình thành công NAT Gateway và luồng định tuyến một chiều cho Private Route Table, bảo vệ an toàn cho hệ thống máy chủ nội bộ.
*   Thiết lập hoàn thiện các chính sách bảo mật tầng Instance (Security Groups) và kiểm thử kết nối hệ thống thành công.