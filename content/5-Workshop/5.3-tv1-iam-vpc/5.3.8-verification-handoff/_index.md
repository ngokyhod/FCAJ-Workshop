---
title: "Verification & Handoff"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 5.3.8. </b> "
---

#### Trí checklist

- [x] VPC `zerobug-vpc` (10.0.0.0/16), 4 subnets with correct CIDR/AZ.
- [x] IGW attached (**IGW ID** copied); NAT **Available** (**NAT ID** copied).
- [x] Public route → IGW; private route → NAT.
- [x] 2 public subnets with auto-assign public IP enabled.
- [x] 4 SGs; EC2-SG has outbound All traffic.
- [x] Group `zerobug-team`, users `tri`, `kiet`, `toan`, `hoa`, `trinh`, inline PassRole policy.
- [x] 3 IAM Roles; ARNs copied to [parameter table](../../5.2-prerequisites/5.2.3-parameter-table/).

#### Handoff

| Recipient | Needs from Trí |
| --- | --- |
| **Kiệt** | Private Subnet A/B, `zerobug-rds-sg`, ARN `zerobug-ec2-role` & `zerobug-lambda-role` |
| **Toàn** | Public + Private Subnet, ALB-SG, EC2-SG, ARN `zerobug-ec2-role` |
| **Hoa** | Private Subnet, Lambda-SG, NAT Available, Lambda + StepFn Role ARN |
| **Trinh** | *(mainly needs ALB DNS from Toàn)* |

After Trí finishes → **Kiệt** continues next.
