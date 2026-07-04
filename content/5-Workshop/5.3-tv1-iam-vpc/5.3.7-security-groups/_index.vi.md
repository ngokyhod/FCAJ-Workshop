---
title: "Security Groups"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 5.3.7. </b> "
---

**Security groups** → **Create security group** (VPC = `zerobug-vpc`).

**Thứ tự:** ALB-SG → EC2-SG → Lambda-SG → RDS-SG.

#### `zerobug-alb-sg`

| Inbound | Port | Source |
| --- | --- | --- |
| HTTP | 80 | `0.0.0.0/0` |
| HTTPS *(tùy chọn)* | 443 | `0.0.0.0/0` |

Outbound: mặc định All traffic.

![](/images/5-Workshop/5.3/42.png)
![](/images/5-Workshop/5.3/43.png)

#### `zerobug-ec2-sg`

| Inbound | Port | Source |
| --- | --- | --- |
| Custom TCP | 8080 | **`zerobug-alb-sg`** (chọn SG, không IP) |
| SSH *(tùy chọn)* | 22 | My IP |

| Outbound | | |
| --- | --- | --- |
| **All traffic** | All | `0.0.0.0/0` |

![](/images/5-Workshop/5.3/44.png)

{{% notice tip %}}
Nếu **Outbound rules = 0**: EC2 không gọi S3, Secrets, RDS, internet. Toàn sẽ lỗi khi deploy. Luôn thêm **All traffic → 0.0.0.0/0**.
{{% /notice %}}

#### `zerobug-lambda-sg`

- Inbound: không cần.
- Outbound: **All traffic** → `0.0.0.0/0` *(HTTPS tới Bedrock Mantle `us-east-1`)*.

![](/images/5-Workshop/5.3/45.png)

#### `zerobug-rds-sg`

| Inbound | Port | Source |
| --- | --- | --- |
| PostgreSQL | 5432 | `zerobug-ec2-sg` |
| PostgreSQL | 5432 | `zerobug-lambda-sg` |

Copy 4 **Security Group ID** vào bảng tham số.

![](/images/5-Workshop/5.3/46.png)