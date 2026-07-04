---
title: "Resource Cleanup"
date: 2024-01-01
weight: 10
chapter: false
pre: " <b> 5.10. </b> "
---

**Required after workshop** — delete resources in **reverse** deployment order to avoid dependency errors and **save costs** (NAT, WAF, RDS, EC2 billed hourly).

#### Deletion order (entire team)

```
Trinh (CloudFront, WAF, API GW, Cognito)
  → Hoa (Step Functions → 6 Lambda → log groups)
    → Toàn (ALB, Target Group, EC2)
      → Kiệt (RDS → subnet group → Secrets → S3 empty + delete bucket)
        → Trí (NAT → EIP → IGW → SG → subnet → VPC → IAM roles)
          → Shared: IAM users/group, Budgets, revoke Bedrock API key
```

| Step | Member | Main resources | Notes |
| --- | --- | --- | --- |
| 1 | Trinh | CloudFront, WAF, API Gateway, Cognito | WAF Global — wait for disassociate |
| 2 | Hoa | State machine, 6 Lambda PascalCase | Delete Step Fn **before** Lambda |
| 3 | Toàn | ALB, Target Group, EC2 | Terminate EC2 before delete ALB |
| 4 | Kiệt | RDS, DB subnet group, Secret, S3 | S3 must be **empty** before delete bucket |
| 5 | Trí | NAT, EIP, IGW, 4 SG, VPC | NAT + EIP costly — delete as early as possible |
| 6 | Shared | IAM group/users/keys, Budgets, Bedrock key | Revoke **one** API key shared by 2 Mantle Lambdas |

#### Pre-completion checklist

- [x] No EC2 **running**, RDS **Available**, NAT **Available**
- [x] ZeroBug S3 bucket empty and deleted
- [x] No ZeroBug Lambda / Step Functions remaining
- [x] CloudFront distribution **Disabled** then deleted
- [x] Workshop Bedrock API key revoked (`us-east-1`)
- [x] Current month billing: no NAT, EIP, RDS, EC2 line items *(may prorate a few hours)*

#### Content

1. [Trinh — Edge & Auth](5.10.1-trinh/)
2. [Hoa — Serverless](5.10.2-hoa/)
3. [Toàn — EC2 & ALB](5.10.3-toan/)
4. [Kiệt — Data](5.10.4-kiet/)
5. [Trí — VPC & NAT](5.10.5-tri/)
6. [IAM & billing confirmation](5.10.6-iam-billing/)

{{% notice warning %}}
**NAT Gateway + Elastic IP** and **WAF** are the most expensive — if pausing overnight, still delete NAT or terminate EC2/RDS. Do not leave NAT running through the week after the workshop.
{{% /notice %}}

{{% notice tip %}}
Lambda cleanup names match the [parameter table](../5.2-prerequisites/5.2.3-parameter-table/): `ProjectImportLambda`, `SourceFileServiceLambda`, `ContextBuilderLambda`, `BedrockInvokeLambda`, `ResultServiceLambda`, `HistoryServiceLambda`.
{{% /notice %}}
