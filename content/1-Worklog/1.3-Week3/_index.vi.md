---
title: "Worklog Tuần 3"
date: 2026-01-01
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3:

* Tiếp tục nghiên cứu và thực hành các bài Lab trên nền tảng AWS.
* Tìm hiểu dịch vụ quản lý danh tính và phân quyền (IAM).
* Nghiên cứu kiến trúc mạng trên AWS thông qua VPC, EC2 và Site-to-Site VPN.
* Thực hành triển khai, cấu hình và quản trị các tài nguyên hạ tầng trên AWS.

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
      <td class="col-task">- Thực hiện Lab Create IAM Group and IAM User <br> - Tạo nhóm quản trị (Admin Group) và tài khoản Admin User <br> - Thực hành đăng nhập bằng IAM User thay vì tài khoản Root</td>
      <td class="col-date">04/05/2026</td>
      <td class="col-date">04/05/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">2</td>
      <td class="col-task">- Thực hiện Lab Create IAM Role and IAM User <br> - Tạo IAM Role với quyền quản trị và tài khoản OperatorUser <br> - Tìm hiểu cơ chế phân quyền thông qua IAM Role</td>
      <td class="col-date">05/05/2026</td>
      <td class="col-date">05/05/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">3</td>
      <td class="col-task">- Thực hiện Lab Switch Role <br> - Cấu hình quyền cho OperatorUser chuyển đổi vai trò <br> - Thực hành truy cập AWS Console bằng cơ chế Switch Role</td>
      <td class="col-date">06/05/2026</td>
      <td class="col-date">06/05/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">4</td>
      <td class="col-task">- Tìm hiểu Firewall trong VPC <br> - Thực hành cấu hình Security Group <br> - Tìm hiểu Network ACLs và VPC Resource Map</td>
      <td class="col-date">07/05/2026</td>
      <td class="col-date">07/05/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">5</td>
      <td class="col-task">- Thực hiện các bước chuẩn bị hạ tầng mạng: <br>&emsp; + Tạo VPC, Subnet, Internet Gateway <br>&emsp; + Tạo Route Table, Security Group <br>&emsp; + Kích hoạt VPC Flow Logs</td>
      <td class="col-date">08/05/2026</td>
      <td class="col-date">08/05/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">6</td>
      <td class="col-task">- Thực hiện Lab Deploying Amazon EC2 Instances <br> - Tạo và cấu hình máy chủ EC2, kiểm tra kết nối <br> - Tìm hiểu NAT Gateway, Reachability Analyzer, EC2 Instance Connect <br> - Thiết lập CloudWatch Monitoring & Alerting</td>
      <td class="col-date">09/05/2026</td>
      <td class="col-date">09/05/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">7</td>
      <td class="col-task">- Thực hiện Lab Setting Up Site-to-Site VPN Connection in AWS <br> - Tạo Virtual Private Gateway, Customer Gateway và VPN Connection <br> - Cấu hình VPN Tunnel và ôn tập kiến thức trong tuần</td>
      <td class="col-date">10/05/2026</td>
      <td class="col-date">10/05/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
  </tbody>
</table>


### Kết quả đạt được tuần 3:

* Hiểu được cơ chế quản lý danh tính và phân quyền thông qua IAM User, IAM Group và IAM Role.
* Thực hành thành công việc chuyển đổi vai trò (Switch Role) trong AWS.
* Nắm được các thành phần mạng cơ bản trong AWS như VPC, Subnet, Route Table, Security Group và Network ACL.
* Triển khai thành công máy chủ Amazon EC2 và thực hiện giám sát bằng CloudWatch.
* Tìm hiểu và cấu hình kết nối Site-to-Site VPN trên AWS.
* Củng cố kiến thức về quản trị hạ tầng và bảo mật mạng trên nền tảng điện toán đám mây AWS.
