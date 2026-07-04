---
title: "Dọn tài nguyên - Trí"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.10.5. </b> "
---

Xóa **NAT Gateway và release Elastic IP trước** — đây là khoản phí lớn nhất sau WAF. VPC chỉ xóa được khi **không còn** ENI/instance/load balancer/subnet group.

{{% notice warning %}}
**Elastic IP chưa release** vẫn tính phí ~0.005 USD/giờ dù NAT đã xóa.
{{% /notice %}}

#### Bước 1 — NAT Gateway

1. **VPC** → **NAT gateways** → chọn `zerobug-nat`.
2. **Actions** → **Delete NAT gateway** → xác nhận.
3. Đợi status **Deleted** (~ vài phút).

![](/images/5-Workshop/5.10/25.png)

#### Bước 2 — Elastic IP

1. **EC2** → **Elastic IPs**.
2. Chọn EIP gắn NAT (Allocation ID từng dùng cho `zerobug-nat`).
3. **Actions** → **Release Elastic IP addresses** → **Release**.

![](/images/5-Workshop/5.10/26.png)
![](/images/5-Workshop/5.10/27.png)

#### Bước 3 — Internet Gateway

1. **VPC** → **Internet gateways** → `zerobug-igw` (hoặc IGW gắn `zerobug-vpc`).
2. **Actions** → **Detach from VPC** → chọn `zerobug-vpc`.
3. **Actions** → **Delete internet gateway**.

![](/images/5-Workshop/5.10/28.png)

#### Bước 4 — Security Groups

**VPC** → **Security groups** (VPC = `zerobug-vpc`) — xóa **theo thứ tự** (tránh dependency):

1. `zerobug-rds-sg`
2. `zerobug-lambda-sg`
3. `zerobug-ec2-sg`
4. `zerobug-alb-sg`

Nếu báo **dependency**: kiểm tra RDS/EC2/Lambda/ALB đã xóa hết (mục 5.10.1–5.10.4).

![](/images/5-Workshop/5.10/29.png)
![](/images/5-Workshop/5.10/30.png)

#### Bước 5 — Subnets

**Subnets** trong `zerobug-vpc` — xóa lần lượt:

- `zerobug-private-a`, `zerobug-private-b`
- `zerobug-public-a`, `zerobug-public-b`

*(Tên thực tế có thể khác — xóa mọi subnet thuộc VPC ZeroBug.)*

![](/images/5-Workshop/5.10/31.png)

#### Bước 6 — Route Tables

1. **Route tables** — xóa route table **custom** (private/public) không còn subnet associate.
2. **Main** route table của VPC — xóa route `0.0.0.0/0` → IGW/NAT nếu còn.

![](/images/5-Workshop/5.10/32.png)

#### Bước 7 — VPC

1. **Your VPCs** → `zerobug-vpc`.
2. **Actions** → **Delete VPC**.

Nếu lỗi dependency: **VPC** → **Resource map** / tab **Details** — tìm ENI, subnet group, endpoint còn sót.

![](/images/5-Workshop/5.10/33.png)

#### Bước 8 — IAM Roles (tùy chọn — sau khi mọi dịch vụ đã xóa)

**IAM** → **Roles** → xóa:

- `zerobug-ec2-role`
- `zerobug-lambda-role`
- `zerobug-stepfn-role`

Xóa **instance profile** tự tạo (nếu có) trước khi xóa role EC2.

![](/images/5-Workshop/5.10/34.png)

#### Checklist xác nhận

- [x] NAT `zerobug-nat` = Deleted
- [x] Elastic IP đã **Release**
- [x] IGW detached + deleted
- [x] 4 Security Groups ZeroBug đã xóa
- [x] Subnets + custom route tables đã xóa
- [x] VPC `zerobug-vpc` đã xóa
- [x] *(Tùy chọn)* 3 IAM Role `zerobug-*` đã xóa

→ Tiếp: [IAM & Billing](5.10.6-iam-billing/)
