---
title: "AWS account"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.2.1. </b> "
---

#### Use 1 shared AWS account

All 5 members **must** use **the same AWS account** because the parts reference each other directly:

- VPC / Subnet / Security Group — RDS, EC2, Lambda must share the same VPC.
- IAM Role — no cross-account usage (unless you configure complex cross-account setup).
- Secrets Manager, S3 — in-account ARNs.
- ALB DNS, Step Functions ARN — in-account integration.

| Issue | Solution |
| --- | --- |
| Permissions | Separate IAM Users `tri`, `kiet`, `toan`, `hoa`, `trinh`, group `zerobug-team` |
| Name collisions | Prefix `zerobug-*` |
| Cost | 1 AWS Budgets alert |
| Handoff | 1 shared parameter table (section 5.2.3) |

#### If you already used Root

- **Do not break** or **redo** existing resources.
- Enable **MFA for Root** immediately.
- **Do not** create Access Keys for Root.
- Create IAM Users → sign in with IAM Users from then on.

#### Choose Region

1. [AWS Console](https://console.aws.amazon.com/) → top-right corner.
2. Select **Asia Pacific (Singapore) `ap-southeast-1`**.

{{% notice note %}}
WAF attached to CloudFront uses **ap-southeast-1**. IAM is global.
{{% /notice %}}

<!-- Image: /images/5-Workshop/5.2-Prerequisite/region.png -->
