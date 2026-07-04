---
title: "Resource Cleanup – Trí"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.10.5. </b> "
---

Delete **NAT Gateway and release Elastic IP first** — this is the largest cost after WAF. VPC can only be deleted when **no** ENI/instance/load balancer/subnet group remains.

{{% notice warning %}}
**Unreleased Elastic IP** still incurs ~0.005 USD/hour even after NAT is deleted.
{{% /notice %}}

#### Step 1 — NAT Gateway

1. **VPC** → **NAT gateways** → select `zerobug-nat`.
2. **Actions** → **Delete NAT gateway** → confirm.
3. Wait for status **Deleted** (~ a few minutes).

![](/images/5-Workshop/5.10/25.png)

#### Step 2 — Elastic IP

1. **EC2** → **Elastic IPs**.
2. Select EIP attached to NAT (Allocation ID used for `zerobug-nat`).
3. **Actions** → **Release Elastic IP addresses** → **Release**.

![](/images/5-Workshop/5.10/26.png)
![](/images/5-Workshop/5.10/27.png)

#### Step 3 — Internet Gateway

1. **VPC** → **Internet gateways** → `zerobug-igw` (or IGW attached to `zerobug-vpc`).
2. **Actions** → **Detach from VPC** → select `zerobug-vpc`.
3. **Actions** → **Delete internet gateway**.

![](/images/5-Workshop/5.10/28.png)

#### Step 4 — Security Groups

**VPC** → **Security groups** (VPC = `zerobug-vpc`) — delete **in order** (avoid dependency):

1. `zerobug-rds-sg`
2. `zerobug-lambda-sg`
3. `zerobug-ec2-sg`
4. `zerobug-alb-sg`

If **dependency** error: verify RDS/EC2/Lambda/ALB are all deleted (sections 5.10.1–5.10.4).

![](/images/5-Workshop/5.10/29.png)
![](/images/5-Workshop/5.10/30.png)

#### Step 5 — Subnets

**Subnets** in `zerobug-vpc` — delete one by one:

- `zerobug-private-a`, `zerobug-private-b`
- `zerobug-public-a`, `zerobug-public-b`

*(Actual names may differ — delete all subnets in ZeroBug VPC.)*

![](/images/5-Workshop/5.10/31.png)

#### Step 6 — Route Tables

1. **Route tables** — delete **custom** route tables (private/public) with no subnet association.
2. **Main** route table for VPC — delete route `0.0.0.0/0` → IGW/NAT if still present.

![](/images/5-Workshop/5.10/32.png)

#### Step 7 — VPC

1. **Your VPCs** → `zerobug-vpc`.
2. **Actions** → **Delete VPC**.

If dependency error: **VPC** → **Resource map** / tab **Details** — find remaining ENI, subnet group, endpoint.

![](/images/5-Workshop/5.10/33.png)

#### Step 8 — IAM Roles (optional — after all services deleted)

**IAM** → **Roles** → delete:

- `zerobug-ec2-role`
- `zerobug-lambda-role`
- `zerobug-stepfn-role`

Delete **instance profile** created manually (if any) before deleting EC2 role.

![](/images/5-Workshop/5.10/34.png)

#### Confirmation checklist

- [x] NAT `zerobug-nat` = Deleted
- [x] Elastic IP **Released**
- [x] IGW detached + deleted
- [x] 4 ZeroBug Security Groups deleted
- [x] Subnets + custom route tables deleted
- [x] VPC `zerobug-vpc` deleted
- [x] *(Optional)* 3 IAM Roles `zerobug-*` deleted

→ Next: [IAM & Billing](5.10.6-iam-billing/)
