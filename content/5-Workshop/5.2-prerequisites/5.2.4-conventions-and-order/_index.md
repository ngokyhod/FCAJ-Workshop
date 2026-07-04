---
title: "Conventions & deployment order"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.2.4. </b> "
---

#### Shared checklist — before any member starts work

1. **Region:** Console → **Asia Pacific (Singapore) `ap-southeast-1`** *(IAM is global; WAF CloudFront = Global)*.
2. **Account:** the whole team uses **one shared AWS account**, signed in with IAM Users `tri`, `kiet`, `toan`, `hoa`, `trinh` — do not use Root for daily work.
3. **Parameter table:** open [5.2.3](5.2.3-parameter-table/) — fill in ID/ARN/DNS as soon as resources are created, share with the team.
4. **PassRole:** group `zerobug-team` already has policy `zerobug-ec2-passrole` ([5.3.1](../5.3-tv1-iam-vpc/5.3.1-iam-users-groups/)) — after adding the policy, **sign out and sign back in**.

#### Naming conventions

- Infrastructure resource prefix: **`zerobug-*`** (VPC, subnet, SG, bucket, RDS…)
- **Lambda function:** fixed PascalCase names — `ProjectImportLambda`, `SourceFileServiceLambda`, `ContextBuilderLambda`, `BedrockInvokeLambda`, `ResultServiceLambda`, `HistoryServiceLambda` *(record in [parameter table](5.2.3-parameter-table/))*
- Suggested tags: `Project=zerobug`, `Owner=Tri` … `trinh`

#### Deployment order

```
Trí (IAM + VPC) → Kiệt (S3 + RDS + Secrets + pgvector + Bedrock model access)
  → Toàn (EC2 + ALB) → Hoa (Lambda + Step Functions)
    → Trinh (Cognito + API GW + CloudFront + WAF)
      → 5.8 End-to-end testing
```

| Member | Minimum dependencies |
| --- | --- |
| Trí | Complete [5.2.1](5.2.1-aws-account/) |
| Kiệt | Trí done (Private Subnet, `zerobug-rds-sg`, Role ARN) |
| Toàn | Trí + Kiệt (Subnet/SG/Role; S3, RDS, DB Secret) |
| Hoa | Trí + Kiệt (NAT, Lambda-SG, Roles; S3, RDS, DB Secret, pgvector, Mantle params) |
| Trinh | **ALB DNS** from Toàn *(can run in parallel with Hoa)* |

#### Pause strategy (writing the report)

- You may **stand up all infrastructure Trí → Kiệt → Toàn → Hoa → Trinh** before the backend/Lambda runs.
- **Pause Toàn** after Java/SSM setup — do **Trinh** first; leave end-to-end testing for section **5.8**.
- CloudFront `/api/health` returning **502** or ALB Target **Unhealthy** when the app is not running — **normal**.

#### Build artifacts (Toàn & Hoa)

On the dev machine run `build-all.bat` (details in [5.2.2](5.2.2-tools-and-build/)):

- JAR → Toàn uploads `s3://<bucket>/deploy/zerobug-agent-app-1.0.0.jar`
- Lambda packages → Hoa deploys each function
