---
title: "Trí – IAM & VPC Foundation"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

**Owner:** Trí  
**Do this first** — complete [5.2 Common Prerequisites](../5.2-prerequisites/) before starting.

#### What does Trí do?

Build the **security and network foundation** for the entire ZeroBug Agent on **one shared VPC**: IAM users/roles, a two-tier subnet VPC, NAT Gateway, route tables, and security groups.

#### Components & where they are used

| Component | Purpose | Used by |
| --- | --- | --- |
| **IAM Users `tri`–`trinh`** | Secure Console sign-in | Whole team on 1 account |
| **IAM Roles (EC2/Lambda/StepFn)** | Runtime permissions per service | EC2 (Toàn), Lambda + Step Fn (Hoa) |
| **VPC + Subnet** | Public (ALB) + Private (EC2, RDS, Lambda) | Entire ZeroBug infrastructure |
| **NAT Gateway** | Private subnet internet egress | Lambda VPC (`ContextBuilderLambda`, `BedrockInvokeLambda`) → Mantle `us-east-1`; EC2 JAR download / SSM |
| **Security Groups** | Traffic control | ALB ↔ EC2 ↔ RDS ↔ Lambda |

#### Role in the production flow

```
Client → CloudFront → WAF → API Gateway → ALB (Public) → EC2 / RDS / Lambda (Private)
```

#### Network design

| Component | Value |
| --- | --- |
| VPC CIDR | `10.0.0.0/16` — `zerobug-vpc` |
| Public Subnet A / B | `10.0.1.0/24` (1a), `10.0.2.0/24` (1b) |
| Private Subnet A / B | `10.0.11.0/24` (1a), `10.0.12.0/24` (1b) |

{{% notice warning %}}
**NAT Gateway is not free tier** (~32 USD/month). Create only **1 NAT**; delete when finished (section 5.10).
{{% /notice %}}

#### Trí-specific preparation

- Complete the checklist in [5.2.4](../5.2-prerequisites/5.2.4-conventions-and-order/).
- **Shortcut (optional):** VPC wizard **"VPC and more"** — if used, verify names/CIDR against the table above. The steps below guide the **manual approach** for controlled naming at handoff.

#### Implementation content

1. [Create IAM Users/Groups](5.3.1-iam-users-groups/)
2. [Create shared IAM Roles](5.3.2-iam-roles/)
3. [Create VPC](5.3.3-vpc/)
4. [Create Public & Private Subnets](5.3.4-subnets/)
5. [Internet Gateway & NAT Gateway](5.3.5-igw-nat/)
6. [Configure Route Tables](5.3.6-route-tables/)
7. [Create Security Groups](5.3.7-security-groups/)
8. [Verification & handoff](5.3.8-verification-handoff/)
