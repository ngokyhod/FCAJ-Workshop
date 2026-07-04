---
title: "Kiểm tra & bàn giao"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 5.3.8. </b> "
---

#### Checklist Trí

- [x] VPC `zerobug-vpc` (10.0.0.0/16), 4 subnet đúng CIDR/AZ.
- [x] IGW attached (**IGW ID** đã copy); NAT **Available** (**NAT ID** đã copy).
- [x] Route public → IGW; route private → NAT.
- [x] 2 subnet public bật auto-assign public IP.
- [x] 4 SG; EC2-SG có outbound All traffic.
- [x] Group `zerobug-team`, users `tri`, `kiet`, `toan`, `hoa`, `trinh`, inline PassRole policy.
- [x] 3 IAM Role; ARN đã copy vào [bảng tham số](../../5.2-prerequisites/5.2.3-parameter-table/).

#### Bàn giao

| Nhận | Cần gì từ Trí |
| --- | --- |
| **Kiệt** | Private Subnet A/B, `zerobug-rds-sg`, ARN `zerobug-ec2-role` & `zerobug-lambda-role` |
| **Toàn** | Public + Private Subnet, ALB-SG, EC2-SG, ARN `zerobug-ec2-role` |
| **Hoa** | Private Subnet, Lambda-SG, NAT Available, Lambda + StepFn Role ARN |
| **Trinh** | *(chủ yếu cần ALB DNS từ Toàn)* |

Sau khi Trí xong → **Kiệt** làm tiếp.
